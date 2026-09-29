<!-- pre-align:aligned sig=6dbb01fc078a -->

<a id="notification-email-api-v20-guide"></a>
## Notification > Email > API v2.0 Guide { #notification-email-api-v20-guide }

<a id="v20-api"></a>
### v2.0 API 소개 { #v20-api }

1. 시크릿 키 인증 도입
	* v2.0 API 호출 시 헤더에 [시크릿 키](./api-guide/#secret-key)를 설정해서 호출해야 합니다.
2. 대량 발송 조회 API 추가
	* 대량 발송건에 대한 조회 API가 추가되었습니다.


[API Domain]

|Environment| Domain |
|---|---|
|Real|	https://email.api.nhncloudservice.com |

[Header]
```
Content-Type: application/json;charset=UTF-8
```

<p id="secret-key"></p>

<a id="secret-key"></a>
### Secret Key { #secret-key }
- 콘솔에서 확인 가능합니다.
- Secret Key가 필요한 API를 호출할 때, 헤더에 아래와 같이 설정해서 호출해야 합니다.
```
Header
X-Secret-Key: [a-zA-Z0-9]{8}
```
**CONSOLE > Notification > Email > URL & AppKey** 에서 확인/생성할 수 있습니다.


[Caution for Curl Example]

- Curl Example may not be properly requested in Windows cmd.

<a id="mail-delivery"></a>
## Mail Delivery { #mail-delivery }

<a id="send-general-mails"></a>
### Send General Mails { #send-general-mails }

<a id="send-general-mails-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/sender/mail|

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

| Value             | Type        | Required | Description                                                  |
| ----------------- | ----------- | -------- | ------------------------------------------------------------ |
| senderAddress     | String      | O        | Sender's mail address                                        |
| senderName        | String      | X        | Sender's name                                                |
| requestDate       | String      | X        | Enter current time when delivery time is missing (yyyy-MM-dd HH:mm:ss) |
| title             | String      | O        | Title                                                        |
| body              | String      | O        | Body                                                         |
| attachFileIdList  | List:String | X        | ID of uploaded attachment                                    |
| templateId        | String      | X        | ID of delivery template                                      |
| templateType | String | X | Template type <br/>DEFAULT (default), FREEMARKER) |
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

[Example 1]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/mail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "senderAddress": "support@example.com",
    "senderName": "발송자이름",
    "title": "샘플 타이틀",
    "body": "샘플 내용",
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "고객1",
            "receiveType": "MRT0"
        }, {
            "receiveMailAddr": "customer2@example.com",
            "receiveName": "고객2",
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
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/mail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "templateId": "TEMPLATE1",
    "templateParameter": {
        "key": "value"
    },
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "고객1",
            "receiveType": "MRT0"
        }, {
            "receiveMailAddr": "customer2@example.com",
            "receiveName": "고객2",
            "receiveType": "MRT1"
        }
    ],
    "userId": "USER",
    "statsId": "statsId"
}'
```

<a id="send-general-mails-response"></a>
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

| Value               | Type    | Description                                             |
| ------------------- | ------- | ------------------------------------------------------- |
| header              | Object  | Header area                                             |
| - isSuccessful      | Boolean | Successful or not                                       |
| - resultCode        | Integer | Failure code                                            |
| - resultMessage     | String  | Failure message                                         |
| body                | Object  | Body area                                               |
| - data              | Object  | Data area                                               |
| -- requestId        | String  | Request ID                                              |
| -- results          | List    | Delivery result                                         |
| --- receiveMailAddr | String  | Recipient's mail address                                |
| --- receiveName     | String  | Recipient's name                                        |
| --- receiveType     | String  | Recipient type (MRT0: recipients , MRT1: Cc, MRT2: Bcc) |
| --- resultCode      | Integer | Result code of recipient delivery request               |
| --- resultMessage   | String  | Result message of recipient delivery request            |


<a id="send-individual-mails"></a>
### Send Individual Mails { #send-individual-mails }

* Mails can be sent to each of many recipients. Even if a same mail is sent to many recipients, each recipient can find his or her name only.  

<a id="send-individual-mails-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/sender/eachMail|

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
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/eachMail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "senderAddress": "support@example.com",
    "senderName": "발송자이름",
    "title": "샘플 타이틀",
    "body": "샘플 내용",
    "attachFileIdList": [1, 2],
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "고객1"
        }
    ],
    "userId": "USER",
    "statsId": "statsId"
}'
```

[Example 2 - Using Templates]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/eachMail' \
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


<a id="send-individual-mails-response"></a>
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

| Value               | Type    | Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |
| header              | Object  | Header area                                                  |
| - isSuccessful      | Boolean | Successful or not                                            |
| - resultCode        | Integer | Failure code                                                 |
| - resultMessage     | String  | Failure message                                              |
| body                | Object  | Body area                                                    |
| - data              | Object  | Data area                                                    |
| -- requestId        | String  | Request ID                                                   |
| -- results          | List    | Delivery result                                              |
| --- receiveMailAddr | String  | Recipient's mail address                                     |
| --- receiveName     | String  | Recipient's name                                             |
| --- receiveType     | String  | Recipient type (MRT0: recipients , MRT1: Cc, MRT2: Bcc)|
| --- resultCode      | Integer | Result code of recipient delivery request                    |
| --- resultMessage   | String  | Result message of recipient delivery request                 |


<a id="sending-general-ad-mails"></a>
### Sending General Ad Mails { #sending-general-ad-mails }

* Request and response information is same as in sending general mails. 

<a id="sending-general-ad-mails-caution-for-sending-ad-mails"></a>
#### Caution for Sending Ad Mails

