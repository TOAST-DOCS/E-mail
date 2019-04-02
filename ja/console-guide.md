## Notification > Email > Console Guide

## Send Mails

### Send General Mails

Enter recipients, title, body, and attachment to send mails.
For the delivery of general mails, you need to enter recipients yourself.

![email_01_201812_en.png](https://static.toastoven.net/prod_email/email_01_201812_en.png)

Select **Disable** for **Enable Template or Not** in the **Deliver Mails** tab.
Select **General Mails** for **Mail Type**.
**Mail Delivery**: Enter an email address to send. Write in the name format to enter sender name and address. .
**Scheduled Delivery**: Specify the date and time of email delivery.
**Attached File**: Attach files to mail.

* Up to five files can be uploaded, not exceeding 10MB.
* The total volume of attachment cannot be more than 10MB.

**Title/Body**: Enter the title and body of the mail.

Enter email addresses for **Recipients** and **Cc Recipients**.

### Send Advertising Mails

You can send advertising mails.

#### Notes for Sending Advertising Mails

In accordance with Act on Promotion of Information and Communications Network Utilization and Information Protection, you are obliged to follow the rules as below, to send advertising or promotional mails. [[check at Korea Internet & Security Agency](https://spam.kisa.or.kr/spam/sub62.do)]

1. Send ad mails only to those recipients who explicitly agreed to receive ones.
The sender shall be held liable for any conflicts that may arise in violation of it.
2. Display the (AD) phrase at the beginning of title.
3. Show sender information, including name, email address, phone number and home address, in the body.
4. Clearly display guides for recipients to block receiving or withdraw consent to receive promotional mails, and come up with technical means to easily follow their choices.

TOAST shall provide technical means as below to 'Advertising Mails' in compliance of the Act on Promotion of Information and Communications Network Utilization and Information Protection:

* Insert the (AD) phrase to title.
* Allow Reject Receiving Ad Mails for recipients.
* Do not send mails to the mail addresses rejecting ad mails.

Send ad mails in reference of the following:

![email_03_201812_en.png](https://static.toastoven.net/prod_email/email_03_201812_en.png)

1. The title must include the "(AD)" phrase.
2. Links to Reject Receiving must be included in Korean/English.
3. Ad mails are not delivered to the users who are registered to reject ones.

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
    * Up to five files can be uploaded, not exceeding 10MB.
    * The total volume of attachment cannot be more than 10MB.
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

* **Confirm and Process**: Check mail address of each recipient and body in the **Retreive Tagged Mail Delivery** tab before sending mails. Keep note that mail cannot be sent unless user delivers.
* **Immediate Delivery**: Send mails immediately without checking receiving information. You can find delivery result in the Query Tagged Mail Delivery tab.

### Upload files to Send Mass Mails

You can use files to send mass mails.

Select **Not Use** for **Template Use** in the **Send Mails** tab, and select **General Mails** for **Mail Type**.

![email_07_201812_en.png](https://static.toastoven.net/prod_email/email_07_201812_en.png)

1. Enter mail content.
    * **Mail Delivery**: Enter an email address to send. Write in the name format to enter sender name and address.
    * **Scheduled Delivery**: Specify the date and time of email delivery.
    * **Attached File**: Attach files to mail.
        * Up to five files can be uploaded, not exceeding 10MB.
        * The total volume of attachment cannot be more than 10MB.
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
    * **Check and Send**: Check mail address of each recipient and body in the **Retrieve Tagged Mail Delivery** tab before sending mails. Keep note that mail cannot be sent unless user delivers.
    * **Immediate Delivery**: Send mails immediately without checking receiving information. You can find delivery result in the Query Tagged Mail Delivery tab.

## View Delivery

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

### Manage Categories and Templates

#### Categories

You can create categories to sort templates for mail delivery.

![email_13_201812_en.png](https://static.toastoven.net/prod_email/email_13_201812_en.png)

1. Select a category from **Categories** on the left.
2. Click **Add Cetegory**.
    * Please make sure that the category is selected first.
3. When **Add Category** window pops up, complete the form.
4. Click **Add**.

To modify category, select a category and click **Modify**. When **Modify Category** window pops up, modify the content and click **Modify**.

#### Templates

![email_14_201812_en.png](https://static.toastoven.net/prod_email/email_14_201812_en.png)

1. To add a template, select a category and click **Add Templates**.
2. Complete the form with frequently used items.
3. Click **Add**.

To modify a template, select a template, modify it, and then click **Modify**.

### Manage Tags

You can create, modify, or delete tags applied to send mails in the **Manage Tags** tab.
Click a tag and the list of UIDs registered to the tag can be retrieved.

![email_15_201812_en.png](https://static.toastoven.net/prod_email/email_15_201812_en.png)

When there is no registered tag, click **Register Tags**, enter **Tag Name** on the **Register/Modify Tags** window, and then click **Register**.

To register UIDs to a tag, select a tag from the list of registered tags and click **Register UIDs**.
Enter an UID on the **Register UIDs** window and click **Register**.

Up to 1,000 UIDs can be registered at once, for a tag.

### Manage UIDs

Go to the **Manage UIDs** tab to manage UIDs and mail addresses.

![email_16_201812_en.png](https://static.toastoven.net/prod_email/email_16_201812_en.png)

UIDs, registered to a tag, can be used to easily send mails to multiple recipients.

![email_17_201812_en.png](https://static.toastoven.net/prod_email/email_17_201812_en.png)

1. Click **Register UIDs**.
2. Click **Download Templates**. You can enter multiple UIDs and mail address in CSV format.
    * Download a template, and enter UID on the first row, and mail address on the second row.
3. Save the file and click **Upload UID Files** to upload the file.
4. Click **Register**.

### Manage Call Rejects

You can manage rejecting users on the **Manage Call Rejct** tab.

![email_20_201812_en.png](https://static.toastoven.net/prod_email/email_20_201812_en.png)

You may search for users who have unsubscribed so far, and register or delete rejections.

#### Register Rejections Manually

![email_21_201812_en.png](https://static.toastoven.net/prod_email/email_21_201812_en.png)

1. Click **Register Rejections**.
2. Enter unsubscription mail address in the **Register Rejections** window.
    * Click **Add** to register up to 10 persons.
3. Click **Register**.

#### Register Rejections by Uploading a Template File

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

### Retrieve Statistics

Check delivery statistics on the **Retrieve Statistics** tab.
You can retrieve by conditions, such as period, statistical type (date, time, or day), mail type (general or mass), or template.

![email_26_201812_en.png](https://static.toastoven.net/prod_email/email_26_201812_en.png)

### Guide for Notice of Personal Information Assignor

When the Customer uses TOAST > Email Service, assignment of personal information between the Customer and the Company arises, and the assignee, the Customer, is obliged to disclose the status (assignor and content of business) of his assignment of personal information to the Company, through the personal information handling policy, in accordance with Act on Promotion of Information and Communications Network Utilization and Information Protection. Accordingly, the Company may provide guidelines as below for the Customer, to abide by relevant regulations in the use of TOAST Email Service and not to be adversely affected for not disclosing his assignment status:

(Example)

[Notice of Personal Information Assignor] To use TOAST Email Service, make sure the following is displayed for 'Personal Information Handling Policy' > Assignment Status of the Customer.

Assignor: NHN Corporation
Content of Business: Send emails in lieu of customers