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
The Sodyo APIs enable developers to create, read, update and delete (CRUD) content and campaigns with Sodyo's Portal. Previously, these operations were only available via the Portal user interface (UI), however, there are cases that a customer will want to automate the creation of content items and campaigns, via code, to tightly integrate Sodyo with customer internal workflows and systems.

Version 1.0 of the Sodyo API enables full campaign management and full content management for content of type “immediate action”. <br>

<aside class="notice">Content type of “Sodyo Ad” is not supported in the API at this time.</aside>

## API Access
The Sodyo administrator must enable API support for a project to allow interacting with the Sodyo system. If the API is not enabled in your project, please feel free to contact us and [request API access](mailto:portalsupport@sodyo.com).

## Supported Functionality
The Sodyo API provides the ability to perform content and campaign related operations.

<aside class="notice">All content & campaign operations are available for content / campaigns of type immediate action only. Referencing content / campaigns of type SODYO_AD via the API will return an error <code>404-Not Found</code></aside>

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

## Supported Actions
Sodyo content supports enabling various actions when a marker is scanned. The table below lists the supported actions and related parameters that can be set for each action.

| Action          	| Description                     	| Params                                       	| Description                                                                                  	| Required                               	| Type                                                                                            	| Example                                                                              	|
|-----------------	|---------------------------------	|----------------------------------------------	|----------------------------------------------------------------------------------------------	|----------------------------------------	|-------------------------------------------------------------------------------------------------	|--------------------------------------------------------------------------------------	|
| ADD_TO_CALENDAR 	| Add a calendar event            	| Title Address Date Type Time                 	| Event Name Event Address Event Date Event Type Event Time                                    	| Yes No Yes Yes Yes for timeRange Event 	| String String String (Epoch rep.) Enum: timeRange or allDay String (Epoch rep.) for start / end 	| Meeting with Sodyo 4 Abc St., Tel Aviv, Israel 1526135094765 timeRange 1525384807804 	|
| URL             	| Navigate to a URL               	| url                                          	| Target URL                                                                                   	| Yes                                    	| String                                                                                          	| http://www.sodyo.com                                                                 	|
| PHONE           	| Call a phone number             	| phone                                        	| Target phone number                                                                          	| Yes                                    	| String (numeric)                                                                                	| 123456789                                                                            	|
| NAVIGATE        	| Navigate to an address          	| address                                      	| Target Address                                                                               	| Yes                                    	| String                                                                                          	| 4 Abc St., Tel Aviv, Israel                                                          	|
| SAVE_CONTACT    	| Add a contact                   	| First Name Last Name Phone Email URL Company 	| Contact First Name Contact Last Name Contact Phone Contact Email Contact URL Contact Company 	| Yes No No No No No                     	| String String String String String String                                                       	| Ron Yagur 123456789 ron@sodyo.com http://www.sodyo.com Sodyo                         	|
| DATA            	| Provide data to the application 	| data                                         	| Data to transfer to the application                                                          	| Yes                                    	| String / json                                                                                   	| {     "name":"John",     "age":30,     "food":"salad"  }                             	|

