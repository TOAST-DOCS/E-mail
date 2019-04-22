## Notification > Email > API v1.1ガイド

[APIドメイン]

| 環境 | ドメイン                        |
| ---- | -------------------------------- |
| リアル | https://api-mail.cloud.toast.com |

[Header]
```
Content-Type: application/json;charset=UTF-8
```


[curl例注意事項]

* Windows cmdでは、curlの例が正常にリクエストされないことがあります。

## メールの送信

### 一般メールの送信

#### リクエスト

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| POST        | /email/v1.1/appKeys/{appKey}/sender/mail |

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |

[Request body]

| 値                 | タイプ          | 必須   | 説明                                       |
| ----------------- | ----------- | ---- | ---------------------------------------- |
| ​senderAddress    | String      | O    | 送信者のメール                             |
| senderName        | String      | X    | 送信者名                             |
| requestDate       | String      | X    | 送信日時が未入力の時は現在時間で送信(yyyy-MM-dd HH:mm:ss) |
| title             | String      | O    | タイトル                                 |
| body              | String      | O    | 内容                                 |
| attachFileIdList  | List:String | X    | アップロードした添付ファイルID                            |
| templateId        | String      | X    | 送信テンプレートID                                |
| templateParameter | Object      | X    | 置換パラメータ(メールのタイトル/内容置換時に入力)                |
| - #key#           | String      | X    | 置換キー(##key##)                            |
| - #value#         | Object      | X    | 置換キーにマッピングされるValue値                  |
| receiverList      | List        | O    | 受信者リスト<br/>最大1,000人まで送信可能(受信者、CC含む) |
| - receiveMailAddr | String      | O    | 受信者のメールアドレス                          |
| - receiveName     | String      | X    | 受信者名                             |
| - receiveType     | String      | O    | 受信者タイプ(MRT0：受信者、MRT1：CC、MRT2：BCC) |
| customHeaders     | Map         | X    | [ユーザー指定ヘッダ](./Overview/#custom-header)   |
| userId            | String      | X    | 送信セパレータ例) admin, system                  |


[注意]

* templateを使用する場合、title、bodyは必須入力から除外(入力時、入力された値がtemplateより優先適用)

* templateParameter引数を使用する時は、templateId引数を必ず入力

[例1]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/sender/mail -d '{"senderAddress":"support@nhnent.com","senderName":"送信者名","title":"サンプルタイトル","body":"サンプル内容","receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"顧客1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhnent.com","receiveName":"顧客2","receiveType":"MRT1"}],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

[例2 - テンプレートの使用]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/sender/mail -d '{"templateId":"TEMPLATE1","templateParameter":{"key":"value"},"receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"顧客1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhnent.com","receiveName":"顧客2","receiveType":"MRT1"}],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

#### レスポンス

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

| 値         | タイプ | 説明                      |
| --------------- | ------- | ----------------------------- |
| header          | Object  | ヘッダ領域                   |
| - isSuccessful  | Boolean | 成否                    |
| - resultCode    | Integer | 失敗コード                   |
| - resultMessage | String  | 失敗メッセージ                  |
| body            | Object  | 本文領域                   |
| - data          | Object  | データ領域                  |
| -- requestId    | String  | リクエストID                         |
| -- statusCode   | String  | リクエストステータスコード(Y：送信準備、N：送信準備失敗) |

### 個別メール送信

* 受信者が複数の時、それぞれの受信者にメールを送信する機能。受信者には受信者が一人に見えます。

#### リクエスト

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| POST        | /email/v1.1/appKeys/{appKey}/sender/eachMail |

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |

[Request body]

| 値            | タイプ  | 必須 | 説明                                                   |
| ------------------- | ----------- | ---- | ------------------------------------------------------------ |
| ​senderAddress       | String      | O    | 送信者メール                                            |
| senderName          | String      | X    | 送信者名                                            |
| requestDate         | String      | X    | 送信日時。未入力の時は現在時間で送信(yyyy-MM-dd HH:mm:ss) |
| title               | String      | O    | タイトル                                                   |
| body                | String      | O    | 内容                                                   |
| attachFileIdList    | List:String | X    | アップロードした添付ファイルID                                        |
| templateId          | String      | X    | 送信テンプレートID                                               |
| receiverList        | List        | O    | 受信者リスト<br/>最大1,000人まで送信可能             |
| - receiveMailAddr   | String      | O    | 受信者のメールアドレス                                       |
| - receiveName       | String      | X    | 受信者名                                            |
| - templateParameter | Object      | X    | 置換パラメータ(メールのタイトル/内容置換時の入力)                   |
| -- #key#            | String      | X    | 置換キー(##key##)                                             |
| -- #value#          | Object      | X    | 置換キーにマッピングされる値                                  |
| customHeaders       | Map         | X    | [ユーザー指定ヘッダ](./Overview/#custom-header)                |
| userId              | String      | X    | 送信セパレータ例) admin, system                                |

[注意]

* templateを使用する場合、title、bodyは必須から除外(入力時、入力された値がtemplateより優先適用)

* templateParameter引数を使用する時は、templateId引数を必ず入力

[例1]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/sender/eachMail -d '{"senderAddress":"support@nhnent.com","senderName":"送信者名","title":"サンプルタイトル","body":"サンプル内容","attachFileIdList":["添付ファイル_ID"],"receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"顧客1"}],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

[例2 - テンプレートの使用]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/sender/eachMail -d '{"templateId":"TEMPLATE1","receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"顧客1","templateParameter":{"key":"value"}}],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

#### レスポンス

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

| 値         | タイプ | 説明                       |
| --------------- | ------- | ------------------------------ |
| header          | Object  | ヘッダ領域                    |
| - isSuccessful  | Boolean | 成否                     |
| - resultCode    | Integer | 失敗コード                    |
| - resultMessage | String  | 失敗メッセージ                   |
| body            | Object  | 本文領域                    |
| - data          | Object  | データ領域                   |
| -- requestId    | String  | リクエストID                          |
| -- statusCode   | String  | リクエストステータスコード(Y：送信準備、N：送信準備失敗) |

### 広告性一般メール送信
* URLの最後のみad-mailに変わり、残りは一般メール送信と同じです。

