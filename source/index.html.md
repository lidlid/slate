---
title: Sodyo SDK Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - objective_c
  - java
  
toc_footers:
  - <a href='mailto:portalsupport@sodyo.com'>Support</a>
  - <a href='https://www.sodyo.com'>Documentation Powered by Sodyo</a>

includes:

search: true
---

# Sodyo Introduction

Sodyo’s technology creates an interactive experience with items in the physical world. Just point your mobile phone camera at any item, place or media display assigned with a Sodyo code and reveal layers of information, content and links.

Sodyo enhances the TV commercial experience by strengthening the sales funnel. Viewers can interact
immediately with the television via with their smartphone and:

* Point and Purchase – The viewer points their phone or mobile device to the TV screen; a web
page instantly appears on their phone which allows them to purchase a product, review their order and complete the purchase if they so choose.

* Point and Learn More – The viewer points their phone or mobile device to the TV screen, and
instantly receives additional information about a product or service.

* Point and Vote – The viewer points their phone or mobile device to the TV screen and votes for
content choices offered on the TV – favorite singer, favorite news story or whatever poll or vote
your TV station offers.

# About the Sodyo SDK

## Purpose
With Sodyo’s SDK, your mobile phone application can now "see" from a distance and recognize any
2D/3D item in the physical world. Apply a Sodyo code to any indoor/outdoor item, place or media display and your mobile device will detect and interact with it from up to dozens of meters.

The Sodyo SDK allows application developers to connect their application's data to Sodyo codes.

## Components
Sodyo’s SDK is a software library that can be easily integrated into any mobile application to provide the ability to scan and receive interactive content. As part of the SDK Sodyo provides:

* A software library for iOS / Android

* An example application that shows the developer how to use the SDK

## Example Projects
Sodyo provides example projects for iOS and Android to be used as a reference for developers.

