---
title: Introduction
excerpt: >-
  This document provides a detailed user guide and reference documentation on
  the Sinch Voice REST API. For general information on how to use the Sinch APIs
  including methods, types, errors and authorization, please check the Using
  REST page.
next:
  pages:
    - voice-rest-api-calling-api
---

## Overview

When using Sinch for voice calling, the Sinch dashboard can be seen as a big telephony switch. It receives incoming phone calls (also known as _incoming call “legs”_), sets up outgoing phone calls (also known as _outgoing call “legs”_), and bridges the two. The incoming call leg may come in over a data connection (from a smartphone or web application using the Sinch SDKs) or through a local phone number (from the PSTN network). Similarly, the outgoing call leg can be over data (to another smartphone or web application using the Sinch SDKs) or the PSTN network.

For most call scenarios, you can use the Sinch SDKs on a smartphone or on web client to establish calls without the need of backend integration. For additional control or flexibility of the calls, you can use the Sinch REST APIs to manage the calls.

Controlling a call from your application backend is done by responding to callbacks from the Sinch platform and/or by calling REST APIs in the Sinch platform from your application backend. For more details on the callbacks triggered from the Sinch platform see the [Callback API](doc:voice-rest-api-callback-api).

For more details on the REST APIs to that can be used to manage calls see the [Calling API](doc:voice-rest-api-calling-api).

These are the typical call scenarios that you can control with the Sinch Callback and Calling APIs:

> - App to phone calls
> - App to app calls
> - Phone to phone calls
> - Text-to-speech calls
> - Conference calls
> - [SIP trunking calls](doc:voice-sip-trunking)

## API Quick Reference

### Callback API

To use callback events you need to assign a callback URL in the Sinch portal under your app settings.

