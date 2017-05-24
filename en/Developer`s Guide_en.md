Notification &gt; Email &gt; Developer's Guide
---------------------------------------------------

Send Mail
---------

\[API domain\]

| Environment | Domain                           |
|-------------|----------------------------------|
| Real        | https://api-mail.cloud.toast.com |

### Send mail

#### Send regular mail

#### Request

\[URL\]

| Http method | URI                                      |
|-------------|------------------------------------------|
| POST        | /email/v1.0/appKeys/{appKey}/sender/mail |

\[Path parameter\]

| Value  | Type   | Description   |
|--------|--------|---------------|
| appKey | String | Unique appKey |

\[Request body\]

| Value             | Type        | Required | Description                                                |
|-------------------|-------------|----------|------------------------------------------------------------|
| ​senderAddress    | String      | O        | Sender mail                                                |
| requestDate       | String      | X        | Sent date (If not entered : send as current time)          |
| title             | String      | O        | Title                                                      |
| body              | String      | O        | Body                                                       |
| attachFileIdList  | List:String | X        | Uploaded attachment id                                     |
| templateId        | String      | X        | Send template ID                                           |
| templateParameter | Object      | X        | Switching parameter (Enter when switching mail title/body) |
| - \#key\#         | String      | X        | Switching key (\#\#key\#\#)                                |
| - \#value\#       | Object      | X        | Mapping value for switching key                            |
| receiverList      | List        | O        | Receiver list                                              |
| - receiveMailAddr | String      | O        | Receiver mail address                                      |
| - receiveType     | String      | O        | Receiver type (MRT0: Receiver , MRT1 : CC)                 |
| - receiveTypeName | String      | X        | Receiver name                                              |
| userId            | String      | X        | Client ID(PAYCO UUID)                                      |

#### Response

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

| Value           | Type    | Description                                                             |
|-----------------|---------|-------------------------------------------------------------------------|
| header          | Object  | Header area                                                             |
| - isSuccessful  | Boolean | Success or failure                                                      |
| - resultCode    | Integer | Failure code                                                            |
| - resultMessage | String  | Failure message                                                         |
| body            | Object  | Body area                                                               |
| - data          | Object  | Data area                                                               |
| -- requestId    | String  | Request ID                                                              |
| -- statusCode   | String  | Request status code (Y: Ready to send, N : Failed to get ready to send) |

#### Send individual mail

#### Request

\[URL\]

| Http method | URI                                          |
|-------------|----------------------------------------------|
| POST        | /email/v1.0/appKeys/{appKey}/sender/eachMail |

\[Path parameter\]

| Value  | Type   | Description   |
|--------|--------|---------------|
| appKey | String | Unique appKey |

\[Request body\]

| Value               | Type        | Required | Description                                                |
|---------------------|-------------|----------|------------------------------------------------------------|
| ​senderAddress      | String      | O        | Sender mail                                                |
| requestDate         | String      | X        | Sent date (If not entered : Send as current time)          |
| title               | String      | O        | Title                                                      |
| body                | String      | O        | Body                                                       |
| attachFileIdList    | List:String | X        | Uploaded attachment id                                     |
| templateId          | String      | X        | Send template ID                                           |
| receiverList        | List        | O        | Receiver list                                              |
| - receiveMailAddr   | String      | O        | Receiver mail address                                      |
| - receiveTypeName   | String      | X        | Receiver name                                              |
| - templateParameter | Object      | X        | Switching parameter (Enter when switching mail title/body) |
| -- \#key\#          | String      | X        | Switching key (\#\#key\#\#)                                |
| -- \#value\#        | Object      | X        | Mapping value for switching key                            |
| userId              | String      | X        | Client ID(PAYCO UUID)                                      |

#### Response

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

| Value           | Type    | Description                                                             |
|-----------------|---------|-------------------------------------------------------------------------|
| header          | Object  | Header area                                                             |
| - isSuccessful  | Boolean | Success or failure                                                      |
| - resultCode    | Integer | Failure code                                                            |
| - resultMessage | String  | Failure message                                                         |
| body            | Object  | Body area                                                               |
| - data          | Object  | Data area                                                               |
| -- requestId    | String  | Request ID                                                              |
| -- statusCode   | String  | Request status code (Y: Ready to send, N : Failed to get ready to send) |

### Retrieve Mail Send List

#### Request

\[URL\]

| Http method | URI                                       |
|-------------|-------------------------------------------|
| GET         | /email/v1.0/appKeys/{appKey}/sender/mails |

\[Path parameter\]

| Value  | Type   | Description   |
|--------|--------|---------------|
| appKey | String | Unique appKey |

\[Query parameter\]

| Value            | Type    | Required           | Description                                                                                      |
|------------------|---------|--------------------|--------------------------------------------------------------------------------------------------|
| requestId        | String  | Required condition | Request ID                                                                                       |
| startSendDate    | String  | Required condition | Start send date value (yyyy-MM-dd HH:mm:ss)                                                      |
| endSendDate      | String  | Required condition | End send date value (yyyy-MM-dd HH:mm:ss)                                                        |
| startReceiveDate | String  | X                  | Start receive date value (yyyy-MM-dd HH:mm:ss)                                                   |
| endReceiveDate   | String  | X                  | End receive date value (yyyy-MM-dd HH:mm:ss)                                                     |
| senderMail       | String  | X                  | Sender mail address                                                                              |
| senderName       | String  | X                  | Sender name                                                                                      |
| receiveMail      | String  | X                  | Receive mail address                                                                             |
| templateId       | String  | Optional           | Template number                                                                                  |
| sendStatus       | String  | Optional           | Send status code SST0:Ready to send, SST1:Sending, SST2:Completed sending, SST3 : Failed to send |
| pageNum          | Integer | Optional           | Page number(Default : 1)                                                                         |
| pageSize         | Integer | Optional           | Number of retrieve(Default : 15)                                                                 |

#### Response

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
                "receiveType": String,
                "receiverName": String
            }]
        }
    }

| Value               | Type    | Description                                                                                      |
|---------------------|---------|--------------------------------------------------------------------------------------------------|
| header              | Object  | Header area                                                                                      |
| - isSuccessful      | Boolean | Success or failure                                                                               |
| - resultCode        | Integer | Failure code                                                                                     |
| - resultMessage     | String  | Failure message                                                                                  |
| body                | Object  | Body area                                                                                        |
| - pageNum           | Integer | Current page number                                                                              |
| -pageSize           | Integer | Number of data retrieved                                                                         |
| - totalCount        | Integer | Total number of data                                                                             |
| - data              | List    | Data area                                                                                        |
| -- requestId        | String  | Request ID                                                                                       |
| -- requestDate      | String  | Sent date                                                                                        |
| -- requestIp        | String  | Sent server IP                                                                                   |
| -- resultDate       | String  | Received date                                                                                    |
| -- templateId       | String  | Template ID                                                                                      |
| -- templateName     | String  | Template name                                                                                    |
| -- masterStatusCode | String  | Ready to send mail status code ( “Y” : Ready to send , “N” : Failed to send)                     |
| -- mailSeq          | String  | Mail sequence                                                                                    |
| -- body             | String  | Body content                                                                                     |
| -- title            | String  | Mail title                                                                                       |
| -- senderMail       | String  | Sender mail address                                                                              |
| -- senderName       | String  | Sender name                                                                                      |
| -- receiverMail     | String  | Receiver mail address                                                                            |
| -- receiveType      | String  | Sender type (MRT0: Receiver, MRT1 : CC)                                                          |
| -- receiverName     | String  | Receiver name                                                                                    |
| -- resultId         | String  | Sent result ID                                                                                   |
| -- resultDate       | String  | Send completed date                                                                              |
| -- mailStatusCode   | String  | Send status code SST0:Ready to send, SST1:Sending, SST2:Completed sending, SST3 : Failed to send |
| -- mailStatusName   | String  | Send status code SST0:Ready to send, SST1:Sending, SST2:Completed sending, SST3 : Failed to send |
| -- requestDate      | String  | Requested send date                                                                              |

### Look up Sending Mail in Detail

#### Request

\[URL\]

| Http method | URI                                                            |
|-------------|----------------------------------------------------------------|
| GET         | /email/v1.0/appKeys/{appKey}/sender/mail/{requestId}/{mailSeq} |

\[Path parameter\]

| Value     | Type    | Description   |
|-----------|---------|---------------|
| appKey    | String  | Unique appKey |
| requestId | String  | Request ID    |
| mailSeq   | Integer | Mail sequence |

#### Response

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
                    "receiveMailAddr": String
                }],
                "attachFileList": [{
                    "filePath": String,
                    "fileName": String,
                    "fileSize": Integer,
                    "fileSequence": Integer,
                    "createDate": String,
                    "updateDate": String
                }]
            }
        }
    }

| Value              | Type    | Description            |
|--------------------|---------|------------------------|
| header             | Object  | Header area            |
| - isSuccessful     | Boolean | Success or failure     |
| - resultCode       | Integer | Failure code           |
| - resultMessage    | String  | Failure message        |
| body               | Object  | Body area              |
| - data             | Object  | Data area              |
| --mailStatusCode   | String  | Send status            |
| --mailStatusName   | String  | Send status name       |
| --templateId       | String  | Template ID            |
| --templateName     | String  | Template name          |
| --senderName       | String  | Sender name            |
| --senderMail       | String  | Sender mail address    |
| --title            | String  | Mail title             |
| --body             | String  | Mail content           |
| --attachFileYn     | String  | Attachment or no       |
| --resultId         | String  | Sending mail ID        |
| --resultDate       | String  | Sending completed date |
| --receivers        | List    | Receiver list          |
| ---requestId       | String  | Request ID             |
| ---mailSequence    | Integer | Mail sequence          |
| ---receiveType     | String  | Receiver type          |
| ---receiveTypeName | String  | Receiver type name     |
| ---receiveName     | String  | Receiver name          |
| ---receiveMailAddr | String  | Receiver mail address  |
| --attachFileList   | List    | Attachment list        |
| ---filePath        | String  | Attachment path        |
| ---fileName        | String  | Attachment name        |
| ---fileSize        | Integer | Attachment size        |
| ---fileSequence    | Integer | Attachment sequence    |
| ---createDate-     | String  | Created date           |
| --updateDate       | String  | Updated date           |

### Upload Attachment

#### Request

\[URL\]

| Http method | URI                                                  |
|-------------|------------------------------------------------------|
| POST        | /email/v1.0/appKeys/{appKey}/attachfile/binaryUpload |

\[Path parameter\]

| Value  | Type   | Description   |
|--------|--------|---------------|
| appKey | String | Unique appKey |

\[Request body\]

    {
        "fileName": String,
        "createUser": String,
        "fileBody": Byte[]
    }

| Value      | Type     | Required | Description                  |
|------------|----------|----------|------------------------------|
| fileName   | String   | Required | File name                    |
| fileBody   | Byte\[\] | Required | Byte\[\] value of file       |
| createUser | String   | Required | File upload user information |

#### Response

    {
      "header": {
        "isSuccessful":  Bollean,
        "resultCode": Integer,
        "resultMessage": String
      },
      "body": {
        "data": {
          "requestId": String,
          "fileName": String,
          "filePath": String
        }
      }
    }

| Value           | Type    | Description                                                         |
|-----------------|---------|---------------------------------------------------------------------|
| header          | Object  | Header area                                                         |
| - isSuccessful  | Boolean | Success or failure                                                  |
| - resultCode    | Integer | Failure code                                                        |
| - resultMessage | String  | Failure message                                                     |
| body            | Object  | Body area                                                           |
| - data          | Object  | Data area                                                           |
| -- requestId    | String  | Temporary request Id                                                |
| -- fileName     | String  | File name                                                           |
| -- filePath     | String  | Attachment basic Path (https://domain/attachFile/filePath/fileName) |

### Look up Template List

#### Request

\[URL\]

| Http method | URI                                    |
|-------------|----------------------------------------|
| GET         | /email/v1.0/appKeys/{appKey}/templates |

\[Path parameter\]

| Value  | Type   | Description   |
|--------|--------|---------------|
| appKey | String | Unique appKey |

\[Query parameter\]

| Value      | Type    | Required | Description                     |
|------------|---------|----------|---------------------------------|
| categoryId | Integer | Optional | Category ID                     |
| useYn      | String  | Optional | Use or no(Y/N)                  |
| pageNum    | Integer | Optional | Page number(Default : 1)        |
| pageSize   | Integer | Optional | Number of look up(Default : 15) |
| all        | Boolean | Optional | Look up all or no               |

#### Response

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
                "sendMailAddress”: String,
                “sendType”: String,
                “sendTypeName”: String,
                “title”: String,
                “body”: String,
                “attachFileYn”: String,
                “delYn”: String,
                “createDate”: String,
                “createUser”: String,
                “updateDate”: String,
                “updateUser”: String,
    }]
        }
    }

| Value             | Type    | Description                   |
|-------------------|---------|-------------------------------|
| header            | Object  | Header area                   |
| - isSuccessful    | Boolean | Success or failure            |
| - resultCode      | Integer | Failure code                  |
| - resultMessage   | String  | Failure message               |
| body              | Object  | Body area                     |
| - pageNum         | Integer | Current page number           |
| -pageSize         | Integer | Number of retrieved data      |
| - totalCount      | Integer | Total number of data          |
| -data             | List    | Data area                     |
| --templateId      | String  | Template ID                   |
| --serviceId       | Integer | Service ID                    |
| --categoryId      | Integer | Category ID                   |
| --categoryName    | String  | Category name                 |
| --sort            | Integer | Sequence                      |
| --templateName    | String  | Template name                 |
| --templateDesc    | String  | Template description          |
| -- useYn          | String  | Use or no                     |
| --sendMailAddress | String  | Template send mail address    |
| --title           | String  | Mail title                    |
| --body            | String  | Mail body                     |
| --attachFileYn    | String  | Add template attachment or no |
| --delYn           | String  | Delete or no                  |
| --createDate      | String  | Created date                  |
| --updateDate      | String  | Updated date                  |

### Look up Template in Detail

#### Request

\[URL\]

| Http method | URI                                                 |
|-------------|-----------------------------------------------------|
| GET         | /email/v1.0/appKeys/{appKey}/templates/{templateId} |

\[Path parameter\]

| Value      | Type   | Description   |
|------------|--------|---------------|
| appKey     | String | Unique appKey |
| templateId | String | Template ID   |

#### Response

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
                “templateName": String,
                "templateDesc": String,
                "useYn": String,
                "sendMailAddress": String,
                "title": String,
                "body": String,
                “delYn": String,
                "createDate": String,
                "createUser": String,
                "updateDate": String,
                "updateUser": String,
                "attachFileList": [{
                    "fileId": Integer,
                    “serviceId” : Integer,
                    "templateId": String,
                    "filePath": String,
                    "fileName": String,
                    “fileSize” : Integer,
                    "createDate": String,
                    "createUser": String,
                    “updateDate” : String,
                    “updateUser” : String
                }]
            }]
        }
    }

| Value             | Type    | Description                         |
|-------------------|---------|-------------------------------------|
| header            | Object  | Header area                         |
| - isSuccessful    | Boolean | Success or failure                  |
| - resultCode      | Integer | Failure code                        |
| - resultMessage   | String  | Failure message                     |
| body              | Object  | Body area                           |
| -data             | List    | Data area                           |
| --templateId      | String  | Template ID                         |
| --serviceId       | Integer | Service ID                          |
| --categoryId      | Integer | Category ID                         |
| --categoryName    | String  | Category name                       |
| --templateName    | String  | Template name                       |
| --templateDesc    | String  | Template description                |
| --useYn           | String  | Use or no (Y= In use, N= Don’t use) |
| --sendMailAddress | String  | Send mail address                   |
| --title           | String  | Mail address                        |
| --body            | String  | Mail body                           |
| --delYn           | String  | Delete or no                        |
| --createDate      | String  | Created date                        |
| --createUser      | String  | Creator                             |
| --updateDate      | String  | Updated date                        |
| --updateUser      | String  | Modifier                            |
| --attachFileList  | List    | Attachment list                     |
| ---fileId         | Integer | Attachment ID                       |
| ---serviceId      | Integer | Service ID                          |
| ---templateId     | String  | Template ID                         |
| ---filePath       | String  | Attachment path                     |
| ---fileName       | String  | Attachment name                     |
| ---fileSize       | Integer | Attachment size                     |
| ---createDate     | String  | Created date                        |
| ---createUser     | String  | Creator                             |
| ---updateDate     | String  | Updated date                        |
| ---updateUser     | String  | Modifier                            |
