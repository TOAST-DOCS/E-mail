## Notification > Email > Troubleshooting Guide

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
- [이메일 발신자 가이드라인](https://support.google.com/a/answer/81126?hl=en)
- [이메일 발신자 가이드라인 FAQ](https://support.google.com/a/answer/14229414?sjid=4363325810454271147-NC)

### About Receiving Emails in Gmail

If the recipient's mail is Gmail, you can't collect whether the mail was received. To check whether the mail was received, an image is planted, but Gmail changes this method by making the intermediate server stop tracking the image. This is an intentional block by Gmail, and it is technically impossible to collect information about email receipts.

``` 
Gmail Image Proxy

Because the Gmail Image Proxy service does not forward users' cookies, you can't use the measurement protocol to track Gmail users. The Gmail Image Proxy service prevents this by having the measurement protocol requests passed through an intermediate server. 
```

#### Reference
[Google Analytics > Email Tracking - Measurement Protocol](https://developers.google.com/analytics/devguides/collection/protocol/v1/email)

### Gmail’s Low Reputation Issue

A brief guide to Gmail reputation. 
Briefly describes how Gmail evaluates 'reputation' and how to raise and maintain 'reputation.' 
For more information, please check the reference document below.

#### Reputation?
When you send an email, your Inbox Service Provider (ISP), Gmail, and the receiving SMTP server evaluate the reputation of the sending SMTP server to determine if it is accepted and to categorize as spam. If you send an email that doesn't match an ISP's reputation rating, it can `slow down your delivery and`make it `harder for recipients`who use that ISP to `receive your email`.
The reputations include IP reputation and domain reputation in general. 
* IP reputation: IP is the IP of the outgoing SMTP. This represents the reputation of IP that the outgoing SMTP has. 
* Domain reputation: Domain is the mail delivery domain. NHN's domain would be 'nhn.com' It refers to the reputation of a domain that is the subject of mail delivery.

#### How Gmail Evaluates Your Reputation
##### Evaluate Domain Reputation More Importantly.
Because multiple domains often share and send IPs, they value domains more than IPs. Value Engagement more than Complaints. Here, Complaints refers to the recipient's treatment of spam, for example. Engagement is when a recipient opens your mail, clicks on a link in the content, or unsubscribes from spam.
##### Evaluate Others.
Determine the reputation by evaluating various factors such as whether it is a personal mail or not, what the reputation of the sending IP is, whether or not there is a link in the mail and the content every day.

#### How to Improve Reputation
##### Warm-up Process Required
Sending a lot of emails from a new IP and domain from scratch is not advisable and can actually hurt your reputation. You must gradually increase over several days, like 50, 100, 500, 1000,...
##### Send to Recipients with Large Engagement
We need to prove that the email we send to Gmail is what the recipient wants. During warm-up, you can boost your reputation by sending to users who are much more loyal than your typical users, who are waiting for your email to arrive.
##### Provide Permissions
The service must provide the feature to set a receiving status.
##### Relevancy
To maintain your reputation, you need to send to an audience that is relevant to the content of your email. It's more important than just sending emails often. The size of destination is not important. You need to segment your mailing content and audience. 66% of unsubscribes are due to irrelevant emails and 55% to message fatigue.

#### References
Gmail Inbox Delivery and Domain Reputation: What You Need to Know Now - [Mailgun](https://www.mailgun.com)
