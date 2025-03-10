---
nav_title: "Push Registration"
article_title: Push Registration
page_order: 2
page_type: reference
description: "This reference article discusses what it means to be registered for push and how we send push messages and deal with push tokens at Braze."
channel:
  - push

---

# Push registration

> This article covers the process through which a user gets assigned a push token, and how Braze sends push messages to your users.

## Push tokens

Understanding push tokens and what they are is a fundamental piece of understanding how we send push messages here at Braze. Push tokens are a unique anonymous identifier generated by a user's device and sent to Braze to identify where to send each recipient's notification. If a device doesn't have a push token, there is no way to send push to it.

Push tokens are generated by push service providers. Braze connects with push service providers like Firebase Cloud Messaging Service (FCMs) for Android and Apple Push Notification Service (APNs) for iOS, and those providers send unique device tokens that identify your app. Each device has one, unique token that is used for messaging. Tokens can [expire](#push-token-expire) or be updated.

There are two ways a [push token][push-tokens] can be classified that are essential to understanding how a push notification can be sent to your users.

1. **Foreground push** provide the ability to send regular visible push notifications to the foreground of a user's device.
2. **Background push** are available regardless of whether a particular device has opted-in to receive push notifications from that brand. Background push allows brands to send silent push notifications - notifications that intentionally aren't displayed - to devices to support key functionalities like uninstall tracking.

When a user profile has a valid foreground push token associated with an app, Braze considers the user "push registered" for the given app. Braze, then, provides a specific segmentation filter, `Push enabled for App,` to help identify these users.

{% alert note %}
The `Push enabled for App` filter only considers the presence of a valid foreground or background push token for the given app. However, the more generic `Push Enabled` filter segments users who have explicitly activated push notifications for any apps in your app group. This count includes only foreground push and doesn't include users who have unsubscribed. You can learn more about these and other filters in [Segmentation Filters]({{site.baseurl}}/user_guide/engagement_tools/segments/segmentation_filters).
{% endalert %}

### Multiple users on one device

Push tokens are specific to both a device and app, so it isn't possible to use push tokens to distinguish between multiple users who are using the same device.

For example, say you have two users: Charlie and Kim. If Charlie has enabled push notifications for your app on his phone and Kim uses Charlie's phone to log out of Charlie's profile and log into her own, the push token will be re-assigned to Kim's profile. The push token will then remain assigned to Kim's profile on that device until she logs out and Charlie logs back in again.

An app or website can only have one push subscription per device. So when a user logs out of a device or website, and a new user logs in, the push token gets reassigned to the new user. This is reflected on the user's profile in **Contact Settings** section of the **Engagement** tab:

![Push token changelog on the **Engagement** tab of a user's profile, which lists when the push token was moved to another user, and what the token was.][4]

Because there isn't a way for push providers (APNs/FCM) to distinguish between multiple users on one device, we pass the push token to the last user who was logged in to determine which user to target on the device for push.

## Push token registration

Platforms deal with push token registration and push permissions in different ways:

- **Android**:
  - **Android 13**:<br>Push permission must be asked of and granted by the user. Receives a token once permission is granted by the user. Your app can manually request permission from the user at opportune times, but if not, users will be prompted automatically once your app creates a [notification channel](https://developer.android.com/reference/android/app/NotificationChannel).
  - **Android 12 and earlier**:<br>All users are considered `Subscribed` upon their first session when Braze automatically requests a push token. At this point, the user is **push enabled** with a valid push token for that device and a default subscription state of `Subscribed`.<br><br>
- **iOS**: Not automatically registered for push.
    - **iOS 12 (with Provisional Authorization)**: <br>Your app can request [provisional authorization]({{site.baseurl}}/user_guide/message_building_by_channel/push/ios/notification_options/#provisional-push) or authorized push. Authorized push requires explicit permission from a user before sending any notifications (receives a token once permission is granted by user), whereas [provisional push][provisional-blog] lets you send notifications __quietly__, directly to the notification center without any sound or alert.
    - **iOS 11 and later & iOS 12 (without Provisional Authorization)**: <br>All users must explicitly opt-in to receive push notifications. Will receive a token once opted-in.<br><br>
- **Web:** You must request explicit opt-in from users via the native browser permission dialog, will receive a token once opt-ed in.  Unlike iOS and Android, which let your app show the permission prompt at any time, some modern browsers will only show the prompt if triggered by a "user gesture" (mouse click or keystroke). If your site tries to request push notification permission on page load, it will likely be ignored or silenced by the browser.

| Get a push token | Send a push token |
| ---------------- | ----------------- |
| Applications must register with a push provider in order to get a push token for a device. | Developers can then target the device using the push token generated by FCM/APNs.|
{: .reset-td-br-1 .reset-td-br-2}

### Check user's push subscription state

![User profile for John Doe with their push subscription state set to Subscribed.][3]{: style="float:right;max-width:35%;margin-left:15px;"}

There are two ways you can check a user's push subscription state with Braze:

1. **User Profile**: You can access individual user profiles through the Braze dashboard on the [User Search][5] page. After finding a user's profile (via email address, phone number, or external user ID), you can select the **Engagement** tab to view and manually adjust a user's subscription state. 
<br>
2. **Rest API Export**: You can export individual user profiles in JSON format using the export [Users by segment][segment] or [Users by identifier][identifier] endpoints. Braze will return a push tokens object that contains push enablement information per device.

### Checking push registration status

On the **Engagement** tab in a user's profile, you will see **Push Registered For** followed by an app name. If no app information exists for that device, you will see two dashes (**&#45;&#45;**). There will be an entry for every device that belongs to the user.

If the device entry's app name is prefixed by `Foreground:`, the app is authorized to receive both foreground push notifications (visible to the user) and background push notifications (not visible to the user) on that device.

![Push Changelog with an example push token.][2]{: style="float:right;max-width:40%;margin-left:15px;margin-top:10px;"}

On the other hand, if the device entry's app name is prefixed by `Background:`, the app is only authorized to receive [background push]({{site.baseurl}}/user_guide/message_building_by_channel/push/types/#background-push-notifications) and cannot display user-visible notifications on that device. This usually indicates the user has disabled notifications for the app on that device.

If a push token is moved to a different user on the same device, that first user will no longer be push registered.

## Push token management

Check out the following chart for actions that lead to push tokens changes or removal from user profiles. 

| Action | Description |
| ------ | ----------- |
| `changeUser()` method called | The Braze `changeUser()` method switches the user ID that the SDKs are assigning user behavior data to. This method is usually called when a user logs into an application. When `changeUser()` is called with a different or new user ID on a specific device, that device's push token will be moved to the appropriate Braze profile with corresponding user ID. |
| Push error occurs | Some common push errors that lead to token removal include `MismatchSenderId`, `InvalidRegistration`, and other types of push bounces. <br><br>Check out our full list of common [push errors][errors]. |
| User uninstalls | When a user uninstalls the application from a device, Braze will remove the user's push token from the profile. |
{: .reset-td-br-1 .reset-td-br-2}

### What does this look like on a broader scale?

When a user opens a new application and grants push access from a push prompt, a call is made from the Braze SDK to the push providers. When that call is made, the push provider runs a check to see if everything is set up correctly. If so, a push token gets passed into your device. When that token arrives, the SDK communicates this to Braze. Once Braze has received the token from the push provider, we update or create a new user profile. These users are now considered registered.

If we want to launch a campaign, we create a campaign in Braze that generates a push payload to send to the push provider. From there, the provider delivers the push payload to the user's device and the SDK passes the messaging state to Braze.

![A flow chart that maps the aforementioned push process between Braze, the customer, and Apple Push Notification Service or Firebase Cloud Messaging.][push-process]

| Registration steps | Messaging steps |
| ------------------ | --------------- |
| 1. Customer (device) registers to push provider<br>2. Provider generates and delivers push token<br>3. Flush tokens in Braze |1. Braze sends push payload to provider<br>2. Provider delivers the push payload to the device<br>3. SDK passes messaging stats to Braze |
{: .reset-td-br-1 .reset-td-br-2}

## Frequently asked questions

### What happens when an opted-in user deletes and then redownloads my app?

Suppose a user opts-in for push, receives some push messaging, and then later deletes the app. This will remove the push consent at the device level. From here, the first bounced push after the uninstall will automatically result in that user being opted-out of future push messaging. After this, if a user were to reinstall the app but not launch it, Braze won't be able to send a push to the user because push tokens have not been re-granted for your app.

Additionally, if a user were to re-enable foreground push, it would require a session start to update this information in their user profile to begin receiving push messaging.
 
### Under what circumstances do push tokens expire? {#push-token-expire}

Unfortunately, APNs and FCM don't really define this. Push tokens can expire when an app is updated, when users transfer their data to a new device, or when they re-install an operating system. For the most part, we don't really have insight into why push providers will expire certain push tokens.

To account for that ambiguity, our SDK push integrations always register and flush tokens on session start to ensure we have the most up-to-date token.

[errors]: {{site.baseurl}}/help/help_articles/push/push_error_codes/#push-bounced-mismatchsenderid
[identifier]: {{site.baseurl}}/api/endpoints/export/user_data/post_users_identifier/
[segment]: {{site.baseurl}}/api/endpoints/export/user_data/post_users_segment/
[push-process]: {% image_buster /assets/img/push_process.png %}
[5]: {{site.baseurl}}/user_guide/engagement_tools/segments/using_user_search/
[2]: {% image_buster /assets/img/push_changelog.png %}
[push-tokens]: {{site.baseurl}}/user_guide/message_building_by_channel/push/push_registration/
[3]: {% image_buster /assets/img/push_example.png %}
[4]: {% image_buster /assets/img/push_token_changelog.png %}


