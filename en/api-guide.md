## Notification > Email > API v2.1 Guide

### v2.1 API Overview

1. Changed query API response
	* Added statId to the response of the email query API.
2. Added API to view the list of completed email delivery updates
3. Added the field for requesting to send authentication and advertising email
    * Added senderGroupingKey field.

[API Domain]

|Environment| Domain |
|---|---|
|Real|	https://email.api.nhncloudservice.com |

[Header]
```
Content-Type: application/json;charset=UTF-8
```

<p id="secret-key"></p>

### Secret Key
- You can check in the console.
- When calling an API that requires Secret Key, you must call after configuring the header as follows.
```
Header
X-Secret-Key: [a-zA-Z0-9]{8}
```
**You can check and create in CONSOLE > Notification > Email > URL & AppKey**.


[Caution for Curl Example]

- Curl Example may not be properly requested in Windows cmd.

## Mail Delivery

### Send General Mails

#### Request

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.1/appKeys/{appKey}/sender/mail|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |


[Request body]

| Value             | Type        | Required | Description                                                  |
| ----------------- | ----------- | -------- | ------------------------------------------------------------ |
| senderAddress     | String      | O        | Sender's mail address                                        |
| senderName        | String      | X        | Sender's name                                                |
| requestDate       | String      | X        | Enter current time when delivery time is missing (yyyy-MM-dd HH:mm:ss) |
| title             | String      | O        | Title                                                        |
| body              | String      | O        | Body                                                         |
| attachFileIdList  | List:String | X        | ID of uploaded attachment                                    |
| templateId        | String      | X        | ID of delivery template                                      |
| templateParameter | Object      | X        | Replacement parameter (to enter mail title/body for replacement) |
| - #key#           | String      | X        | Replacement key (##key##)                                    |
| - #value#         | Object      | X        | Mapped value for replacement key                             |
| receiverList      | List        | O        | List of recipients <br/>Send up to 1,000 persons (including recipients and cc recipients) |
| - receiveMailAddr | String      | O        | Recipient's email address                                    |
| - receiveName     | String      | X        | Recipient's name                                             |
| - receiveType     | String      | O        | Recipient type (MRT0: recipient , MRT1: Cc, MRT2: Bcc)       |
| customHeaders     | Map         | X        | [Custom Header](./console-guide/#custom-header)                   |
| senderGroupingKey | String      | X        | Sender's group key                                           |
| userId            | String      | X        | Delimiter of delivery e.g.) admin, system                    |
| statsId           | String      | X        | Statistics ID (not included in the delivery search conditions)|


[Caution]

* On a template, **senderAddress, title, and body** are not required. These values, if left empty, can be replaced by registered values on template. 
* On a template, **senderAddress, senderName, title, body, and templateType**, if available, are applied before template-registered values.
* When using a template of the Freemarker type, all template parameters must be replaced for the email to be sent.
    * Sending the email will fail if there is parameters that are not replaced.

[Example 1]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/mail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "senderAddress": "support@example.com",
    "senderName": "Sender name",
    "title": "Sample title",
    "body": "Sample body",
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "Customer 1",
            "receiveType": "MRT0"
        }, {
            "receiveMailAddr": "customer2@example.com",
            "receiveName": "Customer 2",
            "receiveType": "MRT1"
        }
    ],
    "userId": "USER",
    "statsId": "statsId"
}'
```

[Example 2 - Using Templates]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/mail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "templateId": "TEMPLATE1",
    "templateParameter": {
        "key": "value"
    },
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "customer1",
            "receiveType": "MRT0"
        }, {
            "receiveMailAddr": "customer2@example.com",
            "receiveName": "customer2",
            "receiveType": "MRT1"
        }
    ],
    "userId": "USER",
    "statsId": "statsId"
}'
```

#### Response

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "success",
        "isSuccessful": true
    },
    "body": {
        "data": {
            "requestId": "20190101000000ABCDEFG0",
            "results": [
                {
                    "receiveMailAddr": "receiver@nhn.com",
                    "receiveName": "receiver",
                    "receiveType": "MRT0",
                    "resultCode": 0,
                    "resultMessage": "success"
                }
            ]
        }
    }
}
```

| Value               | Type    | Not Null|	Description                                             |
|---|---|---|---|
| header              | Object  | O|	Header area                                             |
| - isSuccessful      | Boolean | O|	Successful or not                                       |
| - resultCode        | Integer | O|	Failure code                                            |
| - resultMessage     | String  | O|	Failure message                                         |
| body                | Object  | X|	Body area                                               |
| - data              | Object  | X|	Data area                                               |
| -- requestId        | String  | X|	Request ID                                              |
| -- results          | List    | X|	Delivery result                                         |
| --- receiveMailAddr | String  | X|	Recipient's mail address                                |
| --- receiveName     | String  | X|	Recipient's name                                        |
| --- receiveType     | String  | O|	Recipient type (MRT0: recipients , MRT1: Cc, MRT2: Bcc) |
| --- resultCode      | Integer | O|	Result code of recipient delivery request               |
| --- resultMessage   | String  | O|	Result message of recipient delivery request            |


### Send Individual Mails

* Mails can be sent to each of many recipients. Even if a same mail is sent to many recipients, each recipient can find his or her name only.  

#### Request

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.1/appKeys/{appKey}/sender/eachMail|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Request Body]

|Value| Type | Required | Description |
|---|---|---|---|
|senderAddress|	String|	O| Sender's mail |
|senderName|	String|	X| Sender's name |
|requestDate|	String|	X| Enter current time when delivery time is missing (yyyy-MM-dd HH:mm:ss) |
|title|	String|	O| Title |
|body|	String|	O| Body |
|attachFileIdList|	List:Integer|	X| ID for uploaded attached file |
|templateType| String| X| Template type <br/>DEFAULT(default), FREEMARKER) |
|templateId|	String|	X| ID for delivery template |
|receiverList|	List|	O| List of recipients<br/>Send up to 1,000 persons |
|- receiveMailAddr|	String|	O| Recipient's mail address |
|- receiveName|	String|	X| Recipient's name |
|- templateParameter|	Object|	X| Replacement parameter (to enter mail title/body for replacement) |
|-- #key#|	String|	X| Replacement key (##key##) |
|-- #value#|	Object|	X| Mapped value for replacement key |
|customHeaders| Map| X| [Custom Header](./console-guide/#custom-header) |
|senderGroupingKey| String| X| Sender's group key |
|userId|	String|	X| Delimiter for delivery e.g.) admin,system |
|statsId| String      | X        | Statistics ID (not included in the delivery search conditions)|

[Caution]

* On a template, **senderAddress, title, and body** are not required. These values, if left empty, can be replaced by registered values on template. 
* On a template, **senderAddress, senderName, title, body, and templateType**, if available, are applied before template-registered values.

[Example 1]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/eachMail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "senderAddress": "support@example.com",
    "senderName": "sendername",
    "title": "sample title",
    "body": "sample content",
    "attachFileIdList": [1, 2],
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "customer1"
        }
    ],
    "userId": "USER",
    "statsId": "statsId"
}'
```

[Example 2 - Using Templates]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/eachMail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "templateId": "TEMPLATE1",
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "고객1",
            "templateParameter": {
                "key": "value"
            }
        }
    ],
    "userId": "USER",
    "statsId": "statsId"
}'
```


#### Response

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "success",
        "isSuccessful": true
    },
    "body": {
        "data": {
            "requestId": "20190101000000ABCDEFG0",
            "results": [
                {
                    "receiveMailAddr": "receiver@nhn.com",
                    "receiveName": "receiver",
                    "receiveType": "MRT0",
                    "resultCode": 0,
                    "resultMessage": "success"
                }
            ]
        }
    }
}
```

| Value               | Type    | Not Null|	Description                                                  |
|---|---|---|---|
| header              | Object  | O|	Header area                                                  |
| - isSuccessful      | Boolean | O|	Successful or not                                            |
| - resultCode        | Integer | O|	Failure code                                                 |
| - resultMessage     | String  | O|	Failure message                                              |
| body                | Object  | X|	Body area                                                    |
| - data              | Object  | X|	Data area                                                    |
| -- requestId        | String  | X|	Request ID                                                   |
| -- results          | List    | X|	Delivery result                                              |
| --- receiveMailAddr | String  | O|	Recipient's mail address                                     |
| --- receiveName     | String  | X|	Recipient's name                                             |
| --- receiveType     | String  | O|	Recipient type (MRT0: recipients , MRT1: Cc, MRT2: Bcc)|
| --- resultCode      | Integer | O|	Result code of recipient delivery request                    |
| --- resultMessage   | String  | Result message of recipient delivery request                 |


### Sending General Ad Mails 

* Request and response information is same as in sending general mails. 

#### Caution for Sending Ad Mails

- The title must include the (AD) phrase. 
- For more details, see [Send Advertising Mails](./console-guide/#_3).
* In response to [[Enhanced Gmail sender Guideline](https://support.google.com/a/answer/81126?hl=en)], which will be implemented starting in February 2024, a one-click unsubscribe link will be required when sending marketing and opt-in emails to gmail.com.
    * This link will automatically be replaced with an unsubscribe link.
    * If you wish to use a separate URL, you can do so by adding the "List-Unsubscribe" header to customHeaders.
    * For more information, refer to the [Troubleshooting Guide](./troubleshooting-guide/).
  
[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.1/appKeys/{appKey}/sender/ad-mail|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Example 1]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/ad-mail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "senderAddress": "support@example.com",
    "senderName": "sendername",
    "title": "(ads) sample title",
    "body": "sample content <br /> ##BLOCK_RECEIVER_LINK## <br />##EN_BLOCK_RECEIVER_LINK##",
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "customer1",
            "receiveType": "MRT0"
        }, {
            "receiveMailAddr": "customer2@example.com",
            "receiveName": "customer2",
            "receiveType": "MRT1"
        }
    ],
    "userId": "USER",
    "statsId": "statsId"
}'
```

[Example 2 - Using Templates]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/ad-mail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "templateId": "TEMPLATE1",
    "templateParameter": {
        "key": "value"
    },
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "customer1",
            "receiveType": "MRT0"
        }, {
            "receiveMailAddr": "customer2@example.com",
            "receiveName": "customer2",
            "receiveType": "MRT1"
        }
    ],
    "userId": "USER",
    "statsId": "statsId"
}'
```

### Sending Individual Ad Mails

* Request and response information is same as in sending individual mails.  

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.1/appKeys/{appKey}/sender/ad-eachMail |

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Example 1]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/ad-eachMail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "senderAddress": "support@example.com",
    "senderName": "sendername",
    "title": "(ads) sample title",
    "body": "sample content <br /> ##BLOCK_RECEIVER_LINK## <br />##EN_BLOCK_RECEIVER_LINK##",
    "attachFileIdList": [1, 2],
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "customer1"
        }
    ],
    "userId": "USER",
    "statsId": "statsId"
}'
```

[Example 2 - Using Templates]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/ad-eachMail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "templateId": "TEMPLATE1",
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "고객1",
            "templateParameter": {
                "key": "value"
            }
        }
    ],
    "userId": "USER",
    "statsId": "statsId"
}'
```

### Send Authenticated Mails 

#### Request

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.1/appKeys/{appKey}/sender/auth-mail|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |


[Request Body]

| Value               | Type   | Required | Description                                                  |
| ------------------- | ------ | -------- | ------------------------------------------------------------ |
| senderAddress       | String | O        | Sender's address                                             |
| senderName          | String | X        | Sender's name                                                |
| requestDate         | String | X        | Enter current time when delivery time is missing (yyyy-MM-dd HH:mm:ss) |
| title               | String | O        | Title                                                        |
| body                | String | O        | Body                                                         |
| templateId          | String | X        | ID for delivery template                                     |
| receiver            | Object | O        | Recipient                                                    |
| - receiveMailAddr   | String | O        | Recipient's mail address                                     |
| - receiveName       | String | X        | Recipient's name                                             |
| - templateParameter | Object | X        | Replacement parameter (to enter mail title/body for replacement) |
| -- #key#            | String | X        | Replacement key (##key##)                                    |
| -- #value#          | Object | X        | Mapped value for replacement key                             |
| customHeaders       | Map    | X        | [Custom Header](./console-guide/#custom-header)                   |
|senderGroupingKey| String| X| Sender group key (up to 100 characters )|
| userId              | String | X        | Delimiter for delivery e.g.) admin,system                    |
| statsId             | String | X        | Statistics ID (not included in the delivery search conditions)|

[Caution]

* On a template, **senderAddress, title, and body** are not required. These values, if left empty, can be replaced by registered values on template. 
* On a template, **senderAddress, senderName, title, body, and templateType**, if available, are applied before template-registered values.
* When using a template of the Freemarker type, all template parameters must be replaced for the email to be sent.
    * Sending the email will fail if there is parameters that are not replaced.

#### Differences from General Mails 

Features of authenticated mails are as follows: 

- Allows only single-case deliveries (one recipient).
- File attachment is not supported. Templates including attachment are not supported. 

[Example 1]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/auth-mail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "senderAddress": "support@example.com",
    "senderName": "sendername",
    "title": "sample title",
    "body": "sample content",
    "receiver": {
        "receiveMailAddr": "customer1@example.com",
        "receiveName": "customer1"
    },
    "userId": "USER",
    "statsId": "statsId"
}'
```

[Example 2 - Using Templates]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/auth-mail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "templateId": "TEMPLATE1",
    "receiver": {
        "receiveMailAddr": "customer1@example.com",
        "receiveName": "고객1",
        "templateParameter": {
            "key": "value"
        }
    },
    "userId": "USER",
    "statsId": "statsId"
}'
```

