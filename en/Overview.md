## Notification > Email > Overview

In the NHN Cloud Email service, you can use a number of features, including mail delivery, sending scheduled delivery, checking delivery history and managing templates, etc.
It also provides RESTful APIs for easy integration with proprietary delivery programs and management tools.

## Main Features

- Mass Delivery Support
    - You can enter a list of recipients in Excel file and send mail in bulk.
- Tag Delivery Support
    - You can enter a list of recipients list by using tag and send mail in bulk.
- Scheduled Delivery
    - You can send mails at any time you want.
- Replacement Tag
    - Use Replacement Tags to send personalized mails to each recipient.
- Template Feature
    - You can register and use frequently used mail formats as templates.
- Manage Advertising Mail Unsubscription
    - You can view mail addresses that are denied or added to the Unsubscription Recipient list and manage the Unsubscription List.
- Email Security Feature
    - You can safely send mail by performing SPF / DKIM / DMARC record authentication.
- Retrieve Statistics
    - You can analyze detailed results for each mail delivery case.

- You can use the mail delivery and view RESTful API available in customer’s applications.

## Precautions Before Delivery
To prevent emails sent using NHN Cloud Email service from being treated as spam by an external email service, you should keep in mind the following.

First, DNS must be set up. <br/>
Since it is sent with email address containing a user-owned domain through NHN Cloud Email service's Mail delivery (SMTP) server, additional settings are required in DNS where user-owned domain is serviced.
For more information about additional DNS settings, see **Domain Management Guide**.

- [Domain Management Guide > Domain Authentication and Protection ](./domain-verification/)
- [Domain Management Guide > SPF](./spf-record/)
- [Domain Management Guide > DKIM](./dkim-record/)
- [Domain Management Guide > DMARC](./dmarc-record/)

Starting from February 1, 2024, [Gmail sender guidelines](https://support.google.com/mail/answer/81126?hl=ko#requirements-5k)will change and send requests to some receiving mail systems may be rejected if you do not have domain authentication and SPF, DKIM, and DMARC authentication.

**Restricted Destination Domains When Not Authenticated**
- gmail.com
- yahoo.com

Second, Gmail uses domain reputation as a primary criterion for determining spam. If a sender with a low domain reputation sends a high volume of emails at a high speed, or sends advertising emails that the recipient doesn't want, the emails are stored in their "inbox" but with warning or stored in their ‘spam’ folder, or the recipient can slow down or deny the emails. Therefore, if you want to send out emails in bulk, you should always take care to keep your domain reputation high by referring to the following guide on how to manage your domain reputation.<br>
For more information, see the [troubleshooting guide](./troubleshooting-guide/).
