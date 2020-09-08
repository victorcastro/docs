---
title: Authenticate to the Sinch JavaScript SDK From Your PHP Backend
excerpt: >-
  In this tutorial you will learn how to authenticate to Sinch JS SDK from a PHP
  backend. Use it to securely start the Sinch Client.
---

Define the following class to generate a user ticket to authenticate to the Sinch JavaScript SDK:

```javascript
class SinchTicketGenerator
{

    private $applicationKey;
    private $applicationSecret;

    public function __construct($applicationKey, $applicationSecret)
    {
        $this->applicationKey = $applicationKey;
        $this->applicationSecret = $applicationSecret;
    }

    public function generateTicket($username, DateTime $createdAt, $expiresIn)
    {
        $userTicket = [
            'identity' => [
                'type'      => 'username',
                'endpoint'  => $username,
            ],
            'expiresIn'         => $expiresIn,
            'applicationKey'    => $this->applicationKey,
            'created'           => $createdAt->format('c'),
        ];
        $userTicketJson = preg_replace('/\s+/', '', json_encode($userTicket));

        $userTicketBase64 = $this->base64Encode($userTicketJson);

        $digest = $this->createDigest($userTicketJson);

        $signature = $this->base64Encode($digest);

        $userTicketSigned = $userTicketBase64.':'.$signature;

        return $userTicketSigned;
    }

    private function base64Encode($data)
    {
        return trim(base64_encode($data));
    }

    private function createDigest($data)
    {
        return hash_hmac('sha256', $data, base64_decode($this->applicationSecret), true);
    }
}
```

You can use the above class like so:

```javascript
$generator = new SinchTicketGenerator('YOUR_APP_KEY', 'YOUR_APP_SECRET');
$signedUserTicket = $generator->generateTicket('YOUR_USERNAME', new DateTime(), 3600);
```

“YOUR_USERNAME” is any way you uniquely identify your users, and “3600” is the number of seconds in which the ticket will expire.

If you don’t yet have an app key and secret from Sinch, you can get them for free here: <https://portal.sinch.com/#/signup>.

Once you’ve successfully generated the ticket, you can use it to securely start the Sinch client. Follow any of our client-side JavaScript tutorials to use the Sinch client to make phone calls and send instant messages:

- [Phone calling](doc:using-sinch-js-sdk-to-call-a-phone-number)
- [App-to-app calling](doc:turn-your-browser-into-a-phone-with-the-sinch-js-sdk)
