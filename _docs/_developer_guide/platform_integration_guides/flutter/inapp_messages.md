---
nav_title: In-App Messages
article_title: In-App Messages for Flutter
platform: Flutter
page_order: 4
page_type: reference
description: "This article covers in-app messages for iOS and Android apps using Flutter, including customizing and logging analytics."
channel: in-app messages

---

# In-app messages

Native in-app messages display automatically on Android and iOS when using Flutter. This article covers different customization options for in-app messages.

## Analytics

To log analytics using your `BrazeInAppMessage`, pass the instance into the desired analytics function:
- `logInAppMessageClicked`
- `logInAppMessageImpression`
- `logInAppMessageButtonClicked` (along with the button index)

For example:
```dart
// Log a click
braze.logInAppMessageClicked(inAppMessage);
// Log an impression
braze.logInAppMessageImpression(inAppMessage);
// Log button index `0` being clicked
braze.logInAppMessageButtonClicked(inAppMessage, 0);
```

## Disabling automatic display

To disable automatic in-app message display, make these updates in the native layer.

{% tabs %}
{% tab Android %}

1. Ensure you are using the automatic integration initializer, which is enabled by default starting in version `2.2.0`.
2. Set the in-app message operation default to `DISCARD` by adding the following line to your `braze.xml` file.

```xml
<string name="com_braze_flutter_automatic_integration_iam_operation">DISCARD</string>
```

{% endtab %}
{% tab iOS %}

1. Implement the `BrazeInAppMessageUIDelegate` delegate as described in our iOS article on [core in-app message delegate](https://braze-inc.github.io/braze-swift-sdk/tutorials/braze/c1-inappmessageui).

2. Update your `inAppMessage(_:displayChoiceForMessage:)` delegate method to return `.discard`.

For an example, see [AppDelegate.swift](https://github.com/braze-inc/braze-flutter-sdk/blob/master/example/ios/Runner/AppDelegate.swift) in our sample app.

{% endtab %}
{% endtabs %}

## Receiving in-app message data

To receive in-app message data in your Flutter app, the `BrazePlugin` supports sending in-app message data using [Dart Streams](https://dart.dev/tutorials/language/streams) (recommended) or by using a data callback (legacy).

The `BrazeInAppMessage` object supports a subset of fields available in the native model objects, including `uri`, `message`, `header`, `buttons`, `extras`, and more.

{% alert note %} The legacy data callback method will soon be deprecated. In-app messages can be added to both data streams and data callbacks. If you have already integrated data callbacks and wish to use data streams, remove any callback logic to ensure that in-app messages are processed exactly once. {% endalert %}

### Method 1: In-app message data streams (recommended)

You can set a data stream listener in Dart to receive in-app message data in your Flutter app.

To begin listening to the stream, use the code below to create a `StreamSubscription` in your Flutter app and call the `subscribeToInAppMessages()` method with a function that takes a `BrazeInAppMessage` instance. Remember to `cancel()` the stream subscription when it is no longer needed.

```dart
// Create stream subscription
StreamSubscription inAppMessageStreamSubscription;

inAppMessageStreamSubscription = _braze.subscribeToInAppMessages((BrazeInAppMessage inAppMessage) {
  // Function to handle in-app messages
}

// Cancel stream subscription
inAppMessageStreamSubscription.cancel();
```

For an example, see [main.dart](https://github.com/Appboy/flutter-sdk/blob/develop/braze_plugin/example/lib/main.dart) in our sample app.

### Method 2: In-app message data callback (Legacy)

You can set a callback in Dart to receive Braze in-app message data in the Flutter host app.

To set the callback, call `BrazePlugin.setBrazeInAppMessageCallback()` from your Flutter app with a function that takes a `BrazeInAppMessage` instance.

{% tabs %}
{% tab Android %}

This callback works with no additional integration required.

{% endtab %}
{% tab iOS %}

1. Implement the `BrazeInAppMessageUIDelegate` delegate as described in our iOS article on [core in-app message delegate](https://braze-inc.github.io/braze-swift-sdk/tutorials/braze/c1-inappmessageui).

2. Update your `willPresent` delegate implementation to call `BrazePlugin.process(inAppMessage)`.

For an example, see [AppDelegate.swift](https://github.com/braze-inc/braze-flutter-sdk/blob/master/example/ios/Runner/AppDelegate.swift) in our sample app.

{% endtab %}
{% endtabs %}

#### Replaying the callback for in-app messages

To store any in-app messages triggered before the callback is available and replay them once it is set, add the following entry to the `customConfigs` map when initializing the `BrazePlugin`:
```dart
BrazePlugin braze = new BrazePlugin(customConfigs: {replayCallbacksConfigKey: true});
```

## Test displaying a sample in-app message

Follow these steps to test a sample in-app message.

1. Set an active user in the React application by calling `braze.changeUser('your-user-id')` method.
2. Head to the **Campaigns** page on your dashboard and follow [this guide][1] to create a new in-app message campaign.
3. Compose your test in-app messaging campaign and head over to the **Test** tab. Add the same `user-id` as the test user and click **Send Test**.
4. Tap the push notification and that should display the in-app message on your device.

![A Braze in-app message campaign showing you can add your own user ID as a test recipient to test your in-app message.][2]

[1]: {{site.baseurl}}/user_guide/message_building_by_channel/in-app_messages/create/
[2]: {% image_buster /assets/img/react-native/iam-test.png %} "In-App Messaging Test"
