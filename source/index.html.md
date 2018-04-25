---
title: Sodyo SDK Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - objective_c
  - java
  
toc_footers:
  - <a href='mailto:portalsupport@sodyo.com'>Support</a>
  - <a href='https://www.sodyo.com'>Documentation Powered by Sodyo</a>

includes:
  - errors

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

# The Sodyo SDK
With Sodyo’s SDK, your mobile phone application can now "see" from a distance and recognize any
2D/3D item in the physical world. Apply a Sodyo code to any indoor/outdoor item, place or media display and your mobile device will detect and interact with it from up to dozens of meters.
The Sodyo SDK allows application developers to connect their app's data to Sodyo codes.

# Components of the SDK
Sodyo’s SDK is a software library that can be easily integrated into any mobile application to provide the ability to scan and receive interactive content. As part of the SDK Sodyo provides:
* A software library for iOS / Android
* An example application that shows the developer how to use the SDK

## Example Projects
Sodyo provides example projects for iOS and Android to be used as a reference for developers.

iOS: An example project is available [here](https://github.com/SodyoSDK/SodyoSDKPod).
Android: An example project is available [here](https://bitbucket.org/sodyodevteam/sodyosdkexample-android.git)

# System Description and Workflow

## General
The Sodyo system consists of the SDK integrated in a mobile application, the Sodyo Server and Sodyo’s Portal / content management system.

## Operational Scenario
* Create content using the Sodyo Portal
* Create a campaign using the Sodyo Portal. A Sodyo code is assigned to the campaign.
* Integrate Sodyo’s SDK to make your mobile application “Sodyo enabled”.
* Run your mobile application and open the Sodyo Scanner to start detecting Sodyo codes.
* Once a Sodyo code is detected, the Sodyo SDK will deliver the interactive content associated with
the scanned code to the mobile application.

# Supported Platforms
Sodyo currently runs on iOS 9 and higher as well as on Android 4.1 and higher (minSdkVersion >= 16
and targetSdkVersion <=24).

We have language bindings in Java for Android Developers and Objective C for iOS developers. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Supported Languages
Sodyo currently supports English. The language displayed on Sodyo user interface is determined by the language set in the device's settings.

If the language set on the device is different than the supported languages, the Sodyo user interface will default to English.

# License Agreement
By opening the package, downloading the product or using any of its components, you are consenting to be bound by this agreement. If you do not agree to all of the terms of this agreement, do not download and install this package or proceed.

# Support
Should you have any questions or need any help, please do not hesitate to contact us at
[Support](mailto:portalsupport@sodyo.com).

# SDK Functions

## Installation

### Android
Before you begin, please make sure that minSdkVersion >= 16 and targetSdkVersion <=24 (in your \app\build.gradle file).

1. In your project build.gradle file the following:

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

2. In your app build.gradle file add the following:

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
### iOS
1. Sodyo’s SDK is installed using CocoaPods. For more information on installing CocoaPods on
your system, read [cocoapods documnetation](http://cocoapods.org).

2. In your project’s pod file, add:
``` objective_c
pod 'SodyoSDK'
``` 

3. Add an entry for NSCameraUsageDescription in your info.plist file.




This example API documentation page was created with [Slate](https://github.com/lord/slate). Feel free to edit it and use it as a base for your own API's documentation.



SDK Examples Apps
> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

