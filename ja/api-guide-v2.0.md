<!-- pre-align:aligned sig=6dbb01fc078a -->

<a id="notification-email-api-v20-guide"></a>
## Notification > Email > API v2.0ガイド { #notification-email-api-v20-guide }

<a id="v20-api"></a>
### v2.0 APIの紹介 { #v20-api }

1. シークレットキー認証の導入
	* v2.0 APIを呼び出す際、ヘッダに[シークレットキー](./api-guide/#secret-key)を設定して呼び出す必要があります。
2. 大量送信照会APIの追加
	* 大量送信件に対する照会APIが追加されました。


[APIドメイン]

|環境|	ドメイン|
|---|---|
|Real|	https://email.api.nhncloudservice.com |

[Header]
```
Content-Type: application/json;charset=UTF-8
```

<p id="secret-key"></p>

<a id="secret-key"></a>
### Secret Key { #secret-key }
- コンソールで確認可能です。
- Secret Keyが必要なAPIを呼び出す際、ヘッダに以下のように設定して呼び出す必要があります。
```
Header
X-Secret-Key: [a-zA-Z0-9]{8}
```
**CONSOLE > Notification > Email > URL & AppKey** で確認/作成できます。


[curl例注意事項]

* Windows cmdではcurl例が正常にリクエストされないことがあります。

<a id="mail-delivery"></a>
## メールの送信 { #mail-delivery }

<a id="send-general-mails"></a>
### 一般メールの送信 { #send-general-mails }

<a id="send-general-mails-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/sender/mail|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

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
|customHeaders| Map| X| [ユーザー指定ヘッダ](./console-guide/#custom-header)|
|senderGroupingKey| String| X| 発信者グループキー |
|userId|	String|	X|	送信セパレータ ex)admin,system|
| statsId| String |X| 統計ID(発信検索条件には含まれません) |


[注意]

* テンプレートを使用する場合、**senderAddress、title、body**は必須値ではありません。この値を入力しない場合はテンプレートに登録された値を使用します。
* テンプレートを使用しながら、**senderAddress、senderName、title、body、templateType**を入力する場合は、テンプレートに登録された値より優先して適用されます。

[例1]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/mail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "senderAddress": "support@example.com",
    "senderName": "送信者名",
    "title": "サンプルタイトル",
    "body": "サンプル内容",
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "顧客1",
            "receiveType": "MRT0"
        }, {
            "receiveMailAddr": "customer2@example.com",
            "receiveName": "顧客2",
            "receiveType": "MRT1"
        }
    ],
    "userId": "USER",
    "statsId": "statsId"
}'
```

[例2 - テンプレートの使用]
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
            "receiveName": "顧客1",
            "receiveType": "MRT0"
        }, {
            "receiveMailAddr": "customer2@example.com",
            "receiveName": "顧客2",
            "receiveType": "MRT1"
        }
    ],
    "userId": "USER",
    "statsId": "statsId"
}'
```

<a id="send-general-mails-response"></a>
#### レスポンス

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

<a id="send-individual-mails"></a>
### 個別メール送信 { #send-individual-mails }

* 受信者が複数人いる時、それぞれの受信者に個別にメールを送信する機能です。複数人に送っても、受信者には本人のみ表示されます。

<a id="send-individual-mails-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/sender/eachMail||

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

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
|customHeaders| Map| X| [ユーザー指定ヘッダ](./console-guide/#custom-header)|
|senderGroupingKey| String| X| 発信者グループキー |
|userId|	String|	X|	送信セパレータ ex)admin,system|
|statsId| String |X| 統計ID(発信検索条件には含まれません) |

[注意]

* テンプレートを使用する場合、**senderAddress、title、body**は必須値ではありません。この値を入力しない場合はテンプレートに登録された値を使用します。
* テンプレートを使用しながら、**senderAddress、senderName、title、body、templateType**を入力する場合は、テンプレートに登録された値より優先して適用されます。

[例1]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/eachMail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "senderAddress": "support@example.com",
    "senderName": "送信者名",
    "title": "サンプルタイトル",
    "body": "サンプル内容",
    "attachFileIdList": [1, 2],
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "顧客1"
        }
    ],
    "userId": "USER",
    "statsId": "statsId"
}'
```

[例2 - テンプレートの使用]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/eachMail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "templateId": "TEMPLATE1",
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "顧客1",
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
#### レスポンス

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


<a id="sending-general-ad-mails"></a>
### 広告性一般メール送信 { #sending-general-ad-mails }
* リクエスト、レスポンス情報は、一般メール送信と同じです。

<a id="sending-general-ad-mails-caution-for-sending-ad-mails"></a>
#### 広告メール送信時の注意事項
* タイトルに必ず(広告)文言を挿入する必要があります。
* 詳細は[[広告性メール送信](./console-guide/#_3)]を参照してください。

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/sender/ad-mail|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

[例1]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/ad-mail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "senderAddress": "support@example.com",
    "senderName": "送信者名",
    "title": "(広告)サンプルタイトル",
    "body": "サンプル内容 <br /> ##BLOCK_RECEIVER_LINK## <br />##EN_BLOCK_RECEIVER_LINK##",
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "顧客1",
            "receiveType": "MRT0"
        }, {
            "receiveMailAddr": "customer2@example.com",
            "receiveName": "顧客2",
            "receiveType": "MRT1"
        }
    ],
    "userId": "USER",
    "statsId": "statsId"
}'
```

