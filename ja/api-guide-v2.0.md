## Notification > Email > API v2.0ガイド

### v2.0 API 소개

1. 시크릿 키 인증 도입
	* v2.0 API 호출 시 헤더에 [시크릿 키](./api-guide/#secret-key)를 설정해서 호출해야 합니다.
2. 대량 발송 조회 API 추가
	* 대량 발송건에 대한 조회 API가 추가되었습니다.


[APIドメイン]

|環境|	ドメイン|
|---|---|
|Real|	https://email.api.nhncloudservice.com |

[Header]
```
Content-Type: application/json;charset=UTF-8
```

<p id="secret-key"></p>

### Secret Key
- 콘솔에서 확인 가능합니다.
- Secret Key가 필요한 API를 호출할 때, 헤더에 아래와 같이 설정해서 호출해야 합니다.
```
Header
X-Secret-Key: [a-zA-Z0-9]{8}
```
**CONSOLE > Notification > Email > URL & AppKey** 에서 확인/생성할 수 있습니다.


[curl例注意事項]

* Windows cmdではcurl例が正常にリクエストされないことがあります。

## メールの送信

### 一般メールの送信

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

### 個別メール送信

* 受信者が複数人いる時、それぞれの受信者に個別にメールを送信する機能です。複数人に送っても、受信者には本人のみ表示されます。

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


### 広告性一般メール送信
* リクエスト、レスポンス情報は、一般メール送信と同じです。

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

### 広告性個別メール送信

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

### 認証メールの送信

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
|userId|	String|	X|	送信セパレータ ex)admin,system|
|statsId| String |X| 統計ID(発信検索条件には含まれません) |

[注意]

* テンプレートを使用する場合、**senderAddress、title、body**は必須値ではありません。この値を入力しない場合はテンプレートに登録された値を使用します。
* テンプレートを使用しながら、**senderAddress、senderName、title、body、templateType**を入力する場合は、テンプレートに登録された値より優先して適用されます。

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
        "receiveName": "고객1",
        "templateParameter": {
            "key": "value"
        }
    },
    "userId": "USER",
    "statsId": "statsId"
}'
```

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

### タグメールの送信

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/sender/tagMail|

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
|customHeaders| Map| X| [ユーザー指定ヘッダ](./console-guide/#custom-header)|
| userId  | String | X|送信セパレータ ex)admin,system|
| statsId | String |X| 統計ID(発信検索条件には含まれません) |

[注意]

* templateを使用する場合、title、bodyは必須入力から除外(入力時に入力された値がtemplateより優先適用)


[例1]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/tagMail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "senderAddress": "support@example.com",
    "senderName": "발송자이름",
    "title": "샘플 타이틀",
    "body": "샘플 내용",
    "attachFileIdList": [1, 2],
    "tagExpression": ["tag1", "AND", "tag2"],
    "userId": "USER",
    "statsId": "statsId"
}'
```

