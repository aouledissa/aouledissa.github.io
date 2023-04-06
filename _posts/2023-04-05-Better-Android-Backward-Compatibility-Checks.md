---
layout: post
title:  "Better Android Backward Compatibility Checks 👍"
data: 2023-04-05 03:43:00
category: Android
tags: [android,backward-compatibility]
---

As an android developer, did you ever grew so frustrated with checking the device's Android OS version when using some Android SDK feature that has different implementations between Android versions? 🤔

How many times you had to write this nifty check?

```kotlin
 if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
    // do something 🙋‍♂️
 } else if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
    // yet do another thing 🫣
 } else {
    // Euhh! do this at last 😩
 }
```

Should we all agree that this is very **ugly**, **repetitve**, **error prone** and mostly **not testeable**? Well! luckily our job is to make our job easier 🤓

♻️ Let's transform this garbage into something useful!

## Utilities to help you out ⚙️

We'll define for each android SDK level (that we're using) a utility function that should return a boolean to pass the check of the SDK level

```kotlin
@ChecksSdkIntAtLeast(api = Build.VERSION_CODES.N)
fun isNougatOrAbove(): Boolean = Build.VERSION.SDK_INT >= Build.VERSION_CODES.N

@ChecksSdkIntAtLeast(api = Build.VERSION_CODES.LOLLIPOP)
fun isLollipopOrAbove(): Boolean = Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP
```
{: file="SdkUtils.kt" }

Now we can rewrite the previous code as the following:

```kotlin
when {
    isNougatOrAbove() -> { // do something 🙋‍♂️}
    isLollipopOrAbove() -> { // yet do another thing 🫣 }
    else -> { // Euhh! do this at last 😩 }
}
```

✅ Done! no need to worry again about the previous ugly syntax, just use the new functions to do the check for you.

## Write it, mock it, test it --> PASS ✅

The rule of 👍 in software engineering is **DO NOT EVER TRUST YOUR CODE!**

If you ask yourself _how can I test this code?_ 

```kotlin
 if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
    // do something 🙋‍♂️
 } else if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
    // yet do another thing 🫣
 } else {
    // Euhh! do this at last 😩
 }
```

you will quickly notice that you unfourtunately can't, well at least not without resorting to **Reflection** which will definitely make your test `FLACKY` and in most cases will fail. Simply because mocking `final` properties in not possible with most of mocking frameworks (i.g: `mockito` or `mockk`).

However, mocking the return of a function is 💯 possible and also **recommended** 😉. With that in mind testing your code using `mockk` becomes as easy as the following:

```kotlin
// prepare the toplevel function for static mocking
mockkStatic(::isNougatOrAbove)
// mock the response
every { isNougatOrAbove() } returns true

// assert the code block is called or some expected results are observed
```
{: file="AwesomeFeatureTest.kt" }

And there you go! I believe now life is much better 😌 And as always happy coding and may the **Source** be with you!
