# Huawei Push Messages

Sinch supports Huawei push messages via [Huawei Push Kit](https://developer.huawei.com/consumer/en/doc/development/HMSCore-Guides/service-introduction-0000001050040060). [Huawei Push Kit](https://developer.huawei.com/consumer/en/doc/development/HMSCore-Guides/service-introduction-0000001050040060) (_HPK_) is part of [Huawei Mobile Services](https://developer.huawei.com/consumer/en/hms) (_HMS_). The Sinch SDKs and platform can take care of sending push notification messages via _Huawei Push Kit_ on your behalf as part our _Sinch Managed Push Notifications_ functionality. To enable Huawei push messages for Android devices you will need to do two things:

1. Make use of the _Huawei_ (_HMS_) APIs in the Sinch Android SDK, see details [here](TODO INSERT LINK).
2. Implement an _OAuth 2.0 Authorization Server_ endpoint that can provide Sinch with _OAuth 2.0_ _access_tokens_ required to send messages via _HPK_ on your behalf.

## Huawei OAuth Flow

Sinch will send push notification messages via the [_HPK_ API](https://developer.huawei.com/consumer/en/doc/HMSCore-References-V5/https-send-api-0000001050986197-V5). Huawei Push Kit supports (and require) an _OAuth 2.0_ _Client Credentials_ flow to authenticate against the _Huawei Push Kit_ server endpoint(s). Your Huawei App in Huawei _AppGallery Connect_ will have an _App ID_ and an _App secret_. These are to be used as OAuth client credentials, i.e. `client_id` and `client_secret` respectively.

Sinch supports Huawei OAuth flow by delegation. You will keep your `client_secret` on your backend, and Sinch will request an OAuth `access_token` via an _Authorization Server_ endpoint that you implement. The overall flow is depicted below:

![Sinch - Huawei OAuth flow](images\20200922-sinch-huawei-hpk-oauth.png)

Key takeaways:

1. When Sinch need an `access_token` required to send a push message to _Huawei Push Kit_ server, it will make an OAuth request using a _Client Credentials_ grant type to your _Authorization Server_. This request will be specifying and `client_assertion_type=urn:ietf:params:oauth:client-assertion-type:jwt-bearer` will as value for `client_assertion` provide a JWT that is symmetrically signed with your _Sinch Application Secret_.
2. Your _Authorization Server_ should validate the JWT provided as `client_assertion` by Sinch and that the signed JWT is signed with your _Sinch Application Secret_.
3. Your _Authorization Server_ should request an `access_token` using the Huawei HMS OAuth Authoriation Server endpoint and your Huawei _App ID_ and _App secret_ as `client_id` and `client_secret`.
4. Your _Authorization Server_ should pass the `access_token` (as received from HMS) in the response back to Sinch.

Details on how to validate the JWT provided by Sinch as `client_assertion` is available in the following sections.

### Validating the `client_assertion` JWT provided by Sinch

As described in the overview, Sinch will make a request to your OAuth _Authorization Server_ endpoint, requesting a _HMS_ `access_token`. The request will be on the following form:

```
POST /sinch/rtc/push/oauth2/v1/huawei-hms/token HTTP 1.1
Host: your-domain.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&
scope=https%3A%2F%2Fpush-api.cloud.huawei.com&
client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
client_assertion=<JWT>
```

(_NOTE_: The value of `scope` and `client_assertion_type` in the example above are URL-encoded)

The `JWT` will be making use of the standard JWT header parameters `alg` and `kid`, and the standard claims `iss`, `sub`, `iat`, `exp`, `nonce` and `aud`. Before we jump to the details of how to validate this token, here is an example:

```
// JWT Header (example)
{
  "alg": "HS256",
  "kid": "hkdfv1-20200901"
}

// JWT Payload (example)
{
  "iss": "//rtc.sinch.com/applications/a32e5a8d-f7d8-411c-9645-9038e8dd051d",
  "sub": "a32e5a8d-f7d8-411c-9645-9038e8dd051d",
  "aud": "https://as.your-domain.com/sinch/rtc/push/oauth2/v1/huawei-hms/token",
  "scope": ["https://push-api.cloud.huawei.com"],
  "iat": 1600780504,
  "exp": 1600784104,
  "nonce": "6b438bda-2d5c-4e8c-92b0-39f20a94b34e"
}
```

* Claim `iss` is on the form `//rtc.sinch.com/applications/<your Sinch Application Key>`
* Claim `sub` is your _Sinch Application Key_.
* Claim `aud` will be set to the _Authorization Server_ token endpoint you have configured with Sinch. E.g. `https://as.your-domain.com/sinch/rtc/push/oauth2/v1/huawei-hms/token`
* The JWT claim `scope` will be an array containing the single requested scope `https://push-api.cloud.huawei.com` (representing the Huawei _Push Kit_ server domain)
* Claims `iat`, `exp`, `nonce` are standard JWT claims (see [JWT RFC 7519](https://tools.ietf.org/html/rfc7519))


### `kid` and Deriving a Signing Key from Sinch Application Secret

The `kid` parameter in the JWT header is on the form `hkdfv1-{DATE}` where `{DATE}` is date of signing in UTC on format `YYYYMMDD`.

When validating the JWT, use a signing key that is derived from your _Sinch Application Secret_ as follows. Given:

- A function `HMAC256(key, message)`.
- A date-formatting function `FormatDate(date, format)`.
- The date of signing as variable `signedAt`. This is to be extracted from the JWT header parameter `kid`.
- _Sinch Application Secret_ as variable `applicationSecret`, holding the secret as a _base64_ encoded string.

, derive the signing key as follows:

```
signingKey = HMAC256(BASE64-DECODE(applicationSecret), UTF8-ENCODE(FormatDate(signedAt, "YYYYMMDD")))
```

(__NOTE__: This is the same key derivation scheme as used for [Token-based User registration](doc:voice-android-cloud-application-authentication))

Your _Authorization Server_ should validate the JWT in accordance with section [RFC 7523 - Section 3. JWT Format and Processing Requirements](https://tools.ietf.org/html/rfc7523#section-3)

#### Acquiring an `access_token` from Huawei HMS

After validating the JWT client assertion, your _Authorization Server_ should in turn request an `access_token` from the Huawei HMS OAuth endpoint, using your Huawei _App ID_ and _App secret_ as `client_id` and `client_secret`. The access token received from Huawei should then be included in the response back to Sinch. See [Huawei documentation](https://developer.huawei.com/consumer/en/doc/HMSCore-Guides/open-platform-oauth-0000001050123437-V5#EN-US_TOPIC_0000001050123437__section12493191334711) for how to implement requesting an OAuth _access token_ using Huawei HMS.

Example response to Sinch:

```
HTTP/1.1 200 OK
Content-Type: application/json;charset=utf-8
{
  "access_token": "<access token acquired from Huawei>",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```

Sinch will then be able to use this `access_token` to send push messages to your end-user devices until the token expires, upon which Sinch will issue a new token request to your _Authorization Server_.

#### Rejecting the `access_token` grant request

If your _Authorization Server_ rejects the access token request from Sinch, it should respond with a HTTP response that is compliant with the [OAuth 2.0 standard](https://www.oauth.com/oauth2-servers/access-tokens/access-token-response/). Example:

```
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=utf-8
{
  "error": "unauthorized_client",
  "error_description": "Your helpful error description"
}
```

Please see [https://www.oauth.com/oauth2-servers/access-tokens/access-token-response/](https://www.oauth.com/oauth2-servers/access-tokens/access-token-response/) for details on how to formulate conformant OAuth error responses.


## More Resources

* [Huawei HMS OAuth Client Credentials Flow](https://developer.huawei.com/consumer/en/doc/HMSCore-Guides/open-platform-oauth-0000001050123437-V5#EN-US_TOPIC_0000001050123437__section12493191334711)
* [OAuth 2.0 Client Credentials](https://www.oauth.com/oauth2-servers/access-tokens/client-credentials/)
* [OAuth `client_secret_jwt`](https://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication)