#### 広告メール送信時の注意事項
* タイトルに必ず(広告)文言を挿入します。
* 詳細な内容は、[広告性メール送信](./console-guide/#_3)を参照してください。

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| POST        | /email/v1.1/appKeys/{appKey}/sender/ad-mail |

[例1]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/sender/ad-mail -d '{"senderAddress":"support@nhnent.com","senderName":"送信者名","title":"(広告)サンプルタイトル","body":"サンプル内容\n##BLOCK_RECEIVER_LINK## \n##EN_BLOCK_RECEIVER_LINK##","receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"顧客1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhnent.com","receiveName":"顧客2","receiveType":"MRT1"}],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

[例2 - テンプレートの使用]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/sender/ad-mail -d '{"templateId":"TEMPLATE1","templateParameter":{"key":"value"},"receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"顧客1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhnent.com","receiveName":"顧客2","receiveType":"MRT1"}],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

### 広告性個別メール送信

* URLの最後のみad-eachMailに変わり、残りは個別メール送信と同じです。

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| POST        | /email/v1.1/appKeys/{appKey}/sender/ad-eachMail |

[例1]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/sender/ad-eachMail -d '{"senderAddress":"support@nhnent.com","senderName":"送信者名","title":"(広告)サンプルタイトル","body":"サンプル内容\n##BLOCK_RECEIVER_LINK## \n##EN_BLOCK_RECEIVER_LINK##","attachFileIdList":["添付ファイル_ID"],"receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"顧客1"}],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

[例2 - テンプレートの使用]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/sender/ad-eachMail -d '{"templateId":"TEMPLATE1","receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"顧客1","templateParameter":{"key":"value"}}],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

### タグメールの送信

#### リクエスト

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| POST        | /email/v1.1/appKeys/{appKey}/sender/tagMail |

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |

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

| 値          | タイプ    | 必須   | 説明                                 |
| ---------------- | ----------- | ---- | ---------------------------------------- |
| senderAddress    | String      | O    | 送信者のアドレス                             |
| senderName       | String      | X    | 送信者名                             |
| requestDate      | String      | X    | 送信日時が未入力の時は現在時間で送信(yyyy-MM-dd HH:mm:ss) |
| title            | String      | O    | タイトル                                 |
| body             | String      | O    | 内容                                 |
| templateId       | String      | X    | テンプレートID                                   |
| adYn             | String      | X    | 広告かどうか(デフォルト値'N')                           |
| autoSendYn       | String      | X    | 自動送信するかどうか(デフォルト値'Y')                        |
| attachFileIdList | List:String | X    | 添付ファイルリスト                           |
| tagExpression    | List:String | O    | タグ表現式                             |
| customHeaders    | Map         | X    | [ユーザー指定ヘッダ](./Overview/#custom-header)   |
| userId           | String      | X    | 送信セパレータ<br />例) admin, system             |

[注意]

* templateを使用する場合、title、bodyは必須入力から除外(入力時に入力された値がtemplateより優先適用)

[例1]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/sender/tagMail -d '{"senderAddress":"support@nhnent.com","senderName":"送信者名","title":"サンプルタイトル","body":"サンプル内容","attachFileIdList":["添付ファイル_ID"],"tagExpression":["tag1","AND","tag2"],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

[例2 - テンプレートの使用]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/sender/tagMail -d '{"templateId":"TEMPLATE1","tagExpression":["tag1","AND","tag2"],"customHeaders":{"X-Sample":"sample","Content-Type":"text/html; charset=utf-8"},"userId":"XXXXX"}'
```

#### レスポンス

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

| 値               | タイプ      | 説明     |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - isSuccessful  | Boolean | 成否 |
| - resultCode    | Integer | 失敗コード |
| - resultMessage | String  | 失敗メッセージ |
| body            | Object  | 本文領域 |
| - data          | Object  | データ領域 |
| -- requestId    | String  | リクエストID  |

### 添付ファイルのアップロード

#### リクエスト

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| POST        | /email/v1.1/appKeys/{appKey}/attachfile/binaryUpload |

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |

[Request body]

```
{
    "fileName": String,
    "createUser": String,
    "fileBody": Byte[]
}
```

| 値          | タイプ     | 必須   | 説明            |
| ---------- | ------ | ---- | ------------- |
| fileName   | String | O    | ファイル名   |
| fileBody   | Byte[] | O    | ファイルのByte[]値 |
| createUser | String | O    | ファイルアップロードユーザー情報 |

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/attachfile/binaryUpload -d '{"fileName":"file.csv","createUser":"XXXXX","fileBody":[]}'
```

#### レスポンス

```
{
  "header": {
    "isSuccessful":  Boolean,
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
```

| 値         | タイプ      | 説明                                       |
| --------------- | ------- | ---------------------------------------- |
| header          | Object  | ヘッダ領域                              |
| - isSuccessful  | Boolean | 成否                               |
| - resultCode    | Integer | 失敗コード                              |
| - resultMessage | String  | 失敗メッセージ                             |
| body            | Object  | 本文領域                              |
| - data          | Object  | データ領域                             |
| -- requestId    | String  | 臨時request ID                            |
| -- fileName     | String  | ファイル名                              |
| -- filePath     | String  | 添付ファイル基本パス <br/>(https://domain/attachFile/filePath/fileName) |


### タイトル/本文置換

#### 置換機能メールの作成例

```
* title：##title_name##さん、こんにちは！
* body：##body_content## 送信します。
```

#### 一般メールのリクエスト例
```
{
    "senderAddress" : "support@nhnent.com",
    "templateId": "template1",
    "templateParameter" : {"title_name": "クラウド顧客1", "body_content": "test1"},
    "receiverList" : [
        {
            "receiveMailAddr" : "customer1@nhnent.com",
            "receiveType" : "MRT0"
        }

    ],
    "userId" : "tester"
}
```

#### 個別メールのリクエスト例
```
{
    "senderAddress" : "support@nhnent.com",
    "templateId": "template1",
    "receiverList" : [
        {
            "receiveMailAddr" : "customer1@nhnent.com",
            "templateParameter" : {"title_name": "クラウド顧客1", "body_content": "test1"}
        },
        {
            "receiveMailAddr" : "customer2@nhnent.com",
            "templateParameter" : {"title_name": "クラウド顧客2", "body_content": "test2"}
        }

    ],
    "userId" : "tester"
}
```

#### 結果例
```
* title：クラウド顧客1さん、こんにちは！
* body：test2送信します。
```

## メール照会

### メール送信リストの照会

#### リクエスト

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| GET         | /email/v1.1/appKeys/{appKey}/sender/mails |

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |

[Query parameter]

| 値                | タイプ      | 必須   | 説明                                       |
| ---------------- | ------- | ---- | ---------------------------------------- |
| requestId        | String  | O    | リクエストID                                    |
| startSendDate    | String  | O    | 送信日の開始値(yyyy-MM-dd HH:mm:ss)           |
| endSendDate      | String  | X    | 送信日の終了値(yyyy-MM-dd HH:mm:ss)           |
| startReceiveDate | String  | X    | 受信日の開始値(yyyy-MM-dd HH:mm:ss)           |
| endReceiveDate   | String  | X    | 受信日の終了値(yyyy-MM-dd HH:mm:ss)           |
| senderMail       | String  | X    | 送信メールアドレス                           |
| senderName       | String  | X    | 送信者名                             |
| receiveMail      | String  | X    | 受信メールアドレス                           |
| templateId       | String  | X    | テンプレート番号                             |
| sendStatus       | String  | X    | 送信ステータスコード<br/> SST0：送信準備、SST1：送信中、<br/> SST2：送信完了、SST3：送信失敗 |
| pageNum          | Integer | X    | ページ番号(デフォルト値：1)                           |
| pageSize         | Integer | X    | 照会数(デフォルト値：15)                            |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/sender/mails?startSendDate=2018-03-01+00%3A00&endSendDate=2018-03-07+23%3A59&pageSize=10"
```

#### レスポンス

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

| 値             | タイプ | 説明                                 |
| ------------------- | ------- | ---------------------------------------- |
| header              | Object  | ヘッダ領域                              |
| - isSuccessful      | Boolean | 成否                               |
| - resultCode        | Integer | 失敗コード                              |
| - resultMessage     | String  | 失敗メッセージ                             |
| body                | Object  | 本文領域                              |
| - pageNum           | Integer | 現在のページ番号                          |
| -pageSize           | Integer | 照会されたデータ数                           |
| - totalCount        | Integer | 総データ数                             |
| - data              | List    | データ領域                             |
| -- requestId        | String  | リクエストID                                    |
| -- requestDate      | String  | 送信日時                              |
| -- requestIp        | String  | 送信サーバーIP                                 |
| -- resultDate       | String  | 受信日時                              |
| -- templateId       | String  | テンプレートID                                   |
| -- templateName     | String  | テンプレート名                                |
| -- masterStatusCode | String  | メール送信準備ステータスコード(Y：送信準備、N：送信失敗)        |
| -- mailSeq          | String  | メールの順番                               |
| -- body             | String  | 本文内容                              |
| -- title            | String  | メールのタイトル                              |
| -- senderMail       | String  | 送信者のメールアドレス                          |
| -- senderName       | String  | 送信者名                             |
| -- receiverMail     | String  | 受信者のメールアドレス                          |
| -- receiveType      | String  | 受信者タイプ(MRT0：受信者、MRT1：CC、MRT2：BCC) |
| -- resultId         | String  | 送信結果ID                                 |
| -- resultDate       | String  | 送信完了日時                           |
| -- mailStatusCode   | String  | 送信ステータスコード<br/> SST0：送信準備、SST1：送信中、<br/> SST2：送信完了、SST3：送信失敗 |
| -- mailStatusName   | String  | 送信ステータス名                           |
| -- requestDate      | String  | 送信リクエスト日時                           |

### メール送信詳細の照会

#### リクエスト

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| GET         | /email/v1.1/appKeys/{appKey}/sender/mail/{requestId}/{mailSeq} |

[Path parameter]

| 値         | タイプ      | 説明     |
| --------- | ------- | ------ |
| appKey    | String  | 固有のアプリケーションキー |
| requestId | String  | リクエストID  |
| mailSeq   | Integer | メールの順番 |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/sender/mail/{requestId}/{mailSeq}"
```

#### レスポンス

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
                "filePath": String,
                "fileName": String,
                "fileSize": Integer,
                "fileSequence": Integer,
                "createDate": String,
                "updateDate": String
            }],
            "customHeaders": Map
        }
    }
}
```

| 値             | タイプ | 説明                                 |
| ------------------- | ------- | ---------------------------------------- |
| header              | Object  | ヘッダ領域                              |
| - isSuccessful      | Boolean | 成否                               |
| - resultCode        | Integer | 失敗コード                              |
| - resultMessage     | String  | 失敗メッセージ                             |
| body                | Object  | 本文領域                              |
| - data              | Object  | データ領域                             |
| -- mailStatusCode   | String  | 送信ステータス                            |
| -- mailStatusName   | String  | 送信ステータス名                           |
| -- templateId       | String  | テンプレートID                                   |
| -- templateName     | String  | テンプレート名                             |
| -- senderName       | String  | 送信者名                             |
| -- senderMail       | String  | 送信メールアドレス                           |
| -- title            | String  | メールのタイトル                              |
| -- body             | String  | メール内容                              |
| -- attachFileYn     | String  | 添付ファイルの有無                            |
| -- resultId         | String  | メール送信ID                                 |
| -- resultDate       | String  | 送信完了日時                           |
| -- receivers        | List    | 受信者リスト                             |
| --- requestId       | String  | リクエストID                                     |
| --- mailSequence    | Integer | メールの順番                               |
| --- receiveType     | String  | 受信者タイプ                             |
| --- receiveTypeName | String  | 受信者タイプ名                             |
| --- receiveName     | String  | 受信者名                             |
| --- receiveMailAddr | String  | 受信メールアドレス                           |
| --- readYn          | String  | 受信成否                               |
| --- readDate        | String  | 受信日(yyyy-MM-dd HH:mm:ss.SSS)           |
| -- attachFileList   | List    | 添付ファイルリスト                           |
| --- filePath        | String  | 添付ファイルのパス                           |
| --- fileName        | String  | 添付ファイル名                           |
| --- fileSize        | Integer | 添付ファイルのサイズ                           |
| --- fileSequence    | Integer | 添付ファイルの順番                            |
| --- createDate      | String  | 作成日時                              |
| --- updateDate      | String  | 修正日時                              |
| -- customHeaders    | Map     | [ユーザー指定ヘッダ](./Overview/#custom-header) |

#### v1.1の変更事項

* ユーザー指定ヘッダを表す**customHeaders**項目が追加されました。


### タグメール送信リクエストの照会

#### リクエスト

[URL]

| Http method | URI                                   |
| ----------- | ------------------------------------- |
| GET         | /email/v1.1/appKeys/{appKey}/tagMails |

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |

[Query parameter]

| 値             | タイプ      | 必須   | 説明                                       |
| ------------- | ------- | ---- | ---------------------------------------- |
| requestId     | String  | O    | リクエストID                                    |
| startSendDate | String  | O    | 送信日の開始値(yyyy-MM-dd HH:mm:ss)           |
| endSendDate   | String  | O    | 送信日の終了値(yyyy-MM-dd HH:mm:ss)           |
| senderMail    | String  | X    | 送信メールアドレス                          |
| senderName    | String  | X    | 送信者名                             |
| templateId    | String  | X    | テンプレートID                                   |
| sendStatus    | String  | X    | 送信ステータスコード<br/> WAIT：待機、READY：送信準備、<br/>SENDREADY：送信準備完了、SENDWAIT：送信待機、<br/>SENDING：送信中、COMPLETE：送信完了、<br/>FAIL：送信失敗、CANCEL：送信取消 |
| pageNum       | Integer | X    | ページ番号(デフォルト値：1)                           |
| pageSize      | Integer | X    | 照会数(デフォルト値：15)                            |

[注意]

* requestIdがある場合、startSendDateとendSendDateは必須値ではありません。
* startSendDateとendSendDateがある場合、requestIdは必須値ではありません。

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/tagMails?startSendDate=2018-03-01+00%3A00&endSendDate=2018-03-07+23%3A59&pageSize=10"
```

