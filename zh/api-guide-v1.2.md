## Notification > Email > API v1.2 Guide

[API Domain]

|Environment| Domain |
|---|---|
|Real|	https://api-mail.cloud.toast.com |

[Header]
```
Content-Type: application/json;charset=UTF-8
```

[Caution for curl Example]

* Curl example may not be properly requested in Windows cmd.

## Mail Delivery 

### Send General Mails

#### Request 

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.2/appKeys/{appKey}/sender/mail|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Request Body]

|Value| Type | Required | Description |
|---|---|---|---|
|​senderAddress|	String|	O| Sender's mail address |
|senderName|	String|	X| Sender's name |
|requestDate|	String|	X|	Enter current time when delivery time is missing (yyyy-MM-dd HH:mm:ss) |
|title|	String|	O| Title |
|body|	String|	O| Body |
|attachFileIdList|	List:Integer|	X| ID of uploaded attachment |
|templateId|	String|	X| ID of delivery template |
|templateParameter|	Object|	X| Replacement parameter (to enter mail title/body for replacement) |
|- #key#|	String|	X| Replacement key (##key##) |
|- #value#|	Object|	X| Mapped value for replacement key |
|receiverList|	List|	O| List of recipients <br/>Send up to 1,000 persons (including recipients and cc recipients) |
|- receiveMailAddr|	String|	O| Recipient's mail address |
|- receiveName|	String|	X| Recipient's name |
|- receiveType|	String|	O| Recipient type (MRT0: recipient, MRT1: Cc, MRT2: Bcc) |
|customHeaders| Map| X| [Custom Header](./Overview/#custom-header) |
|userId|	String|	X| Delimiter of delivery <br />e.g.) admin,system |

[Caution]

* To use a template, title and body are not required (input values, if available, precede template values)
* The templateId parameter is required to use templateParameter.

#### Changes in v 1.2

