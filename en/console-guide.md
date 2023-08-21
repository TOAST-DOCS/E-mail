## Notification > Email > Console Guide

## Mail Delivery

### General Mails

Enter recipients, title, body, and attachment to send mails.
For the delivery of general mails, you need to enter recipients yourself.

![email_01_201812_en.png](https://static.toastoven.net/prod_email/email_01_201812_en.png)

Select **Disable** for **Enable Template or Not** in the **Deliver Mails** tab.
Select **General Mails** for **Mail Type**.
**Mail Delivery**: Enter an email address to send. Write in the name format to enter sender name and address. .
**Scheduled Delivery**: Specify the date and time of email delivery.
**Attached File**: Attach files to mail.

* Up to 10 files can be uploaded, not exceeding 30MB.
* The total volume of attachment cannot be more than 30MB.
* Files can be attached up to 30MB, but they may be rejected due to 'Limit Exceeded' or regarded as spams, depending on the mail system.

**Title/Body**: Enter the title and body of the mail.

![email_02_201812](https://static.toastoven.net/prod_email/email_02_201904_en.png)

Enter email addresses for **Recipients** and **Cc Recipients**.

### Advertising Mails

You can send advertising mails.

#### Notes for Sending Advertising Mails

In accordance with Act on Promotion of Information and Communications Network Utilization and Information Protection, you are obliged to follow the rules as below, to send advertising or promotional mails. [[check at Korea Internet & Security Agency](https://spam.kisa.or.kr/spam/sub62.do)]

1. Send ad mails only to those recipients who explicitly agreed to receive ones.
The sender shall be held liable for any conflicts that may arise in violation of it.
2. Display the (AD) phrase at the beginning of title.
3. Show sender information, including name, email address, phone number and home address, in the body.
4. Clearly display guides for recipients to block receiving or withdraw consent to receive promotional mails, and come up with technical means to easily follow their choices.

NHN Cloud shall provide technical means as below to 'Advertising Mails' in compliance of the Act on Promotion of Information and Communications Network Utilization and Information Protection:

* Insert the (AD) phrase to title.
* Allow Reject Receiving Ad Mails for recipients.
* Do not send mails to the mail addresses rejecting ad mails.

Send ad mails in reference of the following:

![email_03_201812_en.png](https://static.toastoven.net/prod_email/email_03_201812_en.png)

1. The title must include the "(AD)" phrase.
2. Links to Reject Receiving must be included in Korean/English.
3. Ad mails are not delivered to the users who are registered to reject ones.

Following keys are provided as part of rejection links: 

| Key                     | Phrase| Example of Usage                                                                                                              |
|-------------------------| - |-------------------------------------------------------------------------------------------------------------------------------|
| EN_BLOCK_RECEIVER_LINK  | [Unsubscription](#) | If you no longer wish to receive these emails, please click the ##EN_BLOCK_RECEIVER_LINK##.                                   |
| BLOCK_RECEIVER_LINK_URL | - | If you no longer wish to receive these emails, please `<a href='##BLOCK_RECEIVER_LINK_URL##' target='_blank'>click here</a>`. |

### Use Templates to Send Mails

You can send mails on a template created in advance.

![email_04_201812_en.png](https://static.toastoven.net/prod_email/email_04_201812_en.png)

1. To use a template, select **Enable** for **Use Templates**.
2. Click **Select a Template**.
3. Select a template created in advance from the Category of **Select a Template**.
    * On how to create a template, refer to **Manage Categories and Templates** section below.
4. Click **Apply Template**.

### Use Tags to Send Mails

You can send mails by using tags.
For more details on tags, refer to **Use Tags to Send** in **[Overview](Overview.md)**.

Select **Disable** for **Enable Template or Not** in the **Deliver Mails** tab and select **General Mails** for **Mail Type**.

* **Mail Delivery**: Enter an email address to send. Write in the name format to enter sender name and address. .
* **Scheduled Delivery**: Specify the date and time of email delivery.
* **Attached File**: Attach files to mail.
    * Up to 10 files can be uploaded, not exceeding 30MB.
    * The total volume of attachment cannot be more than 30MB.
* **Title/Body**: Enter the title and body of the mail.

![email_05_201812_en.png](https://static.toastoven.net/prod_email/email_05_201812_en.png)

1. Select **Tag Delivery** at the bottom of the page and click **Select Tags**.
    * To add tags, please refer to **Manage Tags**.
2. Select tags on **Select Tags** screen.
3. Click **Apply Selectively**.
4. You can select up to four tags.
    * You can apply AND, OR conditions to many tags selected.
        * AND: The destination is recipients who satisfy all the tags.
        * OR: The destination is recipients who satisfy one or more of the many tags.

![email_06_201812_en.png](https://static.toastoven.net/prod_email/email_06_201812_en.png)

Click **Schedule Delivery** at the bottom right of the mail body.
On the **Schedule Delivery** window, click your choice.

* **Confirm and Process**: Check mail address of each recipient and body in the **Retreive Tagged Mail Delivery** tab before sending mails. Unless delivery is requested **within 7 days** after registration, data may be deleted and it may not be properly sent (not supported for scheduled delivery).
* **Immediate Delivery**: Send mails immediately without checking receiving information. You can find delivery result in the Query Tagged Mail Delivery tab.

### Upload files to Send Mass Mails

You can use files to send mass mails.

Select **Not Use** for **Template Use** in the **Send Mails** tab, and select **General Mails** for **Mail Type**.

![email_07_201812_en.png](https://static.toastoven.net/prod_email/email_07_201812_en.png)

1. Enter mail content.
    * **Mail Delivery**: Enter an email address to send. Write in the name format to enter sender name and address.
    * **Scheduled Delivery**: Specify the date and time of email delivery.
    * **Attached File**: Attach files to mail.
        * Up to 10 files can be uploaded, not exceeding 30MB.
        * The total volume of attachment cannot be more than 30MB.
    * **Title**/**Body**: Enter the title and body of the mail. If the title and body of mail is entered in the ##replacement key##format, the mail can be replaced with user specified data. For instance, enter ##name## in the user name, and it is replaced with a user specified name, such as 'Hana Oh' before sent.
2. Go to **Mass Delivery** at the bottom and click **Download Templates**.
3. Templates are available either in CSV or Excel.

![email_08_201812_en.png](https://static.toastoven.net/prod_email/email_08_201812_en.png)

Enter replacement data in the template file, such as recipient's mail address and recipient name.

1. Click **Upload Recipient File** to upload files.
    * File containing template data can be uploaded with 10 thousand persons, up to 3MB.
    * Any error in template data will be shown. Please check and upload it with correct template data.
    * The total number (no more than 10) with detail information shows.
    * **Error Types**
        * When the receive\_mail\_address row does not exist: The receive\_mail\_address row is for recipient's mail address and it's a required field\.
        * When there is no entered data available
        * When recipient's mail address or replacement data is missing
2. Click **Schedule Delivery** at the bottom right of the mail page.
3. On the **Schedule Delivery** window, click your choice.
    * **Check and Send**: Check mail address of each recipient and body in the **Retrieve Tagged Mail Delivery** tab before sending mails. Unless delivery is requested **within 7 days** after registration, data may be deleted and it may not be properly sent (not supported for scheduled delivery).
    * **Immediate Delivery**: Send mails immediately without checking receiving information. You can find delivery result in the Query Tagged Mail Delivery tab.

## Query of Mails

### Retrieve by Mail Request

You can query sent mails by Date and Time of Delivery, Date and Time of Receipt, Template, and Delivery Status on **Retrieve by Mail Request**.

![email_18_201812_en.png](https://static.toastoven.net/prod_email/email_18_201812_en.png)

Click a mail on the list to check its details.

![email_19_201812_en.png](https://static.toastoven.net/prod_email/email_19_201812_en.png)

### Retrieve Bulk Mail Delivery

In the **Retrieve Bulk Mail Delivery** tab, you can retrieve scheduled mass mail delivery or cancel delivery.

![email_09_201812_en.png](https://static.toastoven.net/prod_email/email_09_201812_en.png)

* Click **Send** to send mails: may take quite a while, depending on the number of deliveries.
* Click **Cancel** to cancel sending mails.

**Status of Bulk Mail Delivery**

* Waiting: Recipient file data is still not read.
* Preparing Delivery: Recipient file data is being uploaded.
* Preparing Delivery Completed: Sending mail is completely prepared. Select a schedule (a column on the list) and check mail delivery details per recipient on the list at the bottom.
* Ready for Delivery: It is ready to send mails.
* Delivering: Mail is now on the delivery. Select a schedule (a column on the list) and check the delivery processing rate.
* Delivery Completed: Delivery has been normally completed.
* Delivery Failed: Error occurred during delivery. To check delivery status, query received mails.
* Delivery Cancelled: User has cancelled sending mails.

**Retrieve Mail Delivery per Recipient**

![email_10_201812_en.png](https://static.toastoven.net/prod_email/email_10_201812_en.png)

1. Select a bulk mail delivery record (a row on the list).
2. A detailed list of mail delivery per recipient will be shown at the bottom tab. Click on **View Details**.
3. You can check whether the body content has been successfully replaced by the replacement key.

### Retrieve Tagged Mail Delivery

* In the **Retrieve Tagged Mail Delivery** tab, you can retrieve scheduled tagged mail delivery or cancel delivery.

![email_11_201812_en.png](https://static.toastoven.net/prod_email/email_11_201812_en.png)

* Click **Send** to send mails: may take quite a while, depending on the number of deliveries.
* Click **Cancel** to cancel sending mails.

**Status of Tagged Mails Delivery**

* Waiting: Recipient file data is still not read.
* Preparing Delivery: Recipient file data is being uploaded.
* Preparing Delivery Completed: Sending mail is completely prepared. Select a schedule (a column on the list) and check mail delivery details per recipient on the list at the bottom.
* Ready for Delivery: It is ready to send mails.
* Delivering: Mail is now on the delivery. Select a schedule (a column on the list) and check the delivery processing rate.
* Delivery Completed: Delivery has been normally completed.
* Delivery Failed: Error occurred during delivery. To check delivery status, query received mails.
* Delivery Cancelled: User has cancelled sending mails.

**Retrieve Mail Delivery per Recipient**

![email_12_201812_en.png](https://static.toastoven.net/prod_email/email_12_201812_en.png)

Select a tagged mail schedule (a column on the list) to query the list of mail deliver per recipient at the bottom.

## Manage Categories and Templates

### Categories

You can create categories to sort templates for mail delivery.

![email_13_201812_en.png](https://static.toastoven.net/prod_email/email_13_201812_en.png)

1. Select a category from **Categories** on the left.
2. Click **Add Cetegory**.
    * Please make sure that the category is selected first.
3. When **Add Category** window pops up, complete the form.
4. Click **Add**.

To modify category, select a category and click **Modify**. When **Modify Category** window pops up, modify the content and click **Modify**.

### Templates

![email_14_201812_en.png](https://static.toastoven.net/prod_email/email_14_201812_en.png)

1. To add a template, select a category and click **Add Templates**.
2. Complete the form with frequently used items.
3. Click **Add**.

To modify a template, select a template, modify it, and then click **Modify**.

## Mail Domain Management

Delivery mail domains can be managed from the **Mail Domain Management** tab.<br>
This feature is useful for personal or company-owned domains which can be prevented from third-party abuses.  

* This stage is not a must to enable email services.
* It is unavailable for non-personal or non-company owned domains. 
    * Cannot use domains of email account providers, such as Daum, Naver, or Google. 

### Register Mail Domains
Click **Register Mail Domains** to register domains for delivery mails. <br>
Only root domains can be registered. 

### Verify Mail Domains
Clic **Verify** to verify domain ownership. <br>
Register a token issued from domain registration at TXT record, and click **Verify**. <br>

* If "toast-domain-verification={token}" does not exist nor match, verification fails.  
* It may take time to transmit changes after TXT record is registered at domain. 

### Protect Mail Domains
Click **Protect** to protect domains. <br>
With this, following can be applied. <br>

* Protected domains can be enabled for verified projects only. 
    * When a same domain is verfied by many projects, applying protection to only one project allows authenticated projects to use all domains. 
    * e.g.) Verify "toast.com" for email services of Projects A and B, and protect Project A.
        * "toast.com" can serve as delivery address for Projects A and B only 
        * For Project C, "toast.com" is not available as delivery address since it is not verified for the project. 

### Share Mail Domains
Click **Share > Setting** to share domains. <br>
Main features are as follows.<br>

* Query project with verified domains
    * Provide the list of NHN Cloud projects completed with verification for specific domains.
* Share domain with other projects 
    * No need to repeat the verification process, when verifying same domain for many projects.<br>
    * After verified from a project, domains can be shared by entering appkey for other projects' email services. 

### Register Sub-domains
Click **Main Sub-domain > Setting** to register sub-domains. <br>
Only registered root domain's sub-domains can be registered. <br>
Registered sub-domains can be used as **Custom Header** to send mails. 

* Custom Headers available for Sub-domains 
    * Header Name: X-TC-ENVELOPE-FROM
    * Header Value: Sub-domain
    * Main features are as follows:
        * To inspect SPF records for recipient's mail service
            * e.g.) When mail delivery is requested for Recipient: test@toast.com, X-TC-ENVELOPE-FROM: test@cs.toast.com
                * Inspect SPF records with "cs.toast.com", from recipient's mail service
        * To send returned mails for recipient's mail service
            * e.g.) When mail delivery is requested for Recipient: test@toast.com, X-TC-ENVELOPE-FROM: test@cs.toast.com
                * Send returned mail to test@cs.toast.com, from recipient's mail service 

* For more details, see the guide for [Custom header](https://docs.toast.com/en/Notification/Email/en/Overview/#custom-header).

<span id="dkim"></span>
### DomainKeys Identified Mail (DKIM)
DKIM, or DomainKeys Identified Mail refers to a technique by which the receiver can check if the sender address or email is forged. <br>
To enable DKIM, click **DKIM > Setting**.<br>
Its main features are like follows: <br>


* Register and Authenticate DKIM TXT Records 
    * Click **Authenticate DKIM**.
    * Register issued TXT record onto TXT Record of domain, and click **Authenticate**. 
    * If "v=DKIM1;k=rsa;p={issued RSA publicKey value}" of domain's TXT record does not exist or is not consistent, authentication fails.  
    * It might take time to register TXT record to domain and transmit changes. 
* Enable or Disable DKIM 
    * Click **DKIM Features**.
    * Press **Enable** or **Disable** to control DKIM features. <br>

* You may do like below to check if DKIM works properly. 
    * Authenticate and enable DKIM for a particular sender's domain
    * Send email to a particular domain 
    * Read the original of a receiving email 
    * Check if the Authentication-Results header includes 'dkim=pass'
        * Each email service provider may provide different header for dkim=pass.

## Tag Management

You can create, modify, or delete tags applied to send mails in the **Manage Tags** tab.<br>
Click a tag and the list of UIDs registered to the tag can be retrieved.<br>
태그는 Notification 서비스 전역으로 사용됩니다.
- 태그는 최대 2,048개까지 생성할 수 있습니다.
- 하나의 UID에 태그를 16개까지 추가할 수 있습니다.

![email_15_201812_en.png](https://static.toastoven.net/prod_email/email_15_201812_en.png)

When there is no registered tag, click **Register Tags**, enter **Tag Name** on the **Register/Modify Tags** window, and then click **Register**.

To register UIDs to a tag, select a tag from the list of registered tags and click **Register UIDs**.
Enter an UID on the **Register UIDs** window and click **Register**.

Up to 1,000 UIDs can be registered at once, for a tag.

## UID Management

Go to the **Manage UIDs** tab to manage UIDs and mail addresses.

![email_16_201812_en.png](https://static.toastoven.net/prod_email/email_16_201812_en.png)

UIDs, registered to a tag, can be used to easily send mails to multiple recipients.

![email_17_201812_en.png](https://static.toastoven.net/prod_email/email_17_201812_en.png)

1. Click **Register UIDs**.
2. Click **Download Templates**. You can enter multiple UIDs and mail address in CSV format.
    * Download a template, and enter UID on the first row, and mail address on the second row.
3. Save the file and click **Upload UID Files** to upload the file.
4. Click **Register**.

## Manage Call Rejects

You can manage rejecting users on the **Manage Call Rejcts** tab.

![email_20_201812_en.png](https://static.toastoven.net/prod_email/email_20_201812_en.png)

You may search for users who have unsubscribed so far, and register or delete rejections.

#### Register Rejections

![email_21_201812_en.png](https://static.toastoven.net/prod_email/email_21_201812_en.png)

1. Click **Register Rejections**.
2. Enter unsubscription mail address in the **Register Rejections** window.
    * Click **Add** to register up to 10 persons.
3. Click **Register**.

#### Registration with Template Files

You can upload rejection files in CSV, text (.txt), or excel (.xlsx).

![email_22_201812_en.png](https://static.toastoven.net/prod_email/email_22_201812_en.png)

1. Select **Batch Registration of Mail Addresses** to upload a file.
2. Complete an upload and click **Register**.
    * When they're normally uploaded, the message will show **A total of n mail addresses are found**.

![email_23_201812_en.png](https://static.toastoven.net/prod_email/email_23_201812_en.png)

When it is fully uploaded, you can check the list on the **Manage Call Rejects** tab

#### Delete Rejections

![email_24_201812_en.png](https://static.toastoven.net/prod_email/email_24_201812_en.png)

1. Select email addresses to delete.
2. Click **Delete Rejections**.
3. Check the message in the **Undo Rejection** window and click **Yes**.

![email_20_201812_en.png](https://static.toastoven.net/prod_email/email_20_201812_en.png)

When it is completely deleted, you can check it on the **Manage Call Rejects** tab.

## Retrieve Statistics

Check delivery statistics on the **Retrieve Statistics** tab.
You can retrieve by conditions, such as period, statistical type (date, time, or day), mail type (general or mass), or template.

![email_26_201812_en.png](https://static.toastoven.net/prod_email/email_26_201812_en.png)

<span id='personal-information-assignor'></span>

## 발송 설정
메시지 보관 기간 정책에 따라 90일이 지난 발송 이력 데이터를 백업할 수 있습니다.
백업 사용 여부, 파일 확장자, 파일을 업로드할 저장소 정보를 입력하면 해당 저장소에 백업 일자가 포함된 파일이 생성됩니다.

## Guide for Notice of Personal Information Assignor

When the Customer uses NHN Cloud > Email Service, assignment of personal information between the Customer and the Company arises, and the assignee, the Customer, is obliged to disclose the status (assignor and content of business) of his assignment of personal information to the Company, through the personal information handling policy, in accordance with Act on Promotion of Information and Communications Network Utilization and Information Protection. Accordingly, the Company may provide guidelines as below for the Customer, to abide by relevant regulations in the use of NHN Cloud Email Service and not to be adversely affected for not disclosing his assignment status:

(Example)

[Notice of Personal Information Assignor] To use NHN Cloud Email Service, make sure the following is displayed for 'Personal Information Handling Policy' > Assignment Status of the Customer.

Assignor: NHN Cloud Corporation
Content of Business: Send emails in lieu of customers
