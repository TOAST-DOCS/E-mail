## Notification > Email > Overview

With Email, you can send mails, send by schedule, query delivery history, and manage templates. 
<br>RESTful API is also available for easy integration. 

## Main Features

- Send Mass Mails
  - Enter the list of recipients in excel and send mass mails.  
- Send by Tags 
  \-  Enter the list of recipients using tags and send mass mails.
- Schedule Delivery
  \- Send mails in time of your schedule. 
- Provide Replacement Tags 
  \- Send personalized mails for each recipient with replacement tags.
- Support Template Functions 
  \- Register frequently-used mails as templates. 

- Sending mails  and query of RESTful API are also available on your application. 

## Caution before Sending Mails
To prevent mails sent from NHN Cloud Email being processed as spam mails at external email services, keep note of the following: 

First, DNS setting is required. <br/>Since mail is sent from the email address which includes the owner's domain via NHN Cloud Email mail delivery server (SMTP), additional setting is required for DNS in which user's own domain can be serviced. <br/> _spfblocka.toast.com must be registered for SPF of TXT record at DNS. If SPF setting is not right, a mail may be considered as a spam or one from a faulty user, hence receiving a warning sign or saved at 'Spam Inbox', which makes it hard for the recipient to receive. <br>

Please see [Troubleshooting Guide](./troubleshooting-guide/) on how to set SPF. 
   

Second, Gmail considers highly of domain reputation, even for determining spam mails. 

If a sender of low domain reputation fast sends a bulk email, or sends ad emails to unsolicited users,  such mails may be saved at 'Inbox' but with a warning sign; or they may be saved at 'Spam Inbox', or restricted in speed, or even rejected of receiving. Therefore, any user who wants to send bulk emails must take cautious approach by taking reference of domain reputation management guide, so as to remain high on domain reputation.<br>
For more details, please see [Troubleshooting Guide](./troubleshooting-guide/)

## Reference

<span id='tags-and-uids'></span>
### Tags and UIDs

#### Glossary
|Glossary| Description |
|---|---|
|Tag|A system that classifies UID. <br>Many tags can be attached to an UID so as to help users to easily search and use UID information.|
|UID|ID (identifier) that classifies users. <br>One UID can have multiple contacts to be applied for delivery. |
|Contact|A specified location to contact. <br>Notification provides three products to register contact: Push, Email, and SMS. <br />Push regards to tokens; Email to mail addresses; and, SMS to phone numbers.|

#### Use Tags to Send Mails 
* You can send mails by selecting tags, instead of mail addresses, as recipient information. 

1. Register UID.

* Go to **Manage UIDs** and register UID and one or many mail addresses. 
* For more details, refer to [Manage UIDs](./console-guide/#uid). 

2. Register tags.

* Go to **Manage Tags** and register tags.
* For more details, refer to [Manage Tags](./console-guide/#_11). 

3.  Register UID to a tag.

* Go to **Manage Tags** and register UID to a registered tag.

4. Select tags to send mails. 

* Go to **Send Mails** and Select **Send Tags**, instead of mail addresses, and register tags. 
* Mails are to be sent to mail addresses of UID which is registered to a tag. 
* For more details, refer to [Send Mails using Tags](./console-guide/#_6).

#### Tags of Other Products 
* You can share your tag and UID information of Email with Push or SMS, without the need of re-registration. 
* Other contacts can be added to a same UID of each product console. 

### Custom Header

* Custom header can be added to a receiving email for delivery. 
* NHN Cloud Email Service adopts the headers following the format as described in [RFC 822](https://www.ietf.org/rfc/rfc0822.txt), except below: 

#### Headers Unavailable to Request

* From
* To
* Cc
* Bcc
* Date
* Subject
* Content-Disposition
* Message-ID
* Sender
* Reply-To
* Newsgroups
* Content-ID
* Content-MD5
* MIME-Version
* Content-Transfer-Encoding
* Content-Description

#### Composition 

| Term | Description |
|---|---|
|Header Name| Name of a header to add. <br>Comprised of between 1 and 50 letters, in English and numbers (a-z, A-Z, 0-9), and hyphen only. |
|Header Value| Value of a header to add. <br>Composed of between 1 and 1000 bytes. <br>Non-ASCII characters are encoded in Base64 before delivered. |

### Data Retention Period 
According to the retention policy, email data of the recent 90 days only can be retained. 
Data older than that are deleted and cannot be queried. 
