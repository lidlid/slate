---
title: Sodyo API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - http
  - shell: cURL

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

Version 1.0 of the Sodyo APIs enables full campaign management and full content management for content of type “immediate action”. <br>
Content type of “Sodyo Ad” is not supported in the API at this time.

## API Access
The Sodyo administrator must enable API support for a project to allow interacting with the Sodyo system. If the API is not enabled in your project, please feel free to contact us and [request API access](mailto:portalsupport@sodyo.com).

## Supported Functionality
The Sodyo API provides the ability to perform content and campaign related operations.

<aside class="notice">All content & campaign operations are available for content / campaigns of type immediate action only. Referencing content / campaigns of type SODYO_AD via the API will return <code>404-Not Found</code></aside>

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

## Supported APIs
Supported APIs include:

### Content
* Get all content items
* Get content item by name
* Get content item by UUID
* Create content item
* Update content item
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

# Getting Started

## Prerequisites
Before you can start using the Sodyo API, you need to do the following:
* Create an account in the Sodyo Portal
* Create a project in your account
* Have the API enabled in your project by the Sodyo administrator by [contacting](mailto:portalsupport@sodyo.com) us
* Create an API Key

## Building an API Call
An API call to the Sodyo server must include the following components:
* The Host. The host for Sodyo API requests is always <code>cms.sodyo.com</code>. All API access is via the <code>https</code> protocol.
* An Authorization Header
* An API Key within the Authorization Header
* A Request. When submitting data to a resource via POST or PUT, you must submit your payload in JSON.

## API Response Messages
All responses delivered via the Sodyo API are returned in JSON format with UTF-8 charset. This is specified by including the content-type header in responses indicating application/json;charset=UTF-8.  

## API Endpoint Structure
The following endpoint pattern is used for all external integration API endpoints:

/integration/rest/api/​v1/entity​/{entity-id}/action

Where:

* First part​ is static prefix for all API calls (/integration/rest/api)
* Second part​ is the API version number (/v1)
* Third part​ is the entity name (/entity)
* Fourth part​ is entity UUID (/{enitity-id})
* Fifth part​ is an action (/action)

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

<aside class="warning">For security reasons, Sodyo does not save the API key that is generated for use with an application. Only the hash value is saved for authentication purposes. As such, there is no way to recover the application key if lost. Make sure to copy and save your key in a secure place for use in your code. Once the window is closed, it is not possible to view the key again.
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


# API Reference
## Sodyo Web API V1
The Sodyo API is a RESTful easy to integrate with API providing a full set of functionality for managing content and campaigns.

## Example
```shell
  curl https://sodyo-cms.herokuapp.com/integration/rest/api/v1/content
  -H "Content-Type: application/json"
  -H "X-AUTH-TOKEN: Developer-API-Key"
```

> Make sure to replace `Developer-API-Key` with your API key.

## Using the Sodyo Web API

### Authentication
Every request made via the Sodyo API must be authenticated by including an <code>X-AUTH-TOKEN</code> header as part of the request. The <code>X-AUTH-TOKEN</code> is the API Key assigned to the developer project in the Sodyo portal. When the Sodyo server receives a request from an application, the key hash value is used to authenticate the application with the hash value stored in the server. This provides a secure mechanism that prevents any unauthorized access to the system.

```shell
  curl https://sodyo-cms.herokuapp.com/integration/rest/api/v1/content
  -H "Content-Type: application/json"
  -H "X-AUTH-TOKEN: Developer-API-Key"
```
> Make sure to replace `Developer-API-Key` with your API key.

### Requests
All requests to the Sodyo Web API must be made via HTTPS. It is best practice to include the Content-Type: application/json header in all requests.<br>
The Sodyo Web API is completely RESTful and accepts GET, POST, PUT, and DELETE requests, depending on the resource.

```shell
  curl -X POST "https://sodyo-cms.herokuapp.com/integration/rest/api/v1/content/"
  -H "Content-Type: application/json"
  -H "X-AUTH-TOKEN: Developer-API-Key"
  -d "{\"name\": \"Call content 15\",\"description\": \"desc 15\",\"content\": {\"actionType\": \"DATA\",\"params\": {\"data\": \"blah blah blah\"}}}"
```
> Make sure to replace `Developer-API-Key` with your API key.

```http
POST /integration/rest/api/v1/content/ HTTP/1.1
Host: https:///sodyo-cms.herokuapp.com
Content-Type: application/json
X-AUTH-TOKEN: Developer-API-Key
{
  "name": "Content 1",
  "description": "description 1",
  "content": {
    "actionType": "DATA",
    "params": {
      "data": "blah blah blah"
    }
  }
}
```
> Make sure to replace `Developer-API-Key` with your API key.

### Responses
The Sodyo Web API provides response codes that indicate the status of the API request.

API Request:
```http
GET /integration/rest/api/v1/content HTTP/1.1'</code>
````

API Response:
```http
HTTP/1.1 200 OK
Content-Type: application/json
[
  {
      "uuid": "0d2712a6-6418-432d-bee5-f22e75e08286",
      "name": "Content 1",
      "description": "Descrption 1",
      "content": {
          "actionType": "DATA",
          "params": {
              "data": "hello"
          }
      }
    }
]
```

### Errors
The Sodyo API provides an indication when an error condition occurs. An API call that generated an error condition will include an error code along with a description where applicable.
See below a list of errors that may be returned by the API:

| Code 	| Reason               	| Description                                                            	|
|------	|----------------------	|------------------------------------------------------------------------	|
| 400  	| Bad Request          	| Validation Error                                                       	|
| 400  	| Bad Request          	| Duplicate Name (Content, Campaign)                                     	|
| 400  	| Bad Request          	| CONTENT_NOT_FOUND, Content with such ID does not exist in the account  	|
| 401  	| Unauthorized         	| Trying to access the API without a valid authorization header          	|
| 404  	| Not Found            	| Trying to access the non existent entities                             	|
| 404  	| Not Found            	| Trying to access entities which don’t belong to the API key being used 	|
| 507  	| Insufficient Storage 	| Reached account campaign limit                                         	|

# Content API

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
