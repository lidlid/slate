---
title: Sodyo API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - http
  - shell: cURL

toc_footers:
  - <a href='https://www.sodyo.com'>Sodyo Website</a>
  - <a href='https://github.com/lord/slate'>Documentation powered by Slate</a>

includes:

search: true
---

# Sodyo Introduction

Sodyo’s technology creates an interactive experience with items in the physical world. Just point your mobile phone camera at any item, place or media display assigned with a Sodyo marker and reveal layers of information, content and links.

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
The Sodyo APIs enable developers to create, read, update and delete (CRUD) content and campaigns with Sodyo's Portal. These operations are available via the Portal user interface (UI), however, there are cases that a customer will want to automate the creation of content items and campaigns, via code, to tightly integrate Sodyo with customer internal workflows and systems.

## API Access
The Sodyo administrator must enable API support for a project to allow interacting with the Sodyo system. If the API is not enabled in your project, please feel free to contact us and [request API access](mailto:portalsupport@sodyo.com).

## Supported Functionality
Version 1.0 of the Sodyo API provides the ability to perform full content and campaign management for content of type “immediate action”. <br>

The main functionality provided as part of the Sodyo API v1.0 includes:

* Authentication
	* The project administrator can configure API access keys to be used for integration with the API
	* API authentication & authorization
* Content Operations
	* Manage content of type immediate action
* Campaign Operations
	* Manage campaigns
	* Get Sodyo marker image API
	* Campaign activation / deactivation
* Error Handling

<aside class="notice">All content & campaign operations are available for immediate actions only. Content type of “Sodyo Ad” is not supported in the API at this time. Referencing content / campaigns of type SODYO AD via the API will return an error <code>404-Not Found</code>.</aside>

## Supported Actions
Sodyo content supports enabling various actions when a marker is scanned. The table below lists the supported actions as part of the API.

| Action		| Description				|
|-----------------	|--------------------------------	|
| ADD_TO_CALENDAR	| Add a calendar event			|
| URL             	| Navigate to a URL			|
| PHONE           	| Call a phone number             	|
| NAVIGATE        	| Navigate to an address          	|
| SAVE_CONTACT    	| Add a contact                   	|
| DATA            	| Provide data to the application 	|


## Action Parameters
The table below details the parameters that can be set for each action supported by the API.

| Action | Parameter | Description | Required | Type | Example |
|-----------------|-----------|--------------------|---------------------------|---------------------------|-------------------------------------------|
| ADD_TO_CALENDAR | title | Event Name | Yes | String | Event Title |
|  | address | Event Address | No | String | 3 Main St. |
|  | date | Event Date | Yes | Epoch | 1525384807804 |
|  | eventType | Event Type | Yes | Enum  timeRange or allDay | timeRange |
|  | time | Event Time | Yes if type  is TimeRange | Epoch start & end | start : 1525384807820 end : 1525384807850 |
| URL | url | Target URL | Yes | String | https://www.sodyo.com |
| PHONE | phone | Target Phone # | Yes | String | 123456789 |
| NAVIGATE | address | Target address | Yes | String | 3 Main St. |
| SAVE_CONTACT | firstName | Contact First Name | Yes | String | Ron |
|  | lastName | Contact Last Name | No | String | Yagur |
|  | phone | Contact phone | No | String | 0523334989 |
|  | email | Contact email | Yes | String | ron@sodyo.com |
|  | url | Contact URL | Yes | String | https://www.sodyo.com |
|  | company | Contact Company | No | String | Sodyo |
| DATA | data | data |  | String | {"data": "hello"} |


<aside class="notice">For a DATA action, it may be useful to provide the data as json. In this case, the json may need to be escaped.</aside>