* Data type of **attachFileIdList** has changed from List:String to List:Integer.
* Re-use of ID (attachField) which has a delivery record becomes available since v1.2 API. 
* ID for **attachFileIdList** refers to ID of uploaded file by using  [v1.2 Attachment API](./api-guide/#_14).  

[Example 1]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/sender/mail -d '{"senderAddress":"support@nhnent.com","senderName":"sender's name","title":"sample title","body":"sample body","receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhnent.com","receiveName":"customer2","receiveType":"MRT1"}],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

[Example 2 - Using Templates]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/sender/mail -d '{"templateId":"TEMPLATE1","templateParameter":{"key":"value"},"receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhnent.com","receiveName":"customer2","receiveType":"MRT1"}],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

#### Response

```
{
    "header": {
        "isSuccessful":boolean,
        "resultCode":Integer,
        "resultMessage":String
    },
    "body": {
        "data": {
            "requestId":String,
            "statusCode":String
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
|-- requestId|	String| Request ID |
|-- statusCode|	String| Request status code (Y: preparing for sending , N: preparing for sending failed) |

### Send Individual Mails 

- Send mails to each of the many recipients. Each recipient can see no other recipients but himself. 

#### Request

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.2/appKeys/{appKey}/sender/eachMail||

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request Body]

|Value| Type | Required | Description |
|---|---|---|---|
|​senderAddress|	String|	O| Sender's mail |
|senderName|	String|	X| Sender's name |
|requestDate|	String|	X| Enter current time when delivery time is missing (yyyy-MM-dd HH:mm:ss) |
|title|	String|	O| Title |
|body|	String|	O| Body |
|attachFileIdList|	List:Integer|	X| ID for uploaded attached file |
|templateId|	String|	X| ID for delivery template |
|receiverList|	List|	O| List of recipients<br/>Send up to 1,000 persons |
|- receiveMailAddr|	String|	O| Recipient's mail address |
|- receiveName|	String|	X| Recipient's name |
|- templateParameter|	Object|	X| Replacement parameter (to enter mail title/body for replacement) |
|-- #key#|	String|	X| Replacement key (##key##) |
|-- #value#|	Object|	X| Mapped value for replacement key |
|customHeaders| Map| X| [Custom Header](./Overview/#custom-header) |
|userId|	String|	X| Delimiter for delivery  e.g.) admin,system |

[Caution]

* To use a template, title and body are not required (input values, if available, precede template values)
* The templateId parameter is required to use templateParameter.

* #### Changes in v 1.2

  - Data type of **attachFileIdList** has changed from List:String to List:Integer.
  - Re-use of ID (attachField) which has a delivery record becomes available since v1.2 API. 
  - ID for **attachFileIdList** refers to ID of uploaded file by using [v1.2 Attachment API](./api-guide/#_14).

[Example 1]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/sender/eachMail -d '{"senderAddress":"support@nhnent.com","senderName":"sender's name","title":"sample title","body":"sample body","attachFileIdList":[1, 2],"receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1"}],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

[Example 2 - Using Templates]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/sender/mail -d '{"templateId":"TEMPLATE1","receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1","templateParameter":{"key":"value"}}],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```


#### Response

```
{
    "header": {
        "isSuccessful":boolean,
        "resultCode":Integer,
        "resultMessage":String
    },
    "body": {
        "data": {
            "requestId":String,
            "statusCode":String
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
|-- requestId|	String| Request ID |
|-- statusCode|	String| Request status code (Y: preparing for sending , N: Preparing for sending failed) |

### Send Advertising General Mails 

- Only the URL-end changes to ad-mail, while the rest is same as sending general mails.

#### Caution for Sending Ad Mails 

- The title must include the (Ad) phrase. 
- For more details, see [Send Advertising Mails](./console-guide/#_3).

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.2/appKeys/{appKey}/sender/ad-mail|

[Example 1]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/sender/ad-mail -d '{"senderAddress":"support@nhnent.com","senderName":"sender's name","title":"(Ad) sample title","body":"sample body \n##BLOCK_RECEIVER_LINK## \n##EN_BLOCK_RECEIVER_LINK##","receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhnent.com","receiveName":"customer2","receiveType":"MRT1"}],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

[Example 2 - Using Templates]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/sender/ad-mail -d '{"templateId":"TEMPLATE1","templateParameter":{"key":"value"},"receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhnent.com","receiveName":"customer2","receiveType":"MRT1"}],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

### Send Advertising Individual Mails 

- Only the URL-end changes to ad-eachMail, while the rest is same as sending individual mails. 

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.2/appKeys/{appKey}/sender/ad-eachMail |

[Example 1]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/sender/ad-eachMail -d '{"senderAddress":"support@nhnent.com","senderName":"sender's name","title":"(Ad) sample title","body":"sample body \n##BLOCK_RECEIVER_LINK## \n##EN_BLOCK_RECEIVER_LINK##","attachFileIdList":[1, 2],"receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1"}],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

[Example 2 - Using Templates]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/sender/ad-eachMail -d '{"templateId":"TEMPLATE1","receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1","templateParameter":{"key":"value"}}],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

### Send Authenticated Mails 

#### Request

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.2/appKeys/{appKey}/sender/auth-mail||

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request Body]

|Value| Type | Required | Description |
|---|---|---|---|
|​senderAddress|	String|	O| Sender's address |
|senderName|	String|	X| Sender's name |
|requestDate|	String|	X| Enter current time when delivery time is missing (yyyy-MM-dd HH:mm:ss) |
|title|	String|	O| Title |
|body|	String|	O| Body |
|templateId|	String|	X| ID for delivery template |
|receiver|	Object|	O|	Recipient |
|- receiveMailAddr|	String|	O| Recipient's mail address |
|- receiveName|	String|	X| Recipient's name |
|- templateParameter|	Object|	X| Replacement parameter (to enter mail title/body for replacement) |
|-- #key#|	String|	X| Replacement key (##key##) |
|-- #value#|	Object|	X| Mapped value for replacement key |
|customHeaders| Map| X| [Custom Header](./Overview/#custom-header) |
|userId|	String|	X| Delimiter for delivery e.g.) admin,system |

[Caution]

* To use a template, title and body are not required (input values, if available, precede template values)
* The templateId parameter is required to use templateParameter.

#### Differences from General Mails 
Features of authenticated mails are as follows: 

* Allows only single-case deliveries (one recipient).
* File attachment is not supported. Templates including attachment are not supported. 

[Example 1]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/sender/auth-mail -d '{"senderAddress":"support@nhnent.com","senderName":"sender's name","title":"sample title","body":"sample body","receiver":{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1"},"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

[Example 2 - Using Templates]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/sender/auth-mail -d '{"templateId":"TEMPLATE1","receiver":{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1","templateParameter":{"key":"value"}},"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

#### Response

```
{
    "header": {
        "isSuccessful":boolean,
        "resultCode":Integer,
        "resultMessage":String
    },
    "body": {
        "data": {
            "requestId":String,
            "statusCode":String
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
|-- requestId|	String| Request ID |
|-- statusCode|	String| Request status code (Y: preparing for sending , N: Preparing for sending failed) |

### Send Tagged Mails

#### Request

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.2/appKeys/{appKey}/sender/tagMail|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request Body]

```
{
    "senderAddress": String,
    "senderName": String,
    "requestDate": String,
    "title": String,
    "body": String,
    "templateId": String,
    "adYn": String,
    "autoSendYn": String,
    "attachFileIdList": List:String,
    "tagExpression": List:String,
    "userId": String
}
```

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
| tagExpression    | List:String | O        | Tag expression                                               |
| customHeaders    | Map         | X        | [Custom Header](./Overview/#custom-header)                   |
| userId           | String      | X        | Delimiter of delivery e.g.) admin, system                    |

[Caution]

* To use template, title and body are required (input values, if available, precede template values)

* #### Changes in v 1.2

  - Data type of **attachFileIdList** has changed from List:String to List:Integer.
  - Re-use of ID (attachField) which has a delivery record becomes available since v1.2 API. 
  - ID for **attachFileIdList** refers to ID of uploaded file by using [v1.2 Attachment API](./api-guide/#_14).

[Example 1]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/sender/tagMail -d '{"senderAddress":"support@nhnent.com","senderName":"sender's name","title":"sample title","body":"sample body","attachFileIdList":[1, 2],"tagExpression":["tag1","AND","tag2"],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

[Example 2 - Using Templates]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/sender/tagMail -d '{"templateId":"TEMPLATE1","tagExpression":["tag1","AND","tag2"],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

#### Response

```
{
    "header": {
        "isSuccessful":boolean,
        "resultCode":Integer,
        "resultMessage":String
    },
    "body": {
        "data": {
            "requestId":String
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
| -- requestId    | String  | Request ID        |

### Upload Attached Files 

#### Request 

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.2/appKeys/{appKey}/attachfile/binaryUpload|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request Body]

```
{
    "fileName": String,
    "createUser": String,
    "fileBody": Byte[]
}
```

| Value      | Type   | Required | Description               |
| ---------- | ------ | -------- | ------------------------- |
| fileName   | String | O        | File name                 |
| fileBody   | Byte[] | O        | Byte[]  of a file         |
| createUser | String | O        | File uploader information |

[Example]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/attachfile/binaryUpload -d '{"fileName":"file.csv","createUser":"XXXXX","fileBody":[]}'
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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- data|	Object| Data area |
|-- fileId|	String| File ID |
|-- fileName|	String| File name |

#### Changes in v 1.2

* Response has changed. 
* The v 1.0 Upload Attachment API returned the string-format **requestId** for delivery, while the v 1.2 Upload Attachment API returns the Integer-format **attachFileIdList**. 
* The  **attachFileIdList** of v1.2 Send API can have many on the list.  

### Title/Body Replacement

#### Example of Writing Mails with Replacement

```
* title : Hello, ##title_name## !!
* body : We send ##body_content##.
```

#### Example of General Mail Request 
```
{
    "senderAddress" : "support@nhnent.com",
    "templateId": "template1",
    "templateParameter" : {"title_name": "cloud customer 1", "body_content": "test1"},
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
```
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

#### Example of Result 
```
* title: Hello, Cloud Customer 1!!
* body: We send test2.
```

## Query of Mails

### Query List of Mail Deliveries

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.2/appKeys/{appKey}/sender/mails|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query Parameter]

| Value            | Type    | Required | Description                                                  |
| ---------------- | ------- | -------- | ------------------------------------------------------------ |
| requestId        | String  | O        | Request ID                                                   |
| startSendDate    | String  | O        | Delivery start date (yyyy-MM-dd HH:mm:ss)                    |
| endSendDate      | String  | X        | Delivery end date (yyyy-MM-dd HH:mm:ss)                      |
| startReceiveDate | String  | X        | Receiving start date (yyyy-MM-dd HH:mm:ss)                   |
| endReceiveDate   | String  | X        | Receiving end date (yyyy-MM-dd HH:mm:ss)                     |
| senderMail       | String  | X        | Sender's mail address                                        |
| senderName       | String  | X        | Sender's name                                                |
| receiveMail      | String  | X        | Recipient's mail address                                     |
| templateId       | String  | X        | Template number                                              |
| sendStatus       | String  | X        | Delivery status code <br/> SST0: Preparing for sending, SST1: Sending,  <br/> SST2: Sending completed, SST3: Sending failed |
| pageNum          | Integer | X        | Page number (default: 1)                                     |
| pageSize         | Integer | X        | Number of queries (default: 15)                              |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/sender/mails?startSendDate=2018-03-01+00%3A00&endSendDate=2018-03-07+23%3A59&pageSize=10"
```

#### Response

```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    },
    "body": {
        "pageNum": Integer,
        "pageSize": Integer,
        "totalCount": Integer,
        "data": [{
            "requestId": String,
            "requestIp": String,
            "templateId": String,
            "templateName": String,
            "masterStatusCode": String,
            "mailSeq": String,
            "senderName": String,
            "senderMail": String,
            "title": String,
            "body": String,
            "resultId": String,
            "resultDate": String,
            "mailStatusCode": String,
            "mailStatusName": String,
            "requestDate": String,
            "receiverMail": String,
            "receiveType": String
        }]
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
| -pageSize           | Integer | Number of queried data                                       |
| - totalCount        | Integer | Number of total data                                         |
| - data              | List    | Data area                                                    |
| -- requestId        | String  | Request ID                                                   |
| -- requestDate      | String  | Date and time of sending                                     |
| -- requestIp        | String  | Sender server IP                                             |
| -- resultDate       | String  | Date and time of receiving                                   |
| -- templateId       | String  | Template ID                                                  |
| -- templateName     | String  | Template name                                                |
| -- masterStatusCode | String  | Preparation status code for mail delivery (Y: preparing for sending, N: sending failed) |
| -- mailSeq          | String  | Mail sequence                                                |
| -- body             | String  | Body                                                         |
| -- title            | String  | Title                                                        |
| -- senderMail       | String  | Sender's mail address                                        |
| -- senderName       | String  | Sender's name                                                |
| -- receiverMail     | String  | Recipient's mail address                                     |
| -- receiveType      | String  | Recipient type  (MRT0: recipient, MRT1: cc, MRT2: bcc)       |
| -- resultId         | String  | Delivery result ID                                           |
| -- resultDate       | String  | Date and time of delivery completed                          |
| -- mailStatusCode   | String  | Delivery status code <br/> SST0: Preparing Delivery, SST1: Delivering,  <br/> SST2: Delivery Completed, SST3: Delivery Failed |
| -- mailStatusName   | String  | Name of delivery status                                      |
| -- requestDate      | String  | Date and time of delivery requested                          |

### Query Mail Delivery Details

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.2/appKeys/{appKey}/sender/mail/{requestId}/{mailSeq}|

[Path Parameter]

| Value     | Type    | Description     |
| --------- | ------- | --------------- |
| appKey    | String  | Original appkey |
| requestId | String  | Request ID      |
| mailSeq   | Integer | Mail sequence   |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/sender/mail/{requestId}/{mailSeq}"
```

#### Response

```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    },
    "body": {
        "data": {
            "requestId": String,
            "requestIp": String,
            "requestDate": String,
            "masterStatusCode": String,
            "mailStatusCode": String,
            "mailStatusName": String,
            "templateId": String,
            "templateName": String,
            "senderName": String,
            "senderMail": String,
            "title": String,
            "body": String,
            "attachFileYn": String,
            "resultId": String,
            "resultDate": String,
            "receivers": [{
                "requestId": String,
                "mailSequence": Integer,
                "receiveType": String,
                "receiveTypeName": String,
                "receiveName": String,
                "receiveMailAddr": String,
                "readYn": String,
                "readDate": String
            }],
            "attachFileList": [{
                "fileType": String,
                "fileId": Integer,
                "fileName": String,
                "filePath": String,
                "fileSize": Integer,
                "createDate": String
            }],
            "customHeaders": Map
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
|-- mailStatusCode|	String| Delivery status |
|-- mailStatusName|	String| Name of delivery status |
|-- templateId|	String| Template ID |
|-- templateName|	String| Template name |
|-- senderName|	String| Sender's name |
|-- senderMail|	String| Sender's mail address |
|-- title|	String| Mail title |
|-- body|	String| Mail body |
|-- attachFileYn|	String| Attached or not |
|-- resultId|	String| Mail delivery ID |
|-- resultDate|	String| Date and time of delivery completed |
|-- receivers|	List| List of recipients |
|--- requestId|	String| Request ID |
|--- mailSequence|	Integer| Mail sequence |
|--- receiveType|	String| Recipient type |
|--- receiveTypeName|	String| Name of recipient type |
|--- receiveName|	String| Recipient's name |
|--- receiveMailAddr|	String| Recipient's mail address |
|--- readYn| String| Received or not |
|--- readDate| String| Received date (yyyy-MM-dd HH:mm:ss.SSS) |
|-- attachFileList|	List| List of attached files |
|--- fileType|	String| Type of attached files (MAIL: files attached to mail, TEMPLATE: files attached to template) |
|--- fileId| String| File ID |
|--- fileName|	String| Name of attached file |
|--- filePath|	String| Path of attached file |
|--- fileSize|	Integer| Size of attached file (byte) |
|--- createDate|	String| Date and time of creation |
|-- customHeaders|	Map|	[Custom Header](./Overview/#custom-header) |

#### Changes in v 1.2

* Response has changed. 
* The v 1.0 Upload Attachment API returned the string-format **requestId** for delivery, while the v 1.2 Upload Attachment API returns the Integer-format **attachFileIdList**. 
* The  **attachFileIdList** of v1.2 Send API can have many on the list.  
* Send API and Query API must be of the same version so as to get normal responses for attachment. 
  * When mails sent by v1.0 Send API are queried by v1.2 Query API, **fileType** and **attachFileIdList** of **attachFileIdList** are returned as null. 
  * When mails sent by v1.2 Send API are queried by v1.0 or v1.1 Query API, no attachment shows on **attachFileIdList**. 

### Query Request for Tagged Mail Delivery

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.2/appKeys/{appKey}/tagMails|

[Path Parameter]

|Value| Type | Descrition |
|---|---|---|
|appKey|	String| Original appkey |

[Query Parameter]

| Value         | Type    | Required | Description                                                  |
| ------------- | ------- | -------- | ------------------------------------------------------------ |
| requestId     | String  | O        | Request ID                                                   |
| startSendDate | String  | O        | Start date of delivery (yyyy-MM-dd HH:mm:ss)                 |
| endSendDate   | String  | O        | End date of delivery (yyyy-MM-dd HH:mm:ss)                   |
| senderMail    | String  | X        | Sender's mail address                                        |
| senderName    | String  | X        | Sender's name                                                |
| templateId    | String  | X        | Template ID                                                  |
| sendStatus    | String  | X        | Delivery status code <br/> WAIT: Waiting, READY: Ready, <br/>SENDREADY: Ready for Delivery, SENDWAIT: Waiting for Delivery, <br/>SENDING: Delivering, COMPLETE: Delivery Completed, <br/>FAIL: Delivery Failed, CANCEL: Delivery Canceled |
| pageNum       | Integer | X        | Page number (default: 1)                                     |
| pageSize      | Integer | X        | Number of queries (default: 15)                              |

[Caution]

* If requestId is available, startSendDate and endSendDate are not required.
* If startSendDate and endSendDate are available, requestId is not required.

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/tagMails?startSendDate=2018-03-01+00%3A00&endSendDate=2018-03-07+23%3A59&pageSize=10"
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
                "updateDate": String
            }
        ]
    }
}
```

| Value            | Type        | Description                                                  |
| ---------------- | ----------- | ------------------------------------------------------------ |
| header           | Object      | Header area                                                  |
| - isSuccessful   | Boolean     | Successful or not                                            |
| - resultCode     | Integer     | Failure code                                                 |
| - resultMessage  | String      | Failure message                                              |
| body             | Object      | Body area                                                    |
| - pageNum        | Integer     | Current page number                                          |
| -pageSize        | Integer     | Number of queried data                                       |
| - totalCount     | Integer     | Total number of data                                         |
| - data           | List        | Data area                                                    |
| -- requestId     | String      | Request ID                                                   |
| -- requestIp     | String      | Request IP                                                   |
| -- requestDate   | String      | Request time                                                 |
| -- tagSendStatus | String      | Delivery status code  <br />WAIT: Waiting, READY: Ready, <br/>SENDREADY: Ready for Delivery, SENDWAIT: Waiting for Delivery, SENDING: Delivering, COMPLETE: Delivery Completed, <br/>FAIL: Delivery Failed, CANCEL: Delivery Canceled |
| -- tagExpression | List:String | Tag expression                                               |
| -- templateId    | String      | Template ID                                                  |
| -- templateName  | String      | Template name                                                |
| -- senderName    | String      | Sender's name                                                |
| -- senderMail    | String      | Sender's address                                             |
| -- title         | String      | Title                                                        |
| -- body          | String      | Body                                                         |
| -- attachYn      | String      | Attached or not                                              |
| -- adYn          | String      | Ad or not                                                    |
| -- createUser    | String      | Creator                                                      |
| -- createDate    | String      | Date and time of creation                                    |
| -- updateUser    | String      | Modifier                                                     |
| -- updateDate    | String      | Date and time of modification                                |

### Query Recipients of Tagged Mail Delivery

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.2/appKeys/{appKey}/tagMails/{requestId}|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
|requestId|	String| Request ID |

[Query Parameter]

| Value            | Type    | Required | Description                                                  |
| ---------------- | ------- | -------- | ------------------------------------------------------------ |
| receiveMail      | String  | X        | Recipient's mail address                                     |
| startReceiveDate | String  | X        | Start date of receiving (yyyy-MM-dd HH:mm:ss)                |
| endReceiveDate   | String  | X        | End date of receiving (yyyy-MM-dd HH:mm:ss)                  |
| receiveStatus    | String  | X        | Delivery status code <br/> SST0: Preparing Delivery, SST1: Delivering,  <br/> SST2: Delivery Completed, SST3: Delivery Failed |
| pageNum          | Integer | X        | Page number (default: 1)                                     |
| pageSize         | Integer | X        | Number of queries (default: 15)                              |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/tagMails/{requestId}?startReceiveDate=2018-03-01+00%3A00&endReceiveDate=2018-03-07+23%3A59&pageSize=10"
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
                "createUser": String,
                "createDate": String,
                "updateUser": String,
                "updateDate": String
            }
        ]
    }
}
```

| Value             | Type    | Description                                                  |
| ----------------- | ------- | ------------------------------------------------------------ |
| header            | Object  | Header area                                                  |
| - isSuccessful    | Boolean | Successful or not                                            |
| - resultCode      | Integer | Failure code                                                 |
| - resultMessage   | String  | Failure message                                              |
| body              | Object  | Body area                                                    |
| - pageNum         | Integer | Current page number                                          |
| -pageSize         | Integer | Number of queried data                                       |
| - totalCount      | Integer | Total number of data                                         |
| - data            | List    | Data area                                                    |
| -- requestId      | String  | Request ID                                                   |
| -- mailSequence   | Integer | Mail sequence                                                |
| -- receiveMail    | String  | Recipient's address                                          |
| -- mailStatusCode | String  | Delivery status code <br/> SST0: Preparing Delivery, SST1: Delivering,  <br/> SST2: Delivery Completed, SST3: Delivery Failed |
| -- mailStatusName | String  | Name of mail status                                          |
| -- resultId       | String  | SMTP ID                                                      |
| -- resultDate     | String  | Time of actual delivery                                      |
| -- readYn         | String  | Read or not                                                  |
| -- readDate       | String  | Read time                                                    |
| -- createUser     | String  | Creator                                                      |
| -- createDate     | String  | Date and time of creation                                    |
| -- updateUser     | String  | Modifier                                                     |
| -- updateDate     | String  | Date and time of modification                                |

### Query Details of Tagged Mail Delivery

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.2/appKeys/{appKey}/tagMails/{requestId}/{mailSequence}|

[Path Parameter]

| Value        | Type    | Description     |
| ------------ | ------- | --------------- |
| appKey       | String  | Original appkey |
| requestId    | String  | Request ID      |
| mailSequence | Integer | Mail sequence   |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/tagMails/{requestId}/{mailSequence}"
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
        "data": {
            "requestId": String,
            "requestIp": String,
            "templateId": String,
            "templateName": String,
            "mailStatusCode": String,
            "mailStatusName": String,
            "requestDate": String,
            "resultDate": String,
            "senderName": String,
            "senderMail": String",
            "resultId": String,
            "title": String,
            "body": String",
            "receivers": [
                {
                    "requestId": String
                    "mailSequence": String
                    "receiveType": String
                    "receiveTypeName": String
                    "receiveMailAddr": String
                    "readYn": String
                    "readDate": String
                }
            ],
            "attachFileList": [
                {
                    "fileType": String,
                    "fileId": Integer
                    "fileName": String,
                    "filePath": String,
                    "fileSize": Integer,
                    "createDate": String
                }
            ],
            "customHeaders": Map
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
|- pageNum|	Integer| Current page number |
|-pageSize|	Integer| Number of queried data |
|- totalCount|	Integer| Total number of data |
|- data|	List| Data area |
|-- requestId  | String  | Request ID |
|-- requestIp | String  | Request IP |
|-- templateId | String  | Template ID |
|-- templateName | String  | Template name |
|-- mailStatusCode | String  | Mail status code <br/> SST0: Preparing Delivery, SST1: Delivering,  <br/> SST2: Delivery Completed, SST3: Delivery Failed |
|-- mailStatusName | String  | Name of mail status |
|-- requestDate | String  | Request time |
|-- resultDate | String  | Result time |
|-- senderName | String  | Sender's name |
|-- senderMail | String  | Sender's address |
|-- resultId | String  | SMTP ID |
|-- title | String  | Title |
|-- body | String  | Body |
|-- receivers | List| List of recipients |
|--- requestId | String  | Request ID |
|--- mailSequence | Integer  | Mail sequence |
|--- receiveType | String  | Type of recipients (MRT0: recipient, MRT1: Cc, MRT2: Bcc) |
|--- receiveTypeName | String  | Name of recipient type |
|--- receiveMailAddr | String  | Recipient's mail address |
|--- readYn | String  | Read or not |
|--- readDate | String  | Read time |
|-- attachFileList | List  | List of attached files |
|--- fileType|	String| Type of attached file (MAIL: files attached to mail, TEMPLATE: files attached to template) |
|--- fileId| String| File ID |
|--- fileName|	String| Name of attached file |
|--- filePath|	String| Path of attached file |
|--- fileSize|	Integer| Size of attached file (byte) |
|--- createDate|	String| Date and time of creation |
|-- customHeaders|	Map|	[Custom Header](./Overview/#custom-header) |

#### Changes in v 1.2

- Response has changed. 
- The v 1.0 Upload Attachment API returned the string-format **requestId** for delivery, while the v 1.2 Upload Attachment API returns the Integer-format **attachFileIdList**. 
- The  **attachFileIdList** of v1.2 Send API can have many on the list.  
- Send API and Query API must be of the same version so as to get normal responses for attachment. 
  - When mails sent by v1.0 Send API are queried by v1.2 Query API, **fileType** and **attachFileIdList** of **attachFileIdList** are returned as null. 
  - When mails sent by v1.2 Send API are queried by v1.0 or v1.1 Query API, no attachment shows on **attachFileIdList**. 

## Query of Templates

### Query List of Templates

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.2/appKeys/{appKey}/templates|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query Parameter]

| Value      | Type    | Required | Description                        |
| ---------- | ------- | -------- | ---------------------------------- |
| categoryId | Integer | X        | Category ID                        |
| useYn      | String  | X        | Use or Not (Y/N)                   |
| pageNum    | Integer | X        | Page number (default: 1)           |
| pageSize   | Integer | X        | Number of queries (default: 15)    |
| all        | Boolean | X        | Query list of all templates or not |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/templates?useYn=Y&pageNum=1&pageSize=10"
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
        "data": [{
            "templateId": String,
            "serviceId": Integer,
            "categoryId": Integer,
            "categoryName": String,
            "sort": Integer,
            "templateName": String,
            "templateDesc": String,
            "useYn": String,
            "sendMailAddress": String,
            "sendType": String,
            "sendTypeName": String,
            "title": String,
            "body": String,
            "attachFileYn": String,
            "delYn": String,
            "createDate": String,
            "createUser": String,
            "updateDate": String,
            "updateUser": String,
}]
    }
}
```

| Value             | Type    | Description                         |
| ----------------- | ------- | ----------------------------------- |
| header            | Object  | Header area                         |
| - isSuccessful    | Boolean | Successful or not                   |
| - resultCode      | Integer | Failure code                        |
| - resultMessage   | String  | Failure message                     |
| body              | Object  | Body area                           |
| - pageNum         | Integer | Current page number                 |
| -pageSize         | Integer | Number of queried data              |
| - totalCount      | Integer | Total number of data                |
| -data             | List    | Data area                           |
| --templateId      | String  | Template ID                         |
| --serviceId       | Integer | Service ID                          |
| --categoryId      | Integer | Category ID                         |
| --categoryName    | String  | Category name                       |
| --sort            | Integer | Sequence                            |
| --templateName    | String  | Template name                       |
| --templateDesc    | String  | Template description                |
| -- useYn          | String  | Use or not                          |
| --sendMailAddress | String  | Sender's mail address for templates |
| --title           | String  | Mail title                          |
| --body            | String  | Mail body                           |
| --attachFileYn    | String  | Add template attachment or not      |
| --delYn           | String  | Delete or not                       |
| --createDate      | String  | Date and time of creation           |
| --updateDate      | String  | Date and time of modification       |

### Query Template Details

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.2/appKeys/{appKey}/templates/{templateId}|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
|templateId|	String| Template ID |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/templates/{templateId}"
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
    "data": [{
            "templateId": String,
            "serviceId": Integer,
            "categoryId": Integer,
            "categoryName": String,
            "templateName": String,
            "templateDesc": String,
            "useYn": String,
            "sendMailAddress": String,
            "title": String,
            "body": String,
            "delYn": String,
            "createDate": String,
            "createUser": String,
            "updateDate": String,
            "updateUser": String,
            "attachFileList": [{
                "fileId": Integer,
                "serviceId" : Integer,
                "templateId": String,
                "filePath": String,
                "fileName": String,
                "fileSize" : Integer,
                "createDate": String,
                "createUser": String,
                "updateDate" : String,
                "updateUser" : String
            }]
        }]
    }
}
```

| Value             | Type    | Description                           |
| ----------------- | ------- | ------------------------------------- |
| header            | Object  | Header area                           |
| - isSuccessful    | Boolean | Successful or not                     |
| - resultCode      | Integer | Failure code                          |
| - resultMessage   | String  | Failure message                       |
| body              | Object  | Body area                             |
| -data             | List    | Data area                             |
| --templateId      | String  | Template ID                           |
| --serviceId       | Integer | Service ID                            |
| --categoryId      | Integer | Category ID                           |
| --categoryName    | String  | Category name                         |
| --templateName    | String  | Template name                         |
| --templateDesc    | String  | Template description                  |
| --useYn           | String  | Use or not (Y: In use, N: Not in use) |
| --sendMailAddress | String  | Sender's mail address                 |
| --title           | String  | Mail address                          |
| --body            | String  | Mail body                             |
| --delYn           | String  | Delete or not                         |
| --createDate      | String  | Date and time of creation             |
| --createUser      | String  | Creator                               |
| --updateDate      | String  | Date and time of modification         |
| --updateUser      | String  | Modifier                              |
| --attachFileList  | List    | List of attached files                |
| ---fileId         | Integer | Attachment ID                         |
| ---serviceId      | Integer | Service ID                            |
| ---templateId     | String  | Template ID                           |
| ---filePath       | String  | Path of attached file                 |
| ---fileName       | String  | Name of attached file                 |
| ---fileSize       | Integer | Size of attached file                 |
| ---createDate     | String  | Date and time of creation             |
| ---createUser     | String  | Creator                               |
| ---updateDate     | String  | Date and time of modification         |
| ---updateUser     | String  | Modifier                              |

## Tag Management

### Query Tags

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.2/appKeys/{appKey}/tags|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query Parameter]

|Value| Type | Required | Description |
|---|---|---|---|
|pageNum|	Integer|	X| Page number (Default : 1) |
|pageSize|	Integer|	X| Number of queries (Default : 15) |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/tags?pageNum=1&pageSize=10"
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
                "tagId": String,
                "tagName": String,
                "createdDate": String,
                "updatedDate": String
            }
        ]
    }
}
```

