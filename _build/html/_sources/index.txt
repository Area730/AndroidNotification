### Install

1. Import [this plugin](https://www.assetstore.unity3d.com/#!/content/45507) into your Unity project.
2. Check if you have **AndroidManifest.xml** in Assets/Plugins/Android folder.      
**If you don't** - add this manifest (https://yadi.sk/d/PIzCJa-jif8Zo) to _Assets/Plugins/Android_ folder.      
**If you do** - check if it contains UnityPlayerNativeActivity or the one that extends it.       
If you have UnityPlayerNativeActivity - you are good to go.      
If you have activity that extends **UnityPlayerNativeActivity**- set its full name (e.g. com.unity3d.player.UnityPlayerNativeActivity) in _Window->Ultimate Local Notifications -> Settings_   

![](https://www.dropbox.com/s/k7ffsmb5wlezgni/unityClassField.png?raw=1)

If you don't have any - add this activity to your manifest:

```xml
<activity android:name="com.unity3d.player.UnityPlayerNativeActivity" android:label="@string/app_name">
<intent-filter>
<action android:name="android.intent.action.MAIN" />
<category android:name="android.intent.category.LAUNCHER" />
<category android:name="android.intent.category.LEANBACK_LAUNCHER" />
</intent-filter>
<meta-data android:name="unityplayer.UnityActivity" android:value="true" />
<meta-data android:name="unityplayer.ForwardNativeEventsToDalvik" android:value="false" />
</activity>
```
## Schedule simple notifications
**The package contains code samples in Assets/Area730/Notifications/Examples/Scripts folder.**

The notifications are created using **NotificationBuilder** class. Its constructor takes 3 arguments - id of the notification, title and notification text.

Next example example shows how to schedule the notification that will be shown immediately:
```c#
int id          = 1;
string title    = "Notification titile";
string body     = "Notification body";

NotificationBuilder builder = new NotificationBuilder(id, title, body);
AndroidNotifications.scheduleNotification(builder.build());
```
## Schedule delayed notifications
If you want to set delay - call builder.setDelay(int miliseconds) or builder.setDelay(System.TimeSpan delayTime). The next example shows how to create a notification that will be shown in one hour:

```c#
int id          = 1;
string title    = "Notification titile";
string body     = "Notification body";

// Show notification in one hour
TimeSpan delay  = new TimeSpan(1, 0, 0); 

NotificationBuilder builder = new NotificationBuilder(id, title, body);
builder.setDelay(delay);

AndroidNotifications.scheduleNotification(builder.build());
```
## Customization
NotificationBuilder allows you to set different parameters of your notification such as color, small icon, large icon, auto cancel, alert once, ticker, notification number, sound, vibration pattern, group, sort key and if the notification repeats every interval of time.    

**All methods with description you can find in NotificationBuilder.cs file.**    

Next example shows scheduling of the notification that will be shown in 15 minutes with ticker, default audio and vibration, with autocancel (if you click it - it will be removed from the list), and with red background color (background color is not supported on some Android versions, please refer to Android docs for more info)


```c#
int id          = 1;
string title    = "New notification";
string body     = "You have some unfinished business!";

// Show notification in 15 minutes
TimeSpan delay  = new TimeSpan(0, 15, 0); 

NotificationBuilder builder = new NotificationBuilder(id, title, body);
builder
.setTicker("New notification from your app!")
.setDefaults(NotificationBuilder.DEFAULT_ALL)
.setDelay(delay)
.setAutoCancel(true)
.setColor("#B30000");

AndroidNotifications.scheduleNotification(builder.build()
```

## Repeating notifications
To set repeating notification you should set notification as repeating **and set the time interval**.     
Next example shows scheduling of the notification that will be shown in 5 minutes and then shown every 10 minutes:   

```c#
int id              = 1;
string title        = "New repeating notification";
string body         = "You have some unfinished business!";

// Show notification in 5 minutes
TimeSpan delay      = new TimeSpan(0, 5, 0);

// Show notification with 10 minute interval
TimeSpan interval   = new TimeSpan(0, 10, 0);

NotificationBuilder builder = new NotificationBuilder(id, title, body);
builder
.setDelay(delay)
.setRepeating(true)
.setInterval(interval);

AndroidNotifications.scheduleNotification(builder.build());
```

## Settings custom icons
You can set custom icons for your notification. There are 2 types of icon - small and large. Small icon is mask.  Both icons should be located in _Assets/Plugins/Android/Notfications/res/drawable_ folder or one of the drawable folders (e.g. drawable-mdpi etc.). 

You can use these icon generators:    

1. [Small icon generator](http://romannurik.github.io/AndroidAssetStudio/icons-notification.html#source.space.trim=1&source.space.pad=0&name=ic_stat_example) - generate and download archive with your icons. Then just copy all drawable folders from the archive into _Assets/Plugins/Android/Notifications/res_ folder and set **the name of the icon without extension** as your small icon - **builder.setSmallIcon("myIcon")**      

2. [Large icon generator](http://romannurik.github.io/AndroidAssetStudio/icons-launcher.html#foreground.space.trim=1&foreground.space.pad=0&foreColor=607d8b%2C0&crop=0&backgroundShape=square&backColor=ffffff%2C100&effects=none) - generate and download archive with your icons. The archive will contain mipmap folders (mipmap-mdpi, mipmap-hdpi etc.). Copy the icons into corresponding **drawable** folders in _Assets/Plugins/Android/Notifications/res_ folder (icon from mipmap-hdpi into drawable-hdpi, mipmap-mdpi into drawable-mdpi etc.). Next, set **the name of the icon without extension** as your large icon - **builder.setLargeIcon("myLargeIcon")**   

```c#
int     id      = 1;
string  title   = "Custom icon";
string  body    = "You have some unfinished business!";

// Show notification in 5 minutes
TimeSpan delay = new TimeSpan(0, 5, 0);

// WARNING: icons should be in Assets/Plugins/Android/Notification/res/drawable(-mdpi etc.) folders
NotificationBuilder builder = new NotificationBuilder(id, title, body);
builder
.setDelay(delay)
.setSmallIcon("mySmallIconFilename") 
.setLargeIcon("myLargeIconFilename");

AndroidNotifications.scheduleNotification(builder.build());
``` 

## Settings custom sound
You can set custom sound for your notification. The sound should be located in _Assets/Plugins/Android/Notifications/res/raw_ folder. To set custom sound use builder.setSound("mySound") method. **Name of the sound file should be without extension.**

```c#
int     id      = 1;
string  title   = "Custom sound";
string  body    = "You have some unfinished business!";

// Show notification in 5 minutes
TimeSpan delay = new TimeSpan(0, 5, 0);

// WARNING: the sound should be in Assets/Plugins/Android/Notification/res/raw folder
NotificationBuilder builder = new NotificationBuilder(id, title, body);
builder
.setDelay(delay)
.setSound("mySoundFileName");

AndroidNotifications.scheduleNotification(builder.build());
``` 

## Cancel notification by id (both repeating and one-time)
To cancel the notification by id, simply call AndroidNotifications.cancelNotification(int id).

```c#
//cancel notification with id 7
AndroidNotifications.cancelNotification(7);
```

## Cancel all notifications
```c#
AndroidNotifications.cancelAll();
```

## Clear shown notifications

To clear certain notification use AndroidNotifications.clear(int id).    

```c#
// clear shown notification with id 7
AndroidNotifications.clear(7);
```

To clear all shown notifications use AndroidNotifications.clearAll().    

```c#
// clear all shown notifications
AndroidNotifications.clearAll();
```

## Updating notifications
To update one-time or repeating notification, schedule a notification with updated data and with ID of the notification you want to update.


## Show android toast notification

To show a toast notification use AndroidNotifications.showToast(string text).    


```c#
AndroidNotifications.showToast("Download completed");
```

## Notification editor
Plugin comes with editor extension that allows you to create notifications without the line of code. To open the notification editor window go to _Window -> Android Local Notifications_ . 

![](https://www.dropbox.com/s/6eaaozky16zga9g/window-general.png?raw=1)

In **Help** section you will find some useful links. In **Settings** section you can set custom Unity class if your activity extends _UnityPlayerNativeActivity_ . 
In **Notification List** section you can add and modify notifications. 

![](https://www.dropbox.com/s/oqemhfrotuqwsct/notifEditor.png?raw=1)

When you set custom notification sound or icons in editor window - they will be automatically copied to Notifications/res/drawable and Notifications/res/raw folders. **Though you will still need to add resized versions to drawable-hdpi and other folders using icon generators mentioned above**. 

For detailed information on notification options please refer to [official Android docs](http://developer.android.com/intl/ru/reference/android/support/v4/app/NotificationCompat.Builder.html)

## Schedule notification created in editor
You can get notification you created by its name you set in editor    
![](https://www.dropbox.com/s/h49gx2lixcgku8e/nameNotif.png?raw=1)

Next example shows scheduling of the notification created in editor with name **notificationOne**

```c#
string notificationName = "notificationOne";

// Method returns builder so you can config your notification afterwards if you want
NotificationBuilder builder = AndroidNotifications.GetNotificationBuilderByName(notificationName);

// If notification with specified name doesn't exist builder will be null
if (builder != null)
{
Notification notif = builder.build();
AndroidNotifications.scheduleNotification(notif);
}

```
###Push Notification With [OneSignal](https://documentation.onesignal.com/docs/installing-the-onesignal-sdk)
To configure push notification for android platform follow next steps:

1. Create [GMS](https://console.cloud.google.com/project) application by following [this](https://documentation.onesignal.com/docs/android-generating-a-gcm-push-notification-key) tutorial instruction.

2. Do [step 3](https://documentation.onesignal.com/docs/installing-the-onesignal-sdk) to config your `AndroidManifest.xml`

3. Go to `Assets/Area730/Notifications/PushNotification` drag and drop `PushController.prefab` or just add `CrossPlatformPushNotificationController.cs` script to your gameobject.

4. Fill the values in `CrossPlatformPushNotificationController.cs`.

Now you are ready to send notifications.
After these steps you will be able to send push notification using [OneSignal](https://onesignal.com/) service.



## Modifying a plugin

Source code of the plugin is included in the package. You can easily extend it if you want. Java library is built with **AndroidStudio**. There are 2 tasks in _build.gradle_ file you should modify - **deleteOldJar** and **exportJar**.      
![](https://www.dropbox.com/s/qbqzhfys4fwa0w4/gradleTasks.png?raw=1)     

In **deleteOldJar** task set path to the jar file you will export so every time you run a new build the old version will be deleted. In **exportJar** set the path where you want to export your jar.     

To export jar from AndroidStudio go to _Gradle Projects/Tasks/Other_ and run **exportJar** task.

In Unity plugin is in _Assets/Plugins/Android/Notifications_ folder. It is stored as android library project.

To debug this plugin in AndroidStudio add **Area730Log** log tag to you logcat filter.

## Other
All classes are located in **Area730.Notifications** namespace    

Example scene with sample code is included in the package (_Assets/Area730/Notifications/Examples_)     