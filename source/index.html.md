---
title: Sodyo API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - http
  - shell
  
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

# About the Sodyo API
The Sodyo API provides the mechanism to interact with the Sodyo solution directly from a customer system.

## Overview 
The Sodyo Server APIs enable developers to create, read, update and delete (CRUD) content and campaigns with Sodyo's Portal. Previously, these operations were only available via the Portal user interface (UI), however, there are cases that a customer will want to automate the creation of content items and campaigns, via code, to tightly integrate Sodyo with customer internal workflows and systems.

Version 1.0 of the Sodyo APIs enables full campaign management and full content
management for content of type “immediate action”. Content type of “Sodyo Ad” is not
supported in the API at this time.

## API Access
The Sodyo administrator must enable API support for a project to allow interacting with the Sodyo system. If the API is not enabled in your project, please feel free to contact us and [request API access](mailto:portalsupport@sodyo.com).

## Supported Functionality
The main functionality provided as part of the Sodyo API v1.0 includes:

* Authentication
	* The project administrator can configure API access keys to be used for integration
	* API authentication & authorization is based on the project API access key
* Content Operations
	* CRUD operations on content (limited to immediate action only)
	* Content adaptation to strict API structure
* Campaign Operations
	* CRUD operations on campaigns
	* Get Sodyo marker image API
	* Campaign activation/deactivation and status
* Error handling

# Authentication
Sodyo enables the generation of keys for use with the API. When the Sodyo server receives a request from an application, the key hash value is used to authenticate the application with the hash value stored in the server. This provides a secure mechanism that prevents any unauthorized access to the system.

<aside class="warning">
For security reasons, Sodyo does not save the API key that is generated for use with an application. Only the hash value is saved for authentication purposes. As such, there is no way to recover the application key if lost. Follow the process below to create your key and make sure to copy and save it in a secure place for use in your code.
</aside>

## Generating an API Key
To generate an API Key, follow the process outlined below:

1. Navigate to the [Sodyo Portal](https://cms.sodyo.com) and log into your account. 
2. Select your project.
3. Click on Settings. If API access is enabled for your project, a tab titled "API Integration" is visible. Click the "API Integration" tab.

<aside class="notice">The Settings module is only available for users that have owner or admin role in the project.</aside>

![API Integration Screen](/images/APIIntScr.PNG)

* In the API integration section you will see a list of currently defined API keys for the project. The following columns are shown:
	* Key Name
	* Key ID
	* Key

<aside class="notice">Note that the Key is not displayed. Hovering over the key will display a message that indicates that for security purposes the API key cannot be displayed</aside>

<ol start="4">
<li>Click the + sign to create a new API key. The Add New API Key Screen is displayed.</li>
</ol>
![API New Screen](/images/APINewScr.PNG)

<ol start="5">
<li>Name your key and click "create and view". The API Key Created Screen is displayed with the generated API key.</li>
</ol>
![API Created Screen](/images/APICrtScr.PNG)

The Sodyo API key has the following structure: <br>
SA​ . KEY_ID​ . HMAC_SIGNATURE (SA = Sodyo API)

<ol start="6">
<li>Click the copy button to copy the generated key for use in your application. After the key is copied click close to return to the main screen.</li>
</ol>

<aside class="warning">Make sure to copy and save you key in a secure place for use in your code. Once the window is closed, it is not possible to view the key again for security reasons. 
</aside>

![API Main Screen](/images/APIMainScr.PNG)

<aside class="warning">It is possible to delete an API key by clicking the X button at the end of the row for the specific key. The user is prompted to verify the delete operation. Once deleted the key can not be used for API access.</aside>

## Authentication Process
1. Every API call to the Sodyo server includes the “​X-AUTH-TOKEN​” header with the API key value.
2. The Sodyo Server performs the following checks to ensure authentication:
	* Validation that the API key exists
	* Validation of the HMAC signature
	* If one of the validation checks fail, a <code>401-Unauthorized</code> message is returned.
3. Every request that is authenticated, is authorized with the API role and its associated permission set. The API role is eligible to access external integration API endpoints in the
Sodyo system. Trying to access any other endpoint will return <code>403-Forbidden</code>.
4. As a part of the authorization process, a validation is performed to ensure that API usage is available for the project. If API usage is disabled for the project that the API key belongs to, the server will return <code>403-Forbidden</code>.


# Additional API Information

## API Endpoint Structure
The following endpoint pattern is used for all external integration API endpoints:

/integration/rest/api/​v1/entity​/{entity-id}/action

Where:

* First part​ is static prefix for all API calls (/integration/rest/api)
* Second part​ is the API version number (/v1)
* Third part​ is the entity name (/entity)
* Fourth part​ is entity UUID (/{enitity-id})
* Fifth part​ is an action (/action)

## API Functionality
The Sodyo API provides the ability to perform content and campaign related operations.

<aside class="notice">All content & campaign operations are available for content / campaigns of type immediate action only. Referencing content / campaigns of type SODYO_AD via the API will return <code>404-Not Found</code></aside>

Supported functions:

### Content
* Get all content items
* Get content item by name
* Get content item by UUID
* Create contant item
* Update contant item
* Delete content item

### Campaigns
* Get all campaigns
* Get campaign by name
* Get campaign by UUID
* Create campaign
* Update campaign
* Get campaign marker
* Enable campaign
* Disable campaign

# API Reference

## Get All Content
This function returns all content items in the project

> The above command returns JSON structured like this:

```json
[
	{
		"uuid": ​ "3d7f83f7-dcd5-4f03-ab97-2e1fb9056978"​ ,
		"name": ​ "Call content"​ ,
		"description": ​ "desc"​ ,
		"content": {
			"actionType": ​ "PHONE"​ ,
			"params": {
				"phone": ​ "89787978978"
			}
		}
	},
	{
		"uuid": ​ "3d7f83f7-dcd5-4f03-ab97-2e1fb9056977"​ ,
		"name": ​ "Data content"​ ,
		"description": ​ "desc"​ ,
		"content": {
			"actionType": ​ "DATA"​ ,
			"params": {
				"data": ​ "89787978978"
			}
		}
	}
]```

This endpoint retrieves all content items in the project.

### HTTP Request

`GET /integration/rest/api/v1/content`

### Query Parameters
None

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>





# Old
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

