---
title: Voice Calling
excerpt: >-
  Set up voice calling with the Sinch JavaScript SDK. The SDK supports four
  types of calls: web-to-web (or app) calls, web-to-phone calls, web-to-sip and
  conference calls.
next:
  pages:
    - voice-js-push-notifications
---

The Sinch SDK supports four types of calls: _web-to-web (or app)_ calls, _web-to-phone_ calls, _web-to-sip_ and _conference_ calls. The CallClient is the entry point for the calling functionality of the Sinch SDK. At the moment, calling is only supported in Chrome and Firefox, and we hope to add support in more browsers later on. The `CallClient` is the entry point for the calling functionality of the Sinch SDK.

Calls are placed through the `CallClient` and general events are received through `eventListener` callback on CallClient. The call client is owned by the SinchClient and accessed using `sinchClient.getCallClient()`. When a call is placed, a new `call` object is generated. It’s possible to add eventListener\` callbacks on the created call-object, to listen for events related to this call, such as when the call state changes.

Calling is not enabled by default. Enable calling by instantiating `SinchClient` in the following way:

```javascript
var sinchClient = new SinchClient({
  applicationKey: '...',
  capabilities: { calling: true },
});
```

## Set up a _web-to-web (or app)_ call

Use the CallClient to start the call (the `callUser` method). Pass the user identifier of the callee (the user receiving the call) to the call method, so that Sinch services can connect the call to the callee.

```javascript
var sinchClient = new SinchClient({applicationKey: '...', capabilities: {calling: true}});
var call = sinchClient.callUser('<remote user id>');
call.addEventListener(...);
```

A call object is returned, containing details about the participants in the call, call details such as start time, call state, possible errors, and so on.

Assuming any of callee’s apps are available, the method `onCallProgressing` is called on the `CallListener`. It notifies the application that the outgoing call is progressing. If a progress tone should be played, this is where it should be started.

When the other party answers, the `onCallEstablished` method is called. Now, the users can start talking. If a progress tone was previously played, it should be stopped now. This is also where the incoming audio stream can be connected to a HTML audio element:

```javascript
var sinchClient = new SinchClient({
  applicationKey: '...',
  capabilities: { calling: true },
});

var callListeners = {
  onCallEstablished: function (call) {
    $('audio').attr('src', call.incomingStreamURL); //If audio element has attribute "autoplay"
  },
};

var callClient = sinchClient.getCallClient();
var call = callClient.callUser('<remote user id>');
call.addEventListener(callListeners);
```

> **Note**
>
> If the audio is not connected properly to the audio element, as shown above, there will be no sound.

## Set up an _web-to-phone_ call

An _web-to-phone_ call is a call that is made to a phone on the regular telephone network. Setting up an _web-to-phone_ call is not much different from setting up a _web-to-web_ call. Instead of invoking the `callUser` method, invoke the `callPhoneNumber` method on the `CallClient` object. Sufficient funds must be available on the Sinch account and a valid phone number specified for the call to connect successfully. The phone number should be specified according to the E.164 number formatting (<http://en.wikipedia.org/wiki/E.164>) recommendation and should be prefixed with a ‘+’. E.g. to call the US phone number 415 555 0101, the phone number should be specified as “+14155550101”. The ‘+’ is the required prefix and the US country code ‘1’ prepended to the local subscriber number.

```javascript
var sinchClient = new SinchClient({
  applicationKey: '...',
  capabilities: { calling: true },
});

var callListeners = {
  onCallEstablished: function (call) {
    $('audio').attr('src', call.incomingStreamURL); //If audio element has attribute "autoplay"
  },
};