<aside class="notice">For a DATA action, it may be useful to provide the data as json. In this case, the json may need to be escaped - for additional information see [here](https://www.json.org/).</aside>

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

## API Response Format
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
The Sodyo API is a RESTful API that provides a simple interface with a full set of functionality for managing content and campaigns.

## Example
```shell
  curl https://cms.sodyo.com/integration/rest/api/v1/content
  -H "Content-Type: application/json"
  -H "X-AUTH-TOKEN: DEVLOPER-API-KEY"
```

> Make sure to replace `DEVLOPER-API-KEY` with your API key.

```http
  GET /integration/rest/api/v1/content/ HTTP/1.1
  Host: https://cms.sodyo.com
  Content-Type: application/json
  X-AUTH-TOKEN: DEVLOPER-API-KEY
```
> Make sure to replace `DEVLOPER-API-KEY` with your API key.

## Authentication
Every request made via the Sodyo API must be authenticated by including an <code>X-AUTH-TOKEN</code> header as part of the request. The <code>X-AUTH-TOKEN</code> is the API Key assigned to the developer project in the Sodyo portal. When the Sodyo server receives a request from an application, the key hash value is used to authenticate the application with the hash value stored in the server. This provides a secure mechanism that prevents any unauthorized access to the system.

```shell
  curl "https://cms.sodyo.com/integration/rest/api/v1/content"
  -H "Content-Type: application/json"
  -H "X-AUTH-TOKEN: DEVLOPER-API-KEY"
```
> Make sure to replace `DEVLOPER-API-KEY` with your API key.

```http
  GET /integration/rest/api/v1/content/ HTTP/1.1
  Host: https://cms.sodyo.com
  Content-Type: application/json
  X-AUTH-TOKEN: DEVLOPER-API-KEY
```
> Make sure to replace `DEVLOPER-API-KEY` with your API key.


## Requests
All requests to the Sodyo Web API must be made via HTTPS. It is best practice to include the Content-Type: application/json header in all requests.<br>

The Sodyo Web API is completely RESTful and accepts GET, POST, PUT, and DELETE requests, depending on the resource.

```shell
  curl -X POST "https://cms.sodyo.com/integration/rest/api/v1/content/"
  -H "Content-Type: application/json"
  -H "X-AUTH-TOKEN: DEVLOPER-API-KEY"
  -d "{\"name\": \"Call content 15\",\"description\": \"desc 15\",\"content\": {\"actionType\": \"DATA\",\"params\": {\"data\": \"blah blah blah\"}}}"
```
> Make sure to replace `Developer-API-Key` with your API key.

```http
POST /integration/rest/api/v1/content/ HTTP/1.1
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
> Make sure to replace `Developer-API-Key` with your API key.

## Responses
The Sodyo Web API provides response codes that indicate the status of the API request.

API Request:
```shell
  curl "https://cms.sodyo.com/integration/rest/api/v1/content"
```

```http
GET /integration/rest/api/v1/content HTTP/1.1'</code>
````
API Response:
```shell

```

```http
HTTP/1.1 200 OK
Content-Type: application/json
```
```json
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

# Content API
The Sodyo content API allows performing CRUD operations on content.

## Get All Content [GET]
Returns all content items of type immediate action in the project

Request
```http
GET /integration/rest/api/v1/content HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Example Request
```http
https://cms.sodyo.com/integration/rest/api/v1/content
```
Response
```json
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
## Get Content By Name [GET]
Returns a content item by name

Request
```http
GET /integration/rest/api/v1/content?name={content-name} HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Example Request
```http
https://cms.sodyo.com/integration/rest/api/v1/content?name=content-name
```
Response
```json
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

## Get Content By UUID [GET]
Returns a content item by UUID

Request
```http
GET /integration/rest/api/v1/content/{UUID} HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Example Request
```http
https://cms.sodyo.com/integration/rest/api/v1/content/3d9fa5f7-dcd5-4f03-ab97-2e6e6e656978
```
Response
```json
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

## Create a Content Item [POST]
Creates a new content Item.

Request Attributes

| Attribute   	| Required 	| Type      	| Description                                                            	|
|-------------	|----------	|-----------	|------------------------------------------------------------------------	|
| Name        	| Yes      	| String    	| Content Name                                                           	|
| Description 	| No       	| String    	| Content Description                                                    	|
| Content 	| Yes      	| Object      	| N/A 	|
| Action Type 	| Yes      	| Enum      	| Action Type: DATA, URL, PHONE, NAVIGATE, ADD_TO_CALENDAR, SAVE_CONTACT 	|
| Params      	| Yes      	| Object 	| Action Parameters                                                      	|

<aside class="notice">For details about the required parameters for each action type see the supported actions section.</aside>

Request
```http
POST /integration/rest/api/v1/content/ HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Example Request
```http
POST /integration/rest/api/v1/content HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
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
Response
```json
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

## Update a Content Item [POST]
Updates an existing content Item.

Request Attributes

| Attribute   	| Required 	| Type      	| Description                                                            	|
|-------------	|----------	|-----------	|------------------------------------------------------------------------	|
| Name        	| Yes      	| String    	| Content Name                                                           	|
| Description 	| No       	| String    	| Content Description                                                    	|
| Content 	| Yes      	| Object      	| N/A 	|
| Action Type 	| Yes      	| Enum      	| Action Type: DATA, URL, PHONE, NAVIGATE, ADD_TO_CALENDAR, SAVE_CONTACT 	|
| Params      	| Yes      	| Object 	| Action Parameters                                                      	|

<aside class="notice">For details about the required parameters for each action type see the supported actions section.</aside>

Request
```http
POST /integration/rest/api/v1/content/{content-UUID} HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Example Request
```http
POST /integration/rest/api/v1/content/3d7f83f7-dcd5-4f03-ab97-2e1fb9056977 HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
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
Response
```json
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

## Delete a Content Item [DELETE]
Deletes an existing content Item.

<aside class="warning">Deleted content cannot be restored. If deleted, a new content item will need to be created.</aside>

Request
```http
DELETE /integration/rest/api/v1/content/{content-UUID} HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Example Request
```http
DELETE /integration/rest/api/v1/content/3d7f83f7-dcd5-4f03-ab97-2e1fb9056977 HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Response
```
HTTP/1.1 200 OK
```

# Campaigns API
The Sodyo campaigns API allows performing CRUD operations on campaigns.

## Get All Campaigns [GET]
Returns all campaigns associated with content of type immediate action in the project

Request
```http
GET /integration/rest/api/v1/campaign HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Example Request
```http
https://cms.sodyo.com/integration/rest/api/v1/campaign
```
Response
```json
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
## Get Campaign by Name [GET]
Returns a campaign by Name

Request
```http
GET /integration/rest/api/v1/campaign?name={name} HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Example Request
```http
https://cms.sodyo.com/integration/rest/api/v1/campaign?name=Campaign1
```
Response
```json
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

## Get Campaign by UUID [GET]
Returns a campaign by UUID

Request
```http
GET /integration/rest/api/v1/campaign/{UUID} HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Example Request
```http
https://cms.sodyo.com/integration/rest/api/v1/campaign/f1c700f3-754c-42fe-a64b-1418658123ee
```
Response
```json
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

## Create a Campaign [POST]
Creates a new campaign.

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

Request
```http
POST /integration/rest/api/v1/campaign/ HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Example Request
```http
POST /integration/rest/api/v1/campaign HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
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
Response
```json
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
## Update a Campaign [POST]
Update an existing campaign.

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

Request
```http
POST /integration/rest/api/v1/campaign/{uuid} HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Example Request
```http
POST /integration/rest/api/v1/campaign/efc700f3-754c-43fe-a64b-1418656efdee HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
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
Response
```json
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
## Disable a Campaign [POST]
Disables an existing campaign.

<aside class="notice">Disabling a campaign makes the campaign unavailable to users. Users scanning the associated marker will not receive any content. Note that disabling a campaign makes it unavailable to users but does not delete it. To make the campaign available to users re-enable it.</aside>

Request
```http
POST /integration/rest/api/v1/campaign/{campaign-uuid}/enabled/false HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Example Request
```http
POST /integration/rest/api/v1/campaign/efc700f3-754c-43fe-a64b-1418656efdee/enabled/false HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Response
```json
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

## Enable a Campaign [POST]
Enables an existing campaign.

<aside class="notice">Disabling a campaign makes the campaign unavailable to users. Users scanning the associated marker will not receive any content. Note that disabling a campaign makes it unavailable to users but does not delete it. To make the campaign available to users re-enable it.</aside>

Request
```http
POST /integration/rest/api/v1/campaign/{campaign-uuid}/enabled/true HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Example Request
```http
POST /integration/rest/api/v1/campaign/efc700f3-754c-43fe-a64b-1418656efdee/enabled/true HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Response
```json
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

## Get Campaign Marker [GET]
Provides a graphics file with the campaign marker.

<aside class="notice">Get Campaign Marker provides an optimized graphics file for use. The file is provided in Portable Network Graphics (PNG) format, with rounded corners and a transparent background</aside>

<aside class="warning">Supported sizes for both X and Y dimensions are limited between 24 and 2400 pixels. It is highly recommended to use the default size 480 x 264 pixels</aside>

Request
```http
GET /integration/rest/api/v1/campaign/{campaign-uuid}/marker?width=480&height=264 HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Example Request
```http
GET /integration/rest/api/v1/campaign/efc700f3-754c-43fe-a64b-1418656efdee/marker?width=480&height=264 HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Response

Marker Binary Stream

![Binary Image](/images/Marker.PNG)


## Delete a Campaign [DELETE]
Deletes an existing Campaign.

<aside class="warning">A deleted campaign cannot be resumed. If deleted, a new campaign will need to be created.</aside>

Request
```http
DELETE /integration/rest/api/v1/content/{campaign-UUID} HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Example Request
```http
DELETE /integration/rest/api/v1/campaign/efc700f3-754c-43fe-a64b-1418656efdee HTTP/1.1
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY
```
Response
```
HTTP/1.1 200 OK
```

# Examples
The following section provides example requests of all types.
Parameters for all examples:
Host: https://cms.sodyo.com
Content-Type: application/json
X-AUTH-TOKEN: DEVLOPER-API-KEY


## Get All Content [GET]
GET https://cms.sodyo.com/integration/rest/api/v1/content

## Get Content By Name [GET]
GET https://cms.sodyo.com/integration/rest/api/v1/content?name=Content1


## Get Content By UUID [GET]
GET https://cms.sodyo.com/integration/rest/api/v1/content/4cf73e6e-b59a-4f9e-883a-49ae63f38895


## Create a Content Item [POST]


## Update a Content Item [POST]


## Delete a Content Item [DELETE]
DELETE https://cms.sodyo.com/integration/rest/api/v1/content/4cf73e6e-b59a-4f9e-883a-49ae63f38895


## Get All Campaigns [GET]
GET https://cms.sodyo.com/integration/rest/api/v1/campaign


## Get Campaign by Name [GET]
GET https://cms.sodyo.com/integration/rest/api/v1/campaign?name=Campaign1

## Get Campaign by UUID [GET]
GET https://cms.sodyo.com/integration/rest/api/v1/campaign/d35bea1b-6066-4ad1-9cc7-875bbdf0e912


## Create a Campaign [POST]


## Update a Campaign [POST]


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