#### Response

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "success",
        "isSuccessful": true
    },
    "body": {
        "data": {
            "requestId": "20190101000000ABCDEFG0",
            "results": [
                {
                    "receiveMailAddr": "receiver@nhn.com",
                    "receiveName": "receiver",
                    "receiveType": "MRT0",
                    "resultCode": 0,
                    "resultMessage": "success"
                }
            ]
        }
    }
}
```

| Value               | Type    | Not Null|	Description                                                  |
|---|---|---|---|
| header              | Object  | O|	Header area                                                  |
| - isSuccessful      | Boolean | O|	Successful or not                                            |
| - resultCode        | Integer | O|	Failure code                                                 |
| - resultMessage     | String  | O|	Failure message                                              |
| body                | Object  | X|	Body area                                                    |
| - data              | Object  | X|	Data area                                                    |
| -- requestId        | String  | X|	Request ID                                                   |
| -- results          | List    | X|	Delivery result                                              |
| --- receiveMailAddr | String  | O|	Recipient's mail address                                     |
| --- receiveName     | String  | X|	Recipient's name                                             |
| --- receiveType     | String  | O|	Recipient type (MRT0: recipients , MRT1: Cc, MRT2: Bcc) |
| --- resultCode      | Integer | O|	Result code of recipient delivery request                    |
| --- resultMessage   | String  | O|	Result message of recipient delivery request                 |

### Send Tagged Mails

#### Request

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.1/appKeys/{appKey}/sender/tagMail|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Request body]

| Value            | Type        | Required | Description                                                  |
| ---------------- | ----------- | -------- | ------------------------------------------------------------ |
| senderAddress    | String      | O        | Sender's address                                             |
| senderName       | String      | X        | Sender's name                                                |
| requestDate      | String      | X        | Enter current time when delivery time is missing (yyyy-MM-dd HH:mm:ss) |
| title            | String      | O        | Title                                                        |
| body             | String      | O        | Body                                                         |
| templateId       | String      | X        | Template ID                                                  |
| adYn             | String      | X        | Advertisement or not (default is 'N')                        |
| autoSendYn       | String      | X        | Automatic delivery or not (default is 'Y')                   |
| attachFileIdList | List:String | X        | List of attached files                                       |
| tagExpression    | List:String | O        | Tag expression (including Tag ID)                                               |
| customHeaders    | Map         | X        | [Custom Header](./console-guide/#custom-header)                   |
| userId           | String      | X        | Delimiter of delivery e.g.) admin, system                    |
| statsId          | String      | X        | Statistics ID (not included in the delivery search conditions)|

[Example 1]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/tagMail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "senderAddress": "support@example.com",
    "senderName": "sendername",
    "title": "sample title",
    "body": "sample content",
    "attachFileIdList": [1, 2],
    "tagExpression": ["tag1", "AND", "tag2"],
    "userId": "USER",
    "statsId": "statsId"
}'
```

[Example 2 - Using Templates]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/tagMail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "templateId": "TEMPLATE1",
    "tagExpression": ["tag1", "AND", "tag2"],
    "userId": "USER",
    "statsId": "statsId"
}'
```

#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "data": {
            "requestId": "20190101000000ABCDEFG0"
        }
    }
}
```

| Value           | Type    | Not Null|	Description       |
|---|---|---|---|
| header          | Object  | O|	Header area       |
| - isSuccessful  | Boolean | O|	Successful or not |
| - resultCode    | Integer | O|	Failure code      |
| - resultMessage | String  | X|	Failure message   |
| body            | Object  | X|	Body area         |
| - data          | Object  | X|	Data area         |
| -- requestId    | String  | X|	Request ID        |

### Upload Attached Files 

#### Request 

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.1/appKeys/{appKey}/attachfile/binaryUpload|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Request Body]

| Value      | Type   | Required | Description               |
| ---------- | ------ | -------- | ------------------------- |
| fileName   | String | O        | File name                 |
| fileBody   | Byte[] | O        | Byte[]  of a file         |
| createUser | String | X        | File uploader information |

#### cURL
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/attachfile/binaryUpload' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "fileName": "file.csv",
    "createUser": "USER",
    "fileBody": []
}'
```

#### Response

```
{
  "header": {
    "isSuccessful":  Boolean,
    "resultCode": Integer,
    "resultMessage": String
  },
  "body": {
    "data": {
      "fileId": Integer,
      "fileName": String
    }
  }
}
```

| Value           | Type    | Not Null|	Description       |
|---|---|---|---|
| header          | Object  | O|	Header area       |
| - isSuccessful  | Boolean | O|	Successful or not |
| - resultCode    | Integer | O|	Failure code      |
| - resultMessage | String  | O|	Failure message   |
| body            | Object  | X|	Body area         |
| - data          | Object  | X|	Data area         |
| -- fileId       | Integer  | O|	File ID           |
| -- fileName     | String  | O|	File name         |

### Title/Body Replacement

#### Default Type
* Enter in the (##Replacement Key##) format to replace with user-defined **templateParameter**.
```
* title: Hello, ##title_name## !!
* body: We send ##body_content##.
->
* title: Hello, clouc customer1 !!
* body: We send test2.
```

#### FreeMarker Type
* [FreeMarker Template Engine](https://freemarker.apache.org/) is supported.
* You may use a template language to replace with user-defined **templateParameter**. 
* When using a template of the Freemarker type, all template parameters must be replaced for the email to be sent.
    * Sending the email will fail if there is parameters that are not replaced.

```
* title: Hello, ${title_name}!!
* body: We send ${body_content}.
->
* title: Hello, cloud customer1!!
* body: We send test2.
```

#### Example of General Mail Request 
```json
{
    "senderAddress" : "support@nhnent.com",
    "templateId": "template1",
    "templateParameter" : {"title_name": "cloud customer1", "body_content": "test1"},
    "receiverList" : [
        {
            "receiveMailAddr" : "customer1@nhnent.com",
            "receiveType" : "MRT0"
        }

    ],
    "userId" : "tester"
}
```

#### Example of Individual Mail Request
```json
{
    "senderAddress" : "support@nhnent.com",
    "templateId": "template1",
    "receiverList" : [
        {
            "receiveMailAddr" : "customer1@nhnent.com",
            "templateParameter" : {"title_name": "cloud customer1", "body_content": "test1"}
        },
        {
            "receiveMailAddr" : "customer2@nhnent.com",
            "templateParameter" : {"title_name": "cloud customer2", "body_content": "test2"}
        }

    ],
    "userId" : "tester"
}
```

## Query of Mails

### Query List of Mail Deliveries

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.1/appKeys/{appKey}/sender/mails|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Query Parameter]

| Value            | Type    | Required | Description                                                                                                                                  |
| ---------------- | ------- | -------- |----------------------------------------------------------------------------------------------------------------------------------------------|
| requestId        | String  | O        | Request ID                                                                                                                                   |
| startSendDate    | String  | O        | Delivery start date (yyyy-MM-dd HH:mm:ss)                                                                                                    |
| endSendDate      | String  | X        | Delivery end date (yyyy-MM-dd HH:mm:ss)                                                                                                      |
| startReceiveDate | String  | X        | Receiving start date (yyyy-MM-dd HH:mm:ss)                                                                                                   |
| endReceiveDate   | String  | X        | Receiving end date (yyyy-MM-dd HH:mm:ss)                                                                                                     |
| senderAddress    | String  | X        | Sender's mail address                                                                                                                        |
| senderName       | String  | X        | Sender's name                                                                                                                                |
| receiveMailAddr  | String  | X        | Recipient's mail address                                                                                                                     |
| templateId       | String  | X        | Template number                                                                                                                              |
| mailStatusCode   | String  | X        | Delivery status code   SST0: Preparing for sending, SST1: Sending,    SST2: Sending completed, SST3: Sending failed, SST7: Not Authenticated |
| isReceived       | Boolean | X        | Received or Not                                                                                                                              |
| isOpened         | Boolean | X        | Opened or Not                                                                                                                                |
| senderGroupingKey| String  | X        | Sender's group key                                                                                                                           |
| pageNum          | Integer | X        | Page number (default: 1)                                                                                                                     |
| pageSize         | Integer | X        | Number of queries (default: 15)                                                                                                              |

* **requestId**, or **startSendDate** and **endSendDate** are required fields. 

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/mails?startSendDate='"${START_DATE}"'&endSendDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "pageNum": 1,
        "pageSize": 10,
        "totalCount": 1,
        "data": [
            {
                "requestId": "20190101000000ABCDEFG0",
                "mailSeq": 0,
                "requestDate": "2019-01-01 00:00:00",
                "templateId": "Template",
                "templateName": "Template Name",
                "senderName": "Sender",
                "senderAddress": "sender@nhn.com",
                "title": "Title",
                "mailStatusCode": "SST2",
                "mailStatusName": "발송완료",
                "isReceived": true,
                "resultDate": "2019-01-01 00:00:00",
                "isOpened": true,
                "openedDate": "2019-01-01 00:00:00",
                "receiveMailAddr": "receiver@nhn.com",
                "receiveType": "MRT0",
                "receiveTypeName": "받는사람",
                "receiveName": "Receiver",
                "senderGroupingKey": "A_GROUP",
                "dsnCode": "2.5.0",
                "dsnMessage": "SUCCESS",
                "statsId": "statsId"
            }
        ]
    }
}
```

| Value               | Type    | Not Null|	Description                                                  |
|---|---|---|---|
| header              | Object  | O|	Header area                                                  |
| - isSuccessful      | Boolean | O|	Successful or not                                            |
| - resultCode        | Integer | O|	Failure code                                                 |
| - resultMessage     | String  | O|	Failure message                                              |
| body                | Object  | X|	Body area                                                    |
| - pageNum           | Integer | O|	Current page number                                          |
| - pageSize          | Integer | O|	Number of queried data                                       |
| - totalCount        | Integer | O|	Number of total data                                         |
| - data              | List    | O|	Data area                                                    |
| -- requestId        | String  | O|	Request ID                                                   |
| -- mailSeq          | String  | O|	Mail sequence                                                |
| -- requestDate      | String  | O|	Date and time of sending                                     |
| -- templateId       | String  | X|	Template ID                                                  |
| -- templateName     | String  | X|	Template name                                                |
| -- senderName       | String  | X|	Sender's name                                                |
| -- senderAddress    | String  | O|	Sender's mail address                                        |
| -- title            | String  | X|	Title                                                        |
| -- mailStatusCode   | String  | O|	Delivery status code   SST0: Preparing Delivery, SST1: Delivering,    SST2: Delivery Completed, SST3: Delivery Failed, SST7: Not Authenticated |
| -- mailStatusName   | String  | O|	Name of delivery status                                      |
| -- isReceived       | Boolean | X|	Received or Not                                              |
| -- resultDate       | String  | X|	Date and Time of Receiving                                   |
| -- isOpened         | Boolean | X|	Opened or Not                                                |
| -- openedDate       | String  | X|	Date and Time of Opening                                     |
| -- receiveMailAddr  | String  | O|	Recipient's mail address                                     |
| -- receiveType      | String  | O|	Recipient type  (MRT0: recipient, MRT1: cc, MRT2: bcc)       |
| -- receiveTypeName  | String  | O|	Name of recipient type                                       |
| -- receiveName      | String  | X|	Name of recipient                                            |
| -- senderGroupingKey| String  | X|	Sender's group key                                           |
| -- dsnCode| String|	X| DSN(Delivery Status Notification) Status Code |
| -- dsnMessage| String|	X| DSN(Delivery Status Notification) Status Message |
|-- statsId| String|	X| Key for statistics data grouping |


### Query Mail Delivery Details

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.1/appKeys/{appKey}/sender/mail/{requestId}/{mailSeq}|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|requestId|	String| Request ID |
|mailSeq|	Integer| Mail sequence |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/mail/'"${REQUEST_ID}"'/'"${MAIL_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "data": {
            "requestId": "20190101000000ABCDEFG0",
            "mailSeq": 0,
            "requestIp": "127.0.0.1",
            "requestDate": "2019-01-01 00:00:00",
            "mailStatusCode": "SST2",
            "mailStatusName": "발송완료",
            "templateId": "Template",
            "templateName": "Template Name",
            "senderName": "Sender",
            "senderAddress": "sender@nhn.com",
            "resultId": "<20190101000000ABCDEFG0@test>",
            "title": "Title",
            "body": "Body",
            "receiverList": [
                {
                    "requestId": "20190101000000ABCDEFG0",
                    "mailSeq": 0,
                    "receiveType": "MRT0",
                    "receiveTypeName": "받는사람",
                    "receiveName": "Receiver",
                    "receiveMailAddr": "receiver@nhn.com",
                    "isReceived": true,
                    "resultDate": "2019-01-01 00:00:00",
                    "isOpened": true,
                    "openedDate": "2019-01-01 00:00:00",
                    "dsnCode": "2.5.0",
                    "dsnMessage": "SUCCESS"
                }
            ],
            "attachFileList": [
                {
                    "fileType": "MAIL",
                    "fileId": 1,
                    "fileName": "test.jpg",
                    "filePath": "file/path",
                    "fileSize": 1,
                    "createDate": "2019-01-01 00:00:00"
                }
            ],
            "customHeaders": {
                "Content-Type": "text/html"
            },
            "senderGroupingKey": "A_GROUP",
            "statsId": "statsId"
        }
    }
}
```

| Value               | Type    | Not Null|	Description                                                  |
|---|---|---|---|
| header              | Object  | O|	Header area                                                  |
| - isSuccessful      | Boolean | O|	Successful or not                                            |
| - resultCode        | Integer | O|	Failure code                                                 |
| - resultMessage     | String  | O|	Failure message                                              |
| body                | Object  | X|	Body area                                                    |
| - data              | Object  | O|	Data area                                                    |
| -- requestId        | String  | O|	Request ID                                                   |
| -- mailSeq          | Integer | O|	Mail sequence                                                |
| -- requestIp        | String  | O|	Request IP                                                   |
| -- requestDate      | String  | O|	Date and time of requesting                                  |
| -- mailStatusCode   | String  | O|	Delivery status                                              |
| -- mailStatusName   | String  | O|	Name of delivery status                                      |
| -- templateId       | String  | X|	Template ID                                                  |
| -- templateName     | String  | X|	Template name                                                |
| -- senderName       | String  | X|	Sender's name                                                |
| -- senderAddress    | String  | O|	Sender's mail address                                        |
| -- resultId         | String  | X|	Mail delivery ID                                             |
| -- title            | String  | X|	Mail title                                                   |
| -- body             | String  | X|	Mail body                                                    |
| -- receiverList     | List    | O|	List of recipients                                           |
| --- requestId       | String  | O|	Request ID                                                   |
| --- mailSeq         | Integer | O|	Mail sequence                                                |
| --- receiveType     | String  | O|	Recipient type <br/>MRT0: to , MRT1: cc, MRT2: bcc            |
| --- receiveTypeName | String  | O|	Name of recipient type                                       |
| --- receiveName     | String  | X|	Recipient's name                                             |
| --- receiveMailAddr | String  | O|	Recipient's mail address                                     |
| --- isReceived      | Boolean | O|	Received or Not                                              |
| --- resultDate      | String  | X|	Date and Time of Receiving                                   |
| --- isOpened        | Boolean | O|	Opened or Not                                                |
| --- openedDate      | String  | X|	Date and Time of Opening                                     |
| --- dsnCode| String| X|	DSN(Delivery Status Notification) Status Code |
| --- dsnMessage| String| X|	DSN(Delivery Status Notification) Status Message |
| -- attachFileList   | List    | X|	List of attached files                                       |
| --- fileType        | String  | O|	Type of attached files <br/>MAIL: files attached to mail, TEMPLATE: files attached to template|
| --- fileId          | Integer  | O|	File ID                                                      |
| --- fileName        | String  | O|	Name of attached file                                        |
| --- filePath        | String  | O|	Path of attached file                                        |
| --- fileSize        | Integer | O|	Size of attached file (byte)                                 |
| --- createDate      | String  | O|	Date and time of creation                                    |
| -- customHeaders    | Map     | X|	[Custom Header](./console-guide/#custom-header)                   |
| -- senderGroupingKey|	String  | X|	Sender's group key                                           |
|-- statsId| String| X|	Key for statistics data grouping |

### View the List of Completed Email Delivery Updates

- When sending a regular email, a list of emails with updated status codes is displayed.
- Searches are based on the start and end times of the email status code update.
- The list of emails displayed is a list of emails with updated status codes.

#### Trackable Email Delivery Status Codes

- SST2: Sent
- SST3: Sent Failed
- SST5: Rejected
- SST7: Unverified
- SST8: Failed due to whitelist

#### [Caution]
- The SST2 (Sent) status code is based on the time of receipt, not the time of delivery.
- If delivery processing is delayed, the time of delivery and delivery may differ.
- The SST3 (Sent Failed) status code is updated when the service ultimately determines delivery failure.


#### Request

[URL]

| Http method | 	URI                                             |
|-------------|--------------------------------------------------|
| GET         | /email/v2.1/appKeys/{appKey}/sender/update-mails |

[Query parameter]

| Value                         | 	Type      | Required | 	Description                                                                                   |
|---------------------------|----------|----|---------------------------------------------------------------------------------------|
| startMailStatusUpdateDate | String | O | Start time of email sending status code update (yyyy-MM-dd HH:mm:ss) |
| endMailStatusUpdateDate | String | O | End time of email sending status code update (yyyy-MM-dd HH:mm:ss) |
| mailStatusCode | Integer | X | Sending status code <br/> SST2: Sent, SST3: Sent Failed, <br/> SST5: Rejected, SST7: Unverified, SST8: Failed due to whitelist |
| messageType | String | X | Message sending type (NORMAL: General, DM: Advertisement, AUTH: Authenticated)                                                      |
| pageNum                   | 	Integer | 	X | 	Page number 1(Default)                                                                        |
| pageSize                  | 	Integer | 	X | 	No. of views 15(Default)                                                                        |

[Header]

```
{
  "X-Secret-Key": String
}
```

| Value            | 	Type     | 	Required | 	Description                                           |
|--------------|---------|-----|-----------------------------------------------|
| X-Secret-Key | 	String | O   | Unique secretKey [[Notes](./api-guide/#secret-key)] |

#### cURL

```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/update-mails \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