- The title must include the (AD) phrase. 
- For more details, see [Send Advertising Mails](./console-guide/#_3).

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/sender/ad-mail|

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
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/ad-mail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "senderAddress": "support@example.com",
    "senderName": "발송자이름",
    "title": "(광고) 샘플 타이틀",
    "body": "샘플 내용 <br /> ##BLOCK_RECEIVER_LINK## <br />##EN_BLOCK_RECEIVER_LINK##",
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "고객1",
            "receiveType": "MRT0"
        }, {
            "receiveMailAddr": "customer2@example.com",
            "receiveName": "고객2",
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
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/ad-mail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "templateId": "TEMPLATE1",
    "templateParameter": {
        "key": "value"
    },
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "고객1",
            "receiveType": "MRT0"
        }, {
            "receiveMailAddr": "customer2@example.com",
            "receiveName": "고객2",
            "receiveType": "MRT1"
        }
    ],
    "userId": "USER",
    "statsId": "statsId"
}'
```

<a id="sending-individual-ad-mails"></a>
### Sending Individual Ad Mails { #sending-individual-ad-mails }

* Request and response information is same as in sending individual mails.  

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/sender/ad-eachMail |

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
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/ad-eachMail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "senderAddress": "support@example.com",
    "senderName": "발송자이름",
    "title": "(광고) 샘플 타이틀",
    "body": "샘플 내용 <br /> ##BLOCK_RECEIVER_LINK## <br />##EN_BLOCK_RECEIVER_LINK##",
    "attachFileIdList": [1, 2],
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "고객1"
        }
    ],
    "userId": "USER",
    "statsId": "statsId"
}'
```

[Example 2 - Using Templates]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/ad-eachMail' \
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

<a id="send-authenticated-mails"></a>
### Send Authenticated Mails { #send-authenticated-mails }

<a id="send-authenticated-mails-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/sender/auth-mail|

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
|senderGroupingKey| String| X| 발신자 그룹키 ( 최대 100자 )|
| userId              | String | X        | Delimiter for delivery e.g.) admin,system                    |
| statsId             | String | X        | Statistics ID (not included in the delivery search conditions)|

[Caution]

* On a template, **senderAddress, title, and body** are not required. These values, if left empty, can be replaced by registered values on template. 
* On a template, **senderAddress, senderName, title, body, and templateType**, if available, are applied before template-registered values.

<a id="send-authenticated-mails-differences-from-general-mails"></a>
#### Differences from General Mails 

Features of authenticated mails are as follows: 

- Allows only single-case deliveries (one recipient).
- File attachment is not supported. Templates including attachment are not supported. 

[Example 1]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/auth-mail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "senderAddress": "support@example.com",
    "senderName": "발송자이름",
    "title": "샘플 타이틀",
    "body": "샘플 내용",
    "receiver": {
        "receiveMailAddr": "customer1@example.com",
        "receiveName": "고객1"
    },
    "userId": "USER",
    "statsId": "statsId"
}'
```

[Example 2 - Using Templates]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/auth-mail' \
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

<a id="send-authenticated-mails-response"></a>
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

| Value               | Type    | Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |
| header              | Object  | Header area                                                  |
| - isSuccessful      | Boolean | Successful or not                                            |
| - resultCode        | Integer | Failure code                                                 |
| - resultMessage     | String  | Failure message                                              |
| body                | Object  | Body area                                                    |
| - data              | Object  | Data area                                                    |
| -- requestId        | String  | Request ID                                                   |
| -- results          | List    | Delivery result                                              |
| --- receiveMailAddr | String  | Recipient's mail address                                     |
| --- receiveName     | String  | Recipient's name                                             |
| --- receiveType     | String  | Recipient type (MRT0: recipients , MRT1: Cc, MRT2: Bcc) |
| --- resultCode      | Integer | Result code of recipient delivery request                    |
| --- resultMessage   | String  | Result message of recipient delivery request                 |
<a id="upload-attached-files"></a>
### Upload Attached Files { #upload-attached-files }

<a id="upload-attached-files-request"></a>
#### Request 

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/attachfile/binaryUpload|

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

<a id="upload-attached-files-curl"></a>
#### cURL
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/attachfile/binaryUpload' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "fileName": "file.csv",
    "createUser": "USER",
    "fileBody": []
}'
```

<a id="upload-attached-files-response"></a>
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

| Value           | Type    | Description       |
| --------------- | ------- | ----------------- |
| header          | Object  | Header area       |
| - isSuccessful  | Boolean | Successful or not |
| - resultCode    | Integer | Failure code      |
| - resultMessage | String  | Failure message   |
| body            | Object  | Body area         |
| - data          | Object  | Data area         |
| -- fileId       | Integer  | File ID           |
| -- fileName     | String  | File name         |

<a id="titlebody-replacement"></a>
### Title/Body Replacement { #titlebody-replacement }

