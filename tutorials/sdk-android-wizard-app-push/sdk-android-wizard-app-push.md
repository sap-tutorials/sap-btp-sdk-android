---
parser: v2
author_name: Bruce Meng
author_profile: https://github.com/flyingfish162
primary_tag: software-product>sap-btp-sdk-for-android
auto_validation: true
tags: [  tutorial>beginner, operating-system>android, topic>mobile, software-product>sap-btp-sdk-for-android, software-product>sap-business-technology-platform ]
keywords: sdkforandroid
time: 20
---


# Send Notifications to Your Android Application
<!-- description --> Add foreground and background notifications to your application using Google Firebase.

## Prerequisites
- You completed [Try Out the SAP BTP SDK Wizard for Android](sdk-android-wizard-app).

## You will learn
- How to configure SAP BTP for push
- How to configure Mobile Services for push
- Foreground notifications
- Background notifications

---

### Configure Mobile Services for push notifications


1. Go to **SAP Mobile Services Cockpit**, navigate to **Mobile Applications > Native/MDK** and select the **btp.sdk.wizapp** application.

2. Select **Push Notification**.

    >If you don't see the option, follow the screenshots to add this feature:
    >
    > - ![Add assigned features](add-assigned-features.png)
    > - ![Add push notification](add-push-notification.png)

3. The **Push Notification** screen requires information from [Firebase](https://firebase.google.com/).

    ![Push notification](push-notification-blank.png)

4. In the [Firebase console](https://console.firebase.google.com/), select project **Wiz App**, and then go to **Project settings**.

    ![Project settings](firebase-project-settings.png)

5. Select the **Service accounts** tab and click **Generate new private key** button. Then click **Generate key** in the pop-up window and store the downloaded private key file securely because the key cannot be recovered if lost.

    ![Generate private key](generate-private-key.png)

6. In the SAP Mobile Services **Push Notification** settings screen, click **Browse...** to load the downloaded private key file. After that, click **Save** to save changes.

    ![Push notification android](push-notification-android.png)


### Send a notification


1. Switch to **Push Registrations** on the **Mobile Push Notification** settings page.

    ![Push registrations page](push-registrations.png)

2. Select certain registration to send notifications.

    ![Send notification](send-notification.png)

    >It may be difficult to tell which registration to choose. The last field on the page shows the last time a given registration made a request. If you are unsure which registration to choose, navigate through a few screens in the application and then press the **GO** button to refresh the display, or select all of them and send a greeting to all the registrations.

    >If you don't see the **Send Notifications** button, try logging out and back into the management cockpit.

3. Specify the notification text to send to the app.

    ![Send notification](send-notification2.png)


### Foreground notification


[OPTION BEGIN [Jetpack Compose-based UI]]

Notice that the notification displayed at the top of the screen.

![Notification pop up](pop-up-notification-jc.png)

Tap the notification and you can see the app displays the notification.

![Receive notification](receive-notification-jc.png)

[OPTION END]

[OPTION BEGIN [View-based UI]]

Notice that the notification displayed at the top of the screen.

![Notification pop up](pop-up-notification.png)

Tap the notification and you can see the app displays the notification.

![Receive notification](receive-notification.png)

[OPTION END]


### Background notification


[OPTION BEGIN [Jetpack Compose-based UI]]

1. On an emulator or mobile device, open another app, such as Chrome, which will cause the Wiz App to no longer be the foreground app.

2. If you now send another notification, you will notice that because the app is in the background, or not running, a notification is placed in the notification drawer.

    ![Receive Notification Background](receive-notification-background-jc.png)

3. Tapping the notification will bring the app to the foreground or open the app.

    ![Receive Notification from Background](receive-notification-jc.png)

    >If the app was not running when the notification was tapped, due to a change made in the previous tutorial, you can view it by pressing **Back** and navigating from the **Categories** screen to the **Entity** list screen.

Currently, the message is displayed in a dialog with cancel action.

You can add custom logic to the app to decide on the action to take, such as displaying the new Office Furniture category by modifying the `showMessageDialog` method in the class `FCMPushCallbackListener`.

```Kotlin
private fun showMessageDialog(activity: Activity, message: PushRemoteMessage) {
    activity.addContentView(
        ComposeView(activity).apply {
            setContent {
                var showDialog by remember { mutableStateOf(true) }
                if (showDialog) {
                    val textMessage = message.alert ?: activity.getString(R.string.push_text)
                    val notificationTitle = message.title?: activity.getString(R.string.push_message)
                    AlertDialogComponent(title = notificationTitle, text = textMessage, onPositiveButtonClick = {
                        message.notificationID?.let {
                            BasePushService.setPushMessageStatus(it, PushRemoteMessage.NotificationStatus.CONSUMED)
                        }
                        showDialog = false
                    })
                }
            }
        },
        
        ... ...
    )
}
```

[OPTION END]

[OPTION BEGIN [View-based UI]]

1. On an emulator or mobile device, open another app, such as Chrome, which will cause the Wiz App to no longer be the foreground app.

2. If you now send another notification, you will notice that because the app is in the background, or not running, a notification is placed in the notification drawer.

    ![Receive Notification Background](receive-notification-background.png)

3. Tapping the notification will bring the app to the foreground or open the app.

    ![Receive Notification from Background](receive-notification.png)

    >If the app was not running when the notification was tapped, due to a change made in the previous tutorial, you can view it by pressing **Back** and navigating from the **Categories** screen to the **Entity** list screen.

Currently, the message is displayed in a dialog with cancel action.

You can add custom logic to the app to decide on the action to take, such as displaying the new Office Furniture category by modifying the `showMessageDialog` method in the class `FCMPushCallbackListener`.

```Kotlin
private fun showMessageDialog(activity: FragmentActivity, message: PushRemoteMessage) {
    var textMessage = message.alert ?: activity.getString(R.string.push_text)
    var notificationTitle = message.title?: activity.getString(R.string.push_message)

    DialogHelper(activity).showDialogWithCancelAction(
            fragmentManager = activity.supportFragmentManager,
            message = textMessage,
            negativeAction = {},
            title = notificationTitle,
            positiveAction = {
                message.notificationID?.let {
                    BasePushService.setPushMessageStatus(it, PushRemoteMessage.NotificationStatus.CONSUMED)
                }
            }
    )
}
```

[OPTION END]

>For further information on push, see [Push Notifications](https://help.sap.com/doc/f53c64b93e5140918d676b927a3cd65b/Cloud/en-US/docs-en/guides/features/push/android/push.html), [Push API Notification Scenarios](https://help.sap.com/viewer/38dbd9fbb49240f3b4d954e92335e670/Cloud/en-US/aaec2dbe78ec4fc08ef0a605a899e3dd.html), and [About FCM Messages](https://firebase.google.com/docs/cloud-messaging/concept-options).

Congratulations! You have seen how an app can make use of foreground and background notifications.


---