#### Response

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "pageNum": 1,
    "pageSize": 10,
    "totalCount": 1,
    "data": [
      {
        "requestId": "20250101000000ABCDEFG0",
        "mailSeq": 0,
        "mailStatusCode": "SST2",
        "mailStatusName": "발송 성공",
        "requestDate": "2015-01-01 00:00:00",
        "mailStatusUpdatedDate": "",
        "resultDate": "2019-01-01 00:00:00",
        "openedDate": "2019-01-01 00:00:01",
        "dsnCode": "2.5.0",
        "dsnMessage": "SUCCESS",
        "senderGroupingKey": "groupKey"
      }
    ]
  }
}


```

| Value                       | 	Type      | 	Description                                                                                   |
|-------------------------|----------|---------------------------------------------------------------------------------------|
| header                  | 	Object  | 	Header area                                                                                |
| - isSuccessful          | 	Boolean | 	Success                                                                                |
| - resultCode            | 	Integer | 	Failure code                                                                                |
| - resultMessage         | 	String  | 	Failure message                                                                               |
| data                    | 	Object  | 	Data area                                                                               |
| - requestId             | String   | Request ID                                                                                 |
| - mailSeq               | Integer  | Email order name                                                                                 |
| - mailStatusCode        | 	String  | Delivery status code <br/> SST2: Sent, SST3: Sent Failed, <br/> SST5: Rejected, SST7: Unverified, SST8: Failure due to whitelist |
| - mailStatusName        | 	String  | 	Delivery status name                                                                              |
| - requestDate           | String   | Request date and time                                                                                 |
| - mailStatusUpdatedDate | String   | Email delivery status code update date 일시                                                                   |
| - resultDate            | String   | Received date and time                                                                                 |
| - openedDate            | String   | Read data and time                                                                                 |
| - dsnCode               | String   | DSN(Delivery Status Notification) Status code                                               |
| - dsnMessage            | String   | DSN(Delivery Status Notification) Status message                                              |
| - senderGroupingKey     | String   | Sender group key                                                                               |

### Query Mass Delivery List

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.1/appKeys/{appKey}/mass-mails|

[Path parameter]

|Value|	Type|	Descriptions|
|---|---|---|
|appKey|	String|	Unique appkey|

[Query parameter]

|Value|	Type|	Required|	Descriptions|
|---|---|---|---|
|requestId|	String|	O|	Request ID|
|startSendDate|	String|	O|	Start value of send date(yyyy-MM-dd HH:mm:ss)|
|endSendDate|	String|	O|	End value of send date (yyyy-MM-dd HH:mm:ss)|
|senderMail|	String|	X|	Sender mail address|
|senderName|	String|	X|	Sender name|
|templateId|	String|	X|	Template ID|
|sendStatus|	String|	X|	Send status code <br/> WAIT: wait, READY: ready to send, <br/>SENDREADY: Complete ready to send, SENDWAIT: Wait to send <br/>SENDING: sending, COMPLETE: Complete to send, <br/>FAIL: fail to send, CANCEL: cancel to send|
|pageNum|	Integer|	X|	Page number 1(default)|
|pageSize|	Integer|	X|	The number of queries 15(default)|

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value|	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | Unique secretKey [[note](./api-guide/#secret-key)] |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/mass-mails?startSendDate='"${START_DATE}"'&endSendDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

#### Response

```json
{ 
   "header":{ 
      "isSuccessful":true, 
      "resultCode":0, 
      "resultMessage":"SUCCESS" 
   }, 
   "body":{ 
      "pageNum":1, 
      "pageSize":15, 
      "totalCount":1, 
      "data":[ 
         { 
            "requestId":"202108061843001hIOqv82", 
            "requestDate":"2021-08-06 18:43:00", 
            "sendStatus":"COMPLETE", 
            "sendStatusName":"Complete to send ", 
            "templateId":"templateId", 
            "templateName":"templateName", 
            "senderName":"NHN Cloud", 
            "senderAddress":"email@nhncloud.com", 
            "title":"title", 
            "body":"body", 
            "adYn":"N", 
            "createDate":"2021-08-06 18:43:03", 
            "updateDate":"2021-08-06 18:43:03", 
            "statsId": "statsId" 
         } 
      ] 
   } 
}
```

|Value|	Type|	Not Null|	Descriptions|
|---|---|---|---|
|header|	Object|	O|	Header area|
|- isSuccessful|	Boolean|	O|	Successful or not|
|- resultCode|	Integer|	O|	Failure code|
|- resultMessage|	String|	O|	Failure message|
|body|	Object|	X|	Body area|
|- pageNum|	Integer|	X|	Current page number|
|- pageSize|	Integer|	X|	The number of data queried |
|- totalCount|	Integer|	X|	Total number of data|
|- data|	Object|	X|	Scope of data|
|-- requestId| String| O|	Request ID|
|-- requestDate| String| O|	Date and time of request|
|-- sendStatus|	String|	O|	Send status code <br/> WAIT: wait, READY: ready to send, <br/>SENDREADY: Complete ready to send, SENDWAIT: Wait to send <br/>SENDING: sending, COMPLETE: Complete to send, <br/>FAIL: fail to send, CANCEL: cancel to send|
|-- sendStatusName|	String|	O|	Name of send status |
|-- templateId|	String|	X|	Template ID|
|-- templateName|	String|	X|	Template name|
|-- senderName|	String|	X|	Sender name|
|-- senderAddress|	String|	X|	Sender mail address|
|-- title|	String|	X|	Title|
|-- body|	String|	X|	Content|
|-- adYn |  String  | O|	Advertisement or not |
|-- createDate |  String  | O|	Created date |
|-- updateDate |  String  | X|	Modified date |
|-- statsId| String| X|	Key for statistics data grouping |

### Query Recipient for Mass Delivery

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.1/appKeys/{appKey}/mass-mails/{requestId}|

[Path parameter]

|Value|	Type|	Descriptions|
|---|---|---|
|appKey|	String|	Unique appkey|
|requestId|	String|	Request ID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value|	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | Unique secretKey [[note](./api-guide/#secret-key)] |


[Query parameter]

|Value|	Type|	Required| 	Descriptions                                                                         |
|---|---|---|-----------------------------------------------------------------------------|
|receiveMailAddr|	String|	X| 	Receiving mail address                                                                   |
|startReceiveDate|	String|	X| 	Start value of receiving date (yyyy-MM-dd HH:mm:ss)                                            |
|endReceiveDate|	String|	X| 	End value of receiving date (yyyy-MM-dd HH:mm:ss)                                            |
|mailStatusCode|	String|	X| 	Send status code <br/> SST0:ready to send, SST1:sending,  <br/> SST2:Complete to send, SST3: Fail to send, SST7: Not approved |
|pageNum|	Integer|	X| 	Page number 1(default)                                                              |
|pageSize|	Integer|	X| 	The number of queries 15(default)                                                              |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/mass-mails/'"${REQUEST_ID}"'?startSendDate='"${START_DATE}"'&endSendDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

#### Response

```json
{ 
   "header":{ 
      "isSuccessful":true, 
      "resultCode":0, 
      "resultMessage":"SUCCESS" 
   }, 
   "body":{ 
      "pageNum":1, 
      "pageSize":15, 
      "totalCount":1, 
      "data":[ 
         { 
            "requestId":"20210806120100sA3CcWU2", 
            "mailSeq":1, 
            "mailStatusCode":"SST2", 
            "mailStatusName":" Complete to send ", 
            "resultId":"<20210806120100sA3CcWU2@tcmmsapp-92a901>", 
            "receiveType":"MRT0", 
            "receiveTypeName":"Recepient", 
            "receiveName":"name", 
            "receiveMailAddr":"email@nhncloud.com", 
            "isReceived":true, 
            "resultDate":"2021-08-06 13:02:00", 
            "isOpened":true, 
            "openedDate":"2021-08-06 12:34:30", 
            "dsnCode":"2.5.0", 
            "dsnMessage":"SUCCESS", 
            "createDate":"2021-08-06 12:01:41", 
            "updateDate":"2021-08-06 12:02:00", 
            "statsId": "statsId" 
         } 
      ] 
   } 
}
```

|Value|	Type|	Not Null|	Descriptions|
|---|---|---|---|
|header|	Object|	O|	Header area|
|- isSuccessful|	Boolean|	O|	Successful or not|
|- resultCode|	Integer|	O|	Failure code|
|- resultMessage|	String|	O|	Failure message|
|body|	Object|	X|	Body area|
|- pageNum|	Integer|	O|	Current page number|
|-pageSize|	Integer|	O|	The number of data queried |
|- totalCount|	Integer|	O|	Total number of data|
|- data|	List|	X|	Scope of data|
|-- requestId | String  | O|	Request ID |
|-- mailSeq | Integer  | O|	Mail order |
|-- mailStatusCode | String  | O|	Mail status code <br/> SST0:ready to send, SST1:sending,  <br/> SST2:Complete to send, SST3: Fail to send, SST7: Not approved|
|-- mailStatusName | String  | O|	Mail status name |
|-- resultId | String  | X|	SMTP ID |
|-- receiveType|	String|	O|	Recipient type<br/>MRT0 : Recipient , MRT1 : CC, MRT2 : BCC|
|-- receiveTypeName|	String|	O|	Recipient type name|
|-- receiveName|	String|	X|	Recipient name|
|-- receiveMailAddr|	String|	O|	Recipient email address|
|-- isReceived| Boolean| O|	Received or not |
|-- resultDate| String| X|	Date and time of receiving|
|-- isOpened| Boolean| O|	Opened or not |
|-- openedDate| String| X|	Date and time of open|
|-- dsnCode| String| X|	Delivery Status Notification (DSN) status code|
|-- dsnMessage| String| X|	Delivery Status Notification (DSN) status message |
|-- createDate |  String  | O|	Created date |
|-- updateDate |  String  | O|	Modified date |
|-- statsId| String| X|	Key for statistics data grouping |

### Query Mass Delivery Details

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.1/appKeys/{appKey}/mass-mails/{requestId}/{mailSeq}|

[Path parameter]

|Value|	Type|	Descriptions|
|---|---|---|
|appKey|	String|	Unique appkey|
|requestId|	String|	Request ID|
|mailSeq|	Integer|	Mail order|

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value|	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | Unique secretKey [[note](./api-guide/#secret-key)] |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/mass-mails/'"${REQUEST_ID}"'/'"${MAIL_SEQ}"' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

#### Response

```json
{ 
   "header":{ 
      "isSuccessful":true, 
      "resultCode":0, 
      "resultMessage":"SUCCESS" 
   }, 
   "body":{ 
      "data":{ 
         "requestId":"20210806123100Q60Z9I72", 
         "templateId":"test", 
         "templateName":"3443423", 
         "mailStatusCode":"SST2", 
         "mailStatusName":"Complete to send", 
         "requestDate":"2021-08-06 12:31:00", 
         "senderName":"name", 
         "senderAddress":"email@nhncloud.com", 
         "resultId":"<20210806123100Q60Z9I72@tcmmsapp-92a901>", 
         "resultDate":"2021-08-06 12:34:30", 
         "title":"test", 
         "body":"test", 
         "customHeaders":null, 
         "receiverList":[ 
            { 
               "requestId":"20210806123100Q60Z9I72", 
               "mailSeq":"1", 
               "receiveType":"MRT0", 
               "receiveTypeName":"Recepient", 
               "receiveMailAddr":"email@nhncloud.com", 
               "isReceived":true, 
               "resultDate":"2021-08-06 12:34:30", 
               "isOpened":true, 
               "openedDate":"2021-08-06 12:34:30", 
               "dsnCode":"2.5.0", 
               "dsnMessage":"SUCCESS" 
            } 
         ], 
         "attachFileList":[ 
            { 
               "fileType":"TEMPLATE", 
               "fileId":38915, 
               "fileName":"file", 
               "filePath":"1014535/toast-mt-2021-08-06/1229/38915", 
               "fileSize":1679, 
               "createDate":"2021-08-06 12:29:14" 
            } 
         ], 
         "statsId": "statsId" 
      } 
   } 
}
```

|Value|	Type|	Not Null|	Descriptions|
|---|---|---|---|
|header|	Object|	O|	Header area|
|- isSuccessful|	Boolean|	O|	Successful or not|
|- resultCode|	Integer|	O|	Failure code|
|- resultMessage|	String|	O|	Failure message|
|body|	Object|	X|	Body area|
|- data|	List|	X|	Scope of data|
|-- requestId  | String  | O|	Request ID |
|-- templateId | String  | X|	Template ID |
|-- templateName | String  | X|	Template name |
|-- mailStatusCode | String  | O|	Mail status code <br/> SST0:ready to send, SST1:sending,  <br/> SST2:Complete to send, SST3: Fail to send, SST7: Not approved |
|-- mailStatusName | String  | O|	Mail status name |
|-- requestDate | String  | O|	Request time |
|-- senderName | String  | X|	Sender name |
|-- senderAddress | String  | X|	Sender address |
|-- resultId | String  | X|	SMTP ID |
|-- resultDate | String  | X|	Actual sending time |
|-- title | String  | X|	Title |
|-- body | String  | X|	Content |
|-- customHeaders|	Map|	X|	[Customer Header](./console-guide/#custom-header) |
|-- receiverList | List| O|	List of recipients|
|--- requestId | String  | O|	Request ID |
|--- mailSeq | Integer  | O|	Mail order |
|--- receiveType | String  | O|	Recipient type(MRT0 : recipient , MRT1 : CC, MRT2 : BCC) |
|--- receiveTypeName | String  | O|	Recipient type name |
|--- receiveMailAddr | String  | O|	Recipient email address |
|--- isReceived| Boolean| O|	Received or not |
|--- resultDate| String| X|	Date and time of receiving|
|--- isOpened| Boolean| O|	Opened or not |
|--- openedDate| String| X|	Date and time of open|
|--- dsnCode| String| X|	Delivery Status Notification (DSN) status code|
|--- dsnMessage| String| X|	Delivery Status Notification (DSN) status message |
|-- attachFileList | List  | O|	Attached file list |
|--- fileType|	String|	O|	Attached file type(MAIL: files attached to mail, TEMPLATE: files attached to template)|
|--- fileId| Integer| O|	File ID|
|--- fileName|	String|	O|	Attached file name|
|--- filePath|	String|	O|	Attached file path|
|--- fileSize|	Integer|	O|	Attached file size(byte)|
|--- createDate|	String|	O|	Date and time of creation|
|-- statsId| String| X|	Key for statistics data grouping |

### Query Request for Tagged Mail Delivery

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.1/appKeys/{appKey}/tagMails|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Query Parameter]

| Value         | Type    | Required | Description                                                  |
| ------------- | ------- | -------- | ------------------------------------------------------------ |
| requestId     | String  | O        | Request ID                                                   |
| startSendDate | String  | O        | Start date of delivery (yyyy-MM-dd HH:mm:ss)                 |
| endSendDate   | String  | O        | End date of delivery (yyyy-MM-dd HH:mm:ss)                   |
| senderMail    | String  | X        | Sender's mail address                                        |
| senderName    | String  | X        | Sender's name                                                |
| templateId    | String  | X        | Template ID                                                  |
| sendStatus    | String  | X        | Delivery status code   WAIT: Waiting, READY: Ready,  SENDREADY: Ready for Delivery, SENDWAIT: Waiting for Delivery,  SENDING: Delivering, COMPLETE: Delivery Completed,  FAIL: Delivery Failed, CANCEL: Delivery Canceled |
| pageNum       | Integer | X        | Page number (default: 1)                                     |
| pageSize      | Integer | X        | Number of queries (default: 15)                              |

[Caution]

- If requestId is available, startSendDate and endSendDate are not required.
- If startSendDate and endSendDate are available, requestId is not required.

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/tagMails?startSendDate='"${START_DATE}"'&endSendDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

#### Response

```
{
    "header": {
        "isSuccessful": Boolean,
        "resultCode": Integer,
        "resultMessage": String
    },
    "body": {
        "pageNum": Integer,
        "pageSize": Integer,
        "totalCount": Integer,
        "data": [
            {
                "requestId": String,
                "requestIp": String,
                "requestDate": String,
                "tagSendStatus": String,
                "tagExpression": List:String,
                "templateId": String,
                "templateName": String,
                "senderName": String,
                "senderMailAddress": String,
                "title": String,
                "body": String,
                "attachYn": String,
                "adYn": String,
                "createUser": String,
                "createDate": String,
                "updateUser": String,
                "updateDate": String,
                "statsId": String
            }
        ]
    }
}
```

| Value            | Type        | Not Null|	Description                                                  |
| ---------------- | ----------- | ------------------------------------------------------------ |---|
| header           | Object      | O|	Header area                                                  |
| - isSuccessful   | Boolean     | O|	Successful or not                                            |
| - resultCode     | Integer     | O|	Failure code                                                 |
| - resultMessage  | String      | O|	Failure message                                              |
| body             | Object      | X|	Body area                                                    |
| - pageNum        | Integer     | O|	Current page number                                          |
| -pageSize        | Integer     | O|	Number of queried data                                       |
| - totalCount     | Integer     | O|	Total number of data                                         |
| - data           | List        | O|	Data area                                                    |
| -- requestId     | String      | O|	Request ID                                                   |
| -- requestIp     | String      | X|	Request IP                                                   |
| -- requestDate   | String      | O|	Request time                                                 |
| -- tagSendStatus | String      | O|	Delivery status code   WAIT: Waiting, READY: Ready,  SENDREADY: Ready for Delivery, SENDWAIT: Waiting for Delivery, SENDING: Delivering, COMPLETE: Delivery Completed,  FAIL: Delivery Failed, CANCEL: Delivery Canceled |
| -- tagExpression | List:String | O|	Tag expression (including Tag ID)                                               |
| -- templateId    | String      | X|	Template ID                                                  |
| -- templateName  | String      | X|	Template name                                                |
| -- senderName    | String      | X|	Sender's name                                                |
| -- senderMail    | String      | X|	Sender's address                                             |
| -- title         | String      | X|	Title                                                        |
| -- body          | String      | X|	Body                                                         |
| -- attachYn      | String      | O|	Attached or not                                              |
| -- adYn          | String      | O|	Ad or not                                                    |
| -- createUser    | String      | X|	Creator                                                      |
| -- createDate    | String      | O|	Date and time of creation                                    |
| -- updateUser    | String      | X|	Modifier                                                     |
| -- updateDate    | String      | X|	Date and time of modification                                |
|-- statsId| String|  X|Keys for grouping statistical data |

### Query Recipients of Tagged Mail Delivery

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.1/appKeys/{appKey}/tagMails/{requestId}|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|requestId|	String| Request ID |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Query Parameter]

| Value            | Type    | Required | Description                                                  |
| ---------------- | ------- | -------- | ------------------------------------------------------------ |
| receiveMail      | String  | X        | Recipient's mail address                                     |
| startReceiveDate | String  | X        | Start date of receiving (yyyy-MM-dd HH:mm:ss)                |
| endReceiveDate   | String  | X        | End date of receiving (yyyy-MM-dd HH:mm:ss)                  |
| receiveStatus    | String  | X        | Delivery status code   SST0: Preparing Delivery, SST1: Delivering,    SST2: Delivery Completed, SST3: Delivery Failed, SST7: Not Authenticated |
| pageNum          | Integer | X        | Page number (default: 1)                                     |
| pageSize         | Integer | X        | Number of queries (default: 15)                              |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/tagMails/'"${REQUEST_ID}"'?startSendDate='"${START_DATE}"'&endSendDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

#### Response

```
{
    "header": {
        "isSuccessful": Boolean,
        "resultCode": Integer,
        "resultMessage": String
    },
    "body": {
        "pageNum": Integer,
        "pageSize": Integer,
        "totalCount": Integer,
        "data": [
            {
                "requestId": String,
                "mailSequence": Integer,
                "receiveMail": String,
                "mailStatusCode": String,
                "mailStatusName": String,
                "resultId": String,
                "resultDate": String,
                "readYn": String,
                "readDate": String,
                "dsnCode": String,
                "dsnMessage": String,
                "createUser": String,
                "createDate": String,
                "updateUser": String,
                "updateDate": String,
                "statsId": "statsId",
                "mailSequence": 0
            }
        ]
    }
}
```

| Value             | Type    | Not Null|	Description                                                  |
| ----------------- | ------- | ------------------------------------------------------------ |---|
| header            | Object  | O|	Header area                                                  |
| - isSuccessful    | Boolean | O|	Successful or not                                            |
| - resultCode      | Integer | O|	Failure code                                                 |
| - resultMessage   | String  | O|	Failure message                                              |
| body              | Object  | X|	Body area                                                    |
| - pageNum         | Integer | O|	Current page number                                          |
| -pageSize         | Integer | O|	Number of queried data                                       |
| - totalCount      | Integer | O|	Total number of data                                         |
| - data            | List    | O|	Data area                                                    |
| -- requestId      | String  | O|	Request ID                                                   |
| -- mailSequence   | Integer | O|	Mail sequence                                                |
| -- receiveMail    | String  | O|	Recipient's address                                          |
| -- mailStatusCode | String  | O|	Delivery status code   SST0: Preparing Delivery, SST1: Delivering,    SST2: Delivery Completed, SST3: Delivery Failed, SST7: Not Authenticated |
| -- mailStatusName | String  | O|	Name of mail status                                          |
| -- resultId       | String  | X|	SMTP ID                                                      |
| -- resultDate     | String  | X|	Time of actual delivery                                      |
| -- readYn         | String  | X|	Read or not                                                  |
| -- readDate       | String  | X|	Read time                                                    |
| -- dsnCode| String| X|	DSN(Delivery Status Notification) Status Code |
| -- dsnMessage| String| X|	DSN(Delivery Status Notification) Status Message |
| -- createUser     | String  | X|	Creator                                                      |
| -- createDate     | String  | O|	Date and time of creation                                    |
| -- updateUser     | String  | X|	Modifier                                                     |
| -- updateDate     | String  | X|	Date and time of modification                                |
|-- statsId| String| X|	Key for statistics data grouping |

### Query Details of Tagged Mail Delivery

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.1/appKeys/{appKey}/tagMails/{requestId}/{mailSequence}|

[Path Parameter]

| Value        | Type    | Description     |
| ------------ | ------- | --------------- |
| appKey       | String  | Original appKey |
| requestId    | String  | Request ID      |
| mailSequence | Integer | Mail sequence   |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/tagMails/'"${REQUEST_ID}"'/'"${MAIL_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "data": {
            "requestId": "20190101000000ABCDEFG0",
            "requestIp": "127.0.0.1",
            "templateId": "TEMPLATE",
            "templateName": "Template",
            "mailStatusCode": "SST2",
            "mailStatusName": "Sending completed",
            "requestDate": "2019-01-01 00:00:00.0",
            "senderName": "sender",
            "senderMail": "sender@example.com",
            "resultId": "<20190101000000ABCDEFG0@mail>",
            "resultDate": "2019-01-01 00:00:00.0",
            "title": "Title",
            "body": "Body",
            "customHeaders": {
                "Content-Type": "text/html"
            },
            "receivers": [
                {
                    "requestId": "20190101000000ABCDEFG0",
                    "receiveType": "MRT0",
                    "receiveTypeName": "Recipient",
                    "receiveMailAddr": "receiver@example.com",
                    "readYn": "Y",
                    "readDate": "2019-01-01 00:00:00.0",
                    "dsnCode": "2.5.0",
                    "dsnMessage": "SUCCESS",
                    "mailSequence": "0"
                }
            ],
            "attachFileList": [
                {
                    "fileType": "MAIL",
                    "fileId": 12345,
                    "fileName": "test.csv",
                    "filePath": "file/path",
                    "fileSize": 10,
                    "createDate": "2019-01-01 00:00:00.0"
                }
            ],
            "statsId": "statsId"
        }
    }
}
```