<a id="titlebody-replacement-default-type"></a>
#### Default Type
* Enter in the (##Replacement Key##) format to replace with user-defined **templateParameter**.
```
* title: Hello, ##title_name## !!
* body: We send ##body_content##.
->
* title: Hello, clouc customer1 !!
* body: We send test2.
```

<a id="titlebody-replacement-freemarker-type"></a>
#### FreeMarker Type
* [FreeMarker Template Engine](https://freemarker.apache.org/) is supported.
* You may use a template language to replace with user-defined **templateParameter**. 
```
* title: Hello, ${title_name}!!
* body: We send ${body_content}.
->
* title: Hello, cloud customer1!!
* body: We send test2.
```

<a id="titlebody-replacement-example-of-general-mail-request"></a>
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

<a id="titlebody-replacement-example-of-individual-mail-request"></a>
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

<a id="query-of-mails"></a>
## Query of Mails { #query-of-mails }

<a id="query-list-of-mail-deliveries"></a>
### Query List of Mail Deliveries { #query-list-of-mail-deliveries }

<a id="query-list-of-mail-deliveries-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/sender/mails|

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

<a id="query-list-of-mail-deliveries-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/mails?startSendDate='"${START_DATE}"'&endSendDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

<a id="query-list-of-mail-deliveries-response"></a>
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
                "dsnMessage": "SUCCESS"
            }
        ]
    }
}
```

| Value               | Type    | Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |
| header              | Object  | Header area                                                  |
| - isSuccessful      | Boolean | Successful or not                                            |
| - resultCode        | Integer | Failure code                                                 |
| - resultMessage     | String  | Failure message                                              |
| body                | Object  | Body area                                                    |
| - pageNum           | Integer | Current page number                                          |
| - pageSize          | Integer | Number of queried data                                       |
| - totalCount        | Integer | Number of total data                                         |
| - data              | List    | Data area                                                    |
| -- requestId        | String  | Request ID                                                   |
| -- mailSeq          | String  | Mail sequence                                                |
| -- requestDate      | String  | Date and time of sending                                     |
| -- templateId       | String  | Template ID                                                  |
| -- templateName     | String  | Template name                                                |
| -- senderName       | String  | Sender's name                                                |
| -- senderAddress    | String  | Sender's mail address                                        |
| -- title            | String  | Title                                                        |
| -- mailStatusCode   | String  | Delivery status code   SST0: Preparing Delivery, SST1: Delivering,    SST2: Delivery Completed, SST3: Delivery Failed, SST7: Not Authenticated |
| -- mailStatusName   | String  | Name of delivery status                                      |
| -- isReceived       | Boolean | Received or Not                                              |
| -- resultDate       | String  | Date and Time of Receiving                                   |
| -- isOpened         | Boolean | Opened or Not                                                |
| -- openedDate       | String  | Date and Time of Opening                                     |
| -- receiveMailAddr  | String  | Recipient's mail address                                     |
| -- receiveType      | String  | Recipient type  (MRT0: recipient, MRT1: cc, MRT2: bcc)       |
| -- receiveTypeName  | String  | Name of recipient type                                       |
| -- receiveName      | String  | Name of recipient                                            |
| -- senderGroupingKey| String  | Sender's group key                                           |
| -- dsnCode| String| DSN(Delivery Status Notification) Status Code |
| -- dsnMessage| String| DSN(Delivery Status Notification) Status Message |


<a id="query-mail-delivery-details"></a>
### Query Mail Delivery Details { #query-mail-delivery-details }

<a id="query-mail-delivery-details-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/sender/mail/{requestId}/{mailSeq}|

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

<a id="query-mail-delivery-details-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/mail/'"${REQUEST_ID}"'/'"${MAIL_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

<a id="query-mail-delivery-details-response"></a>
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
            "senderGroupingKey": "A_GROUP"
        }
    }
}
```