var call = callClient.callPhoneNumber('+46000000000');
call.addEventListener(callListeners);
```

Placing an _web-to-phone_ call requires a developer account with credits. Topping up credits can be done on the Account page. Credits are used each time an _web-to-phone_ call is placed and the balance history is updated after each call.

_Web-to-phone_ calls can be tested by calling the following test number: _+46000000000_. When placing a call to this number, you will hear a voice prompt stating that the call has been connected, and shortly after that the call will automatically be ended.

## Setting up an _app-to-sip_ call

An _app-to-sip_ call is a call that is made to a SIP server. Setting up an _app-to-sip_ call is not much different from setting up an _app-to-app_ call. Instead of invoking the `callUser` method, invoke the `callSip` method on the `CallClient` object. The SIP identity follows the form of email addresses (<user@domain>), for example <Alice@SipProviderA.com>.

```javascript
var sinchClient = new SinchClient({
  applicationKey: '...',
  capabilities: { calling: true },
});

var callListeners = {
  onCallEstablished: function (call) {
    $('audio').attr('src', call.incomingStreamURL); //If audio element has attribute "autoplay"
  },
};

var call = callClient.callSip('Alice@SipProviderA.com');
call.addEventListener(callListeners);
```

When customized SIP headers are passed as a parameter, the headers should be prefixed with ‘x-’. If the SIP-server reported any errors, the call object will contain an error object with the headers, listen for this using the onCallEnded callback and check for call.errors.

## Set up a _conference_ call

A _conference_ call can be made to connect a user to a conference room where multiple users can be connected at the same time. The identifier for a conference room may not be longer than 64 characters.

```javascript
var sinchClient = new SinchClient({
  applicationKey: '...',
  capabilities: { calling: true },
});

var callListeners = {
  onCallEstablished: function (call) {
    $('audio').attr('src', call.incomingStreamURL); //If audio element has attribute "autoplay"
  },
};

var call = callClient.callConference('CONFERENCE_ROOM_HASH');
call.addEventListener(callListeners);
```

It is also possible to connect users to a conference call via the [Sinch REST API](doc:voice-rest-api-onprem-calling-api#text-to-speech).

## Play ringback tone

For a good user experience, users expect a ringback tone while waiting for receiving end to pick up the phone. For this purpose, the following eventListener for a call may be a good template.

```javascript
var callListeners = {
  onCallProgressing: function (call) {
    $('audio#ringback').prop('currentTime', 0); //Ensure ringback start from beginning
    $('audio#ringback').trigger('play'); //Play ringback when call is progressing
  },
  onCallEstablished: function (call) {
    $('audio#ringback').trigger('pause'); //End ringback

    $('audio#incoming').attr('src', call.incomingStreamURL); //Connect incoming stream to audio element
  },
  onCallEnded: function (call) {
    $('audio#ringback').trigger('pause'); //End the ringback
    $('audio#incoming').attr('src', ''); //Ensure no incoming stream is playing

    //Optional: Enable user interface to make another call
  },
};
```

## Handle incoming calls

In order to receive calls, `SinchClient` must be instantiated with configuration option `supportActiveConnection` set to true and invoking `startActiveConnection` after your `SinchClient` has successfully started.

```javascript
var sinchClient = new SinchClient({
  applicationKey: '<application_key>',
  capabilities: { calling: true },
  supportActiveConnection: true,
});
```

To answer calls, the application must be notified when the user receives an incoming call.

Add a `eventListener` to the `CallClient` to act on the incoming calls. The `eventListener` is notified using onIncomingCall as calls come in to the application.

```javascript
CallClient callClient = sinchClient.getCallClient();

