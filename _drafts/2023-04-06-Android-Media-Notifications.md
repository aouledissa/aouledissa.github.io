---
layout: post
title:  "Android Media Notifications: A Beautiful Chaos! 🤦‍♂️"
data: 2023-04-06 02:01:50
category: Android
tags: [android,media,notification]
toc: true
---

![Retro cassette player in garage](/assets/retro_cassette_player_in_garage%20.jpeg)
_Retro cassette player in garage by [E M](https://www.pexels.com/@e-m-3766677/)_

From **Spotify** to **Netflix** or **Youtube Premium**, one of the best user experience features these apps provide in their mobile app is so far the all mighty `Media Notification`. This nifty little gem serves as a handy shortcut, allowing users to manage their playback experience by pausing, stopping, rewinding, or skipping through content. And the best part? It does all of this without interrupting the user's flow while using other apps or forcing them to unlock their phone and navigate to the app just to make these changes. 

If you're an Android developer, you know that it's fairely easy to integrate this type of notifications in your app just by using the `Notification` or `NotificationCompat` builders. It's as easy as:

```kotlin
val notificationBuilder = NotificationCompat.Builder(context)
    .setContentTitle("Awesome Notification")
```

But what if we want to control a media playback, video or audio, directly from the system notification? 🤔

## A Media Notification to rule them all! 👑

To make a notification acts as a Media Notification, we will have to indicate to the `NotificationCompat.Builder` to use the `MediaStyle` using the `setStyle()` builder function.

```kotlin
val notificationBuilder = NotificationCompat.Builder(context)
    ..
    .setStyle(MediaTheme(mediaSession))
```

## Lost in versions maze 🌀 