| Value               | Type    | Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |
| header              | Object  | Header area                                                  |
| - isSuccessful      | Boolean | Successful or not                                            |
| - resultCode        | Integer | Failure code                                                 |
| - resultMessage     | String  | Failure message                                              |
| body                | Object  | Body area                                                    |
| - data              | Object  | Data area                                                    |
| -- requestId        | String  | Request ID                                                   |
| -- mailSeq          | Integer | Mail sequence                                                |
| -- requestIp        | String  | Request IP                                                   |
| -- requestDate      | String  | Date and time of requesting                                  |
| -- mailStatusCode   | String  | Delivery status                                              |
| -- mailStatusName   | String  | Name of delivery status                                      |
| -- templateId       | String  | Template ID                                                  |
| -- templateName     | String  | Template name                                                |
| -- senderName       | String  | Sender's name                                                |
| -- senderAddress    | String  | Sender's mail address                                        |
| -- resultId         | String  | Mail delivery ID                                             |
| -- title            | String  | Mail title                                                   |
| -- body             | String  | Mail body                                                    |
| -- receiverList     | List    | List of recipients                                           |
| --- requestId       | String  | Request ID                                                   |
| --- mailSeq         | Integer | Mail sequence                                                |
| --- receiveType     | String  | Recipient type <br/>MRT0: to , MRT1: cc, MRT2: bcc            |
| --- receiveTypeName | String  | Name of recipient type                                       |
| --- receiveName     | String  | Recipient's name                                             |
| --- receiveMailAddr | String  | Recipient's mail address                                     |
| --- isReceived      | Boolean | Received or Not                                              |
| --- resultDate      | String  | Date and Time of Receiving                                   |
| --- isOpened        | Boolean | Opened or Not                                                |
| --- openedDate      | String  | Date and Time of Opening                                     |
| --- dsnCode| String| DSN(Delivery Status Notification) Status Code |
| --- dsnMessage| String| DSN(Delivery Status Notification) Status Message |
| -- attachFileList   | List    | List of attached files                                       |
| --- fileType        | String  | Type of attached files <br/>MAIL: files attached to mail, TEMPLATE: files attached to template|
| --- fileId          | Integer  | File ID                                                      |
| --- fileName        | String  | Name of attached file                                        |
| --- filePath        | String  | Path of attached file                                        |
| --- fileSize        | Integer | Size of attached file (byte)                                 |
| --- createDate      | String  | Date and time of creation                                    |
| -- customHeaders    | Map     | [Custom Header](./console-guide/#custom-header)                   |
| -- senderGroupingKey|	String  | Sender's group key                                           |


<a id="query-of-mails-1"></a>
### 대량 메일 리스트 조회 { #query-of-mails-1 }

<a id="query-of-mails-1-1"></a>
#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/mass-mails|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수|	설명|
|---|---|---|---|
|requestId|	String|	O|	요청 ID|
|startSendDate|	String|	O|	발송 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endSendDate|	String|	O|	발송 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|senderMail|	String|	X|	발신메일 주소|
|senderName|	String|	X|	발신자 이름|
|templateId|	String|	X|	템플릿 ID|
|sendStatus|	String|	X|	발송상태 코드 <br/> WAIT: 대기, READY: 발송준비, <br/>SENDREADY: 발송준비완료, SENDWAIT: 발송대기, <br/>SENDING: 발송중, COMPLETE: 발송완료, <br/>FAIL: 발송실패, CANCEL: 발송취소|
|pageNum|	Integer|	X|	페이지 번호 1(기본값)|
|pageSize|	Integer|	X|	조회 건수 15(기본값)|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

<a id="query-of-mails-1-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/mass-mails?startSendDate='"${START_DATE}"'&endSendDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

<a id="query-of-mails-1-2"></a>
#### 응답

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
            "sendStatusName":"발송완료",
            "templateId":"templateId",
            "templateName":"templateName",
            "senderName":"NHN Cloud",
            "senderAddress":"email@nhncloud.com",
            "title":"title",
            "body":"body",
            "adYn":"N",
            "createDate":"2021-08-06 18:43:03",
            "updateDate":"2021-08-06 18:43:03"
         }
      ]
   }
}
```

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- pageNum|	Integer|	현재 페이지 번호|
|- pageSize|	Integer|	조회된 데이터 건수|
|- totalCount|	Integer|	총 데이터 건수|
|- data|	Object|	데이터 영역|
|-- requestId| String| 요청 ID|
|-- requestDate| String| 요청 일시|
|-- sendStatus|	String|	발송상태 코드 <br/> WAIT: 대기, READY: 발송준비, <br/>SENDREADY: 발송준비완료, SENDWAIT: 발송대기, <br/>SENDING: 발송중, COMPLETE: 발송완료, <br/>FAIL: 발송실패, CANCEL: 발송취소|
|-- sendStatusName|	String|	발송 상태 명|
|-- templateId|	String|	템플릿 ID|
|-- templateName|	String|	템플릿 명|
|-- senderName|	String|	발신자 이름|
|-- senderAddress|	String|	발신자 메일주소|
|-- title|	String|	메일 제목|
|-- body|	String|	메일 내용|
|-- adYn |  String  | 광고여부 |
|-- createDate |  String  | 생성 일시 |
|-- updateDate |  String  | 수정 일시 |

<a id="query-of-mails-2"></a>
### 대량 메일 발송 수신자 조회 { #query-of-mails-2 }

<a id="query-of-mails-2-1"></a>
#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/mass-mails/{requestId}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|requestId|	String|	요청 ID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |


[Query parameter]

|값|	타입|	필수| 	설명                                                                         |
|---|---|---|-----------------------------------------------------------------------------|
|receiveMailAddr|	String|	X| 	수신 메일 주소                                                                   |
|startReceiveDate|	String|	X| 	수신 날짜 시작 값(yyyy-MM-dd HH:mm:ss)                                            |
|endReceiveDate|	String|	X| 	수신 날짜 종료 값(yyyy-MM-dd HH:mm:ss)                                            |
|mailStatusCode|	String|	X| 	발송상태 코드 <br/> SST0:발송준비, SST1:발송중,  <br/> SST2:발송완료, SST3: 발송실패, SST7: 미인증 |
|pageNum|	Integer|	X| 	페이지 번호 1(기본값)                                                              |
|pageSize|	Integer|	X| 	조회 건수 15(기본값)                                                              |

<a id="query-of-mails-2-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/mass-mails/'"${REQUEST_ID}"'?startSendDate='"${START_DATE}"'&endSendDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

<a id="query-of-mails-2-2"></a>
#### 응답

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
            "mailStatusName":"발송완료",
            "resultId":"<20210806120100sA3CcWU2@tcmmsapp-92a901>",
            "receiveType":"MRT0",
            "receiveTypeName":"받는사람",
            "receiveName":"name",
            "receiveMailAddr":"email@nhncloud.com",
            "isReceived":true,
            "resultDate":"2021-08-06 13:02:00",
            "isOpened":true,
            "openedDate":"2021-08-06 12:34:30",
            "dsnCode":"2.5.0",
            "dsnMessage":"SUCCESS",
            "createDate":"2021-08-06 12:01:41",
            "updateDate":"2021-08-06 12:02:00"
         }
      ]
   }
}
```

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- pageNum|	Integer|	현재 페이지 번호|
|-pageSize|	Integer|	조회된 데이터 건수|
|- totalCount|	Integer|	총 데이터 건수|
|- data|	List|	데이터 영역|
|-- requestId | String  | 요청 ID |
|-- mailSeq | Integer  | 메일 순번 |
|-- mailStatusCode | String  | 메일 상태 코드 <br/> SST0:발송준비, SST1:발송중,  <br/> SST2:발송완료, SST3: 발송실패, SST7: 미인증|
|-- mailStatusName | String  | 메일 상태명 |
|-- resultId | String  | SMTP ID |
|-- receiveType|	String|	수신자 타입<br/>MRT0 : 받는사람 , MRT1 : 참조, MRT2 : 숨은참조|
|-- receiveTypeName|	String|	수신자 타입 명|
|-- receiveName|	String|	수신자 이름|
|-- receiveMailAddr|	String|	수신자 메일 주소|
|-- isReceived| Boolean| 수신 여부 |
|-- resultDate| String| 수신 일시|
|-- isOpened| Boolean| 읽음 여부 |
|-- openedDate| String| 읽은 일시|
|-- dsnCode| String| DSN(Delivery Status Notification) 상태 코드|
|-- dsnMessage| String| DSN(Delivery Status Notification) 상태 메시지 |
|-- createDate |  String  | 생성 일시 |
|-- updateDate |  String  | 수정 일시 |

<a id="query-of-mails-3"></a>
### 대량 메일 발송 상세 조회 { #query-of-mails-3 }

<a id="query-of-mails-3-1"></a>
#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/mass-mails/{requestId}/{mailSeq}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|requestId|	String|	요청 ID|
|mailSeq|	Integer|	메일 순번|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

<a id="query-of-mails-3-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/mass-mails/'"${REQUEST_ID}"'/'"${MAIL_SEQ}"' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

