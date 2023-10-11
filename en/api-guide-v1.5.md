## Notification > Email > API v1.5 Guide

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

## Mail Delivery

### Send General Mails

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
| customHeaders     | Map         | X        | [Custom Header](./Overview/#custom-header)                   |
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

#### Updated for v1.5

* Added the Sender Group Key field. Setting is available by request.  
* When it is requested for delivery, specify **senderGroupingKey** field to be made available to query request. 

### Send Individual Mails

* Mails can be sent to each of many recipients. Even if a same mail is sent to many recipients, each recipient can find his or her name only.  

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
|customHeaders| Map| X| [Custom Header](./Overview/#custom-header) |
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

#### Updated for v1.5 

* Added the Sender Group Key field. Setting is available by request.  
* When it is requested for delivery, specify **senderGroupingKey** field to be made available to query request. 

### Sending General Ad Mails 

* Request and response information is same as in sending general mails. 

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

### Sending Individual Ad Mails

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

### Send Authenticated Mails 

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
| customHeaders       | Map    | X        | [Custom Header](./Overview/#custom-header)                   |
| userId              | String | X        | Delimiter for delivery e.g.) admin,system                    |

[Caution]

- To use a template, title and body are not required (input values, if available, precede template values)

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

### Send Tagged Mails

#### Request

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/sender/tagMail|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Request body]

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
| tagExpression    | List:String | O        | Tag expression (including Tag ID)                                               |
| customHeaders    | Map         | X        | [Custom Header](./Overview/#custom-header)                   |
| userId           | String      | X        | Delimiter of delivery e.g.) admin, system                    |

[Caution]

- To use template, title and body are required (input values, if available, precede template values)

[Example 1]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/tagMail -d '{"senderAddress":"support@nhnent.com","senderName":"sender's name","title":"sample title","body":"sample body","attachFileIdList":[1, 2],"tagExpression":["tag1","AND","tag2"],"userId":"USER"}'
```

[Example 2 - Using Templates]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/tagMail -d '{"templateId":"TEMPLATE1","tagExpression":["tag1","AND","tag2"],"userId":"USER"}'
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
```
* title: Hello, ${title_name}!!
* body: We send ${body_content}.
->
* title: Hello, cloud customer1!!
* body: We send test2.
```

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

## Query of Mails

### Query List of Mail Deliveries

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

| Value            | Type    | Required | Description                                                  |
| ---------------- | ------- | -------- | ------------------------------------------------------------ |
| requestId        | String  | O        | Request ID                                                   |
| startSendDate    | String  | O        | Delivery start date (yyyy-MM-dd HH:mm:ss)                    |
| endSendDate      | String  | X        | Delivery end date (yyyy-MM-dd HH:mm:ss)                      |
| startReceiveDate | String  | X        | Receiving start date (yyyy-MM-dd HH:mm:ss)                   |
| endReceiveDate   | String  | X        | Receiving end date (yyyy-MM-dd HH:mm:ss)                     |
| senderAddress    | String  | X        | Sender's mail address                                        |
| senderName       | String  | X        | Sender's name                                                |
| receiveMailAddr  | String  | X        | Recipient's mail address                                     |
| templateId       | String  | X        | Template number                                              |
| mailStatusCode   | String  | X        | Delivery status code   SST0: Preparing for sending, SST1: Sending,    SST2: Sending completed, SST3: Sending failed |
| isReceived       | Boolean | X        | Received or Not                                              |
| isOpened         | Boolean | X        | Opened or Not                                                |
| senderGroupingKey| String  | X        | Sender's group key                                           |
| pageNum          | Integer | X        | Page number (default: 1)                                     |
| pageSize         | Integer | X        | Number of queries (default: 15)                              |

* **requestId**, or **startSendDate** and **endSendDate** are required fields. 

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/mails?startSendDate=2018-03-01+00%3A00&endSendDate=2018-03-07+23%3A59&pageSize=10"
```

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
| -- mailStatusCode   | String  | Delivery status code   SST0: Preparing Delivery, SST1: Delivering,    SST2: Delivery Completed, SST3: Delivery Failed |
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

#### Updated for v1.5

* Added **senderGroupingKey**, the sender group key field.
* Added **isReceived**, the received or not field.
* Added **isOpened**, the opened or not field.
* Added **openedDate**, date and time of opening field.
* Changed the field name of delivery status code into **mailStatusCode**.
* Changed the field name of recipient's mail address into **receiveMailAddr**.
* Changed the field name of sender's mail address into **senderAddress**.


### Query Mail Delivery Details

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
| -- customHeaders    | Map     | [Custom Header](./Overview/#custom-header)                   |
| -- senderGroupingKey|	String  | Sender's group key                                           |

#### Updated for v1.5

* Added **senderGroupingKey**, the sender group key field.
* Changed the field name of recipient list into **receiverList**.
* Added **isReceived**, the received or not field.
* Changed the field name of opened or not for recipients into **isOpened**.
* Changed the field name of the opened date time into **openedDate**.
* Changed the field name of sender's mail address into **senderAddress**.


### Query Request for Tagged Mail Delivery

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/tagMails|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

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

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/tagMails?startSendDate=2018-03-01+00%3A00&endSendDate=2018-03-07+23%3A59&pageSize=10"
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
| -- tagSendStatus | String      | Delivery status code   WAIT: Waiting, READY: Ready,  SENDREADY: Ready for Delivery, SENDWAIT: Waiting for Delivery, SENDING: Delivering, COMPLETE: Delivery Completed,  FAIL: Delivery Failed, CANCEL: Delivery Canceled |
| -- tagExpression | List:String | Tag expression (including Tag ID)                                               |
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
|GET|	/email/v1.5/appKeys/{appKey}/tagMails/{requestId}|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|requestId|	String| Request ID |

[Query Parameter]

| Value            | Type    | Required | Description                                                  |
| ---------------- | ------- | -------- | ------------------------------------------------------------ |
| receiveMail      | String  | X        | Recipient's mail address                                     |
| startReceiveDate | String  | X        | Start date of receiving (yyyy-MM-dd HH:mm:ss)                |
| endReceiveDate   | String  | X        | End date of receiving (yyyy-MM-dd HH:mm:ss)                  |
| receiveStatus    | String  | X        | Delivery status code   SST0: Preparing Delivery, SST1: Delivering,    SST2: Delivery Completed, SST3: Delivery Failed |
| pageNum          | Integer | X        | Page number (default: 1)                                     |
| pageSize         | Integer | X        | Number of queries (default: 15)                              |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/tagMails/{requestId}?startReceiveDate=2018-03-01+00%3A00&endReceiveDate=2018-03-07+23%3A59&pageSize=10"
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
| -- mailStatusCode | String  | Delivery status code   SST0: Preparing Delivery, SST1: Delivering,    SST2: Delivery Completed, SST3: Delivery Failed |
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
|GET|	/email/v1.5/appKeys/{appKey}/tagMails/{requestId}/{mailSequence}|

[Path Parameter]

| Value        | Type    | Description     |
| ------------ | ------- | --------------- |
| appKey       | String  | Original appKey |
| requestId    | String  | Request ID      |
| mailSequence | Integer | Mail sequence   |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/tagMails/{requestId}/{mailSequence}"
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

| Value               | Type    | Description                                                  |
| ------------------- | ------- | ------------------------------------------------------------ |
| header              | Object  | Header area                                                  |
| - isSuccessful      | Boolean | Successful or not                                            |
| - resultCode        | Integer | Failure code                                                 |
| - resultMessage     | String  | Failure message                                              |
| body                | Object  | Body area                                                    |
| - pageNum           | Integer | Current page number                                          |
| -pageSize           | Integer | Number of queried data                                       |
| - totalCount        | Integer | Total number of data                                         |
| - data              | List    | Data area                                                    |
| -- requestId        | String  | Request ID                                                   |
| -- requestIp        | String  | Request IP                                                   |
| -- templateId       | String  | Template ID                                                  |
| -- templateName     | String  | Template name                                                |
| -- mailStatusCode   | String  | Mail status code   SST0: Preparing Delivery, SST1: Delivering,    SST2: Delivery Completed, SST3: Delivery Failed |
| -- mailStatusName   | String  | Name of mail status                                          |
| -- requestDate      | String  | Request time                                                 |
| -- resultDate       | String  | Result time                                                  |
| -- senderName       | String  | Sender's name                                                |
| -- senderMail       | String  | Sender's address                                             |
| -- resultId         | String  | SMTP ID                                                      |
| -- title            | String  | Title                                                        |
| -- body             | String  | Body                                                         |
| -- receivers        | List    | List of recipients                                           |
| --- requestId       | String  | Request ID                                                   |
| --- mailSequence    | Integer | Mail sequence                                                |
| --- receiveType     | String  | Type of recipients (MRT0: recipient, MRT1: Cc, MRT2: Bcc)    |
| --- receiveTypeName | String  | Name of recipient type                                       |
| --- receiveMailAddr | String  | Recipient's mail address                                     |
| --- readYn          | String  | Read or not                                                  |
| --- readDate        | String  | Read time                                                    |
| -- attachFileList   | List    | List of attached files                                       |
| --- fileType        | String  | Type of attached file (MAIL: files attached to mail, TEMPLATE: files attached to template) |
| --- fileId          | Integer  | File ID                                                      |
| --- fileName        | String  | Name of attached file                                        |
| --- filePath        | String  | Path of attached file                                        |
| --- fileSize        | Integer | Size of attached file (byte)                                 |
| --- createDate      | String  | Date and time of creation                                    |
| -- customHeaders    | Map     | [Custom Header](./Overview/#custom-header)                   |

<p id="category"></p>

## Category Management 

### List  

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

### Query Details 

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


### Register

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


### Modify

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

### Delete

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

## Query of Templates

### Query List of Templates

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

### Query Template Details

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

### Register Templates 

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

### Upload Attached Files 

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
|---|---|---|---|---|
|fileName|  String| 100|O| File name |
|fileBody|  Byte[]| - |O| Byte [] value |
|userId|    String| 50|X| User ID |

[Example]

``` sh
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/templates/attachfile/binaryUpload -d '{"fileName":"file.csv","userId":"USER","fileBody":[]}'
```

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

[Caution]

* If a file is attached to a template after uploaded, the same file cannot be attached to another template: it must be modified or newly uploaded first, to be attached.  

### Modify Templates

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

### Delete Templates

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


## Tag Management

### Query Tags

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/tags|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Query Parameter]

| Value    | Type    | Required | Description                      |
| -------- | ------- | -------- | -------------------------------- |
| pageNum  | Integer | X        | Page number (Default : 1)        |
| pageSize | Integer | X        | Number of queries (Default : 15) |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/tags?pageNum=1&pageSize=10"
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
|POST|	/email/v1.5/appKeys/{appKey}/tags|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Request Body]

|Value| Type | Required | Description |
|---|---|---|---|
|tagName|	String|	O| Tag name |

[Example]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/tags -d '{"tagName":"sample tag"}'
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

##### Description
- 태그는 최대 2,048개까지 생성할 수 있습니다.

##### 태그에 UID 추가 생성
- 태그에 UID를 추가(append)하는 것으로, 기존에 있던 UID를 추가하면 UID의 태그는 늘어납니다.
- 한 UID에 태그를 16개까지 추가할 수 있습니다.


### Modify Tags

#### Request

[URL]

|Http method|	URI|
|---|---|
|PUT|	/email/v1.5/appKeys/{appKey}/tags/{tagId}|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|tagId|	String| Tag ID |

[Request Body]

|Value| Type | Required | Description |
|---|---|---|---|
|tagName|	String|	O| Tag name |

[Example]

```
curl -X PUT -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/tags/{tagId} -d '{"tagName":"sample tag2"}'
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
|DELETE|	/email/v1.5/appKeys/{appKey}/tags/{tagId}|

[Path Parameter]

|Value| Type | Desription |
|---|---|---|
|appKey|	String| Original appKey |
|tagId|	String| Tag ID |

[Example]

```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/tags/{tagId}
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
|GET|	/email/v1.5/appKeys/{appKey}/uids|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

[Query Parameter]

| Value     | Type        | Required | Description                                                  |
| --------- | ----------- | -------- | ------------------------------------------------------------ |
| wheres    | List:String | X        | Query conditions.</br> Character strings comprised of alphabets, numbers, and parentheses. </br>Allows one parenthesis, and up to three AND or ORs.</br> e.g.) tagId1,AND,tagId2 |
| offsetUid | String      | X        | offset UID                                                   |
| offset    | Integer     | X        | offset (default: 0)                                          |
| limit     | Integer     | X        | Number of queries (default: 15)                              |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/uids?wheres=tagId1,OR,tagId2&offset=0&limit=10"
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
| --- contacts     | List    | List of contacts                  |
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
|GET|	/email/v1.5/appKeys/{appKey}/uids/{uid}|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|uid|	String|	UID |

