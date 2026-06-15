<!-- pre-align:aligned sig=56a08ae36765 -->

## Notification > Email > API v2.0 가이드

<a id="v20-api"></a>

### v2.0 API 소개

1. 시크릿 키 인증 도입
	* v2.0 API 호출 시 헤더에 [시크릿 키](./api-guide/#secret-key)를 설정해서 호출해야 합니다.
2. 대량 발송 조회 API 추가
	* 대량 발송건에 대한 조회 API가 추가되었습니다.

[API 도메인]

|환경|	도메인|
|---|---|
|Real|	https://email.api.nhncloudservice.com |

[Header]
```
Content-Type: application/json;charset=UTF-8
```

<p id="secret-key"></p>

<a id="secret-key"></a>

### Secret Key
- 콘솔에서 확인 가능합니다.
- Secret Key가 필요한 API를 호출할 때, 헤더에 아래와 같이 설정해서 호출해야 합니다.
```
Header
X-Secret-Key: [a-zA-Z0-9]{8}
```
**CONSOLE > Notification > Email > URL & AppKey** 에서 확인/생성할 수 있습니다.

[curl 예시 주의 사항]

* Windows cmd 에서는 curl 예시가 정상적으로 요청되지 않을 수 있습니다.

<a id="mail-delivery"></a>

## 메일 발송

<a id="send-general-mails"></a>

### 일반 메일 발송

<a id="request"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/sender/mail|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|


[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |


[Request body]

|값|	타입|	필수|	설명|
|---|---|---|---|
|senderAddress|	String|	O|	발신자 메일 주소 ( 최대 100자 )|
|senderName|	String|	X|	발신자 이름 ( 최대 100자 )|
|requestDate|	String|	X|	발송 일시 (yyyy-MM-dd HH:mm:ss) <br/>현재 일시(기본값) |
|title|	String|	O|	메일 제목 (최대 998자)|
|body|	String|	O|	메일 내용|
|attachFileIdList|	List:Integer|	X|	업로드한 첨부파일 id (파일 업로드 시 file id를 반환)|
|templateId|	String|	X|	발송 템플릿 ID ( 최대 50자 )|
|templateType| String| X| 템플릿 타입 <br/>DEFAULT(기본값), FREEMARKER)|
|templateParameter|	Object|	X|	치환 파라미터 (메일 제목/내용 치환시 입력)|
|- key|	String|	X|	치환 키 (##key##)|
|- value|	Object|	X|	치환 키에 매핑되는 Value값|
|receiverList|	List|	O|	수신자 리스트<br/> 최대 1000명까지 발송 가능(받는 사람, 참조자 포함)|
|- receiveMailAddr|	String|	O|	수신자 메일 주소|
|- receiveName|	String|	X|	수신자명|
|- receiveType|	String|	O|	수신자 타입 (MRT0 : 받는 사람 , MRT1 : 참조, MRT2 : 숨은 참조)|
|customHeaders| Map| X| [사용자 지정 헤더](./console-guide/#custom-header)|
|senderGroupingKey| String| X| 발신자 그룹 키(최대 100자)|
|userId|	String|	X|	발송 구분자 ex)admin,system ( 최대 50자 )|
|statsId| String  | X | 통계 ID(발신 검색 조건에는 포함되지 않습니다.) |


[주의]

* 템플릿을 사용하는 경우 **senderAddress, title, body**는 필수 값이 아닙니다. 이 값을 입력하지 않는 경우 템플릿에 등록된 값을 사용합니다.
* 템플릿을 사용하면서 **senderAddress, senderName, title, body, templateType**을 입력한다면 템플릿에 등록된 값보다 우선 적용됩니다.

[예시 1]
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

[예시 2 - 템플릿 사용]
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

<a id="response"></a>

#### 응답

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
                    "receiveMailAddr": "receiver@example.com",
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	Object|	데이터 영역|
|-- requestId|	String|	요청 ID|
|-- results|	List|	발송 결과|
|--- receiveMailAddr|	String|	수신자 메일 주소|
|--- receiveName|	String|	수신자명|
|--- receiveType|	String|	수신자 타입 (MRT0 : 받는 사람 , MRT1 : 참조, MRT2 : 숨은 참조)|
|--- resultCode|	Integer|	수신자 발송 요청 결과 코드|
|--- resultMessage|	String|	수신자 발송 요청 결과 메시지|

<a id="send-individual-mails"></a>

### 개별 메일 발송

* 수신자가 여러 명일 때 수신자 각각에게 메일을 발송하는 기능입니다. 여러 명에게 보내도 수신자에게는 본인만 표시됩니다.

<a id="request-2"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/sender/eachMail|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

[Request body]

|값|	타입|	필수|	설명|
|---|---|---|---|
|senderAddress|	String|	O|	발신자 메일 주소 ( 최대 100자 )|
|senderName|	String|	X|	발신자 이름 ( 최대 100자 )|
|requestDate|	String|	X|	발송 일시 (yyyy-MM-dd HH:mm:ss) <br/>현재 일시(기본값) |
|title|	String|	O|	메일 제목 (최대 998자)|
|body|	String|	O|	메일 내용|
|attachFileIdList|	List:Integer|	X|	업로드한 첨부파일 id (파일 업로드 시 file id를 반환)|
|templateId|	String|	X|	발송 템플릿 ID ( 최대 50자 )|
|templateType| String| X| 템플릿 타입 <br/>DEFAULT(기본값), FREEMARKER)|
|receiverList|	List|	O|	수신자 리스트<br/> 최대 1000명까지 발송 가능|
|- receiveMailAddr|	String|	O|	수신자 메일 주소|
|- receiveName|	String|	X|	수신자명|
|- templateParameter|	Object|	X|	치환 파라미터 (메일 제목/내용 치환시 입력)|
|-- #key#|	String|	X|	치환 키 (##key##)|
|-- #value#|	Object|	X|	치환 키에 매핑되는 Value값|
|customHeaders| Map| X| [사용자 지정 헤더](./console-guide/#custom-header)|
|senderGroupingKey| String| X| 발신자 그룹 키(최대 100자) |
|userId|	String|	X|	발송 구분자 ex)admin,system ( 최대 50자 )|
|statsId| String  | X | 통계 ID(발신 검색 조건에는 포함되지 않습니다.) |

[주의]

* 템플릿을 사용하는 경우 **senderAddress, title, body**는 필수 값이 아닙니다. 이 값을 입력하지 않는 경우 템플릿에 등록된 값을 사용합니다.
* 템플릿을 사용하면서 **senderAddress, senderName, title, body, templateType**을 입력한다면 템플릿에 등록된 값보다 우선 적용됩니다.

[예시 1]
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

[예시 2 - 템플릿 사용]
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

<a id="response-2"></a>

#### 응답

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
                    "receiveMailAddr": "receiver@example.com",
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	Object|	데이터 영역|
|-- requestId|	String|	요청 ID|
|-- results|	List|	발송 결과|
|--- receiveMailAddr|	String|	수신자 메일 주소|
|--- receiveName|	String|	수신자명|
|--- receiveType|	String|	수신자 타입 (MRT0 : 받는 사람 , MRT1 : 참조, MRT2 : 숨은 참조)|
|--- resultCode|	Integer|	수신자 발송 요청 결과 코드|
|--- resultMessage|	String|	수신자 발송 요청 결과 메시지|

<a id="sending-general-ad-mails"></a>

### 광고성 일반 메일 발송
* 요청, 응답 정보는 일반 메일 발송과 동일합니다.

<a id="caution-for-sending-ad-mails"></a>

#### 광고 메일 전송 시 유의 사항
* 제목에 반드시 (광고) 문구를 삽입하도록 강제하고 있습니다.
* 자세한 내용은 [[광고성 메일 발송](./console-guide/#_3)]를 참고해주세요.

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/sender/ad-mail|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

[예시 1]
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

[예시 2 - 템플릿 사용]
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

### 광고성 개별 메일 발송

* 요청, 응답 정보는 개별 메일 발송과 동일합니다.

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/sender/ad-eachMail |

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

[예시 1]
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

[예시 2 - 템플릿 사용]
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

### 인증 메일 발송

<a id="request-3"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/sender/auth-mail|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

[Request body]

|값|	타입|	필수|	설명|
|---|---|---|---|
|senderAddress|	String|	O|	발신자 메일 ( 최대 100자 )|
|senderName|	String|	X|	발신자 이름 ( 최대 100자 )|
|requestDate|   String| X|  발송 일시 (yyyy-MM-dd HH:mm:ss) <br/>현재 일시(기본값) |
|title|	String|	O|	제목 ( 최대 998자 )|
|body|	String|	O|	내용|
|templateId|	String|	X|	발송 템플릿 ID|
|receiver|	Object|	O|	수신자 |
|- receiveMailAddr|	String|	O|	수신자 메일 주소|
|- receiveName|	String|	X|	수신자명|
|- templateParameter|	Object|	X|	치환 파라미터 (메일 제목/내용 치환시 입력)|
|-- #key#|	String|	X|	치환 키 (##key##)|
|-- #value#|	Object|	X|	치환 키에 매핑되는 Value값|
|customHeaders| Map| X| [사용자 지정 헤더](./console-guide/#custom-header) ( 최대 100자 )|
|senderGroupingKey| String| X| 발신자 그룹 키(최대 100자 )|
|userId|	String|	X|	발송 구분자 ex)admin,system ( 최대 50자 )|
|statsId| String  | X | 통계 ID(발신 검색 조건에는 포함되지 않습니다.) |


[주의]

* 템플릿을 사용하는 경우 **senderAddress, title, body**는 필수 값이 아닙니다. 이 값을 입력하지 않는 경우 템플릿에 등록된 값을 사용합니다.
* 템플릿을 사용하면서 **senderAddress, senderName, title, body, templateType**을 입력한다면 템플릿에 등록된 값보다 우선 적용됩니다.

<a id="differences-from-general-mails"></a>

#### 일반 메일과 다른 점
인증 메일 성격상 다음과 같이 다른 특성들이 있습니다.

* 단건 발송(1명의 수신자)만 가능합니다.
* 첨부 파일 기능을 지원하지 않습니다. 첨부 파일이 포함된 템플릿은 지원하지 않습니다.

[예시 1]
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

[예시 2 - 템플릿 사용]
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

<a id="response-3"></a>

#### 응답

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
                    "receiveMailAddr": "receiver@example.com",
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	Object|	데이터 영역|
|-- requestId|	String|	요청 ID|
|-- results|	List|	발송 결과|
|--- receiveMailAddr|	String|	수신자 메일 주소|
|--- receiveName|	String|	수신자명|
|--- receiveType|	String|	수신자 타입 (MRT0 : 받는 사람 , MRT1 : 참조, MRT2 : 숨은 참조)|
|--- resultCode|	Integer|	수신자 발송 요청 결과 코드|
|--- resultMessage|	String|	수신자 발송 요청 결과 메시지|
<a id="upload-attached-files"></a>

### 첨부파일 업로드

<a id="request-4"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/attachfile/binaryUpload|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

[Request body]

|값|	타입|	필수|	설명|
|---|---|---|---|
|fileName|	String|	O|	파일 이름|
|fileBody|	Byte[]|	O|	파일의 Byte[] 값|
|createUser|	String|	X|	파일 업로드 유저 정보|

<a id="curl"></a>

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


<a id="response-4"></a>

#### 응답

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	Object|	데이터 영역|
|-- fileId|	Integer|	파일 ID|
|-- fileName|	String|	파일명|


<a id="titlebody-replacement"></a>

### 제목/본문 치환

<a id="default-type"></a>

#### 기본 타입
* (##치환 Key##) 형식으로 입력하면 사용자가 입력한 **templateParameter**로 치환할 수 있습니다.
```
* title : ##title_name##님 안녕하세요 !!
* body : ##body_content## 발송합니다.
->
* title : 클라우드고객1님 안녕하세요!!
* body : test2 발송합니다.
```

<a id="freemarker-type"></a>

#### FreeMarker 타입
* [FreeMarker 템플릿 엔진](https://freemarker.apache.org/)을 지원합니다.
* 템플릿 언어를 사용하여 사용자가 입력한 **templateParameter**로 치환할 수 있습니다.
```
* title : ${title_name}님 안녕하세요 !!
* body : ${body_content} 발송합니다.
->
* title : 클라우드고객1님 안녕하세요!!
* body : test2 발송합니다.
```

<a id="example-of-general-mail-request"></a>

#### 일반 메일요청 예시
```json
{
    "senderAddress" : "support@example.com",
    "templateId": "template1",
    "templateParameter" : {"title_name": "클라우드고객1", "body_content": "test1"},
    "receiverList" : [
        {
            "receiveMailAddr" : "customer1@example.com",
            "receiveType" : "MRT0"
        }

    ],
    "userId" : "tester"
}
```

<a id="example-of-individual-mail-request"></a>

#### 개별 메일요청 예시
```json
{
    "senderAddress" : "support@example.com",
    "templateId": "template1",
    "receiverList" : [
        {
            "receiveMailAddr" : "customer1@example.com",
            "templateParameter" : {"title_name": "클라우드고객1", "body_content": "test1"}
        },
        {
            "receiveMailAddr" : "customer2@example.com",
            "templateParameter" : {"title_name": "클라우드고객2", "body_content": "test2"}
        }

    ],
    "userId" : "tester"
}
```

<a id="query-of-mails"></a>

## 메일 조회

<a id="query-list-of-mail-deliveries"></a>

### 메일 발송 리스트 조회

<a id="request-5"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/sender/mails|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

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
|requestId|	String|	O|	요청 ID|
|startSendDate|	String|	O|	발송 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endSendDate|	String|	O|	발송 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|startReceiveDate|	String|	X|	수신 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endReceiveDate|	String|	X|	수신 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|senderAddress|	String|	X|	발신 메일 주소|
|senderName|	String|	X|	발신자 이름|
|receiveMailAddr|	String|	X|	수신 메일 주소|
|templateId|	String|	X|	템플릿 ID|
|mailStatusCode|	String|	X|	발송 상태 코드 <br/> SST0:발송 준비, SST1:발송 중,  <br/> SST2:발송 완료, SST3: 발송 실패, SST7: 미인증|
|isReceived| Boolean| X | 수신 여부 |
|isOpened| Boolean| X | 읽음 여부 |
|senderGroupingKey| String| X| 발신자 그룹 키|
|pageNum|	Integer|	X|	페이지 번호 1(기본값)|
|pageSize|	Integer|	X|	조회 건수 15(기본값)|

* **requestId** 또는 **startSendDate**, **endSendDate** 요청 필드가 필수입니다.

<a id="curl-2"></a>

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/mails?startSendDate='"${START_DATE}"'&endSendDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

<a id="response-5"></a>

#### 응답

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
                "senderAddress": "sender@example.com",
                "title": "Title",
                "mailStatusCode": "SST2",
                "mailStatusName": "발송 완료",
                "isReceived": true,
                "resultDate": "2019-01-01 00:00:00",
                "isOpened": true,
                "openedDate": "2019-01-01 00:00:00",
                "receiveMailAddr": "receiver@example.com",
                "receiveType": "MRT0",
                "receiveTypeName": "받는 사람",
                "receiveName": "Receiver",
                "senderGroupingKey": "A_GROUP",
                "dsnCode": "2.5.0",
                "dsnMessage": "SUCCESS"
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
|- data|	List|	데이터 영역|
|-- requestId|	String|	요청 ID|
|-- mailSeq|	Integer|	한 요청에 여러 메일을 보낸 경우, 각 메일에 부여되는 순번|
|-- requestDate|	String|	발신일시|
|-- templateId|	String|	템플릿ID|
|-- templateName|	String|	템플릿명|
|-- senderName|	String|	발신자 이름|
|-- senderAddress|	String|	발신자 메일 주소|
|-- title|	String|	메일 제목|
|-- mailStatusCode|	String|	발송 상태 코드 <br/> SST0:발송 준비, SST1:발송 중,  <br/> SST2:발송 완료, SST3: 발송 실패, SST7: 미인증|
|-- mailStatusName|	String|	발송 상태명|
|-- isReceived|	Boolean|	수신 여부|
|-- resultDate|	String|	수신 일시|
|-- isOpened| Boolean| 읽음 여부 |
|-- openedDate| String| 읽은 일시|
|-- receiveMailAddr|	String|	수신자 메일 주소|
|-- receiveType|	String|	수신자 타입 <br/>MRT0 : 받는 사람 , MRT1 : 참조, MRT2 : 숨은 참조|
|-- receiveTypeName| String| 수신자 타입 이름|
|-- receiveName| String| 수신자 이름|
|-- senderGroupingKey| String| 발신자 그룹 키|
|-- dsnCode| String| DSN(Delivery Status Notification) 상태 코드|
|-- dsnMessage| String| DSN(Delivery Status Notification) 상태 메시지 |


<a id="query-mail-delivery-details"></a>

### 메일 발송 상세 조회

<a id="request-6"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/sender/mail/{requestId}/{mailSeq}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|requestId|	String|	요청ID|
|mailSeq|	Integer| 메일 순번 (메일 리스트 조회 시 반환되는 mailSeq 값)|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

<a id="curl-3"></a>

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/mail/'"${REQUEST_ID}"'/'"${MAIL_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="response-6"></a>

#### 응답

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
            "mailStatusName": "발송 완료",
            "templateId": "Template",
            "templateName": "Template Name",
            "senderName": "Sender",
            "senderAddress": "sender@example.com",
            "resultId": "<20190101000000ABCDEFG0@test>",
            "title": "Title",
            "body": "Body",
            "receiverList": [
                {
                    "requestId": "20190101000000ABCDEFG0",
                    "mailSeq": 0,
                    "receiveType": "MRT0",
                    "receiveTypeName": "받는 사람",
                    "receiveName": "Receiver",
                    "receiveMailAddr": "receiver@example.com",
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	Object|	데이터 영역|
|-- requestId| String| 요청 ID|
|-- mailSeq| Integer| 메일 순번|
|-- requestIp| String| 요청 IP|
|-- requestDate| String| 요청 일시|
|-- mailStatusCode|	String| 발송 상태 코드 <br/> SST0:발송 준비, SST1:발송 중,  <br/> SST2:발송 완료, SST3: 발송 실패, SST7: 미인증|
|-- mailStatusName|	String|	발송 상태명|
|-- templateId|	String|	템플릿 ID|
|-- templateName|	String|	템플릿명|
|-- senderName|	String|	발신자 이름|
|-- senderAddress|	String|	발신자 메일 주소|
|-- resultId|	String|	메일 발송 ID|
|-- title|	String|	메일 제목|
|-- body|	String|	메일 내용|
|-- receiverList|	List|	수신자 리스트|
|--- requestId|	String|	요청 ID|
|--- mailSeq|	Integer|	메일 순번|
|--- receiveType|	String|	수신자 타입<br/>MRT0 : 받는 사람 , MRT1 : 참조, MRT2 : 숨은 참조|
|--- receiveTypeName|	String|	수신자 타입명|
|--- receiveName|	String|	수신자 이름|
|--- receiveMailAddr|	String|	수신자 메일 주소|
|--- isReceived| Boolean| 수신 여부 |
|--- resultDate| String| 수신 일시|
|--- isOpened| Boolean| 읽음 여부 |
|--- openedDate| String| 읽은 일시|
|--- dsnCode| String| DSN(Delivery Status Notification) 상태 코드|
|--- dsnMessage| String| DSN(Delivery Status Notification) 상태 메시지 |
|-- attachFileList|	List| 첨부파일 리스트|
|--- fileType|	String|	첨부파일 타입 <br/>MAIL: 메일에 첨부된 파일, TEMPLATE: 템플릿에 첨부된 파일|
|--- fileId| Integer| 파일 ID|
|--- fileName|	String|	첨부파일 이름|
|--- filePath|	String|	첨부파일 경로|
|--- fileSize|	Integer|	첨부 파일 크기(byte)|
|--- createDate|	String|	생성 일시|
|-- customHeaders|	Map|	[사용자 지정 헤더](./console-guide/#custom-header) |
|-- senderGroupingKey|	String|	발신자 그룹 키 |

<a id="query-of-mails-1"></a>

### 대량 메일 리스트 조회

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
|senderMail|	String|	X|	발신 메일 주소|
|senderName|	String|	X|	발신자 이름|
|templateId|	String|	X|	템플릿 ID|
|sendStatus|	String|	X|	발송 상태 코드 <br/> WAIT: 대기, READY: 발송 준비, <br/>SENDREADY: 발송 준비완료, SENDWAIT: 발송대기, <br/>SENDING: 발송 중, COMPLETE: 발송 완료, <br/>FAIL: 발송 실패, CANCEL: 발송취소|
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

<a id="curl-4"></a>

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
            "sendStatusName":"발송 완료",
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
|-- sendStatus|	String|	발송 상태 코드 <br/> WAIT: 대기, READY: 발송 준비, <br/>SENDREADY: 발송 준비완료, SENDWAIT: 발송대기, <br/>SENDING: 발송 중, COMPLETE: 발송 완료, <br/>FAIL: 발송 실패, CANCEL: 발송취소|
|-- sendStatusName|	String|	발송 상태명|
|-- templateId|	String|	템플릿 ID|
|-- templateName|	String|	템플릿명|
|-- senderName|	String|	발신자 이름|
|-- senderAddress|	String|	발신자 메일 주소|
|-- title|	String|	메일 제목|
|-- body|	String|	메일 내용|
|-- adYn |  String  | 광고 여부 |
|-- createDate |  String  | 생성 일시 |
|-- updateDate |  String  | 수정 일시 |

<a id="query-of-mails-2"></a>

### 대량 메일 발송 수신자 조회

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

|값|	타입|	필수|	설명|
|---|---|---|---|
|receiveMailAddr|	String|	X|	수신 메일 주소|
|startReceiveDate|	String|	X|	수신 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endReceiveDate|	String|	X|	수신 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|mailStatusCode|	String|	X|	발송 상태 코드 <br/> SST0:발송 준비, SST1:발송 중,  <br/> SST2:발송 완료, SST3: 발송 실패, SST7: 미인증|
|pageNum|	Integer|	X|	페이지 번호 1(기본값)|
|pageSize|	Integer|	X|	조회 건수 15(기본값)|

<a id="curl-5"></a>

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
            "mailStatusName":"발송 완료",
            "resultId":"<20210806120100sA3CcWU2@tcmmsapp-92a901>",
            "receiveType":"MRT0",
            "receiveTypeName":"받는 사람",
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
|-- mailStatusCode | String  | 메일 상태 코드 <br/> SST0:발송 준비, SST1:발송 중,  <br/> SST2:발송 완료, SST3: 발송 실패, SST7: 미인증|
|-- mailStatusName | String  | 메일 상태명 |
|-- resultId | String  | SMTP ID |
|-- receiveType|	String|	수신자 타입<br/>MRT0 : 받는 사람 , MRT1 : 참조, MRT2 : 숨은 참조|
|-- receiveTypeName|	String|	수신자 타입명|
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

### 대량 메일 발송 상세 조회

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

<a id="curl-6"></a>

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
         "mailStatusName":"발송 완료",
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
               "receiveTypeName":"받는 사람",
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
|-- templateName | String  | 템플릿명 |
|-- mailStatusCode | String  | 메일 상태 코드 <br/> SST0:발송 준비, SST1:발송 중,  <br/> SST2:발송 완료, SST3: 발송 실패, SST7: 미인증 |
|-- mailStatusName | String  | 메일 상태명 |
|-- requestDate | String  | 요청 시간 |
|-- senderName | String  | 발신자명 |
|-- senderAddress | String  | 발신자 주소 |
|-- resultId | String  | SMTP ID |
|-- resultDate | String  | 실제 발송 시간 |
|-- title | String  | 제목 |
|-- body | String  | 내용 |
|-- customHeaders|	Map|	[사용자 지정 헤더](./console-guide/#custom-header) |
|-- receiverList | List| 수신자 리스트|
|--- requestId | String  | 요청 ID |
|--- mailSeq | Integer  | 메일 순번 |
|--- receiveType | String  | 수신자 타입 (MRT0 : 받는 사람 , MRT1 : 참조, MRT2 : 숨은 참조) |
|--- receiveTypeName | String  | 수신자 타입명 |
|--- receiveMailAddr | String  | 수신자 메일 주소 |
|--- isReceived| Boolean| 수신 여부 |
|--- resultDate| String| 수신 일시|
|--- isOpened| Boolean| 읽음 여부 |
|--- openedDate| String| 읽은 일시|
|--- dsnCode| String| DSN(Delivery Status Notification) 상태 코드|
|--- dsnMessage| String| DSN(Delivery Status Notification) 상태 메시지 |
|-- attachFileList | List  | 첨부파일 리스트 |
|--- fileType|	String|	첨부 파일 타입(MAIL: 메일에 첨부된 파일, TEMPLATE: 템플릿에 첨부된 파일)|
|--- fileId| Integer| 파일 ID|
|--- fileName|	String|	첨부파일 이름|
|--- filePath|	String|	첨부파일 경로|
|--- fileSize|	Integer|	첨부 파일 크기(byte)|
|--- createDate|	String|	생성 일시|
<a id="scheduled-delivery-management"></a>

## 예약 관리

<a id="list-scheduled-delivery"></a>

### 예약 발송 리스트 조회
<a id="request-7"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v2.0/appKeys/{appKey}/sender/reservations|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

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
| requestId | String | X | 요청 ID |
| startSendDate | String | X | 발송 요청 시작 시간(yyyy-MM-dd HH:mm:ss) |
| endSendDate | String | X | 발송 요청 종료 시간(yyyy-MM-dd HH:mm:ss) |
| senderMail | String | X | 발신 메일 주소 |
| receiveMail | String | X | 수신 메일 주소 |
| templateId | String | X | 템플릿 ID |
| pageNum | Integer | X | 페이지 번호 1(기본값) |
| pageSize | Integer | X | 조회 건수 15(기본값) |

<a id="curl-7"></a>

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```


<a id="response-7"></a>

#### 응답

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
            "receiveTypeName":"받는 사람",
            "requestDate":"2020-06-05 17:53:00",
            "mailStatusCode":"SST4",
            "mailStatusName":"예약대기",
            "senderGroupingKey":"senderKey"
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
|- data|	List|	데이터 영역|
|-- requestId|	String | 요청 ID |
|-- mailSeq|	Integer| 메일 순번 |
|-- title|	String| 메일 제목 |
|-- templateId|	String|	템플릿 ID|
|-- templateName|	String|	템플릿 이름|
|-- senderAddress|	String|	발신자 메일 주소|
|-- senderName|	String|	발신자 이름|
|-- receiveMailAddr|	String|	수신자 메일 주소|
|-- receiveName|	String|	수신자 이름|
|-- receiveType|	String|	수신자 타입 (MRT0 : 받는 사람 , MRT1 : 참조, MRT2 : 숨은 참조)|
|-- receiveTypeName|	String|	수신자 타입 이름|
|-- requestDate|	String|	요청 시간|
|-- mailStatusCode|	String|	메일 상태 코드 <br/> SST0: 발송 준비, SST1: 발송 중  <br/> SST2: 발송 완료, SST3: 발송 실패 <br/> SST4: 예약 대기, SST7: 미인증 |
|-- mailStatusName|	String|	메일 상태 이름|
|-- senderGroupingKey|	String|	발신자 그룹 키 (최대 100자) |


<a id="query-detail-scheduled-delivery"></a>

### 예약 발송 상세 조회
<a id="request-8"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v2.0/appKeys/{appKey}/sender/reservations/{requestId}/{mailSeq}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|requestId|	String|	요청 ID|
|mailSeq|	Integer| 메일 순번 (메일 리스트 조회 시 반환되는 mailSeq 값) |

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

<a id="curl-8"></a>

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations/'"${REQUEST_ID}"'/'"${MAIL_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="response-8"></a>

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
               "receiveTypeName":"받는 사람",
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	List|	데이터 영역|
|-- requestId|	String | 요청 ID |
|-- mailSeq|	Integer| 메일 순번 |
|-- title|	String| 메일 제목 |
|-- body|	String| 메일 본문 |
|-- templateId|	String|	템플릿 ID|
|-- templateName|	String|	템플릿 이름|
|-- senderAddress|	String|	발신자 메일 주소|
|-- senderName|	String|	발신자 이름|
|-- requestDate|	String|	요청 시간|
|-- mailStatusCode|	String|	메일 상태 코드 <br/> SST0: 발송 준비, SST1: 발송 중  <br/> SST2: 발송 완료, SST3: 발송 실패 <br/> SST4: 예약 대기, SST7: 미인증 |
|-- mailStatusName|	String|	메일 상태 이름|
|-- receiverLis | List | 수신자 리스트 |
|--- requestId|	String | 요청 ID |
|--- mailSeq|	Integer| 메일 순번 |
|--- receiveType|	String|	수신자 타입 (MRT0 : 받는 사람 , MRT1 : 참조, MRT2 : 숨은 참조)|
|--- receiveName|	String|	수신자 이름|
|--- receiveTypeName|	String|	수신자 타입 이름|
|--- receiveMailAddr|	String|	수신자 메일 주소|
|-- attachFileList|	List| 첨부파일 리스트|
|--- fileType|	String|	첨부파일 타입 <br/>MAIL: 메일에 첨부된 파일, TEMPLATE: 템플릿에 첨부된 파일|
|--- fileId| Integer| 파일 ID <br/> v1.0, v1.1 API를 통해 업로드된 첨부파일은 파일 ID 값이 -1으로 표기됩니다.|
|--- fileName|	String|	첨부파일 이름|
|--- filePath|	String|	첨부파일 경로|
|--- fileSize|	Integer|	첨부 파일 크기(byte)|
|--- createDate|	String|	생성 일시|
|-- customHeaders|	Map|	[사용자 지정 헤더](./console-guide/#custom-header) |
|-- senderGroupingKey|	String|	발신자 그룹 키 (최대 100자) |

<a id="cancel-scheduled-delivery-by-request"></a>

### 예약 발송 취소 - 요청별
<a id="request-9"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|PUT| /email/v2.0/appKeys/{appKey}/sender/reservations/{requestId}

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

<a id="curl-9"></a>

#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations/'"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

<a id="response-9"></a>

#### 응답

```json
{
    "header" : {
        "isSuccessful" : true,
        "resultCode" : 0,
        "resultMessage" : "SUCCESS"
    }
}
```

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|

<a id="cancel-scheduled-delivery-by-recipient"></a>

### 예약 발송 취소 - 수신자별
<a id="request-10"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|PUT| /email/v2.0/appKeys/{appKey}/sender/reservations/{requestId}/{mailSeq}

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|requestId|	String|	요청 ID|
|mailSeq|	Integer| 메일 순번 (메일 리스트 조회 시 반환되는 mailSeq 값) |

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

<a id="curl-10"></a>

#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations/'"${REQUEST_ID}"'/'"${MAIL_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

<a id="response-10"></a>

#### 응답

```json
{
    "header" : {
        "isSuccessful" : true,
        "resultCode" : 0,
        "resultMessage" : "SUCCESS"
    }
}
```

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|

<a id="cancel-scheduled-delivery---multiple-filter"></a>

### 예약 발송 취소 - 다중 필터
<a id="request-11"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|PUT| /email/v2.0/appKeys/{appKey}/sender/reservations/search-cancels

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

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

|값|	타입|	필수|	설명|
|---|---|---|---|
| searchParameter.requestId | String | X | 요청 ID |
| searchParameter.startSendDate | String | O | 예약 날짜 시작 (yyyy-MM-dd HH:mm:ss) |
| searchParameter.endSendDate | String | O | 예약 날짜 종료 (yyyy-MM-dd HH:mm:ss) |
| searchParameter.senderMail | String | X | 발신자 메일 주소 |
| searchParameter.receiveMail | String | X | 수신자 메일 주소 |
| searchParameter.templateId | String | X | 템플릿 ID |
| updateUser | String | O | 예약 취소 요청자 |

* **startSendDate**, **endSendDate**, **updateUser** 요청 필드가 필수입니다.

<a id="curl-11"></a>

#### cURL
```
curl -X PUT \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations/search-cancels' \
-H 'Content-Type: application/json;charset=UTF-8' \
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

<a id="response-11"></a>

#### 응답

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	List|	데이터 영역|
|-- reservationCancelId|	String | 예약 취소 ID |
|-- requestedDateTime|	String| 예약 취소 시간(yyyy-MM-dd HH:mm:ss) |
|-- reservationCancelStatus|	String| 예약 취소 상태 </br>- READY: 예약 준비</br>- PROCESSING: 예약 취소 중</br>- COMPLETED: 예약 취소 완료</br>- FAILED: 예약 취소 실패|


<a id="list-request-of-scheduled-delivery-cancellation---multiple-filter"></a>

### 예약 발송 취소 요청 목록 검색 - 다중 필터
<a id="request-12"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v2.0/appKeys/{appKey}/sender/reservations/search-cancels

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

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
| startRequestedDateTime | String | X | 예약 취소 요청 시작 시간(yyyy-MM-dd HH:mm:ss) |
| endRequestedDateTime | String | X | 예약 취소 요청 종료 시간(yyyy-MM-dd HH:mm:ss) |
| reservationCancelId | String | X | 예약 취소 ID |
| pageNum | Integer | X | 페이지 번호 1(기본값) |
| pageSize | Integer | X | 조회 건수 15(기본값) |

<a id="curl-12"></a>

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/sender/reservations/search-cancels' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="response-12"></a>

#### 응답

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
|- data|	List|	데이터 영역|
|-- reservationCancelId|	String | 예약 취소 ID |
|-- searchParameter | Map<String, Object> | 예약 취소 요청 파라미터 |
|-- requestedDateTime|	String | 예약 취소 요청 시간 |
|-- completedDateTime|	String | 예약 취소 완료 시간 |
|-- reservationCancelStatus|	String| 예약 취소 상태 </br>- READY: 예약 준비</br>- PROCESSING: 예약 취소 중</br>- COMPLETED: 예약 취소 완료</br>- FAILED: 예약 취소 실패|
|-- totalCount | Integer | 예약 취소 대상 건수 |
|-- successCount | Integer | 예약 취소 성공 건수 |
|-- createUser | Integer | 예약 취소 요청자 |
|-- createdDateTime | String | 예약 취소 요청 생성 시간 |
|-- updatedDateTime | String | 예약 취소 수정 시간 |

<p id="category"></p>

<a id="category-management"></a>

## 카테고리 관리

<a id="list"></a>

### 카테고리 목록 조회

<a id="request-13"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v2.0/appKeys/{appKey}/categories|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

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
|useYn|	String|	X|	사용 여부 Y, N |
|categoryParentId|	Integer|	X|	부모 카테고리 ID |
|pageNum|	Integer|	X|	페이지 번호 1(기본값)|
|pageSize|	Integer|	X|	조회 건수 15(기본값)|

<a id="curl-13"></a>

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/categories' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="response-13"></a>

#### 응답

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
|- data|	List|	데이터 영역|
|-- categoryId|	Integer|	카테고리 ID|
|-- categoryParentId|	Integer| 부모 카테고리 ID(최상위 카테고리인 경우 0)|
|-- depth|	Integer| depth (최상위 카테고리인 경우 0) |
|-- categoryName|	String|	카테고리 이름|
|-- categoryDesc|	String|	카테고리 설명|
|-- useYn|	String|	사용 여부|
|-- createUser|	String|	생성자|
|-- createDate|	String|	생성 일시|
|-- updateUser|	String|	수정자|
|-- updateDate|	String|	수정 일시|

<a id="query-details"></a>

### 카테고리 상세 조회

<a id="request-14"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/categories/{categoryId}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|categoryId|	String|	카테고리 ID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

<a id="curl-14"></a>

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/categories/'"${CATEGORY_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="response-14"></a>

#### 응답

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	List|	데이터 영역|
|-- categoryId|	Integer|	카테고리 ID|
|-- categoryParentId|	Integer| 부모 카테고리 ID(최상위 카테고리인 경우 0)|
|-- depth|	Integer| depth (최상위 카테고리인 경우 0) |
|-- categoryName|	String|	카테고리 이름|
|-- categoryDesc|	String|	카테고리 설명|
|-- useYn|	String|	사용 여부|
|-- createUser|	String|	생성자|
|-- createDate|	String|	생성 일시|
|-- updateUser|	String|	수정자|
|-- updateDate|	String|	수정 일시|


<a id="register"></a>

### 카테고리 등록

<a id="request-15"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|/email/v2.0/appKeys/{appKey}/categories|


[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

[Request body]

|값|	타입|	최대 길이 | 필수|	설명|
|---|---|---|---|---|
| categoryParentId |	Integer|	- | X |	부모 카테고리 ID <br/> 최상위 카테고리 ID(기본값) |
| categoryName |	String|	200 | O |	카테고리 이름 |
| categoryDesc |	String| 1000 |	X |	카테고리 설명|
| useYn |	String| 1 |	X|	사용 여부 Y(기본값), N|
| userId | String | 50 | X | 사용자 ID |

<a id="curl-15"></a>

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


<a id="response-15"></a>

#### 응답

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

|값|	타입| 설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	Object|	데이터 영역|
|-- categoryId|	Integer|	카테고리 ID|


<a id="modify"></a>

### 카테고리 수정

<a id="request-16"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|PUT|/email/v2.0/appKeys/{appKey}/categories/{categoryId}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|categoryId|	Integer|	카테고리 ID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

[Request body]

|값|	타입|	최대 길이 | 필수|	설명|
|---|---|---|---|---|
| categoryName |	String|	200 | X |	카테고리 이름 |
| categoryDesc |	String| 1000 |	X |	카테고리 설명|
| useYn |	String| 1 |	X|	사용 여부 Y, N|
| userId | String | 50 | X | 사용자 ID |

<a id="curl-16"></a>

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

<a id="response-16"></a>

#### 응답

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean| 성공 여부 |
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|

<a id="delete"></a>

### 카테고리 삭제

<a id="request-17"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|DELETE|/email/v2.0/appKeys/{appKey}/categories/{categoryId}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|categoryId|	Integer|	카테고리 ID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

<a id="curl-17"></a>

#### cURL
```
curl -X DELETE \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/categories/'"${CATEGORY_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="response-17"></a>

#### 응답

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|

<p id="template"></p>

<a id="query-of-templates"></a>

## 템플릿 관리

<a id="query-list-of-templates"></a>

### 템플릿 리스트 조회

<a id="request-18"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/templates|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

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
|categoryId|	Integer|	X|	카테고리 ID|
|useYn|	String|	X|	사용 여부(Y/N)|
|pageNum|	Integer|	X|	페이지 번호 1(기본값)|
|pageSize|	Integer|	X|	조회 건수 15(기본값)|

<a id="curl-18"></a>

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/templates' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="response-18"></a>

#### 응답

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
|- data|	List|	데이터 영역|
|-- templateId|	String|	템플릿 ID|
|-- categoryId|	Integer|	카테고리 ID|
|-- categoryName|	String|	카테고리명|
|-- templateName|	String|	템플릿명|
|-- templateDesc|	String|	템플릿 설명|
|-- useYn|	String|	사용 여부|
|-- delYn|	String|	삭제 여부|
|-- title|	String|	메일 제목|
|-- createDate|	String|	생성 일시|
|-- updateDate|	String|	수정 일시|

<a id="query-template-details"></a>

### 템플릿 상세 조회

<a id="request-19"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/templates/{templateId}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|templateId|	String|	템플릿 ID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

<a id="curl-19"></a>

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="response-19"></a>

#### 응답

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
            "sendMailAddress": "test@example.com",
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	List|	데이터 영역|
|-- templateId|	String|	템플릿 ID|
|-- categoryId|	Integer|	카테고리 ID|
|-- categoryName|	String|	카테고리명|
|-- templateName|	String|	템플릿명|
|-- templateDesc|	String|	템플릿 설명|
|-- useYn|	String|	사용 여부 (Y= 사용 중, N= 사용 안 함)|
|-- delYn|	String|	삭제 여부(Y= 삭제, N= 삭제 아님)|
|-- sendMailAddress|	String|	발신 메일 주소|
|-- title|	String|	메일 주소|
|-- templateType|	String|	템플릿 타입 <br/>DEFAULT(기본값), FREEMARKER)|
|-- body|	String|	메일 내용|
|-- createDate|	String|	생성 일시|
|-- updateDate|	String|	수정 일시|
|-- attachFileList|	List|	첨부파일 리스트|
|--- fileType|	String|	첨부 파일 타입(MAIL: 메일에 첨부된 파일, TEMPLATE: 템플릿에 첨부된 파일)|
|--- fileId| Integer| 파일 ID|
|--- fileName|	String|	첨부파일 이름|
|--- filePath|	String|	첨부파일 경로|
|--- fileSize|	Integer|	첨부 파일 크기(byte)|
|--- createDate|	String|	생성 일시|

<a id="register-templates"></a>

### 템플릿 등록

<a id="request-20"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/templates|


[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

[Request body]

|값|	타입|	최대 길이 | 필수|	설명|
|---|---|---|---|---|
| categoryId |	Integer|	- | O |	카테고리 ID |
| templateId | String | 50 | O | 템플릿 ID |
| templateName |	String| 200 |	O |	템플릿명|
| templateDesc |	String| 4000 |	X |	템플릿 설명|
| useYn |	String| 1 |	X|	사용 여부 Y(기본값), N|
| sendMailAddress | String| 300 | O| 발신 메일 주소 |
| title | String | 500 | O | 메일 제목 |
| templateType |	String| 10 |	X|	템플릿 타입 <br/>DEFAULT(기본값), FREEMARKER)|
| body | String | - | O | 메일 본문 |
| attachFileIdList | List<Integer> | - | X | 첨부 파일 ID(fileId) |
| userId | String | 50 | X | 사용자 ID |

<a id="curl-20"></a>

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

<a id="response-20"></a>

#### 응답

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

|값|	타입|	설명|
|---|---|---|
|header|    Object| 헤더 영역|
|- isSuccessful|    Boolean| 성공 여부 |
|- resultCode|  Integer|    실패 코드|
|- resultMessage|   String| 실패 메시지|

<a id="upload-attached-files-2"></a>

### 템플릿 첨부 파일 업로드

<a id="request-21"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v2.0/appKeys/{appKey}/templates/attachfile/binaryUpload|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

[Request body]

|값|	타입| 	최대 길이 | 필수|	설명|
|---|---|--------|---|---|
|fileName|	String| 	-     |O|	파일 이름|
|fileBody|	Byte[]| 	-     |O|	파일의 Byte[] 값|
|userId|	String| 	50    |X|	유저 ID|

<a id="curl-21"></a>

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

<a id="response-21"></a>

#### 응답

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

|값| 타입| 설명|
|---|---|---|
|header|    Object| 헤더 영역|
|- isSuccessful|    Boolean| 성공 여부 |
|- resultCode|  Integer|    실패 코드|
|- resultMessage|   String| 실패 메시지|
|body|  Object| 본문 영역|
|- data|    Object| 데이터 영역|
|-- fileId| Integer| 파일 ID|
|-- fileName|   String| 파일 이름|

<a id="modify-templates"></a>

### 템플릿 수정

<a id="request-22"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|PUT|	/email/v2.0/appKeys/{appKey}/templates/{templateId}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|templateId|	String|	템플릿 ID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

[Request body]

|값|	타입|	최대 길이 | 필수|	설명|
|---|---|---|---|---|
| templateName |	String| 200 |	X |	템플릿명|
| templateDesc |	String| 4000 |	X |	템플릿 설명|
| useYn |	String| 1 |	X |	사용 여부 Y, N|
| sendMailAddress | String| 300 | X| 발신 메일 주소 |
| title | String | 500 | X | 메일 제목 |
| templateType |    String| 10 |    X| 템플릿 타입 <br/>DEFAULT(기본값), FREEMARKER |
| body | String | - | X | 메일 본문 |
| attachFileIdList | List<Integer> | - | X | 첨부 파일 ID(fileId) |
| userId | String | 50 | X | 사용자 ID |

<a id="curl-22"></a>

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

<a id="response-22"></a>

#### 응답

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

|값| 타입| 설명|
|---|---|---|
|header|    Object| 헤더 영역|
|- isSuccessful|    Boolean| 성공 여부 |
|- resultCode|  Integer|    실패 코드|
|- resultMessage|   String| 실패 메시지|

<a id="delete-templates"></a>

### 템플릿 삭제

<a id="request-23"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|DELETE|	/email/v2.0/appKeys/{appKey}/templates/{templateId}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|templateId|	String|	템플릿 ID|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

<a id="curl-23"></a>

#### cURL
```
curl -X DELETE \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"'' 
```

<a id="response-23"></a>

#### 응답

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

|값| 타입| 설명|
|---|---|---|
|header|    Object| 헤더 영역|
|- isSuccessful|    Boolean| 성공 여부 |
|- resultCode|  Integer|    실패 코드|
|- resultMessage|   String| 실패 메시지|
<a id="query-statistics"></a>

## 통계 조회

<a id="query-daily-statistics"></a>

### 통합 통계 조회

<a id="request-24"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v2.0/appKeys/{appKey}/statistics/view |

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

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

|값|	타입|	필수| 설명|
|---|---|---|---|
|from|	String|	O | 통계 조회 시작 날짜<br/> yyyy-mm-dd HH:mm|
|to|	String|	O | 통계 조회 종료 날짜<br/> yyyy-mm-dd HH:mm|
|searchType| String | O | 통계 구분<br/>DATE:날짜별, TIME:시간별, DAY:요일별 |
|mailTypes | String | X | 메일 타입<br/>NORMAL:일반, MASS:대량<br/>복수 입력 가능(mailTypes=NORMAL&mailTypes=MASS) |
|adYn | String | X | 광고 여부<br>Y:광고, N:광고 아님<br>입력하지 않으면 전체|
|templateId | String | X | 템플릿 ID |

<a id="curl-24"></a>

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/statistics/view?from='"${FROM}"'&to='"${TO}"'&searchType='"${SEARCH_TYPE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```


<a id="response-24"></a>

#### 응답

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	List|	데이터 영역|
|-- divisionName | String | 통계 기준(날짜/시간/요일) |
|-- requestedCount | Long | 발송 요청 카운트 |
|-- sentCount | Long | 발송 카운트 |
|-- receivedCount | Long | 수신 카운트 |
|-- openedCount | Long | 오픈 카운트 |
|-- sentRate | String | 발송률 |
|-- receivedRate | String | 수신율 |
|-- openedRate | String | 오픈율 |

<a id="rejection-management"></a>

## 수신 거부 관리

<a id="query-rejections"></a>

### 수신 거부 조회

<a id="request-25"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
| GET |	/email/v2.0/appKeys/{appKey}/block-receivers |

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

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

|값|	타입|	필수| 설명|
|---|---|---|---|
|mailAddress|	String|	X| 수신 거부 목록에 등록되어 있는 이메일 주소|
|startBlockDate|	String|	X| 수신 거부 시작값 (yyyy-MM-dd HH:mm:ss)|
|endBlockDate|	String|	X| 수신 거부 종료값 (yyyy-MM-dd HH:mm:ss)|
|pageNum|	Integer|	X|	페이지 번호 1(기본값)|
|pageSize|	Integer|	X|	조회 건수 15(기본값)|

<a id="curl-25"></a>

#### cURL
```
curl -X GET \
'https://email.api.nhncloudservice.com/email/v2.0/appKeys/'"${APP_KEY}"'/block-receivers' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: '"${SECRET_KEY}"''
```

<a id="response-25"></a>

#### 응답
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
                "mailAddress": "block@example.com",
                "blockDate": "2019-01-01 00:00:00.0"
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
|-- mailAddress | String | 수신 거부 이메일 주소 |
|-- blockDate | String | 수신 거부 날짜 (yyyy-MM-dd HH:mm:ss.S)

<a id="register-rejections"></a>

### 수신 거부 등록

<a id="request-26"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
| POST |	/email/v2.0/appKeys/{appKey}/block-receivers |

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

[Request body]

|값|	타입|	필수 | 설명|
|---|---|---|---|
| blockReceiverList |  ㅣList | O | 수신 거부 리스트 |
| - mailAddress | String | O | 수신 거부 이메일 주소 |
| - blockDate | String | X | 수신 거부 날짜 (yyyy-MM-dd HH:mm:ss) |

<a id="curl-26"></a>

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

<a id="response-26"></a>

#### 응답
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|

<a id="delete-rejections"></a>

### 수신 거부 삭제
<a id="request-27"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
| PUT |	/email/v2.0/appKeys/{appKey}/block-receivers |

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Header]

```
{
  "X-Secret-Key": String
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|X-Secret-Key|	String| O | 고유의 secretKey [[참고](./api-guide/#secret-key)] |

[Request body]

|값|	타입|	필수 | 설명|
|---|---|---|---|
| deleted | Boolean | O | 수신 거부 삭제를 명시하는 필드 |
| blockReceiverList |  ㅣList | O | 수신 거부 리스트 |
| - mailAddress | String | O | 수신 거부 이메일 주소 |

<a id="curl-27"></a>

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

<a id="response-27"></a>

#### 응답
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