<a id="query-of-mails-3-2"></a>
#### 응답

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
         "mailStatusName":"발송완료",
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
               "receiveTypeName":"받는사람",
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
         ]
      }
   }
}
```

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	List|	데이터 영역|
|-- requestId  | String  | 요청 ID |
|-- templateId | String  | 템플릿 ID |
|-- templateName | String  | 템플릿 명 |
|-- mailStatusCode | String  | 메일 상태 코드 <br/> SST0:발송준비, SST1:발송중,  <br/> SST2:발송완료, SST3: 발송실패, SST7: 미인증 |
|-- mailStatusName | String  | 메일 상태 명 |
|-- requestDate | String  | 요청 시간 |
|-- senderName | String  | 발신자 명 |
|-- senderAddress | String  | 발신자 주소 |
|-- resultId | String  | SMTP ID |
|-- resultDate | String  | 실제 발송 시간 |
|-- title | String  | 제목 |
|-- body | String  | 내용 |
|-- customHeaders|	Map|	[사용자 지정 헤더](./console-guide/#custom-header) |
|-- receiverList | List| 수신자 리스트|
|--- requestId | String  | 요청 ID |
|--- mailSeq | Integer  | 메일 순번 |
|--- receiveType | String  | 수신자 타입 (MRT0 : 받는사람 , MRT1 : 참조, MRT2 : 숨은참조) |
|--- receiveTypeName | String  | 수신자 타입명 |
|--- receiveMailAddr | String  | 수신자 메일 주소 |
|--- isReceived| Boolean| 수신 여부 |
|--- resultDate| String| 수신 일시|
|--- isOpened| Boolean| 읽음 여부 |
|--- openedDate| String| 읽은 일시|
|--- dsnCode| String| DSN(Delivery Status Notification) 상태 코드|
|--- dsnMessage| String| DSN(Delivery Status Notification) 상태 메시지 |
|-- attachFileList | List  | 첨부파일 리스트 |
|--- fileType|	String|	첨부파일 타입 (MAIL: 메일에 첨부된 파일, TEMPLATE: 템플릿에 첨부된 파일)|
|--- fileId| Integer| 파일 ID|
|--- fileName|	String|	첨부파일 이름|
|--- filePath|	String|	첨부파일 경로|
|--- fileSize|	Integer|	첨부파일 크기 (byte)|
|--- createDate|	String|	생성 일시|
<a id="scheduled-delivery-management"></a>
## Scheduled Delivery Management { #scheduled-delivery-management }

<a id="list-scheduled-delivery"></a>
### List Scheduled Delivery { #list-scheduled-delivery }
<a id="list-scheduled-delivery-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v2.0/appKeys/{appKey}/sender/reservations|

[Path parameter]

|Value|	Type|	Description|
|---|---|---|
|appKey|	String|	Unique appKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|Value|	Type|	Required|	Description|
|---|---|---|---|
|X-Secret-Key|	String| O | Unique secretKey [[Note](./api-guide/#secret-key)] |


[Query parameter]

|Value|	Type|	Required|	Description|
|---|---|---|---|
| requestId | String | X | Request ID |
| startSendDate | String | X | Start time of delivery request (yyyy-MM-dd HH:mm:ss) |
| endSendDate | String | X | End time of delivery request (yyyy-MM-dd HH:mm:ss) |
| senderMail | String | X | Sender email address |
| receiveMail | String | X | Recipient email address |
| templateId | String | X | Template ID |
| pageNum | Integer | X | Page number 1 (default) |
| pageSize | Integer | X | Number of results to retrieve 15 (default) |

<a id="list-scheduled-delivery-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```


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

<a id="list-scheduled-delivery-response"></a>
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
            "senderGroupingKey":"senderKey"
         }
      ]
   }
}
```

| Value               | Type    | Description                                                  |
|---|---|---|
| header              | Object  | Header area                                                  |
| - isSuccessful      | Boolean | Successful or not                                            |
| - resultCode        | Integer | Failure code                                                 |
| - resultMessage     | String  | Failure message                                              |
| body                | Object  | Body area                                                    |
| - pageNum           | Integer | Current page number                                          |
| - pageSize          | Integer | Number of queried data                                       |
| - totalCount        | Integer | Number of total data                                         |
| - data              | List    | Data area                                                    |
| -- requestId        | String  | Request ID                                                   |
| -- mailSeq          | Integer | Mail sequence                                                |
| -- title            | String  | Title                                                        |
| -- templateId       | String  | Template ID                                                  |
| -- templateName     | String  | Template name                                                |
| -- senderAddress    | String  | Sender's mail address                                        |
| -- senderName       | String  | Sender's name                                                |
| -- receiveMailAddr  | String  | Recipient's mail address                                     |
| -- receiveName      | String  | Name of recipient                                            |
| -- receiveType      | String  | Recipient type  (MRT0: recipient, MRT1: cc, MRT2: bcc)       |
| -- receiveTypeName  | String  | Name of recipient type                                       |
| -- requestDate      | String  | Date and time of sending                                     |
| -- mailStatusCode   | String  | Delivery status code<br/>SST0: Preparing Delivery, SST1: Delivering<br/>SST2: Delivery Completed, SST3: Delivery Failed<br/>SST4: Scheduled Waiting, SST7: Not Authenticated     |
| -- mailStatusName   | String  | Name of delivery status                                      |
| -- senderGroupingKey| String  | Sender's group key                                           |

<a id="query-detail-scheduled-delivery"></a>
### Query Detail Scheduled Delivery { #query-detail-scheduled-delivery }
<a id="query-detail-scheduled-delivery-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v2.0/appKeys/{appKey}/sender/reservations/{requestId}/{mailSeq}|

[Path parameter]

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

<a id="query-detail-scheduled-delivery-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations/'"${REQUEST_ID}"'/'"${MAIL_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

<a id="query-detail-scheduled-delivery-response"></a>
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
         "mailStatusName":"예약대기",
         "receivers":[
            {
               "requestId":"202006050000008j6bx5Q1",
               "mailSeq":0,
               "receiveType":"MRT0",
               "receiveTypeName":"받는사람",
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
         "senderGroupingKey":"senderKey"
      }
   }
}
```