| Value               | Type    | Not Null|	Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ | ------- |
| header              | Object  | O|	Header area                                                  |
| - isSuccessful      | Boolean | O|	Successful or not                                            |
| - resultCode        | Integer | O|	Failure code                                                 |
| - resultMessage     | String  | O|	Failure message                                              |
| body                | Object  | X|	Body area                                                    |
| - pageNum           | Integer | O|	Current page number                                          |
| -pageSize           | Integer | O|	Number of queried data                                       |
| - totalCount        | Integer | O|	Total number of data                                         |
| - data              | List    | X|	Data area                                                    |
| -- requestId        | String  | X|	Request ID                                                   |
| -- requestIp        | String  | X|	Request IP                                                   |
| -- templateId       | String  | X|	Template ID                                                  |
| -- templateName     | String  | X|	Template name                                                |
| -- mailStatusCode   | String  | O|	Mail status code   SST0: Preparing Delivery, SST1: Delivering,    SST2: Delivery Completed, SST3: Delivery Failed, SST7: Not Authenticated |
| -- mailStatusName   | String  | O|	Name of mail status                                          |
| -- requestDate      | String  | O|	Request time                                                 |
| -- resultDate       | String  | X|	Result time                                                  |
| -- senderName       | String  | X|	Sender's name                                                |
| -- senderMail       | String  | X|	Sender's address                                             |
| -- resultId         | String  | X|	SMTP ID                                                      |
| -- title            | String  | X|	Title                                                        |
| -- body             | String  | X|	Body                                                         |
| -- receivers        | List    | List of recipients                                           |
| --- requestId       | String  | O|	Request ID                                                   |
| --- receiveType     | String  | O|	Type of recipients (MRT0: recipient, MRT1: Cc, MRT2: Bcc)    |
| --- receiveTypeName | String  | X|	Name of recipient type                                       |
| --- receiveMailAddr | String  | O|	Recipient's mail address                                     |
| --- readYn          | String  | X|	Read or not                                                  |
| --- readDate        | String  | X|	Read time                                                    |
| --- dsnCode| String| X|	DSN(Delivery Status Notification) Status Code |
| --- dsnMessage| String| X|	DSN(Delivery Status Notification) Status Message |
| --- mailSequence    | Integer | O|	Mail sequence                                                |
| -- attachFileList   | List    | O|	List of attached files                                       |
| --- fileType        | String  | O|	Type of attached file (MAIL: files attached to mail, TEMPLATE: files attached to template) |
| --- fileId          | Integer  | O|	File ID                                                      |
| --- fileName        | String  | O|	Name of attached file                                        |
| --- filePath        | String  | O|	Path of attached file                                        |
| --- fileSize        | Integer | O|	Size of attached file (byte)                                 |
| --- createDate      | String  | O|	Date and time of creation                                    |
| -- customHeaders    | Map     | X|	[Custom Header](./console-guide/#custom-header)                   |
|-- statsId| String| X|	Key for statistical data grouping |


## Scheduled Delivery Management

### List Scheduled Delivery
#### Request

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v2.1/appKeys/{appKey}/sender/reservations|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String|	Original appKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Query parameter]

