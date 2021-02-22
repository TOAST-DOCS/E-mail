## Notification > Email > Troubleshooting Guide

### Register Sender Policy Framework (SPF)
As mails sent  by NHN Cloud Email are often considered spams, we provide the guide as below to register SPF.   

#### Circumstance and Cause 

* Circumstance: Sent mails are not delivered to recipient's mail address but returned as spams. 
* Cause: SPF not registered 

#### What is SPF?

|Term| Description |
|---|---|
|SPF Record|As part of a DNS (Domain Name System) record, it validates mail servers which can send mails from a domain.|

* Create an SPF record for the domain of email addresses for **Mails to Send**.
* If SPF record is not available for a domain, mail receiving may be rejected, as recipient cannot confirm whether the mail comes from an authorized mail server.  

#### NHN Cloud SPF Record Setting 

Even after SPF setting for TXT record is changed, it may take 10 minutes to 24 hours until DNS change is fully transmitted. It is safe to send emails after hours since SPF setting change is done.  

#### How to Find TXT Record of DNS
For Linux 
```
nslookup -q=TXT <your.domain.name>
```
```
dig -t TXT <your.domain.name>
```
For Windows 
```
nslookup -q=TXT <your.domain.name>
```

### Confirmation of Receipt on Gmail 

On Gmail, it is unavailable to collect whether mails are properly received. Generally, to check mail receiving, images are inserted; but for Gmail, the in-between server changes to disallow the image tracking. Such intentional block makes it technically impossible to collect data on mail receiving.  

```
Gmail Image Proxy

Because the Gmail Image Proxy service does not forward users' cookies, you can't use the measurement protocol to track Gmail users. The Gmail Image Proxy service prevents this by having the measurement protocol requests passed through an intermediate server.
```

#### For Reference 
[Google Analytics > Email Tracking - Measurement Protocol](https://developers.google.com/analytics/devguides/collection/protocol/v1/email)

### Issue of Low Reputation on Gmail

Here is a brief guide of Gmail on reputation: how 'reputation' is evaluated on Gmail, and how to raise or remain 'reputation'. 
For more details, please see reference at the bottom of the page. 

#### What is Reputation? 
For a mail delivery, the Inbox Service Provider, or ISP (Gmail receiver SMPT server), evaluates the reputation of sender SMTP server to decide whether to receive, and also to classify Spam messages. If mail sending is not right for the reputation evaluation method, it may cause  `Slower delivery speed` and `difficulties in receiving mails from recipients` who use the same ISP. 
 Reputation is largely categorized into IP Reputation, and Domain Reputation. 

* IP Reputation:  IP refers to the IP of sender SMTP. It is the reputation of IP of the sender SMTP. 
* Domain Reputation: Domain refers to the mail sender's domain. For instance, NHN's domain is 'nhn.com'. It is the reputation of mail sender's domain. 

#### Evaluation Method of Reputation by Gmail 
##### Focus More on Domain Reputation    
Since, in many cases, a single IP is shared by many domains, domain is regarded as a more important point of evaluation than IP. Engagement is considered more highly than complaints: Complaints refer to spamming by recipients, while Engagement refers to the click of a link or unspamming. 
##### Evaluate Many Other Points As Well  
 Reputation is determined in consideration of many aspects of a mail: if it is personal or not, reputation of its sender IP, existence of a link within, or the content of it.   

#### How to Raise Reputation
##### Need the Warm-up Process
It is not recommended to abruptly send bulk mails to new a IP or domain, which may cause low reputation. It is better to increase the number gradually, like from 50 to 100, 500, and then 1,000, throughout many days. 
##### Send to Largely Engaged Recipients 
It must be proved that the mails we sent by Gmail are what recipients have wanted. Sending mails, during the warm-up period, to much loyal users who are anxious to receive mails is a good way of raising reputation. 
##### Allow Permissions 
The feature of permission of receiving must be enabled.
##### Relevancy
To remain high reputation, mails must be sent to those who are relevant. This is more important than frequent mailing. Size of target doesn't really matter; but, mail content and target must be separated for delivery. About 66% of canceling subscription is due to irrelevant mails and 55% to fatiguing messages. 

#### For Reference 
Gmail Inbox Delivery and Domain Reputation: What You Need to Know Now - [Bronto](https://bronto.com/)
Domain Reputation Or IP Reputation: Which One Does Gmail Care About More? - [Mailgun](https://www.mailgun.com)