| Value               | Type    | Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |
| header              | Object  | Header area                                                  |
| - isSuccessful      | Boolea{}n | Successful or not                                            |
| - resultCode        | Integer | Failure code                                                 |
| - resultMessage     | String  | Failure message                                              |
| body                | Object  | Body area                                                    |
| - data              | Object  | Data area                                                    |
| -- requestId        | String  | Request ID                                                   |
| -- mailSeq          | Integer | Mail sequence                                                |
| -- title            | String  | Mail title                                                   |
| -- body             | String  | Mail body                                                    |
| -- templateId       | String  | Template ID                                                  |
| -- templateName     | String  | Template name                                                |
| -- senderAddress    | String  | Sender's mail address                                        |
| -- senderName       | String  | Sender's name                                                |
| -- requestDate      | String  | Date and time of sending                                     |
| -- mailStatusCode   | String  | Delivery status code<br/>SST0: Preparing Delivery, SST1: Delivering<br/>SST2: Delivery Completed, SST3: Delivery Failed<br/>SST4: Scheduled Waiting , SST7: Not Authenticated    |
| -- mailStatusName   | String  | Name of delivery status                                      |
| -- receiverList     | List    | List of recipients                                           |
| --- requestId        | String  | Request ID                                                   |
| --- mailSeq          | Integer | Mail sequence                                                |
| --- receiveType      | String  | Recipient type  (MRT0: recipient, MRT1: cc, MRT2: bcc)       |
| --- receiveName      | String  | Name of recipient                                            |
| --- receiveTypeName  | String  | Name of recipient type                                       |
| --- receiveMailAddr  | String  | Recipient's mail address                                     |
| -- attachFileList   | List    | List of attached files                                       |
| --- fileType        | String  | Type of attached files <br/>MAIL: files attached to mail, TEMPLATE: files attached to template|
| --- fileId          | Integer  | File ID <br/> Attachments uploaded through the v1.0, v1.1 APIs are marked with a file ID value of -1 |
| --- fileName        | String  | Name of attached file                                        |
| --- filePath        | String  | Path of attached file                                        |
| --- fileSize        | Integer | Size of attached file (byte)                                 |
| --- createDate      | String  | Date and time of creation                                    |
| -- customHeaders    | Map     | [Custom Header](./console-guide/#custom-header)                   |
| -- senderGroupingKey| String  | Sender's group key                                           |


<a id="cancel-scheduled-delivery-by-request"></a>
### Cancel Scheduled Delivery by Request { #cancel-scheduled-delivery-by-request }
<a id="cancel-scheduled-delivery-by-request-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
|PUT| /email/v2.0/appKeys/{appKey}/sender/reservations/{requestId}

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

<a id="cancel-scheduled-delivery-by-request-curl"></a>
#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations/'"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

<a id="cancel-scheduled-delivery-by-request-response"></a>
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

| Value               | Type    | Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |
| header              | Object  | Header area                                                  |
| - isSuccessful      | Boolean | Successful or not                                            |
| - resultCode        | Integer | Failure code                                                 |
| - resultMessage     | String  | Failure message                                              |

<a id="cancel-scheduled-delivery-by-recipient"></a>
### Cancel Scheduled Delivery by recipient { #cancel-scheduled-delivery-by-recipient }
<a id="cancel-scheduled-delivery-by-recipient-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
|PUT| /email/v2.0/appKeys/{appKey}/sender/reservations/{requestId}/{mailSeq}

[Path parameter]

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

<a id="cancel-scheduled-delivery-by-recipient-curl"></a>
#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations/'"${REQUEST_ID}"'/'"${MAIL_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

<a id="cancel-scheduled-delivery-by-recipient-response"></a>
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

| Value               | Type    | Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |
| header              | Object  | Header area                                                  |
| - isSuccessful      | Boolean | Successful or not                                            |
| - resultCode        | Integer | Failure code                                                 |
| - resultMessage     | String  | Failure message                                              |

<a id="cancel-scheduled-delivery---multiple-filter"></a>
### Cancel Scheduled Delivery - Multiple Filter { #cancel-scheduled-delivery---multiple-filter }
<a id="cancel-scheduled-delivery---multiple-filter-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
|PUT| /email/v2.0/appKeys/{appKey}/sender/reservations/search-cancels

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

<a id="cancel-scheduled-delivery---multiple-filter-curl"></a>
#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations/search-cancels' \
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

<a id="cancel-scheduled-delivery---multiple-filter-response"></a>
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

| Value               | Type    | Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |
| header              | Object  | Header area                                                  |
| - isSuccessful      | Boolean | Successful or not                                            |
| - resultCode        | Integer | Failure code                                                 |
| - resultMessage     | String  | Failure message                                              |
| body                | Object  | Body area                                                    |
| - data              | Object  | Data area                                                    |
| -- reservationCancelId|	String |	Schedule Cancellation ID|
| -- requestedDateTime|	String|	Time for Schedule Cancellation(yyyy-MM-dd HH:mm:ss)|
| -- reservationCancelStatus|	String|	Status of Schedule Cancellation<br/>- READY : Preparing for Scheduling<br/>- PROCESSING : Cancelling Schedule  <br/>- COMPLETED : Schedule Cancellation Completed<br/>- FAILED : Schedule Cancellation Failed |


<a id="list-request-of-scheduled-delivery-cancellation---multiple-filter"></a>
### List Request of Scheduled Delivery Cancellation - Multiple Filter { #list-request-of-scheduled-delivery-cancellation---multiple-filter }
<a id="list-request-of-scheduled-delivery-cancellation---multiple-filter-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v2.0/appKeys/{appKey}/sender/reservations/search-cancels

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

<a id="list-request-of-scheduled-delivery-cancellation---multiple-filter-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations/search-cancels' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

<a id="list-request-of-scheduled-delivery-cancellation---multiple-filter-response"></a>
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

| Value               | Type    | Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |
| header              | Object  | Header area                                                  |
| - isSuccessful      | Boolean | Successful or not                                            |
| - resultCode        | Integer | Failure code                                                 |
| - resultMessage     | String  | Failure message                                              |
| body                | Object  | Body area                                                    |
| - pageNum           | Integer | Current page number                                          |
| - pageSize          | Integer | Number of queried data                                       |
| - totalCount        | Integer | Number of total data                                         |
| - data              | List    | Data area                                                    |
|-- reservationCancelId|	String | Schedule Cancellation ID |
|-- searchParameter | Map<String, Object> | Request Parameter for Schedule Cancellation |
|-- requestedDateTime|	String | Request Time for Schedule Cancellation|
|-- completedDateTime|	String | Request End Time for Schedule Cancellation |
|-- reservationCancelStatus|	String| 	Status of Schedule Cancellation<br/>- READY : Preparing for Scheduling<br/>- PROCESSING : Cancelling Schedule <br/>- COMPLETED : Schedule Cancellation Completed<br/>- FAILED : Schedule Cancellation Failed |
|-- totalCount | Integer |  Number of Scheduled Cancellation Targets |
|-- successCount | Integer |Number of Successful Schedule Cancellation |
|-- createUser |	String| Requester of Scheduled Cancellation	|
|-- createdDateTime | String | Request Creation Time for Schedule Cancellation |
|-- updatedDateTime | String | Modified Time for Scheduled Cancellation  |


<p id="category"></p>

<a id="category-management"></a>
## Category Management { #category-management }

<a id="list"></a>
### List { #list }

<a id="list-request"></a>
#### Request 

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v2.0/appKeys/{appKey}/categories|

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

<a id="list-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/categories' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

<a id="list-response"></a>
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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- pageNum|	Integer| Current page number |
|- pageSize|	Integer| Queried data count |
|- totalCount|	Integer| Total data count |
|- data|	List| Data area |
|-- categoryId|	Integer| Category ID |
|-- categoryParentId|	Integer| Parent category ID (0, for the highest-level category) |
|-- depth|	Integer| Depth (0, for the highest-level category) |
|-- categoryName|	String| Category name |
|-- categoryDesc|	String| Category description |
|-- useYn|	String| Use or not |
|-- createUser|	String| Creator |
|-- createDate|	String| Date and time of creation |
|-- updateUser|	String| Modifier |
|-- updateDate|	String| Date and time of modification |

<a id="query-details"></a>
### Query Details { #query-details }

<a id="query-details-request"></a>
#### Request 

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/categories/{categoryId}|

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

<a id="query-details-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/categories/'"${CATEGORY_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

<a id="query-details-response"></a>
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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- data|	List| Data area |
|-- categoryId|	Integer| Category ID |
|-- categoryParentId|	Integer| Parent category ID (0, for the highest-level category) |
|-- depth|	Integer| Depth (0, for the highest-level category) |
|-- categoryName|	String| Category name |
|-- categoryDesc|	String| Category description |
|-- useYn|	String| Use or not |
|-- createUser|	String| Creator |
|-- createDate|	String| Date and time of creation |
|-- updateUser|	String| Modifier |
|-- updateDate|	String| Date and time of modification |


<a id="register"></a>
### Register { #register }

<a id="register-request"></a>
#### Request 

[URL]

|Http method|	URI|
|---|---|
|POST|/email/v2.0/appKeys/{appKey}/categories|


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

<a id="register-curl"></a>
#### cURL
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/categories'
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


<a id="register-response"></a>
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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- data|	Object| Data area |
|-- categoryId|	Integer| Cateogry ID |


<a id="modify"></a>
### Modify { #modify }

<a id="modify-request"></a>
#### Request 

[URL]

|Http method|	URI|
|---|---|
|PUT|/email/v2.0/appKeys/{appKey}/categories/{categoryId}|

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

<a id="modify-curl"></a>
#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/categories/'"${CATEGORY_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "categoryName": "Category",
    "categoryDesc": "Top Category",
    "useYn": "Y",
    "userId": "USER"
}'
```

<a id="modify-response"></a>
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
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |

<a id="delete"></a>
### Delete { #delete }

<a id="delete-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
|DELETE|/email/v2.0/appKeys/{appKey}/categories/{categoryId}|

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

<a id="delete-curl"></a>
#### cURL
```
curl -X DELETE \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/categories/'"${CATEGORY_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="delete-response"></a>
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
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |

<p id="template"></p>

<a id="query-of-templates"></a>
## Query of Templates { #query-of-templates }

<a id="query-list-of-templates"></a>
### Query List of Templates { #query-list-of-templates }

<a id="query-list-of-templates-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/templates|

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
| all        | Boolean | X        | Query list of all templates or not |

<a id="query-list-of-templates-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/templates' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="query-list-of-templates-response"></a>
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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- pageNum|	Integer| Current page number |
|- pageSize|	Integer| Number of queried data |
|- totalCount|	Integer| Total number of data |
|- data|	List| Data area |
|-- templateId|	String| Template ID |
|-- categoryId|	Integer| Category ID |
|-- categoryName|	String| Category name |
|-- templateName|	String| Template name |
|-- templateDesc|	String| Template description |
|-- useYn|	String| Use or not |
|-- delYn|	String| Delete or not |
|-- title|	String| Title |
|-- createDate|	String| Date and time of creation |
|-- updateDate|	String| Date and time of modification |

<a id="query-template-details"></a>
### Query Template Details { #query-template-details }

<a id="query-template-details-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/templates/{templateId}|

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

<a id="query-template-details-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="query-template-details-response"></a>
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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- data|	List| Data area |
|-- templateId|	String| Template ID |
|-- categoryId|	Integer| Category ID |
|-- categoryName|	String| Category name |
|-- templateName|	String| Template name |
|-- templateDesc|	String| Template description |
|-- useYn|	String| Use or not (Y= In use, N= Not in use) |
|-- delYn|	String| Delete or not (Y= Delete, N= Not delete) |
|-- sendMailAddress|	String| Sender's mail address |
|-- title|	String| Mail address |
|-- templateType|	String| Template type <br/>DEFAULT(default), FREEMARKER) |
|-- body|	String| Body |
|-- createDate|	String| Date and time of creation |
|-- updateDate|	String| Date and time of modification |
|-- attachFileList|	List| List of attached files |
|--- fileType|	String| Attachment type (MAIL: Files attached to mail, TEMPLATE: Files attached to template) |
|--- fileId| Integer| File ID |
|--- fileName|	String| Name of attached file |
|--- filePath|	String| Path of attached file |
|--- fileSize|	Integer| Size of attached file (byte) |
|--- createDate|	String| Date and time of creation |

