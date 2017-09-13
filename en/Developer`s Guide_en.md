## Notification > Email > Developer's Guide

[API 도메인]

|환경|	도메인|
|---|---|
|Real|	https://api-mail.cloud.toast.com |

[Header]
```
Content-Type: application/json;charset=UTF-8
```

## 메일발송

### 일반 메일발송

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.0/appKeys/{appKey}/sender/mail|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request body]

|값|	타입|	필수|	설명|
|---|---|---|---|
|​senderAddress|	String|	O|	발신자 메일|
|senderName|	String|	X|	발신자 이름|
|requestDate|	String|	X|	발송 일자 미입력시 : 현재 시간으로 발송 (yyyy-MM-dd HH:mm:ss) |
|title|	String|	O|	제목|
|body|	String|	O|	내용|
|attachFileIdList|	List:String|	X|	업로드한 첨부파일 id|
|templateId|	String|	X|	발송 템플릿 아이디|
|templateParameter|	Object|	X|	치환 파라미터 (메일 제목/내용 치환시 입력)|
|- #key#|	String|	X|	치환 키 (##key##)|
|- #value#|	Object|	X|	치환 키에 매핑되는 Value값|
|receiverList|	List|	O|	수신자 리스트<br/> 최대 1000명까지 발송 가능(받는사람, 참조자 포함)|
|- receiveMailAddr|	String|	O|	수신자 메일주소|
|- receiveName|	String|	X|	수신자 명|
|- receiveType|	String|	O|	발신자타입 (MRT0: 받는사람 , MRT1 : 참조자)|
|userId|	String|	X|	요청자 아이디(페이코 UUID)|

[주의] : template을 사용 할 경우 title, body는 필수입력 제외 (입력 시 입력된 값이 template보다 우선 적용)

[Request body 예시]

```
{
    "senderAddress" : "support@nhnent.com",
    "senderName" : "발송자이름",
    "title" : "샘플 타이틀",
    "body" : "샘플 내용",
    "attachFileIdList": ["첨부파일_ID"],
    "receiverList" : [
        {
            "receiveMailAddr" : "customer1@nhnent.com",
            "receiveName" : "고객1",
            "receiveType" : "MRT0"
        },
        {
            "receiveMailAddr" : "customer2@nhnent.com",
            "receiveName" : "고객2",
            "receiveType" : "MRT1"
        }

    ],
    "userId" : "XXXXX"
}
```

#### 응답

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	Object|	데이터 영역|
|-- requestId|	String|	요청 아이디|
|-- statusCode|	String|	요청 상태 코드 (Y: 발송준비 , N : 발송준비실패)|

### 개별 메일발송

* 다중 수신자에 대해서 수신자 각각에게 메일을 발송하는 기능. 수신자는 수신인이 한명으로 보인다.

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.0/appKeys/{appKey}/sender/eachMail||

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request body]

|값|	타입|	필수|	설명|
|---|---|---|---|
|​senderAddress|	String|	O|	발신자 메일|
|senderName|	String|	X|	발신자 이름|
|requestDate|	String|	X|	발송 일자, 미입력시 : 현재 시간으로 발송 (yyyy-MM-dd HH:mm:ss)|
|title|	String|	O|	제목|
|body|	String|	O|	내용|
|attachFileIdList|	List:String|	X|	업로드한 첨부파일 id|
|templateId|	String|	X|	발송 템플릿 아이디|
|receiverList|	List|	O|	수신자 리스트<br/> 최대 1000명까지 발송 가능|
|- receiveMailAddr|	String|	O|	수신자 메일주소|
|- receiveName|	String|	X|	수신자 명|
|- templateParameter|	Object|	X|	치환 파라미터 (메일 제목/내용 치환시 입력)|
|-- #key#|	String|	X|	치환 키 (##key##)|
|-- #value#|	Object|	X|	치환 키에 매핑되는 Value값|
|userId|	String|	X|	요청자 아이디(페이코 UUID)|

[주의] : template을 사용 할 경우 title, body는 필수 제외 (입력 시 입력된 값이 template 보다 우선적용)

[Request body 예시]

```
{
    "senderAddress" : "support@nhnent.com",
    "senderName" : "발송자이름",
    "title" : "샘플 타이틀",
    "body" : "샘플 내용",
    "attachFileIdList": ["첨부파일_ID"],
    "receiverList" : [
        {
            "receiveMailAddr" : "customer1@nhnent.com",
            "receiveName" : "고객1"
        }

    ],
    "userId" : "XXXXX"
}
```

#### 응답

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	Object|	데이터 영역|
|-- requestId|	String|	요청 아이디|
|-- statusCode|	String|	요청 상태 코드 (Y: 발송준비 , N : 발송준비실패)|

### 광고성 일반 메일발송

* URL의 끝에만 ad-mail로 바뀌며 나머지는 일반 메일발송과 동일하다.

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.0/appKeys/{appKey}/sender/ad-mail|

### 광고성 개별 메일발송

* URL의 끝에만 ad-eachMail로 바뀌며 나머지는 개별 메일발송과 동일하다.

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.0/appKeys/{appKey}/sender/ad-eachMail |

### 첨부파일 업로드

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|	/email/v1.0/appKeys/{appKey}/attachfile/binaryUpload|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request body]

```
{
    "fileName": String,
    "createUser": String,
    "fileBody": Byte[]
}
```

|값|	타입|	필수|	설명|
|---|---|---|---|
|fileName|	String|	O|	파일이름|
|fileBody|	Byte[]|	O|	파일의 Byte[] 값|
|createUser|	String|	O|	파일 업로드 유저 정보|

#### 응답

```
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
```

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	Object|	데이터 영역|
|-- requestId|	String|	임시 request Id|
|-- fileName|	String|	파일명|
|-- filePath|	String|	첨부파일 기본Path <br/> (https://domain/attachFile/filePath/fileName)|


### 제목/본문 치환

#### 치환기능 메일작성 예시

```
* title : ##title_name## 님 안녕하세요 !!
* body : ##body_content## 발송 합니다.
```

#### 일반 메일요청 예시
```
{
    "senderAddress" : "support@nhnent.com",
    "templateId": "template1",
    "templateParameter" : {"title_name": "클라우드고객1", "body_content": "test1"},
    "receiverList" : [
        {
            "receiveMailAddr" : "customer1@nhnent.com",
            "receiveType" : "MRT0"
        }

    ],
    "userId" : "tester"
}
```

#### 개별 메일요청 예시
```
{
    "senderAddress" : "support@nhnent.com",
    "templateId": "template1",
    "receiverList" : [
        {
            "receiveMailAddr" : "customer1@nhnent.com",
            "templateParameter" : {"title_name": "클라우드고객1", "body_content": "test1"}
        },
        {
            "receiveMailAddr" : "customer2@nhnent.com",
            "templateParameter" : {"title_name": "클라우드고객2", "body_content": "test2"}
        }

    ],
    "userId" : "tester"
}
```

#### 결과 예시
```
* title : 클라우드고객1 님 안녕하세요!!
* body : test2 발송 합니다.
```

## 메일 조회

### 메일 발송리스트 조회

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.0/appKeys/{appKey}/sender/mails|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수|	설명|
|---|---|---|---|
|requestId|	String|	O|	요청 아이디|
|startSendDate|	String|	O|	발송 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endSendDate|	String|	X|	발송 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|startReceiveDate|	String|	X|	수신 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endReceiveDate|	String|	X|	수신 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|senderMail|	String|	X|	발신메일 주소|
|senderName|	String|	X|	발신자 이름|
|receiveMail|	String|	X|	수신메일 주소|
|templateId|	String|	X|	템플릿번호|
|sendStatus|	String|	X|	발송상태 코드 <br/> SST0:발송준비, SST1:발송중,  <br/> SST2:발송완료, SST3 : 발송실패|
|pageNum|	Integer|	X|	페이지 번호(Default : 1)|
|pageSize|	Integer|	X|	조회 건수(Default : 15)|

#### 응답

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- pageNum|	Integer|	현재 페이지 번호|
|-pageSize|	Integer|	조회된 데이터 건수|
|- totalCount|	Integer|	총 데이터 건수|
|- data|	List|	데이터 영역|
|-- requestId|	String|	요청 아이디|
|-- requestDate|	String|	발신일시|
|-- requestIp|	String|	발신서버 IP|
|-- resultDate|	String|	수신일시|
|-- templateId|	String|	템플릿아이디|
|-- templateName|	String|	템플릿명|
|-- masterStatusCode|	String|	메일 발송 준비 상태코드 ( “Y” : 발송준비 , “N” : 발송실패)|
|-- mailSeq|	String|	메일 순번|
|-- body|	String|	본문내용|
|-- title|	String|	메일 제목|
|-- senderMail|	String|	발신자 메일주소|
|-- senderName|	String|	발신자 이름|
|-- receiverMail|	String|	수신자 메일주소|
|-- receiveType|	String|	발신자타입 (MRT0: 받는사람 , MRT1 : 참조자)|
|-- resultId|	String|	발송결과 ID|
|-- resultDate|	String|	발송 완료 일시|
|-- mailStatusCode|	String|	발송상태 코드 <br/> SST0:발송준비, SST1:발송중,  <br/> SST2:발송완료, SST3 : 발송실패|
|-- mailStatusName|	String|	발송상태 코드 <br/> SST0:발송준비, SST1:발송중,  <br/> SST2:발송완료, SST3 : 발송실패|
|-- requestDate|	String|	발송 요청일시|

### 메일발송 상세조회

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.0/appKeys/{appKey}/sender/mail/{requestId}/{mailSeq}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|requestId|	String|	요청아이디|
|mailSeq|	Integer|	메일 순번|

#### 응답

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
            }]
        }
    }
}
```

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	Object|	데이터 영역|
|--mailStatusCode|	String|	발송상태|
|--mailStatusName|	String|	발송상태명|
|--templateId|	String|	템플릿ID|
|--templateName|	String|	템플릿 명|
|--senderName|	String|	발신자 이름|
|--senderMail|	String|	발신 메일주소|
|--title|	String|	메일 제목|
|--body|	String|	메일 내용|
|--attachFileYn|	String|	첨부파일 여부|
|--resultId|	String|	메일 발송 아이디비|
|--resultDate|	String|	발송완료일시|
|--receivers|	List|	수신자 리스트|
|---requestId|	String|	요청ID|
|---mailSequence|	Integer|	메일 순번|
|---receiveType|	String|	수신자 타입|
|---receiveTypeName|	String|	수신자 타입명|
|---receiveName|	String|	수신자 이름|
|---receiveMailAddr|	String|	수신 메일 주소|
|---readYn| String| 수신 여부 |
|---readDate| String| 수신 날짜(yyyy-MM-dd HH:mm:ss.SSS)|
|--attachFileList|	List|	첨부파일 리스트|
|---filePath|	String|	첨부파일 경로|
|---fileName|	String|	첨부파일 이름|
|---fileSize|	Integer|	첨부파일 크기|
|---fileSequence|	Integer|	첨부파일 순번|
|---createDate-|	String|	생성일시|
|--updateDate|	String|	수정일시|

