## Notification > Email > API v1.5ガイド

[APIドメイン]

|環境|	ドメイン|
|---|---|
|Real|	https://email.api.nhncloudservice.com |

[Header]
```
Content-Type: application/json;charset=UTF-8
```


[curl例注意事項]

* Windows cmdではcurl例が正常にリクエストされないことがあります。

## メールの送信

### 一般メールの送信

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/sender/mail|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Request body]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|​senderAddress|	String|	O|	送信者のメール|
|senderName|	String|	X|	送信者名|
|requestDate|	String|	X|	送信日時が未入力の時は現在時間で送信(yyyy-MM-dd HH:mm:ss) |
|title|	String|	O|	タイトル|
|body|	String|	O|	内容|
|attachFileIdList|	List:Integer|	X|	アップロードした添付ファイルid|
|templateId|	String|	X|	送信テンプレートID|
|templateType| String| X| テンプレートタイプ <br/>DEFAULT(default)、FREEMARKER)|
|templateParameter|	Object|	X|	置換パラメータ(メールのタイトル/内容置換時に入力)|
|- #key#|	String|	X|	置換キー(##key##)|
|- #value#|	Object|	X|	置換キーにマッピングされるValue値|
|receiverList|	List|	O|	受信者リスト<br/>最大1000人まで送信可能(受信者、CC含む)|
|- receiveMailAddr|	String|	O|	受信者のメールアドレス|
|- receiveName|	String|	X|	受信者名|
|- receiveType|	String|	O|	受信者タイプ(MRT0：受信者、MRT1：CC、MRT2：BCC)|
|customHeaders| Map| X| [ユーザー指定ヘッダ](./Overview/#custom-header)|
|senderGroupingKey| String| X| 発信者グループキー |
|userId|	String|	X|	送信セパレータ ex)admin,system|


[注意]

* templateを使用する場合、title、bodyは任意のフィールドです。(入力時、入力された値がtemplateより優先適用)

[例1]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/mail -d '{"senderAddress":"support@nhnent.com","senderName":"送信者名","title":"サンプルタイトル","body":"サンプル内容","receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"顧客1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhnent.com","receiveName":"顧客2","receiveType":"MRT1"}],"userId":"USER"}'
```

[例2 - テンプレートの使用]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/mail -d '{"templateId":"TEMPLATE1","templateParameter":{"key":"value"},"receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"顧客1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhnent.com","receiveName":"顧客2","receiveType":"MRT1"}],"userId":"USER"}'
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- requestId|	String|	リクエストID|
|-- results|	List|	送信結果|
|--- receiveMailAddr|	String|	受信者のメールアドレス|
|--- receiveName|	String|	受信者名|
|--- receiveType|	String|	受信者タイプ(MRT0：受信者、MRT1：CC、MRT2：BCC)|
|--- resultCode|	Integer|	受信者送信リクエスト結果コード|
|--- resultMessage|	String|	受信者送信リクエスト結果メッセージ|

#### v1.5の変更事項

* 発信者グループキーフィールドを追加しました。リクエスト単位で設定できます。
* 送信リクエストをする時、**senderGroupingKey**フィールドを指定して、リクエスト照会時に活用できます。

### 個別メール送信

* 受信者が複数人いる時、それぞれの受信者に個別にメールを送信する機能です。複数人に送っても、受信者には本人のみ表示されます。

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/sender/eachMail||

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Request body]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|​senderAddress|	String|	O|	送信者のメール|
|senderName|	String|	X|	送信者名|
|requestDate|	String|	X|	送信日時、未入力時は現在時間で送信(yyyy-MM-dd HH:mm:ss)|
|title|	String|	O|	タイトル|
|body|	String|	O|	内容|
|attachFileIdList|	List:Integer|	X|	アップロードした添付ファイルid|
|templateType| String| X| テンプレートタイプ <br/>DEFAULT(default)、FREEMARKER)|
|templateId|	String|	X|	送信テンプレートID|
|receiverList|	List|	O|	受信者リスト<br/>最大1000人まで送信可能|
|- receiveMailAddr|	String|	O|	受信者のメールアドレス|
|- receiveName|	String|	X|	受信者名|
|- templateParameter|	Object|	X|	置換パラメータ(メールのタイトル/内容置換時の入力)|
|-- #key#|	String|	X|	置換キー(##key##)|
|-- #value#|	Object|	X|	置換キーにマッピングされるValue値|
|customHeaders| Map| X| [ユーザー指定ヘッダ](./Overview/#custom-header)|
|senderGroupingKey| String| X| 発信者グループキー |
|userId|	String|	X|	送信セパレータ ex)admin,system|

[注意]

* templateを使用する場合、title、bodyは必須から除外(入力時に入力された値がtemplateより優先適用)


[例1]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/eachMail -d '{"senderAddress":"support@nhnent.com","senderName":"送信者名","title":"サンプルタイトル","body":"サンプル内容","attachFileIdList":[1, 2],"receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"顧客1"}],"userId":"USER"}'
```

[例2 - テンプレートの使用]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/mail -d '{"templateId":"TEMPLATE1","receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"顧客1","templateParameter":{"key":"value"}}],"userId":"USER"}'
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- requestId|	String|	リクエストID|
|-- results|	List|	送信結果|
|--- receiveMailAddr|	String|	受信者のメールアドレス|
|--- receiveName|	String|	受信者名|
|--- receiveType|	String|	受信者タイプ(MRT0：受信者、MRT1：CC、MRT2：BCC)|
|--- resultCode|	Integer|	受信者送信リクエスト結果コード|
|--- resultMessage|	String|	受信者送信リクエスト結果メッセージ|


#### v1.5の変更事項

* 発信者グループキーフィールドを追加しました。リクエスト単位で設定できます。
* 送信リクエストをする時、**senderGroupingKey**フィールドを指定して、リクエスト照会時に活用できます。