For additional information see [JSON.Org](https://www.json.org).

# Getting Started

## Prerequisites
Before you can start using the Sodyo API, you need to do the following:

* Create an account in the Sodyo Portal
* Create a project in your account
* Have the API enabled in your project by the Sodyo administrator by [contacting us](mailto:portalsupport@sodyo.com).
* Create an API Key

### Creating an Account
Create an account in the Sodyo Portal - for detailed step by step information see the [Sodyo Portal User Guide](https://www.sodyo.com/documentation).

### Creating a Project
Create a project in the Sodyo Portal - for detailed step by step information see the [Sodyo Portal User Guide](https://www.sodyo.com/documentation).

### Generating an API Key
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

<aside class="notice">Note that the Key is not displayed due to security reasons.</aside>

<ol start="4">
<li>Click the + sign to create a new API key. The Add New API Key Screen is displayed.</li>
</ol>
![API New Screen](/images/APINewScr.PNG)

<ol start="5">
<li>Name your key and click "Create and View". The API Key Created Screen is displayed with the generated API key.</li>
</ol>
![API Created Screen](/images/APICrtScr.PNG)

<ol start="6">
<li>Click the copy button to copy the generated key for use in your application. After the key is copied click close to return to the main screen.</li>
</ol>

<aside class="warning">Make sure to copy and save your key in a secure place for use in your code. Once the window is closed, it is not possible to view the key again.
</aside>

![API Main Screen](/images/APIMainScr.PNG)

<aside class="notice">It is possible to delete an API key by clicking the X button at the end of the row for the specific key. The user is prompted to verify the delete operation. Once deleted the key can not be used for API access.</aside>

## API Endpoint Structure
The following endpoint pattern is used for all external integration API endpoints:

/integration/rest/api/​v1/entity​/{entity-id}/action

Where:

* First part​ is static prefix for all API calls (/integration/rest/api)
* Second part​ is the API version number (/v1)
* Third part​ is the entity name (/entity)
* Fourth part​ is entity UUID (/{enitity-id})
* Fifth part​ is an action (/action)

## Requests - Building an API Call

An API call to the Sodyo server must include the following components:

* The Host. The host for all Sodyo API requests is <code>cms.sodyo.com</code>. All API access is via the <code>https</code> protocol.
* An Authorization Header that includes the API Key assigned to the project.
* A Request. When submitting data to a resource via POST or PUT, you must submit your payload in JSON.

<aside class="notice">All requests to the Sodyo API must be made via HTTPS. Make sure to include the Content-Type: application/json header in all requests.</aside>

```shell
  curl -X POST "https://cms.sodyo.com/integration/rest/api/v1/content"
  -H "Content-Type: application/json"
  -H "X-AUTH-TOKEN: DEVLOPER-API-KEY"
  -d "{\"name\": \"Call content 15\",\"description\": \"desc 15\",\"content\": {\"actionType\": \"DATA\",\"params\": {\"data\": \"blah blah blah\"}}}"
```

```http
POST /integration/rest/api/v1/content HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
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

> Make sure to replace `DEVLOPER-API-KEY` with your API key.


## Reponses - API Response Format
All responses delivered via the Sodyo API are returned in JSON format with UTF-8 charset. This is specified by including the content-type header in responses indicating application/json;charset=UTF-8.
The Sodyo API provides standard http response codes that indicate the status of the API request.

## Errors
The Sodyo API provides an indication when an error condition occurs. An API call that generated an error condition will include an error code along with a description where applicable.

See below a list of errors that may be returned by the API:

| Code 	| Reason               	| Description                                                            	|
|------	|----------------------	|------------------------------------------------------------------------	|
| 400  	| Bad Request          	| Validation Error                                                       	|
| 400  	| Bad Request          	| Duplicate Name (Content, Campaign)                                     	|
| 400  	| Bad Request          	| Validation Error, Entity with such ID does not exist in the account  	|
| 401  	| Unauthorized         	| Trying to access the API without a valid authorization header          	|
| 404  	| Not Found            	| Trying to access the non existent entities                             	|
| 404  	| Not Found            	| Trying to access entities which don’t belong to the API key being used 	|
| 507  	| Insufficient Storage 	| Reached account campaign limit                                         	|

## Authentication & Authentication Process
Every request made via the Sodyo API must be authenticated by including an <code>X-AUTH-TOKEN</code> header as part of the request. The <code>X-AUTH-TOKEN</code> is the API Key assigned to the developer project in the Sodyo portal.

1. Every API call to the Sodyo server includes the “​X-AUTH-TOKEN​” header with the API key value.
2. The Sodyo Server validates every API call. If one of the validation checks fail, a <code>401-Unauthorized</code> message is returned.
3. Every request that is authenticated, is authorized with the API role and its associated permission set. The API role is eligible to access external integration API endpoints in the
Sodyo system. Trying to access any other endpoint will return <code>403-Forbidden</code>.

<aside class="notice">If API usage is disabled for the project that the API key belongs to, the server will return <code>403-Forbidden</code>.</aside>

```shell
  curl "https://cms.sodyo.com/integration/rest/api/v1/content"
  -H "Content-Type: application/json"
  -H "X-AUTH-TOKEN: DEVLOPER-API-KEY"
```

```http
  GET /integration/rest/api/v1/content HTTP/1.1
  Host: https://cms.sodyo.com
  Content-Type: application/json
  X-AUTH-TOKEN: DEVLOPER-API-KEY
```
> Make sure to replace `DEVLOPER-API-KEY` with your API key.

# API Reference v1.0
The Sodyo API is a RESTful API that provides a simple interface with a full set of functionality for managing content and campaigns.

## Content API
The Sodyo content API allows performing CRUD operations on content.

### Get All Content [GET]
Returns all content items of type immediate action in the project

> Request

```http
GET https://cms.sodyo.com/integration/rest/api/v1/content HTTP/1.1
```

> Response

```json
HTTP/1.1 200 OK
[
	{
		"uuid": ​ "3d9fa5f7-dcd5-4f03-ab97-2e6e6e656978"​ ,
		"name": ​ "Content1 - Phone"​ ,
		"description": ​ "Description1"​ ,
		"content": {
			"actionType": ​ "PHONE"​ ,
			"params": {
				"phone": ​ "123456789"
			}
		}
	},
	{
		"uuid": ​ "3d7f83f7-dcd5-4f03-ab97-2e1fb9056977"​ ,
		"name": ​ "Content2 - Data"​ ,
		"description": ​ "Description2"​ ,
		"content": {
			"actionType": ​ "DATA"​ ,
			"params": {
				"data": ​ "Hello World"
			}
		}
	}
]
```

### Get Content By Name [GET]
Returns a content item by name

> Request

```http
GET https://cms.sodyo.com/integration/rest/api/v1/content?name={content-name} HTTP/1.1
```

> Response

```json
HTTP/1.1 200 OK
[
	{
		"uuid": ​ "3d9fa5f7-dcd5-4f03-ab97-2e6e6e656978"​ ,
		"name": ​ "Content1 - Phone"​ ,
		"description": ​ "Description1"​ ,
		"content": {
			"actionType": ​ "PHONE"​ ,
			"params": {
				"phone": ​ "123456789"
			}
		}
	}
]
```

### Get Content By UUID [GET]
Returns a content item by UUID

> Request

```http
GET https://cms.sodyo.com/integration/rest/api/v1/content/{content-UUID} HTTP/1.1
```

> Response

```json
HTTP/1.1 200 OK
[
	{
		"uuid": ​ "3d9fa5f7-dcd5-4f03-ab97-2e6e6e656978"​ ,
		"name": ​ "Content1 - Phone"​ ,
		"description": ​ "Description1"​ ,
		"content": {
			"actionType": ​ "PHONE"​ ,
			"params": {
				"phone": ​ "123456789"
			}
		}
	}
]
```

### Create a Content Item [POST]
Creates a new content Item.

> Request

```http
POST https://cms.sodyo.com/integration/rest/api/v1/content HTTP/1.1
```

> Request Body

```json
{
    "name": "test data content",
    "description": "description 1",
    "content": {
      "actionType": "DATA",
      "params": {
        "data": "blah blah blah"
      }
    }
  }
```

> Response

```json
HTTP/1.1 200 OK
[
  {
    "uuid": "3d7f83f7-dcd5-4f03-ab97-2e1fb9056977",
    "name": "Call content updated",
    "description": "desc",
    "content": {
      "actionType": "DATA",
      "params": {
        "data": "blah blah"
      }
    }
  }
]
```


Request Attributes

| Attribute   	| Required 	| Type      	| Description                                                            	|
|-------------	|----------	|-----------	|------------------------------------------------------------------------	|
| Name        	| Yes      	| String    	| Content Name                                                           	|
| Description 	| No       	| String    	| Content Description                                                    	|
| Content 	| Yes      	| Object      	| N/A 	|
| Action Type 	| Yes      	| Enum      	| Action Type: DATA, URL, PHONE, NAVIGATE, ADD_TO_CALENDAR, SAVE_CONTACT 	|
| Params      	| Yes      	| Object 	| Action Parameters                                                      	|

For details about the supported actions see the [Supported Actions section](#supported-actions) and for details regarding the required parameters for each action type see the [Action Parameters section](#action-parameters).

### Update a Content Item [POST]
Updates an existing content Item.

> Request

```http
POST https://cms.sodyo.com/integration/rest/api/v1/content/{content-UUID} HTTP/1.1
```

> Request Body

```json
{
    "name": "test data content updated",
    "description": "description 1 updated",
    "content": {
      "actionType": "DATA",
      "params": {
        "data": "blah blah blah updated"
      }
    }
  }
```

> Response

```json
HTTP/1.1 200 OK
[
  {
    "uuid": "3d7f83f7-dcd5-4f03-ab97-2e1fb9056977",
    "name": "test data content updated",
    "description": "description 1 updated",
    "content": {
      "actionType": "DATA",
      "params": {
        "data": "blah blah blah updated"
      }
    }
  }
]
```

Request Attributes

| Attribute   	| Required 	| Type      	| Description                                                            	|
|-------------	|----------	|-----------	|------------------------------------------------------------------------	|
| Name        	| Yes      	| String    	| Content Name                                                           	|
| Description 	| No       	| String    	| Content Description                                                    	|
| Content 	| Yes      	| Object      	| N/A 	|
| Action Type 	| Yes      	| Enum      	| Action Type: DATA, URL, PHONE, NAVIGATE, ADD_TO_CALENDAR, SAVE_CONTACT 	|
| Params      	| Yes      	| Object 	| Action Parameters                                                      	|

For details about the supported actions see the [Supported Actions section](#supported-actions) and for details regarding the required parameters for each action type see the [Action Parameters section](#action-parameters).


### Delete a Content Item [DELETE]
Deletes an existing content Item.

<aside class="warning">Deleted content cannot be restored. If deleted, a new content item will need to be created.</aside>

> Request

```http
DELETE https://cms.sodyo.com/integration/rest/api/v1/content/{content-UUID} HTTP/1.1
```
> Response

```
HTTP/1.1 200 OK
```

## Campaigns API
The Sodyo campaigns API allows performing CRUD operations on campaigns.

### Get All Campaigns [GET]
Returns all campaigns associated with content of type immediate action in the project

> Request

```http
GET https://cms.sodyo.com/integration/rest/api/v1/campaign HTTP/1.1
```

> Response

```json
HTTP/1.1 200 OK
[
  {
    "uuid": "41c700f3-754c-42fe-a64b-141865829dee",
    "name": "monitoring campaign",
    "mediaType": "TV",
    "location": "Mobile",
    "notes": null,
    "address": null,
    "contentUuid": "85971fe1-4236-4ee0-b292-2f648c72d519",
    "contentName": "monitoring content",
    "createDate": "2017-06-27T12:09:20.523+0000",
    "updateDate": "2018-04-11T13:38:21.666+0000",
    "enabled": true
  },
  {
    "uuid": "bdbfff6a-4eac-4476-9f9c-bb5e9382727d",
    "name": "some campaign",
    "mediaType": "Billboard",
    "location": "Stationary",
    "notes": "some notes",
    "address": "some address",
    "contentUuid": "3d7f83f7-dcd5-4f03-ab97-2e1fb9056977",
    "contentName": "Call content",
    "createDate": "2018-04-16T08:07:47.569+0000",
    "updateDate": "2018-04-16T08:07:47.569+0000",
    "enabled": true
  }
]
```

### Get Campaign by Name [GET]
Returns a campaign by Name

> Request

```http
GET https://cms.sodyo.com/integration/rest/api/v1/campaign?name={campaign-name} HTTP/1.1
```

> Response

```json
HTTP/1.1 200 OK
[
  {
    "uuid": "f1c700f3-754c-42fe-a64b-1418658123ee",
    "name": "Campaign1",
    "mediaType": "TV",
    "location": "Mobile",
    "notes": null,
    "address": null,
    "contentUuid": "85971fe1-4236-4ee0-b292-2f648c72d519",
    "contentName": "monitoring content",
    "createDate": "2017-06-27T12:09:20.523+0000",
    "updateDate": "2018-04-11T13:38:21.666+0000",
    "enabled": true
  }
]
```

### Get Campaign by UUID [GET]
Returns a campaign by UUID

> Request

```http
GET https://cms.sodyo.com/integration/rest/api/v1/campaign/{campaign-UUID} HTTP/1.1
```

> Response

```json
HTTP/1.1 200 OK
[
  {
    "uuid": "f1c700f3-754c-42fe-a64b-1418658123ee",
    "name": "Campaign1",
    "mediaType": "TV",
    "location": "Mobile",
    "notes": null,
    "address": null,
    "contentUuid": "85971fe1-4236-4ee0-b292-2f648c72d519",
    "contentName": "monitoring content",
    "createDate": "2017-06-27T12:09:20.523+0000",
    "updateDate": "2018-04-11T13:38:21.666+0000",
    "enabled": true
  }
]
```

### Create a Campaign [POST]
Creates a new campaign.

> Request

```http
POST https://cms.sodyo.com/integration/rest/api/v1/campaign HTTP/1.1
```

> Request Body

```json
{
    "name": "Campaign1",
    "mediaType": "TV",
    "location": "Mobile",
    "notes": "This is a great campaign",
    "address": "1245 East Broadway St., New York City, NY",
    "contentUuid": "8331fe1-4236-4ee0-b292-2f648e3f4a519",
    "enabled": true
}
```

> Response

```json
HTTP/1.1 200 OK
{
    "uuid": "efc700f3-754c-43fe-a64b-1418656efdee",
    "name": "Campaign1",
    "mediaType": "TV",
    "location": "Mobile",
    "notes": "This is a great campaign",
    "address": "1245 East Broadway St., New York City, NY",
    "contentUuid": "8331fe1-4236-4ee0-b292-2f648e3f4a519",
    "contentName": "monitoring content",
    "createDate": "2017-06-27T12:09:20.523+0000",
    "updateDate": "2018-04-11T13:38:21.666+0000",
    "enabled": true
  }
```

Request Attributes

| Attribute   	| Required 	| Type      	| Description                                                            	|
|-------------	|----------	|-----------	|------------------------------------------------------------------------	|
| name        	| Yes      	| String    	| Campaign Name                                                           	|
| mediaType 	| Yes       	| Enum    	| The type of media for the campaign: TV, Billboard, Digital screen, Other                                                     	|
| location 	| Yes      	| Enum      	| Marker Location: Stationary, Mobile 	|
| notes 	| No      	| String      	| Campaign Notes 	|
| address 	| No      	| String      	| Campaign Address if applicable 	|
| contentUuid      	| Yes      	| string 	| UUID of the content that is associated with the campaign                                                        	|
| enabled      	| Yes      	| string (bool) 	| true / false                                                         	|

### Update a Campaign [POST]
Update an existing campaign.

> Request

```http
POST https://cms.sodyo.com/integration/rest/api/v1/campaign/{campaign-UUID} HTTP/1.1
```

> Request Body

```json
{
    "name": "Campaign 1",
    "mediaType": "Other",
    "location": "Mobile",
    "notes": "This is a great updated campaign",
    "address": "1245 East Broadway St., New York City, NY",
    "contentUuid": "8331fe1-4236-4ee0-b292-2f648e3f4a519",
    "enabled": false
}
```

> Response

```json
HTTP/1.1 200 OK
{
    "uuid": "efc700f3-754c-43fe-a64b-1418656efdee",
    "name": "Campaign 1",
    "mediaType": "Other",
    "location": "Mobile",
    "notes": "This is a great updated campaign",
    "address": "1245 East Broadway St., New York City, NY",
    "contentUuid": "8331fe1-4236-4ee0-b292-2f648e3f4a519",
    "contentName": "monitoring content",
    "createDate": "2017-06-27T12:09:20.523+0000",
    "updateDate": "2018-04-11T13:38:21.666+0000",
    "enabled": false
  }
```

Request Attributes

| Attribute   	| Required 	| Type      	| Description                                                            	|
|-------------	|----------	|-----------	|------------------------------------------------------------------------	|
| name        	| Yes      	| String    	| Campaign Name                                                           	|
| mediaType 	| Yes       	| Enum    	| The type of media for the campaign: TV, Billboard, Digital screen, Other                                                     	|
| location 	| Yes      	| Enum      	| Marker Location: Stationary, Mobile 	|
| notes 	| No      	| String      	| Campaign Notes 	|
| address 	| No      	| String      	| Campaign Address if applicable 	|
| contentUuid      	| Yes      	| string 	| UUID of the content that is associated with the campaign                                                        	|
| enabled      	| Yes      	| string (bool) 	| true / false                                                         	|

### Disable a Campaign [POST]
Disables an existing campaign.

<aside class="notice">Disabling a campaign makes the campaign unavailable to users. Users scanning the associated marker will not receive any content. Note that disabling a campaign makes it unavailable to users but does not delete it. To make the campaign available to users re-enable it.</aside>

> Request

```http
POST https://cms.sodyo.com/integration/rest/api/v1/campaign/{campaign-UUID}/enabled/false HTTP/1.1
```

> Response

```json
HTTP/1.1 200 OK
{
    "uuid": "efc700f3-754c-43fe-a64b-1418656efdee",
    "name": "Campaign 1",
    "mediaType": "Other",
    "location": "Mobile",
    "notes": "This is a great updated campaign",
    "address": "1245 East Broadway St., New York City, NY",
    "contentUuid": "8331fe1-4236-4ee0-b292-2f648e3f4a5199",
    "contentName": "monitoring content",
    "createDate": "2018-05-10T17:45:53.387+0000",
    "updateDate": "2018-05-14T13:44:58.942+0000",
    "enabled": false
}
```


### Enable a Campaign [POST]
Enables an existing campaign.

<aside class="notice">Disabling a campaign makes the campaign unavailable to users. Users scanning the associated marker will not receive any content. Note that disabling a campaign makes it unavailable to users but does not delete it. To make the campaign available to users re-enable it.</aside>

> Request

```http
POST https://cms.sodyo.com/integration/rest/api/v1/campaign/{campaign-UUID}/enabled/true HTTP/1.1
```

> Response

```json
HTTP/1.1 200 OK
{
    "uuid": "efc700f3-754c-43fe-a64b-1418656efdee",
    "name": "Campaign 1",
    "mediaType": "Other",
    "location": "Mobile",
    "notes": "This is a great updated campaign",
    "address": "1245 East Broadway St., New York City, NY",
    "contentUuid": "8331fe1-4236-4ee0-b292-2f648e3f4a5199",
    "contentName": "monitoring content",
    "createDate": "2018-05-10T17:45:53.387+0000",
    "updateDate": "2018-05-14T13:44:58.942+0000",
    "enabled": true
}
```

### Get Campaign Marker [GET]
Provides a graphics file with the campaign marker.

> Request

```http
GET https://cms.sodyo.com/integration/rest/api/v1/campaign/{campaign-UUID}/marker?width=480&height=264 HTTP/1.1
```

> Response

```http
HTTP/1.1 200 OK

Marker Binary Stream
```

<aside class="notice">Get Campaign Marker provides an optimized graphics file for use. The file is provided in Portable Network Graphics (PNG) format, with rounded corners and a transparent background</aside>

<aside class="notice">Supported sizes for both X and Y dimensions are limited between 24 and 2400 pixels. It is recommended that the marker dimensions are relative to the media that the marker is displayed on.  For example, for TV is highly recommended to use the default size 480 x 264 pixels or to keep the ratio to be inline with a standard TV screen.</aside>

![BinaryImage](/images/Marker.png)

Feel free to [contact us](mailto:portalsupport@sodyo.com) to consult on the optimal marker size.

### Delete a Campaign [DELETE]
Deletes an existing Campaign.

<aside class="warning">A deleted campaign cannot be resumed. If deleted, a new campaign will need to be created.</aside>

> Request

```http
DELETE https://cms.sodyo.com/integration/rest/api/v1/content/{campaign-UUID} HTTP/1.1
```

> Response

```http
HTTP/1.1 200 OK
```

# Additional Examples
The following section provides example requests of all types.

Parameters for all examples:

* Host: https://cms.sodyo.com
* Content-Type: application/json
* X-AUTH-TOKEN: DEVLOPER-API-KEY


## Get All Content [GET]
GET https://cms.sodyo.com/integration/rest/api/v1/content

## Get Content By Name [GET]
GET https://cms.sodyo.com/integration/rest/api/v1/content?name=Content1


## Get Content By UUID [GET]
GET https://cms.sodyo.com/integration/rest/api/v1/content/4cf73e6e-b59a-4f9e-883a-49ae63f38895


## Create a Content Item [POST]
POST https://cms.sodyo.com/integration/rest/api/v1/content

> Request Body - Data Action

```json
{
    "name": "test data content",
    "description": "description 1",
    "content": {
      "actionType": "DATA",
      "params": {
        "data": "blah blah blah"
      }
    }
}
```

> Request Body - URL Action

```json
{
    "name": "test url content",
    "description": "description 1",
    "content": {
      "actionType": "URL",
      "params": {
        "url": "https://www.sodyo.com"
      }
    }
}
```

> Request Body - Phone Action

```json
{
    "name": "test phone content",
    "description": "description 1",
    "content": {
      "actionType": "PHONE",
      "params": {
        "phone": "0523334989"
      }
    }
}
```

> Request Body - Add to Calendar Action - All Day

```json
{
    "name": "test add to calendar all day content",
    "description": "description 1",
    "content": {
      "actionType": "ADD_TO_CALENDAR",
      "params": {
        "title": "Event Titel",
        "address": "1234 erewr",
        "date": 1525422727804,
        "eventType": "allDay"
      }
    }
}
```

> Request Body - Add to Calendar Action - With Time Range

```json
{
    "name": "test add to calendar time range content",
    "description": "description 1",
    "content": {
      "actionType": "ADD_TO_CALENDAR",
      "params": {
        "title": "Event Titel",
        "address": "1234 erewr",
        "date": 1525422727804,
        "eventType": "timeRange",
        "time": {
          "start": 1525384807804,
          "end": 1525388407804
        }
      }
    }
}
```

> Request Body - Add Contact Action

```json
{
    "name": "test add contact content",
    "description": "description 1",
    "content": {
      "actionType": "SAVE_CONTACT",
      "params": {
        "firstName": "Ron",
        "lastName": "Yagur",
        "phone": "123456789",
        "email": "ron@sodyo.com",
        "url": "https://www.sodyo.com",
        "company": "Sodyo Ltd."
      }
    }
}
```

## Update a Content Item [POST]
POST https://cms.sodyo.com/integration/rest/api/v1/content/4cf73e6e-b59a-4f9e-883a-49ae63f38895

> Request Body - Data Action

```json
{
    "name": "test data content",
    "description": "description 1",
    "content": {
      "actionType": "DATA",
      "params": {
        "data": "blah blah blah"
      }
    }
}
```

> Request Body - URL Action

```json
{
    "name": "test url content",
    "description": "description 1",
    "content": {
      "actionType": "URL",
      "params": {
        "url": "https://www.sodyo.com"
      }
    }
}
```

> Request Body - Phone Action

```json
{
    "name": "test phone content",
    "description": "description 1",
    "content": {
      "actionType": "PHONE",
      "params": {
        "phone": "0523334989"
      }
    }
}
```

> Request Body - Add to Calendar Action - All Day

```json
{
    "name": "test add to calendar all day content",
    "description": "description 1",
    "content": {
      "actionType": "ADD_TO_CALENDAR",
      "params": {
        "title": "Event Titel",
        "address": "1234 erewr",
        "date": 1525422727804,
        "eventType": "allDay"
      }
    }
}
```

> Request Body - Add to Calendar Action - With Time Range

```json
{
    "name": "test add to calendar time range content",
    "description": "description 1",
    "content": {
      "actionType": "ADD_TO_CALENDAR",
      "params": {
        "title": "Event Titel",
        "address": "1234 erewr",
        "date": 1525422727804,
        "eventType": "timeRange",
        "time": {
          "start": 1525384807804,
          "end": 1525388407804
        }
      }
    }
}
```

> Request Body - Add Contact Action

```json
{
    "name": "test add contact content",
    "description": "description 1",
    "content": {
      "actionType": "SAVE_CONTACT",
      "params": {
        "firstName": "Ron",
        "lastName": "Yagur",
        "phone": "123456789",
        "email": "ron@sodyo.com",
        "url": "https://www.sodyo.com",
        "company": "Sodyo Ltd."
      }
    }
}
```

## Delete a Content Item [DELETE]
DELETE https://cms.sodyo.com/integration/rest/api/v1/content/4cf73e6e-b59a-4f9e-883a-49ae63f38895

## Get All Campaigns [GET]
GET https://cms.sodyo.com/integration/rest/api/v1/campaign

## Get Campaign by Name [GET]
GET https://cms.sodyo.com/integration/rest/api/v1/campaign?name=Campaign1

## Get Campaign by UUID [GET]
GET https://cms.sodyo.com/integration/rest/api/v1/campaign/d35bea1b-6066-4ad1-9cc7-875bbdf0e912

## Create a Campaign [POST]
POST https://cms.sodyo.com/integration/rest/api/v1/campaign

> Request Body

```json
{
    "name": "Campaign1",
    "mediaType": "TV",
    "location": "Mobile",
    "notes": "This is a great campaign",
    "address": "1245 East Broadway St., New York City, NY",
    "contentUuid": "8331fe1-4236-4ee0-b292-2f648e3f4a519",
    "enabled": true
}
```

## Update a Campaign [POST]
POST https://cms.sodyo.com/integration/rest/api/v1/campaign/efc700f3-754c-43fe-a64b-1418656efdee

> Request Body

```json
{
    "name": "Campaign1 updated",
    "mediaType": "TV",
    "location": "Mobile",
    "notes": "This is a great updated campaign",
    "address": "1245 East Broadway St., New York City, NY",
    "contentUuid": "8331fe1-4236-4ee0-b292-2f648e3f4a518",
    "enabled": false
}
```

## Disable a Campaign [POST]
POST https://cms.sodyo.com/integration/rest/api/v1/campaign/d35bea1b-6066-4ad1-9cc7-875bbdf0e912/enabled/false


## Enable a Campaign [POST]
POST https://cms.sodyo.com/integration/rest/api/v1/campaign/d35bea1b-6066-4ad1-9cc7-875bbdf0e912/enabled/true

## Get Campaign Marker [GET]
GET https://cms.sodyo.com/integration/rest/api/v1/campaign/d35bea1b-6066-4ad1-9cc7-875bbdf0e912/marker?width=480&height=264


## Delete a Campaign [GET]
DELETE https://cms.sodyo.com/integration/rest/api/v1/campaign/d35bea1b-6066-4ad1-9cc7-875bbdf0e912

# Sodyo License Agreement
By opening the package, downloading the product or using any of its components, you are consenting to be bound by the [Sodyo license agreement](http://cms.sodyo.com/assets/docs/SDK_License_Agreement.pdf).

If you do not agree to all of the terms of this agreement, do not download and install this package or proceed.

# Support
Should you have any questions or need any help, please do not hesitate to contact us at
[Support](mailto:portalsupport@sodyo.com).