<a id="register-templates"></a>
### Register Templates { #register-templates }

<a id="register-templates-request"></a>
#### Request 

[URL]

|Http method|   URI|
|---|---|
|POST|  /email/v2.0/appKeys/{appKey}/templates|

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

<a id="register-templates-curl"></a>
#### cURL
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/templates' \
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


<a id="register-templates-response"></a>
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

<a id="query-of-templates-upload-attached-files"></a>
### Upload Attached Files { #query-of-templates-upload-attached-files }

<a id="query-of-templates-upload-attached-files-request"></a>
#### Request

[URL]

|Http method|   URI|
|---|---|
|POST|  /email/v2.0/appKeys/{appKey}/templates/attachfile/binaryUpload|

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
|fileName|  String| - |O| File name |
|fileBody|  Byte[]| - |O| Byte [] value |
|userId|    String| 50|X| User ID |

<a id="query-of-templates-upload-attached-files-curl"></a>
#### cURL
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/templates/attachfile/binaryUpload' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "fileName": "file.csv",
    "userId": "USER",
    "fileBody": []
}'
```

<a id="query-of-templates-upload-attached-files-response"></a>
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

|Value| Type | Description |
|---|---|---|
|header|    Object| Header area |
|- isSuccessful|    Boolean| Successful or not |
|- resultCode|  Integer| Failure code |
|- resultMessage|   String| Failure message |
|body|  Object| Body area |
|- data|    Object| Data area |
|-- fileId| Integer| File ID |
|-- fileName|   String| File name |

<a id="modify-templates"></a>
### Modify Templates { #modify-templates }

<a id="modify-templates-request"></a>
#### Request

[URL]

|Http method|   URI|
|---|---|
|PUT|   /email/v2.0/appKeys/{appKey}/templates/{templateId}|

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

<a id="modify-templates-curl"></a>
#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
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

<a id="modify-templates-response"></a>
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

<a id="delete-templates"></a>
### Delete Templates { #delete-templates }

<a id="delete-templates-request"></a>
#### Request

[URL]

|Http method|   URI|
|---|---|
|DELETE|    /email/v2.0/appKeys/{appKey}/templates/{templateId}|

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

<a id="delete-templates-curl"></a>
#### cURL
```
curl -X DELETE \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="delete-templates-response"></a>
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
<a id="query-statistics"></a>
## Query Statistics { #query-statistics }

