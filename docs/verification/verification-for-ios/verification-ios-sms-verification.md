---
title: SMS Verification
excerpt: >-
  Verify a user through SMS. Verification of a phone number is performed in two
  steps, a verification SMS is requested and a verification code for that
  particular verification session is sent to the recipient. Read more.
next:
  pages:
    - verification-ios-callout-verification
---
Verification of a phone number is performed in two steps, a verification SMS is requested and a verification code for that particular verification session is sent to the recipient. It's the responsibility of the developer to ask the end-user to provide the verification code from the SMS.

## Request an SMS verification [Objective-C]

To initiate a SMS verification, start by creating a `SINVerification`, then request an SMS by invoking `-[SINVerification initiateWithCompletionHandler:]`.
```objectivec
// Get user's current region by carrier info
NSString* defaultRegion = [SINDeviceRegion currentCountryCode];

NSError *parseError = nil;
id<SINPhoneNumber> phoneNumber = [SINPhoneNumberUtil() parse:@"<user input>"
                                               defaultRegion:defaultRegion
                                                       error:&parseError];

if (!phoneNumber){
  // Handle invalid user input
}

NSString *phoneNumberInE164 = [SINPhoneNumberUtil() formatNumber:phoneNumber
                                                          format:SINPhoneNumberFormatE164];

id<SINVerification> verification = [SINVerification SMSVerificationWithApplicationKey:@"<application key>"
                                                                          phoneNumber:phoneNumberInE164];

self.verification = verification; // retain the verification instance

[verification initiateWithCompletionHandler:^(id<SINInitiationResult> result, NSError *error) {
  if (result.success) {
    // Show UI for entering the code which will be received via SMS
  }
}];
```

## Request an SMS verification [Swift]
To initiate a SMS verification, start by creating a `SINVerification`, then request an SMS by invoking `-[SINVerification initiateWithCompletionHandler:]`.
```swift
// Get user's current region by carrier info
let defaultRegion: String = SINDeviceRegion.currentCountryCode()
let parseError: NSError! = nil

...continue please!
```


The call to `initiateWithCompletionHandler:` triggers sending a verification SMS. This method can be called multiple times, in case another SMS should be sent.

### Phone numbers - Parsing and E.164 Formatting

> **WARNING: Important**    
>
> When passing a number as a [NSString*` to create a `SINVerification`, the string should contain a number in *E.164* format. See the section `Phone numbers](doc:verification-ios-phone-numbers) for details.

## Set the content language of an SMS verification

It is possible to specify the content language when initiating an SMS verification from the SDK. This is specified via a list of [IETF](https://tools.ietf.org/html/rfc3282) language tags in order of priority. If the first language is not available, the next one will be selected and so forth. The default is "en-US".

The content language of the actual SMS verification message can be retrieved in `id<SINInitiationResult> result` of the `initiateWithCompletionHandler:` callback.
```objectivec
NSArray<NSString *> *languages = @[ @"zh-CN", @"es-ES", @"en-US" ];
id<SINVerification> verification = [SINVerification SMSVerificationWithApplicationKey:@"<APP KEY>"
                                                                          phoneNumber:phoneNumberInE164
                                                                            languages:languages];

[verification initiateWithCompletionHandler:^(id<SINInitiationResult> result, NSError *error) {
  if (result.success) {
    NSLog(@"Content Language is: %@", result.contentLanguage);
    // Show UI for entering the code which will be received via SMS
  }
}];
```




> **Note**    
>
> The content language specified can be overridden by carrier provider specific templates, due to compliance and legal requirements, such as [US shortcode requirements (pdf)](https://www.wmcglobal.com/storage/us_resources/ctia-short-code-monitoring-handbook-current-Short-Code-Monitoring-Handbook-v1.7.pdf).

## Validate code

To complete the verification of the phone number, the user should be instructed to enter the code from the SMS to the application, and the code should be passed to `-[SINVerification verifyCode:completionHandler:]`. For example:
```objectivec
- (IBAction)done:(id)sender {
  // User pressed a "Done" button after entering the code from the SMS.

  NSString* code = @"<get code from user input text field>";

  [self.verification verifyCode:code
              completionHandler:^(BOOL success, NSError* error) {
                if (success) {
                  // Phone number was successfully verified
                } else {
                  // Ask user to re-attempt verification
                }
              }];
}
```


The method `verifyCode:completionHandler:` may be invoked multiple times (for a limited number of times within a short duration). So for example, if the completion handler is invoked with an `NSError` with domain `SINVerificationErrorDomain` and code `SINVerificationErrorInvalidInput` or `SINVerificationErrorIncorrectCode`, the application may hint to the user that the code was incorrect, let the user adjust it, and call `verifyCode:completionHandler:` once again on the same `SINVerification`-instance.