[例2 - テンプレートの使用]
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/tagMail' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "templateId": "TEMPLATE1",
    "tagExpression": ["tag1", "AND", "tag2"],
    "userId": "USER",
    "statsId": "statsId"
}'
```

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
            "requestId": "20190101000000ABCDEFG0"
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

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/mails?startSendDate='"${START_DATE}"'&endSendDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
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


### メール送信詳細の照会

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

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/mail/'"${REQUEST_ID}"'/'"${MAIL_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
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


### 대량 메일 리스트 조회

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

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/mass-mails?startSendDate='"${START_DATE}"'&endSendDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

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

### 대량 메일 발송 수신자 조회

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

|값|	타입|	필수|	설명|
|---|---|---|---|
|receiveMailAddr|	String|	X|	수신 메일 주소|
|startReceiveDate|	String|	X|	수신 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endReceiveDate|	String|	X|	수신 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|mailStatusCode|	String|	X|	발송상태 코드 <br/> SST0:발송준비, SST1:발송중,  <br/> SST2:발송완료, SST3: 발송실패, SST7: 미인증|
|pageNum|	Integer|	X|	페이지 번호 1(기본값)|
|pageSize|	Integer|	X|	조회 건수 15(기본값)|

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/mass-mails/'"${REQUEST_ID}"'?startSendDate='"${START_DATE}"'&endSendDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

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

### 대량 메일 발송 상세 조회

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

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/mass-mails/'"${REQUEST_ID}"'/'"${MAIL_SEQ}"' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

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

|값|	타입| 	설명                                                                         |
|---|---|-----------------------------------------------------------------------------|
|header|	Object| 	헤더 영역                                                                      |
|- isSuccessful|	Boolean| 	성공 여부                                                                      |
|- resultCode|	Integer| 	실패 코드                                                                      |
|- resultMessage|	String| 	실패 메시지                                                                     |
|body|	Object| 	본문 영역                                                                      |
|- data|	List| 	데이터 영역                                                                     |
|-- requestId  | String  | 요청 ID                                                                       |
|-- templateId | String  | 템플릿 ID                                                                      |
|-- templateName | String  | 템플릿 명                                                                       |
|-- mailStatusCode | String  | 메일 상태 코드 <br/> SST0:발송준비, SST1:발송중,  <br/> SST2:발송완료, SST3: 발송실패, SST7: 미인증 |
|-- mailStatusName | String  | 메일 상태 명                                                                     |
|-- requestDate | String  | 요청 시간                                                                       |
|-- senderName | String  | 발신자 명                                                                       |
|-- senderAddress | String  | 발신자 주소                                                                      |
|-- resultId | String  | SMTP ID                                                                     |
|-- resultDate | String  | 실제 발송 시간                                                                    |
|-- title | String  | 제목                                                                          |
|-- body | String  | 내용                                                                          |
|-- customHeaders|	Map| 	[사용자 지정 헤더](./console-guide/#custom-header)                                |
|-- receiverList | List| 수신자 리스트                                                                     |
|--- requestId | String  | 요청 ID                                                                       |
|--- mailSeq | Integer  | 메일 순번                                                                       |
|--- receiveType | String  | 수신자 타입 (MRT0 : 받는사람 , MRT1 : 참조, MRT2 : 숨은참조)                               |
|--- receiveTypeName | String  | 수신자 타입명                                                                     |
|--- receiveMailAddr | String  | 수신자 메일 주소                                                                   |
|--- isReceived| Boolean| 수신 여부                                                                       |
|--- resultDate| String| 수신 일시                                                                       |
|--- isOpened| Boolean| 읽음 여부                                                                       |
|--- openedDate| String| 읽은 일시                                                                       |
|--- dsnCode| String| DSN(Delivery Status Notification) 상태 코드                                     |
|--- dsnMessage| String| DSN(Delivery Status Notification) 상태 메시지                                    |
|-- attachFileList | List  | 첨부파일 리스트                                                                    |
|--- fileType|	String| 	첨부파일 타입 (MAIL: 메일에 첨부된 파일, TEMPLATE: 템플릿에 첨부된 파일)                          |
|--- fileId| Integer| 파일 ID                                                                       |
|--- fileName|	String| 	첨부파일 이름                                                                    |
|--- filePath|	String| 	첨부파일 경로                                                                    |
|--- fileSize|	Integer| 	첨부파일 크기 (byte)                                                             |
|--- createDate|	String| 	생성 일시                                                                      |


### タグメール送信リクエストの照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/tagMails|

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

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/tagMails?startSendDate='"${START_DATE}"'&endSendDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
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
|-pageSize|	Integer|	照会されたデータ件数|
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
|GET|	/email/v2.0/appKeys/{appKey}/tagMails/{requestId}|

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
|receiveMail|	String|	X|	受信メールアドレス|
|startReceiveDate|	String|	X|	受信日の開始値(yyyy-MM-dd HH:mm:ss)|
|endReceiveDate|	String|	X|	受信日の終了値(yyyy-MM-dd HH:mm:ss)|
|receiveStatus|	String|	X|	送信ステータスコード<br/> SST0：送信準備、SST1：送信中<br/> SST2：送信完了、SST3：送信失敗、 SST7：認証なし|
|pageNum|	Integer|	X|	ページ番号(Default：1)|
|pageSize|	Integer|	X|	照会件数(Default：15)|

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/tagMails/'"${REQUEST_ID}"'?startSendDate='"${START_DATE}"'&endSendDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
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
                "dsnCode": String,
                "dsnMessage": String,
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
|-- mailStatusCode | String  | メールステータスコード<br/> SST0：送信準備、SST1：送信中、<br/> SST2：送信完了、SST3：送信失敗、 SST7：認証なし|
|-- mailStatusName | String  | メールのステータス名 |
|-- resultId | String  | SMTP ID |
|-- resultDate | String  | 実際の送信時間 |
|-- readYn | String  | 既読かどうか |
|-- readDate | String  | 読んだ時間 |
|-- dsnCode| String| DSN(Delivery Status Notification) ステータスコード|
|-- dsnMessage| String| DSN(Delivery Status Notification) ステータスメッセージ |
|-- createUser |  String  | 作成者 |
|-- createDate |  String  | 作成日時 |
|-- updateUser |  String  | 修正者 |
|-- updateDate |  String  | 修正日時 |

### タグメール送信詳細照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/tagMails/{requestId}/{mailSequence}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|requestId|	String|	リクエストID|
|mailSequence|	Integer|	メールの順番|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/tagMails/'"${REQUEST_ID}"'/'"${MAIL_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
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
                    "requestId": String,
                    "receiveType": String,
                    "receiveTypeName": String,
                    "receiveMailAddr": String,
                    "readYn": String,
                    "readDate": String,
                    "dsnCode": String,
                    "dsnMessage": String,
                    "mailSequence": String
                }
            ],
            "attachFileList": [
                {
                    "fileType": String,
                    "fileId": Integer,
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
|-- mailStatusCode | String  | メールステータスコード<br/> SST0：送信準備、SST1：送信中、<br/> SST2：送信完了、SST3：送信失敗、 SST7：認証なし |
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
|--- receiveType | String  | 受信者タイプ(MRT0：受信者、MRT1：CC、MRT2：BCC) |
|--- receiveTypeName | String  | 受信者タイプ名 |
|--- receiveMailAddr | String  | 受信者メールアドレス |
|--- readYn | String  | 既読かどうか |
|--- readDate | String  | 読んだ時間 |
|--- dsnCode| String| DSN(Delivery Status Notification) ステータスコード|
|--- dsnMessage| String| DSN(Delivery Status Notification) ステータスメッセージ |
|--- mailSequence | String  | メールの順番 |
|-- attachFileList | List  | 添付ファイルリスト |
|--- fileType|	String|	添付ファイルタイプ(MAIL：メールに添付されたファイル、TEMPLATE：テンプレートに添付されたファイル)|
|--- fileId| Integer| ファイルID|
|--- fileName|	String|	添付ファイル名|
|--- filePath|	String|	添付ファイルパス|
|--- fileSize|	Integer|	添付ファイルサイズ(byte)|
|--- createDate|	String|	作成日時|
|-- customHeaders|	Map|	[ユーザー指定ヘッダ](./console-guide/#custom-header) |


## 予約送信管理

### 予約送信リストの照会
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

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

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

### 予約送信の詳細照会
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

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations/'"${REQUEST_ID}"'/'"${MAIL_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

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

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- requestId| String| リクエストID|
|-- mailSeq|	Integer| メールの順番 순번 |
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

### 予約発送の取り消し - リクエスト別
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

#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations/'"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

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

### 予約発送の取り消し - 受信者別
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

#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations/'"${REQUEST_ID}"'/'"${MAIL_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

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

### 予約送信キャンセル - 多重フィルタ
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


### 予約送信キャンセルリクエストリスト照会 - 多重フィルタ
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

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations/search-cancels' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

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
|-- createUser | Integer | 예약 취소 요청자 |
|-- createdDateTime | String | 予約キャンセルリクエスト作成時間 |
|-- updatedDateTime | String | 予約キャンセル修正時間 |


<p id="category"></p>

## カテゴリーの管理

### カテゴリーリストの照会

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

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/categories' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

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

### カテゴリー詳細照会

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

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/categories/'"${CATEGORY_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

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


### カテゴリーの登録

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


### カテゴリーの修正

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

### カテゴリーの削除

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

#### cURL
```
curl -X DELETE \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/categories/'"${CATEGORY_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

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

