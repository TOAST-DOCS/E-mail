<!-- pre-align:aligned sig=7fd573c335e4 -->

## Notification > Email > API v1.5 가이드

[API 도메인]

|환경|	도메인|
|---|---|
|Real|	https://email.api.nhncloudservice.com |

[Header]
```
Content-Type: application/json;charset=UTF-8
```


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
|POST|	/email/v1.5/appKeys/{appKey}/sender/mail|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

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


[주의]

* template을 사용할 경우 title, body는 필수 값이 아닙니다. (입력 시 입력된 값이 template보다 우선 적용)

[예시 1]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/mail -d '{"senderAddress":"support@nhn.com","senderName":"발송자이름","title":"샘플 타이틀","body":"샘플 내용","receiverList":[{"receiveMailAddr":"customer1@nhn.com","receiveName":"고객1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhn.com","receiveName":"고객2","receiveType":"MRT1"}],"userId":"USER"}'
```

[예시 2 - 템플릿 사용]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/mail -d '{"templateId":"TEMPLATE1","templateParameter":{"key":"value"},"receiverList":[{"receiveMailAddr":"customer1@nhn.com","receiveName":"고객1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhn.com","receiveName":"고객2","receiveType":"MRT1"}],"userId":"USER"}'
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

<a id="updated-for-v15"></a>

#### v1.5에서 달라진 사항

* 발신자 그룹 키 필드가 추가되었습니다. 요청 단위로 설정할 수 있습니다.
* 발송을 요청할 때 **senderGroupingKey** 필드를 지정하고, 요청 조회 시 활용할 수 있습니다.

<a id="send-individual-mails"></a>

### 개별 메일 발송

* 수신자가 여러 명일 때 수신자 각각에게 메일을 발송하는 기능입니다. 여러 명에게 보내도 수신자에게는 본인만 표시됩니다.

<a id="request-2"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/sender/eachMail||

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

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

[주의]

* template을 사용할 경우 title, body는 필수 값이 아닙니다. (입력 시 입력된 값이 template 보다 우선적용)


[예시 1]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/eachMail -d '{"senderAddress":"support@nhn.com","senderName":"발송자이름","title":"샘플 타이틀","body":"샘플 내용","attachFileIdList":[1,2],"receiverList":[{"receiveMailAddr":"customer1@nhn.com","receiveName":"고객1"}],"userId":"USER"}'
```

[예시 2 - 템플릿 사용]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/mail -d '{"templateId":"TEMPLATE1","receiverList":[{"receiveMailAddr":"customer1@nhn.com","receiveName":"고객1","templateParameter":{"key":"value"}}],"userId":"USER"}'
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
|--- receiveType|	String|	수신자 타입 (MRT0 : 받는 사람 , MRT1 : 참조, MRT2 : 숨은 참조) |
|--- resultCode|	Integer|	수신자 발송 요청 결과 코드|
|--- resultMessage|	String|	수신자 발송 요청 결과 메시지|


<a id="updated-for-v15-2"></a>

#### v1.5에서 달라진 사항

* 발신자 그룹 키 필드가 추가되었습니다. 요청 단위로 설정할 수 있습니다.
* 발송을 요청할 때 **senderGroupingKey** 필드를 지정하고, 요청 조회 시 활용할 수 있습니다.

<a id="sending-general-ad-mails"></a>

### 광고성 일반 메일 발송
* 요청, 응답 정보는 일반 메일 발송과 동일합니다.

<a id="caution-for-sending-ad-mails"></a>