callClient.addEventListener({
    onIncomingCall: function(incomingCall) {...}
});
```

When the incoming call method is executed, the call can either be connected automatically without any user action, or it can wait for the user to press the answer or the hangup button. If the call is set up to wait for a user response, we recommended that a ringtone is played to notify the user that there is an incoming call.

```javascript
callClient.addEventListener({
    onIncomingCall: function(incomingCall) {
        //Play some groovy tunes & show UI
        ...
        //Add event listeners to the new call object representing the incoming call
        incomingCall.addEventListener(callListeners);
    }
});
```

To get events related to the call, add a call listener. The call object contains details about participants, start time, potential error codes, and error messages.

### Answer incoming call

To answer the call, use the `answer` method on the call to accept it. If a ringtone was previously played, it should be stopped now.

User presses the answer button:

```javascript
// User answers the call
call.answer();
// Stop playing ringing tone
...
```

Now, the clients on both ends establish the connection. When the call is established and the voice streams are running in both directions, the `onCallEstablished` listener method is called. In this listener method you should connect the incoming audio to an HTML audio element to hear the other side.

### Decline incoming call

If the call should not be answered, use the `hangup` method on the call to decline. The caller is notified that the incoming call was denied. If a ringtone was previously played, it should be stopped now.

User presses the hangup button:

```javascript
// User does not want to answer
call.hangup();
// Stop playing ringing tone
...
```

## Disconnecting a Call

When the user wants to disconnect an ongoing call, use the `hangup` method. Either user taking part in a call can disconnect it.

Hanging up a call:

```javascript
call.hangup();
```

When either party disconnects a call, the application is notified using the call listener method `onCallEnded`. This allows the user interface to be updated, an alert tone to be played, or similar actions to occur.

A call can be disconnected before it has been completely established.

Hanging up a connecting call:

```javascript
// Starting a call
var call = callClient.callPhoneNumber('<remote participant>');

// User changed his/her mind, let’s hangup
call.hangup();
```

## Video calling

<!-- > **WARNING: Beta**
>
> The Sinch JS SDK Video Calling features are still under development and not suitable for a production environment -->

Video calling can be activated for data calling by adding the capability “video” when starting the client. With this capability, video will now be added to all data-calls. Web to phone (PSTN) calling will be unaffected. A video call is set up in the same way as a voice only call:

```javascript
var sinchClient = new SinchClient({applicationKey: '...', capabilities: {calling: true, video: true}}); // Notice the video capability
var call = sinchClient.callUser('<remote user id>');
call.addEventListener(...);
```

For callbacks, hangup and other methods on the call object when performing a video call, please reference the API docs.

<!-- for audio-only calls. -->

<!-- ## Group calling

> **WARNING: Beta**
>
> The Sinch JS SDK group calling feature is still under development and not suitable for a production environment

Group calling allows participants to broadcast their presence to a particular “conference room”, upon which all participants already online will automatically add the new participant to the call. This results in a flexible “all-to-all” video conferencing solution.

For starting the sinchClient with the capability of making a group call, the capability “multiCall” must be set.
```javascript
var sinchClient = new SinchClient({applicationKey: '...', capabilities: {calling: true, video: true, multiCall: true}}); // Notice the multiCall capability
```


When joining a particular group and acting on the incoming media streams from other participants, the Sinch JS SDK will automatically take care of all call management, but you have to initiate the process and act on certain callbacks in order to connect incoming media streams to the relevant HTML elements.
```javascript
var callClient = sinchClient.getCallClient();

sinchClient.start({username: "<username>", password: "<password>"}).then(function() { // After the client is started, place the call
    var groupCall = callClient.callGroup("<name_of_group>");

    groupCall.addEventListener({
        onGroupRemoteCallAdded: function(call) { // Called when a remote participant stream is ready
            $('video#other').attr('src', call.incomingStreamURL);
        },
        onGroupLocalMediaAdded: function(stream) { // Called when the local media stream is ready (optional)
            $('video#me').attr('src', window.URL.createObjectURL(stream));
        },
        onGroupRemoteCallRemoved: function(call) { // Called when a remote participant has left and the stream needs to be removed from the HTML element
            $('video#other').attr('src', (remoteCalls[index] || {}).incomingStreamURL || '');
        },
    });
});
```


In this example, the group call is initiated after the client is started. A number of listeners was added in order to connect the incoming remote media streams (first callback), connect the local media stream and also manage lost participants. When the callGroup method is called, the callClient will immediatley start communication with other participants on the specified Channel in order to establish active video calls. -->