| Value           | Type    | Description                   |
| --------------- | ------- | ----------------------------- |
| header          | Object  | Header area                   |
| - isSuccessful  | Boolean | Successful or not             |
| - resultCode    | Integer | Failure code                  |
| - resultMessage | String  | Failure message               |
| body            | Object  | Body area                     |
| - data          | List    | Data area                     |
| -- tagId        | String  | Tag ID                        |
| -- tagName      | String  | Tag name                      |
| -- createdDate  | String  | Date and time of creation     |
| -- updatedDate  | String  | Date and time of modification |

### Register Tags

#### Request

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.2/appKeys/{appKey}/tags|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request Body]

|Value| Type | Required | Description |
|---|---|---|---|
|tagName|	String|	O| Tag name |

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/tags -d '{"tagName":"sample tag"}'
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
        "data": {
            "tagId": String
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
| - data          | List    | Data area         |
| -- tagId        | String  | Tag ID            |

### Modify Tags

#### Request

[URL]

|Http method|	URI|
|---|---|
|PUT|	/email/v1.2/appKeys/{appKey}/tags/{tagId}|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
|tagId|	String| Tag ID |

[Request Body]

|Value| Type | Required | Description |
|---|---|---|---|
|tagName|	String|	O| Tag name |

[Example]
```
curl -X PUT -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/tags/{tagId} -d '{"tagName":"sample tag2"}'
```

#### Response

```
{
    "header": {
        "isSuccessful": Boolean,
        "resultCode": Integer,
        "resultMessage": String
    }
}
```

| Value           | Type    | Description       |
| --------------- | ------- | ----------------- |
| header          | Object  | Header area       |
| - isSuccessful  | Boolean | Successful or not |
| - resultCode    | Integer | Failure code      |
| - resultMessage | String  | Failure message   |

### Delete Tags

#### Request

[URL]

|Http method|	URI|
|---|---|
|DELETE|	/email/v1.2/appKeys/{appKey}/tags/{tagId}|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
|tagId|	String| Tag ID |

[Example]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/tags/{tagId}
```

#### Response

```
{
    "header": {
        "isSuccessful": Boolean,
        "resultCode": Integer,
        "resultMessage": String
    }
}
```

| Value           | Type    | Description       |
| --------------- | ------- | ----------------- |
| header          | Object  | Header area       |
| - isSuccessful  | Boolean | Successful or not |
| - resultCode    | Integer | Failure code      |
| - resultMessage | String  | Failure message   |

## UID Management

### Query UIDs

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.2/appKeys/{appKey}/uids|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query Parameter]

| Value     | Type        | Required | Description                                                  |
| --------- | ----------- | -------- | ------------------------------------------------------------ |
| wheres    | List:String | X        | Query conditions.</br> Character strings comprised of alphabets, numbers, and parentheses. </br>Allows one parenthesis, and up to three AND or ORs.</br> e.g.) tagId1,AND,tagId2 |
| offsetUid | String      | X        | offset UID                                                   |
| offset    | Integer     | X        | offset (default: 0)                                          |
| limit     | Integer     | X        | Number of queries (default: 15)                              |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/uids?wheres=tagId1,OR,tagId2&offset=0&limit=10"
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
        "data": {
            "uids": [
                {
                    "uid": String,
                    "tags": [
                        {
                            "tagId": String,
                            "tagName": String,
                            "createDate": String,
                            "updateDate": String
                        }
                    ],
                    "contacts": [
                        {
                            "contactType": String,
                            "contact": String,
                            "createDate": String"
                        }
                    ]
                }
            ],
            "isLast": Boolean,
            "totalCount": Integer
        }
    }
}
```

| Value            | Type    | Description                       |
| ---------------- | ------- | --------------------------------- |
| header           | Object  | Header area                       |
| - isSuccessful   | Boolean | Successful or not                 |
| - resultCode     | Integer | Failure code                      |
| - resultMessage  | String  | Failure message                   |
| body             | Object  | Body area                         |
| - data           | List    | Data area                         |
| -- uids          | List    | List of UIDs                      |
| --- uid          | String  | UID                               |
| --- tags         | List    | List of tag information           |
| ---- tagId       | String  | Tag ID                            |
| ---- tagName     | String  | Tag name                          |
| ---- createDate  | String  | Date and time of tag creation     |
| ---- updateDate  | String  | Date and time of tag modification |
| --- contacts     | List    | List of contact                   |
| ---- contactType | String  | Contact type (EMAIL_ADDRESS)      |
| ---- contact     | String  | Contact (mail address)            |
| ---- createDate  | String  | Date and time of contact creation |
| -- isLast        | Boolean | Last on list or not               |
| -- totalCount    | Integer | Total number of data              |

### Get UIDs

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.2/appKeys/{appKey}/uids/{uid}|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
|uid|	String|	UID |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/uids/{uid}"
```

#### Response

```
{
    "header": {
        "isSuccessful": String,
        "resultCode": Integer,
        "resultMessage": String
    },
    "body": {
        "data": {
            "uid": "",
            "tags": [
                {
                    "tagId": String,
                    "tagName": String,
                    "createDate": String,
                    "updateDate": String
                }
            ],
            "contacts": [
                {
                    "contactType": String,
                    "contact": String,
                    "createDate": String
                }
            ]
        }
    }
}
```

| Value           | Type    | Description                       |
| --------------- | ------- | --------------------------------- |
| header          | Object  | Header area                       |
| - isSuccessful  | Boolean | Successful or not                 |
| - resultCode    | Integer | Failure code                      |
| - resultMessage | String  | Failure message                   |
| body            | Object  | Body area                         |
| - data          | List    | Data area                         |
| -- uid          | String  | UID                               |
| -- tags         | List    | List of tag information           |
| --- tagId       | String  | Tag ID                            |
| --- tagName     | String  | Tag name                          |
| --- createDate  | String  | Date and time of tag creation     |
| --- updateDate  | String  | Date and time of tag modification |
| -- contacts     | List    | List of contacts                  |
| --- contactType | String  | Contact type                      |
| --- contact     | String  | Contact (mail address)            |
| --- createDate  | String  | Date and time of contact creation |

### Register UIDs

#### Request

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.2/appKeys/{appKey}/uids|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request Body]

| Value        | Type        | Require | Description            |
| ------------ | ----------- | ------- | ---------------------- |
| uid          | String      | O       | UID                    |
| tagIds       | List:String | O       | List of tag IDs        |
| contacts     | List        | O       | List of mail addresses |
| -contactType | String      | O       | Contact type           |
| -contact     | String      | O       | Contact (mail address) |

[Caution]

* When tagIds is provided, contacts is not required.
* When contacts is provided, tagIds is not required.
* For this product, contactType must be requested in the "EMAIL_ADDRESS" value.

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/uids -d '{"uids":[{"uid":"sample-uid","tagIds":["tagId1"],"contacts":[{"contactType":"EMAIL_ADDRESS","contact":"customer1@nhnent.com"},{"contactType":"EMAIL_ADDRESS","contact":"customer2@nhnent.com"}]}]}'
```

#### Response

```
{
    "header": {
        "isSuccessful": Boolean,
        "resultCode": Integer,
        "resultMessage": String
    }
}
```

| Value           | Type    | Description       |
| --------------- | ------- | ----------------- |
| header          | Object  | Header area       |
| - isSuccessful  | Boolean | Successful or not |
| - resultCode    | Integer | Failure code      |
| - resultMessage | String  | Failure message   |

### Delete UIDs

#### Request

[URL]

|Http method|	URI|
|---|---|
|DELETE|	/email/v1.2/appKeys/{appKey}/uids/{uid}|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
|uid|	String|	UID|

[Example]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/uids/{uid}
```

#### Response

```
{
    "header": {
        "isSuccessful": Boolean,
        "resultCode": Integer,
        "resultMessage": String
    }
}
```

| Value           | Type    | Description       |
| --------------- | ------- | ----------------- |
| header          | Object  | Header area       |
| - isSuccessful  | Boolean | Successful or not |
| - resultCode    | Integer | Failure code      |
| - resultMessage | String  | Failure message   |

### Register Mail Addresses

#### Request

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.2/appKeys/{appKey}/uids/{uid}/email-addresses|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
|uid|	String|	UID|

[Request Body]

|Value| Type | Required | Description |
|---|---|---|---|
|emailAddress|	String|	O| Mail address |

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/uids/{uid}/email-addresses -d '{"emailAddress" : "customer1@nhnent.com"}'
```

#### Response

```
{
    "header": {
        "isSuccessful": Boolean,
        "resultCode": Integer,
        "resultMessage": String
    }
}
```

| Value           | Type    | Description       |
| --------------- | ------- | ----------------- |
| header          | Object  | Header area       |
| - isSuccessful  | Boolean | Successful or not |
| - resultCode    | Integer | Failure code      |
| - resultMessage | String  | Failure message   |

### Delete Mail Addresses

#### Request

[URL]

|Http method|	URI|
|---|---|
|DELETE|	/email/v1.2/appKeys/{appKey}/uids/{uid}/email-addresses/{emailAddress}|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
|uid|	String|	UID|
|emailAddress|	String| Mail address |

[Example]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/uids/{uid}/email-addresses/customer1@nhnent.com
```

#### Response

```
{
    "header": {
        "isSuccessful": Boolean,
        "resultCode": Integer,
        "resultMessage": String
    }
}
```

| Value           | Type    | Description       |
| --------------- | ------- | ----------------- |
| header          | Object  | Header area       |
| - isSuccessful  | Boolean | Successful or not |
| - resultCode    | Integer | Failure code      |
| - resultMessage | String  | Failure message   |

## Query Statistics

### Query Daily Statistics

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.2/appKeys/{appKey}/statistics/daily

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query Parameter]

| Value   | Type   | Required | Description                                                  |
| ------- | ------ | -------- | ------------------------------------------------------------ |
| from    | String | O        | Start date of statistical query (year-month-day)<br/>(yyyy-MM-dd) |
| to      | String | O        | End date of statistical query (year-month-day)<br/>(yyyy-MM-dd) |
| type    | String | X        | Query type<br/>* REQUEST:Request<br/>* SENT: Sent<br/>* RECEIVED :Received<br/>* OPENED: Opened |
| filters | String | X        | Search conditions (multiple parameters are available)<br/>* MAILTYPE: Statistics per mail type (general/mass)<br/>* TEMPLATE: Statistics per template |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/statistics/daily?from=2018-03-01&to=2018-03-07&type=OPENED&filters=MAILTYPE&filters=TEMPLATE"
```

#### Response

```
{
    "isSuccessful": Boolean,
    "resultCode": Integer,
    "resultMessage": String
},
    "body" : {
        "data" : [
        {
            "ymd" :  String,
            "appKey" :  String,
            "type" :  String,
            "mailType" :  String,
            "templateId" :  String,
            "templateName" :  String,
            "tm00" :  int,
            "tm01" :  int,
            "tm02" :  int,
            "tm03" :  int,
            "tm04" :  int,
            "tm05" :  int,
            "tm06" :  int,
            "tm07" :  int,
            "tm08" :  int,
            "tm09" :  int,
            "tm10" :  int,
            "tm11" :  int,
            "tm12" :  int,
            "tm13" :  int,
            "tm14" :  int,
            "tm15" :  int,
            "tm16" :  int,
            "tm17" :  int,
            "tm18" :  int,
            "tm19" :  int,
            "tm20" :  int,
            "tm21" :  int,
            "tm22" :  int,
            "tm23" :  int,
            "sum" : int
        }
        ]
    }
}
```

| Value           | Type    | Description                                                  |
| --------------- | ------- | ------------------------------------------------------------ |
| header          | Object  | Header area                                                  |
| - isSuccessful  | Boolean | Successful or not                                            |
| - resultCode    | Integer | Failure code                                                 |
| - resultMessage | String  | Failure message                                              |
| body            | Object  | Body area                                                    |
| - data          | List    | Data area                                                    |
| -- ymd          | String  | Date and time of statistics  (yyyyMMdd)                      |
| -- appKey       | String  | Appkey                                                       |
| -- type         | String  | Type of statistical query <br/>REQUEST: Request<br/>SENT: Sent<br/>RECEIVED: Received<br/>OPENED: Opened |
| -- mailType     | String  | Mail type<br/>* MAILTYPE: Statistics per mail type (general/mass) <br/>* TEMPLATE: Statistics per template |
| -- templateId   | String  | Template ID                                                  |
| -- templateName | String  | Template name                                                |
| -- tm00         | int     | 00:00:00 ~ 00:59:59                                          |
| -- tm01         | int     | 01:00:00 ~ 01:59:59                                          |
| -- tm02         | int     | 02:00:00 ~ 02:59:59                                          |
| -- tm03         | int     | 03:00:00 ~ 03:59:59                                          |
| -- tm04         | int     | 04:00:00 ~ 04:59:59                                          |
| -- tm05         | int     | 05:00:00 ~ 05:59:59                                          |
| -- tm06         | int     | 06:00:00 ~ 06:59:59                                          |
| -- tm07         | int     | 07:00:00 ~ 07:59:59                                          |
| -- tm08         | int     | 08:00:00 ~ 08:59:59                                          |
| -- tm09         | int     | 09:00:00 ~ 09:59:59                                          |
| -- tm10         | int     | 10:00:00 ~ 10:59:59                                          |
| -- tm11         | int     | 11:00:00 ~ 11:59:59                                          |
| -- tm12         | int     | 12:00:00 ~ 12:59:59                                          |
| -- tm13         | int     | 13:00:00 ~ 13:59:59                                          |
| -- tm14         | int     | 14:00:00 ~ 14:59:59                                          |
| -- tm15         | int     | 15:00:00 ~ 15:59:59                                          |
| -- tm16         | int     | 16:00:00 ~ 16:59:59                                          |
| -- tm17         | int     | 17:00:00 ~ 17:59:59                                          |
| -- tm18         | int     | 18:00:00 ~ 18:59:59                                          |
| -- tm19         | int     | 19:00:00 ~ 19:59:59                                          |
| -- tm20         | int     | 20:00:00 ~ 20:59:59                                          |
| -- tm21         | int     | 21:00:00 ~ 21:59:59                                          |
| -- tm22         | int     | 22:00:00 ~ 22:59:59                                          |
| -- tm23         | int     | 23:00:00 ~ 23:59:59                                          |
| -- sum          | int     | Total                                                        |

### Query Monthly Statistics

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.2/appKeys/{appKey}/statistics/monthly |

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query Parameter]

