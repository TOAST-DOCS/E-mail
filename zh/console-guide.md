## Notification > Email > Console Guide

### Enable Email Service

To use Email Service, access console, click **Select Services** and go to **Notification > Email**, and you can find enabled services in the search menu on the left.  

### Send General Mails

Enter recipients, title, body, and attachment to send mails.
For the delivery of general mails, you need to enter recipients yourself.

1. In the search menu on the left, click **Notification > Email > Send Emails**.
2. Fill in the blanks of the format.
   Enter email addresses for **Recipients** and **Cc Recipients**.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_03.jpg)

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

-  The title must include the "(AD)" phrase.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_23.jpg)
<p>

- Links to Reject Receiving must be included in Korean/English.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_24.jpg)
<p>

- Ad mails are not delivered to the users who are registered to reject ones.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_25.jpg)

### Use Templates to Send Mails  

You can send mails on a template created in advance.

To use a template, select **Enable** for **Use Templates** and click **Select a Template**.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_04.jpg)
<p></p>
Select a template created in advance from the Category of **Select a Template**. On how to create a template, refer to **Manage Categories and Templates** section below.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_05.jpg)
<p></p>

![](http://static.toastoven.net/prod_email/email_getting_0622_2_06.jpg)


### Use Tags to Send Mails

You can send mails by using tags.
For more details on tags, refer to **Use Tags to Send** in **[Overview](Overview.md)**.

Select **Not Use** for **Template Use** in the **Send Mails** tab, and select **General Mails** for **Mail Type**.

- **Mails to Send **: Enter an email address to send. Write in the name <emailAddress> format to enter sender name and address.   .
- **Scheduled Delivery**: Specify the date and time of email delivery.
- **Attachment **: Attach files to mail.
  - Up to five files can be uploaded, not exceeding 10MB.
  - The total volume of attachment cannot be more than 10MB.
- **Title/Body**: Enter the title and body of the mail.

Select **Deliver Tags** at the **bottom of the page** and click **Select Tags**. Register tags, in reference of **Manage Tags**.  

Select tags on **Select Tags** screen and click **Apply Selection**.

You can select up to four tags.
![](http://static.toastoven.net/prod_email/email_getting_0122_04.png)

You can apply AND, OR conditions to many tags selected.  
- AND: The destination is recipients who satisfy all the tags.
- OR: The destination is recipients who satisfy one or more of the many tags.

![](http://static.toastoven.net/prod_email/email_getting_0122_05.png)

Click **Schedule Delivery** at the bottom right of the mail page. On the **Schedule Delivery** window, click your choice.
![](http://static.toastoven.net/prod_email/email_getting_0122_06.png)

- **Check and Send**: Check mail address of each recipient and body in the **Query Tagged Mail Delivery** tab before sending mails. Keep note that mail cannot be sent unless user delivers.
- **Immediately Send**: Send mails immediately without checking receiving information. You can find delivery result in the Query Tagged Mail Delivery tab.

### Upload files to Send Mass Mails  

You can use files to send mass mails.

Select **Not Use** for **Template Use** in the **Send Mails** tab, and select **General Mails** for **Mail Type**.
![](http://static.toastoven.net/prod_email/email_getting_0622_2_07.jpg)

- **Mails to Send**: Enter an email address to send. Write in the name <emailAddress> format to enter sender name and address.
- **Scheduled Delivery**: Specify the date and time of email delivery.
- **Attachment**: Attach files to mail.
  - Up to five files can be uploaded, not exceeding 10MB.
  - The total volume of attachment cannot be more than 10MB.
- **Title**/**Body**: Enter the title and body of the mail. If the title and body of mail is entered in the ##replacement key##format, the mail can be replaced with user specified data. For instance, enter ##name## in the user name, and it is replaced with a user specified  name, such as 'Hana Oh' before sent.

Go to **Send Mass Mails** at the bottom and click **Download Template**.
![](http://static.toastoven.net/prod_email/email_getting_0622_2_08.jpg)

Templates are available either in CSV or Excel.
![](http://static.toastoven.net/prod_email/email_getting_0622_2_09.jpg)

Enter replacement data in the template file, such as recipient's mail address and recipient name.
![](http://static.toastoven.net/prod_email/email_getting_0622_2_10.jpg)

Click **Upload Recipient Files** to upload files.
Errors, if any in template data, will show: check and correct them.
The total number (no more than 10) with detail information shows.

And, the file containing template data can be uploaded with 10 thousand persons, up to 3MB.  
![](http://static.toastoven.net/prod_email/email_getting_0622_2_11.jpg)

**Error Types**

  - When the receive_mail_address row does not exist: The receive_mail_address row is for recipient's mail address and is required.  
  - When there is no entered data available
  -  When recipient's mail address or replacement data is missing

Click **Schedule Delivery** at the bottom right of the mail page.
On the **Schedule Delivery** window, click your choice.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_12.jpg)

  - Check and Send**: Check mail address of each recipient and body in the **Query Tagged Mail Delivery** tab before sending mails. Keep note that mail cannot be sent unless user delivers.
- **Immediately Send**: Send mails immediately without checking receiving information. You can find delivery result in the Query Tagged Mail Delivery tab.

### Query Delivery of Mass Mails
In the **Query Delivery of Mass Mails** tab, you can retrieve scheduled mass mail delivery or cancel delivery.

  - Click **Send** to send mails: may take quite a while, depending on the number of deliveries.
  - Click **Cancel** to cancel sending mails.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_13.jpg)

**Status of Mass Mails Delivery**

  - Waiting: Recipient file data is still not read.
  - Preparing Delivery: Recipient file data is being uploaded.
  - Preparing Delivery Completed: Sending mail is completely prepared. Select a schedule (a column on the list) and check mail delivery details per recipient on the list at the bottom.
  - Ready for Delivery: It is ready to send mails.
  - Delivering: Mail is now on the delivery. Select a schedule (a column on the list) and check the delivery processing rate.
  - Delivery Completed: Delivery has been normally completed.
  - Delivery Failed: Error occurred during delivery. To check delivery status, query received mails.
  - Delivery Cancelled:  User has cancelled sending mails.

**Query Mail Delivery per Recipient**

Select a mass mail schedule (a column on the list) to query the list of mail deliver per recipient at the bottom.
![](http://static.toastoven.net/prod_email/email_getting_0622_2_14.jpg)

### Query Delivery of Tagged Mails
- In the **Query Delivery of Tagged Mails** tab, you can retrieve scheduled tagged mail delivery or cancel delivery.
  - Click **Send** to send mails: may take quite a while, depending on the number of deliveries.
  - Click **Cancel** to cancel sending mails.

![](http://static.toastoven.net/prod_email/email_getting_0122_07.png)

**Status of Tagged Mails Delivery**

- Waiting: Recipient file data is still not read.
- Preparing Delivery: Recipient file data is being uploaded.
- Preparing Delivery Completed: Sending mail is completely prepared. Select a schedule (a column on the list) and check mail delivery details per recipient on the list at the bottom.
- Ready for Delivery: It is ready to send mails.
- Delivering: Mail is now on the delivery. Select a schedule (a column on the list) and check the delivery processing rate.
- Delivery Completed: Delivery has been normally completed.
- Delivery Failed: Error occurred during delivery. To check delivery status, query received mails.
- Delivery Cancelled:  User has cancelled sending mails.

**Query Mail Delivery per Recipient**

Select a tagged mail schedule (a column on the list) to query the list of mail deliver per recipient at the bottom.
![](http://static.toastoven.net/prod_email/email_getting_0122_08.png)

### Manage Categories and Templates

Create categories, and classify and manage templates applied for mail delivery.

Click the **Manage Templates** tab and select category from **Categories** on the left, and click **Add Categories**. Make sure a category must be selected first, before clicking **Add Categories**.

When the window pops up for **Add Categories**, fill in the blanks and click **Add**.  

![](http://static.toastoven.net/prod_email/email_getting_0622_2_15.jpg)

To modify category, select a category to modify first and then click **Modify**. When the window pops up for **Modify Category**, fill in the blanks and click **Modify**.

To add a template, select a created category first and click **Add Templates**.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_16.jpg)

Write frequently used items and click **Add** to register a template.

To modify a template, select a template to modify, modify it, and then click **Modify**.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_17.jpg)

### Manage Tags

You can create, modify, or delete tags applied to send mails in the **Manage Tags** tab.
Click a tag and the list of UIDs registered to the tag can be retrieved.

When there is no registered tag, click **Register Tags**, enter **Tag Name** on the **Register/Modify Tags** window, and then click **Register**.

![](http://static.toastoven.net/prod_email/email_getting_0122_09.png)



![](http://static.toastoven.net/prod_email/email_getting_0122_11.png)

To register UIDs to a tag, select a tag from the list of registered tags and click **Register UIDs**.
Enter an UID on the **Register UIDs** window and click **Register**.

Up to 1,000 UIDs can be registered at once, for a tag.

![](http://static.toastoven.net/prod_email/email_getting_0122_10.png)


### Manage UIDs

Go to the **Manage UIDs** tab to manage UIDs and mail addresses.
<br>UIDs, registered to a tag, can be used to easily send mails to many recipients.

![](http://static.toastoven.net/prod_email/email_getting_0122_12.png)

Click **Register UIDs** and you can enter many UIDs and mail address in CSV.

![](http://static.toastoven.net/prod_email/email_getting_0122_13.png)

Download a template, and enter UID on the first row, and mail address on the second row.
<br>Save the files as entered and upload them: then, registration is completed.

![](http://static.toastoven.net/prod_email/email_getting_0122_14.png)

### Query per Mail Request

You can query sent mails by Date and Time of Delivery, Date and Time of Receipt, Template, and Delivery Status on **Query per Mail Request**.  

![](http://static.toastoven.net/prod_email/email_getting_0622_2_18.jpg)

Click a mail on the list to check its details.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_19.jpg)

### Manage Rejections

You can manage rejecting users on the **Manage Rejections** tab.
You may query users who have rejected so far, and register or delete rejections.
![](http://static.toastoven.net/prod_email/email_getting_0622_2_26.jpg)

#### Enter Rejections for Registration

Click **Register Rejections**.
![](http://static.toastoven.net/prod_email/email_getting_0622_2_27.jpg)

Enter mail address rejecting receiving mails on the **Register Rejections** window.  
Click **Add** to register up to 10 persons.  
![](http://static.toastoven.net/prod_email/email_getting_0622_2_28.jpg)

Click **Register** and check the list of registration.
![](http://static.toastoven.net/prod_email/email_getting_0622_2_29.jpg)

#### Upload Rejection Files for Registration  

You can upload rejection files in CSV, text (.txt), or excel (.xlsx).
Click **Register Mail Addresses in a Bundle** to upload files.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_30.jpg)

**CSV Files for Uploads**

![](http://static.toastoven.net/prod_email/email_getting_0622_2_31.jpg)

Complete uploads and click **Register**.
When they're normally uploaded, the message will show **A total of n files have been found**.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_32.jpg)

When it is fully uploaded, you can check the list on the **Manage Rejections** tab.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_33.jpg)

#### Delete Rejections

Select a rejection to delete, and click **Delete Rejections**.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_34.jpg)

Check the message of the **Undo Rejection** window and click **Yes**.  

![](http://static.toastoven.net/prod_email/email_getting_0622_2_35.jpg)

When it is completely deleted, you can check it on the **Manage Rejections** tab.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_36.jpg)

### Query Statistical Data

Check delivery statistics on the **Query Statistics** tab.  
You can retrieve by conditions, such as period, statistical type (date, time, or day), mail type (general or mass), or template.


**By Date**
![](http://static.toastoven.net/prod_email/email_getting_0622_2_20.jpg)


**By Time Zone**
![](http://static.toastoven.net/prod_email/email_getting_0622_2_21.jpg)

**By Day**
![](http://static.toastoven.net/prod_email/email_getting_0622_2_22.jpg)

### Guide for Notice of Personal Information Assignor

When the Customer uses TOAST > Email Service, assignment of personal information between the Customer and the Company arises, and the assignee, the Customer, is obliged to disclose the status (assignor and content of business) of his assignment of personal information to the Company, through the personal information handling policy, in accordance with Act on Promotion of Information and Communications Network Utilization and Information Protection. Accordingly, the Company may provide guidelines as below for the Customer, to abide by relevant regulations in the use of TOAST Email Service and not to be adversely affected for not disclosing his assignment status:

(Example)

[Notice of Personal Information Assignor] To use TOAST Email Service, make sure the following is displayed for 'Personal Information Handling Policy' > Assignment Status of the Customer.  

Assignor: NHN Entertainment<br>
Content of Business: Send emails in lieu of customers<br>
