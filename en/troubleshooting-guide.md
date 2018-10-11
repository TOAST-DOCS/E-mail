## Notification > Email > Troubleshooting Guide

### Register Sender Policy Framework (SPF)
As mails sent  by TOAST Email are often considered spams, we provide the guide as below to register SPF.   

#### Circumstance and Cause 

* Circumstance: Sent mails are not delivered to recipient's mail address but returned as spams. 
* Cause: SPF not registered 

#### What is SPF?

|Term| Description |
|---|---|
|SPF Record|As part of a DNS (Domain Name System) record, it validates mail servers which can send mails from a domain.|

* Create an SPF record for the domain of email addresses for **Mails to Send**.
* If SPF record is not available for a domain, mail receiving may be rejected, as recipient cannot confirm whether the mail comes from an authorized mail server.  

#### TOAST SPF Record 
Register below to TXT record of DNS. 
For more details, contact your DNS manager. 

```
v=spf1 include:_spfblocka.toast.com ~all
```
