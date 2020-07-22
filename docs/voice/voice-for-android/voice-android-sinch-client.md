---
title: Sinch Client
excerpt: ''
next:
  pages:
    - voice-android-calling
---
The _SinchClient_ is the Sinch SDK entry point. It is used to configure the user’s and device’s capabilities, as well as to provide access to feature classes such as the _CallClient_, _MessageClient_ and _AudioController_.

## Create a _SinchClient_

```java
// Instantiate a SinchClient using the SinchClientBuilder.
android.content.Context context = this.getApplicationContext();
SinchClient sinchClient = Sinch.getSinchClientBuilder().context(context)
                                                  .applicationKey("<application key>")
                                                  .applicationSecret("<application secret>")
                                                  .environmentHost("clientapi.sinch.com")
                                                  .userId("<user id>")
                                                  .build();
```

* The _Application Key_ is obtained from the [Sinch Developer Dashboard - Apps](https://portal.sinch.com/#/apps). 
* The _User ID_ should uniquely identify the user on the particular device.

_Note:_ All listener callbacks emitted from the Sinch SDK are invoked on the same thread that the call to `SinchClientBuilder.build` is made on. If the invoking thread is _not_ the main-thread, it needs to have an associated `Looper`.

## Specify capabilities

The SinchClient can be configured to enable or disable certain functionality. Please see the [Reference](reference\index.html?com\sinch\android\rtc\SinchClient.html) for a comprehensive description of each capability.

The following example shows how to setup the client with voice calling enabled.

```java
// Specify the client capabilities.
sinchClient.setSupportCalling(true);
sinchClient.setSupportManagedPush(true);
// or
sinchClient.setSupportActiveConnectionInBackground(true);
sinchClient.startListeningOnActiveConnection()
```

Calling `startListeningOnActiveConnection` allows your application to receive incoming calls and messages without using push notifications.

> **Note**
>
> If the application is meant to only make outgoing calls but not receive incoming calls, don’t call `startListeningOnActiveConnection` or `setSupportManagedPush`. Outgoing calls can be made after calling the start method.

## Start the Sinch client

Before starting the client, add a client listener (see [Reference](reference\index.html?com\sinch\android\rtc\SinchClientListener.html) documentation):

```java
sinchClient.addSinchClientListener(new SinchClientListener() {

    public void onClientStarted(SinchClient client) { }

    public void onClientStopped(SinchClient client) { }

    public void onClientFailed(SinchClient client, SinchError error) { }

    public void onRegistrationCredentialsRequired(SinchClient client, ClientRegistration registrationCallback) { }

    public void onLogMessage(int level, String area, String message) { }
});

sinchClient.start();
```

### Terminate the Sinch client

When the app is done using the SinchClient, it should be stopped. If the client is currently listening for incoming events, it needs to stop listening as well. After `terminate` is called, any object retrieved directly from the client object (that is, `CallClient`, `MessageClient`, and `AudioController`) is considered invalid.

Terminating the client:

```java
sinchClient.stopListeningOnActiveConnection();
sinchClient.terminate();
```