### 広告性一般メール送信
* リクエスト、レスポンス情報は、一般メール送信と同じです。

#### 広告メール送信時の注意事項
* タイトルに必ず(広告)文言を挿入する必要があります。
* 詳細は[[広告性メール送信](./console-guide/#_3)]を参照してください。

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/sender/ad-mail|

[例1]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/ad-mail -d '{"senderAddress":"support@nhnent.com","senderName":"送信者名","title":"(広告)サンプルタイトル","body":"サンプル内容<br /> ##BLOCK_RECEIVER_LINK## <br />##EN_BLOCK_RECEIVER_LINK##","receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"顧客1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhnent.com","receiveName":"顧客2","receiveType":"MRT1"}],"userId":"USER"}'
```

[例2 - テンプレートの使用]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/ad-mail -d '{"templateId":"TEMPLATE1","templateParameter":{"key":"value"},"receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"顧客1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhnent.com","receiveName":"顧客2","receiveType":"MRT1"}],"userId":"USER"}'
```

### 広告性個別メール送信

* URLの最後のみad-eachMailに変わり、残りは個別メール送信と同じです。

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/sender/ad-eachMail |

[例1]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/ad-eachMail -d '{"senderAddress":"support@nhnent.com","senderName":"送信者名","title":"(広告)サンプルタイトル","body":"サンプル内容<br /> ##BLOCK_RECEIVER_LINK## <br />##EN_BLOCK_RECEIVER_LINK##","attachFileIdList":[1, 2],"receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"顧客1"}],"userId":"USER"}'
```

[例2 - テンプレートの使用]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/ad-eachMail -d '{"templateId":"TEMPLATE1","receiverList":[{"receiveMailAddr":"customer1@nhnent.com","receiveName":"顧客1","templateParameter":{"key":"value"}}],"userId":"USER"}'
```

### 認証メールの送信

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/sender/auth-mail||

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Request body]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|​senderAddress|	String|	O|	送信者のメール|
|senderName|	String|	X|	送信者名|
|requestDate|	String|	X|	送信日時、未入力時は現在時間で送信(yyyy-MM-dd HH:mm:ss)|
|title|	String|	O|	タイトル|
|body|	String|	O|	内容|
|templateId|	String|	X|	送信テンプレートID|
|receiver|	Object|	O|	受信者 |
|- receiveMailAddr|	String|	O|	受信者のメールアドレス|
|- receiveName|	String|	X|	受信者名|
|- templateParameter|	Object|	X|	置換パラメータ(メールのタイトル/内容置換時の入力)|
|-- #key#|	String|	X|	置換キー(##key##)|
|-- #value#|	Object|	X|	置換キーにマッピングされるValue値|
|customHeaders| Map| X| [ユーザー指定ヘッダ](./Overview/#custom-header)|
|userId|	String|	X|	送信セパレータ ex)admin,system|

[注意]

* templateを使用する場合、title、bodyは必須から除外(入力時に入力された値がtemplateより優先適用)

#### 一般メールと異なる点
認証メールの性格上、次のように異なる特性があります。

* 単件送信(1人の受信者)のみ可能です。
* 添付ファイル機能をサポートしません。添付ファイルが含まれるテンプレートはサポートしません。

[例1]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/auth-mail -d '{"senderAddress":"support@nhnent.com","senderName":"送信者名","title":"サンプルタイトル","body":"サンプル内容","receiver":{"receiveMailAddr":"customer1@nhnent.com","receiveName":"顧客1"},"userId":"USER"}'
```

[例2 - テンプレートの使用]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/auth-mail -d '{"templateId":"TEMPLATE1","receiver":{"receiveMailAddr":"customer1@nhnent.com","receiveName":"顧客1","templateParameter":{"key":"value"}},"userId":"USER"}'
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- requestId|	String|	リクエストID|
|-- statusCode|	String|	リクエストステータスコード(Y：送信準備、N：送信準備失敗)|

### タグメールの送信

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/sender/tagMail|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

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

|値|	タイプ|	必須|	説明|
| - | - | - | - |
| senderAddress  | String | O|送信者のアドレス |
| senderName | String | X|送信者名 |
| requestDate | String | X|送信日時が未入力の時は現在時間で送信(yyyy-MM-dd HH:mm:ss) |
| title  | String | O|タイトル |
| body  | String | O|内容 |
| templateId  | String | X|テンプレートID |
| adYn  | String | X|広告かどうか(default 'N') |
| autoSendYn  | String | X|自動送信かどうか(default 'Y') |
| attachFileIdList  | List:Integer | X|添付ファイルリスト |
| tagExpression  | List:String | O|タグ表現式 (タグID含む) |
|customHeaders| Map| X| [ユーザー指定ヘッダ](./Overview/#custom-header)|
| userId  | String | X|送信セパレータ ex)admin,system|

[注意]

* templateを使用する場合、title、bodyは必須入力から除外(入力時に入力された値がtemplateより優先適用)


[例1]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/tagMail -d '{"senderAddress":"support@nhnent.com","senderName":"送信者名","title":"サンプルタイトル","body":"サンプル内容","attachFileIdList":[1, 2],"tagExpression":["tag1","AND","tag2"],"userId":"USER"}'
```

[例2 - テンプレートの使用]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/tagMail -d '{"templateId":"TEMPLATE1","tagExpression":["tag1","AND","tag2"],"userId":"USER"}'
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- requestId|	String|	リクエストID|

### 添付ファイルのアップロード

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/attachfile/binaryUpload|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Request body]