## テンプレートの照会

### テンプレートリスト照会

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

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/templates' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

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

### テンプレート詳細照会

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

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

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

### テンプレートの登録

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

### テンプレート添付ファイルのアップロード

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

|値| タイプ| 最大文字数| 必須| 説明|
|---|---|---|---|---|
|fileName|  String| 100|O| ファイル名 |
|fileBody|  Byte[]| - |O| ファイルのバイト[]値 |
|userId|    String| 50|X| ユーザーID |

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

### テンプレートの修正

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

### テンプレートの削除

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

#### cURL
```
curl -X DELETE \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

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

## タグ管理

### タグ照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/tags|

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
|pageNum|	Integer|	X|	ページ番号(Default：1)|
|pageSize|	Integer|	X|	照会件数(Default：15)|

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/tags' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

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
|POST|	/email/v2.0/appKeys/{appKey}/tags|

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
|tagName|	String|	O|	タグ名|

#### cURL
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/tags' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "tagName": "API-Guide-Sample"
}'
```

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
            "tagId": "ABCD1234"
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
|PUT|	/email/v2.0/appKeys/{appKey}/tags/{tagId}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|tagId|	String|	タグID|

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
|tagName|	String|	O|	タグ名|

#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/tags/'"${TAG_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "tagName": "API-Guide-Sample"
}'
```

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

### タグの削除

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|DELETE|	/email/v2.0/appKeys/{appKey}/tags/{tagId}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|tagId|	String|	タグID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

#### cURL
```
curl -X DELETE \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/tags/'"${TAG_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

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

