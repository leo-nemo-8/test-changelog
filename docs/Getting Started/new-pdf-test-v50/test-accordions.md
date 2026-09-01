---
title: test Accordions
deprecated: false
hidden: false
metadata:
  robots: index
---
In this document, you can explore concise troubleshooting tips and FAQs for In-App Notifications. You can click on each heading to expand sections for detailed information.

# FAQs

<details>

<summary>Sending In-App Messages</summary>

### Is it possible to send a GIF in an in-app message?

We do support GIFs within the interstitial in-apps for users whose CleverTap SDK version is above 3.3.0, and you can send the GIFs in the In-App campaigns by writing a custom HTML as well.<br />Refer to the [Aspect Ratio and Image Size Guide](https://docs.clevertap.com/docs/create-message-inapp).

### Does the in-app message support GIF files?

The CleverTap SDK version 3.3.0 and above supports GIF within the interstitial in-app messages. You can also send GIF files in in-app campaigns using a custom HTML.

For more information, refer to [Template Aspect Ratio and Image Size Guide](https://docs.clevertap.com/docs/inapp-campaigns#section-template-aspect-ratio-and-image-size-guide).

### Can I handle the URL at the client side for in-app campaigns?

This is supported for CleverTap Android SDK version 3.6.1 and above and CleverTap iOS SDK version 3.7.1 and above. It works for the following templates:

- Cover
- Interstitial
- Half-interstitial
- Header
- Footer

You can add the key-value pair for a button-click in the _What_ section of the campaign creation screen:


<Image src="https://files.readme.io/d2a7f0b5291f4aae75ab6fb45cada14610b056aa5ba8be869642921299f6eb0a-In_App_Key_Value.png" align="center" border={true} />


You must implement the button-click callback in your [iOS](https://developer.clevertap.com/docs/ios#section-in-app-notification-button-on-click-callbacks) and [Android](https://developer.clevertap.com/docs/android#section-in-app-notifications) apps where you can get these key-value pairs.

### How do you add custom fonts in in-app campaigns?

You can use custom fonts in a regular HTML page by importing them in the head tag of the HTML code, then using them in the CSS.

`<head> tag of HTML`

```html
<link href='[PLACE SOURCE URL FOR FONT HERE]' rel='stylesheet' type='text/css'>
  <link href="https://fonts.googleapis.com/css2?family=Oswald:wght@700&family=Roboto&display=swap" rel="stylesheet">
```

`CSS`

```css
.sample-class { 
  font-family: '[Name of font]', arial, serif; 
}
font-family: 'Oswald', sans-serif;
```

<Callout icon="📘" theme="info">
  ### Note

  You must select custom HTML to use your custom font in the in-app campaign.
</Callout>

</details>

<details>

<summary>In-App Message Analysis</summary>

### How do you analyze the CTA button for in-app campaigns?

The data for a number of clicks on each CTA button is available under the _Analytics_ section in the dashboard.

Follow the steps below:

1. In _Events_, query for the _Notification Clicked_ event filtered by the campaign ID, then in the result, navigate to _Trend & Properties_.
2. Scroll down to the _Event property_ table and select the event property as "wzrk_c2a" from the dropdown to see a distribution of clicks across the various buttons on that respective email campaign.


<Image src="https://files.readme.io/9223cbd-New_final_CTA_Button_Analysis_2.png" alt="1163" align="center" border={true} />


### How do I analyze the CTA button for in-app campaigns?

To view the data for a number of clicks on each CTA button, navigate to _Analytics_ > _Events_ from the dashboard. Then, follow the steps below:

1. Select the _Notification Clicked_ event and filter it by the campaign ID.<br />Click the **View Details** button. The event details page displays.
2. Click the **Trend & Properties** tab.
3. Scroll down to the _Event property_ table and select the event property as "wzrk_c2a" from the list.
4. A distribution of clicks across the various buttons on that respective in-app, pop-up campaign displays.


<Image src="https://files.readme.io/ddcf857-CTA_Wzrk_property.png" alt="1439" align="center" border={true} />


### My in-app shows 0 views even after qualifying multiple users. What could be wrong?

The in-app notifications require SDK version 3.5.1 and above. The warm-up time for the campaign is three to five minutes. Check the following before scheduling an in-app campaign:

- In-apps require the app to be open and the event must be fired from the mobile SDK. Hence, the in-app campaigns do not work on an event that is triggered via an API.
- There cannot be multiple in-apps running on the same event. The in-app that was created first is displayed, and the other events show 0 views.

</details>

<details>

<summary>In-App Notification Rendering</summary>

### Why does the in-app notification give an error FCM payload too large?

The test in-app is sent within a push notification. The limit for push notification payload as defined by FCM is 4KB beyond which the notification is not sent.

If the in-app notification has a large HTML code or a heavy size, it shows an error and the test in-app is not sent; however, this does not apply to a live campaign because there are no push notifications.

To resolve this error, you can first schedule a live campaign with this HTML for internal users.

### Why does the in-app campaign not render an API event?

The in-app works for SDK events, such as events raised from the device. There can be delays in the APIs and by design, the in-app is meant to be sent in real-time to the user. Therefore, the in-apps must be triggered by a mobile SDK event.

### Why can I not see the videos on _Interstitial_ in-app notifications?

The video must meet the following requirements:

- The video must be in MP4 format.
- The video size must be lower than 50MB.
- The dependencies which are required to include audio/video in the in-app notification are included.

Add the following dependencies below in your application’s build.gradle file:

```groovy
Dependencies {

 implementation 'com.google.android.exoplayer:exoplayer:2.8.4'

 implementation 'com.google.android.exoplayer:exoplayer-hls:2.8.4'

implementation 'com.google.android.exoplayer:exoplayer-ui:2.8.4'}
```

### Why does the in-app notification disappear from the user's device in a few seconds?

The in-app notification is dismissed if you display it over the splash/logo screen because the in-app is automatically dismissed when the underlying splash screen is dismissed. Exclude the splash screen to avoid displaying an in-app.

Add the following meta-data in the AndroidManifest.xml to exclude the splash screen from your in-app notification:

```xml
{code}
<meta-data
android:name="CLEVERTAP_INAPP_EXCLUDE"
android:value="YourSplashActivity1, YourSplashActivity2" />
{code}
```

Add the activities to exclude from in-app notifications in the `android:value` row of the metadata tag.

</details>

<details>

<summary>In-App Interaction and Configuration</summary>

### How can one close the in-app notification with the click of a button?

The workaround is to create a button and add the reference of that button to a dummy hyperlink. This closes the in-app notification because the link does not exist, so there is no redirection.

Here is a sample link: wzrk://thisisadummylink

### How do I exclude in-app from the Android activity?

If your application has a splash screen (for example, logo screen or loading page) displayed on app launch, then the in-app triggered on _App Launch_ would be attempted to be displayed on this screen. As soon as this screen is dismissed, the in-app will be dismissed too.

In these types of cases, the splash screen must be excluded. Mention the _Activities_ on which you do not want in-app notifications to show in the android:value of the metadata tag in the AndroidManifest.xml file.

```xml
<meta-data
    android:name="CLEVERTAP_INAPP_EXCLUDE"
    android:value="YourSplashActivity1, YourSplashActivity2" />
```

For more information, refer to [In-app Notifications](https://developer.clevertap.com/docs/android#section-in-app-notifications).

### How do I close an in-app notification automatically after a specific duration?

You can use the set `timeout` function in custom HTML to dismiss the in-app.

Following is a sample code:

```html
<!DOCTYPE html >
<html id="myClass">
<head>
    <title></title>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.0/jquery.min.js"></script>
</head>
<body style="background: white">
<h1>Test</h1>
<!-- <button style="visibility: hidden;" id = "theSubmitButton" onclick="window.location.href='wzrk://this_leads_to_nowhere'">Click me</button>  -->

<p><button id="theSubmitButton" onclick="window.location.href='wzrk://exit'" style="visibility: hidden;">Click me</button></p>

<p id="demo"> </p>
<script>
   setTimeout(function myFunction() {
       document.getElementById('theSubmitButton').click();
   }, 1000); // Change time for whatever miliseconds required.
   </script></body>
</html>
```

</details>

# Troubleshooting

<details>

<summary>Testing</summary>

### Why do I receive a CleverTap push notification even after sending a test in-app?

An in-app test is always sent like a push notification. The entry action (event) does not matter. When you click the _send a test notification_ option, this message, "Touch this notification to open your app and view your view your notification" is sent out as a push notification. When this notification is clicked, the app opens and displays your in-app notification. This is applicable only to a test notification. The live campaign will schedule as usual and show the in-app after a user qualifies (performing the entry event).

```groovy
Dependencies {

 implementation 'com.google.android.exoplayer:exoplayer:2.8.4'

 implementation 'com.google.android.exoplayer:exoplayer-hls:2.8.4'

implementation 'com.google.android.exoplayer:exoplayer-ui:2.8.4'}
```

</details>
