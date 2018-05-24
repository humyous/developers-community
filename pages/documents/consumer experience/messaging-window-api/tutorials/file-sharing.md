---
title: File Sharing
level1: Documents
level2: Consumer Experience
level3: Messaging Window API
level4: Tutorials
order: 31
permalink: consumer-int-file-sharing.html
indicator: messaging
keywords: file sharing, photo sharing
---

### Introduction

The LiveEngage Messaging Window API includes a file sharing feature. This feature enables consumers to share files with agents, such as images.


A typical flow of setting up the file sharing feature using the Messaging Window API:

1. Enable file sharing capability

2. Create a conversation

3. Request an upload URL

4. Upload the file to storage

5. Publish the file as a message inside the conversation  

6. Request a download URL

7. Download file from storage


Below, we will examine and explain each stage in detail.

**Note:** If you would like to follow the steps using bash, please see the [Prerequisites document](consumer-int-get-msg.html#prerequisites).

### Step 1 - Enable File Sharing

In order to enable file sharing on your account, you should first enable the feature itself. run the following commands, inserting the account admin username and admin password instead of the placeholders below:

```sh
LP_USER= ADMIN_USERNAME
LP_PASS= ADMIN_PASSWORD
LP_BEARER=`curl -c cookies -X POST -H "Content-Type: application/json" -H "Accept: application/json" -H "Cache-Control: no-cache" -d '{"username": "'$LP_USER'","password":"'$LP_PASS'"}' "https://$LP_AGENTVEP/api/account/$LP_ACCOUNT/login?v=1.1" | jq -r .bearer`
./set_site_property.sh $LP_BEARER true messaging.file.sharing.enabled
```

### Step 2 - Create a Conversation

You will need to have an active conversation, along with its converationID, where the files will be shared. Below, creating a conversation is summarized but please follow the [Messaging Window API](https://developers.liveperson.com/consumer-int-overview.html) documentation if you need any more information. Make sure you have an active conversation and a conversationId at hand by the end of this step.

#### Using bash

Open a connection to the messaging service.

```sh
	wscat -k 60 -H "Authorization:jwt $LP_JWT" -c "wss://$LP_ASYNCMESSAGINGENT/ws_api/account/$LP_ACCOUNT/messaging/consumer?v=3"
```

Then, ask to create a new conversation:

```json
	{"kind":"req","id":1,"type":"cm.ConsumerRequestConversation"}
```

In response, you will get a conversation ID that will be used in the next steps.

###  Step 3 - Request Upload URL

Use the following request to retrieve an upload url, specifying the type and size of the file.


**Note**: The supported file types are `JPG`, `JPEG`, `PNG`, `GIF`. Each file can be up to 3MB and the preview must be under 30KB.

See full documentation for generating temporary upload URL [here](consumer-int-msg-generate-temp-upload-url.html).



Request Body Example:

```json
	{
		"kind": "req",
		"id": 1,
		"body": {
		"fileSize": "1000",
		"fileType": "JPG"
		},
		"type": "ms.GenerateURLForUploadFile"
	}
```

Request Body Example:

```json
	{
		"kind": "resp",
		"reqId": "1",
		"code": 200,
		"body": {
			"relativePath": "",
			"queryParams": {
				"temp_url_sig": "12345",
				"temp_url_expires": "1234"
			}
		},
		"type": "ms.GenerateURLResponse"
	}
```

Extract `relativePath`, `temp_url_sig`, `temp_url_expires` from the response. We'll use them in the next request, to upload the file.

#### Using bash

```json
{"kind":"req","id":"3","body":{"fileSize":1000,"fileType":"PNG"},"type":"ms.GenerateURLForUploadFile"    }
```

In response you will get the following message:

```json
{"kind":"resp","reqId":"3","code":200,"body":{"relativePath":"/v1/AUTH_async-images/qa57221676/8a66a22f-81ee-4447-b92f-78e9c3ecc819.PNG","queryParams":{"temp_url_sig":"6f52625b7f148325071c2518c714109134acd7a3","temp_url_expires":"1474973420"}},"type":"ms.BaseGenerateURLResponse"}
```

### Step 4 - Upload File to Storage

Use the following request and the parameters obtained in steps 1 & 2 (`relativePath`, `temp_url_sig`, `temp_url_expires`) to upload the file to storage.

**Note**: file expiration is set to 1 minute by default.

#### Upload request

| Method | URL |
| :--- | :--- |
| PUT | https://{swiftDomain}/{relativePath}?temp_url_sig={temp_url_sig}&temp_url_expires={temp_url_expires} |

Upload the file as binary.

#### Upload response

**Response Codes**

| Code | Description |
| :--- | :--- |
| 201 | CREATED |
| 401 | Temp URL invalid |


**Upload and download file requests are also available as a postman collection**. Use the following [link](assets/content/Swift.postman_collection){:target="_blank"}


Unfamiliar with Postman? [Click here for more information](https://www.getpostman.com/){:target="_blank"}

### Step 5 Publish Message

Once the file is saved in storage, publish the file URL along with an optional caption and thumbnail as a message. See full documentation [here](https://developers.liveperson.com/consumer-int-msg-reqs.html). Event type should be `HostedFile`

Example message:

```json
	{
	  "kind": "req",
	  "id": 1,
	  "body": {
	    "dialogId": "",
	    "event": {
	      "type": "ContentEvent",
	      "contentType": "hosted/file",
	      "message": {
	        "caption": "Hello Kitten",
	        "relativePath": "",
	        "fileType": "JPG",
	        "preview": ""
	      }
	    }
	  },
	  "type": "ms.PublishEvent"
	}
```

#### Using bash

```json
{"kind":"req","id":"22","body":{"dialogId":"__CONVERSATION_ID__","event":{"type":"ContentEvent","message":{"caption":"LivePerson logo","relativePath":"__relative_path__","fileType":"PNG","preview":"data:image/png;base64,<Base64Image>"},"contentType":"hosted/file"}},"type":"ms.PublishEvent"}
```

### Step 6 Request Download URL

**Request a download url**:

Using the messaging API, request a download url, specifying the relative path of the file. See full documentation [here](consumer-int-msg-generate-temp-download-url.html).


Request Body Example:

```json
	{
  	"kind": "req",
  	"id": 1,
  	"body": {
    	"relativePath": "1234.JPG"
  	},
  	"type": "ms.GenerateURLForDownloadFile"
	}
```

#### Using bash

```json
{"kind":"req","id":"33","body":{"relativePath":"__relative_path__"},"type":"ms.GenerateURLForDownloadFile"}
```

In response, you will get the URL details:

```json
{"kind":"resp","reqId":"3","code":200,"body":{"relativePath":"/v1/AUTH_async-images/qa57221676/8a66a22f-81ee-4447-b92f-78e9c3ecc819.PNG","queryParams":{"temp_url_sig":"6f52625b7f148325071c2518c714109134acd7a3","temp_url_expires":"1474973420"}},"type":"ms.BaseGenerateURLResponse"}
```

Extract `relativePath`, `temp_url_sig`, `temp_url_expires` from the response. We'll use it in the next request, to download the file.


## Step 7 - Download File From Storage

Use the following request to download the file, using the parameters from the previous step.

| Method | URL |
| :--- | :--- |
| GET | https://{swiftDomain}/{relativePath}?temp_url_sig={temp_url_sig}&temp_url_expires={temp_url_expires} |

**Note**: file expiration is set to 1 minute by default.