| Event  | HTTP Verb | Functionality                                                                                         |
| ------ | --------- | ----------------------------------------------------------------------------------------------------- |
| ICE    | POST      | [Incoming Call Event callback](doc:voice-rest-api-callback-api#incoming-call-event-callback-ice)      |
| ACE    | POST      | [Answered Call Event callback](doc:voice-rest-api-callback-api#answered-call-event-callback-ace)      |
| DiCE   | POST      | [Disconnect Call Event callback](doc:voice-rest-api-callback-api#disconnect-call-event-callback-dice) |
| PIE    | POST      | [Prompt Input Event callback](doc:voice-rest-api-callback-api#prompt-input-event-callback-pie)        |
| Notify | POST      | [Notify Event callback](doc:voice-rest-api-callback-api#notify-event-callback-notify)                 |

### Calling API

The calling-related APIs are divided in two categories - **"traffic management"** and **"configuration management"**. "Traffic management" encompasses management of individual calls. Since a call always belongs to a region, the corresponding regional endpoints must be used. Configuration management is agnostic to individual calls and hence a global endpoint is used.

### Regional Endpoints - Traffic Management (TM)

    https://calling-euc1.api.sinch.com/calling/[version]  - Europe
    https://calling-use1.api.sinch.com/calling/[version]  - United States
    https://calling-sae1.api.sinch.com/calling/[version]  - South America
    https://calling-apse1.api.sinch.com/calling/[version] - South East Asia 1
    https://calling-apse2.api.sinch.com/calling/[version] - South East Asia 2

    It is also possible to use the endpoint:
    https://calling.api.sinch.com/calling/[version] - redirected by Sinch to an appropriate region

    Current  version is "v1"

For cases where the call is the result of an incoming PSTN, SIP or data call, the endpoint to use for managing that call is supplied in the ICE event.

### Global Endpoint - Configuration Management (CM)

    https://callingapi.sinch.com/calling/[version]

    Current  version is "v1"

ICE callbacks will also provide region-specific URLs in the `callResourceUrl` property. Use the nearest base URL to access the following voice service resources:

| Type | URL                                                    | HTTP Verb | Functionality                                                                                                    |
| ---- | ------------------------------------------------------ | --------- | ---------------------------------------------------------------------------------------------------------------- |
| CM   | /configuration/numbers/                                | GET       | [Get a list of your numbers](doc:voice-rest-api-calling-api#get-numbers)                                         |
| CM   | /configuration/numbers/                                | POST      | [Assign numbers to an app](doc:voice-rest-api-calling-api#update-numbers)                                        |
| CM   | /configuration/numbers/                                | DELETE    | [Un-assign numbers to an app](doc:voice-rest-api-calling-api#un-assign-number)                                   |
| CM   | /configuration/callbacks/applications/{applicationkey} | GET       | [Get callback URLs of your app](doc:voice-rest-api-calling-api#get-callbacks)                                    |
| CM   | /configuration/callbacks/applications/{applicationkey} | POST      | [Update the callback URLs of your app](doc:voice-rest-api-calling-api#update-callbacks)                          |
| CM   | /calling/query/number/{number}                         | GET       | [Query a number](doc:voice-rest-api-calling-api#query-number)                                                    |
| TM   | /calls/id/{callId}                                     | PATCH     | [Manage Call](doc:voice-rest-api-calling-api#manage-call)                                                        |
| TM   | /calls/id/{callId}                                     | GET       | [Get call result](doc:voice-rest-api-calling-api#get-call-result)                                                |
| TM   | /conferences/id/{conferenceId}                         | GET       | [Get conference info](doc:voice-rest-api-calling-api#get-conference-info)                                        |
| TM   | /conferences/id/{conferenceId}/{callId}                | PATCH     | [Mute/Unmute conference participant](doc:voice-rest-api-calling-api#mute-unmute-conference-participant)          |
| TM   | /conferences/id/{conferenceId}/{callId}                | DELETE    | [Kick conference participant](doc:voice-rest-api-calling-api#kick-conference-participant)                        |
| TM   | /conferences/id/{conferenceId}                         | DELETE    | [Kick all conference participants](doc:voice-rest-api-calling-api#kick-all-conference-participants)              |
| TM   | /callouts                                              | POST      | [Place text-to-speech or conference call](doc:voice-rest-api-calling-api#conference-and-text-to-speech-callouts) |

## App to phone calls

In this scenario, calls are originated from an app using the iOS, Android or Javascript SDK and are terminated to the fixed or mobile phone network\*. For additional call control, you can configure a callback URL under your app’s voice settings in the Sinch dashboard, where Sinch will send call-related events. By capturing and responding to these events from your backend, you can allow or deny calls to go through. Events will also be triggered when the calls will be answered or disconnected.

For more information please check the [Callback API](doc:voice-rest-api-callback-api). The callback events that are used in app to phone calls are the Incoming Call Event callback, the Answer Call Event callback and the Disconnect Call Event callback. You can also manage an ongoing call from your backend with the Manage Call API, which is part of the [Calling API](doc:voice-rest-api-calling-api).

## App to App calls

In this scenario, calls are originated from and terminated to an app using the iOS, Android or Javascript SDK. Both call legs are established over the data connection of the smartphone or computer (VoIP). For additional call control, you can specify a callback URL where Sinch will send call-related events. By capturing and responding to these events from your backend, you can allow, deny and control the calls. You can configure the call back URL under your app’s voice settings in the Sinch [dashboard](https://portal.sinch.com/#/login).

For more information please check the [Callback API](doc:voice-rest-api-callback-api). The callback event that is used in app to app calls is the Incoming Call Event callback.

## Phone to phone calls

In this scenario, calls are originated from a voice number and are terminated to the fixed or mobile phone network. You can rent and configure voice numbers from the Sinch [dashboard](https://portal.sinch.com/#/login) by following these steps:

1.  Rent a Voice number from the Sinch [dashboard](https://portal.sinch.com/#/login), under the tab “Numbers”.
2.  Assign the number to your application. Under the “Apps” tab, select your app and assign the number under the app Voice settings. Alternatively, you can rent and configure numbers with REST APIs. For more information please check the[ Number Administration documentation](doc:log-in).
3.  Configure a callback URL under your app’s Voice settings, where Sinch will send call-related events.

When a user calls your configured voice number, the Sinch platform will trigger an Incoming Call Event callback towards your callback URL. The destination number - where the call will be connected to - has to be specified in your response to the Incoming Call Event callback. Similarly to app to phone calls, the Sinch platform will trigger additional events for call control.

For more information please check the [Callback API](doc:voice-rest-api-callback-api). The callback events that are used in phone to phone calls are the Incoming Call Event callback, the Answer Call Event callback and the Disconnect Call Event callback. You can also manage an ongoing call from your backend with the Manage Call API, which is part of the [Calling API](doc:voice-rest-api-calling-api).

## Conference calls

The Sinch dashboard supports a variety of different ways of initiating a conference call and connecting participants.

By using the Sinch Voice SDKs, you can allow your users to call in a conference from a mobile or web app. For more information please check the Voice SDKs documentation.

If you have not specified a callback URL under your app settings for voice, the participants will be directly added to the conference that is uniquely identified by the conference Id specified in the SDK client method.

If you have specified a callback URL under your app settings for voice, an Incoming Call Event callback will be triggered towards your URL, containing information on the conference Id that the caller wants to connect to. By responding to this event you can allow or deny the connection to the conference, or even specify a different conference Id.

For more information check the Incoming Call Event callback and the ConnectConf action.

You can also allow users to dial in a conference by calling a fixed phone number. To do this, first follow the steps mentioned in [Phone to phone calls](#phone-to-phone-calls) to configure a number in your app and set a callback URL. Every time a user calls your configured number, an Incoming Call Event callback will be triggered towards your URL. By responding to this event with the ConnectConf action, you can connect the call to a conference.

For more information check the Incoming Call Event callback and the ConnectConf action.

By using the conference callout API, you can trigger calls to fixed or mobile phones and connect them all to the same conference room.

For more information please check the [Callouts API](doc:voice-rest-api-calling-api#conference-and-text-to-speech-callouts) .

The Sinch dashboard allows you to control an ongoing conference through REST APIs. There are several conference-control options available, such as muting/unmuting participants or kicking out a participant or all participants from the conference when the conference ends.

For more information check the conferencing APIs that are available under the [Calling API](doc:voice-rest-api-calling-api) .

The Sinch dashboard allows recording of conference calls. The recorded files are stored in your own Amazon S3 bucket. For more information on how to record a conference, please check the ConnectConf action in the [Callback API](doc:voice-rest-api-callback-api) .

Conference recording is disabled by default. To enable conference recording for your account please contact Sinch support, providing your Amazon S3 bucket information, where the recordings will be stored.

## Text to speech calls

With the text-to-speech REST API, you can trigger a call to be placed to a fixed or mobile phone number and play a synthesized text message.

For more information please check the [Callouts API](doc:voice-rest-api-calling-api#conference-and-text-to-speech-callouts).