```
{
    "fileName": String,
    "createUser": String,
    "fileBody": Byte[]
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|fileName|	String|	O|	ファイル名|
|fileBody|	Byte[]|	O|	ファイルのByte[]値|
|createUser|	String|	X|	ファイルアップロードユーザー情報|

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/attachfile/binaryUpload -d '{"fileName":"file.csv","createUser":"USER","fileBody":[]}'
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
      "fileId": Integer,
      "fileName": String
    }
  }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- fileId|	Integer|	ファイルID|
|-- fileName|	String|	ファイル名|


### タイトル/本文置換

#### 基本タイプ
* (##置換Key##)形式で入力すると、ユーザーが入力した**templateParameter**で置換できます。
```
* title：##title_name##さん、こんにちは！
* body：##body_content## 送信します。
->
* title：クラウド顧客1さん、こんにちは！
* body：test2送信します。
```

#### FreeMarkerタイプ
* [FreeMarkerテンプレートエンジン](https://freemarker.apache.org/)をサポートします。
* テンプレート言語を使用してユーザーが入力した**templateParameter**に置換できます。
```
* title：${title_name}さん、こんにちは！
* body：${body_content}送信します。
->
* title：クラウド顧客1さん、こんにちは！
* body：test2送信します。
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

## メール照会

### メール送信リストの照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/sender/mails|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Query parameter]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|requestId|	String|	O|	リクエストID|
|startSendDate|	String|	O|	送信日の開始値(yyyy-MM-dd HH:mm:ss)|
|endSendDate|	String|	X|	送信日の終了値(yyyy-MM-dd HH:mm:ss)|
|startReceiveDate|	String|	X|	受信日の開始値(yyyy-MM-dd HH:mm:ss)|
|endReceiveDate|	String|	X|	受信日の終了値(yyyy-MM-dd HH:mm:ss)|
|senderAddress|	String|	X|	送信メールアドレス|
|senderName|	String|	X|	送信者名|
|receiveMailAddr|	String|	X|	受信メールアドレス|
|templateId|	String|	X|	テンプレート番号|
|mailStatusCode|	String|	X|	送信ステータスコード<br/> SST0：送信準備、SST1：送信中、<br/> SST2：送信完了、SST3：送信失敗|
|isReceived| Boolean| X | 受信情報 |
|isOpened| Boolean| X | 既読情報 |
|senderGroupingKey| String| X| 発信者グループキー|
|pageNum|	Integer|	X|	ページ番号(Default：1)|
|pageSize|	Integer|	X|	照会件数(Default：15)|

* **requestId**または**startSendDate**、**endSendDate**リクエストフィールドは必須です。

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/mails?startSendDate=2018-03-01+00%3A00&endSendDate=2018-03-07+23%3A59&pageSize=10"
```

#### レスポンス

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
                "mailStatusName": "",
                "isReceived": true,
                "resultDate": "2019-01-01 00:00:00",
                "isOpened": true,
                "openedDate": "2019-01-01 00:00:00",
                "receiveMailAddr": "receiver@nhn.com",
                "receiveType": "MRT0",
                "receiveTypeName": "",
                "receiveName": "Receiver",
                "senderGroupingKey": "A_GROUP"
            }
        ]
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- pageNum|	Integer|	現在のページ番号|
|- pageSize|	Integer|	照会されたデータ件数|
|- totalCount|	Integer|	総データ件数|
|- data|	List|	データ領域|
|-- requestId|	String|	リクエストID|
|-- mailSeq|	Integer|	メールの順番|
|-- requestDate|	String|	送信日時|
|-- templateId|	String|	テンプレートID|
|-- templateName|	String|	テンプレート名|
|-- senderName|	String|	送信者名|
|-- senderAddress|	String|	送信者のメールアドレス|
|-- title|	String|	メールのタイトル|
|-- mailStatusCode|	String|	送信ステータスコード<br/> SST0：送信準備、SST1：送信中 <br/> SST2：送信完了、SST3：送信失敗|
|-- mailStatusName|	String|	送信ステータス名|
|-- isReceived|	Boolean|	受信情報|
|-- resultDate|	String|	受信日時|
|-- isOpened| Boolean| 既読情報 |
|-- openedDate| String| 開封日時|
|-- receiveMailAddr|	String|	受信者のメールアドレス|
|-- receiveType|	String|	受信者タイプ(MRT0：受信者、MRT1：CC、MRT2：BCC)|
|-- receiveTypeName| String| 受信者タイプ名|
|-- receiveName| String| 受信者名|
|-- senderGroupingKey| String| 発信者グループキー|

#### v1.5の変更事項

* 発信者グループキーフィールド**senderGroupingKey**を追加しました。
* 受信情報フィールド**isReceived**を追加しました。
* 既読情報フィールド**isOpened**を追加しました。
* 開封日時フィールド**openedDate**を追加しました。
* リクエストフィールドのうち、送信ステータスコードを表すフィールドの名前を**mailStatusCode**に変更しました。
* 受信者のメールアドレスを表すフィールドの名前を**receiveMailAddr**に変更しました。
* 送信者のメールアドレスを表すフィールドの名前を**senderAddress**に変更しました。


### メール送信詳細の照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/sender/mail/{requestId}/{mailSeq}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|requestId|	String|	リクエストID|
|mailSeq|	Integer|	メールの順番|

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/mail/{requestId}/{mailSeq}"
```

#### レスポンス

