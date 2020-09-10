---
title: Capability
excerpt: >-
  Requesting a Capability Lookup for a contact
hidden: false
---

## Introduction to Capability <span class="betabadge">Beta</span>

A Capability Lookup means checking available options to reach the [**contact**](doc:conversation#contact) on the channels where it has a channel identity. 

Capability Lookups can only be executed for contacts that already exist in a project/app. For executing the request it is required to provide either the contactId or the channel recipient identities of the contact.

The request is executed asynchronously, therefore it immediately responds we the registered lookup request. The result of the Capability Lookup is sent to the registered webhook for the CAPABILITY trigger. To set up webhooks with the proper trigger read about [**webhooks**](doc:conversation#webhook).

### Capability Request

Below is an example for executing a Capability Lookup request using the contactId.

Conversation API POST `capability:query`

```json
{
  "app_id": "{{APP_ID}}",
  "project_id": "{{PROJECT_ID}}",
  "recipient": {
    "contact_id": "{{CONTACT_ID}}"
  }
}
```

Another alternative is using a list fo channel recipient identities instead of contactId.

```json
{
  "app_id": "{{APP_ID}}",
  "project_id": "{{PROJECT_ID}}",
  "recipient": {
    "identified_by": {
      "channel_identities" : [
        {
         "channel": "WHATSAPP",
         "identity": "46712312312"
        }
      ]
    }
  }
}
```

### Capability Response

The response for the Capability Lookup is the registered request. The result of the lookup is returned on the webhook with CAPABILITY trigger, see [**Capability Notification**](doc:conversation-capability#capability-notification).

### Capability Notification

The result of a Capability Lookup is posted to the webhook having CAPABILITY trigger set.

Below is an example Capability Notification.

```json
{
  "contact_id": "{{CONTACT_ID}}",
  "identity": "46712312312",
  "channel": "WHATSAPP",
  "capability_status" : "CAPABILITY_FULL"
}
```

The Capability Notification can have the following fields.

| Field                      | Description                                                                                  |
| -------------------------- | -------------------------------------------------------------------------------------------- |
| contact_id                 | The id of the contact that the lookup was executed for.                                      |
| identity                   | The channel recipient identity of the contact.                                               |
| channel                    | The channel that the capability information was retrieved from.                              |
| capability_status          | The capability of the contact reported by the channel. See possible values below the table.  |
| reason                     | If the capability lookup failed the reason is present in this field.                         |
| channel_capabilities       | If the underlying channel has a list of specific capabilities then it is listed here.        |

Possible values for ``capability_status`` are:
* ``CAPABILITY_UNKNOWN`` : the channel capability for the identity is unknown, due to the underlying channel not making this information available.
* ``CAPABILITY_FULL`` : the specified identity supports all the features of the channel.
* ``CAPABILITY_PARTIAL`` : the specified identity supports a subset of the channel features.
* ``NO_CAPABILITY`` : the specified identity has no capability on the channel.