iOS: An example project is available [here](https://github.com/SodyoSDK/SodyoSDKPod).

Android: An example project is available [here](https://bitbucket.org/sodyodevteam/sodyosdkexample-android.git)

## System Description and Workflow
The Sodyo system consists of the SDK integrated in a mobile application, the Sodyo Server and Sodyo’s Portal / content management system.

## Operational Scenario
* Create content using the Sodyo Portal
* Create a campaign using the Sodyo Portal. A Sodyo code is assigned to the campaign.
* Integrate Sodyo’s SDK to make your mobile application “Sodyo enabled”.
* Run your mobile application and open the Sodyo Scanner to start detecting Sodyo codes.
* Once a Sodyo code is detected, the Sodyo SDK will deliver the interactive content associated with
the scanned code to the mobile application.

## Supported Platforms
Sodyo currently runs on iOS 9 and higher as well as on Android 4.1 and higher (minSdkVersion >= 16
and targetSdkVersion <=24).

We have language bindings in Java for Android Developers and Objective C for iOS developers. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

## Supported Languages
Sodyo currently supports English. The language displayed on Sodyo user interface is determined by the language set in the device's settings.

If the language set on the device is different than the supported languages, the Sodyo user interface will default to English.

# Sodyo License Agreement
By opening the package, downloading the product or using any of its components, you are consenting to be bound by the [Sodyo license agreement](http://cms.sodyo.com/assets/docs/SDK_License_Agreement.pdf). 

If you do not agree to all of the terms of this agreement, do not download and install this package or proceed.

# Support
Should you have any questions or need any help, please do not hesitate to contact us at
[Support](mailto:portalsupport@sodyo.com).

# Android SDK

## Installation
Before you begin, please make sure that the minSdkVersion >= 16 and targetSdkVersion <=24 (in your \app\build.gradle file).

```java
	allprojects {
		repositories {
			maven {
				credentials {
					//Bitbucket credentials
					username "sodyoltd@gmail.com"
					password "sodyo@sdk"
				}
				url "https://api.bitbucket.org/1.0/repositories/sodyodevteam/sodyosdk-android/raw/releases"
			}
		jcenter()
		}
	}

```
1. In your project build.gradle file add the following:

```java
android {
...
	packagingOptions {
	exclude "META-INF/DEPENDENCIES.txt"
	exclude "META-INF/LICENSE.txt"
	exclude "META-INF/NOTICE.txt"
	exclude "META-INF/NOTICE"
	exclude "META-INF/LICENSE"
	exclude "META-INF/DEPENDENCIES"
	exclude "META-INF/notice.txt"
	exclude "META-INF/license.txt"
	exclude "META-INF/dependencies.txt"
	exclude "META-INF/LGPL2.1"
	exclude "META-INF/ASL2.0"
	exclude "META-INF/maven/com.google.guava/guava/pom.properties"
	exclude "META-INF/maven/com.google.guava/guava/pom.xml"
        }
}
dependencies {
...
	compile "com.sodyo:sodyo-android-sdk:3.00.04"
}
```
<br><br><br><br><br><br><br><br><br><br><br><br>

<ol start="2">
<li>In your app build.gradle file add the following:</li>
<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
</ol>

## Initialization
Prior to launching the Sodyo Scanner, the application should initialize the Sodyo engine. This will instantiate the Sodyo SDK and load your app data that is binded with your Sodyo Markers. The initialization method should be from your Main activity class.

1. Call the initialization method from your main activity onCreate():

```java
import com.sodyo.sdk.Sodyo;
@Override
public void onCreate() {
// init Sodyo engine App
Sodyo.init(application, SODYO_APP_KEY, initializationCallback);
// define a detection callback
Sodyo.getInstance().setSodyoScannerCallback(this);
...
super.onCreate();
}
```

<br><br><br><br><br><br><br><br><br><br>
<aside class="notice">
Parameters Description:

<ul>- application: application context</ul>

<ul>- SODYO_APP_KEY: the application key assigned to the account</ul>

<ul>- initializationCallback: Used to provide an async callback to identify whether Sodyo SDK initialization failed or finished successfully.</ul>
</aside>

<aside class="success">
The <code>SODYO_APP_KEY</code> can be found in the application provider / SDK module of your account in the Sodyo Portal
</aside>

<ol start="2">
<li>Make your activity implement the SodyoInitCallback interface to get notified on the result of the initialization process:</li>
</ol>

```java
import com.sodyo.sdk.SodyoInitCallback;

public void onSodyoAppLoadSuccess();
public void onSodyoAppLoadFailed(String error);
```

<br><br><br><br><br><br><br><br>

## Marker Detection
The app invokes the Sodyo Scanner to start detecting markers. Sodyo scans for Markers in the camera
field of view and uses a callback procedure for each detected Marker. The Sodyo Scanner will keep
running until it is finished.

```java
import com.sodyo.sdk.SodyoScannerCallback;

Sodyo.getInstance().setSodyoScannerCallback(this);
```

1. Set a callback object that implements SodyoScannerCallback and receives the scanning results.
<br><br><br><br>
Make your activity implement the SodyoScannerCallback interface to get notified on marker scan:

```java
@Override
public void onMarkerDetect(String data,String error) {
...
}
```
<br><br><br><br><br>

<ol start="2">
<li>Launch SodyoScanner Activity using startActivityForResult(Intent, int). For example:</li>
</ol>

```java
import com.sodyo.app.SodyoScanner;

private static final int SODYO_REQUEST_CODE = 1111;

Intent intent = new Intent(MainActivity.this, SodyoScanner.class);
startActivityForResult(intent, SODYO_REQUEST_CODE);
```
<br><br><br><br><br><br><br>

<ol start="3">
<li>When ready, dismiss the SodyoScanner activity using finishActivity(int requestCode). Use the
request code that was used in the previous section. For example:</li>
</ol>

```java
finishActivity(SODYO_REQUEST_CODE);
```
<br><br>

## Intent Filter
Add additional intent filter for application to handle sodyo actions:

```java
<intent-filter>
	<action android:name="android.intent.action.VIEW"/>
	<category android:name="android.intent.category.DEFAULT"/>
	<category android:name="android.intent.category.BROWSABLE"/>
	<data android:scheme="sodyo"/>
</intent-filter>
```

<br><br><br><br><br><br>

## Setting Up ProGuard
Add the following lines to your project’s proguard-rules.pro file:

```java
-dontwarn com.sodyo.**
-keep class com.crittercism.**
-keepclassmembers class com.com.sodyo.** { *; }
```

<br><br><br><br>

# IOS SDK

## Installation
1. Sodyo’s SDK is installed using CocoaPods. For more information on installing CocoaPods on
your system, read the [CocoaPods website](http://cocoapods.org).

2. In your project’s pod file, add:


```objective_c
pod 'SodyoSDK'
```

<br><br>

<ol start="3">
<li>Add an entry for NSCameraUsageDescription in your info.plist file.</li>
</ol>

## Initialization
Prior to launching the Sodyo Scanner, the application should initialize the Sodyo engine. This will instantiate the Sodyo SDK and load your application’s data that is binded with your Sodyo
Markers.

The initialization method should be called once in your AppDelegate or anywhere in the code.

1. Include the header file:

```objective_c
#import <SodyoSDK/SodyoSDK.h>
```
<br><br><br>

<ol start="2">
<li>Call the initialization method:</li>
</ol>

```objective_c
[SodyoSDK LoadApp:<SODYO_APP_KEY> Delegate:NSObject<SodyoSDKDelegate> MarkerDelegate:NS
Object<SodyoMarkerDelegate> PresentingViewController:UIViewController*];
```
<br><br><br><br>
<aside class="notice">
Parameters Description:

<ul>- SODYO_APP_KEY: the application key assigned to the account</ul>

<ul>- SodyoSDKDelegate: delegate that allows getting notified on the result of the Sodyo SDK initialization process.</ul>

<ul>- MarkerDelegate: An object that conforms to the SodyoMarkerDelegate protocol (see below).</ul>

<ul>- PresentingViewController: For SodyoAd markers, the UIViewController that will be used to present SodyoAd’s UIViewController.</ul>

</aside>

<aside class="success">
The <code>SODYO_APP_KEY</code> can be found in the application provider / SDK module of your account in the Sodyo Portal
</aside>

<ol start="3">
<li>Optionally Implement the SodyoSDKDelegate functions below to get notified on the result of
the initialization process</li>
</ol>

```objective_c
(void) onSodyoAppLoadSuccess:(NSInteger)AppID;

(void) onSodyoAppLoadFailed:(NSInteger)AppID error:(NSError *)error;
```
<br><br><br>

## Marker Detection
The application invokes the Sodyo Scanner to start detecting markers. Sodyo scans for Markers in the camera’s field of view and calls back a delegate function for each detected Marker.

The Sodyo Scanner will keep running until it is dismissed.

1. Launch Sodyo Marker Scanner by presenting it as a UIViewController. For example:

```objective_c
#import <SodyoSDK/SodyoSDK.h>
...
[self presentViewController:[SodyoSDK initSodyoScanner] animated:YES completion:nil];
```
<br><br><br><br><br>

<ol start="2">
<li>For data markers, implement the following delegate method. Sodyo will call this method upon a Sodyo Data Marker detection:</li>
</ol>


```objective_c
- (void) SodyoMarkerDetectedWithData:(NSDictionary*)Data;
```

<br><br><br><br>

<aside class="notice">
Note:
<ul>- Data: A JSON object: {SodyoMarkerData: <string data>}</ul>
</aside>

<aside class="success">
Not relevant when using Sodyo’s Ad platform.
</aside>

<ol start="3">
<li>When ready, dismiss the Sodyo Marker Scanner. Use the UIViewController that you presented
earlier. For example:</li>
</ol>

```objective_c
[self dismissViewControllerAnimated:YES completion:nil];
```

<br><br><br><br><br><br><br>

# Migration
Migrating from previous versions of the SDK

## Android
To upgrade from previous versions of the SDK, make sure to pull the latest version of the SDK as
indicated in the dependencies of the app build.gradle file. In addition, change your `init` function as indicated in the initialization section.

## IOS
To upgrade from previous versions of the SDK, make sure to perform a `POD update` operation.

