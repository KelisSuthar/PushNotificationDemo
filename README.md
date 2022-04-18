# Welcome To PushNotificationDemo #

**Steps To implement The PushNotification in Your Project**

[**Note - This proect is Only gives you example of implementing push notifications in react native app**]

(https://enappd.com/blog/firebase-push-notifications-in-react-native/81/)

(https://www.npmjs.com/package/react-native-push-notification)

[**Note - I have refer this site to do this project gives you best Exaple as well**] 

**I Have Diveded thsi project in two parts first is in react native and second in firebase consol

=> Create New Project In React native 

    react-native init APPNAME
    
=> Implement Firebase Notifications
  
  first you need in install firebase pushnotifiation into this
  
    npm install --save react-native-push-notification
=> Make Changes in Android 
    
  -> in build.gradle file
   
        ext {    
            buildToolsVersion = "28.0.3"
            minSdkVersion = 16
            compileSdkVersion = 28
            targetSdkVersion = 28
            supportLibVersion = "28.0.0"
            googlePlayServicesVersion = "16.1.0" // default: "+"//ADD THIS
            firebaseVersion = "17.3.4" // default: "+"//ADD THIS
            }
    
    
   -> Add Permissions in andoroid manifest file

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />
        <permission
            android:name="${applicationId}.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="${applicationId}.permission.C2D_MESSAGE" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
        
   ->Add Some Meta deta and services in application tag in manifest file   
    
         <!-- Change the value to true to enable pop-up for in foreground on receiving remote notifications (for prevent duplicating while showing local notifications set this to false) -->
        <meta-data  android:name="com.dieam.reactnativepushnotification.notification_foreground"
                    android:value="false"/>
        <!-- Change the resource name to your App's accent color - or any other color you want -->
        <meta-data  android:name="com.dieam.reactnativepushnotification.notification_color"
                    android:resource="@color/white"/> <!-- or @android:color/{name} to use a standard color -->

        <receiver android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationActions" />
        <receiver android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationPublisher" />
        <receiver android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationBootEventReceiver">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.intent.action.QUICKBOOT_POWERON" />
                <action android:name="com.htc.intent.action.QUICKBOOT_POWERON"/>
            </intent-filter>
        </receiver>

        <service
            android:name="com.dieam.reactnativepushnotification.modules.RNPushNotificationListenerService"
            android:exported="false" >
            <intent-filter>
                <action android:name="com.google.firebase.MESSAGING_EVENT" />
            </intent-filter>
        </service>
        
  -> Add Below Code to integrate push notification
    
        include ':react-native-push-notification'
        project(':react-native-push-notification').projectDir = file('../node_modules/react-native-push-notification/android')
        
        
  ->Make Sure you have this in your build.gradle file

      dependencies {
          ...
          classpath('com.google.gms:google-services:4.3.3')
          ...
      }
      
    also this in dependency area
    
        dependencies {
          ...
          implementation 'com.google.firebase:firebase-analytics:17.3.0'
          implementation project(':react-native-push-notification')
          ...
        }

        apply plugin: 'com.google.gms.google-services'
        
  ->Add this if you dont allow autolinking in your android project

        include ':react-native-push-notification'
        project(':react-native-push-notification').projectDir = file('../node_modules/react-native-push-notification/android')
=> Add A file in react native

    Now open your react native app in your aditor and add a file name NotificationController.js(You Can name any as you like)
      
        import PushNotificationIOS from "@react-native-community/push-notification-ios";
        import PushNotification from "react-native-push-notification";

        // Must be outside of any component LifeCycle (such as `componentDidMount`).
        PushNotification.configure({
          // (optional) Called when Token is generated (iOS and Android)
          onRegister: function (token) {
            console.log("TOKEN:", token);
          },

          // (required) Called when a remote is received or opened, or local notification is opened
          onNotification: function (notification) {
            console.log("NOTIFICATION:", notification);

            // process the notification

            // (required) Called when a remote is received or opened, or local notification is opened
            notification.finish(PushNotificationIOS.FetchResult.NoData);
          },

          // (optional) Called when Registered Action is pressed and invokeApp is false, if true onNotification will be called (Android)
          onAction: function (notification) {
            console.log("ACTION:", notification.action);
            console.log("NOTIFICATION:", notification);

            // process the action
          },

          // (optional) Called when the user fails to register for remote notifications. Typically occurs when APNS is having issues, or the device is a simulator. (iOS)
          onRegistrationError: function(err) {
            console.error(err.message, err);
          },

          // IOS ONLY (optional): default: all - Permissions to register.
          permissions: {
            alert: true,
            badge: true,
            sound: true,
          },

          // Should the initial notification be popped automatically
          // default: true
          popInitialNotification: true,

          /**
           * (optional) default: true
           * - Specified if permissions (ios) and token (android and ios) will requested or not,
           * - if not, you must call PushNotificationsHandler.requestPermissions() later
           * - if you are not using remote notification or do not have Firebase installed, use this:
           *     requestPermissions: Platform.OS === 'ios'
           */
          requestPermissions: true,
        });
**\\\\\\\\\\Your Part for the react native is complete here\\\\\\\\\\**


=>Now on the firebase consol add new project (If you dont have any)
   
     ->Create a new project in firebase consol 
     
     ->Add an application regarding your needs(iOS or Android)
     
     ->After adding app you find a suggestion by firebase to download google-services.json file download that file and add the file in andori project file
        (if you dont know how to add that file in andorid project  i am adding a lonk you can refer - https://www.youtube.com/watch?v=qXFJ6E1D-c8)
        
=>Test Notification

  ->To test ntofication  open firebase consol and open cloud messenging , type your test ntoification and click on send notification
      
**Thankyou**
