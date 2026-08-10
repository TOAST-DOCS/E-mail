<!-- pre-align:aligned sig=d462b46f3021 -->

<a id="notification-email-api-v15-guide"></a>
## Notification > Email > API v1.5 Guide { #notification-email-api-v15-guide }

[API Domain]

|Environment| Domain |
|---|---|
|Real|	https://email.api.nhncloudservice.com |

[Header]
```
Content-Type: application/json;charset=UTF-8
```

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
|POST|	/email/v1.5/appKeys/{appKey}/sender/mail|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

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


[Caution]

* To use a template, title and body are not required (input values, if available, precede template values)

[Example 1]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/mail -d '{"senderAddress":"support@nhnent.com","senderName":"sender's name","title":"sample title","body":"sample body","receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhnent.com","receiveName":"customer2","receiveType":"MRT1"}],"userId":"USER"}'
```

[Example 2 - Using Templates]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/mail -d '{"templateId":"TEMPLATE1","templateParameter":{"key":"value"},"receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhnent.com","receiveName":"customer2","receiveType":"MRT1"}],"userId":"USER"}'
```

<a id="send-general-mails-response"></a>
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
            "results": [
                {
                    "receiveMailAddr": String,
                    "receiveName": String,
                    "receiveType": String,
                    "resultCode": Integer,
                    "resultMessage": String
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

<a id="send-general-mails-updated-for-v15"></a>
#### Updated for v1.5

* Added the Sender Group Key field. Setting is available by request.  
* When it is requested for delivery, specify **senderGroupingKey** field to be made available to query request. 

<a id="send-individual-mails"></a>
### Send Individual Mails { #send-individual-mails }

* Mails can be sent to each of many recipients. Even if a same mail is sent to many recipients, each recipient can find his or her name only.  

<a id="send-individual-mails-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/sender/eachMail||

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

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

[Caution]

* To use a template, title and body are not required (input values, if available, precede template values)


[Example 1]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/eachMail -d '{"senderAddress":"support@nhnent.com","senderName":"sender's name","title":"sample title","body":"sample body","attachFileIdList":[1, 2],"receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1"}],"userId":"USER"}'
```

[Example 2 - Using Templates]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/mail -d '{"templateId":"TEMPLATE1","receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1","templateParameter":{"key":"value"}}],"userId":"USER"}'
```


<a id="send-individual-mails-response"></a>
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
            "results": [
                {
                    "receiveMailAddr": String,
                    "receiveName": String,
                    "receiveType": String,
                    "resultCode": Integer,
                    "resultMessage": String
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

<a id="send-individual-mails-updated-for-v15"></a>
#### Updated for v1.5 

* Added the Sender Group Key field. Setting is available by request.  
* When it is requested for delivery, specify **senderGroupingKey** field to be made available to query request. 

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
|POST|	/email/v1.5/appKeys/{appKey}/sender/ad-mail|

[Example 1]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/ad-mail -d '{"senderAddress":"support@nhnent.com","senderName":"sender's name","title":"(AD)sample title","body":"sample body <br /> ##BLOCK_RECEIVER_LINK## <br />##EN_BLOCK_RECEIVER_LINK##","receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhnent.com","receiveName":"customer2","receiveType":"MRT1"}],"userId":"USER"}'
```

[Example 2 - Using Templates]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/ad-mail -d '{"templateId":"TEMPLATE1","templateParameter":{"key":"value"},"receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhnent.com","receiveName":"customer2","receiveType":"MRT1"}],"userId":"USER"}'
```

<a id="sending-individual-ad-mails"></a>
### Sending Individual Ad Mails { #sending-individual-ad-mails }

* Request and response information is same as in sending individual mails.  

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/sender/ad-eachMail |

[Example 1]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/ad-eachMail -d '{"senderAddress":"support@nhnent.com","senderName":"sender's name","title":"(AD) sample title","body":"sample body <br /> ##BLOCK_RECEIVER_LINK## <br />##EN_BLOCK_RECEIVER_LINK##","attachFileIdList":[1, 2],"receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1"}],"userId":"USER"}'
```

[Example 2 - Using Templates]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/ad-eachMail -d '{"templateId":"TEMPLATE1","receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1","templateParameter":{"key":"value"}}],"userId":"USER"}'
```

<a id="send-authenticated-mails"></a>
### Send Authenticated Mails { #send-authenticated-mails }

<a id="send-authenticated-mails-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/sender/auth-mail||

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

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
| userId              | String | X        | Delimiter for delivery e.g.) admin,system                    |

[Caution]

- To use a template, title and body are not required (input values, if available, precede template values)

<a id="send-authenticated-mails-differences-from-general-mails"></a>
#### Differences from General Mails 

Features of authenticated mails are as follows: 

- Allows only single-case deliveries (one recipient).
- File attachment is not supported. Templates including attachment are not supported. 

[Example 1]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/auth-mail -d '{"senderAddress":"support@nhnent.com","senderName":"sender's name","title":"sample title","body":"sample body","receiver":{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1"},"userId":"USER"}'
```

[Example 2 - Using Templates]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/auth-mail -d '{"templateId":"TEMPLATE1","receiver":{"receiveMailAddr":"customer1@nhnent.com","receiveName":"customer1","templateParameter":{"key":"value"}},"userId":"USER"}'
```

<a id="send-authenticated-mails-response"></a>
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

| Value           | Type    | Description                                                  |
| --------------- | ------- | ------------------------------------------------------------ |
| header          | Object  | Header area                                                  |
| - isSuccessful  | Boolean | Successful or not                                            |
| - resultCode    | Integer | Failure code                                                 |
| - resultMessage | String  | Failure message                                              |
| body            | Object  | Body area                                                    |
| - data          | Object  | Data area                                                    |
| -- requestId    | String  | Request ID                                                   |
| -- statusCode   | String  | Request status code (Y: preparing for sending , N: Preparing for sending failed) |
<a id="upload-attached-files"></a>
### Upload Attached Files { #upload-attached-files }

<a id="upload-attached-files-request"></a>
#### Request 

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/attachfile/binaryUpload|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

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
| createUser | String | X        | File uploader information |

[Example]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/attachfile/binaryUpload -d '{"fileName":"file.csv","createUser":"USER","fileBody":[]}'
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
```
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

<a id="query-of-mails"></a>
## Query of Mails { #query-of-mails }

<a id="query-list-of-mail-deliveries"></a>
### Query List of Mail Deliveries { #query-list-of-mail-deliveries }

<a id="query-list-of-mail-deliveries-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/sender/mails|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

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

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/mails?startSendDate=2018-03-01+00%3A00&endSendDate=2018-03-07+23%3A59&pageSize=10"
```

<a id="query-list-of-mail-deliveries-response"></a>
#### Response

``` json
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
                "senderGroupingKey": "A_GROUP"
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

<a id="query-list-of-mail-deliveries-updated-for-v15"></a>
#### Updated for v1.5

* Added **senderGroupingKey**, the sender group key field.
* Added **isReceived**, the received or not field.
* Added **isOpened**, the opened or not field.
* Added **openedDate**, date and time of opening field.
* Changed the field name of delivery status code into **mailStatusCode**.
* Changed the field name of recipient's mail address into **receiveMailAddr**.
* Changed the field name of sender's mail address into **senderAddress**.


<a id="query-mail-delivery-details"></a>
### Query Mail Delivery Details { #query-mail-delivery-details }

<a id="query-mail-delivery-details-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/sender/mail/{requestId}/{mailSeq}|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|requestId|	String| Request ID |
|mailSeq|	Integer| Mail sequence |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/mail/{requestId}/{mailSeq}"
```

<a id="query-mail-delivery-details-response"></a>
#### Response

```
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
                    "openedDate": "2019-01-01 00:00:00"
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
| -- attachFileList   | List    | List of attached files                                       |
| --- fileType        | String  | Type of attached files <br/>MAIL: files attached to mail, TEMPLATE: files attached to template|
| --- fileId          | Integer  | File ID                                                      |
| --- fileName        | String  | Name of attached file                                        |
| --- filePath        | String  | Path of attached file                                        |
| --- fileSize        | Integer | Size of attached file (byte)                                 |
| --- createDate      | String  | Date and time of creation                                    |
| -- customHeaders    | Map     | [Custom Header](./console-guide/#custom-header)                   |
| -- senderGroupingKey|	String  | Sender's group key                                           |

<a id="query-mail-delivery-details-updated-for-v15"></a>
#### Updated for v1.5

* Added **senderGroupingKey**, the sender group key field.
* Changed the field name of recipient list into **receiverList**.
* Added **isReceived**, the received or not field.
* Changed the field name of opened or not for recipients into **isOpened**.
* Changed the field name of the opened date time into **openedDate**.
* Changed the field name of sender's mail address into **senderAddress**.
<a id="category-management"></a>
## Category Management { #category-management }

<a id="list"></a>
### List { #list }

<a id="list-request"></a>
#### Request 

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v1.5/appKeys/{appKey}/categories|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Query parameter]

|Value| Type | Required | Description |
|---|---|---|---|
|useYn|	String|	X|	Use or Not: Y, N |
|categoryParentId|	Integer|	X|	Parent category ID |
|pageNum|	Integer|	X| Page number (default: 1) |
|pageSize|	Integer|	X| Number of queries (default: 15) |

[Example]

``` sh
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/categories?useYn=Y&categoryParentId=1&pageNum=1&pageSize=10"
```

<a id="list-response"></a>
#### Response 

``` json
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
                "updateDate": "2019-07-23 00:00:00.0",
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
|GET|	/email/v1.5/appKeys/{appKey}/categories/{categoryId}|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|categoryId|	String| Category ID |

[Example]

``` sh
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/categories/{categoryId}"
```

<a id="query-details-response"></a>
#### Response

``` json
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
|POST|/email/v1.5/appKeys/{appKey}/categories|


[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Request body]

|Value| Type |	Max Length | Required | Description |
|---|---|---|---|---|
| categoryParentId |	Integer|	- | X |	Parent category ID <br/>Highest-level category ID (default) |
| categoryName |	String|	200 | O |	Category name |
| categoryDesc |	String| 1000 |	X | Category description |
| useYn |	String| 1 |	X| Use or not: Y (default), N |
| userId | String | 50 | X | User ID |

[Example]

``` sh
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/categories -d '{"categoryParentId":12345,"categoryName":"Category","categoryDesc":"Top Category","useYn":"Y","userId":"USER"}'
```


<a id="register-response"></a>
#### Response

``` json
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
|PUT|/email/v1.5/appKeys/{appKey}/categories/{categoryId}|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|categoryId|	Integer| Category ID |

[Request body]

|Value| Type |	Max Length | Required | Description |
|---|---|---|---|---|
| categoryName |	String|	200 | X |	Category name |
| categoryDesc |	String| 1000 |	X | Category description |
| useYn |	String| 1 |	X| Use or not: Y, N |
| userId | String | 50 | X | User ID |

[Example]

``` sh
curl -X PUT -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/categories/{categoryId} -d '{"categoryName":"Category","categoryDesc":"Top Category","useYn":"Y","userId":"USER"}'
```

<a id="modify-response"></a>
#### Response

``` json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
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
|DELETE|/email/v1.5/appKeys/{appKey}/categories/{categoryId}|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|categoryId|	Integer| Category ID |

[Example]

``` sh
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/categories/{categoryId}
```

<a id="delete-response"></a>
#### Response

``` json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
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
|GET|	/email/v1.5/appKeys/{appKey}/templates|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

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
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/templates?useYn=Y&pageNum=1&pageSize=10"
```

<a id="query-list-of-templates-response"></a>
#### Response

```
{
    "header": {
        "resultCode": Integer,
        "resultMessage": String,
        "isSuccessful": Boolean
    },
    "body": {
        "pageNum": Integer,
        "pageSize": Integer,
        "totalCount": Integer,
        "data": [
            {
                "templateId": String,
                "categoryId": Integer,
                "categoryName": String,
                "templateName": String,
                "templateDesc": String,
                "useYn": String,
                "delYn": String,
                "title": String,
                "createDate": String,
                "updateDate": String
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
|GET|	/email/v1.5/appKeys/{appKey}/templates/{templateId}|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|templateId|	String| Template ID |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/templates/{templateId}"
```

<a id="query-template-details-response"></a>
#### Response

```
{
    "header": {
        "resultCode": Integer,
        "resultMessage": String,
        "isSuccessful": Boolean
    },
    "body": {
        "data": {
            "templateId": String,
            "categoryId": Integer,
            "categoryName": String,
            "templateName": String,
            "templateDesc": String,
            "useYn": String,
            "delYn": String,
            "sendMailAddress": String,
            "title": String,
            "templateType": String,
            "body": String,
            "createDate": String,
            "updateDate": String,
            "attachFileList": [
                {
                    "fileType": String,
                    "fileId": Integer,
                    "fileName": String,
                    "filePath": String,
                    "fileSize": Integer,
                    "createDate": String
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
|POST|  /email/v1.5/appKeys/{appKey}/templates|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|    String| Original appKey |

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

[Example]

``` sh
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/templates -d '{"categoryId":1,"templateId":"TEAMPLTE_ID","templateName":"Template name","templateDesc":"Template description","useYn":"Y","sendMailAddress":"test@nhn.com","title":"Mail title","templateType":"DEFAULT","body":"Mail body","attachFileIdList":[1,2,3],"userId":"USER"}'
```


<a id="register-templates-response"></a>
#### Response

``` json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
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
|POST|  /email/v1.5/appKeys/{appKey}/templates/attachfile/binaryUpload|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|    String| Original appKey |

[Request body]

|Value| Type | Max Length | Required | Description |
|---|---|------------|---|---|
|fileName|  String| -          |O| File name |
|fileBody|  Byte[]| -          |O| Byte [] value |
|userId|    String| 50         |X| User ID |

[Example]

``` sh
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/templates/attachfile/binaryUpload -d '{"fileName":"file.csv","userId":"USER","fileBody":[]}'
```

<a id="query-of-templates-upload-attached-files-response"></a>
#### Response

``` json
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
|PUT|   /email/v1.5/appKeys/{appKey}/templates/{templateId}|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|    String| Original appKey |
|templateId|    String| Template ID |

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

[Example]

``` sh
curl -X PUT -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/templates/{templateId} -d '{"templateName":"template name","templateDesc":"template description","useYn":"Y","sendMailAddress":"test@nhn.com","title":"mail title","templateType":"DEFAULT","body":"mail body","attachFileIdList":[1,2,3],"userId":"USER"}'
```

<a id="modify-templates-response"></a>
#### Response

``` json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
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
|DELETE|    /email/v1.5/appKeys/{appKey}/templates/{templateId}|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|    String| Original appKey |
|templateId|    String| Template ID |

[Example]

``` sh
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/templates/{templateId}
```

<a id="delete-templates-response"></a>
#### Response

``` json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
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
|GET|	/email/v1.5/appKeys/{appKey}/statistics/view |

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Query Parameter]

|Value| Type | Required | Description |
|---|---|---|---|
|from|	String|	O | Start date of statistical query<br/> yyyy-mm-dd HH:mm |
|to|	String|	O | End date of statistical query<br/> yyyy-mm-dd HH:mm |
|searchType| String | O | Statistics Type<br/>DATE: By date, TIME: By time, DAY: By day |
|mailTypes | String | X | Mail Type<br/>NORMAL: General, MASS: Mass<br/>Multiple inputs are available (mailTypes=NORMAL&mailTypes=MASS) |
|adYn | String | X | Ad or not<br>Y: Ad, N: Not an ad<br>All, if left blank |
|templateId | String | X | Template ID |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/statistics/view?from=2018-03-21+00%3A00&to=2018-03-23+00%3A00&searchType=DATE&mailTypes=NORMAL&adYn=Y&templateId=templateId1"
```

<a id="query-daily-statistics-response"></a>
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
| GET |	/email/v1.5/appKeys/{appKey}/block-receivers |

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Query Parameter]

| Value       | Type    | Required | Description                                        |
| ----------- | ------- | -------- | -------------------------------------------------- |
| mailAddress | String  | X        | Email address registered on the list of rejections |
|startBlockDate|	String|	X| Start date of rejection (yyyy-MM-dd HH:mm:ss)|
|endBlockDate|	String|	X| End date of rejection (yyyy-MM-dd HH:mm:ss)|
| pageNum     | Integer | X        | Page number (default: 1)                           |
| pageSize    | Integer | X        | Number of queries (default: 15)                    |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/block-receivers?mailAddress=customer1@nhnent.com&pageNum=1&pageSize=10"
```

<a id="query-rejections-response"></a>
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

<a id="register-rejections"></a>
### Register Rejections { #register-rejections }

<a id="register-rejections-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
| POST |	/email/v1.5/appKeys/{appKey}/block-receivers |

[Request Body]

| Value             | Type   | Required | Description                             |
| ----------------- | ------ | -------- | --------------------------------------- |
| blockReceiverList | ㅣList | O        | List of rejections                      |
| - mailAddress     | String | O        | Email address to reject                 |
| - blockDate       | String | X        | Date of rejection (yyyy-MM-dd HH:mm:ss) |

[Example]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/block-receivers -d '{"blockReceiverList":[{"mailAddress":"customer1@nhnent.com","blockDate":"2018-03-01 00:00:00"}]}'
```

<a id="register-rejections-response"></a>
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

<a id="delete-rejections"></a>
### Delete Rejections { #delete-rejections }

<a id="delete-rejections-request"></a>
#### Request

[URL]

|Http method|	URI|
|---|---|
| PUT |	/email/v1.5/appKeys/{appKey}/block-receivers |

[Request Body]

| Value             | Type    | Required | Description                             |
| ----------------- | ------- | -------- | --------------------------------------- |
| deleted           | Boolean | O        | Field specifying rejections are deleted |
| blockReceiverList | ㅣList  | O        | List of rejections                      |
| - mailAddress     | String  | O        | Email address rejecting ads             |

[Example]

```
curl -X PUT -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/block-receivers -d '{"deleted":true,"blockReceiverList":[{"mailAddress":"customer1@nhnent.com"}]}'
```

<a id="delete-rejections-response"></a>
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
