---
title: Improving verification performance with a great UI
excerpt: >-
  Improve verification performance with great UI. Here is a UI that will capture
  correct input from the user.
next:
  pages:
    - verification-android-miscellaneous
---
For better verification performance, it should be straightforward and easy for users to enter their phone phone numbers. To simplify this step and build a UI that accurately captures the correct input from the users, both Android (starting from API level 21) and Sinch SDK provide some utility APIs.

## PhoneNumberFormattingTextWatcher

The Android SDK provides a [PhoneNumberFormattingTextWatcher](http://developer.android.com/reference/android/telephony/PhoneNumberFormattingTextWatcher.html). This watcher can be added as a text changed listener to a text field and will format it automatically if a phone number is entered in local or international format.

## SinchPhoneNumberUtils

Another convenient helper utility class available in Sinch SDK is `SinchPhoneNumberUtils`. The function `isPossiblePhoneNumber` is especially useful to check phone number typed by the user seems to be correct. If it's not the text field could be highlighted with a different color (red).