``` json
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
            "mailStatusName": "",
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
                    "receiveTypeName": "",
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- requestId| String| リクエストID|
|-- mailSeq| Integer| メールの順番|
|-- requestIp| String| リクエストIP|
|-- requestDate| String| リクエスト時間|
|-- mailStatusCode|	String| 送信ステータスコード<br/> SST0：送信準備、SST1：送信中 <br/> SST2：送信完了、SST3：送信失敗|
|-- mailStatusName|	String|	送信ステータス名|
|-- templateId|	String|	テンプレートID|
|-- templateName|	String|	テンプレート名|
|-- senderName|	String|	送信者名|
|-- senderAddress|	String|	送信者のアドレス|
|-- resultId|	String|	送信結果ID|
|-- title|	String|	タイトル|
|-- body|	String|	内容|
|-- receiverList|	List|	受信者リスト|
|--- requestId|	String|	リクエストID|
|--- mailSeq|	Integer|	メールの順番|
|--- receiveType|	String|	受信者タイプ(MRT0：受信者、MRT1：CC、MRT2：BCC)|
|--- receiveTypeName| String| 受信者タイプ名|
|--- receiveName| String| 受信者名|
|--- receiveMailAddr|	String|	受信者メールアドレス|
|--- isReceived| Boolean| 受信情報|
|--- resultDate| String| 受信日時|
|--- isOpened| Boolean| 既読情報|
|--- openedDate| String| 開封日時|
|-- attachFileList|	List| 添付ファイルリスト|
|--- fileType|	String|	添付ファイルタイプ <br/>MAIL: メールに添付されたファイル、TEMPLATE：テンプレートに添付されたファイル|
|--- fileId| Integer| ファイルID|
|--- fileName|	String|	添付ファイル名|
|--- filePath|	String|	添付ファイルパス|
|--- fileSize|	Integer| 添付ファイルサイズ(byte)|
|--- createDate|	String|	作成日時|
|-- customHeaders|	Map|	[ユーザー指定ヘッダ](./Overview/#custom-header) |
|-- senderGroupingKey|	String|	発信者グループキー|

#### v1.5에서 달라진 사항

* 発信者グループキーフィールド**senderGroupingKey**を追加しました。
* 受信者リストのフィールドの名前を**receiverList**に変更しました。
* 受信情報フィールド**isReceived**を追加しました。
* 受信者の既読情報フィールドの名前を**isOpened**に変更しました。
* 受信者の開封日時のフィールドの名前を**openedDate**に変更しました。
* 送信者のメールアドレスを表すフィールドの名前を**senderAddress**に変更しました。


### タグメール送信リクエストの照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/tagMails|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Query parameter]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|requestId|	String|	O|	リクエストID|
|startSendDate|	String|	O|	送信日の開始値(yyyy-MM-dd HH:mm:ss)|
|endSendDate|	String|	O|	送信日終了値(yyyy-MM-dd HH:mm:ss)|
|senderMail|	String|	X|	送信メールアドレス|
|senderName|	String|	X|	送信者名|
|templateId|	String|	X|	テンプレートID|
|sendStatus|	String|	X|	送信ステータスコード<br/> WAIT：待機、READY：送信準備、<br/>SENDREADY：送信準備完了、SENDWAIT：送信待機、<br/>SENDING：送信中、COMPLETE：送信完了、<br/>FAIL：送信失敗、CANCEL：送信取消|
|pageNum|	Integer|	X|	ページ番号(Default：1)|
|pageSize|	Integer|	X|	照会件数(Default：15)|

[注意]

* requestIdがある場合、startSendDateとendSendDateは必須値ではありません。
* startSendDateとendSendDateがある場合、requestIdは必須値ではありません。

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/tagMails?startSendDate=2018-03-01+00%3A00&endSendDate=2018-03-07+23%3A59&pageSize=10"
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- pageNum|	Integer|	現在のページ番号|
|- pageSize|	Integer|	照会されたデータ件数|
|- totalCount|	Integer|	総データ件数|
|- data|	List|	データ領域|
|-- requestId | String  | リクエストID |
|-- requestIp |  String  | リクエストIP |
|-- requestDate |  String  | リクエスト時間 |
|-- tagSendStatus |  String  | 送信ステータスコード<br/> WAIT：待機、READY：送信準備、<br/>SENDREADY：送信準備完了、SENDWAIT：送信待機、<br/>SENDING：送信中、COMPLETE：送信完了、<br/>FAIL：送信失敗、CANCEL：送信取消 |
|-- tagExpression |  List:String  | タグ表現式 (タグID含む) |
|-- templateId |  String  | テンプレートID |
|-- templateName |  String  | テンプレート名 |
|-- senderName |  String  | 送信者名 |
|-- senderMail |  String  | 送信者のアドレス |
|-- title |  String  | タイトル |
|-- body |  String  | 内容 |
|-- attachYn |  String  | 添付ファイルの有無 |
|-- adYn |  String  | 広告かどうか |
|-- createUser |  String  | 作成者 |
|-- createDate |  String  | 作成日時 |
|-- updateUser |  String  | 修正者 |
|-- updateDate |  String  | 修正日時 |

### タグメールの送信受信者照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/tagMails/{requestId}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|requestId|	String|	リクエストID|

[Query parameter]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|receiveMail|	String|	X|	受信メールアドレス|
|startReceiveDate|	String|	X|	受信日の開始値(yyyy-MM-dd HH:mm:ss)|
|endReceiveDate|	String|	X|	受信日の終了値(yyyy-MM-dd HH:mm:ss)|
|receiveStatus|	String|	X|	送信ステータスコード<br/> SST0：送信準備、SST1：送信中<br/> SST2：送信完了、SST3：送信失敗|
|pageNum|	Integer|	X|	ページ番号(Default：1)|
|pageSize|	Integer|	X|	照会件数(Default：15)|

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/tagMails/{requestId}?startReceiveDate=2018-03-01+00%3A00&endReceiveDate=2018-03-07+23%3A59&pageSize=10"
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- pageNum|	Integer|	現在のページ番号|
|-pageSize|	Integer|	照会されたデータ件数|
|- totalCount|	Integer|	総データ件数|
|- data|	List|	データ領域|
|-- requestId | String  | リクエストID |
|-- mailSequence | Integer  | メールの順番 |
|-- receiveMail | String  | 受信者のアドレス |
|-- mailStatusCode | String  | メールステータスコード<br/> SST0：送信準備、SST1：送信中、<br/> SST2：送信完了、SST3：送信失敗|
|-- mailStatusName | String  | メールのステータス名 |
|-- resultId | String  | SMTP ID |
|-- resultDate | String  | 実際の送信時間 |
|-- readYn | String  | 既読かどうか |
|-- readDate | String  | 読んだ時間 |
|-- createUser |  String  | 作成者 |
|-- createDate |  String  | 作成日時 |
|-- updateUser |  String  | 修正者 |
|-- updateDate |  String  | 修正日時 |

### タグメール送信詳細照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/tagMails/{requestId}/{mailSequence}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|requestId|	String|	リクエストID|
|mailSequence|	Integer|	メールの順番|

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/tagMails/{requestId}/{mailSequence}"
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- pageNum|	Integer|	現在のページ番号|
|-pageSize|	Integer|	照会されたデータ件数|
|- totalCount|	Integer|	総データ件数|
|- data|	List|	データ領域|
|-- requestId  | String  | リクエストID |
|-- requestIp | String  | リクエストIP |
|-- templateId | String  | テンプレートID |
|-- templateName | String  | テンプレート名 |
|-- mailStatusCode | String  | メールステータスコード<br/> SST0：送信準備、SST1：送信中、<br/> SST2：送信完了、SST3：送信失敗 |
|-- mailStatusName | String  | メールのステータス名 |
|-- requestDate | String  | リクエスト時間 |
|-- resultDate | String  | 結果時間 |
|-- senderName | String  | 送信者名 |
|-- senderMail | String  | 送信者のアドレス |
|-- resultId | String  | SMTP ID |
|-- title | String  | タイトル |
|-- body | String  | 内容 |
|-- receivers | List| 受信者リスト|
|--- requestId | String  | リクエストID |
|--- mailSequence | Integer  | メールの順番 |
|--- receiveType | String  | 受信者タイプ(MRT0：受信者、MRT1：CC、MRT2：BCC) |
|--- receiveTypeName | String  | 受信者タイプ名 |
|--- receiveMailAddr | String  | 受信者メールアドレス |
|--- readYn | String  | 既読かどうか |
|--- readDate | String  | 読んだ時間 |
|-- attachFileList | List  | 添付ファイルリスト |
|--- fileType|	String|	添付ファイルタイプ(MAIL：メールに添付されたファイル、TEMPLATE：テンプレートに添付されたファイル)|
|--- fileId| Integer| ファイルID|
|--- fileName|	String|	添付ファイル名|
|--- filePath|	String|	添付ファイルパス|
|--- fileSize|	Integer|	添付ファイルサイズ(byte)|
|--- createDate|	String|	作成日時|
|-- customHeaders|	Map|	[ユーザー指定ヘッダ](./Overview/#custom-header) |

<p id="category"></p>

## カテゴリーの管理

### カテゴリーリストの照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v1.5/appKeys/{appKey}/categories|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|

[Query parameter]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|useYn|	String|	X|	使用有無Y、N |
|categoryParentId|	Integer|	X|	親カテゴリーID |
|pageNum|	Integer|	X|	ページ番号(デフォルト値：1)|
|pageSize|	Integer|	X|	照会件数(デフォルト値：15)|

[例]
``` sh
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/categories?useYn=Y&categoryParentId=1&pageNum=1&pageSize=10"
```

#### レスポンス

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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- pageNum|	Integer|	現在のページ番号|
|- pageSize|	Integer|	照会されたデータ件数|
|- totalCount|	Integer|	総データ件数|
|- data|	List|	データ領域|
|-- categoryId|	Integer|	カテゴリーID|
|-- categoryParentId|	Integer| 親カテゴリーID(最上位カテゴリーの場合は0)|
|-- depth|	Integer| 深さ(depth)(最上位カテゴリーの場合は0) |
|-- categoryName|	String|	カテゴリー名|
|-- categoryDesc|	String|	カテゴリーの説明|
|-- useYn|	String|	使用有無|
|-- createUser|	String|	作成者|
|-- createDate|	String|	作成日時|
|-- updateUser|	String|	修正者|
|-- updateDate|	String|	修正日時|

### カテゴリー詳細照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/categories/{categoryId}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|
|categoryId|	String|	カテゴリーID|

[例]
``` sh
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/categories/{categoryId}"
```

#### レスポンス

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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	List|	データ領域|
|-- categoryId|	Integer|	カテゴリーID|
|-- categoryParentId|	Integer| 親カテゴリーID(最上位カテゴリーの場合は0)|
|-- depth|	Integer| 深さ(depth)(最上位カテゴリーの場合は0) |
|-- categoryName|	String|	カテゴリー名|
|-- categoryDesc|	String|	カテゴリーの説明|
|-- useYn|	String|	使用有無|
|-- createUser|	String|	作成者|
|-- createDate|	String|	作成日時|
|-- updateUser|	String|	修正者|
|-- updateDate|	String|	修正日時|


### カテゴリーの登録

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|POST|/email/v1.5/appKeys/{appKey}/categories|


[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|

[Request body]

|値|	タイプ|	最大文字数 | 必須|	説明|
|---|---|---|---|---|
| categoryParentId |	Integer|	- | X |	親カテゴリーID <br/> 最上位カテゴリーID(デフォルト値) |
| categoryName |	String|	200 | O |	カテゴリー名 |
| categoryDesc |	String| 1000 |	X |	カテゴリーの説明|
| useYn |	String| 1 |	X|	使用有無Y(デフォルト値)、N|
| userId | String | 50 | X | ユーザーID |

[例]
``` sh
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/categories -d '{"categoryParentId":12345,"categoryName":"Category","categoryDesc":"Top Category","useYn":"Y","userId":"USER"}'
```


#### レスポンス

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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- categoryId|	Integer|	カテゴリーID|


### カテゴリーの修正

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|PUT|/email/v1.5/appKeys/{appKey}/categories/{categoryId}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|
|categoryId|	Integer|	カテゴリーID|

[Request body]

|値|	タイプ|	最大文字数 | 必須|	説明|
|---|---|---|---|---|
| categoryName |	String|	200 | X |	カテゴリー名 |
| categoryDesc |	String| 1000 |	X |	カテゴリーの説明|
| useYn |	String| 1 |	X|	使用有無Y、N|
| userId | String | 50 | X | ユーザーID |

[例]
``` sh
curl -X PUT -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/categories/{categoryId} -d '{"categoryName":"Category","categoryDesc":"Top Category","useYn":"Y","userId":"USER"}'
```

#### レスポンス

``` json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean| 成否 |
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|