## UIDの管理

### UIDの照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/uids|

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
|wheres|	List:String|	X|	照会条件。</br>英数字、括弧で構成された文字列。</br>括弧は1個、AND、ORは3個まで使用できる。</br> (例) tagId1,AND,tagId2|
|offsetUid|	String|	X|	offset UID|
|offset|	Integer|	X| offset(Default : 0)|
|limit|	Integer|	X|	照会件数(Default：15)|

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/uids' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

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
                            "contact": "test@nhn.com",
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
|GET|	/email/v2.0/appKeys/{appKey}/uids/{uid}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|uid|	String|	UID |

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

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
                    "contact": "test@nhn.com",
                    "createdDate": "2019-01-01 00:00:00"
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
|POST|	/email/v2.0/appKeys/{appKey}/uids|

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
|uid|	String|	O|	UID|
|tagIds|	List:String|	O|	タグIDリスト|
|contacts|	List|	O|	メールアドレスリスト |
|-contactType| String| O| 連絡先タイプ |
|-contact| String| O| 連絡先(メールアドレス) |

[注意]

* tagIdsが与えられている場合、contactsは必須値ではない。
* contactsが与えられている場合、tagIdsは必須値ではない。
* 本サービスの場合、contactTypeは必ず"EMAIL_ADDRESS"値でリクエストする必要がある。

#### cURL
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/uids' \
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

### UIDの削除

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|DELETE|	/email/v2.0/appKeys/{appKey}/uids/{uid}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|uid|	String|	UID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

#### cURL
```
curl -X DELETE \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

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

### メールアドレスの登録

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/uids/{uid}/email-addresses|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|uid|	String|	UID|

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
|emailAddress|	String|	O|	メールアドレス|

#### cURL
```
curl -X POST \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}"'/email-addresses' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' \
-d '{
    "emailAddress": "customer1@example.com"
}'
```

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

### メールアドレスの削除

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|DELETE|	/email/v2.0/appKeys/{appKey}/uids/{uid}/email-addresses/{emailAddress}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|uid|	String|	UID|
|emailAddress|	String|	メールアドレス|

[Header]

```
{
  "X-Secret-Key": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|X-Secret-Key|	String| O | 固有のsecretKey [[参考](./api-guide/#secret-key)] |

#### cURL
```
curl -X DELETE \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}"'/email-addresses/'"${EMAIL_ADDR}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

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

## 統計照会

### 統合統計照会

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

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/statistics/view?from='"${FROM}"'&to='"${TO}"'&searchType='"${SEARCH_TYPE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

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

## 受信拒否管理

### 受信拒否照会

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
#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/block-receivers' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

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

### 受信拒否登録

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

### 受信拒否削除
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