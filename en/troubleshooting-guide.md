## Notification > Email > Troubleshooting Guide

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
