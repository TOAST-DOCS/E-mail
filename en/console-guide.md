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

* Up to 10 files can be uploaded, not exceeding 30MB.
* The total volume of attachment cannot be more than 30MB.
* Files can be attached up to 30MB, but they may be rejected due to 'Limit Exceeded' or regarded as spams, depending on the mail system.

**Title/Body**: Enter the title and body of the mail.

![email_02_201812](https://static.toastoven.net/prod_email/email_02_201904_en.png)

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

Following keys are provided as part of rejection links: 

| Key | Phrase|  Example of Usage |
| - | - | - |
| EN_BLOCK_RECEIVER_LINK | [Unsubscription](#) | If you no longer wish to receive these emails, please click the ##EN_BLOCK_RECEIVER_LINK##. |
| BLOCK_RECEIVER_LINK | - | If you no longer wish to receive these emails, please `<a href='##BLOCK_RECEIVER_LINK##' target='_blank'>click here</a>`. |

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

## 메일 도메인 관리

**메일 도메인 관리** 탭에서 발송 메일 도메인을 관리할 수 있습니다.
발송 메일의 도메인이 개인 혹은 회사에서 소유한 도메인 경우, 제 3자에 의해 악용될 수 있는 문제점을 보완할 수 있는 기능입니다.

* 이메일 서비스를 이용하기 위해 필수로 진행해야 하는 부분은 아닙니다.
* 개인 혹은 회사에서 소유한 도메인이 아닌 경우 사용이 불가능합니다.
    * 다음, 네이버, Google 등의 메일 계정 발급 서비스를 제공하는 업체의 도메인은 사용할 수 없습니다.

### 메일 도메인 등록
**메인 도메인 등록** 버튼을 클릭해 발송 메일에서 사용할 도메인을 등록할 수 있습니다.
도메인 등록은 루트 도메인만 등록 가능합니다.

### 메일 도메인 인증
**인증하기** 버튼을 클릭해 도메인 소유권을 인증할 수 있습니다.
도메인 등록 시 발급받은 토큰값을 도메인의 TXT 레코드에 등록한 후 **인증하기** 버튼을 클릭합니다.<br>

* 도메인의 TXT 레코드에 "toast-domain-verification={토큰}"값이 존재하지 않거나 일치하지 않으면 인증에 실패합니다.
* 도메인에 TXT 레코드를 등록한 후 변경 사항이 전파되기까지는 시간이 걸릴 수 있습니다.

### 메일 도메인 보호
**보호하기** 버튼을 클릭해 도메인을 보호할 수 있습니다.
도메인을 보호하면 아래 사항들이 적용됩니다.<br>

* 보호된 도메인은 이미 인증받은 프로젝트에서만 사용할 수 있습니다.
    * 동일한 도메인을 인증받은 프로젝트가 여러 개일 때, 그중 하나의 프로젝트에만 보호 적용을 해도 인증받은 프로젝트는 모두 도메인을 사용할 수 있습니다.
    * 예) A, B 프로젝트의 이메일 서비스에서 "toast.com" 도메인을 인증받은 후, A 프로젝트에서 보호 적용을 진행
        * A, B 프로젝트에서만 "toast.com" 도메인을 발송 주소로 사용 가능
        * C 프로젝트에서는 "toast.com" 도메인을 인증받지 않았으므로 "toast.com" 도메인을 발송 주소로 사용할 수 없음

### 메일 도메인 공유
**공유 > 설정** 버튼을 클릭하면 도메인을 공유할 수 있습니다.
주요 기능은 다음과 같습니다.<br>

* 도메인을 인증받은 프로젝트 조회
    * 특정 도메인의 인증 절차를 완료한 TOAST 프로젝트 목록을 제공합니다.
* 도메인을 다른 프로젝트로 공유
    * 프로젝트 여러 개에 동일한 도메인을 인증할 때 인증 과정을 여러 번 진행하지 않아도 됩니다.<br>
    * 한 프로젝트에서 인증받은 후 공유 기능을 활용해 다른 프로젝트의 이메일 서비스 앱키를 입력하면 도메인 공유가 가능합니다.

### 서브 도메인 등록
**메인 서브 도메인 > 설정** 버튼을 클릭해 서브 도메인을 등록할 수 있습니다.
등록된 루트 도메인의 서브 도메인만 등록할 수 있습니다.
등록된 서브 도메인은 메일 발송 시 **사용자 지정 헤더(custom header)** 기능으로 이용할 수 있습니다.

* 서브 도메인을 사용할 수 있는 사용자 지정 헤더(custom header)
    * 헤더명: X-TC-ENVELOPE-FROM
    * 헤더값: 서브 도메인
    * 주요 기능은 다음과 같습니다.
        * 수신자의 메일 서비스에서 SPF 레코드를 검사할 도메인으로 활용
            * 예) 받는 사람: test@toast.com, X-TC-ENVELOPE-FROM: test@cs.toast.com으로 메일 발송을 요청한 경우
                * 수신자의 메일 서비스에서 "cs.toast.com" 도메인으로 SPF 레코드 검사를 진행
        * 수신자의 메일 서비스에서 반송 메일을 보내줄 메일 주소로 활용
            * 예) 받는 사람: test@toast.com, X-TC-ENVELOPE-FROM: test@cs.toast.com으로 메일 발송을 요청한 경우
                * 수신 측 메일 서비스에서 test@cs.toast.com 메일 주소로 반송 메일 발송