<a id="query-daily-statistics"></a>
### Query Daily Statistics { #query-daily-statistics }

<a id="query-daily-statistics-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/statistics/view |

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

<a id="query-daily-statistics-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/statistics/view?from='"${FROM}"'&to='"${TO}"'&searchType='"${SEARCH_TYPE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="query-daily-statistics-response"></a>
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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- data|	List| Data area |
|-- divisionName | String | Statistical criteria (date/time/day) |
|-- requestedCount | Long | Number of delivery requests |
|-- sentCount | Long | Number of deliveries |
|-- receivedCount | Long | Number of receipts |
|-- openedCount | Long | Number of openings |
|-- sentRate | String | Delivery rate |
|-- receivedRate | String | Receiving rate |
|-- openedRate | String | Opening rate |

<a id="rejection-management"></a>
## Rejection Management { #rejection-management }

<a id="query-rejections"></a>
### Query Rejections { #query-rejections }

<a id="query-rejections-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
| GET |	/email/v2.0/appKeys/{appKey}/block-receivers |

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

<a id="query-rejections-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/block-receivers' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="query-rejections-response"></a>
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

| Value           | Type    | Description                               |
| --------------- | ------- | ----------------------------------------- |
| header          | Object  | Header area                               |
| - isSuccessful  | Boolean | Successful or not                         |
| - resultCode    | Integer | Failure code                              |
| - resultMessage | String  | Failure message                           |
| body            | Object  | Body area                                 |
| - pageNum       | Integer | Current page number                       |
| -pageSize       | Integer | Number of queried data                    |
| - totalCount    | Integer | Total number of data                      |
| - data          | List    | Data area                                 |
| -- mailAddress  | String  | Email address to reject                   |
| -- blockDate    | String  | Date of rejection (yyyy-MM-dd HH:mm:ss.S) |

<a id="register-rejections"></a>
### Register Rejections { #register-rejections }

<a id="register-rejections-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
| POST |	/email/v2.0/appKeys/{appKey}/block-receivers |

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

<a id="register-rejections-curl"></a>
#### cURL
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/block-receivers' \
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

<a id="register-rejections-response"></a>
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

| Value           | Type    | Description       |
| --------------- | ------- | ----------------- |
| header          | Object  | Header area       |
| - isSuccessful  | Boolean | Successful or not |
| - resultCode    | Integer | Failure code      |
| - resultMessage | String  | Failure message   |

<a id="delete-rejections"></a>
### Delete Rejections { #delete-rejections }

<a id="delete-rejections-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
| PUT |	/email/v2.0/appKeys/{appKey}/block-receivers |

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

<a id="delete-rejections-curl"></a>
#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/block-receivers' \
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

<a id="delete-rejections-response"></a>
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

| Value           | Type    | Description       |
| --------------- | ------- | ----------------- |
| header          | Object  | Header area       |
| - isSuccessful  | Boolean | Successful or not |
| - resultCode    | Integer | Failure code      |
| - resultMessage | String  | Failure message   |
