---
title: Initial Messaging Limits & Quality Rating
---

## Quality Rating

The quality rating shows how messages have been received by your customers in a rolling window of the past 24 hours. We show it in three different states - Green, Yellow and Red - with Green/Yellow/Red representing high/medium/low messaging quality respectively.

## Messaging Limits

Messaging limits determine how many users your business can send messages to on a daily basis. This includes new conversations as well as existing conversations with users. The messaging limit does NOT limit the number of messages your business can send, just the number of users you are trying to message. It also does NOT apply to messages sent in response to a user-initiated message within a 24-hour period.

    - Tier 1: Allows your business to send messages to 1K unique customers in a rolling 24-hour period.
    - Tier 2: Allows your business to send messages to 10K unique customers in a rolling 24-hour period.
    - Tier 3: Allows your business to send messages to 100K unique customers in a rolling 24-hour period.

**Note: A business starts in Tier 1 when it registers its phone number.**

A business’ phone number will be upgraded to the next tier if:

    - Its quality rating is not low, and
    - The cumulative amount of users it sends notifications to adds up to twice its current messaging limit within a 7-day period. Once the business reaches this threshold, it will be moved to the next tier. The minimum amount of time that this upgrade can occur is after 48 hours, when the business is sending messages up to their current limit every day.

See the charts below for examples of how a business might move to the next tier:

Example 1: A business is upgraded from Tier 1 to Tier 2 when it messages a total of 2,000 users within a 7-day period.

![image](whatsapp-http-rest\images\tier-example-1.png)

Example 2: The chart explains the concept of messaging limit in a rolling 24-hour window.

![image](whatsapp-http-rest\images\tier-example-2.png)

## Status

There are different statuses for phone numbers that usually refer to its connection status (e.g., Connected, Offline, etc.). Occasionally, a phone number may show one of two particular statuses that are related to quality and messaging limits: Flagged and Restricted.

    - Flagged: When the quality rating reaches a low state, the phone number is moved to a Flagged status. If the message quality improves to a high or medium state and continues to maintain this for the next 7 days, the phone number will return to a Connected status. If the quality rating does not improve, the number will return to a Connected status but with a lower messaging limit imposed on it.
    - Restricted: Phone numbers that reach their messaging limit are moved to a Restricted status. During a Restricted phase, the number will be unable to send any notification messages until the 24-hour window for sending messages is reset. They can still respond to any messages initiated by users.

You will receive a notification in the Business Manager if a phone number status from your WhatsApp business account(s) changes to Flagged or Restricted. If your messaging limit changes, you will also receive a notification about this change.

Other phone number statuses include:

    - Pending: This is when a business first sets up a phone number that hasn’t yet been registered.
    - Connected: The phone number is online and functioning normally.
    - Offline: The phone number is currently not reachable by WhatsApp servers or is not otherwise online.