|Value| Type |	Required | Description |
|---|---|---|---|
| requestId | String | X | Request ID |
| startSendDate | String | X | Start date of sending (yyyy-MM-dd HH:mm:ss) |
| endSendDate | String | X | End date of sending (yyyy-MM-dd HH:mm:ss) |
| senderMail | String | X | Sender's mail address                                        |
| receiveMail | String | X | Recipient's mail address                                     |
| templateId | String | X | Template ID |
| pageNum|	Integer|	X| Page number (default: 1) |
| pageSize|	Integer|	X| Number of queries (default: 15) |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/reservations' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```


#### Response

```json
{
   "header":{
      "isSuccessful":true,
      "resultCode":0,
      "resultMessage":"SUCCESS"
   },
   "body":{
      "totalCount":1,
      "pageNum": 1,
      "pageSize": 15,
      "data":[
         {
            "requestId":"20200605175300tu5z04e1",
            "mailSeq":0,
            "title":"Title",
            "templateId":"TemplateId",
            "templateName":"TemplateName",
            "senderAddress":"sender@example.com",
            "senderName":"senderName",
            "receiveMailAddr":"receiver@example.com",
            "receiveName":"receiverName",
            "receiveType":"MRT0",
            "receiveTypeName":"받는사람",
            "requestDate":"2020-06-05 17:53:00",
            "mailStatusCode":"SST4",
            "mailStatusName":"예약대기",
            "senderGroupingKey":"senderKey",
            "statsId": "statsId"
         }
      ]
   }
}
```

| Value               | Type    | Not Null|	Description                                                  |
|---|---|---|---|
| header              | Object  | O|	Header area                                                  |
| - isSuccessful      | Boolean | O|	Successful or not                                            |
| - resultCode        | Integer | O|	Failure code                                                 |
| - resultMessage     | String  | O|	Failure message                                              |
| body                | Object  | X|	Body area                                                    |
| - pageNum           | Integer | O|	Current page number                                          |
| - pageSize          | Integer | O|	Number of queried data                                       |
| - totalCount        | Integer | O|	Number of total data                                         |
| - data              | List    | X|	Data area                                                    |
| -- requestId        | String  | O|	Request ID                                                   |
| -- mailSeq          | Integer | O|	Mail sequence                                                |
| -- title            | String  | X|	Title                                                        |
| -- templateId       | String  | X|	Template ID                                                  |
| -- templateName     | String  | X|	Template name                                                |
| -- senderAddress    | String  | X|	Sender's mail address                                        |
| -- senderName       | String  | X|	Sender's name                                                |
| -- receiveMailAddr  | String  | O|	Recipient's mail address                                     |
| -- receiveName      | String  | O|	Name of recipient                                            |
| -- receiveType      | String  | O|	Recipient type  (MRT0: recipient, MRT1: cc, MRT2: bcc)       |
| -- receiveTypeName  | String  | X|	Name of recipient type                                       |
| -- requestDate      | String  | O|	Date and time of sending                                     |
| -- mailStatusCode   | String  | O|	Delivery status code<br/>SST0: Preparing Delivery, SST1: Delivering<br/>SST2: Delivery Completed, SST3: Delivery Failed<br/>SST4: Scheduled Waiting, SST7: Not Authenticated     |
| -- mailStatusName   | String  | O|	Name of delivery status                                      |
| -- senderGroupingKey| String  | X|	Sender's group key                                           |
|-- statsId| String| X|	Key for statistical data grouping |


### Query Detail Scheduled Delivery
#### Request

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v2.1/appKeys/{appKey}/sender/reservations/{requestId}/{mailSeq}|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|requestId|	String| Request ID |
|mailSeq|	Integer| Mail order (mailSeq value returned when a mailing list is queried) |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/reservations/'"${REQUEST_ID}"'/'"${MAIL_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

#### Response

```json
{
   "header":{
      "isSuccessful":true,
      "resultCode":0,
      "resultMessage":"SUCCESS"
   },
   "body":{
      "data":{
         "requestId":"202006050000008j6bx5Q1",
         "mailSeq":0,
         "title":"Title",
         "body":"Body",
         "templateId": "TemplateId",
         "templateName":"TemplateName",
         "senderAddress":"sender@example.com",
         "senderName":"senderName",
         "requestDate":"2020-06-05 00:00:00",
         "mailStatusCode":"SST4",
         "mailStatusName":"Waiting for reservation",
         "receivers":[
            {
               "requestId":"202006050000008j6bx5Q1",
               "mailSeq":0,
               "receiveType":"MRT0",
               "receiveTypeName":"Recipient",
               "receiveName":"receiverName",
               "receiveMailAddr":"receiver@example.com"
            }
         ],
         "attachFileList":[
            {
               "fileType":"Mail",
               "fileId": 123456,
               "fileName":"attachement.jpg",
               "filePath":"28578/toast-mt-2020-06-03/2344/183037",
               "fileSize":8074,
               "createDate":"2020-06-03 23:44:27"
            }
         ],
         "customHeaders":{
                "Content-Type": "text/html"
         },
         "senderGroupingKey":"senderKey",
         "statsId": "statsId"
      }
   }
}
```

| Value               | Type    | Not Null|	Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |---|
| header              | Object  | X|	Header area                                                  |
| - isSuccessful      | Boolean | O|	Successful or not                                            |
| - resultCode        | Integer | O|	Failure code                                                 |
| - resultMessage     | String  | O|	Failure message                                              |
| body                | Object  | O|	Body area                                                    |
| - data              | Object  | X|	Data area                                                    |
| -- requestId        | String  | O|	Request ID                                                   |
| -- mailSeq          | Integer | O|	Mail sequence                                                |
| -- title            | String  | X|	Mail title                                                   |
| -- body             | String  | X|	Mail body                                                    |
| -- templateId       | String  | X|	Template ID                                                  |
| -- templateName     | String  | X|	Template name                                                |
| -- senderAddress    | String  | X|	Sender's mail address                                        |
| -- senderName       | String  | X|	Sender's name                                                |
| -- requestDate      | String  | O|	Date and time of sending                                     |
| -- mailStatusCode   | String  | O|	Delivery status code<br/>SST0: Preparing Delivery, SST1: Delivering<br/>SST2: Delivery Completed, SST3: Delivery Failed<br/>SST4: Scheduled Waiting , SST7: Not Authenticated    |
| -- mailStatusName   | String  | O|	Name of delivery status                                      |
| -- receiverList     | List    | O|	List of recipients                                           |
| --- requestId        | String  | O|	Request ID                                                   |
| --- mailSeq          | Integer | O|	Mail sequence                                                |
| --- receiveType      | String  | O|	Recipient type  (MRT0: recipient, MRT1: cc, MRT2: bcc)       |
| --- receiveName      | String  | X|	Name of recipient                                            |
| --- receiveTypeName  | String  | O|	Name of recipient type                                       |
| --- receiveMailAddr  | String  | O|	Recipient's mail address                                     |
| -- attachFileList   | List    | O|	List of attached files                                       |
| --- fileType        | String  | O|	Type of attached files <br/>MAIL: files attached to mail, TEMPLATE: files attached to template|
| --- fileId          | Integer  | O|	File ID <br/> Attachments uploaded through the v1.0, v1.1 APIs are marked with a file ID value of -1 |
| --- fileName        | String  | O|	Name of attached file                                        |
| --- filePath        | String  | O|	Path of attached file                                        |
| --- fileSize        | Integer | O|	Size of attached file (byte)                                 |
| --- createDate      | String  | O|	Date and time of creation                                    |
| -- customHeaders    | Map     | O|	[Custom Header](./console-guide/#custom-header)                   |
| -- senderGroupingKey| String  | X|	Sender's group key (up to 100 characters)                                           |
|-- statsId| String|	X| Keys for grouping statistical data |


### Cancel Scheduled Delivery by Request
#### Request

[URL]

|Http method|	URI|
|---|---|
|PUT| /email/v2.1/appKeys/{appKey}/sender/reservations/{requestId}

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|requestId|	String| Request ID |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/reservations/'"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

#### Response

```json
{
    "header" : {
        "isSuccessful" : true,
        "resultCode" : 0,
        "resultMessage" : "SUCCESS"
    }
}
```

| Value               | Type    | Not Null|	Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |---|
| header              | Object  | O|	Header area                                                  |
| - isSuccessful      | Boolean | O|	Successful or not                                            |
| - resultCode        | Integer | O|	Failure code                                                 |
| - resultMessage     | String  | O|	Failure message                                              |

### Cancel Scheduled Delivery by recipient
#### Request

[URL]

|Http method|	URI|
|---|---|
|PUT| /email/v2.1/appKeys/{appKey}/sender/reservations/{requestId}/{mailSeq}

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|requestId|	String| Request ID |
|mailSeq|	Integer| Mail order (mailSeq value returned when a mailing list is queried) |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/reservations/'"${REQUEST_ID}"'/'"${MAIL_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

#### Response

```json
{
    "header" : {
        "isSuccessful" : true,
        "resultCode" : 0,
        "resultMessage" : "SUCCESS"
    }
}
```

| Value               | Type    | Not Null|	Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |---|
| header              | Object  | O|	Header area                                                  |
| - isSuccessful      | Boolean | O|	Successful or not                                            |
| - resultCode        | Integer | O|	Failure code                                                 |
| - resultMessage     | String  | O|	Failure message                                              |

### Cancel Scheduled Delivery - Multiple Filter
#### Request

[URL]

|Http method|	URI|
|---|---|
|PUT| /email/v2.1/appKeys/{appKey}/sender/reservations/search-cancels

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String|	Original appkey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Request body]

```json
{
    "searchParameter" : {
        "requestId" : "202006051209288SxRXhejd20",
        "startSendDate" : "2020-02-01 00:00",
        "endSendDate" : "2020-02-01 10:00",
        "senderMail" : "sender@example.com",
        "receiveMail" : "receiver@example.com",
        "templateId" : "templateId"
    },
    "updateUser" : "UpdateUser"
}
```

| Value            | Type    | Required | Description                                                  |
|---|---|---|---|
| searchParameter.requestId | String | X | Request ID |
| searchParameter.startSendDate | String | O | Start date of delivery (yyyy-MM-dd HH:mm:ss) |
| searchParameter.endSendDate | String | O | End date of delivery (yyyy-MM-dd HH:mm:ss) |
| searchParameter.senderMail | String | X | Sender's mail address |
| searchParameter.receiveMail | String | X | Recipient's mail address |
| searchParameter.templateId | String | X | Template ID |
| updateUser | String | O | Requester of Scheduled Cancellation |

* **startSendDate**, **endSendDate**, **updateUser** are required fields.