#### レスポンス

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

| 値               | タイプ        | 説明                                                         |
| ---------------- | ----------- | ------------------------------------------------------------ |
| header           | Object      | ヘッダ領域                                              |
| - isSuccessful   | Boolean     | 成否                                               |
| - resultCode     | Integer     | 失敗コード                                              |
| - resultMessage  | String      | 失敗メッセージ                                            |
| body             | Object      | 本文領域                                              |
| - pageNum        | Integer     | 現在のページ番号                                       |
| -pageSize        | Integer     | 照会されたデータ数                                        |
| - totalCount     | Integer     | 総データ数                                            |
| - data           | List        | データ領域                                            |
| -- requestId     | String      | リクエストID                                                      |
| -- requestIp     | String      | リクエストIP                                                      |
| -- requestDate   | String      | リクエスト時間                                              |
| -- tagSendStatus | String      | 送信ステータスコード<br/>WAIT：待機、READY：送信準備、<br/>SENDREADY：送信準備完了、SENDWAIT：送信待機、<br/>SENDING：送信中、COMPLETE：送信完了、<br/>FAIL：送信失敗、CANCEL：送信取消 |
| -- tagExpression | List:String | タグ表現式                                            |
| -- templateId    | String      | テンプレートID                                                    |
| -- templateName  | String      | テンプレート名                                                |
| -- senderName    | String      | 送信者名                                            |
| -- senderMail    | String      | 送信者のアドレス                                             |
| -- title         | String      | タイトル                                                   |
| -- body          | String      | 内容                                                   |
| -- attachYn      | String      | 添付ファイルの有無                                          |
| -- adYn          | String      | 広告かどうか                                               |
| -- createUser    | String      | 作成者                                                  |
| -- createDate    | String      | 作成日時                                              |
| -- updateUser    | String      | 修正者                                                  |
| -- updateDate    | String      | 修正日                                              |

