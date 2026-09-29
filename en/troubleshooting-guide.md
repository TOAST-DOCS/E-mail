<!-- pre-align:aligned sig=afb63c3648e0 -->

<a id="notification-email-troubleshooting-guide"></a>
## Notification > Email > Troubleshooting Guide { #notification-email-troubleshooting-guide }

<a id="gmail-2024-2-1"></a>
### Gmail Email Sender Guideline Updates (Effective February 1, 2024) { #gmail-2024-2-1 }
Starting February 1, 2024, guidelines have been strengthened for email senders who send 5,000 or more emails per day to Gmail accounts.

<a id="gmail-2024-2-1-1"></a>
#### 대량메일 발신자 기준
- When calculating the mail sending limit (5,000), all mail sent from the same base domain is counted in aggregate.
- For example, if you send 2,500 emails from `solarmora.com` and 2,500 emails from `promotions.solarmora.com` to personal Gmail accounts every day, all 5,000 emails are considered to have been sent from the same primary domain (`solarmora.com`), so you are classified as a bulk sender.
- A sender who has met the above criteria at least once is permanently considered a bulk mail sender.

<a id="gmail-2024-2-1-2"></a>
#### 원클릭 수신 거부
If you send more than 5,000 emails per day, marketing emails and emails requiring consent to receive must support one-click unsubscribe.
NHN Cloud Email service provides a one-click unsubscribe feature for Gmail.

The added headers are as follows.
>List-Unsubscribe-Post: List-Unsubscribe=One-Click
>
>List-Unsubscribe: <https://solarmora.com/unsubscribe/example>

When a recipient unsubscribes using one-click unsubscribe, the following POST request is sent.
>"POST /unsubscribe/example HTTP/1.1
> Host: solarmora.com
> Content-Type: application/x-www-form-urlencoded
> Content-Length: 26
> List-Unsubscribe=One-Click"

The unsubscribe option is also available, but it does not replace one-click unsubscribe.


<a id="gmail-2024-2-1-3"></a>
#### 안정적인 메일 발송을 위한 필수 가이드라인
When using the NHN Cloud Email service, please consider the following.
- Add SPF, DKIM, and DMARC records to your sending domain and complete the authentication. If authentication fails, mail delivery to Gmail may be restricted.
- Do not include different types of content in the same email. For example, do not include promotions in a purchase receipt email.
- Don't send mail to users who have not agreed to receive mail. These recipients may mark your mail as spam, and future mail sent to these recipients may also be marked as spam.
- When sending mail, classify the sender by mail type. For example, send purchase receipt emails from a purchase receipt sender, and promotional emails from a promotional sender.

<a id="gmail-2024-2-1-4"></a>
#### 참조
- [Email Sender Guidelines](https://support.google.com/a/answer/81126?hl=en)
- [Email Sender Guidelines FAQ](https://support.google.com/a/answer/14229414?sjid=4363325810454271147-NC)

<a id="about-receiving-emails-in-gmail"></a>
### About Receiving Emails in Gmail { #about-receiving-emails-in-gmail }

If the recipient's mail is Gmail, you can't collect whether the mail was received. To check whether the mail was received, an image is planted, but Gmail changes this method by making the intermediate server stop tracking the image. This is an intentional block by Gmail, and it is technically impossible to collect information about email receipts.

``` 
Gmail Image Proxy

Because the Gmail Image Proxy service does not forward users' cookies, you can't use the measurement protocol to track Gmail users. The Gmail Image Proxy service prevents this by having the measurement protocol requests passed through an intermediate server. 
```

<a id="about-receiving-emails-in-gmail-reference"></a>
#### Reference
[Google Analytics > Email Tracking - Measurement Protocol](https://developers.google.com/analytics/devguides/collection/protocol/v1/email)

<a id="gmails-low-reputation-issue"></a>
### Gmail’s Low Reputation Issue { #gmails-low-reputation-issue }

A brief guide to Gmail reputation. 
Briefly describes how Gmail evaluates 'reputation' and how to raise and maintain 'reputation.' 
For more information, please check the reference document below.

<a id="gmails-low-reputation-issue-reputation"></a>
#### Reputation?
When you send an email, your Inbox Service Provider (ISP), Gmail, and the receiving SMTP server evaluate the reputation of the sending SMTP server to determine if it is accepted and to categorize as spam. If you send an email that doesn't match an ISP's reputation rating, it can `slow down your delivery and`make it `harder for recipients`who use that ISP to `receive your email`.
The reputations include IP reputation and domain reputation in general. 
* IP reputation: IP is the IP of the outgoing SMTP. This represents the reputation of IP that the outgoing SMTP has. 
* Domain reputation: Domain is the mail delivery domain. NHN's domain would be 'nhn.com' It refers to the reputation of a domain that is the subject of mail delivery.

<a id="gmails-low-reputation-issue-how-gmail-evaluates-your-reputation"></a>
#### How Gmail Evaluates Your Reputation
##### Evaluate Domain Reputation More Importantly.
Because multiple domains often share and send IPs, they value domains more than IPs. Value Engagement more than Complaints. Here, Complaints refers to the recipient's treatment of spam, for example. Engagement is when a recipient opens your mail, clicks on a link in the content, or unsubscribes from spam.
##### Evaluate Others.
Determine the reputation by evaluating various factors such as whether it is a personal mail or not, what the reputation of the sending IP is, whether or not there is a link in the mail and the content every day.

<a id="gmails-low-reputation-issue-how-to-improve-reputation"></a>
#### How to Improve Reputation
##### Warm-up Process Required
Sending a lot of emails from a new IP and domain from scratch is not advisable and can actually hurt your reputation. You must gradually increase over several days, like 50, 100, 500, 1000,...
##### Send to Recipients with Large Engagement
We need to prove that the email we send to Gmail is what the recipient wants. During warm-up, you can boost your reputation by sending to users who are much more loyal than your typical users, who are waiting for your email to arrive.
##### Provide Permissions
The service must provide the feature to set a receiving status.
##### Relevancy
To maintain your reputation, you need to send to an audience that is relevant to the content of your email. It's more important than just sending emails often. The size of destination is not important. You need to segment your mailing content and audience. 66% of unsubscribes are due to irrelevant emails and 55% to message fatigue.

<a id="gmails-low-reputation-issue-references"></a>
#### References
Gmail Inbox Delivery and Domain Reputation: What You Need to Know Now - [Mailgun](https://www.mailgun.com)