#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/reservations/search-cancels' \
-H 'Content-Type: application/json;charset=UTF-8'  \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "searchParameter": {
        "requestId": "202006051209288SxRXhejd20",
        "startSendDate": "2020-02-01 00:00",
        "endSendDate": "2020-02-01 10:00",
        "senderMail": "sender@example.com",
        "receiveMail": "receiver@example.com",
        "templateId": "templateId"
    },
    "updateUser": "UpdateUser"
}'
```

#### Response

```json
{
    "header" : {
        "resultCode" : 0,
        "resultMessage" : "success",
        "isSuccessful" : true
    },
    "body" : {
        "data" : {
            "reservationCancelId" : "202006051209288SxRXhejd20",
            "requestedDateTime" : "2020-06-05 16:09:24",
            "reservationCancelStatus" : "READY"
        }
    }
}
```

| Value               | Type    | Not Null|	Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |---|
| header              | Object  | X|	Header area                                                  |
| - isSuccessful      | Boolean | O|	Successful or not                                            |
| - resultCode        | Integer | O|	Failure code                                                 |
| - resultMessage     | String  | O|	Failure message                                              |
| body                | Object  | O|	Body area                                                    |
| - data              | Object  | X|	Data area                                                    |
| -- reservationCancelId|	String |	O|	Schedule Cancellation ID|
| -- requestedDateTime|	String|	O|	Time for Schedule Cancellation(yyyy-MM-dd HH:mm:ss)|
| -- reservationCancelStatus|	String|	O|	Status of Schedule Cancellation<br/>- READY : Preparing for Scheduling<br/>- PROCESSING : Cancelling Schedule  <br/>- COMPLETED : Schedule Cancellation Completed<br/>- FAILED : Schedule Cancellation Failed |


### List Request of Scheduled Delivery Cancellation - Multiple Filter
#### Request

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v2.1/appKeys/{appKey}/sender/reservations/search-cancels

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String|	Original appkey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Query parameter]

|Value| Type |	Required | Description |
|---|---|---|---|
| startRequestedDateTime | String | X |  Request Start Time for Schedule Cancellation(yyyy-MM-dd HH:mm:ss) |
| endRequestedDateTime | String | X | Request End Time for Schedule Cancellation(yyyy-MM-dd HH:mm:ss) |
| reservationCancelId | String | X | Schedule Cancellation ID|
| pageNum|	Integer|	X| Page number (default: 1) |
| pageSize|	Integer|	X| Number of queries (default: 15) |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/sender/reservations/search-cancels' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

#### Response

```json
{
    "header" : {
        "resultCode" : 0,
        "resultMessage" : "success",
        "isSuccessful" : true
    },
    "body" : {
        "pageNum" : 1,
        "pageSize" : 15,
        "totalCount" : 1,
        "data" : [
            {
                "reservationCancelId" : "202006051209288SxRXhejd20",
                "searchParameter" : {
                    "serviceId" : 12345,
                    "requestId" : "2020060asdasd8SxRXhejd20",
                    "startSendDate" : "2020-06-01 00:00:00",
                    "endSendDate" : "2020-06-11 00:00:00",
                    "senderMail" : "sender@example.com",
                    "receiveMail" : "receiver@example.com",
                    "templateId" : "templateId",
                    "sendStatus" : null,
                    "searchedBySendDate" : true
                },
                "requestedDateTime" : "2020-06-05 12:09:28",
                "completedDateTime" : "2020-06-05 12:16:05",
                "reservationCancelStatus" : "COMPLETED",
                "totalCount" : 3,
                "successCount" : 3,
                "createUser" : "tester",
                "createdDateTime" : "2020-06-05 12:09:28",
                "updatedDateTime" : "2020-06-05 12:16:05"
            }
        ]
    }
}
```

| Value               | Type    | Not Null|	Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |---|
| header              | Object  | X|	Header area                                                  |
| - isSuccessful      | Boolean | O|	Successful or not                                            |
| - resultCode        | Integer | O|	Failure code                                                 |
| - resultMessage     | String  | O|	Failure message                                              |
| body                | Object  | O|	Body area                                                    |
| - pageNum           | Integer | O|	Current page number                                          |
| - pageSize          | Integer | O|	Number of queried data                                       |
| - totalCount        | Integer | O|	Number of total data                                         |
| - data              | List    | X|	Data area                                                    |
|-- reservationCancelId|	String | O|	Schedule Cancellation ID |
|-- searchParameter | Map<String, Object> | O|	Request Parameter for Schedule Cancellation |
|-- requestedDateTime|	String | O|	Request Time for Schedule Cancellation|
|-- completedDateTime|	String | X|	Request End Time for Schedule Cancellation |
|-- reservationCancelStatus|	String| 	O|	Status of Schedule Cancellation<br/>- READY : Preparing for Scheduling<br/>- PROCESSING : Cancelling Schedule <br/>- COMPLETED : Schedule Cancellation Completed<br/>- FAILED : Schedule Cancellation Failed |
|-- totalCount | Integer |  O|	Number of Scheduled Cancellation Targets |
|-- successCount | Integer |O|	Number of Successful Schedule Cancellation |
|-- createUser |	String| 	X| Requester of Scheduled Cancellation	|
|-- createdDateTime | String | X|	Request Creation Time for Schedule Cancellation |
|-- updatedDateTime | String | X|	Modified Time for Scheduled Cancellation  |

<p id="category"></p>

## Category Management 

### List  

#### Request 

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v2.1/appKeys/{appKey}/categories|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Query parameter]

|Value| Type | Required | Description |
|---|---|---|---|
|useYn|	String|	X|	Use or Not: Y, N |
|categoryParentId|	Integer|	X|	Parent category ID |
|pageNum|	Integer|	X| Page number (default: 1) |
|pageSize|	Integer|	X| Number of queries (default: 15) |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/categories' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

#### Response 

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "pageNum": 1,
        "pageSize": 15,
        "totalCount": 3,
        "data": [
            {
                "categoryId": 12345,
                "categoryParentId": 0,
                "depth": 0,
                "categoryName": "Category",
                "categoryDesc": "Top Category",
                "useYn": "Y",
                "createUser": "user",
                "createDate": "2019-07-23 00:00:00.0",
                "updateUser": "user",
                "updateDate": "2019-07-23 00:00:00.0"
            }
        ]
    }
}
```

|Value| Type | Not Null|	Description |
|---|---|---|---|
|header|	Object| X|	Header area |
|- isSuccessful|	Boolean| O|	Successful or not |
|- resultCode|	Integer| O|	Failure code |
|- resultMessage|	String| O|	Failure message |
|body|	Object| O|	Body area |
|- pageNum|	Integer| O|	Current page number |
|- pageSize|	Integer| O|	Queried data count |
|- totalCount|	Integer| O|	Total data count |
|- data|	List| X|	Data area |
|-- categoryId|	Integer| O|	Category ID |
|-- categoryParentId|	Integer| O|	Parent category ID (0, for the highest-level category) |
|-- depth|	Integer| O|	Depth (0, for the highest-level category) |
|-- categoryName|	String| O|	Category name |
|-- categoryDesc|	String| X|	Category description |
|-- useYn|	String| O|	Use or not |
|-- createUser|	String| X|	Creator |
|-- createDate|	String| O|	Date and time of creation |
|-- updateUser|	String| X|	Modifier |
|-- updateDate|	String| X|	Date and time of modification |

### Query Details 

#### Request 

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.1/appKeys/{appKey}/categories/{categoryId}|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|categoryId|	String| Category ID |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/categories/'"${CATEGORY_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "data": {
            "categoryId": 12345,
            "categoryParentId": 0,
            "depth": 0,
            "categoryName": "Category",
            "categoryDesc": "Top Category",
            "useYn": "Y",
            "createUser": "user",
            "createDate": "2019-07-23 00:00:00.0",
            "updateUser": "user",
            "updateDate": "2019-07-23 00:00:00.0"
        }
    }
}
```

|Value| Type | Not Null|	Description |
|---|---|---|---|
|header|	Object| O|	Header area |
|- isSuccessful|	Boolean| O|	Successful or not |
|- resultCode|	Integer| O|	Failure code |
|- resultMessage|	String| O|	Failure message |
|body|	Object| X|	Body area |
|- data|	List| O|	Data area |
|-- categoryId|	Integer| O|	Category ID |
|-- categoryParentId|	Integer| O|	Parent category ID (0, for the highest-level category) |
|-- depth|	Integer| O|	Depth (0, for the highest-level category) |
|-- categoryName|	String| O|	Category name |
|-- categoryDesc|	String| X|	Category description |
|-- useYn|	String| O|	Use or not |
|-- createUser|	String| X|	Creator |
|-- createDate|	String| O|	Date and time of creation |
|-- updateUser|	String| X|	Modifier |
|-- updateDate|	String| X|	Date and time of modification |


### Register

#### Request 

[URL]

|Http method|	URI|
|---|---|
|POST|/email/v2.1/appKeys/{appKey}/categories|


[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Request body]

|Value| Type |	Max Length | Required | Description |
|---|---|---|---|---|
| categoryParentId |	Integer|	- | X |	Parent category ID <br/>Highest-level category ID (default) |
| categoryName |	String|	200 | O |	Category name |
| categoryDesc |	String| 1000 |	X | Category description |
| useYn |	String| 1 |	X| Use or not: Y (default), N |
| userId | String | 50 | X | User ID |

#### cURL
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/categories'
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "categoryParentId": 12345,
    "categoryName": "Category",
    "categoryDesc": "Top Category",
    "useYn": "Y",
    "userId": "USER"
}'
```


#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "data": {
            "categoryId": 12346
        }
    }
}
```

|Value| Type | Not Null|	Description |
|---|---|---|---|
|header|	Object| O|	Header area |
|- isSuccessful|	Boolean| O|	Successful or not |
|- resultCode|	Integer| O|	Failure code |
|- resultMessage|	String| O|	Failure message |
|body|	Object| X|	Body area |
|- data|	Object| O|	Data area |
|-- categoryId|	Integer| O|	Cateogry ID |


### Modify

#### Request 

[URL]

|Http method|	URI|
|---|---|
|PUT|/email/v2.1/appKeys/{appKey}/categories/{categoryId}|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|categoryId|	Integer| Category ID |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Request body]

|Value| Type |	Max Length | Required | Description |
|---|---|---|---|---|
| categoryName |	String|	200 | X |	Category name |
| categoryDesc |	String| 1000 |	X | Category description |
| useYn |	String| 1 |	X| Use or not: Y, N |
| userId | String | 50 | X | User ID |

#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/categories/'"${CATEGORY_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "categoryName": "Category",
    "categoryDesc": "Top Category",
    "useYn": "Y",
    "userId": "USER"
}'
```

#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": null
}
```

|Value| Type | Not Null|	Description |
|---|---|---|---|
|header|	Object| O|	Header area |
|- isSuccessful|	Boolean| O|	Successful or not |
|- resultCode|	Integer| O|	Failure code |
|- resultMessage|	String| O|	Failure message |

### Delete

#### Request

[URL]

|Http method|	URI|
|---|---|
|DELETE|/email/v2.1/appKeys/{appKey}/categories/{categoryId}|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|categoryId|	Integer| Category ID |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

#### cURL
```
curl -X DELETE \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/categories/'"${CATEGORY_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": null
}
```

|Value| Type | Not Null|	Description |
|---|---|---|---|
|header|	Object| O|	Header area |
|- isSuccessful|	Boolean| O|	Successful or not |
|- resultCode|	Integer| O|	Failure code |
|- resultMessage|	String| O|	Failure message |

<p id="template"></p>

## Query of Templates

### Query List of Templates

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.1/appKeys/{appKey}/templates|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Query Parameter]

| Value      | Type    | Required | Description                        |
| ---------- | ------- | -------- | ---------------------------------- |
| categoryId | Integer | X        | Category ID                        |
| useYn      | String  | X        | Use or Not (Y/N)                   |
| pageNum    | Integer | X        | Page number (default: 1)           |
| pageSize   | Integer | X        | Number of queries (default: 15)    |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/templates' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

#### Response

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "success",
        "isSuccessful": true
    },
    "body": {
        "pageNum": 1,
        "pageSize": 15,
        "totalCount": 1,
        "data": [
            {
                "templateId": "TEMPLATE",
                "categoryId": 12345,
                "categoryName": "Category",
                "templateName": "Template",
                "templateDesc": "Template",
                "useYn": "Y",
                "delYn": "N",
                "title": "Title",
                "createDate": "2019-01-01 00:00:00.0",
                "updateDate": "2019-01-01 00:00:00.0"
            }
        ]
    }
}
```

|Value| Type | Not Null|	Description |
|---|---|---|---|
|header|	Object| O|	Header area |
|- isSuccessful|	Boolean| O|	Successful or not |
|- resultCode|	Integer| O|	Failure code |
|- resultMessage|	String| O|	Failure message |
|body|	Object| X|	Body area |
|- pageNum|	Integer| O|	Current page number |
|- pageSize|	Integer| O|	Number of queried data |
|- totalCount|	Integer| O|	Total number of data |
|- data|	List| X|	Data area |
|-- templateId|	String| O|	Template ID |
|-- categoryId|	Integer| O|	Category ID |
|-- categoryName|	String| O|	Category name |
|-- templateName|	String| O|	Template name |
|-- templateDesc|	String| X|	Template description |
|-- useYn|	String| O|	Use or not |
|-- delYn|	String| O|	Delete or not |
|-- title|	String| O|	Title |
|-- createDate|	String| O|	Date and time of creation |
|-- updateDate|	String| O|	Date and time of modification |

### Query Template Details

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.1/appKeys/{appKey}/templates/{templateId}|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|templateId|	String| Template ID |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

#### Response

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "success",
        "isSuccessful": true
    },
    "body": {
        "data": {
            "templateId": "TEMPLATE",
            "categoryId": 12345,
            "categoryName": "Category",
            "templateName": "Template",
            "templateDesc": "Template",
            "useYn": "Y",
            "delYn": "N",
            "sendMailAddress": "test@nhn.com",
            "title": "Title",
            "templateType": "DEFAULT",
            "body": "Body",
            "createDate": "2019-01-01 00:00:00.0",
            "updateDate": "2019-01-01 00:00:00.0",
            "attachFileList": [
                {
                    "fileType": "TEMPLATE",
                    "fileId": 12345,
                    "fileName": "test.csv",
                    "filePath": "file/path",
                    "fileSize": 10,
                    "createDate": "2019-01-01 00:00:00.0"
                }
            ]
        }
    }
}
```

|Value| Type | Not Null|	Description |
|---|---|---|---|
|header|	Object| O|	Header area |
|- isSuccessful|	Boolean| O|	Successful or not |
|- resultCode|	Integer| O|	Failure code |
|- resultMessage|	String| O|	Failure message |
|body|	Object| X|	Body area |
|- data|	List| O|	Data area |
|-- templateId|	String| O|	Template ID |
|-- categoryId|	Integer| O|	Category ID |
|-- categoryName|	String| O|	Category name |
|-- templateName|	String| O|	Template name |
|-- templateDesc|	String| X|	Template description |
|-- useYn|	String| O|	Use or not (Y= In use, N= Not in use) |
|-- delYn|	String| O|	Delete or not (Y= Delete, N= Not delete) |
|-- sendMailAddress|	String| X|	Sender's mail address |
|-- title|	String| O|	Mail address |
|-- templateType|	String| O|	Template type <br/>DEFAULT(default), FREEMARKER) |
|-- body|	String| X|	Body |
|-- createDate|	String| O|	Date and time of creation |
|-- updateDate|	String| O|	Date and time of modification |
|-- attachFileList|	List| O|	List of attached files |
|--- fileType|	String| O|	Attachment type (MAIL: Files attached to mail, TEMPLATE: Files attached to template) |
|--- fileId| Integer| O|	File ID |
|--- fileName|	String| O|	Name of attached file |
|--- filePath|	String| O|	Path of attached file |
|--- fileSize|	Integer| O|	Size of attached file (byte) |
|--- createDate|	String| O|	Date and time of creation |

### Register Templates 

#### Request 

[URL]

|Http method|   URI|
|---|---|
|POST|  /email/v2.1/appKeys/{appKey}/templates|


[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|    String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Request body]

|Value| Type | Max Length | Required | Description |
|---|---|---|---|---|
| categoryId |  Integer|    - | O | Category ID |
| templateId | String | 50 | O | Template ID |
| templateName |    String| 200 |   O | Template name |
| templateDesc |    String| 4000 |  X | Template description |
| useYn |   String| 1 | X| Use or not: Y (default), N |
| sendMailAddress | String| 300 | O| Sender's mail address |
| title | String | 500 | O | Mail title |
| templateType |    String| 10 |    X| Template type <br/>DEFAULT (default), FREEMARKER |
| body | String | - | O | Mail body |
| attachFileIdList | List<Integer> | - | X | Attached file ID (fileId) |
| userId | String | 50 | X | User ID |

#### cURL
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/templates' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "categoryId": 1,
    "templateId": "TEAMPLTE_ID",
    "templateName": "템플릿 이름",
    "templateDesc": "템플릿 설명",
    "useYn": "Y",
    "sendMailAddress": "test@example.com",
    "title": "메일 제목",
    "templateType": "DEFAULT",
    "body": "메일 내용",
    "attachFileIdList": [1, 2, 3],
    "userId": "USER"
}'
```