### タグメールの送信受信者照会

#### リクエスト

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| GET         | /email/v1.1/appKeys/{appKey}/tagMails/{requestId} |

[Path parameter]

| 値         | タイプ     | 説明     |
| --------- | ------ | ------ |
| appKey    | String | 固有のアプリケーションキー |
| requestId | String | リクエストID  |

[Query parameter]

| 値                | タイプ      | 必須   | 説明                                       |
| ---------------- | ------- | ---- | ---------------------------------------- |
| receiveMail      | String  | X    | 受信メールアドレス                           |
| startReceiveDate | String  | X    | 受信日の開始値(yyyy-MM-dd HH:mm:ss)           |
| endReceiveDate   | String  | X    | 受信日の終了値(yyyy-MM-dd HH:mm:ss)           |
| receiveStatus    | String  | X    | 送信ステータスコード<br/> SST0：送信準備、SST1：送信中、<br/> SST2：送信完了、SST3：送信失敗 |
| pageNum          | Integer | X    | ページ番号(デフォルト値：1)                           |
| pageSize         | Integer | X    | 照会数(デフォルト値：15)                            |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/tagMails/{requestId}?startReceiveDate=2018-03-01+00%3A00&endReceiveDate=2018-03-07+23%3A59&pageSize=10"
```

#### レスポンス

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

| 値                 | タイプ      | 説明                                       |
| ----------------- | ------- | ---------------------------------------- |
| header            | Object  | ヘッダ領域                              |
| - isSuccessful    | Boolean | 成否                               |
| - resultCode      | Integer | 失敗コード                              |
| - resultMessage   | String  | 失敗メッセージ                             |
| body              | Object  | 本文領域                              |
| - pageNum         | Integer | 現在のページ番号                          |
| -pageSize         | Integer | 照会されたデータ数                           |
| - totalCount      | Integer | 総データ数                             |
| - data            | List    | データ領域                             |
| -- requestId      | String  | リクエストID                                    |
| -- mailSequence   | Integer | メールの順番                               |
| -- receiveMail    | String  | 受信者のアドレス                             |
| -- mailStatusCode | String  | メールステータスコード<br/> SST0：送信準備、SST1：送信中、<br/> SST2：送信完了、SST3：送信失敗 |
| -- mailStatusName | String  | メールのステータス名                           |
| -- resultId       | String  | SMTP ID                                  |
| -- resultDate     | String  | 実際の送信時間                           |
| -- readYn         | String  | 既読かどうか                                |
| -- readDate       | String  | 読んだ時間                              |
| -- createUser     | String  | 作成者                                 |
| -- createDate     | String  | 作成日時                              |
| -- updateUser     | String  | 修正者                                 |
| -- updateDate     | String  | 修正日時                              |

### タグメール送信詳細照会

#### リクエスト

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| GET         | /email/v1.1/appKeys/{appKey}/tagMails/{requestId}/{mailSequence} |

[Path parameter]

| 値            | タイプ      | 説明     |
| ------------ | ------- | ------ |
| appKey       | String  | 固有のアプリケーションキー |
| requestId    | String  | リクエストID  |
| mailSequence | Integer | メールの順番 |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/tagMails/{requestId}/{mailSequence}"
```

#### レスポンス

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
                    "filePath": String
                    "fileName": String
                    "fileSize": Integer,
                    "fileSequence": Integer,
                    "createDate": String
                    "updateDate": String
                }
            ],
            "customHeaders": Map
        }
    }
}
```

| 値             | タイプ | 説明                                 |
| ------------------- | ------- | ---------------------------------------- |
| header              | Object  | ヘッダ領域                              |
| - isSuccessful      | Boolean | 成否                               |
| - resultCode        | Integer | 失敗コード                              |
| - resultMessage     | String  | 失敗メッセージ                             |
| body                | Object  | 本文領域                              |
| - pageNum           | Integer | 現在のページ番号                          |
| -pageSize           | Integer | 照会されたデータ数                           |
| - totalCount        | Integer | 総データ数                             |
| - data              | List    | データ領域                             |
| -- requestId        | String  | リクエストID                                    |
| -- requestIp        | String  | リクエストIP                                    |
| -- templateId       | String  | テンプレートID                                   |
| -- templateName     | String  | テンプレート名                             |
| -- mailStatusCode   | String  | メールステータスコード<br/> SST0：送信準備、SST1：送信中、<br/> SST2：送信完了、SST3：送信失敗 |
| -- mailStatusName   | String  | メールのステータス名                           |
| -- requestDate      | String  | リクエスト時間                              |
| -- resultDate       | String  | 結果時間                              |
| -- senderName       | String  | 送信者名                             |
| -- senderMail       | String  | 送信者のアドレス                             |
| -- resultId         | String  | SMTP ID                                  |
| -- title            | String  | タイトル                                 |
| -- body             | String  | 内容                                 |
| -- receivers        | List    | 受信者リスト                             |
| --- requestId       | String  | リクエストID                                    |
| --- mailSequence    | Integer | メールの順番                               |
| --- receiveType     | String  | 受信者タイプ(MRT0：受信者、MRT1：CC、MRT2：BCC) |
| --- receiveTypeName | String  | 受信者タイプ名                          |
| --- receiveMailAddr | String  | 受信者のメールアドレス                          |
| --- readYn          | String  | 既読かどうか                               |
| --- readDate        | String  | 読んだ時間                              |
| -- attachFileList   | List    | 添付ファイルリスト                           |
| --- filePath        | String  | ファイルパス                              |
| --- fileName        | String  | ファイル名                                |
| --- fileSize        | Long    | ファイルサイズ                             |
| --- fileSequence    | Integer | ファイル番号                              |
| --- createDate      | String  | ファイル作成日時                           |
| --- updateDate      | String  | ファイル修正日時                           |
| -- customHeaders    | Map     | [ユーザー指定ヘッダ](./Overview/#custom-header)(v1.1からフィールド追加) |

#### v1.1の変更事項

* ユーザー指定ヘッダを表す**customHeaders**項目が追加されました。


## テンプレートの照会

### テンプレートリストの照会

#### リクエスト

[URL]

| Http method | URI                                    |
| ----------- | -------------------------------------- |
| GET         | /email/v1.1/appKeys/{appKey}/templates |

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |

[Query parameter]

| 値          | タイプ      | 必須   | 説明              |
| ---------- | ------- | ---- | --------------- |
| categoryId | Integer | X    | カテゴリーID         |
| useYn      | String  | X    | 使用有無(Y/N)      |
| pageNum    | Integer | X    | ページ番号(デフォルト値：1)  |
| pageSize   | Integer | X    | 照会数(デフォルト値：15)   |
| all        | Boolean | X    | 全てのテンプレートリストを照会するかどうか |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/templates?useYn=Y&pageNum=1&pageSize=10"
```

