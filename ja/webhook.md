## Notification > Email > Webフック

Emailサービス内で特定イベントが発生した時、Webフック設定に定義されたURLへPOSTリクエストを作成します。<br>
作成されたPOSTリクエストについてのAPI文書です。

### Webフック送信

[URL]

| Http method | 	URI              |
|-------------|-------------------|
| POST        | Webフック設定に定義した対象URL |

[Header]

| 値                       | 	タイプ   | 	説明          |
|---------------------------|---------|----------------|
| X-Toast-Webフック-Signature | 	String | Webフック設定時に入力した署名 |

[Request body]

| 値             | 	タイプ        | 	説明                                                                                 |
|-----------------|--------------|---------------------------------------------------------------------------------------|
| hooksId         | 	String      | Webフック設定に定義されたURLでPOSTリクエストを行うたびに一意に作成されるID                                           |
| webhookConfigId | 	String      | Webフック設定ID                                                                              |
| productName     | 	String      | 	Webフックイベントが発生したサービス名                                                                   |
| appKey          | 	String      | Webフックイベントが発生したサービスアプリキー                                                                  |
| event           | 	String      | Webフックイベント名<br>* UNSUBSCRIBE:広告メール受信アドレス登録                                            |
| hooks           | 	List\<Map\> | Webフックイベント発生時のデータ<br>* 詳細な内容は[イベントタイプ別フック(hook)定義](./api-guide/#event-hooks)を参照してください。 |

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

### イベントタイプ別hooks定義

Webフック設定に定義されたURLへPOSTリクエストを作成する時のイベントタイプ別フック(hook)データです。

#### 広告メール受信アドレス登録

| 値               | 	タイプ        | 	説明                                         |
|-------------------|--------------|-----------------------------------------------|
| hooks             | 	List\<Map\> | Webフックイベント発生時のデータ                             |
| - hookId          | 	String      | サービスでイベントが発生した時に作成される固有ID                   |
| - receiveMailAddr | 	String      | 	受信拒否をリクエストした受信者メールアドレス                       |
| - createdDateTime | 	String      | 受信拒否リクエスト日時<br>* yyyy-MM-dd'T'HH:mm:ss.SSSXXX |

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

#### メッセージの送信結果の更新

'- 受信日時、ステータスコード、ステータスメッセージは送信完了(SST2)状態の時のみ提供されます。

| 値                 | 	タイプ        | 	説明                                                                                                                                                 |
|---------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| hooks               | 	List\<Map\> | Webフックイベント発生時のデータ                                                                                                                                     |
| - messageType       | 	String      | メールタイプ<br>NORMAL_MAIL<br>NORMAL_MAIL_AD<br>NORMAL_MAIL_AUTH<br>MASS_MAIL<br>MASS_MAIL_AD<br>MASS_MAIL_AUTH<br>TAG_MAIL<br>TAG_MAIL_AD<br>TAG_MAIL_AUTH |
| - requestId         | 	String      | リクエストID                                                                                                                                                 |
| - mailSeq           | 	Integer     | メールの順番                                                                                                                                                |
| - senderAddress     | 	String      | 発信者メールアドレス                                                                                                                                           |
| - receiveMailAddr   | 	String      | 受信者メールアドレス                                                                                                                                           |
| - mailStatusCode    | 	String      | 送信ステータスコード <br/> SST2:送信完了、 SST3:送信失敗、 <br/> SST5:受信拒否、 SST7:未認証、 SST8:ホワイトリストによる失敗                                                          |
| - requestDate       | 	String      | リクエスト日時                                                                                                                                               |
| - createDate        | 	String      | 作成日時                                                                                                                                               |
| - resultDate        | 	String      | 受信日時                                                                                                                                               |
| - dsnCode           | 	String      | DSN(Delivery Status Notification)ステータスコード                                                                                                             |
| - dsnMessage        | 	String      | DSN(Delivery Status Notification)ステータスメッセージ                                                                                                            |
| - senderGroupingKey | 	String      | 発信者グループキー                                                                                                                                            |
| - _links            | 	Object      | 	リンク                                                                                                                                                 |
| - self              | 	Object      | 	-                                                                                                                                                    |
| - href              | 	String      | 	メッセージ照会APIリンク                                                                                                                                      |
| - hookId            | 	String      | サービスでイベントが発生した時に作成される固有のID                                                                                                                           |


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