| Value   | Type   | Required | Description                                                  |
| ------- | ------ | -------- | ------------------------------------------------------------ |
| from    | String | O        | Start date of statistical query (year-month)<br/>(yyyy-MM)   |
| to      | String | O        | End date of statistical query (year-month)<br/>(yyyy-MM)     |
| type    | String | X        | Query type<br/>REQUEST: Request<br/>SENT: Sent <br/>RECEIVED: Received <br/>OPENED: Opened |
| filters | String | X        | Search conditions (multiple parameters are available)<br/>* MAILTYPE: Statistics per mail type (general/mass)<br/>* TEMPLATE: Statistics per template |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/statistics/monthly?from=2018-03&to=2018-04&type=OPENED&filters=MAILTYPE&filters=TEMPLATE"
```

#### Response

```json
{
    "isSuccessful": Boolean,
    "resultCode": Integer,
    "resultMessage": String
},
    "body" : {
        "data" : [
        {
            "ymd" :  String,
            "appKey" :  String,
            "type" :  String,
            "mailType" :  String,
            "templateId" :  String,
            "templateName" :  String,
            "tm00" :  int,
            "tm01" :  int,
            "tm02" :  int,
            "tm03" :  int,
            "tm04" :  int,
            "tm05" :  int,
            "tm06" :  int,
            "tm07" :  int,
            "tm08" :  int,
            "tm09" :  int,
            "tm10" :  int,
            "tm11" :  int,
            "tm12" :  int,
            "tm13" :  int,
            "tm14" :  int,
            "tm15" :  int,
            "tm16" :  int,
            "tm17" :  int,
            "tm18" :  int,
            "tm19" :  int,
            "tm20" :  int,
            "tm21" :  int,
            "tm22" :  int,
            "tm23" :  int,
            "sum" : int
        }
        ]
    }
}
```

| Value           | Type    | Description                                                  |
| --------------- | ------- | ------------------------------------------------------------ |
| header          | Object  | Header area                                                  |
| - isSuccessful  | Boolean | Successful or not                                            |
| - resultCode    | Integer | Failure code                                                 |
| - resultMessage | String  | Failure message                                              |
| body            | Object  | Body area                                                    |
| - data          | List    | Data area                                                    |
| -- ymd          | String  | Date of statistics (yyyyMMdd)                                |
| -- appKey       | String  | Appkey                                                       |
| -- type         | String  | Type of statistical query <br/>* REQUEST: Request <br/>* SENT: Sent <br/>* RECEIVED: Received <br/>* OPENED: Opened |
| -- mailType     | String  | Mail type<br/>* MAILTYPE: Statistics per mail type (general/mass) <br/>* TEMPLATE: Statistics per template |
| -- templateId   | String  | Template ID                                                  |
| -- templateName | String  | Template name                                                |
| -- tm00         | int     | 00:00:00 ~ 00:59:59                                          |
| -- tm01         | int     | 01:00:00 ~ 01:59:59                                          |
| -- tm02         | int     | 02:00:00 ~ 02:59:59                                          |
| -- tm03         | int     | 03:00:00 ~ 03:59:59                                          |
| -- tm04         | int     | 04:00:00 ~ 04:59:59                                          |
| -- tm05         | int     | 05:00:00 ~ 05:59:59                                          |
| -- tm06         | int     | 06:00:00 ~ 06:59:59                                          |
| -- tm07         | int     | 07:00:00 ~ 07:59:59                                          |
| -- tm08         | int     | 08:00:00 ~ 08:59:59                                          |
| -- tm09         | int     | 09:00:00 ~ 09:59:59                                          |
| -- tm10         | int     | 10:00:00 ~ 10:59:59                                          |
| -- tm11         | int     | 11:00:00 ~ 11:59:59                                          |
| -- tm12         | int     | 12:00:00 ~ 12:59:59                                          |
| -- tm13         | int     | 13:00:00 ~ 13:59:59                                          |
| -- tm14         | int     | 14:00:00 ~ 14:59:59                                          |
| -- tm15         | int     | 15:00:00 ~ 15:59:59                                          |
| -- tm16         | int     | 16:00:00 ~ 16:59:59                                          |
| -- tm17         | int     | 17:00:00 ~ 17:59:59                                          |
| -- tm18         | int     | 18:00:00 ~ 18:59:59                                          |
| -- tm19         | int     | 19:00:00 ~ 19:59:59                                          |
| -- tm20         | int     | 20:00:00 ~ 20:59:59                                          |
| -- tm21         | int     | 21:00:00 ~ 21:59:59                                          |
| -- tm22         | int     | 22:00:00 ~ 22:59:59                                          |
| -- tm23         | int     | 23:00:00 ~ 23:59:59                                          |
| -- sum          | int     | Total                                                        |

### Query Integrated Statistics

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.2/appKeys/{appKey}/statistics/view |

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query Parameter]

| Value      | Type   | Required | Description                                                  |
| ---------- | ------ | -------- | ------------------------------------------------------------ |
| from       | String | O        | Start date of statistical query <br/> yyyy-mm-dd HH:mm       |
| to         | String | O        | End date of statistical query <br/> yyyy-mm-dd HH:mm         |
| searchType | String | O        | Category of statistics <br/>DATE: By date, TIME: By time, DAY: By day |
| mailTypes  | String | X        | Mail type<br/>NORMAL: General, MASS: Mass<br/>Multiple entries are available (mailTypes=NORMAL&mailTypes=MASS) |
| adYn       | String | X        | Ad or not<br>Y:Ad, N: Not an ad<br>All, if left blank        |
| templateId | String | X        | Template ID                                                  |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/statistics/view?from=2018-03-21+00%3A00&to=2018-03-23+00%3A00&searchType=DATE&mailTypes=NORMAL&adYn=Y&templateId=templateId1"
```