#### レスポンス

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

| 値                 | タイプ      | 説明              |
| ----------------- | ------- | --------------- |
| header            | Object  | ヘッダ領域     |
| - isSuccessful    | Boolean | 成否      |
| - resultCode      | Integer | 失敗コード     |
| - resultMessage   | String  | 失敗メッセージ    |
| body              | Object  | 本文領域     |
| - pageNum         | Integer | 現在のページ番号 |
| -pageSize         | Integer | 照会されたデータ数  |
| - totalCount      | Integer | 総データ数    |
| -data             | List    | データ領域    |
| --templateId      | String  | テンプレートID          |
| --serviceId       | Integer | サービスID          |
| --categoryId      | Integer | カテゴリーID         |
| --categoryName    | String  | カテゴリー名   |
| --sort            | Integer | 順序        |
| --templateName    | String  | テンプレート名    |
| --templateDesc    | String  | テンプレートの説明    |
| -- useYn          | String  | 使用有無       |
| --sendMailAddress | String  | テンプレート送信メールアドレス |
| --title           | String  | メールのタイトル     |
| --body            | String  | メール本文     |
| --attachFileYn    | String  | テンプレート添付ファイルの追加有無 |
| --delYn           | String  | 削除されているかどうか       |
| --createDate      | String  | 作成日時     |
| --updateDate      | String  | 修正日時     |

### テンプレート詳細照会

#### リクエスト

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| GET         | /email/v1.1/appKeys/{appKey}/templates/{templateId} |

[Path parameter]

| 値    | タイプ | 説明 |
| ---------- | ------ | ------ |
| appKey     | String | 固有のアプリケーションキー |
| templateId | String | テンプレートID |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/templates/{templateId}"
```

#### レスポンス

```
{
    "header": {
        "isSuccessful": boolean,
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

| 値                 | タイプ      | 説明                       |
| ----------------- | ------- | ------------------------ |
| header            | Object  | ヘッダ領域              |
| - isSuccessful    | Boolean | 成否               |
| - resultCode      | Integer | 失敗コード              |
| - resultMessage   | String  | 失敗メッセージ             |
| body              | Object  | 本文領域              |
| -data             | List    | データ領域             |
| --templateId      | String  | テンプレートID                   |
| --serviceId       | Integer | サービスID                   |
| --categoryId      | Integer | カテゴリーID                  |
| --categoryName    | String  | カテゴリー名            |
| --templateName    | String  | テンプレート名             |
| --templateDesc    | String  | テンプレート説明             |
| --useYn           | String  | 使用有無(Y：使用中、N：使用しない) |
| --sendMailAddress | String  | 送信メールアドレス          |
| --title           | String  | メールアドレス              |
| --body            | String  | メール内容              |
| --delYn           | String  | 削除されているかどうか                |
| --createDate      | String  | 作成日時              |
| --createUser      | String  | 作成者                 |
| --updateDate      | String  | 修正日時              |
| --updateUser      | String  | 修正者                 |
| --attachFileList  | List    | 添付ファイルリスト           |
| ---fileId         | Integer | 添付ファイルID                 |
| ---serviceId      | Integer | サービスID                   |
| ---templateId     | String  | テンプレートID                   |
| ---filePath       | String  | 添付ファイルのパス           |
| ---fileName       | String  | 添付ファイル名           |
| ---fileSize       | Integer | 添付ファイルのサイズ           |
| ---createDate     | String  | 作成日時              |
| ---createUser     | String  | 作成者                 |
| ---updateDate     | String  | 修正日時              |
| ---updateUser     | String  | 修正者                 |

## タグ管理

### タグ照会

#### リクエスト

[URL]

| Http method | URI                               |
| ----------- | --------------------------------- |
| GET         | /email/v1.1/appKeys/{appKey}/tags |

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |

[Query parameter]

| 値  | タイプ | 必須 | 説明       |
| -------- | ------- | ---- | -------------- |
| pageNum  | Integer | X    | ページ番号(デフォルト値：1) |
| pageSize | Integer | X    | 照会数(デフォルト値：15)  |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/tags?pageNum=1&pageSize=10"
```

#### レスポンス

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

| 値               | タイプ      | 説明     |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - isSuccessful  | Boolean | 成否 |
| - resultCode    | Integer | 失敗コード |
| - resultMessage | String  | 失敗メッセージ |
| body            | Object  | 本文領域 |
| - data          | List    | データ領域 |
| -- tagId        | String  | タグID  |
| -- tagName      | String  | タグ名 |
| -- createdDate  | String  | 作成日時 |
| -- updatedDate  | String  | 修正日時 |

### タグ登録

#### リクエスト

[URL]

| Http method | URI                               |
| ----------- | --------------------------------- |
| POST        | /email/v1.1/appKeys/{appKey}/tags |

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |

[Request body]

| 値       | タイプ     | 必須   | 説明    |
| ------- | ------ | ---- | ----- |
| tagName | String | O    | タグ名 |

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/tags -d '{"tagName":"サンプルタグ"}'
```

#### レスポンス

```
{
    "header": {
        "isSuccessful": boolean,
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

| 値               | タイプ      | 説明     |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - isSuccessful  | Boolean | 成否 |
| - resultCode    | Integer | 失敗コード |
| - resultMessage | String  | 失敗メッセージ |
| body            | Object  | 本文領域 |
| - data          | List    | データ領域 |
| -- tagId        | String  | タグID  |

### タグの修正

#### リクエスト

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| PUT         | /email/v1.1/appKeys/{appKey}/tags/{tagId} |

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |
| tagId  | String | タグID  |

[Request body]

| 値       | タイプ     | 必須   | 説明    |
| ------- | ------ | ---- | ----- |
| tagName | String | O    | タグ名 |

[例]
```
curl -X PUT -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/tags/{tagId} -d '{"tagName":"サンプルタグ2"}'
```

#### レスポンス

```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    }
}
```

| 値               | タイプ      | 説明     |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - isSuccessful  | Boolean | 成否 |
| - resultCode    | Integer | 失敗コード |
| - resultMessage | String  | 失敗メッセージ |

### タグの削除

#### リクエスト

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| DELETE      | /email/v1.1/appKeys/{appKey}/tags/{tagId} |

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |
| tagId  | String | タグID  |

[例]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/tags/{tagId}
```