[例2 - テンプレートの使用]
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
            "receiveName": "顧客1",
            "receiveType": "MRT0"
        }, {
            "receiveMailAddr": "customer2@example.com",
            "receiveName": "顧客2",
            "receiveType": "MRT1"
        }
    ],
    "userId": "USER",
    "statsId": "statsId"
}'
```

<a id="sending-individual-ad-mails"></a>
### 広告性個別メール送信 { #sending-individual-ad-mails }

* URLの最後のみad-eachMailに変わり、残りは個別メール送信と同じです。

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/sender/ad-eachMail |

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

[例1]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/ad-eachMail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "senderAddress": "support@example.com",
    "senderName": "送信者名",
    "title": "(広告)サンプルタイトル",
    "body": "サンプル内容 <br /> ##BLOCK_RECEIVER_LINK## <br />##EN_BLOCK_RECEIVER_LINK##",
    "attachFileIdList": [1, 2],
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "顧客1"
        }
    ],
    "userId": "USER",
    "statsId": "statsId"
}'
```

[例2 - テンプレートの使用]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/ad-eachMail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "templateId": "TEMPLATE1",
    "receiverList": [{
            "receiveMailAddr": "customer1@example.com",
            "receiveName": "顧客1",
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
### 認証メールの送信 { #send-authenticated-mails }

<a id="send-authenticated-mails-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/sender/auth-mail||

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

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
|customHeaders| Map| X| [ユーザー指定ヘッダ](./console-guide/#custom-header)|
|senderGroupingKey| String| X| 送信者グループキー (最大100文字) |
|userId|	String|	X|	送信セパレータ ex)admin,system|
|statsId| String |X| 統計ID(発信検索条件には含まれません) |


[注意]

* テンプレートを使用する場合、**senderAddress、title、body**は必須値ではありません。この値を入力しない場合はテンプレートに登録された値を使用します。
* テンプレートを使用しながら、**senderAddress、senderName、title、body、templateType**を入力する場合は、テンプレートに登録された値より優先して適用されます。

<a id="send-authenticated-mails-differences-from-general-mails"></a>
#### 一般メールと異なる点
認証メールの性格上、次のように異なる特性があります。

* 単件送信(1人の受信者)のみ可能です。
* 添付ファイル機能をサポートしません。添付ファイルが含まれるテンプレートはサポートしません。

[例1]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/auth-mail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "senderAddress": "support@example.com",
    "senderName": "送信者名",
    "title": "サンプルタイトル",
    "body": "サンプル内容",
    "receiver": {
        "receiveMailAddr": "customer1@example.com",
        "receiveName": "顧客1"
    },
    "userId": "USER",
    "statsId": "statsId"
}'
```

[例2 - テンプレートの使用]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/auth-mail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "templateId": "TEMPLATE1",
    "receiver": {
        "receiveMailAddr": "customer1@example.com",
        "receiveName": "顧客1",
        "templateParameter": {
            "key": "value"
        }
    },
    "userId": "USER",
    "statsId": "statsId"
}'
```

<a id="send-authenticated-mails-response"></a>
#### レスポンス

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
<a id="upload-attached-files"></a>
### 添付ファイルのアップロード { #upload-attached-files }

<a id="upload-attached-files-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/attachfile/binaryUpload|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

[Request body]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|fileName|	String|	O|	ファイル名|
|fileBody|	Byte[]|	O|	ファイルのByte[]値|
|createUser|	String|	X|	ファイルアップロードユーザー情報|

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


<a id="titlebody-replacement"></a>
### タイトル/本文置換 { #titlebody-replacement }

<a id="titlebody-replacement-default-type"></a>
#### 基本タイプ
* (##置換Key##)形式で入力すると、ユーザーが入力した**templateParameter**で置換できます。
```
* title：##title_name##さん、こんにちは！
* body：##body_content## 送信します。
->
* title：クラウド顧客1さん、こんにちは！
* body：test2送信します。
```

<a id="titlebody-replacement-freemarker-type"></a>
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

<a id="titlebody-replacement-example-of-general-mail-request"></a>
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

<a id="titlebody-replacement-example-of-individual-mail-request"></a>
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

<a id="query-of-mails"></a>
## メール照会 { #query-of-mails }

<a id="query-list-of-mail-deliveries"></a>
### メール送信リストの照会 { #query-list-of-mail-deliveries }

<a id="query-list-of-mail-deliveries-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/sender/mails|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

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
|mailStatusCode|	String|	X|	送信ステータスコード<br/> SST0：送信準備、SST1：送信中、<br/> SST2：送信完了、SST3：送信失敗、 SST7：認証なし|
|isReceived| Boolean| X | 受信情報 |
|isOpened| Boolean| X | 既読情報 |
|senderGroupingKey| String| X| 発信者グループキー|
|pageNum|	Integer|	X|	ページ番号(Default：1)|
|pageSize|	Integer|	X|	照会件数(Default：15)|

* **requestId**または**startSendDate**、**endSendDate**リクエストフィールドは必須です。

<a id="query-list-of-mail-deliveries-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/mails?startSendDate='"${START_DATE}"'&endSendDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="query-list-of-mail-deliveries-response"></a>
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
                "senderGroupingKey": "A_GROUP",
                "dsnCode": "2.5.0",
                "dsnMessage": "SUCCESS"
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
|-- mailStatusCode|	String|	送信ステータスコード<br/> SST0：送信準備、SST1：送信中 <br/> SST2：送信完了、SST3：送信失敗、 SST7：認証なし|
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
|-- dsnCode| String| DSN(Delivery Status Notification) ステータスコード|
|-- dsnMessage| String| DSN(Delivery Status Notification) ステータスメッセージ |


<a id="query-mail-delivery-details"></a>
### メール送信詳細の照会 { #query-mail-delivery-details }

<a id="query-mail-delivery-details-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/sender/mail/{requestId}/{mailSeq}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|requestId|	String|	リクエストID|
|mailSeq|	Integer|	メールの順番|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

<a id="query-mail-delivery-details-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/mail/'"${REQUEST_ID}"'/'"${MAIL_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="query-mail-delivery-details-response"></a>
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
|-- mailStatusCode|	String| 送信ステータスコード<br/> SST0：送信準備、SST1：送信中 <br/> SST2：送信完了、SST3：送信失敗、 SST7：認証なし|
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
|--- dsnCode| String| DSN(Delivery Status Notification) ステータスコード|
|--- dsnMessage| String| DSN(Delivery Status Notification) ステータスメッセージ |
|-- attachFileList|	List| 添付ファイルリスト|
|--- fileType|	String|	添付ファイルタイプ <br/>MAIL: メールに添付されたファイル、TEMPLATE：テンプレートに添付されたファイル|
|--- fileId| Integer| ファイルID|
|--- fileName|	String|	添付ファイル名|
|--- filePath|	String|	添付ファイルパス|
|--- fileSize|	Integer| 添付ファイルサイズ(byte)|
|--- createDate|	String|	作成日時|
|-- customHeaders|	Map|	[ユーザー指定ヘッダ](./console-guide/#custom-header) |
|-- senderGroupingKey|	String|	発信者グループキー|


<a id="query-of-mails-1"></a>
### 大量メールリスト照会 { #query-of-mails-1 }

<a id="query-of-mails-1-1"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/mass-mails|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Query parameter]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|requestId|	String|	O|	リクエストID|
|startSendDate|	String|	O|	送信日の開始値(yyyy-MM-dd HH:mm:ss)|
|endSendDate|	String|	O|	送信日の終了値(yyyy-MM-dd HH:mm:ss)|
|senderMail|	String|	X|	送信メールアドレス|
|senderName|	String|	X|	送信者名|
|templateId|	String|	X|	テンプレートID|
|sendStatus|	String|	X|	送信ステータスコード <br/> WAIT:待機、 READY:送信準備、 <br/>SENDREADY:送信準備完了、 SENDWAIT:送信待機、 <br/>SENDING:送信中、 COMPLETE:送信完了、 <br/>FAIL:送信失敗、 CANCEL:送信キャンセル|
|pageNum|	Integer|	X|	ページ番号 1(デフォルト)|
|pageSize|	Integer|	X|	照会件数 15(デフォルト)|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

<a id="query-of-mails-1-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/mass-mails?startSendDate='"${START_DATE}"'&endSendDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

<a id="query-of-mails-1-2"></a>
#### レスポンス

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
            "sendStatusName":"送信完了",
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
|- data|	Object|	データ領域|
|-- requestId| String| リクエストID|
|-- requestDate| String| リクエスト日時|
|-- sendStatus|	String|	送信ステータスコード <br/> WAIT:待機、 READY:送信準備、 <br/>SENDREADY:送信準備完了、 SENDWAIT:送信待機、 <br/>SENDING:送信中、 COMPLETE:送信完了、 <br/>FAIL:送信失敗、 CANCEL:送信キャンセル|
|-- sendStatusName|	String|	送信ステータス名|
|-- templateId|	String|	テンプレートID|
|-- templateName|	String|	テンプレート名|
|-- senderName|	String|	送信者名|
|-- senderAddress|	String|	送信者のメールアドレス|
|-- title|	String|	メールの件名|
|-- body|	String|	メールの内容|
|-- adYn |  String  | 広告かどうか |
|-- createDate |  String  | 作成日時 |
|-- updateDate |  String  | 修正日時 |

<a id="query-of-mails-2"></a>
### 大量メール送信の受信者照会 { #query-of-mails-2 }

<a id="query-of-mails-2-1"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/mass-mails/{requestId}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|requestId|	String|	リクエストID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |


[Query parameter]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|receiveMailAddr|	String|	X|	受信メールアドレス|
|startReceiveDate|	String|	X|	受信日の開始値(yyyy-MM-dd HH:mm:ss)|
|endReceiveDate|	String|	X|	受信日の終了値(yyyy-MM-dd HH:mm:ss)|
|mailStatusCode|	String|	X|	送信ステータスコード <br/> SST0:送信準備、 SST1:送信中、  <br/> SST2:送信完了、 SST3:送信失敗、 SST7:未認証|
|pageNum|	Integer|	X|	ページ番号 1(デフォルト)|
|pageSize|	Integer|	X|	照会件数 15(デフォルト)|

<a id="query-of-mails-2-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/mass-mails/'"${REQUEST_ID}"'?startSendDate='"${START_DATE}"'&endSendDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

<a id="query-of-mails-2-2"></a>
#### レスポンス

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
            "mailStatusName":"送信完了",
            "resultId":"<20210806120100sA3CcWU2@tcmmsapp-92a901>",
            "receiveType":"MRT0",
            "receiveTypeName":"受信者",
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
|-- mailSeq | Integer  | メール通し番号 |
|-- mailStatusCode | String  | メールステータスコード <br/> SST0:送信準備、 SST1:送信中、  <br/> SST2:送信完了、 SST3:送信失敗、 SST7:未認証|
|-- mailStatusName | String  | メールステータス名 |
|-- resultId | String  | SMTP ID |
|-- receiveType|	String|	受信者タイプ<br/>MRT0 : 宛先 , MRT1 : CC, MRT2 : BCC|
|-- receiveTypeName|	String|	受信者タイプ名|
|-- receiveName|	String|	受信者名|
|-- receiveMailAddr|	String|	受信者のメールアドレス|
|-- isReceived| Boolean| 受信したかどうか |
|-- resultDate| String| 受信日時|
|-- isOpened| Boolean| 既読かどうか |
|-- openedDate| String| 既読日時|
|-- dsnCode| String| DSN(Delivery Status Notification)ステータスコード|
|-- dsnMessage| String| DSN(Delivery Status Notification)ステータスメッセージ |
|-- createDate |  String  | 作成日時 |
|-- updateDate |  String  | 修正日時 |

<a id="query-of-mails-3"></a>
### 大量メール送信の詳細照会 { #query-of-mails-3 }

<a id="query-of-mails-3-1"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/mass-mails/{requestId}/{mailSeq}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|requestId|	String|	リクエストID|
|mailSeq|	Integer|	メール通し番号|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

<a id="query-of-mails-3-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/mass-mails/'"${REQUEST_ID}"'/'"${MAIL_SEQ}"' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

<a id="query-of-mails-3-2"></a>
#### レスポンス

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
         "mailStatusName":"送信完了",
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
               "receiveTypeName":"受信者",
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

|値|	タイプ| 	説明                                                                        |
|---|---|-----------------------------------------------------------------------------|
|header|	Object| 	ヘッダ領域                                                                     |
|- isSuccessful|	Boolean| 	成否                                                                     |
|- resultCode|	Integer| 	失敗コード                                                                     |
|- resultMessage|	String| 	失敗メッセージ                                                                    |
|body|	Object| 	本文領域                                                                     |
|- data|	List| 	データ領域                                                                    |
|-- requestId  | String  | リクエストID                                                                       |
|-- templateId | String  | テンプレートID                                                                      |
|-- templateName | String  | テンプレート名                                                                      |
|-- mailStatusCode | String  | メールステータスコード <br/> SST0:送信準備、 SST1:送信中、  <br/> SST2:送信完了、 SST3:送信失敗、 SST7:未認証 |
|-- mailStatusName | String  | メールステータス名 |
|-- requestDate | String  | リクエスト時間                                                                      |
|-- senderName | String  | 送信者名 |
|-- senderAddress | String  | 送信者アドレス |
|-- resultId | String  | SMTP ID                                                                     |
|-- resultDate | String  | 実際の送信時間 |
|-- title | String  | 件名 |
|-- body | String  | 内容                                                                         |
|-- customHeaders|	Map| 	[ユーザー指定ヘッダ](./console-guide/#custom-header)                                |
|-- receiverList | List| 受信者リスト                                                                    |
|--- requestId | String  | リクエストID                                                                       |
|--- mailSeq | Integer  | メール通し番号                                                                       |
|--- receiveType | String  | 受信者タイプ (MRT0 : 宛先 , MRT1 : CC, MRT2 : BCC) |
|--- receiveTypeName | String  | 受信者タイプ名                                                                    |
|--- receiveMailAddr | String  | 受信者のメールアドレス |
|--- isReceived| Boolean| 受信したかどうか |
|--- resultDate| String| 受信日時                                                                      |
|--- isOpened| Boolean| 既読かどうか |
|--- openedDate| String| 既読日時 |
|--- dsnCode| String| DSN(Delivery Status Notification)ステータスコード                                    |
|--- dsnMessage| String| DSN(Delivery Status Notification)ステータスメッセージ |
|-- attachFileList | List  | 添付ファイルリスト                                                                   |
|--- fileType|	String| 	添付ファイルタイプ(MAIL:メールに添付されたファイル、 TEMPLATE:テンプレートに添付されたファイル)                          |
|--- fileId| Integer| ファイルID                                                                       |
|--- fileName|	String| 	添付ファイル名                                                                   |
|--- filePath|	String| 	添付ファイルパス                                                                   |
|--- fileSize|	Integer| 	添付ファイルサイズ(byte)                                                             |
|--- createDate|	String| 	作成日時                                                                     |
<a id="scheduled-delivery-management"></a>
## 予約送信管理 { #scheduled-delivery-management }

<a id="list-scheduled-delivery"></a>
### 予約送信リストの照会 { #list-scheduled-delivery }
<a id="list-scheduled-delivery-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v2.0/appKeys/{appKey}/sender/reservations|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |


[Query parameter]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
| requestId | String | X | リクエストID |
| startSendDate | String | X | 送信日の開始値(yyyy-MM-dd HH:mm:ss) |
| endSendDate | String | X | 送信日の終了値(yyyy-MM-dd HH:mm:ss) |
| senderMail | String | X | 送信メールアドレス |
| receiveMail | String | X | 受信メールアドレス |
| templateId | String | X | テンプレートID |
|pageNum|	Integer|	X|	ページ番号(Default：1)|
|pageSize|	Integer|	X|	照会件数(Default：15)|

<a id="list-scheduled-delivery-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="list-scheduled-delivery-response"></a>
#### レスポンス

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
            "receiveTypeName":"受信者",
            "requestDate":"2020-06-05 17:53:00",
            "mailStatusCode":"SST4",
            "mailStatusName":"予約待ち",
            "senderGroupingKey":"senderKey"
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
|-- requestId|	String | リクエストID |
|-- mailSeq|	Integer| メールの順番 |
|-- title|	String| メールのタイトル |
|-- templateId|	String|	テンプレートID|
|-- templateName|	String|	テンプレート名|
|-- senderAddress|	String|	送信者のメールアドレス|
|-- senderName|	String|	送信者名|
|-- receiveMailAddr|	String|	受信者のメールアドレス|
|-- receiveName|	String|	受信者名|
|-- receiveType|	String|	受信者タイプ(MRT0：受信者、MRT1：CC、MRT2：BCC)|
|-- receiveTypeName|	String|	受信者タイプ名|
|-- requestDate|	String|	送信日時|
|-- mailStatusCode|	String|	送信ステータスコード<br/> SST0：送信準備、SST1：送信中 <br/> SST2：送信完了、SST3：送信失敗<br/>SST4: 予約待ち、 SST7：認証なし|
|-- mailStatusName|	String|	送信ステータス名|
|-- senderGroupingKey|	String|	発信者グループキー |

<a id="query-detail-scheduled-delivery"></a>
### 予約送信の詳細照会 { #query-detail-scheduled-delivery }
<a id="query-detail-scheduled-delivery-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v2.0/appKeys/{appKey}/sender/reservations/{requestId}/{mailSeq}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|requestId|	String|	リクエストID|
|mailSeq|	Integer|	メールの順番|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

<a id="query-detail-scheduled-delivery-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations/'"${REQUEST_ID}"'/'"${MAIL_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="query-detail-scheduled-delivery-response"></a>
#### レスポンス

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
         "mailStatusName":"予約待ち",
         "receivers":[
            {
               "requestId":"202006050000008j6bx5Q1",
               "mailSeq":0,
               "receiveType":"MRT0",
               "receiveTypeName":"受信者",
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- requestId| String| リクエストID|
|-- mailSeq|	Integer| メールの順番 |
|-- title|	String| タイトル |
|-- body|	String| 内容 |
|-- templateId|	String|	テンプレートID|
|-- templateName|	String|	テンプレート名|
|-- senderAddress|	String|	送信者のアドレス|
|-- senderName|	String|	送信者名|
|-- requestDate|	String|	リクエスト時間|
|-- mailStatusCode|	String|	送信ステータスコード<br/> SST0：送信準備、SST1：送信中 <br/> SST2：送信完了、SST3：送信失敗<br/>SST4: 予約待ち、 SST7：認証なし|
|-- mailStatusName|	String|	送信ステータス名|
|-- receiverLis | List | 受信者リスト |
|--- requestId|	String | リクエストID |
|--- mailSeq|	Integer| メールの順番 |
|--- receiveType|	String|	受信者タイプ(MRT0：受信者、MRT1：CC、MRT2：BCC)|
|--- receiveName|	String|	受信者名|
|--- receiveTypeName|	String|	受信者タイプ名|
|--- receiveMailAddr|	String|	受信者メールアドレス|
|-- attachFileList|	List| 添付ファイルリスト|
|--- fileType|	String|	添付ファイルタイプ <br/>MAIL: メールに添付されたファイル、TEMPLATE：テンプレートに添付されたファイル|
|--- fileId| Integer| ファイルID|
|--- fileName|	String|	添付ファイル名|
|--- filePath|	String|	添付ファイルパス|
|--- fileSize|	Integer| 添付ファイルサイズ(byte)|
|--- createDate|	String|	作成日時|
|-- customHeaders|	Map|	[ユーザー指定ヘッダ](./console-guide/#custom-header) |
|-- senderGroupingKey|	String|	発信者グループキー |

<a id="cancel-scheduled-delivery-by-request"></a>
### 予約発送の取り消し - リクエスト別 { #cancel-scheduled-delivery-by-request }
<a id="cancel-scheduled-delivery-by-request-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|PUT| /email/v2.0/appKeys/{appKey}/sender/reservations/{requestId}

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|requestId|	String|	リクエストID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

<a id="cancel-scheduled-delivery-by-request-curl"></a>
#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations/'"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="cancel-scheduled-delivery-by-request-response"></a>
#### レスポンス

```json
{
    "header" : {
        "isSuccessful" : true,
        "resultCode" : 0,
        "resultMessage" : "SUCCESS"
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|

<a id="cancel-scheduled-delivery-by-recipient"></a>
### 予約発送の取り消し - 受信者別 { #cancel-scheduled-delivery-by-recipient }
<a id="cancel-scheduled-delivery-by-recipient-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|PUT| /email/v2.0/appKeys/{appKey}/sender/reservations/{requestId}/{mailSeq}

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|requestId|	String|	リクエストID|
|mailSeq|	Integer|	メールの順番|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

<a id="cancel-scheduled-delivery-by-recipient-curl"></a>
#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations/'"${REQUEST_ID}"'/'"${MAIL_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="cancel-scheduled-delivery-by-recipient-response"></a>
#### レスポンス

```json
{
    "header" : {
        "isSuccessful" : true,
        "resultCode" : 0,
        "resultMessage" : "SUCCESS"
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|

<a id="cancel-scheduled-delivery---multiple-filter"></a>
### 予約送信キャンセル - 多重フィルタ { #cancel-scheduled-delivery---multiple-filter }
<a id="cancel-scheduled-delivery---multiple-filter-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|PUT| /email/v2.0/appKeys/{appKey}/sender/reservations/search-cancels

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

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

|値|	タイプ|	必須|	説明|
|---|---|---|---|
| searchParameter.requestId | String | X | リクエストID |
| searchParameter.startSendDate | String | O | 送信日の開始値(yyyy-MM-dd HH:mm) |
| searchParameter.endSendDate | String | O | 送信日の終了値(yyyy-MM-dd HH:mm) |
| searchParameter.senderMail | String | X | 送信者のアドレス |
| searchParameter.receiveMail | String | X | 受信者メールアドレス |
| searchParameter.templateId | String | X | テンプレートID |
| updateUser | String | O | 予約キャンセルリクエスト者 |

* **startSendDate**, **endSendDate**, **updateUser** リクエストフィールドは必須です。

<a id="cancel-scheduled-delivery---multiple-filter-curl"></a>
#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations/search-cancels' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
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
#### レスポンス

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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- reservationCancelId|	String | 予約キャンセルID |
|-- requestedDateTime|	String| 予約キャンセルリクエスト時間(yyyy-MM-dd HH:mm:ss) |
|-- reservationCancelStatus|	String| 予約キャンセル状態<br/>- READY :予約準備<br/>- PROCESSING :予約キャンセル中<br/>- COMPLETED :予約キャンセル完了<br/>- FAILED :予約キャンセル失敗|


<a id="list-request-of-scheduled-delivery-cancellation---multiple-filter"></a>
### 予約送信キャンセルリクエストリスト照会 - 多重フィルタ { #list-request-of-scheduled-delivery-cancellation---multiple-filter }
<a id="list-request-of-scheduled-delivery-cancellation---multiple-filter-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v2.0/appKeys/{appKey}/sender/reservations/search-cancels

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

[Query parameter]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
| startRequestedDateTime | String | X | 予約キャンセルリクエスト開始時間(yyyy-MM-dd HH:mm:ss) |
| endRequestedDateTime | String | X | 予約キャンセルリクエスト終了時間(yyyy-MM-dd HH:mm:ss) |
| reservationCancelId | String | X | 予約キャンセルID |
|pageNum|	Integer|	X|	ページ番号(デフォルト値：1)|
|pageSize|	Integer|	X|	照会件数(デフォルト値：15)|

<a id="list-request-of-scheduled-delivery-cancellation---multiple-filter-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations/search-cancels' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="list-request-of-scheduled-delivery-cancellation---multiple-filter-response"></a>
#### レスポンス

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
|-- reservationCancelId|	String | 予約キャンセルID |
|-- searchParameter | Map<String, Object> | 予約キャンセルリクエストパラメータ |
|-- requestedDateTime|	String | 予約キャンセルリクエスト時間|
|-- completedDateTime|	String | 予約キャンセル完了時間 |
|-- reservationCancelStatus|	String| 予約キャンセル状態<br/>- READY :予約準備<br/>- PROCESSING :予約キャンセル中<br/>- COMPLETED :予約キャンセル完了<br/>- FAILED :予約キャンセル失敗|
|-- totalCount | Integer | 予約キャンセル対象件数 |
|-- successCount | Integer | 予約キャンセル成功件数 |
|-- createUser | Integer | 予約キャンセルリクエスト者 |
|-- createdDateTime | String | 予約キャンセルリクエスト作成時間 |
|-- updatedDateTime | String | 予約キャンセル修正時間 |


<p id="category"></p>

<a id="category-management"></a>
## カテゴリーの管理 { #category-management }

<a id="list"></a>
### カテゴリーリストの照会 { #list }

<a id="list-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v2.0/appKeys/{appKey}/categories|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

[Query parameter]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|useYn|	String|	X|	使用有無Y、N |
|categoryParentId|	Integer|	X|	親カテゴリーID |
|pageNum|	Integer|	X|	ページ番号(デフォルト値：1)|
|pageSize|	Integer|	X|	照会件数(デフォルト値：15)|

<a id="list-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/categories' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="list-response"></a>
#### レスポンス

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

<a id="query-details"></a>
### カテゴリー詳細照会 { #query-details }

<a id="query-details-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/categories/{categoryId}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|
|categoryId|	String|	カテゴリーID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

<a id="query-details-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/categories/'"${CATEGORY_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="query-details-response"></a>
#### レスポンス

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


<a id="register"></a>
### カテゴリーの登録 { #register }

<a id="register-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|POST|/email/v2.0/appKeys/{appKey}/categories|


[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

[Request body]

|値|	タイプ|	最大文字数 | 必須|	説明|
|---|---|---|---|---|
| categoryParentId |	Integer|	- | X |	親カテゴリーID <br/> 最上位カテゴリーID(デフォルト値) |
| categoryName |	String|	200 | O |	カテゴリー名 |
| categoryDesc |	String| 1000 |	X |	カテゴリーの説明|
| useYn |	String| 1 |	X|	使用有無Y(デフォルト値)、N|
| userId | String | 50 | X | ユーザーID |

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
#### レスポンス

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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- categoryId|	Integer|	カテゴリーID|


<a id="modify"></a>
### カテゴリーの修正 { #modify }

<a id="modify-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|PUT|/email/v2.0/appKeys/{appKey}/categories/{categoryId}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|
|categoryId|	Integer|	カテゴリーID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

[Request body]

|値|	タイプ|	最大文字数 | 必須|	説明|
|---|---|---|---|---|
| categoryName |	String|	200 | X |	カテゴリー名 |
| categoryDesc |	String| 1000 |	X |	カテゴリーの説明|
| useYn |	String| 1 |	X|	使用有無Y、N|
| userId | String | 50 | X | ユーザーID |

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
#### レスポンス

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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean| 成否 |
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|

<a id="delete"></a>
### カテゴリーの削除 { #delete }

<a id="delete-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|DELETE|/email/v2.0/appKeys/{appKey}/categories/{categoryId}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|
|categoryId|	Integer|	カテゴリーID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

<a id="delete-curl"></a>
#### cURL
```
curl -X DELETE \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/categories/'"${CATEGORY_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="delete-response"></a>
#### レスポンス

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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|

<p id="template"></p>

<a id="query-of-templates"></a>
## テンプレートの照会 { #query-of-templates }

<a id="query-list-of-templates"></a>
### テンプレートリスト照会 { #query-list-of-templates }

<a id="query-list-of-templates-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/templates|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

[Query parameter]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|categoryId|	Integer|	X|	カテゴリーID|
|useYn|	String|	X|	使用有無(Y/N)|
|pageNum|	Integer|	X|	ページ番号(Default：1)|
|pageSize|	Integer|	X|	照会件数(Default：15)|
|all|	Boolean|	X|	全てのテンプレートリストを照会するかどうか|

<a id="query-list-of-templates-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/templates' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="query-list-of-templates-response"></a>
#### レスポンス

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

<a id="query-template-details"></a>
### テンプレート詳細照会 { #query-template-details }

<a id="query-template-details-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/templates/{templateId}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|templateId|	String|	テンプレートID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

<a id="query-template-details-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="query-template-details-response"></a>
#### レスポンス

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

<a id="register-templates"></a>
### テンプレートの登録 { #register-templates }

<a id="register-templates-request"></a>
#### リクエスト

[URL]

|Http method|   URI|
|---|---|
|POST|  /email/v2.0/appKeys/{appKey}/templates|


[Path parameter]

|値| タイプ| 説明|
|---|---|---|
|appKey|    String| 固有のアプリケーションキー|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

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
    "templateName": "Name",
    "templateDesc": "Description",
    "useYn": "Y",
    "sendMailAddress": "test@example.com",
    "title": "Title",
    "templateType": "DEFAULT",
    "body": "Body",
    "attachFileIdList": [1, 2, 3],
    "userId": "USER"
}'
```

<a id="register-templates-response"></a>
#### レスポンス

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

|値| タイプ| 説明|
|---|---|---|
|header|    Object| ヘッダ領域|
|- isSuccessful|    Boolean| 成否 |
|- resultCode|  Integer|  失敗コード|
|- resultMessage|   String| 失敗メッセージ|

<a id="query-of-templates-upload-attached-files"></a>
### テンプレート添付ファイルのアップロード { #query-of-templates-upload-attached-files }

<a id="query-of-templates-upload-attached-files-request"></a>
#### リクエスト

[URL]

|Http method|   URI|
|---|---|
|POST|  /email/v2.0/appKeys/{appKey}/templates/attachfile/binaryUpload|

[Path parameter]

|値| タイプ| 説明|
|---|---|---|
|appKey|    String| 固有のアプリケーションキー|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

[Request body]


|値|	タイプ| 	最大長 | 必須|	説明|
|---|---|--------|---|---|
|fileName|	String| 	-     |O|	ファイル名|
|fileBody|	Byte[]| 	-     |O|	ファイルのByte[]値|
|userId|	String| 	50    |X|	ユーザーID|

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
#### レスポンス

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

<a id="modify-templates"></a>
### テンプレートの修正 { #modify-templates }

<a id="modify-templates-request"></a>
#### リクエスト

[URL]

|Http method|   URI|
|---|---|
|PUT|   /email/v2.0/appKeys/{appKey}/templates/{templateId}|

[Path parameter]

|値| タイプ| 説明|
|---|---|---|
|appKey|    String| 固有のアプリケーションキー|
|templateId|    String| テンプレートID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

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

<a id="modify-templates-curl"></a>
#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "templateName": "Template Name",
    "templateDesc": "Template Description",
    "useYn": "Y",
    "sendMailAddress": "test@example.com",
    "title": "Title",
    "templateType": "DEFAULT",
    "body": "Body",
    "attachFileIdList": [1, 2, 3],
    "userId": "USER"
}'
```

<a id="modify-templates-response"></a>
#### レスポンス

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

|値| タイプ| 説明|
|---|---|---|
|header|    Object| ヘッダ領域|
|- isSuccessful|    Boolean| 成否 |
|- resultCode|  Integer|  失敗コード|
|- resultMessage|   String| 失敗メッセージ|

<a id="delete-templates"></a>
### テンプレートの削除 { #delete-templates }

<a id="delete-templates-request"></a>
#### リクエスト

[URL]

|Http method|   URI|
|---|---|
|DELETE|    /email/v2.0/appKeys/{appKey}/templates/{templateId}|

[Path parameter]

|値| タイプ| 説明|
|---|---|---|
|appKey|    String| 固有のアプリケーションキー|
|templateId|    String| テンプレートID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

<a id="delete-templates-curl"></a>
#### cURL
```
curl -X DELETE \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="delete-templates-response"></a>
#### レスポンス

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

|値| タイプ| 説明|
|---|---|---|
|header|    Object| ヘッダ領域|
|- isSuccessful|    Boolean| 成否 |
|- resultCode|  Integer|  失敗コード|
|- resultMessage|   String| 失敗メッセージ|
<a id="query-statistics"></a>
## 統計照会 { #query-statistics }

<a id="query-daily-statistics"></a>
### 統合統計照会 { #query-daily-statistics }

<a id="query-daily-statistics-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/statistics/view |

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

[Query parameter]

|値|	タイプ|	必須| 説明|
|---|---|---|---|
|from|	String|	O | 統計照会の開始日<br/> yyyy-mm-dd HH:mm|
|to|	String|	O | 統計照会の終了日<br/> yyyy-mm-dd HH:mm|
|searchType| String | O | 統計区分<br/>DATE：日付別、TIME：時間別、DAY：曜日別 |
|mailTypes | String | X | メールタイプ<br/>NORMAL：一般、MASS：大量<br/>複数入力可能(mailTypes=NORMAL&mailTypes=MASS) |
|adYn | String | X | 広告かどうか<br>Y：広告、N：広告ではない<br>入力しなければ全て|
|templateId | String | X | テンプレートID |

<a id="query-daily-statistics-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/statistics/view?from='"${FROM}"'&to='"${TO}"'&searchType='"${SEARCH_TYPE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="query-daily-statistics-response"></a>
#### レスポンス

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

<a id="rejection-management"></a>
## 受信拒否管理 { #rejection-management }

<a id="query-rejections"></a>
### 受信拒否照会 { #query-rejections }

<a id="query-rejections-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
| GET |	/email/v2.0/appKeys/{appKey}/block-receivers |

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

[Query parameter]

|値|	タイプ|	必須| 説明|
|---|---|---|---|
|mailAddress|	String|	X| 受信拒否リストに登録されているメールアドレス|
|startBlockDate|	String|	X| 受信拒否日開始値 (yyyy-MM-dd HH:mm:ss)|
|endBlockDate|	String|	X| 受信拒否日終了値 (yyyy-MM-dd HH:mm:ss)|
|pageNum|	Integer|	X|	ページ番号(Default：1)|
|pageSize|	Integer|	X|	照会件数(Default：15)|
<a id="query-rejections-curl"></a>
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/block-receivers' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="query-rejections-response"></a>
#### レスポンス
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

<a id="register-rejections"></a>
### 受信拒否登録 { #register-rejections }

<a id="register-rejections-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
| POST |	/email/v2.0/appKeys/{appKey}/block-receivers |

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

[Request body]

|値|	タイプ|	必須 | 説明|
|---|---|---|---|
| blockReceiverList | ㅣList | O | 受信拒否リスト |
| - mailAddress | String | O | 受信拒否メールアドレス |
| - blockDate | String | X | 受信拒否日(yyyy-MM-dd HH:mm:ss) |

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
#### レスポンス
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|

<a id="delete-rejections"></a>
### 受信拒否削除 { #delete-rejections }
<a id="delete-rejections-request"></a>
#### リクエスト

[URL]

|Http method|	URI|
|---|---|
| PUT |	/email/v2.0/appKeys/{appKey}/block-receivers |

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

[Request body]

|値|	タイプ|	必須 | 説明|
|---|---|---|---|
| deleted | Boolean | O | 受信拒否削除を明示するフィールド |
| blockReceiverList | ㅣList | O | 受信拒否リスト |
| - mailAddress | String | O | 受信拒否メールアドレス |

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
#### レスポンス
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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