#### Response

```json
{
    "isSuccessful": Boolean,
    "resultCode": Integer,
    "resultMessage": String
},
    "body" : {
        "data" : [
          {
                  "divisionName": String,
                  "requestedCount": long,
                  "sentCount": long,
                  "receivedCount": long,
                  "openedCount": long,
                  "sentRate": String,
                  "receivedRate": String,
                  "openedRate": String
        }
        ]
    }
}
```

| Value             | Type    | Description                          |
| ----------------- | ------- | ------------------------------------ |
| header            | Object  | Header area                          |
| - isSuccessful    | Boolean | Successful or not                    |
| - resultCode      | Integer | Failure code                         |
| - resultMessage   | String  | Failure message                      |
| body              | Object  | Body area                            |
| - data            | List    | Data area                            |
| -- divisionName   | String  | Statistical criteria (date/time/day) |
| -- requestedCount | Long    | Number of delivery requests          |
| -- sentCount      | Long    | Number of deliveries                 |
| -- receivedCount  | Long    | Number of receipts                   |
| -- openedCount    | Long    | Number of openings                   |
| -- sentRate       | String  | Delivery rate                        |
| -- receivedRate   | String  | Receiving rate                       |
| -- openedRate     | String  | Opening rate                         |

## Rejection Management

### Query Rejections