## 템플릿 조회

### 템플릿 리스트 조회

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.0/appKeys/{appKey}/templates|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수|	설명|
|---|---|---|---|
|categoryId|	Integer|	X|	카테고리 아이디|
|useYn|	String|	X|	사용 여부(Y/N)|
|pageNum|	Integer|	X|	페이지 번호(Default : 1)|
|pageSize|	Integer|	X|	조회 건수(Default : 15)|
|all|	Boolean|	X|	전체 조회 여부|

#### 응답

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
```

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- pageNum|	Integer|	현재 페이지 번호|
|-pageSize|	Integer|	조회된 데이터 건수|
|- totalCount|	Integer|	총 데이터 건수|
|-data|	List|	데이터 영역|
|--templateId|	String|	템플릿 ID|
|--serviceId|	Integer|	서비스 ID|
|--categoryId|	Integer|	카테고리 ID|
|--categoryName|	String|	카테고리 명|
|--sort|	Integer|	순서|
|--templateName|	String|	템플릿 명|
|--templateDesc|	String|	템플릿 설명|
|-- useYn|	String|	사용여부|
|--sendMailAddress|	String|	템플릿 발신메일주소|
|--title|	String|	메일 제목|
|--body|	String|	메일 본문|
|--attachFileYn|	String|	템플릿 첨부파일 추가여부|
|--delYn|	String|	삭제 여부|
|--createDate|	String|	생성일시|
|--updateDate|	String|	수정일시|

### 템플릿 상세 조회

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.0/appKeys/{appKey}/templates/{templateId}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|templateId|	String|	템플릿 아이디|

#### 응답

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
```

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|-data|	List|	데이터 영역|
|--templateId|	String|	템플릿ID|
|--serviceId|	Integer|	서비스ID|
|--categoryId|	Integer|	카테고리ID|
|--categoryName|	String|	카테고리명|
|--templateName|	String|	템플릿명|
|--templateDesc|	String|	템플릿 설명|
|--useYn|	String|	사용여부 (Y= 사용중, N= 사용안함)|
|--sendMailAddress|	String|	발신메일주소|
|--title|	String|	메일 주소|
|--body|	String|	메일 내용|
|--delYn|	String|	삭제여부|
|--createDate|	String|	생성일시|
|--createUser|	String|	생성자|
|--updateDate|	String|	수정일시|
|--updateUser|	String|	수정자|
|--attachFileList|	List|	첨부파일 리스트|
|---fileId|	Integer|	첨부파일ID|
|---serviceId|	Integer|	서비스 ID|
|---templateId|	String|	템플릿 ID|
|---filePath|	String|	첨부파일 경로|
|---fileName|	String|	첨부파일명|
|---fileSize|	Integer|	첨부파일 크기|
|---createDate|	String|	생성일시|
|---createUser|	String|	생성자|
|---updateDate|	String|	수정일시|
|---updateUser|	String|	수정자|