#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": null
}
```

|Value| Type | Not Null|	Description |
|---|---|---|---|
|header|    Object| 	O|Header area |
|- isSuccessful|    Boolean| 	O|Successful or not |
|- resultCode|  Integer| 	O|Failure code |
|- resultMessage|   String| 	O|Failure message |

### Upload Attached Files 

#### Request

[URL]

|Http method|   URI|
|---|---|
|POST|  /email/v2.1/appKeys/{appKey}/templates/attachfile/binaryUpload|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|    String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Request body]

|Value| Type | Max Length | Required | Description |
|---|---|---|---|---|---|
|fileName|  String| - |O| File name |
|fileBody|  Byte[]| - |O| Byte [] value |
|userId|    String| 50|X| User ID |

#### cURL
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/templates/attachfile/binaryUpload' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "fileName": "file.csv",
    "userId": "USER",
    "fileBody": []
}'
```

#### Response

```json
{
  "header": {
    "isSuccessful":  true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "fileId": 1,
      "fileName": "file.csv"
    }
  }
}
```

|Value| Type | Not Null|	Description |
|---|---|---|---|
|header|    Object| O|	Header area |
|- isSuccessful|    Boolean| O|	Successful or not |
|- resultCode|  Integer| O|	Failure code |
|- resultMessage|   String| O|	Failure message |
|body|  Object| X|	Body area |
|- data|    Object| X|	Data area |
|-- fileId| Integer| O|	File ID |
|-- fileName|   String| O|	File name |

### Modify Templates

#### Request

[URL]

|Http method|   URI|
|---|---|
|PUT|   /email/v2.1/appKeys/{appKey}/templates/{templateId}|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|    String| Original appKey |
|templateId|    String| Template ID |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Request body]

|Value| Type | Max Length | Required | Description |
|---|---|---|---|---|
| templateName |    String| 200 |   X | Template name |
| templateDesc |    String| 4000 |  X | Template description |
| useYn |   String| 1 | X | Use or not: Y, N |
| sendMailAddress | String| 300 | X| Sender's mail address |
| title | String | 500 | X | Mail title |
| templateType |    String| 10 |    X| Template type <br/>DEFAULT (default), FREEMARKER |
| body | String | - | X | Mail body |
| attachFileIdList | List<Integer> | - | X | Attached file ID (fileId) |
| userId | String | 50 | X | User ID |

#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "templateName": "템플릿 이름",
    "templateDesc": "템플릿 설명",
    "useYn": "Y",
    "sendMailAddress": "test@example.com",
    "title": "메일 제목",
    "templateType": "DEFAULT",
    "body": "메일 내용",
    "attachFileIdList": [1, 2, 3],
    "userId": "USER"
}'
```

#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": null
}
```

|Value| Type | Description |
|---|---|---|
|header|    Object| Header area |
|- isSuccessful|    Boolean| Successful or not |
|- resultCode|  Integer| Failure code |
|- resultMessage|   String| Failure message |

### Delete Templates

#### Request

[URL]

|Http method|   URI|
|---|---|
|DELETE|    /email/v2.1/appKeys/{appKey}/templates/{templateId}|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|    String| Original appKey |
|templateId|    String| Template ID |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

#### cURL
```
curl -X DELETE \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": null
}
```

|Value| Type | Description |
|---|---|---|
|header|    Object| Header area |
|- isSuccessful|    Boolean| Successful or not |
|- resultCode|  Integer| Failure code |
|- resultMessage|   String| Failure message |


## Tag Management

### Query Tags

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.1/appKeys/{appKey}/tags|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Query Parameter]

| Value    | Type    | Required | Description                      |
| -------- | ------- | -------- | -------------------------------- |
| pageNum  | Integer | X        | Page number (Default : 1)        |
| pageSize | Integer | X        | Number of queries (Default : 15) |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/tags' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "pageNum": 1,
        "pageSize": 1,
        "totalCount": 1,
        "data": [
            {
                "tagId": "ABCD1234",
                "tagName": "TAG",
                "createdDate": "2019-01-01 00:00:00",
                "updatedDate": "2019-01-01 00:00:00"
            }
        ]
    }
}
```

| Value           | Type    | Not Null|	Description                   |
| --------------- | ------- | ------- |----------------------------- |
| header          | Object  | O|	Header area                   |
| - isSuccessful  | Boolean | O|	Successful or not             |
| - resultCode    | Integer | O|	Failure code                  |
| - resultMessage | String  | O|	Failure message               |
| body            | Object  | X|	Body area                     |
| - data          | List    | X|	Data area                     |
| -- tagId        | String  | O|	Tag ID                        |
| -- tagName      | String  | O|	Tag name                      |
| -- createdDate  | String  | O|	Date and time of creation     |
| -- updatedDate  | String  | O|	Date and time of modification |

### Register Tags

#### Request

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.1/appKeys/{appKey}/tags|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Request Body]

|Value| Type | Required | Description |
|---|---|---|---|
|tagName|	String|	O| Tag name |

#### cURL
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/tags' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "tagName": "API-Guide-샘플-태그"
}'
```

#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "data": {
            "tagId": "ABCD1234"
        }
    }
}
```

| Value           | Type    | Not Null|	Description       |
| --------------- | ------- | ------- |----------------- |
| header          | Object  | O|	Header area       |
| - isSuccessful  | Boolean | O|	Successful or not |
| - resultCode    | Integer | O|	Failure code      |
| - resultMessage | String  | O|	Failure message   |
| body            | Object  | X|	Body area         |
| - data          | List    | O|	Data area         |
| -- tagId        | String  | O|	Tag ID            |

##### Description
- You can create up to 2,048 tags.

##### Adding a UID to a Tag
- This method adds a UID to a tag. If you add an existing UID, the tag of the UID will increase.
- Up to 16 tags can be added to a single UID.


### Modify Tags

#### Request

[URL]

|Http method|	URI|
|---|---|
|PUT|	/email/v2.1/appKeys/{appKey}/tags/{tagId}|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|tagId|	String| Tag ID |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Request Body]

|Value| Type | Required | Description |
|---|---|---|---|
|tagName|	String|	O| Tag name |

#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/tags/'"${TAG_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "tagName": "API-Guide-샘플-태그2"
}'
```


#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": null
}
```

| Value           | Type    | Not Null|	Description       |
| --------------- | ------- | ------- |----------------- |
| header          | Object  | O|	Header area       |
| - isSuccessful  | Boolean | O|	Successful or not |
| - resultCode    | Integer | O|	Failure code      |
| - resultMessage | String  | O|	Failure message   |

### Delete Tags	

#### Request 

[URL]

|Http method|	URI|
|---|---|
|DELETE|	/email/v2.1/appKeys/{appKey}/tags/{tagId}|

[Path Parameter]

|Value| Type | Desription |
|---|---|---|
|appKey|	String| Original appKey |
|tagId|	String| Tag ID |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

#### cURL
```
curl -X DELETE \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/tags/'"${TAG_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": null
}
```

| Value           | Type    | Not Null|	Description       |
| --------------- | ------- | ------- |----------------- |
| header          | Object  | O|	Header area       |
| - isSuccessful  | Boolean | O|	Successful or not |
| - resultCode    | Integer | O|	Failure code      |
| - resultMessage | String  | O|	Failure message   |

## UID Management

### Query UIDs

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.1/appKeys/{appKey}/uids|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Query Parameter]

| Value     | Type        | Required | Description                                                  |
| --------- | ----------- | -------- | ------------------------------------------------------------ |
| wheres    | List:String | X        | Query conditions.</br> Character strings comprised of alphabets, numbers, and parentheses. </br>Allows one parenthesis, and up to three AND or ORs.</br> e.g.) tagId1,AND,tagId2 |
| offsetUid | String      | X        | offset UID                                                   |
| offset    | Integer     | X        | offset (default: 0)                                          |
| limit     | Integer     | X        | Number of queries (default: 15)                              |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/uids' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "data": {
            "uids": [
                {
                    "uid": "UID",
                    "tags": [
                        {
                            "tagId": "ABCD1234",
                            "tagName": "TAG",
                            "createdDate": "2019-01-01 00:00:00",
                            "updatedDate": "2019-01-01 00:00:00"
                        }
                    ],
                    "contacts": [
                        {
                            "contactType": "EMAIL_ADDRESS",
                            "contact": "test@example.com",
                            "createdDate": "2019-01-01 00:00:00"
                        }
                    ]
                }
            ],
            "isLast": false,
            "totalCount": 5
        }
    }
}
```

| Value            | Type    | Not Null|	Description                       |
| ---------------- | ------- | ------- |--------------------------------- |
| header           | Object  | O|	Header area                       |
| - isSuccessful   | Boolean | O|	Successful or not                 |
| - resultCode     | Integer | O|	Failure code                      |
| - resultMessage  | String  | O|	Failure message                   |
| body             | Object  | X|	Body area                         |
| - data           | List    | X|	Data area                         |
| -- uids          | List    | O|	List of UIDs                      |
| --- uid          | String  | O|	UID                               |
| --- tags         | List    | O|	List of tag information           |
| ---- tagId       | String  | O|	Tag ID                            |
| ---- tagName     | String  | O|	Tag name                          |
| ---- createDate  | String  | O|	Date and time of tag creation     |
| ---- updateDate  | String  | O|	Date and time of tag modification |
| --- contacts     | List    | O|	List of contacts                  |
| ---- contactType | String  | O|	Contact type (EMAIL_ADDRESS)      |
| ---- contact     | String  | O|	Contact (mail address)            |
| ---- createDate  | String  | O|	Date and time of contact creation |
| -- isLast        | Boolean | O|	Last on list or not               |
| -- totalCount    | Integer | O|	Total number of data              |

### Get UIDs

#### Request 

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.1/appKeys/{appKey}/uids/{uid}|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|uid|	String|	UID |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "data": {
            "uid": "UID",
            "tags": [
                {
                    "tagId": "ABCD1234",
                    "tagName": "TAG",
                    "createdDate": "2019-01-01 00:00:00",
                    "updatedDate": "2019-01-01 00:00:00"
                }
            ],
            "contacts": [
                {
                    "contactType": "EMAIL_ADDRESS",
                    "contact": "test@example.com",
                    "createdDate": "2019-01-01 00:00:00"
                }
            ]
        }
    }
}
```

| Value           | Type    | Not Null|	Description                       |
| --------------- | ------- | ------- |--------------------------------- |
| header          | Object  | O|	Header are                        |
| - isSuccessful  | Boolean | O|	Successful or not                 |
| - resultCode    | Integer | O|	Failure code                      |
| - resultMessage | String  | O|	Failure message                   |
| body            | Object  | X|	Body area                         |
| - data          | List    | X|	Data area                         |
| -- uid          | String  | O|	UID                               |
| -- tags         | List    | O|	List of tag information           |
| --- tagId       | String  | O|	Tag ID                            |
| --- tagName     | String  | O|	Tag name                          |
| --- createDate  | String  | O|	Date and time of tag creation     |
| --- updateDate  | String  | O|	Date and time of tag modification |
| -- contacts     | List    | O|	List of contacts                  |
| --- contactType | String  | O|	Contact type                      |
| --- contact     | String  | O|	Contact (mail address)            |
| --- createDate  | String  | O|	Date and time of contact creation |

### Register UIDs

#### Request

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.1/appKeys/{appKey}/uids|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Request Body]

| Value        | Type        | Require | Description            |
| ------------ | ----------- | ------- | ---------------------- |
| uid          | String      | O       | UID                    |
| tagIds       | List:String | O       | List of tag IDs        |
| contacts     | List        | O       | List of mail addresses |
| -contactType | String      | O       | Contact type           |
| -contact     | String      | O       | Contact (mail address) |

[Caution]

- When tagIds is provided, contacts is not required.
- When contacts is provided, tagIds is not required.
- For this product, contactType must be requested in the "EMAIL_ADDRESS" value.

#### cURL
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/uids' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "uids": [{
            "uid": "sample-uid",
            "tagIds": [],
            "contacts": [{
                    "contactType": "EMAIL_ADDRESS",
                    "contact": "customer1@example.com"
                }
            ]
        }
    ]
}'
```

#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": null
}
```

| Value           | Type    | Not Null|	Description       |
| --------------- | ------- | ------- |----------------- |
| header          | Object  | O|	Header area       |
| - isSuccessful  | Boolean | O|	Successful or not |
| - resultCode    | Integer | O|	Failure code      |
| - resultMessage | String  | O|	Failure message   |

### Delete UIDs

#### Request

[URL]

|Http method|	URI|
|---|---|
|DELETE|	/email/v2.1/appKeys/{appKey}/uids/{uid}|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|uid|	String|	UID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

#### cURL
```
curl -X DELETE \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": null
}
```

| Value           | Type    | Not Null|	Description       |
| --------------- | ------- | ------- |----------------- |
| header          | Object  | O|	Header area       |
| - isSuccessful  | Boolean | O|	Successful or not |
| - resultCode    | Integer | O|	Failure code      |
| - resultMessage | String  | O|	Failure message   |

### Register Mail Addresses

#### Request

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.1/appKeys/{appKey}/uids/{uid}/email-addresses|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|uid|	String|	UID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Request Body]

|Value| Type | Required | Description |
|---|---|---|---|
|emailAddress|	String|	O| Mail address |

#### cURL
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}"'/email-addresses' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "emailAddress": "customer1@example.com"
}'
```

