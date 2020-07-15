---
title: Introduction
excerpt: >-
  Get introduced to the features and capabilities that the Sinch XML API offers.
  Send and receive rich multimedia content. Read more.
next:
  pages:
    - xml-service-savemms
---
**Some of the key features of XML API are:**

  - **Sure Route Mobile Terminated Message:** Customer’s don’t have to
    specify the destination network operator while using the XML API,
    they can just specify the telephone number where the message needs
    to be delivered and Sinch with determine the correct network
    operator for this number.

  - **Device Profile:** The device profile, if available, is also
    included along with the delivery receipt and MO MMS message.
    System-wide database of last know device profile per phone number is
    maintained, updated whenever carrier database is queried or delivery
    receipt is received.

  - **Fallback SMS:** Fallback SMS is available thorough our XML API, if
    the rich media file size is larger than the device’s maximum file
    size, a fallback SMS will be sent. Also, also if the device does not
    support rich media content then a fallback SMS is sent. The customer
    can specify the default message text to be delivered to end users.

  - **Transcoding and Content Adaptation:** XML API offers transcoding
    and content adaptation to our customers where adaptation of source
    media is needed to match the destination terminal profile. This
    feature uses device discovery results.

  - **Higher Throughput:** Customers using XML API send messages over
    the MM1 connection thus providing higher throughput

  - **saveMMS/sendSavedMMS:** XML API allow users to save their MMS from
    XML using the saveMMS API, once the MMS is saved, it can be utilized
    by other functions through the mms-id returned. Users can then call
    sendSavedMMS API to send stored content from a specified account.
    This allows user to send a burst of messages with avoids any latency
    and consumes less bandwidth.

**Sinch XML MMS API Methods:**

|         Action         |                      Functionality                                                                                                                                                     |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `save_mms`       | This API stores an MMS from XML.                                                                                                                                          |
| `send_saved_mms` | This API sends stored content from a specified account using an mms-id to a single mobile number.                                                                         |
| `send_mms`       | Sends an MMS defined in the XML containing slides of embedded with,video, audio, images and/or text to a single or list of mobile numbers,in international number format. |