### カテゴリーの削除

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|DELETE|/email/v1.5/appKeys/{appKey}/categories/{categoryId}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|
|categoryId|	Integer|	カテゴリーID|

[例]
``` sh
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/categories/{categoryId}
```

#### レスポンス

``` json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|

<p id="template"></p>

## テンプレートの照会

### テンプレートリスト照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/templates|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Query parameter]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|categoryId|	Integer|	X|	カテゴリーID|
|useYn|	String|	X|	使用有無(Y/N)|
|pageNum|	Integer|	X|	ページ番号(Default：1)|
|pageSize|	Integer|	X|	照会件数(Default：15)|
|all|	Boolean|	X|	全てのテンプレートリストを照会するかどうか|

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/templates?useYn=Y&pageNum=1&pageSize=10"
```

#### レスポンス

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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- pageNum|	Integer|	現在のページ番号|
|- pageSize|	Integer|	照会されたデータ件数|
|- totalCount|	Integer|	総データ件数|
|- data|	List|	データ領域|
|-- templateId|	String|	テンプレートID|
|-- categoryId|	Integer|	カテゴリーID|
|-- categoryName|	String|	カテゴリー名|
|-- templateName|	String|	テンプレート名|
|-- templateDesc|	String|	テンプレートの説明|
|-- useYn|	String|	使用有無|
|-- delYn|	String|	削除されているかどうか|
|-- title|	String|	メールのタイトル|
|-- createDate|	String|	作成日時|
|-- updateDate|	String|	修正日時|