#### レスポンス

```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    }
}
```

| 値               | タイプ      | 説明     |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - isSuccessful  | Boolean | 成否 |
| - resultCode    | Integer | 失敗コード |
| - resultMessage | String  | 失敗メッセージ |

## UIDの管理

### UIDの照会

#### リクエスト

[URL]

| Http method | URI                               |
| ----------- | --------------------------------- |
| GET         | /email/v1.1/appKeys/{appKey}/uids |

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |

[Query parameter]

| 値   | タイプ    | 必須 | 説明                                 |
| --------- | ----------- | ---- | ---------------------------------------- |
| wheres    | List:String | X    | 照会条件。</br>英数字、括弧で構成された文字列。</br>括弧は1個、AND、ORは3個まで使用できる。</br>例) tagId1,AND,tagId2 |
| offsetUid | String      | X    | offset UID                               |
| offset    | Integer     | X    | offset(デフォルト値：0)                           |
| limit     | Integer     | X    | 照会数(デフォルト値：15)                            |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/uids?wheres=tagId1,OR,tagId2&offset=0&limit=10"
```

#### レスポンス

```
{
    "header": {
        "isSuccessful": boolean,
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

| 値                | タイプ      | 説明                    |
| ---------------- | ------- | --------------------- |
| header           | Object  | ヘッダ領域           |
| - isSuccessful   | Boolean | 成否            |
| - resultCode     | Integer | 失敗コード           |
| - resultMessage  | String  | 失敗メッセージ          |
| body             | Object  | 本文領域           |
| - data           | List    | データ領域          |
| -- uids          | List    | UIDリスト          |
| --- uid          | String  | UID                   |
| --- tags         | List    | タグ情報リスト        |
| ---- tagId       | String  | タグID                 |
| ---- tagName     | String  | タグ名           |
| ---- createDate  | String  | タグ作成日時        |
| ---- updateDate  | String  | タグ修正日時        |
| --- contacts     | List    | 連絡先リスト          |
| ---- contactType | String  | 連絡先タイプ(EMAIL_ADDRESS) |
| ---- contact     | String  | 連絡先(メールアドレス)            |
| ---- createDate  | String  | 連絡先作成日時       |
| -- isLast        | Boolean | 最後のリストかどうか        |
| -- totalCount    | Integer | 総データ数          |

### UID単件照会

#### リクエスト

[URL]

| Http method | URI                                     |
| ----------- | --------------------------------------- |
| GET         | /email/v1.1/appKeys/{appKey}/uids/{uid} |

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |
| uid    | String | UID    |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/uids/{uid}"
```

#### レスポンス

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

| 値               | タイプ      | 説明         |
| --------------- | ------- | ---------- |
| header          | Object  | ヘッダ領域 |
| - isSuccessful  | Boolean | 成否 |
| - resultCode    | Integer | 失敗コード |
| - resultMessage | String  | 失敗メッセージ |
| body            | Object  | 本文領域 |
| - data          | List    | データ領域 |
| -- uid          | String  | UID        |
| -- tags         | List    | タグ情報リスト |
| --- tagId       | String  | タグID      |
| --- tagName     | String  | タグ名 |
| --- createDate  | String  | タグ作成日時 |
| --- updateDate  | String  | タグ修正日時 |
| -- contacts     | List    | 連絡先リスト |
| --- contactType | String  | 連絡先タイプ |
| --- contact     | String  | 連絡先(メールアドレス) |
| --- createDate  | String  | 連絡先作成日時 |

### UIDの登録

#### リクエスト

[URL]

| Http method | URI                               |
| ----------- | --------------------------------- |
| POST        | /email/v1.1/appKeys/{appKey}/uids |

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |

[Request body]

| 値      | タイプ    | 必須 | 説明   |
| ------------ | ----------- | ---- | ---------- |
| uid          | String      | O    | UID        |
| tagIds       | List:String | O    | タグIDリスト |
| contacts     | List        | O    | メールアドレスリスト |
| -contactType | String      | O    | 連絡先タイプ |
| -contact     | String      | O    | 連絡先(メールアドレス) |

[注意]

* tagIdsが与えられる場合、contactsは必須値ではありません。
* contactsが与えられる場合、tagIdsは必須値ではありません。
* 本サービスの場合、contactTypeは必ず"EMAIL_ADDRESS"値でリクエストする必要があります。

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/uids -d '{"uids":[{"uid":"sample-uid","tagIds":["tagId1"],"contacts":[{"contactType":"EMAIL_ADDRESS","contact":"customer1@nhnent.com"},{"contactType":"EMAIL_ADDRESS","contact":"customer2@nhnent.com"}]}]}'
```

#### レスポンス

```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    }
}
```

| 値               | タイプ      | 説明     |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - isSuccessful  | Boolean | 成否 |
| - resultCode    | Integer | 失敗コード |
| - resultMessage | String  | 失敗メッセージ |

### UIDの削除

#### リクエスト

[URL]

| Http method | URI                                     |
| ----------- | --------------------------------------- |
| DELETE      | /email/v1.1/appKeys/{appKey}/uids/{uid} |

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |
| uid    | String | UID    |

[例]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/uids/{uid}
```

#### レスポンス

```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    }
}
```

| 値               | タイプ      | 説明     |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - isSuccessful  | Boolean | 成否 |
| - resultCode    | Integer | 失敗コード |
| - resultMessage | String  | 失敗メッセージ |

### メールアドレスの登録

#### リクエスト

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| POST        | /email/v1.1/appKeys/{appKey}/uids/{uid}/email-addresses |

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |
| uid    | String | UID    |

[Request body]

| 値            | タイプ     | 必須   | 説明    |
| ------------ | ------ | ---- | ----- |
| emailAddress | String | O    | メールアドレス |

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/uids/{uid}/email-addresses -d '{"emailAddress" : "customer1@nhnent.com"}'
```

#### レスポンス

```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    }
}
```

| 値               | タイプ      | 説明     |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - isSuccessful  | Boolean | 成否 |
| - resultCode    | Integer | 失敗コード |
| - resultMessage | String  | 失敗メッセージ |

### メールアドレスの削除

#### リクエスト

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| DELETE      | /email/v1.1/appKeys/{appKey}/uids/{uid}/email-addresses/{emailAddress} |

[Path parameter]

| 値      | タイプ | 説明 |
| ------------ | ------ | ------ |
| appKey       | String | 固有のアプリケーションキー |
| uid          | String | UID    |
| emailAddress | String | メールアドレス |

