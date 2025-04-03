## Notification > Email > Webhook Guide
When a specific event occurs in the Email service, create POST request with the URL specified by the webhook settings.<br>
The API documentation for the generated POST request.

### Send Webhook

[URL]

|Http method|	URI|
|---|---|
| POST | The destination URL defined in the webhook settings |

[Header]

|Value|	Type|	Descriptions|
|---|---|---|
|X-Toast-Webhook-Signature|	String| The signature entered when webhook is configured |

[Request body]

|Value|	Type|	Descriptions|
|---|---|---|
|hooksId|	String| A unique ID created every time a POST request is sent to the URL specified by webhook settings |
|webhookConfigId|	String|Webhook setup ID|
|productName|	String|	The name of the service where a webhook event occurred |
|appKey|	String| The service appkey where the webhook event occurred |
|event|	String| Webhook event name<br>* UNSUBSCRIBE: Register an address to receive advertising emails |
|hooks|	List\<Map\> | Data when a webhook event occurs<br>\* For more information, see  [Hook definitions by event type](./api-guide/#event-hooks). |

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

### Hook definitions by event type
Hook data per event type when generating a POST request to the URL defined in the webhook settings.
#### Register an address to receive advertising mail
|Value|	Type|	Descriptions|
|---|---|---|
|hooks|	List\<Map\> | Data when a webhook event occurs |
|- hookId|	String| A unique ID created when an event occurs in a service |
|- receiveMailAddr|	String|	The email address of the recipient who requested the unsubscribe |
|- createdDateTime|	String| Date of unsubscribe request<br>\* yyyy-MM-dd'T'HH:mm:ss.SSSXXX|

```json
{
  "hooks": [
    {
      "hookId": "202007271010101010sadasdavas",
      "receiveMailAddr": "help@toast.com",
      "createdDateTime": "2020-09-09T11:25:10.000+09:00"
    }
  ]
}
```

#### Update the message sending result code

- 수신 일시, 상태 코드, 상태 메세지는 발송 완료(SST2) 상태일 때만 제공됩니다.

|Value|	Type|	Descriptions|
|---|---|---|
|hooks|	List\<Map\> | Data when a webhook event occurs |
|- messageType|	String| Mail type<br>NORMAL_MAIL<br>NORMAL_MAIL_AD<br>NORMAL_MAIL_AUTH<br>MASS_MAIL<br>MASS_MAIL_AD<br>MASS_MAIL_AUTH<br>TAG_MAIL<br>TAG_MAIL_AD<br>TAG_MAIL_AUTH  |
|- requestId|	String| Request ID |
|- mailSeq|	Maximum number of unavailable nodes. Minimum: 1, Maximum: Current number of nodes in the worker node group, Default: 1)| Mail order |
|- senderAddress|	String| Sender email address |
|- receiveMailAddr|	String| Recipient email address |
|- mailStatusCode|	String| Delivery status code <br/> SST2: 발송 완료, SST3: 발송 실패, <br/> SST5: 수신 거부, SST7: 미인증, SST8: 화이트리스트로 인한 실패        |
|- requestDate|	String| Date and time of request |
|- createDate|	String| Date and time of creation |
|- resultDate|	String| Date and time of receiving |
|- dsnCode|	String| Delivery Status Notification (DSN) status code |
|- dsnMessage|	String| Delivery Status Notification (DSN) status message |
|- senderGroupingKey |	String| 발신자 그룹키 |
|- _links|	Object|	Link |
|- self|	Object|	- |
|- href|	String|	Query Message API link |
|- hookId|	String| A unique ID created when an event occurs in a service |

```json
{
  "hooks": [
    {
      "messageType": "NORMAL_MAIL",
      "requestId": "20190101000000ABCDEFG0",
      "mailSeq": "0",
      "senderAddress": "sender@nhncloud.com",
      "receiveMailAddr": "receiver@nhncloud.com",
      "mailStatusCode": "SST2",
      "requestDate": "2020-09-09T11:25:10.000+09:00",
      "createDate": "2020-09-09T11:25:10.000+09:00",
      "resultDate": "2020-09-09T11:25:10.000+09:00",
      "dsnCode": "2.5.0",
      "dsnMessage": "SUCCESS",
      "senderGroupKey": "groupKey",
      "_links": {
        "self": {
          "href": "https://email.api.nhncloudservice.com/email/v2.0/appKeys/hVYsda0xPcasTT5hC6z/sender/mail/20190101000000ABCDEFG0/0"
        }
      },
      "hookId": "202007271010101010sadasdavas"
    }
  ]
}
```