## 통계 조회

### 일별 통계 조회
[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.0/appKeys/{appKey}/statistics/daily?from={from}&to={to}&type={type}&filters={filters}&filters={filters}

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수| 설명|
|---|---|---|---|
|from|	String|	O | 통계 조회 시작 날짜(년-월)<br/>(yyyy-MM) |
|to| String| O | 통계 조회 종료 날짜(년-월)<br/>(yyyy-MM) |
|type| String| X | 조회 타입<br/>* REQUEST:요청<br/>* SENT:발송<br/>* RECEIVED :수신<br/>* OPENER:오픈 |
|filters| String| X | 검색 조건(다중 파라미터 가능)<br/>* MAILTYPE:메일타입별(일반/대량) 통계<br/>* TEMPLATE:템플릿별 통계 |
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	List|	데이터 영역|
|-- ymd| String| 통계 날짜(yyyyMMdd)|
|-- appKey| String| appKey|
|-- type| String| 통계 조회 타입<br/>REQUEST:요청<br/>SENT:발송<br/>RECEIVED:수신<br/>OPENER:오픈 |
|-- mailType| String| 메일 타입<br/>* MAILTYPE:메일타입별(일반/대량) 통계<br/>* TEMPLATE:템플릿별 통계|
|-- templateId| String| 템플릿아이디|
|-- templateName| String| 템플릿명|
|-- tm00| int| 00:00:00 ~ 00:59:59|
|-- tm01| int| 01:00:00 ~ 01:59:59|
|-- tm02| int| 02:00:00 ~ 02:59:59|
|-- tm03| int| 03:00:00 ~ 03:59:59|
|-- tm04| int| 04:00:00 ~ 04:59:59|
|-- tm05| int| 05:00:00 ~ 05:59:59|
|-- tm06| int| 06:00:00 ~ 06:59:59|
|-- tm07| int| 07:00:00 ~ 07:59:59|
|-- tm08| int| 08:00:00 ~ 08:59:59|
|-- tm09| int| 09:00:00 ~ 09:59:59|
|-- tm10| int| 10:00:00 ~ 10:59:59|
|-- tm11| int| 11:00:00 ~ 11:59:59|
|-- tm12| int| 12:00:00 ~ 12:59:59|
|-- tm13| int| 13:00:00 ~ 13:59:59|
|-- tm14| int| 14:00:00 ~ 14:59:59|
|-- tm15| int| 15:00:00 ~ 15:59:59|
|-- tm16| int| 16:00:00 ~ 16:59:59|
|-- tm17| int| 17:00:00 ~ 17:59:59|
|-- tm18| int| 18:00:00 ~ 18:59:59|
|-- tm19| int| 19:00:00 ~ 19:59:59|
|-- tm20| int| 20:00:00 ~ 20:59:59|
|-- tm21| int| 21:00:00 ~ 21:59:59|
|-- tm22| int| 22:00:00 ~ 22:59:59|
|-- tm23| int| 23:00:00 ~ 23:59:59|
|-- sum| int| 합계|

### 월별 통계 조회

[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.0/appKeys/{appKey}/statistics/monthly?from={from}&to={to}&type={type}&filters={filters}&filters={filters} |

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수| 설명|
|---|---|---|---|
|from|	String|	O | 통계 조회 시작 날짜(년-월)<br/>(yyyy-MM) |
|to| String| O | 통계 조회 종료 날짜(년-월)<br/>(yyyy-MM) |
|type| String| X | 조회 타입<br/>REQUEST:요청<br/>SENT:발송<br/>RECEIVED :수신<br/>OPENER:오픈 |
|filters| String| X | 검색 조건(다중 파라미터 가능)<br/>* MAILTYPE:메일타입별(일반/대량) 통계<br/>* TEMPLATE:템플릿별 통계 |

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	List|	데이터 영역|
|-- ymd| String| 통계 날짜(yyyyMMdd)|
|-- appKey| String| appKey|
|-- type| String| 통계 조회 타입<br/>* REQUEST:요청<br/>* SENT:발송<br/>* RECEIVED :수신<br/>* OPENER:오픈 |
|-- mailType| String| 메일 타입<br/>* MAILTYPE:메일타입별(일반/대량) 통계<br/>* TEMPLATE:템플릿별 통계|
|-- templateId| String| 템플릿아이디|
|-- templateName| String| 템플릿명|
|-- tm00| int| 00:00:00 ~ 00:59:59|
|-- tm01| int| 01:00:00 ~ 01:59:59|
|-- tm02| int| 02:00:00 ~ 02:59:59|
|-- tm03| int| 03:00:00 ~ 03:59:59|
|-- tm04| int| 04:00:00 ~ 04:59:59|
|-- tm05| int| 05:00:00 ~ 05:59:59|
|-- tm06| int| 06:00:00 ~ 06:59:59|
|-- tm07| int| 07:00:00 ~ 07:59:59|
|-- tm08| int| 08:00:00 ~ 08:59:59|
|-- tm09| int| 09:00:00 ~ 09:59:59|
|-- tm10| int| 10:00:00 ~ 10:59:59|
|-- tm11| int| 11:00:00 ~ 11:59:59|
|-- tm12| int| 12:00:00 ~ 12:59:59|
|-- tm13| int| 13:00:00 ~ 13:59:59|
|-- tm14| int| 14:00:00 ~ 14:59:59|
|-- tm15| int| 15:00:00 ~ 15:59:59|
|-- tm16| int| 16:00:00 ~ 16:59:59|
|-- tm17| int| 17:00:00 ~ 17:59:59|
|-- tm18| int| 18:00:00 ~ 18:59:59|
|-- tm19| int| 19:00:00 ~ 19:59:59|
|-- tm20| int| 20:00:00 ~ 20:59:59|
|-- tm21| int| 21:00:00 ~ 21:59:59|
|-- tm22| int| 22:00:00 ~ 22:59:59|
|-- tm23| int| 23:00:00 ~ 23:59:59|
|-- sum| int| 합계|

### 통합 통계 조회
[URL]

|Http method|	URI|
|---|---|
|GET|	/email/v1.0/appKeys/{appKey}/statistics/view?from={from}&to={to}&searchType={searchType}&mailTypes={mailTypes}&templateId={templateId} |

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
|templateId | String | X | 템플릿 아이디 |

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	List|	데이터 영역|
|-- divisionName | String | 통계 기준(날짜/시간/요일) |
|-- requestedCount | Long | 발송 요청 카운트 |
|-- sentCount | Long | 발송 카운트 |
|-- receivedCount | Long | 수신 카운트 |
|-- openedCount | Long | 오픈 카운트 |
|-- sentRate | String | 발송율 |
|-- receivedRate | String | 수신율 |
|-- openedRate | String | 오픈율 |

## 수신 거부 관리

### 수신 거부 조회

#### 요청

|Http method|	URI|
|---|---|
| GET |	/appKeys/{appKey}/block-receivers?mailAddress={mailAddress}&pageNum={pageNum}&pageSize={pageSize} |

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수| 설명|
|---|---|---|---|
|mailAddress|	String|	X| 수신거부 이메일 주소 필터|
|pageNum|	Integer|	X|	페이지 번호(Default : 1)|
|pageSize|	Integer|	X|	조회 건수(Default : 15)|

#### 응답
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- pageNum|	Integer|	현재 페이지 번호|
|-pageSize|	Integer|	조회된 데이터 건수|
|- totalCount|	Integer|	총 데이터 건수|
|- data|	List|	데이터 영역|
|-- mailAddress | String | 수신거부 이메일 주소 |
|-- blockDate | String | 수신거부 날짜 (yyyy-MM-dd HH:mm:ss.S)

### 수신 거부 등록

#### 요청

[URL]

|Http method|	URI|
|---|---|
| POST |	/appKeys/{appKey}/block-receivers |

[Request body]
```json
{
  "blockReceiveList" :
  [
  {
    "mailAddress" :  String,
    "blockDate" :  String
  }
  ]
}
```

|값|	타입|	필수 | 설명|
|---|---|---|---|
| blockReceiverList |  ㅣList | O | 수신거부 리스트 |
| - mailAddress | String | O | 수신거부 이메일 주소 |
| - blockDate | String | X | 수신 거부 날짜 (yyyy-MM-dd HH:mm:ss) |

#### 응답
```
{
  "header": {
    "isSuccessful":  Bollean,
    "resultCode": Integer,
    "resultMessage": String
  }
}
```
|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|

### 수신 거부 삭제
#### 요청

[URL]

|Http method|	URI|
|---|---|
| PUT |	/appKeys/{appKey}/block-receivers |

[Request body]
```json
{
  "deleted":Boolean,
  "blockReceiverList" :
  [
  {
    "mailAddress" :  String
  }
  ]
}
```

|값|	타입|	필수 | 설명|
|---|---|---|---|
| deleted | Boolean | O | 수신 거부 삭제를 명시하는 필드 |
| blockReceiverList |  ㅣList | O | 수신거부 리스트 |
| - mailAddress | String | O | 수신거부 이메일 주소 |

#### 응답
```
{
  "header": {
    "isSuccessful":  Bollean,
    "resultCode": Integer,
    "resultMessage": String
  }
}
```
|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