[例]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/uids/{uid}/email-addresses/customer1@nhnent.com
```

#### レスポンス

```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    }
}
```

| 値               | タイプ      | 説明     |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - isSuccessful  | Boolean | 成否 |
| - resultCode    | Integer | 失敗コード |
| - resultMessage | String  | 失敗メッセージ |

## 統計照会

### 日別統計照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.1/appKeys/{appKey}/statistics/daily

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |

[Query parameter]

| 値       | タイプ     | 必須   | 説明                                       |
| ------- | ------ | ---- | ---------------------------------------- |
| from    | String | O    | 統計照会の開始日(年-月-日)<br/>(yyyy-MM-dd)      |
| to      | String | O    | 統計照会の終了日(年-月-日)<br/>(yyyy-MM-dd)      |
| type    | String | X    | 照会タイプ<br/>* REQUEST：リクエスト<br/>* SENT：送信<br/>* RECEIVED：受信<br/>* OPENED：開封 |
| filters | String | X    | 検索条件(多重パラメータ可能)<br/>* MAILTYPE：メールタイプ別(一般/大量)統計<br/>* TEMPLATE：テンプレート別統計 |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/statistics/daily?from=2018-03-01&to=2018-03-07&type=OPENED&filters=MAILTYPE&filters=TEMPLATE"
```

#### レスポンス

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

| 値         | タイプ      | 説明                                       |
| --------------- | ------- | ---------------------------------------- |
| header          | Object  | ヘッダ領域                              |
| - isSuccessful  | Boolean | 成否                               |
| - resultCode    | Integer | 失敗コード                              |
| - resultMessage | String  | 失敗メッセージ                             |
| body            | Object  | 本文領域                              |
| - data          | List    | データ領域                             |
| -- ymd          | String  | 統計日(yyyyMMdd)                          |
| -- appKey       | String  | アプリケーションキー                                 |
| -- type         | String  | 統計照会タイプ<br/>REQUEST：リクエスト<br/>SENT：送信<br/>RECEIVED：受信<br/>OPENED：開封 |
| -- mailType     | String  | メールタイプ<br/>* MAILTYPE：メールタイプ別(一般/大量)統計<br/>* TEMPLATE：テンプレート別統計 |
| -- templateId   | String  | テンプレートID                                   |
| -- templateName | String  | テンプレート名                             |
| -- tm00         | int     | 00:00:00 ~ 00:59:59                      |
| -- tm01         | int     | 01:00:00 ~ 01:59:59                      |
| -- tm02         | int     | 02:00:00 ~ 02:59:59                      |
| -- tm03         | int     | 03:00:00 ~ 03:59:59                      |
| -- tm04         | int     | 04:00:00 ~ 04:59:59                      |
| -- tm05         | int     | 05:00:00 ~ 05:59:59                      |
| -- tm06         | int     | 06:00:00 ~ 06:59:59                      |
| -- tm07         | int     | 07:00:00 ~ 07:59:59                      |
| -- tm08         | int     | 08:00:00 ~ 08:59:59                      |
| -- tm09         | int     | 09:00:00 ~ 09:59:59                      |
| -- tm10         | int     | 10:00:00 ~ 10:59:59                      |
| -- tm11         | int     | 11:00:00 ~ 11:59:59                      |
| -- tm12         | int     | 12:00:00 ~ 12:59:59                      |
| -- tm13         | int     | 13:00:00 ~ 13:59:59                      |
| -- tm14         | int     | 14:00:00 ~ 14:59:59                      |
| -- tm15         | int     | 15:00:00 ~ 15:59:59                      |
| -- tm16         | int     | 16:00:00 ~ 16:59:59                      |
| -- tm17         | int     | 17:00:00 ~ 17:59:59                      |
| -- tm18         | int     | 18:00:00 ~ 18:59:59                      |
| -- tm19         | int     | 19:00:00 ~ 19:59:59                      |
| -- tm20         | int     | 20:00:00 ~ 20:59:59                      |
| -- tm21         | int     | 21:00:00 ~ 21:59:59                      |
| -- tm22         | int     | 22:00:00 ~ 22:59:59                      |
| -- tm23         | int     | 23:00:00 ~ 23:59:59                      |
| -- sum          | int     | 合計                                 |

### 月別統計照会

#### リクエスト

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| GET         | /email/v1.1/appKeys/{appKey}/statistics/monthly |

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |

[Query parameter]

| 値       | タイプ     | 必須   | 説明                                       |
| ------- | ------ | ---- | ---------------------------------------- |
| from    | String | O    | 統計照会の開始日(年-月)<br/>(yyyy-MM)           |
| to      | String | O    | 統計照会の終了日(年-月)<br/>(yyyy-MM)           |
| type    | String | X    | 照会タイプ<br/>REQUEST：リクエスト<br/>SENT：送信<br/>RECEIVED：受信<br/>OPENED：開封 |
| filters | String | X    | 検索条件(多重パラメータ可能)<br/>* MAILTYPE：メールタイプ別(一般/大量)統計<br/>* TEMPLATE：テンプレート別統計 |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/statistics/monthly?from=2018-03&to=2018-04&type=OPENED&filters=MAILTYPE&filters=TEMPLATE"
```

#### レスポンス

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

| 値         | タイプ      | 説明                                       |
| --------------- | ------- | ---------------------------------------- |
| header          | Object  | ヘッダ領域                              |
| - isSuccessful  | Boolean | 成否                               |
| - resultCode    | Integer | 失敗コード                              |
| - resultMessage | String  | 失敗メッセージ                             |
| body            | Object  | 本文領域                              |
| - data          | List    | データ領域                             |
| -- ymd          | String  | 統計日(yyyyMMdd)                          |
| -- appKey       | String  | アプリケーションキー                                 |
| -- type         | String  | 統計照会タイプ<br/>* REQUEST：リクエスト<br/>* SENT：送信<br/>* RECEIVED：受信<br/>* OPENED：開封 |
| -- mailType     | String  | メールタイプ<br/>* MAILTYPE：メールタイプ別(一般/大量)統計<br/>* TEMPLATE：テンプレート別統計 |
| -- templateId   | String  | テンプレートID                                   |
| -- templateName | String  | テンプレート名                             |
| -- tm00         | int     | 00:00:00 ~ 00:59:59                      |
| -- tm01         | int     | 01:00:00 ~ 01:59:59                      |
| -- tm02         | int     | 02:00:00 ~ 02:59:59                      |
| -- tm03         | int     | 03:00:00 ~ 03:59:59                      |
| -- tm04         | int     | 04:00:00 ~ 04:59:59                      |
| -- tm05         | int     | 05:00:00 ~ 05:59:59                      |
| -- tm06         | int     | 06:00:00 ~ 06:59:59                      |
| -- tm07         | int     | 07:00:00 ~ 07:59:59                      |
| -- tm08         | int     | 08:00:00 ~ 08:59:59                      |
| -- tm09         | int     | 09:00:00 ~ 09:59:59                      |
| -- tm10         | int     | 10:00:00 ~ 10:59:59                      |
| -- tm11         | int     | 11:00:00 ~ 11:59:59                      |
| -- tm12         | int     | 12:00:00 ~ 12:59:59                      |
| -- tm13         | int     | 13:00:00 ~ 13:59:59                      |
| -- tm14         | int     | 14:00:00 ~ 14:59:59                      |
| -- tm15         | int     | 15:00:00 ~ 15:59:59                      |
| -- tm16         | int     | 16:00:00 ~ 16:59:59                      |
| -- tm17         | int     | 17:00:00 ~ 17:59:59                      |
| -- tm18         | int     | 18:00:00 ~ 18:59:59                      |
| -- tm19         | int     | 19:00:00 ~ 19:59:59                      |
| -- tm20         | int     | 20:00:00 ~ 20:59:59                      |
| -- tm21         | int     | 21:00:00 ~ 21:59:59                      |
| -- tm22         | int     | 22:00:00 ~ 22:59:59                      |
| -- tm23         | int     | 23:00:00 ~ 23:59:59                      |
| -- sum          | int     | 合計                                 |

### 統合統計照会

#### リクエスト

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| GET         | /email/v1.1/appKeys/{appKey}/statistics/view |

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |

[Query parameter]

| 値          | タイプ     | 必須   | 説明                                       |
| ---------- | ------ | ---- | ---------------------------------------- |
| from       | String | O    | 統計照会の開始日<br/> yyyy-mm-dd HH:mm        |
| to         | String | O    | 統計照会の終了日<br/> yyyy-mm-dd HH:mm        |
| searchType | String | O    | 統計区分<br/>DATE：日付別、TIME：時間別、DAY：曜日別 |
| mailTypes  | String | X    | メールタイプ<br/>NORMAL：一般、MASS：大量<br/>複数入力可能(mailTypes=NORMAL&mailTypes=MASS) |
|adYn | String | X | 広告かどうか<br>Y：広告、N：広告ではない<br>入力しなければ全て|
| templateId | String | X    | テンプレートID                                   |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/statistics/view?from=2018-03-21+00%3A00&to=2018-03-23+00%3A00&searchType=DATE&mailTypes=NORMAL&templateId=templateId1"
```