### テンプレート詳細照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/templates/{templateId}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|templateId|	String|	テンプレートID|

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/templates/{templateId}"
```

#### レスポンス

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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	List|	データ領域|
|-- templateId|	String|	テンプレートID|
|-- categoryId|	Integer|	カテゴリーID|
|-- categoryName|	String|	カテゴリー名|
|-- templateName|	String|	テンプレート名|
|-- templateDesc|	String|	テンプレートの説明|
|-- useYn|	String|	使用有無(Y=使用中、N=使用しない)|
|-- delYn|	String|	削除されているかどうか(Y=削除、N=削除されていない)|
|-- sendMailAddress|	String|	送信メールアドレス|
|-- title|	String|	メールアドレス|
|-- templateType|	String|	テンプレートタイプ <br/>DEFAULT(default)、FREEMARKER)|
|-- body|	String|	メール内容|
|-- createDate|	String|	作成日時|
|-- updateDate|	String|	修正日時|
|-- attachFileList|	List|	添付ファイルリスト|
|--- fileType|	String|	添付ファイルタイプ(MAIL：メールに添付されたファイル、TEMPLATE：テンプレートに添付されたファイル)|
|--- fileId| Integer| ファイルID|
|--- fileName|	String|	添付ファイル名|
|--- filePath|	String|	添付ファイルパス|
|--- fileSize|	Integer|	添付ファイルサイズ(byte)|
|--- createDate|	String|	作成日時|

### テンプレートの登録

#### リクエスト

[URL]

|Http method|   URI|
|---|---|
|POST|  /email/v1.5/appKeys/{appKey}/templates|


[Path parameter]

|値| タイプ| 説明|
|---|---|---|
|appKey|    String| 固有のアプリケーションキー|

[Request body]

|値| タイプ| 最大文字数 | 必須| 説明|
|---|---|---|---|---|
| categoryId |  Integer|    - | O | カテゴリーID |
| templateId | String | 50 | O | テンプレートID |
| templateName |    String| 200 |   O | テンプレート名|
| templateDesc |    String| 4000 |  X | テンプレートの説明|
| useYn |   String| 1 | X| 使用有無Y(デフォルト値)、N|
| sendMailAddress | String| 300 | O| 発信メールアドレス |
| title | String | 500 | O | メールタイトル |
| templateType |    String| 10 |    X| テンプレートタイプ <br/>DEFAULT(デフォルト値)、FREEMARKER |
| body | String | - | O | メール本文 |
| attachFileIdList | List<Integer> | - | X | 添付ファイルID(fileId) |
| userId | String | 50 | X | ユーザーID |

[例]
``` sh
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/templates -d '{"categoryId":1,"templateId":"TEAMPLTE_ID","templateName":"テンプレート名","templateDesc":"テンプレートの説明","useYn":"Y","sendMailAddress":"test@nhn.com","title":"メールタイトル","templateType":"DEFAULT","body":"メールの内容","attachFileIdList":[1,2,3],"userId":"USER"}'
```


#### レスポンス

``` json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

|値| タイプ| 説明|
|---|---|---|
|header|    Object| ヘッダ領域|
|- isSuccessful|    Boolean| 成否 |
|- resultCode|  Integer|  失敗コード|
|- resultMessage|   String| 失敗メッセージ|

### テンプレート添付ファイルのアップロード

#### リクエスト

[URL]

|Http method|   URI|
|---|---|
|POST|  /email/v1.5/appKeys/{appKey}/templates/attachfile/binaryUpload|

[Path parameter]

|値| タイプ| 説明|
|---|---|---|
|appKey|    String| 固有のアプリケーションキー|

[Request body]

|値| タイプ| 最大文字数| 必須| 説明|
|---|---|---|---|---|
|fileName|  String| 100|O| ファイル名 |
|fileBody|  Byte[]| - |O| ファイルのバイト[]値 |
|userId|    String| 50|X| ユーザーID |

