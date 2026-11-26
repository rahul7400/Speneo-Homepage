# Manual notifications with Firebase Console

The Android app uses Firebase Cloud Messaging in the existing Firebase project.
No custom push API, token registration endpoint or backend deployment is required.

1. Sync Gradle and build/install the updated app.
2. Reach the main screen and choose Enable notifications, then Allow on Android 13+.
3. In the same Firebase project, open Messaging and create a Notifications campaign.
4. Enter the title/text, select the Android app `in.macrocodes.splitai`, and configure
   the audience and delivery time. Review before publishing.
5. Test with the app foregrounded and backgrounded. Tapping opens the app normally.

Firebase handles the installation token and background display. The application's
Messaging service handles foreground display. The notification icon and channel
are configured for both paths. The app does not log or upload tokens to its API.
The Firebase Console's separate Send test message action needs the installation's
FCM registration token; it is different from a Firebase Auth token.

These are general app announcements. They can reach an installation after logout
and are not restricted to an authenticated SplitAi user. Do not use this path for
private account/financial messages. Disable them using Android notification settings.
Logout no longer makes a push-related network call or waits for notification cleanup.

Auto-init is enabled after notification permission is available and disabled when
permission is unavailable on a later app resume. Android controls display permission;
disabling auto-init alone does not delete an existing FCM registration token.
Existing backend token workers from the earlier implementation are canceled on resume.
Previously prepared push backend files have been removed; the unrelated monthly
lifecycle API patch is unchanged. This task has not applied or deployed any backend code.

Real SDK build and on-device delivery still require Android Studio: the task runtime
cannot download the uncached Messaging SDK and Gradle cache access is restricted.