#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": null
}
```

| Value           | Type    | Not Null|	Description       |
| --------------- | ------- | ------- |----------------- |
| header          | Object  | O|	Header area       |
| - isSuccessful  | Boolean | O|	Successful or not |
| - resultCode    | Integer | O|	Failure code      |
| - resultMessage | String  | O|	Failure message   |

### Delete Mail Addresses

#### Request

[URL]

|Http method|	URI|
|---|---|
|DELETE|	/email/v2.1/appKeys/{appKey}/uids/{uid}/email-addresses/{emailAddress}|

[Path Parameter]

| Value        | Type   | Description     |
| ------------ | ------ | --------------- |
| appKey       | String | Original appKey |
| uid          | String | UID             |
| emailAddress | String | Mail address    |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

#### cURL
```
curl -X DELETE \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}"'/email-addresses/'"${EMAIL_ADDR}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": null
}
```

| Value           | Type    | Not Null|	Description       |
| --------------- | ------- | ------- |----------------- |
| header          | Object  | O|	Header area       |
| - isSuccessful  | Boolean | O|	Successful or not |
| - resultCode    | Integer | O|	Failure code      |
| - resultMessage | String  | O|	Failure message   |

## Query Statistics	

### Query Daily Statistics

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.1/appKeys/{appKey}/statistics/view |

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Query Parameter]

|Value| Type | Required | Description |
|---|---|---|---|
|from|	String|	O | Start date of statistical query<br/> yyyy-mm-dd HH:mm |
|to|	String|	O | End date of statistical query<br/> yyyy-mm-dd HH:mm |
|searchType| String | O | Statistics Type<br/>DATE: By date, TIME: By time, DAY: By day |
|mailTypes | String | X | Mail Type<br/>NORMAL: General, MASS: Mass<br/>Multiple inputs are available (mailTypes=NORMAL&mailTypes=MASS) |
|adYn | String | X | Ad or not<br>Y: Ad, N: Not an ad<br>All, if left blank |
|templateId | String | X | Template ID |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/statistics/view?from='"${FROM}"'&to='"${TO}"'&searchType='"${SEARCH_TYPE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

#### Response

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "data": [
            {
                "divisionName": "Tue",
                "requestedCount": 1,
                "sentCount": 1,
                "receivedCount": 1,
                "openedCount": 0,
                "sentRate": "100.00",
                "receivedRate": "100.00",
                "openedRate": "0.00"
            }
        ]
    }
}
```

|Value| Type | Not Null|	Description |
|---|---|---|---|
|header|	Object| O|	Header area |
|- isSuccessful|	Boolean| O|	Successful or not |
|- resultCode|	Integer| O|	Failure code |
|- resultMessage|	String| O|	Failure message |
|body|	Object| X|	Body area |
|- data|	List| O|	Data area |
|-- divisionName | String | O|	Statistical criteria (date/time/day) |
|-- requestedCount | Long | O|	Number of delivery requests |
|-- sentCount | Long | O|	Number of deliveries |
|-- receivedCount | Long | O|	Number of receipts |
|-- openedCount | Long | O|	Number of openings |
|-- sentRate | String | O|	Delivery rate |
|-- receivedRate | String | O|	Receiving rate |
|-- openedRate | String | O|	Opening rate |

## Rejection Management

### Query Rejections

#### Request

[URL]

|Http method|	URI|
|---|---|
| GET |	/email/v2.1/appKeys/{appKey}/block-receivers |

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Query Parameter]

| Value       | Type    | Required | Description                                        |
| ----------- | ------- | -------- | -------------------------------------------------- |
| mailAddress | String  | X        | Email address registered on the list of rejections |
|startBlockDate|	String|	X| Start date of rejection (yyyy-MM-dd HH:mm:ss)|
|endBlockDate|	String|	X| End date of rejection (yyyy-MM-dd HH:mm:ss)|
| pageNum     | Integer | X        | Page number (default: 1)                           |
| pageSize    | Integer | X        | Number of queries (default: 15)                    |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/block-receivers' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

#### Response
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "totalCount": 1,
        "data": [
            {
                "mailAddress": "block@nhn.com",
                "blockDate": "2019-01-01 00:00:00.0"
            }
        ]
    }
}
```

| Value           | Type    | Not Null|	Description                               |
| --------------- | ------- | ------- |----------------------------------------- |
| header          | Object  | O|	Header area                               |
| - isSuccessful  | Boolean | O|	Successful or not                         |
| - resultCode    | Integer | O|	Failure code                              |
| - resultMessage | String  | O|	Failure message                           |
| body            | Object  | X|	Body area                                 |
| - pageNum       | Integer | O|	Current page number                       |
| -pageSize       | Integer | O|	Number of queried data                    |
| - totalCount    | Integer | O|	Total number of data                      |
| - data          | List    | O|	Data area                                 |
| -- mailAddress  | String  | O|	Email address to reject                   |
| -- blockDate    | String  | O|	Date of rejection (yyyy-MM-dd HH:mm:ss.S) |

### Register Rejections

#### Request

[URL]

|Http method|	URI|
|---|---|
| POST |	/email/v2.1/appKeys/{appKey}/block-receivers |

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Request Body]

| Value             | Type   | Required | Description                             |
| ----------------- | ------ | -------- | --------------------------------------- |
| blockReceiverList | ㅣList | O        | List of rejections                      |
| - mailAddress     | String | O        | Email address to reject                 |
| - blockDate       | String | X        | Date of rejection (yyyy-MM-dd HH:mm:ss) |

#### cURL
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/block-receivers' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "blockReceiverList": [{
            "mailAddress": "customer1@example.com",
            "blockDate": "2018-03-01 00:00:00"
        }
    ]
}'
```

#### Response
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": null
}
```

| Value           | Type    | Not Null|	Description       |
| --------------- | ------- | ------- |----------------- |
| header          | Object  | O|	Header area       |
| - isSuccessful  | Boolean | O|	Successful or not |
| - resultCode    | Integer | O|	Failure code      |
| - resultMessage | String  | O|	Failure message   |

### Delete Rejections

#### Request

[URL]

|Http method|	URI|
|---|---|
| PUT |	/email/v2.1/appKeys/{appKey}/block-receivers |

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value| Type | Required | Description |
|---|---|---|---|
|X-Secret-Key|	String| O | Original secretKey [[Note](./api-guide/#secret-key)] |

[Request Body]

| Value             | Type    | Required | Description                             |
| ----------------- | ------- | -------- | --------------------------------------- |
| deleted           | Boolean | O        | Field specifying rejections are deleted |
| blockReceiverList | ㅣList  | O        | List of rejections                      |
| - mailAddress     | String  | O        | Email address rejecting ads             |

#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.1/appKeys/'"${APP_KEY}"'/block-receivers' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "deleted": true,
    "blockReceiverList": [{
            "mailAddress": "customer1@example.com"
        }
    ]
}'
```

#### Response
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": null
}
```

| Value           | Type    | Not Null|	Description       |
| --------------- | ------- | ------- |----------------- |
| header          | Object  | O|	Header area       |
| - isSuccessful  | Boolean | O|	Successful or not |
| - resultCode    | Integer | O|	Failure code      |
| - resultMessage | String  | O|	Failure message   |

## Statistics
### Query Statistics
#### Request

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v2.1/appKeys/{appKey}/stats|

[Path parameter]

|Value|	Type|	Descriptions|
|---|---|---|
|appKey|	String|	Unique appkey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value|	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | Unique secretKey [[note](./api-guide/#secret-key)] |


[Query parameter]

| Value              | 	Type          | 	Required | 	Descriptions                                                                                              |
|----------------|--------------|-----|--------------------------------------------------------------------------------------------------|
| eventCategory  | String       | O   | Event category. MESSAGE                                                                               |
| statisticsType | String       | X   | Descrption format of queried statistical data. NORMAL(default), MINUTELY, HOURLY, DAILY, BY_DAY                                  |
| timeUnit       | String       | X   | Time unit of statistical data. Defaults are determined according to the query period, MINUTES, HOURS, DAYS                                           |
| from           | String       | X   | Start value of query range, up to last 30 days (ISO 8601, e.g. YYYY-MM-DDThh:mm:ss.SSSTZD, 2018-04-24T06:00:00.000%2B09:00) |
| to             | String       | X   | End value of query range, up to last 30 days (ISO 8601, e.g. YYYY-MM-DDThh:mm:ss.SSSTZD, 2018-04-24T06:00:00.000%2B09:00) |
| extra1s        | Array String | X   | If eventCategory is MESSAGE, it can be filtered by mail type. NORMAL, MASS, TAG, SMTP                               |
| extra2s        | Array String | X   | If eventCategory is MESSAGE, it can be filtered by the sending domain                                                      |
| extra3s        | Array String | X   | If eventCategory is MESSAGE, it can be filtered by the receiving domain                                                      |
| messageId      | String       | X   | Message ID                                                                                          |
| statsIds       | Array String | X   | Statistics event key ID                                                                                     |
| templateIds    | Array String | X   | Template ID                                                                                           |
| statsCriteria  | Array String | X   | Statistical criteria for sum, if not set, calculates sum as default. EVENT(default), EXTRA_1, EXTRA_2, EXTRA_3, TEMPLATE_ID            |

##### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appkeys/'"${APP_KEY}"'/stats?eventCategory='"${EVENT_CATEGORY}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

#### Response
```json
{
	"header": {
		"resultCode": 0,
		"resultMessage": "success",
		"isSuccessful": true
	},
	"stats": [{
		"eventDateTime": "2020-08-12T00:00:00.000+09:00",
		"events": {
			"{statsCriteriaValue}.RECEIVED": 0,
			"{statsCriteriaValue}.SENT_FAILED": 0,
			"{statsCriteriaValue}.SENT": 0,
			"{statsCriteriaValue}.OPENED": 0
		}
	}]
}
```

| Value                       | 	Type      | 	Not Null|	Descriptions                                                      |
|-------------------------|----------|----------------------------------------------------------|----------|
| header                  | 	Object  | 		O|Header area                                                   |
| - isSuccessful          | 	Boolean | 		O|Successful or not                                                   |
| - resultCode            | 	Integer | 		O|Failure code                                                   |
| - resultMessage         | 	String  | 		O|Failure message                                                  |
| stats                   | 	Object  | 		X|Body area                                                   |
| - eventDateTime         | 	String  | 		X|Event occurrence time                                               |
| - events                | 	List    | 		X|Scope of data                                                  |
| -- {statsCriteriaValue} | 	String  | 		X|{statsCriteriaValue} is the value of statsCriteria entered as a query condition. |
| --- REQUESTED           | 	Integer | 	X|The number of requested cases                                                    |
| --- SENT                | 	Integer | 		X|The number of sent cases                                                   |
| --- SENT_FAILED         | 	Integer | 		X|The number of failure cases                                                   |
| --- RECEIVED            | 	Integer | 		X|The number of success cases                                                   |
| --- OPENED              | 	Integer | 		X|The number of opened cases                                                   |

### Query Statistical Totals
#### Request

[URL]

| Http method | 	URI                                     |
|-------------|------------------------------------------|
| GET         | /email/v2.1/appKeys/{appKey}/stats/total |

[Path parameter]

|Value|	Type|	Descriptions|
|---|---|---|
|appKey|	String|	Unique appkey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value|	Type|	Required|	Descriptions|
|---|---|---|---|
|X-Secret-Key|	String| O | Unique secretKey [[note](./api-guide/#secret-key)] |


[Query parameter]

| Value              | 	Type          | 	Required | 	Descriptions                                                                                              |
|----------------|--------------|-----|--------------------------------------------------------------------------------------------------|
| eventCategory  | String       | O   | Event category. MESSAGE                                                                               |
| statisticsType | String       | X   | Descrption format of queried statistical data. NORMAL(default), MINUTELY, HOURLY, DAILY, BY_DAY                                  |
| timeUnit       | String       | X   | Time unit of statistical data. Defaults are determined according to the query period, MINUTES, HOURS, DAYS                                           |
| from           | String       | X   | Start value of query range, up to last 30 days (ISO 8601, e.g. YYYY-MM-DDThh:mm:ss.SSSTZD, 2018-04-24T06:00:00.000%2B09:00) |
| to             | String       | X   | End value of query range, up to last 30 days (ISO 8601, e.g. YYYY-MM-DDThh:mm:ss.SSSTZD, 2018-04-24T06:00:00.000%2B09:00) |
| extra1s        | Array String | X   | If eventCategory is MESSAGE, it can be filtered by mail type. NORMAL, MASS, TAG, SMTP                               |
| extra2s        | Array String | X   | If eventCategory is MESSAGE, it can be filtered by the sending domain                                                      |
| extra3s        | Array String | X   | If eventCategory is MESSAGE, it can be filtered by the receiving domain                                                      |
| messageId      | String       | X   | Message ID                                                                                          |
| statsIds       | Array String | X   | Statistics event key ID                                                                                     |
| templateIds    | Array String | X   | Template ID                                                                                           |
| statsCriteria  | Array String | X   | Statistical criteria for sum, if not set, calculates sum as default. EVENT(default), EXTRA_1, EXTRA_2, EXTRA_3, TEMPLATE_ID            |

##### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.1/appkeys/'"${APP_KEY}"'/stats?eventCategory='"${EVENT_CATEGORY}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

#### Response
```json
{
	"header": {
		"resultCode": 0,
		"resultMessage": "success",
		"isSuccessful": true
	},
	"total" : {
		"SENT" : 120,
		"SENT_FAILED" : 50,
		"RECEIVED": 0,
		"OPENED": 0
	}
}
```

| Value                                   | 	Type      | 	Not Null|Descriptions        |
|-------------------------------------|----------|------------|------------|
| header                              | 	Object  | 		O|Header area     |
| - isSuccessful                      | 	Boolean | 		O|Successful or not     |
| - resultCode                        | 	Integer | 		O|Failure code     |
| - resultMessage                     | 	String  | 		O|Failure message    |
| total                               | 	Object  | 		X|Body area     |
| - {statsCriteriaValue}.REQUESTED   | 	Integer | 	X|The number of requested cases      |
| - {statsCriteriaValue}.SENT        | 	Integer | 		X|The number of sent cases     |
| - {statsCriteriaValue}.SENT_FAILED | 	Integer | 		X|The number of failure cases     |
| - {statsCriteriaValue}.RECEIVED    | 	Integer | 		X|The number of success cases     |
| - {statsCriteriaValue}.OPENED      | 	Integer | 		X|The number of opened cases     |