#### レスポンス

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

| 値                 | タイプ      | 説明              |
| ----------------- | ------- | --------------- |
| header            | Object  | ヘッダ領域     |
| - isSuccessful    | Boolean | 成否      |
| - resultCode      | Integer | 失敗コード     |
| - resultMessage   | String  | 失敗メッセージ    |
| body              | Object  | 本文領域     |
| - data            | List    | データ領域    |
| -- divisionName   | String  | 統計基準(日付/時間/曜日) |
| -- requestedCount | Long    | 送信リクエスト件数  |
| -- sentCount      | Long    | 送信件数     |
| -- receivedCount  | Long    | 受信件数     |
| -- openedCount    | Long    | 開封件数     |
| -- sentRate       | String  | 送信率        |
| -- receivedRate   | String  | 受信率        |
| -- openedRate     | String  | 開封率        |

## 受信拒否管理

### 受信拒否照会

#### リクエスト

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| GET         | /email/v1.1/appKeys/{appKey}/block-receivers |

[Path parameter]

| 値      | タイプ     | 説明     |
| ------ | ------ | ------ |
| appKey | String | 固有のアプリケーションキー |

[Query parameter]

| 値           | タイプ      | 必須   | 説明                       |
| ----------- | ------- | ---- | ------------------------ |
| mailAddress | String  | X    | 受信拒否リストに登録されているメールアドレス |
| pageNum     | Integer | X    | ページ番号(デフォルト値：1)           |
| pageSize    | Integer | X    | 照会数(デフォルト値：15)            |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/block-receivers?mailAddress=customer1@nhnent.com&pageNum=1&pageSize=10"
```

#### レスポンス
```
{
    "header": {
        "isSuccessful": boolean,
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

| 値               | タイプ      | 説明                              |
| --------------- | ------- | ------------------------------- |
| header          | Object  | ヘッダ領域                     |
| - isSuccessful  | Boolean | 成否                      |
| - resultCode    | Integer | 失敗コード                     |
| - resultMessage | String  | 失敗メッセージ                    |
| body            | Object  | 本文領域                     |
| - pageNum       | Integer | 現在のページ番号                 |
| -pageSize       | Integer | 照会されたデータ数                  |
| - totalCount    | Integer | 総データ数                    |
| - data          | List    | データ領域                    |
| -- mailAddress  | String  | 受信拒否メールアドレス              |
| -- blockDate    | String  | 受信拒否日(yyyy-MM-dd HH:mm:ss.S) |

### 受信拒否登録

#### リクエスト

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| POST        | /email/v1.1/appKeys/{appKey}/block-receivers |

[Request body]

| 値                 | タイプ     | 必須   | 説明                            |
| ----------------- | ------ | ---- | ----------------------------- |
| blockReceiverList | ㅣList  | O    | 受信拒否リスト                |
| - mailAddress     | String | O    | 受信拒否メールアドレス            |
| - blockDate       | String | X    | 受信拒否日(yyyy-MM-dd HH:mm:ss) |

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/block-receivers -d '{"blockReceiverList":[{"mailAddress":"customer1@nhnent.com","blockDate":"2018-03-01 00:00:00"}]}'
```

#### レスポンス
```
{
  "header": {
    "isSuccessful":  Boolean,
    "resultCode": Integer,
    "resultMessage": String
  }
}
```
| 値               | タイプ      | 説明     |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - isSuccessful  | Boolean | 成否 |
| - resultCode    | Integer | 失敗コード |
| - resultMessage | String  | 失敗メッセージ |

### 受信拒否削除
#### リクエスト

[URL]

| Http method | URI                                      |
| ----------- | ---------------------------------------- |
| PUT         | /email/v1.1/appKeys/{appKey}/block-receivers |

[Request body]

| 値                 | タイプ      | 必須   | 説明                |
| ----------------- | ------- | ---- | ----------------- |
| deleted           | Boolean | O    | 受信拒否削除を明示するフィールド |
| blockReceiverList | ㅣList   | O    | 受信拒否リスト    |
| - mailAddress     | String  | O    | 受信拒否メールアドレス |

[例]
```
curl -X PUT -H "Content-Type: application/json;charset=UTF-8" https://api-mail.cloud.toast.com/email/v1.1/appKeys/{appKey}/block-receivers -d '{"deleted":true,"blockReceiverList":[{"mailAddress":"customer1@nhnent.com"}]}'
```

#### レスポンス
```
{
  "header": {
    "isSuccessful":  Boolean,
    "resultCode": Integer,
    "resultMessage": String
  }
}
```
| 値               | タイプ      | 説明     |
| --------------- | ------- | ------ |
| header          | Object  | ヘッダ領域 |
| - isSuccessful  | Boolean | 成否 |
| - resultCode    | Integer | 失敗コード |
| - resultMessage | String  | 失敗メッセージ |
