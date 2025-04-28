## Notification > Email > 문제 해결 가이드

### Gmail 이메일 발신자 가이드라인 강화 (2024년 2월 1일 시행)
2024년 2월 1일부터 Gmail 계정으로 하루에 5,000개 이상의 메일을 보내는 이메일 발신자에 대한 가이드라인이 강화되었습니다.

#### 대량메일 발신자 기준
- 메일 전송 한도(5,000개)를 계산할 때는 동일한 기본 도메인에서 전송된 모든 메일 수를 집계합니다.
- 예를 들어 매일 개인 Gmail 계정으로 `solarmora.com`에서 2,500개의 메일을 보내고, `promotions.solarmora.com`에서 2,500개의 메일을 보내는 경우, 5,000개의 메일이 모두 동일한 기본 도메인(`solarmora.com`)에서 전송되었으므로 대량메일 발신자로 간주됩니다.
- 위의 기준을 한 번 이상 충족한 발신자는 영구적으로 대량메일 발신자로 간주됩니다.

#### 원클릭 수신 거부
하루에 5,000개가 넘는 메일을 보내는 경우 마케팅 및 수신 동의 메일은 원클릭 수신 거부를 지원해야 합니다.
NHN Cloud Email 서비스는 Gmail 메일에 원클릭 수신 거부 기능을 제공합니다.

추가되는 헤더는 다음과 같습니다. 
>List-Unsubscribe-Post: List-Unsubscribe=One-Click
> 
>List-Unsubscribe: <https://solarmora.com/unsubscribe/example>

사용자가 수신자가 원클릭 수신 거부를 사용하여 수신을 거부하면 다음과 같은 POST 요청이 전송됩니다.
>"POST /unsubscribe/example HTTP/1.1
> Host: solarmora.com
> Content-Type: application/x-www-form-urlencoded
> Content-Length: 26
> List-Unsubscribe=One-Click"

수신 거부 옵션도 사용할 수 있지만 원클릭 수신 거부를 대체하지는 않습니다.


#### 안정적인 메일 발송을 위한 필수 가이드라인
NHN Cloud Email 서비스를 사용하는 경우 다음 사항을 고려해 주시기 바랍니다.
- 발신 도메인에 SPF, DKIM, DMARC 레코드를 추가하고 인증을 완료하세요. 인증에 실패할 경우 Gmail로의 메일 발송이 제한될 수 있습니다.
- 같은 메일에 다른 유형의 콘텐츠를 함께 포함하지 마세요. 예를 들어 구매 영수증 메일에 프로모션을 포함하지 마세요.
- 메일 수신에 동의하지 않은 사용자에게 메일을 보내지 마세요. 이러한 수신자는 내 메일을 스팸으로 표시할 수 있으며, 향후 이러한 수신자에게 보내는 메일도 스팸으로 표시됩니다.
- 발송 시 발신자를 메일 유형별로 분류하여 발송하세요. 예를 들어, 구매 영수증 메일은 구매 영수증 발신자로, 프로모션 메일은 프로모션 발신자로 발송하세요.

#### 참조
- [이메일 발신자 가이드라인](https://support.google.com/a/answer/81126?hl=ko)
- [이메일 발신자 가이드라인 FAQ](https://support.google.com/a/answer/14229414?sjid=4363325810454271147-NC)


### Gmail 수신 확인 관련

수신자의 메일이 Gmail인 경우 메일의 수신 확인 여부를 수집할 수 없습니다. 수신 확인을 위해 이미지를 심어 놓는 방식을 사용하는데, Gmail에서는 중간 서버가 해당 이미지를 추적할 수 없도록 바꾸게 됩니다. 이는 Gmail에서 의도적으로 막아 놓은 것으로 기술적으로 수신 확인에 대한 정보를 수집하는 것은 불가능합니다.

```
Gmail Image Proxy

Because the Gmail Image Proxy service does not forward users' cookies, you can't use the measurement protocol to track Gmail users. The Gmail Image Proxy service prevents this by having the measurement protocol requests passed through an intermediate server.
```

#### 참조
[Google Analytics > Email Tracking - Measurement Protocol](https://developers.google.com/analytics/devguides/collection/protocol/v1/email)

### Gmail 낮은 평판(low reputation) 문제

Gmail 평판(reputation)에 관한 간략한 가이드입니다.
Gmail에서 어떻게 '평판'을 평가하는지, '평판'을 올리고 유지하는 방법은 무엇인지 짧게 설명합니다.
더 자세한 내용은 문서 아래의 참고 문서를 확인해 주시기 바랍니다.

#### 평판(reputation)?
메일 발송 시 ISP(Inbox Service Provider, Gmail, 수신 SMPT 서버)는 발송 SMTP 서버의 평판을 평가해 수신 여부를 정하고, 스팸을 분류합니다. ISP의 평판 평가 방식에 맞지 않게 메일을 발송하면, `발송 속도 저하`와 해당 ISP를 사용하는 `수신인의 메일 수신이 어려워`질 수 있습니다.
평판은 크게 IP 평판, 도메인 평판이 있습니다.
* IP 평판:  IP는 발송 SMTP의 IP입니다. 발송 SMTP가 가지는 IP의 평판을 의미합니다.
* 도메인 평판: 도메인은 메일 발송 도메인입니다. NHN의 도메인은 'nhn.com'이 됩니다. 메일 발송의 주체가 되는 도메인의 평판을 의미합니다.

#### Gmail의 평판 평가 방식
##### 도메인 평판을 더 중요하게 평가한다.
여러 도메인이 IP를 공유해서 발송하는 경우가 많기 때문에 IP보다 도메인을 더 중요하게 평가합니다. Complaints(불만)보다 Engagement(참여) 더 중요하게 평가합니다. 여기서 Complaints는 수신인의 스팸 처리 등을 뜻합니다. Engagement는 수신인이 메일 내용 확인 및 내용에 있는 링크 클릭, 스팸 처리 해제 등을 뜻합니다.
##### 그 밖에 여러 가지를 평가한다.
개인적인 메일인지 아닌지, 발송 IP의 평판은 어떤지, 메일 내 링크가 들어가 있는가, 매일 내용 등 여러 가지 요소들을 평가하여 평판을 결정합니다.

#### 평판을 올리는 방법
##### 준비(warm-Up) 과정 필요
처음부터 새로운 IP와 도메인으로 많은 양의 메일을 발송하는 것은 바람직하지 않으며, 오히려 평판이 낮아질 수 있습니다. 며칠에 걸쳐서 50, 100, 500, 1000, ...처럼 서서히 증가시켜야 합니다.
##### Engagement가 큰 수신인을 대상으로 발송
Gmail에 우리가 발송하는 메일은 수신인들이 원하는 것이라는 것을 증명해야 합니다. Warm-Up 시 일반적인 사용자들보다 훨씬 충성도 높은, 메일이 오기만을 기다리고 있는 사용자들에게 발송하면 평판을 높일 수 있습니다.
##### Permissions 제공
서비스에서 수신 여부 설정을 할 수 있도록 기능을 제공해야 합니다.
##### Relevancy
평판을 유지하기 위해서는 메일 내용과 관련성이 있는 대상에게 발송해야 합니다. 단지 메일을 자주 보내는 것보다 더 중요합니다. 발송 대상의 크기는 중요하지 않습니다. 메일 내용과 대상을 세분화해서 발송해야 합니다. 구독 취소의 원인 66%는 관련성이 없는 메일, 55%는 메시지의 피로도 때문입니다.

#### 참고
Domain Reputation Or IP Reputation: Which One Does Gmail Care About More? - [Mailgun](https://www.mailgun.com)