* 보다 자세한 내용은 [사용자 지정 헤더(custom header)](https://docs.toast.com/ko/Notification/Email/ko/Overview/#custom-header) 가이드를 참고 부탁드립니다.

<span id="dkim"></span>
### DKIM
DKIM(Domain Keys Identified Mail)이란 발신자 및 메일 내용 등이 위변조되지 않았는지 수신자 측에서 검증할 수 있는 기술입니다.
**DKIM > 설정** 버튼을 클릭하면 DKIM 기능을 이용할 수 있습니다.
주요 기능은 다음과 같습니다.<br>


* DKIM TXT 레코드 등록 및 인증
    * **DKIM 인증** 탭을 클릭합니다.
    * 발급받은 TXT 레코드값을 도메인의 TXT 레코드에 등록한 후 **인증** 버튼을 클릭합니다.
    * 도메인의 TXT 레코드에 "v=DKIM1;k=rsa;p={발급된 RSA publicKey 값}"값이 존재하지 않거나 일치하지 않으면 인증에 실패합니다.
    * 도메인에 TXT 레코드를 등록한 후 변경 사항이 전파되기까지는 시간이 걸릴 수 있습니다.
* DKIM 기능 활성화 또는 비활성화
    * **DKIM 기능** 탭을 클릭합니다.
    * **활성화** 또는 **비활성화** 버튼을 클릭하여 DKIM 기능을 제어할 수 있습니다.<br>

* DKIM 기능이 정상적으로 동작하는지 확인하는 방법은 다음과 같습니다.
    * 특정 발송 도메인에 대해 DKIM 인증 및 활성화
    * 특정 발송 도메인으로 이메일 발송
    * 수신받은 메일의 원본 보기
    * Authentication-Results 헤더값에 'dkim=pass'이 포함돼 있는지 확인
        * 메일 서비스 제공 업체별로 dkim=pass 정보를 나타내는 헤더는 다를 수 있습니다.


## Manage Tags

You can create, modify, or delete tags applied to send mails in the **Manage Tags** tab.
Click a tag and the list of UIDs registered to the tag can be retrieved.

![email_15_201812_en.png](https://static.toastoven.net/prod_email/email_15_201812_en.png)

When there is no registered tag, click **Register Tags**, enter **Tag Name** on the **Register/Modify Tags** window, and then click **Register**.

To register UIDs to a tag, select a tag from the list of registered tags and click **Register UIDs**.
Enter an UID on the **Register UIDs** window and click **Register**.

Up to 1,000 UIDs can be registered at once, for a tag.

## Manage UIDs

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

Assignor: NHN
Content of Business: Send emails in lieu of customers
