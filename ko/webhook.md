## Notification > Email > 웹훅 가이드
Email 서비스 내 특정 이벤트가 발생하면 웹훅 설정에 정의된 URL로 POST 요청을 생성합니다.<br>
생성된 POST 요청에 대한 API 문서입니다.

### 웹훅 발송

[URL]

|Http method|	URI|
|---|---|
| POST | 웹훅 설정에 정의한 대상 URL |

[Header]

|값|	타입|	설명|
|---|---|---|
|X-Toast-Webhook-Signature|	String| 웹훅 설정 시 입력한 서명 |

[Request body]

|값|	타입|	설명|
|---|---|---|
|hooksId|	String| 웹훅 설정에 정의된 URL로 POST 요청을 할 때마다 고유하게 생성되는 ID |
|webhookConfigId|	String|웹훅 설정 ID|
|productName|	String|	웹훅 이벤트가 발생한 서비스명 |
|appKey|	String| 웹훅 이벤트가 발생한 서비스 앱키 |
|event|	String| 웹훅 이벤트명<br>* UNSUBSCRIBE: 광고 메일 수신 주소 등록 |
|hooks|	List\<Map\> | 웹훅 이벤트 발생 시 데이터<br>* 상세한 내용은 [이벤트 유형별 훅(hook) 정의](./api-guide/#event-hooks)를 참고해 주세요. |

#### cURL
```
curl -X POST \
    '{TargetUrl}' \
    -H 'Content-Type: application/json;charset=UTF-8' \
    -H 'X-Toast-Webhook-Signature: application/json;charset=UTF-8' \
    -H 'X-Secret-Key: '"${SECRET_KEY}"'' \
    -d '{
        "hooksId":"202007271010101010sadasdavas",
        "webhookConfigId":"String",
        "productName":"Email",
        "appKey":"akb3dukdmdjsdSvgk",
        "event":"UNSUBSCRIBE",
        "hooks":[
            {
                ...
            }
        ]
    }'
```

<span id="event-hooks"></span>

### 이벤트 유형별 hooks 정의
웹훅 설정에 정의된 URL로 POST 요청을 생성할 때 이벤트 타입별 훅(hook) 데이터입니다.
#### 광고 메일 수신 주소 등록
|값|	타입|	설명|
|---|---|---|
|hooks|	List\<Map\> | 웹훅 이벤트 발생 시 데이터 |
|- hookId|	String| 서비스에서 이벤트가 발생할 때 생성되는 고유 ID |
|- receiveMailAddr|	String|	수신 거부를 요청한 수신자 메일 주소 |
|- createdDateTime|	String| 수신 거부 요청 일시<br>* yyyy-MM-dd'T'HH:mm:ss.SSSXXX|

```json
"hooks":[
    {
        "hookId":"202007271010101010sadasdavas",
        "receiveMailAddr":"help@toast.com",
        "createdDateTime":"2020-09-09T11:25:10.000+09:00"
    }
]
```

#### 메시지 발송 결과 코드 업데이트
|값|	타입|	설명|
|---|---|---|
|hooks|	List\<Map\> | 웹훅 이벤트 발생 시 데이터 |
|- messageType|	String| 메일 타입<br>NORMAL_MAIL<br>NORMAL_MAIL_AD<br>NORMAL_MAIL_AUTH<br>MASS_MAIL<br>MASS_MAIL_AD<br>MASS_MAIL_AUTH<br>TAG_MAIL<br>TAG_MAIL_AD<br>TAG_MAIL_AUTH  |
|- requestId|	String| 요청 ID |
|- mailSeq|	Integer| 메일 순번 |
|- senderName|	String| 발신자 이름 |
|- senderAddress|	String| 발신자 메일 주소 |
|- receiveName|	String| 수신자 이름 |
|- receiveMailAddr|	String| 수신자 메일 주소 |
|- mailStatusCode|	String| 발송 상태 코드 <br/> SST0: 발송 준비, SST1: 발송 중, <br/> SST2: 발송 완료, SST3: 발송 실패, SST7: 미인증|
|- requestDate|	String| 요청 일시 |
|- createDate|	String| 생성 일시 |
|- resultDate|	String| 수신 일시 |
|- dsnCode|	String| DSN(Delivery Status Notification) 상태 코드 |
|- dsnMessage|	String| DSN(Delivery Status Notification) 상태 메시지 |
| - senderGroupingKey |	String| 발신자 그룹 키 |
|- _links|	Object|	링크 |
|- self|	Object|	- |
|- href|	String|	메시지 조회 API 링크 |
|- hookId|	String| 서비스에서 이벤트가 발생할 때 생성되는 고유 ID |

```json
"hooks":[
    {
    "messageType":"NORMAL_MAIL",
    "requestId":"20190101000000ABCDEFG0",
    "mailSeq":"0",
    "senderName":"NHN Cloud",
    "senderAddress":"sender@nhncloud.com",
    "receiveName":"NHN Cloud",
    "receiveMailAddr":"receiver@nhncloud.com",
    "mailStatusCode":"SST2",
    "requestDate":"2020-09-09T11:25:10.000+09:00",
    "createDate":"2020-09-09T11:25:10.000+09:00",
    "resultDate":"2020-09-09T11:25:10.000+09:00",
    "dsnCode":"2.5.0",
    "dsnMessage":"SUCCESS",
    "senderGroupKey":"groupKey",     
    "_links":{
        "self":{
            "href":"https://email.api.nhncloudservice.com/email/v2.0/appKeys/hVYsda0xPcasTT5hC6z/sender/mail/20190101000000ABCDEFG0/0"
        }
    },
    "hookId": "202007271010101010sadasdavas"
    }
]
```
