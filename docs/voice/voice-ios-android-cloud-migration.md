---
title: Sinch Voice and Video SDK Migration Guide
excerpt: >-
  Follow this migration guide to start using the latest Sinch mobile Voice and Video SDKs.
hidden: true
---

## Intended Audience

This guide serves the purpose of facilitating the ease of migration from the old, legacy Sinch Moble SDKs to the new cloud-based platform. We assume that you're familiar with key concepts, operations and glossary of the Sinch Voice and Video SDK. Here we provide a comprehensive list of the _changes_ required both in the application _and_ on your backend to get started with the new cloud-based Sinch mobile SDKs.

Otherwise, if you had never used Sinch Voice and Video SDK before, please refer to the respective platform documentation:
- [Sinch cloud-based Android SDK v.4](doc:voice-for-android-cloud)
- [Sinch cloud-based iOS SDK v.5](doc:voice-ios-cloud)

## Contents

- [General Changes](doc:voice-ios-android-cloud-migration#general-changes)
- [Common Registration Changes](doc:voice-ios-android-cloud-migration#common-registration-changes)
- [Backend User Authentication Server](doc:voice-ios-android-cloud-migration#backend-user-authentication-server)
- [Android and UserController](doc:voice-ios-android-cloud-migration#android-and-usercontroller)
- [Backend OAuth Server for Huawei Push](doc:voice-ios-android-cloud-migration#backend-oauth-server-for-huawei-push)
- [Build and Dependencies](doc:voice-ios-android-cloud-migration#build-and-dependencies)

## General Changes

The changes can be grouped by the platform (iOS/Android) or along the client/server boundaries. The table below provides a good eagle-eye overview of them:

|                                                               | Client    | Backend   |  Android | iOS|
| --------------------------------------------------------------|-----------|-----------|----------| ---|
|User Registration Changes|v|v|v|v|
|Backend User Authentication Server||v|v|v|
|UserController API|v||v||
|Huawei Push Notifications|v||v||
|Backend OAuth Server for Huawei Push ||v|v||

## Common Registration Changes

When you initiate `SinchClient` you have to provide _user identity_. In the legacy Sinch SDKs, user authentication and the authorization of the registration was based on credentials provided in a way of an opaque token, based on the _SHA1_ digest of Sinch _Application Secret_. There was also a way to provide _Application Secret_ directly to the _SinchClient_.

In the new SDK, registration is made both more secure and transparent:
- _Application Secret_ must never be stored in the application, and there is no way to provide _Application Secret_ to the _SinchClient_ that could encourage that, instead:
- Authorization is granted by providing industry-standard JWT registration token. 

Both [iOS](doc:voice-ios-cloud-auth) and [Android](doc:voice-android-cloud-application-authentication) documents provide the same coverage on the formation of the JWT registration token, including links to the server-side sample code.

## Backend User Authentication Server

> ❗️ 
> MISSING SECTION (Christoffer)

## Android and UserController

There is a new way to register _User_ on Android: _UserController API_. [UserController](voice-for-android-cloud/reference/com/sinch/android/rtc/UserController.html) provides a way independently from the `SinchClient` lifecycle, to register a _User_ for incoming calls via push notifications. You can also use it to unregister push token if receiving of incoming calls is no longer desirable (e.g. on logout, or changing users).

_UserController_ provides two types of callbacks:
- related to the general _User_ registration on Sinch backend;
- related to the push device token registration.

> ❗️
>
> Though the use of _UserController_ is __optional__ for the FCM Push Notification case, since the _SinchClient_ can 
> register itself to receive FCM Push, it is __mandatory__ for the Huawei Push Notifications, since the _push device
> token_ have to be acquired by the _Application_ before constructing _UserController_.
> Thus, it is __highly recommended__ to use _UserController_ in both cases.

The action flow diagram of the _User_ registration via _UserController_ is provided below. _UserController_'s callbacks are highlighted in pale blue.

![Registering User via UserController](voice-for-android-cloud\images\usercontroller-callbacks.pu.png)

## Backend OAuth Server for Huawei Push

> ❗️ 
> MISSING SECTION (Christoffer)

## Build and Dependencies

> ❗️ 
> MISSING SECTION (Vicror, Christoffer)