#### Request

[URL]

|Http method|	URI|
|---|---|
| GET |	/email/v1.2/appKeys/{appKey}/block-receivers |

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query Parameter]

| Value       | Type    | Required | Description                                        |
| ----------- | ------- | -------- | -------------------------------------------------- |
| mailAddress | String  | X        | Email address registered on the list of rejections |
| pageNum     | Integer | X        | Page number (default: 1)                           |
| pageSize    | Integer | X        | Number of queries (default: 15)                    |

[Example]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/block-receivers?mailAddress=customer1@nhnent.com&pageNum=1&pageSize=10"
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
        "totalCount": Integer,
        "data": [
            {
                "mailAddress": String,
                "blockDate": String
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

### Register Rejections

#### Request

[URL]

|Http method|	URI|
|---|---|
| POST |	/email/v1.2/appKeys/{appKey}/block-receivers |

[Request Body]

| Value             | Type   | Required | Description                             |
| ----------------- | ------ | -------- | --------------------------------------- |
| blockReceiverList | ㅣList | O        | List of rejections                      |
| - mailAddress     | String | O        | Email address to reject                 |
| - blockDate       | String | X        | Date of rejection (yyyy-MM-dd HH:mm:ss) |

[Example]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/block-receivers -d '{"blockReceiverList":[{"mailAddress":"customer1@nhnent.com","blockDate":"2018-03-01 00:00:00"}]}'
```

#### Response
```
{
  "header": {
    "isSuccessful":  Boolean,
    "resultCode": Integer,
    "resultMessage": String
  }
}
```
| Value           | Type    | Description       |
| --------------- | ------- | ----------------- |
| header          | Object  | Header area       |
| - isSuccessful  | Boolean | Successful or not |
| - resultCode    | Integer | Failure code      |
| - resultMessage | String  | Failure message   |

### Delete Rejections

#### Request

[URL]

|Http method|	URI|
|---|---|
| PUT |	/email/v1.2/appKeys/{appKey}/block-receivers |

[Request Body]

| Value             | Type    | Required | Description                             |
| ----------------- | ------- | -------- | --------------------------------------- |
| deleted           | Boolean | O        | Field specifying rejections are deleted |
| blockReceiverList | ㅣList  | O        | List of rejections                      |
| - mailAddress     | String  | O        | Email address rejecting ads             |

[Example]
```
curl -X PUT -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.2/appKeys/{appKey}/block-receivers -d '{"deleted":true,"blockReceiverList":[{"mailAddress":"customer1@nhnent.com"}]}'
```

#### Response
```
{
  "header": {
    "isSuccessful":  Boolean,
    "resultCode": Integer,
    "resultMessage": String
  }
}
```
| Value           | Type    | Description       |
| --------------- | ------- | ----------------- |
| header          | Object  | Header area       |
| - isSuccessful  | Boolean | Successful or not |
| - resultCode    | Integer | Failure code      |
| - resultMessage | String  | Failure message   |