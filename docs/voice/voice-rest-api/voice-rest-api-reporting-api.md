---
title: Call Report
excerpt: >-
  Get call reports from the Sinch platform. Get call data for a specific user in
  the last 30 days, number of calls and more... Get more information about the
  API here.
hidden: 'true'
next:
  pages:
    - voice-rest-api-recording
---

[URL]
https://reportingapi.sinch.com/[version]

    Current version is "v1"

## Call Report Definition

    [CallReportItem]
        string - start
        int - duration
        int - success
        int - failed

## User Call Report

Gets the aggregated call data for a specific user for a duration of up to 30 days.

### Authorization

This is a protected resource and requires an application signed request \<applicationsignedrequest\>.

### Request

    https://reportingapi.sinch.com/v1/users/{type}/{endpoint}/calls/{domain}

**type**, **endpoint**, **user identity** (see Calling API \<voice-cloud-callingapi\>).

**domain** Optional parameter that specifies the terminating domain. Can be data or pstn. Default: **data**

    * data - Calls that were terminated over data
    * pstn - Calls that were terminated over the phone network

    Filters:
        time - \_start
        time - \_stop

_Example_

    [GET] https://reportingapi.sinch.com/v1/users/username/abc123def/calls/pstn?_start=2014-04-28&_stop=2014-04-29

### Response

    CallReportItem
        string - start
        int - duration
        int - success
        int - failed

**duration** is the sum of the duration of all calls in seconds.

**success** shows the number of successful calls.

**failed** shows the number of failed calls.

_Example_

    {
        "success": 2,
        "duration": 623,
        "failed": 0,
        "start": "2014-04-28T12:00:00Z"
    }

## Counters

Gets current value of a predefined instrumentation counter

### Authorization

This is a protected resource and requires an application signed request \<applicationsignedrequest\>.

### Request

    https://reportingapi.sinch.com/v1/counters/{id}

**id** is a predefined counter id string - this needs to be set up in cooperation with Sinch support team

_Example_

    [GET] https://reportingapi.sinch.com/v1/counters/currentcalls

### Response

    Counter
        long - value
        time - modified

**value** is the current value of the counter **modified** shows the date and time value of the last update to the counter

_Example_

    {
        "value": 12384955,
        "modified": "2014-04-28T12:00:00Z"
    }

### Supported counters

#### Current Calls

Number of ongoing calls

**Note:** The system updates the value of the counter every two minutes. The result value depends on how many calls were ongoing when the system updated the counter, and not when the request to the API was made.

_Example_

    [GET] https://reportingapi.sinch.com/v1/counters/currentcalls

## Get Service Status

Gets the current status of a predefined service

### Request

    https://reportingapi.sinch.com/v1/services/{id}

**id** is a predefined service id - this needs to be set up in cooperation with Sinch Noc - please contact Sinch to be able to get the status of a particular service in the platform

_Example_

    [GET] https://reportingapi.sinch.com/v1/services/rtpproxy

### Response

    ServiceStatus
        string - status

**status** shows current status of service. It can take these values:

> - “up” = service is available
> - “down” = service is unavailable

_Example_

    {
        "status": "up"
    }