[例]
``` sh
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/templates/attachfile/binaryUpload -d '{"fileName":"file.csv","userId":"USER","fileBody":[]}'
```

#### レスポンス

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

|値| タイプ| 説明|
|---|---|---|
|header|    Object| ヘッダ領域|
|- isSuccessful|    Boolean| 成否 |
|- resultCode|  Integer|  失敗コード|
|- resultMessage|   String| 失敗メッセージ|
|body|  Object| 本文領域|
|- data|    Object| データ領域|
|-- fileId| Integer| ファイルID|
|-- fileName|   String| ファイル名|

[注意]

* ファイルをアップロードした後、テンプレートに添付すると、他のテンプレートにそのファイルを添付できません。添付されたファイルを修正するか、新たにアップロードした後に添付する必要があります。

### テンプレートの修正

#### リクエスト

[URL]

|Http method|   URI|
|---|---|
|PUT|   /email/v1.5/appKeys/{appKey}/templates/{templateId}|

[Path parameter]

|値| タイプ| 説明|
|---|---|---|
|appKey|    String| 固有のアプリケーションキー|
|templateId|    String| テンプレートID|

[Request body]

|値| タイプ| 最大文字数 | 必須| 説明|
|---|---|---|---|---|
| templateName |    String| 200 |   X | テンプレート名|
| templateDesc |    String| 4000 |  X | テンプレートの説明|
| useYn |   String| 1 | X | 使用有無Y、N|
| sendMailAddress | String| 300 | X| 発信メールアドレス |
| title | String | 500 | X | メールタイトル |
| templateType |    String| 10 |    X| テンプレートタイプ <br/>DEFAULT(デフォルト値)、FREEMARKER |
| body | String | - | X | メール本文 |
| attachFileIdList | List<Integer> | - | X | 添付ファイルID(fileId) |
| userId | String | 50 | X | ユーザーID |

[例]
``` sh
curl -X PUT -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/templates/{templateId} -d '{"templateName":"テンプレート名","templateDesc":"テンプレートの説明","useYn":"Y","sendMailAddress":"test@nhn.com","title":"メールタイトル","templateType":"DEFAULT","body":"メールの内容","attachFileIdList":[1,2,3],"userId":"USER"}'
```

#### レスポンス

``` json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

|値| タイプ| 説明|
|---|---|---|
|header|    Object| ヘッダ領域|
|- isSuccessful|    Boolean| 成否 |
|- resultCode|  Integer|  失敗コード|
|- resultMessage|   String| 失敗メッセージ|

### テンプレートの削除

#### リクエスト

[URL]

|Http method|   URI|
|---|---|
|DELETE|    /email/v1.5/appKeys/{appKey}/templates/{templateId}|

[Path parameter]

|値| タイプ| 説明|
|---|---|---|
|appKey|    String| 固有のアプリケーションキー|
|templateId|    String| テンプレートID|

[例]
``` sh
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/templates/{templateId}
```

#### レスポンス

``` json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

|値| タイプ| 説明|
|---|---|---|
|header|    Object| ヘッダ領域|
|- isSuccessful|    Boolean| 成否 |
|- resultCode|  Integer|  失敗コード|
|- resultMessage|   String| 失敗メッセージ|

## タグ管理

### タグ照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/tags|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Query parameter]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|pageNum|	Integer|	X|	ページ番号(Default：1)|
|pageSize|	Integer|	X|	照会件数(Default：15)|

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/tags?pageNum=1&pageSize=10"
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	List|	データ領域|
|-- tagId | String | タグID |
|-- tagName | String | タグ名 |
|-- createdDate | String | 作成日時 |
|-- updatedDate | String | 修正日時 |

### タグ登録

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/tags|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Request body]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|tagName|	String|	O|	タグ名|

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/tags -d '{"tagName":"サンプルタグ"}'
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	List|	データ領域|
|-- tagId | String | タグID |

##### Description
- タグは最大2,048個まで作成できます。

##### タグにUID追加作成
- タグにUIDを追加(append)すること。既存のUIDを追加するとUIDのタグは増えます。
- 1つのUIDの最大タグ数は16個。

### タグの修正

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|PUT|	/email/v1.5/appKeys/{appKey}/tags/{tagId}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|tagId|	String|	タグID|

[Request body]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|tagName|	String|	O|	タグ名|