#### 광고메일 전송 시 유의 사항
* 제목에 반드시 (광고) 문구를 삽입하도록 강제하고 있습니다.
* 자세한 내용은 [[광고성 메일 발송](./console-guide/#_3)]를 참고해주세요.

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/sender/ad-mail|

[예시 1]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/ad-mail -d '{"senderAddress":"support@nhn.com","senderName":"발송자이름","title":"(광고) 샘플 타이틀","body":"샘플 내용 <br /> ##BLOCK_RECEIVER_LINK## <br />##EN_BLOCK_RECEIVER_LINK##","receiverList":[{"receiveMailAddr":"customer1@nhn.com","receiveName":"고객1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhn.com","receiveName":"고객2","receiveType":"MRT1"}],"userId":"USER"}'
```

[예시 2 - 템플릿 사용]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/ad-mail -d '{"templateId":"TEMPLATE1","templateParameter":{"key":"value"},"receiverList":[{"receiveMailAddr":"customer1@nhn.com","receiveName":"고객1","receiveType":"MRT0"},{"receiveMailAddr":"customer2@nhn.com","receiveName":"고객2","receiveType":"MRT1"}],"userId":"USER"}'
```

<a id="sending-individual-ad-mails"></a>

### 광고성 개별 메일 발송

* 요청, 응답 정보는 개별 메일 발송과 동일합니다.

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/sender/ad-eachMail |

[예시 1]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/ad-eachMail -d '{"senderAddress":"support@nhn.com","senderName":"발송자이름","title":"(광고) 샘플 타이틀","body":"샘플 내용 <br /> ##BLOCK_RECEIVER_LINK## <br />##EN_BLOCK_RECEIVER_LINK##","attachFileIdList":[1,2],"receiverList":[{"receiveMailAddr":"customer1@nhn.com","receiveName":"고객1"}],"userId":"USER"}'
```

[예시 2 - 템플릿 사용]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/ad-eachMail -d '{"templateId":"TEMPLATE1","receiverList":[{"receiveMailAddr":"customer1@nhn.com","receiveName":"고객1","templateParameter":{"key":"value"}}],"userId":"USER"}'
```

<a id="send-authenticated-mails"></a>

### 인증 메일 발송

<a id="request-3"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/sender/auth-mail||

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

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
|customHeaders| Map| X| [사용자 지정 헤더](./console-guide/#custom-header)|
|userId|	String|	X|	발송 구분자 ex)admin,system ( 최대 50자 )|

[주의]

* template을 사용할 경우 title, body는 필수 값이 아닙니다. (입력 시 입력된 값이 template 보다 우선적용)

<a id="differences-from-general-mails"></a>

#### 일반 메일과 다른 점
인증 메일 성격상 다음과 같이 다른 특성들이 있습니다.

* 단건 발송(1명의 수신자)만 가능합니다.
* 첨부 파일 기능을 지원하지 않습니다. 첨부 파일이 포함된 템플릿은 지원하지 않습니다.

[예시 1]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/auth-mail -d '{"senderAddress":"support@nhn.com","senderName":"발송자이름","title":"샘플 타이틀","body":"샘플 내용","receiver":{"receiveMailAddr":"customer1@nhn.com","receiveName":"고객1"},"userId":"USER"}'
```

[예시 2 - 템플릿 사용]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/auth-mail -d '{"templateId":"TEMPLATE1","receiver":{"receiveMailAddr":"customer1@nhn.com","receiveName":"고객1","templateParameter":{"key":"value"}},"userId":"USER"}'
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
|POST|	/email/v1.5/appKeys/{appKey}/attachfile/binaryUpload|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request body]

|값|	타입|	필수|	설명|
|---|---|---|---|
|fileName|	String|	O|	파일 이름|
|fileBody|	Byte[]|	O|	파일의 Byte[] 값|
|createUser|	String|	X|	파일 업로드 유저 정보|

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/attachfile/binaryUpload -d '{"fileName":"file.csv","createUser":"USER","fileBody":[]}'
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
```
{
    "senderAddress" : "support@nhn.com",
    "templateId": "template1",
    "templateParameter" : {"title_name": "클라우드고객1", "body_content": "test1"},
    "receiverList" : [
        {
            "receiveMailAddr" : "customer1@nhn.com",
            "receiveType" : "MRT0"
        }

    ],
    "userId" : "tester"
}
```

<a id="example-of-individual-mail-request"></a>

#### 개별 메일요청 예시
```
{
    "senderAddress" : "support@nhn.com",
    "templateId": "template1",
    "receiverList" : [
        {
            "receiveMailAddr" : "customer1@nhn.com",
            "templateParameter" : {"title_name": "클라우드고객1", "body_content": "test1"}
        },
        {
            "receiveMailAddr" : "customer2@nhn.com",
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
|GET|	/email/v1.5/appKeys/{appKey}/sender/mails|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수| 	설명                                                                           |
|---|---|---|-------------------------------------------------------------------------------|
|requestId|	String|	O| 	요청 ID                                                                        |
|startSendDate|	String|	O| 	발송 날짜 시작 값(yyyy-MM-dd HH:mm:ss)                                              |
|endSendDate|	String|	O| 	발송 날짜 종료 값(yyyy-MM-dd HH:mm:ss)                                              |
|startReceiveDate|	String|	X| 	수신 날짜 시작 값(yyyy-MM-dd HH:mm:ss)                                              |
|endReceiveDate|	String|	X| 	수신 날짜 종료 값(yyyy-MM-dd HH:mm:ss)                                              |
|senderAddress|	String|	X| 	발신 메일 주소                                                                      |
|senderName|	String|	X| 	발신자 이름                                                                       |
|receiveMailAddr|	String|	X| 	수신 메일 주소                                                                     |
|templateId|	String|	X| 	템플릿 ID                                                                       |
|mailStatusCode|	String|	X| 	발송 상태 코드 <br/> SST0:발송 준비, SST1:발송 중,  <br/> SST2:발송 완료, SST3: 발송 실패, SST7: 미인증 |
|isReceived| Boolean| X | 수신 여부                                                                         |
|isOpened| Boolean| X | 읽음 여부                                                                         |
|senderGroupingKey| String| X| 발신자 그룹 키                                                                       |
|pageNum|	Integer|	X| 	페이지 번호 1(기본값)                                                                |
|pageSize|	Integer|	X| 	조회 건수 15(기본값)                                                                |

* **requestId** 또는 **startSendDate**, **endSendDate** 요청 필드가 필수입니다.


[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/mails?startSendDate=2018-03-01+00%3A00&endSendDate=2018-03-07+23%3A59&pageSize=10"
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
                "senderAddress": "sender@nhn.com",
                "title": "Title",
                "mailStatusCode": "SST2",
                "mailStatusName": "발송 완료",
                "isReceived": true,
                "resultDate": "2019-01-01 00:00:00",
                "isOpened": true,
                "openedDate": "2019-01-01 00:00:00",
                "receiveMailAddr": "receiver@nhn.com",
                "receiveType": "MRT0",
                "receiveTypeName": "받는 사람",
                "receiveName": "Receiver",
                "senderGroupingKey": "A_GROUP"
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

<a id="updated-for-v15-3"></a>

#### v1.5에서 달라진 사항

* 발신자 그룹 키 필드 **senderGroupingKey**가 추가되었습니다.
* 수신 여부 필드 **isReceived**가 추가되었습니다.
* 읽음 여부 필드 **isOpened**가 추가되었습니다.
* 읽은 일시 필드 **openedDate**가 추가되었습니다.
* 요청 필드 중 발송 상태 코드를 나타내는 필드 이름이 **mailStatusCode**로 변경되었습니다. 
* 수신자 메일 주소를 나타내는 필드 이름이 **receiveMailAddr**로 변경되었습니다.
* 발신자 메일 주소를 나타내는 필드 이름이 **senderAddress**로 변경되었습니다.


<a id="query-mail-delivery-details"></a>

### 메일 발송 상세 조회

<a id="request-6"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/sender/mail/{requestId}/{mailSeq}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|requestId|	String|	요청ID|
|mailSeq|	Integer| 메일 순번 (메일 리스트 조회 시 반환되는 mailSeq 값)|

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/sender/mail/{requestId}/{mailSeq}"
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
            "senderAddress": "sender@nhn.com",
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
|-- attachFileList|	List| 첨부파일 리스트|
|--- fileType|	String|	첨부파일 타입 <br/>MAIL: 메일에 첨부된 파일, TEMPLATE: 템플릿에 첨부된 파일|
|--- fileId| Integer| 파일 ID|
|--- fileName|	String|	첨부파일 이름|
|--- filePath|	String|	첨부파일 경로|
|--- fileSize|	Integer|	첨부 파일 크기(byte)|
|--- createDate|	String|	생성 일시|
|-- customHeaders|	Map|	[사용자 지정 헤더](./console-guide/#custom-header) |
|-- senderGroupingKey|	String|	발신자 그룹 키 |

<a id="updated-for-v15-4"></a>

#### v1.5에서 달라진 사항

* 발신자 그룹 키 필드 **senderGroupingKey**가 추가되었습니다.
* 수신자 목록를 나타내는 필드 이름이 **receiverList**로 변경되었습니다.
* 수신자의 수신 여부 필드 **isReceived**가 추가되었습니다.
* 수신자의 읽음 여부 필드 이름이 **isOpened**로 변경되었습니다.
* 수신자의 읽은 일시 필드 이름이 **openedDate**로 변경되었습니다.
* 발신자 메일 주소를 나타내는 필드 이름이 **senderAddress**로 변경되었습니다.
<a id="category-management"></a>

## 카테고리 관리

<a id="list"></a>

### 카테고리 목록 조회

<a id="request-7"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET| /email/v1.5/appKeys/{appKey}/categories|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수|	설명|
|---|---|---|---|
|useYn|	String|	X|	사용 여부 Y, N |
|categoryParentId|	Integer|	X|	부모 카테고리 ID |
|pageNum|	Integer|	X|	페이지 번호 1(기본값)|
|pageSize|	Integer|	X|	조회 건수 15(기본값)|

[예시]
``` sh
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/categories?useYn=Y&categoryParentId=1&pageNum=1&pageSize=10"
```

<a id="response-7"></a>

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
                "updateDate": "2019-07-23 00:00:00.0",
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

<a id="request-8"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/categories/{categoryId}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|categoryId|	String|	카테고리 ID|

[예시]
``` sh
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/categories/{categoryId}"
```

<a id="response-8"></a>

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

<a id="request-9"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|/email/v1.5/appKeys/{appKey}/categories|


[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request body]

|값|	타입|	최대 길이 | 필수|	설명|
|---|---|---|---|---|
| categoryParentId |	Integer|	- | X |	부모 카테고리 ID <br/> 최상위 카테고리 ID(기본값) |
| categoryName |	String|	200 | O |	카테고리 이름 |
| categoryDesc |	String| 1000 |	X |	카테고리 설명|
| useYn |	String| 1 |	X|	사용 여부 Y(기본값), N|
| userId | String | 50 | X | 사용자 ID |

[예시]
``` sh
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/categories -d '{"categoryParentId":12345,"categoryName":"Category","categoryDesc":"Top Category","useYn":"Y","userId":"USER"}'
```


<a id="response-9"></a>

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

|값|	타입|	설명|
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

<a id="request-10"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|PUT|/email/v1.5/appKeys/{appKey}/categories/{categoryId}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|categoryId|	Integer|	카테고리 ID|

[Request body]

|값|	타입|	최대 길이 | 필수|	설명|
|---|---|---|---|---|
| categoryName |	String|	200 | X |	카테고리 이름 |
| categoryDesc |	String| 1000 |	X |	카테고리 설명|
| useYn |	String| 1 |	X|	사용 여부 Y, N|
| userId | String | 50 | X | 사용자 ID |

[예시]
``` sh
curl -X PUT -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/categories/{categoryId} -d '{"categoryName":"Category","categoryDesc":"Top Category","useYn":"Y","userId":"USER"}'
```

<a id="response-10"></a>

#### 응답

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
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

<a id="request-11"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|DELETE|/email/v1.5/appKeys/{appKey}/categories/{categoryId}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|categoryId|	Integer|	카테고리 ID|

[예시]
``` sh
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/categories/{categoryId}
```

<a id="response-11"></a>

#### 응답

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
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

<a id="request-12"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/templates|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수|	설명|
|---|---|---|---|
|categoryId|	Integer|	X|	카테고리 ID|
|useYn|	String|	X|	사용 여부(Y/N)|
|pageNum|	Integer|	X|	페이지 번호 1(기본값)|
|pageSize|	Integer|	X|	조회 건수 15(기본값)|

[예시]
``` sh
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/templates?useYn=Y&pageNum=1&pageSize=10"
```

<a id="response-12"></a>

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

<a id="request-13"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/templates/{templateId}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|templateId|	String|	템플릿 ID|

[예시]
``` sh
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/templates/{templateId}"
```

<a id="response-13"></a>

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

<a id="request-14"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/templates|


[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

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

[예시]
``` sh
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/templates -d '{"categoryId":1,"templateId":"TEAMPLTE_ID","templateName":"템플릿 이름","templateDesc":"템플릿 설명","useYn":"Y","sendMailAddress":"test@nhn.com","title":"메일 제목","templateType":"DEFAULT","body":"메일 내용","attachFileIdList":[1,2,3],"userId":"USER"}'
```


<a id="response-14"></a>

#### 응답

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
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

<a id="request-15"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.5/appKeys/{appKey}/templates/attachfile/binaryUpload|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request body]

|값|	타입| 	최대 길이 | 필수|	설명|
|---|---|--------|---|---|
|fileName|	String| 	-     |O|	파일 이름|
|fileBody|	Byte[]| 	-     |O|	파일의 Byte[] 값|
|userId|	String| 	50    |X|	유저 ID|

[예시]
``` sh
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/templates/attachfile/binaryUpload -d '{"fileName":"file.csv","userId":"USER","fileBody":[]}'
```

<a id="response-15"></a>

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

<a id="request-16"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|PUT|	/email/v1.5/appKeys/{appKey}/templates/{templateId}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|templateId|	String|	템플릿 ID|

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

[예시]
``` sh
curl -X PUT -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/templates/{templateId} -d '{"templateName":"템플릿 이름","templateDesc":"템플릿 설명","useYn":"Y","sendMailAddress":"test@nhn.com","title":"메일 제목","templateType":"DEFAULT","body":"메일 내용","attachFileIdList":[1,2,3],"userId":"USER"}'
```

<a id="response-16"></a>

#### 응답

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
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

<a id="request-17"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|DELETE|	/email/v1.5/appKeys/{appKey}/templates/{templateId}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|templateId|	String|	템플릿 ID|

[예시]
``` sh
curl -X DELETE -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/templates/{templateId}
```

<a id="response-17"></a>

#### 응답

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
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

<a id="request-18"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.5/appKeys/{appKey}/statistics/view |

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수| 설명|
|---|---|---|---|
|from|	String|	O | 통계 조회 시작 날짜<br/> yyyy-mm-dd HH:mm|
|to|	String|	O | 통계 조회 종료 날짜<br/> yyyy-mm-dd HH:mm|
|searchType| String | O | 통계 구분<br/>DATE:날짜별, TIME:시간별, DAY:요일별 |
|mailTypes | String | X | 메일 타입<br/>NORMAL:일반, MASS:대량<br/>복수 입력 가능(mailTypes=NORMAL&mailTypes=MASS) |
|adYn | String | X | 광고 여부<br>Y:광고, N:광고 아님<br>입력하지 않으면 전체|
|templateId | String | X | 템플릿 ID |

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/statistics/view?from=2018-03-21+00%3A00&to=2018-03-23+00%3A00&searchType=DATE&mailTypes=NORMAL&adYn=Y&templateId=templateId1"
```

<a id="response-18"></a>

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

<a id="request-19"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
| GET |	/email/v1.5/appKeys/{appKey}/block-receivers |

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수| 설명|
|---|---|---|---|
|mailAddress|	String|	X| 수신 거부 목록에 등록되어 있는 이메일 주소|
|startBlockDate|	String|	X| 수신 거부 시작값 (yyyy-MM-dd HH:mm:ss)|
|endBlockDate|	String|	X| 수신 거부 종료값 (yyyy-MM-dd HH:mm:ss)|
|pageNum|	Integer|	X|	페이지 번호 1(기본값)|
|pageSize|	Integer|	X|	조회 건수 15(기본값)|

[예시]
```
curl -X GET -H "Content-Type: application/json;charset=UTF-8" "https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/block-receivers?mailAddress=customer1@nhn.com&pageNum=1&pageSize=10"
```

<a id="response-19"></a>

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
                "mailAddress": "block@nhn.com",
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

<a id="request-20"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
| POST |	/email/v1.5/appKeys/{appKey}/block-receivers |

[Request body]

|값|	타입|	필수 | 설명|
|---|---|---|---|
| blockReceiverList |  ㅣList | O | 수신 거부 리스트 |
| - mailAddress | String | O | 수신 거부 이메일 주소 |
| - blockDate | String | X | 수신 거부 날짜 (yyyy-MM-dd HH:mm:ss) |

[예시]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/block-receivers -d '{"blockReceiverList":[{"mailAddress":"customer1@nhn.com","blockDate":"2018-03-01 00:00:00"}]}'
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
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공 여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|

<a id="delete-rejections"></a>

### 수신 거부 삭제
<a id="request-21"></a>

#### 요청

[URL]

|Http method|	URI|
|---|---|
| PUT |	/email/v1.5/appKeys/{appKey}/block-receivers |

[Request body]

|값|	타입|	필수 | 설명|
|---|---|---|---|
| deleted | Boolean | O | 수신 거부 삭제를 명시하는 필드 |
| blockReceiverList |  ㅣList | O | 수신 거부 리스트 |
| - mailAddress | String | O | 수신 거부 이메일 주소 |

[예시]
```
curl -X PUT -H "Content-Type: application/json;charset=UTF-8" https://email.api.nhncloudservice.com/email/v1.5/appKeys/{appKey}/block-receivers -d '{"deleted":true,"blockReceiverList":[{"mailAddress":"customer1@nhn.com"}]}'
```

<a id="response-21"></a>

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
