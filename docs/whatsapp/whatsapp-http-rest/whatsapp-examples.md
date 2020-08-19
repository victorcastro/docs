---
title: Examples
excerpt: Use cases for the Sinch WhatsApp API
---
## Interactive message templates

Interactive message templates have buttons that can be pressed by the recipent of the message, triggering some action. This is what they can look like on the recipient's phone.

![image](images\interactive-message-template-cta-buttons.png)

![image](images\interactive-message-template-quick-reply-buttons.png)

## Message template with quick reply button

Suppose there exists a message template with the following data.

Template name: `deal_offer`

Template body: `Hi {{1}}.\n\nYour order of {{2}} has been prepared for shipping. We can send it to you in three different ways.\nOption 1: {{3}}\nOption 2: {{4}}\nOption 3: {{5}}\n\n Please select your preferred delivery option.`

Template quick reply buttons:

1.  Option 1
2.  Option 2
3.  Option 3

The template can be used to send a message like this:

```
{
  "to": [
    "46732019282"
  ],
  "message": {
    "type": "template",
    "template_name": "deal_offer",
    "language": "en",
    "body_params": [
      "John",
      "books",
      "By train",
      "By car",
      "By drone"
    ],
    "media" : {
      "type": "text"
    },
    "buttons" : [
      {
        "type": "quick_reply",
        "payload": "books train"
      },
      {
        "type": "quick_reply",
        "payload": "books car"
      },
      {
        "type": "quick_reply",
        "payload": "books drone"
      }
    ],
  }
}
```

The quick reply button payload, which is returned in a callback notification when the recipient presses that button, could be used to easily collect statistics about the most popular delivery choices for books, for instance.