[例]
```
curl -X PUT -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/tags/{tagId} -d '{"tagName":"サンプルタグ2"}'
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|

### タグの削除

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|DELETE|	/email/v1.5/appKeys/{appKey}/tags/{tagId}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|tagId|	String|	タグID|

[例]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/tags/{tagId}
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|

## UIDの管理

### UIDの照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/uids|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Query parameter]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|wheres|	List:String|	X|	照会条件。</br>英数字、括弧で構成された文字列。</br>括弧は1個、AND、ORは3個まで使用できる。</br> (例) tagId1,AND,tagId2|
|offsetUid|	String|	X|	offset UID|
|offset|	Integer|	X| offset(Default : 0)|
|limit|	Integer|	X|	照会件数(Default：15)|

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/uids?wheres=tagId1,OR,tagId2&offset=0&limit=10"
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	List|	データ領域|
|-- uids|	List|	UIDリスト|
|--- uid | String  | UID |
|--- tags | List | タグ情報リスト |
|---- tagId | String | タグID |
|---- tagName | String | タグ名 |
|---- createDate | String | タグ作成日時 |
|---- updateDate | String | タグ修正日時 |
|--- contacts | List | 連絡先リスト |
|---- contactType | String | 連絡先タイプ(EMAIL_ADDRESS)|
|---- contact | String | 連絡先(メールアドレス)) |
|---- createDate | String | 連絡先作成日時 |
|-- isLast | Boolean | 最後のリストかどうか |
|-- totalCount | Integer | 総データ件数 |

### UID単件照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/uids/{uid}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|uid|	String|	UID |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/uids/{uid}"
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	List|	データ領域|
|-- uid | String  | UID |
|-- tags | List | タグ情報リスト |
|--- tagId | String | タグID |
|--- tagName | String | タグ名 |
|--- createDate | String | タグ作成日時 |
|--- updateDate | String | タグ修正日時 |
|-- contacts | List | 連絡先リスト |
|--- contactType | String | 連絡先タイプ |
|--- contact | String | 連絡先(メールアドレス) |
|--- createDate | String | 連絡先作成日時 |

### UIDの登録

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/uids|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Request body]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|uid|	String|	O|	UID|
|tagIds|	List:String|	O|	タグIDリスト|
|contacts|	List|	O|	メールアドレスリスト |
|-contactType| String| O| 連絡先タイプ |
|-contact| String| O| 連絡先(メールアドレス) |

[注意]

* tagIdsが与えられている場合、contactsは必須値ではない。
* contactsが与えられている場合、tagIdsは必須値ではない。
* 本サービスの場合、contactTypeは必ず"EMAIL_ADDRESS"値でリクエストする必要がある。

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/uids -d '{"uids":[{"uid":"sample-uid","tagIds":["tagId1"],"contacts":[{"contactType":"EMAIL_ADDRESS","contact":"customer1@nhnent.com"},{"contactType":"EMAIL_ADDRESS","contact":"customer2@nhnent.com"}]}]}'
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|

### UIDの削除

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|DELETE|	/email/v1.5/appKeys/{appKey}/uids/{uid}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|uid|	String|	UID|

[例]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/uids/{uid}
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|

### メールアドレスの登録

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/uids/{uid}/email-addresses|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|uid|	String|	UID|

[Request body]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|emailAddress|	String|	O|	メールアドレス|

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/uids/{uid}/email-addresses -d '{"emailAddress" : "customer1@nhnent.com"}'
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|

### メールアドレスの削除

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|DELETE|	/email/v1.5/appKeys/{appKey}/uids/{uid}/email-addresses/{emailAddress}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|uid|	String|	UID|
|emailAddress|	String|	メールアドレス|

[例]
```
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/uids/{uid}/email-addresses/customer1@nhnent.com
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|

## 統計照会

### 統合統計照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/statistics/view |

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Query parameter]

|値|	タイプ|	必須| 説明|
|---|---|---|---|
|from|	String|	O | 統計照会の開始日<br/> yyyy-mm-dd HH:mm|
|to|	String|	O | 統計照会の終了日<br/> yyyy-mm-dd HH:mm|
|searchType| String | O | 統計区分<br/>DATE：日付別、TIME：時間別、DAY：曜日別 |
|mailTypes | String | X | メールタイプ<br/>NORMAL：一般、MASS：大量<br/>複数入力可能(mailTypes=NORMAL&mailTypes=MASS) |
|adYn | String | X | 広告かどうか<br>Y：広告、N：広告ではない<br>入力しなければ全て|
|templateId | String | X | テンプレートID |

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/statistics/view?from=2018-03-21+00%3A00&to=2018-03-23+00%3A00&searchType=DATE&mailTypes=NORMAL&adYn=Y&templateId=templateId1"
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	List|	データ領域|
|-- divisionName | String | 統計基準(日付/時間/曜日) |
|-- requestedCount | Long | 送信リクエストカウント |
|-- sentCount | Long | 送信カウント |
|-- receivedCount | Long | 受信カウント |
|-- openedCount | Long | 開封カウント |
|-- sentRate | String | 送信率 |
|-- receivedRate | String | 受信率 |
|-- openedRate | String | 開封率 |

## 受信拒否管理

### 受信拒否照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
| GET |	/email/v1.5/appKeys/{appKey}/block-receivers |

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Query parameter]

|値|	タイプ|	必須| 説明|
|---|---|---|---|
|mailAddress|	String|	X| 受信拒否リストに登録されているメールアドレス|
|startBlockDate|	String|	X| 受信拒否日開始値 (yyyy-MM-dd HH:mm:ss)|
|endBlockDate|	String|	X| 受信拒否日終了値 (yyyy-MM-dd HH:mm:ss)|
|pageNum|	Integer|	X|	ページ番号(Default：1)|
|pageSize|	Integer|	X|	照会件数(Default：15)|

[例]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/block-receivers?mailAddress=customer1@nhnent.com&pageNum=1&pageSize=10"
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- pageNum|	Integer|	現在のページ番号|
|-pageSize|	Integer|	照会されたデータ件数|
|- totalCount|	Integer|	総データ件数|
|- data|	List|	データ領域|
|-- mailAddress | String | 受信拒否メールアドレス |
|-- blockDate | String | 受信拒否日(yyyy-MM-dd HH:mm:ss.S)

### 受信拒否登録

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
| POST |	/email/v1.5/appKeys/{appKey}/block-receivers |

[Request body]

|値|	タイプ|	必須 | 説明|
|---|---|---|---|
| blockReceiverList | ㅣList | O | 受信拒否リスト |
| - mailAddress | String | O | 受信拒否メールアドレス |
| - blockDate | String | X | 受信拒否日(yyyy-MM-dd HH:mm:ss) |

[例]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/block-receivers -d '{"blockReceiverList":[{"mailAddress":"customer1@nhnent.com","blockDate":"2018-03-01 00:00:00"}]}'
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
|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|

### 受信拒否削除
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
| PUT |	/email/v1.5/appKeys/{appKey}/block-receivers |

[Request body]

|値|	タイプ|	必須 | 説明|
|---|---|---|---|
| deleted | Boolean | O | 受信拒否削除を明示するフィールド |
| blockReceiverList | ㅣList | O | 受信拒否リスト |
| - mailAddress | String | O | 受信拒否メールアドレス |

[例]
```
curl -X PUT -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/block-receivers -d '{"deleted":true,"blockReceiverList":[{"mailAddress":"customer1@nhnent.com"}]}'
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
|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