[Example]

```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/uids/{uid}"
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
| header          | Object  | Header are                        |
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
|POST|	/email/v1.5/appKeys/{appKey}/uids|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |

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

[Example]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/uids -d '{"uids":[{"uid":"sample-uid","tagIds":["tagId1"],"contacts":[{"contactType":"EMAIL_ADDRESS","contact":"customer1@nhnent.com"},{"contactType":"EMAIL_ADDRESS","contact":"customer2@nhnent.com"}]}]}'
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
|DELETE|	/email/v1.5/appKeys/{appKey}/uids/{uid}|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|uid|	String|	UID|

[Example]

```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/uids/{uid}
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
|POST|	/email/v1.5/appKeys/{appKey}/uids/{uid}/email-addresses|

[Path Parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appKey |
|uid|	String|	UID|

[Request Body]

|Value| Type | Required | Description |
|---|---|---|---|
|emailAddress|	String|	O| Mail address |

[Example]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/uids/{uid}/email-addresses -d '{"emailAddress" : "customer1@nhnent.com"}'
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
|DELETE|	/email/v1.5/appKeys/{appKey}/uids/{uid}/email-addresses/{emailAddress}|

[Path Parameter]

| Value        | Type   | Description     |
| ------------ | ------ | --------------- |
| appKey       | String | Original appKey |
| uid          | String | UID             |
| emailAddress | String | Mail address    |

[Example]

```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/uids/{uid}/email-addresses/customer1@nhnent.com
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

## Rejection Management

### Query Rejections

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