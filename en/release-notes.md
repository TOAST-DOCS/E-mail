## Notification > Email > Release Notes

### 2025. 03. 11

#### 기능 추가

* [API] 메일 발송 업데이트 완료 목록 조회 API 추가
    * Email 서비스에서 발송 처리가 완료된 메일의 발송 상태를 조회할 수 있는 API가 추가되었습니다.
    * 해당 API는 메시지 발송 결과 업데이트 시간 기준으로 검색됩니다.
    * 발송 결과가 업데이트되는 기준은 다음과 같습니다.
        * 메일이 수신 완료될 경우 (수신 SMTP에서 수신 완료 응답을 받은 경우)
        * 메일 발송 처리간 발송 실패 처리된 경우(수신 거부, 화이트리스트 등)
* [CONSOLE] 반송메일 설정 옵션 기능 추가
    * 반송메일 설정 옵션을 추가하여 반송메일 수신 여부를 설정할 수 있습니다.
    * no-reply, noreply를 메일 주소로 설정하면 설정과 관련없이 반송메일을 수신하지 않습니다.
* [API] v2.1 광고 메일 발송/인증 메일 발송 API senderGroupingKey 추가
    * 광고 메일 발송/인증 메일 발송 API에 senderGroupingKey를 추가하여 발신자 그룹키를 설정할 수 있습니다.

#### 기능 개선/변경

* [API] 메일 읽음 이벤트 처리 오류 수정
    * 일부 상황에서 메일 발송이 완료되지 않은 상태에서 읽음 처리되는 문제를 개선하였습니다.
* [CONSOLE] Freemarker 템플릿 미리보기 기능 개선
    * Freemarker 템플릿 미리보기시 템플릿 파라미터를 전체 적용하지 않을 경우 적용하지 않은 파라미터를 확인할 수 있도록 개선하였습니다.
* [API] 템플릿 발송 유효성 개선
    * 템플릿 발송 시 템플릿 파라미터가 누락된 경우에도 템플릿 원문이 발송되는 문제를 개선하였습니다.
    * 템플릿 발송 시 템플릿 파라미터가 누락된 경우 발송이 실패처리 됩니다.
* [API] 웹훅 발송 상태 추가
    * 웹훅이 발송되는 발송 상태 코드 조건이 추가되었습니다.
    * 추가되는 발송 상태 코드는 다음과 같습니다.
        * SST3: 발송 실패
        * SST4: 수신 거부
        * SST7: 미인증
        * SST8: 발송 실패(화이트리스트로 인한 필터링)

### August 27, 2024

#### Feature Updates

* [Console] Improved statistical event handling
    * Improved the duplicate processing of statistical data during email sending.
    * Categorized the 'Receive' events into 'Received' and 'Failed to receive'.
        * ‘Received' event is when email has been successfully delivered to the recipient.
        * ‘Failed to receive’ event is when email is not delivered to the recipient (e.g., Soft Bounce, Hard Bounce).
* [Console] Improved template preview feature.
    * Improved preview feature for FreeMarker templates.
    * When applying template parameters, if some parameters are not applied, they appear as empty values and preview is
      available.
* [Console] Added error codes for failed DNS queries
    * Added new error codes for DNS queries that fail during Domain Ownership Authentication, SPF Authentication, DKIM
      Authentication, and DMARC Authentication to provide a clearer indication of the cause.
    * This is to help developers and operators quickly diagnose and respond to DNS-related issues by better providing
      the information needed for troubleshooting.
    * Major changes
        * Error code: -2726
        * Error message: DNS LookUp failed. lookup failed message: {}
            * {} contains the error message that occurred during the DNS query, so you can determine the specific reason
              for the failure. For example, it could be a domain misspelling, a network error, or a text record
              misspelling.
    * You can see the error code in real time through the developer tools.

### July 23, 2024

#### Feature Updates

* [Console] Changed the masking policy
    * Changed the email address masking policy to minimise the exposure of personal information
    * If the local part is digit, it is fully masked (a@nhn.com → *@nhn.com)
    * If the local part is 3 digits or less, only 1 digit is exposed (aaa@nhn.com → a**@nhn.com)
    * If the local part is 4 digits or more, only 2 digits are exposed (aaaa@nhn.com → aa**@nhn.com)

### June 25, 2024

#### Feature Updates

* [API] Changed the template attachment upload policy
    * Changed the policy to allow for registration of multiple templates when uploading template attachments
    * Previously, if you uploaded a file and then attached it to a template, you could not attach it to another template
    * You can not attach the same file to multiple templates.

* [API/CONSOLE] Changed the domain management policy
    * Changed so that protection is automatically enabled when a domain is verified.
    * When domain protection is enabled, outgoing requests are restricted for projects that have not verified or shared
      the domain
    * Domain protection is automatically enabled for domains that are already verified.
    * To turn off domain protection, you can turn it off from the `Manage Domains` screen.

### May 28, 2024

#### Feature Updates

* [API/Console] Changed the maximum days for scheduled delivery
    * Changed the maximum days for scheduled delivery from 30 days to 60 days.
    * On the scheduled delivery and lookup screens, you can set a scheduled delivery time of up to 60 days.

### March 26, 2024

#### Added Features

* [Console] Added the feature to manage templates - Modify Category/Template
    * Added the feature to modify templates and categories, allowing you to modify selected items.
    * You can select a category or template on `Manage Template` screen and drag it to any desired categories.

#### Feature Updates

* [API] API version update v2.1
    * Changed Query API Response
        * Added statistics event keys to responses from the query mail API.
    * Added API to query by statistical event key.
        * Added a summation API to sum queried statistical data.
* [API] Strengthen SPF validation logic
    * SPF validation logic is to be strengthened.
        * SPF validation fails if it is looked up more than 10 times.
        * SPF validation fails if `~all` direction exists in the middle of spf records.
        * If more than two SPF record exists, SPF validation fails.
* [Console] Changed the file upload limit for mass mail recipients
    * The limits on uploading recipient files have been changed. The maximum limit on recipients will now disappear.
        * Previous: Mass mail recipient files can upload up to 500,000 people and up to 30 MB.
        * Current: Mass mail recipient files can be uploaded up to 30 MB.

### February 27, 2024

#### Added Features

* [API] Added StatsId field (v2.0 API)
    * Added the StatsId field to mail delivery API request parameters for statistical classification.
* [Console]
    * Renewed the Query Statistics screen
        * Changed the existing Query Statistics menu to (Old) Query Statistics.
        * Added a menu to view statistics by event occurrence time.
    * Added statistics event key settings menu
        * Added a menu to add a StatsId for use in the API and console.
* [Console] Role segmentation
    * Added the feature to grant separate Email menu access and feature control permissions based on role.
    * For more information, see the [Console User Guide](https://docs.nhncloud.com/en/nhncloud/en/console-guide/#_24).

#### Feature Updates

* [Console] Changed conditions for the mail delivery query
    * Delivery query condition changes from `Received or not` to `Receive Type`.
    * The conditions change from "All", "Received", "Not Received" to "All", "Success", "Failed-(Soft Bounce)", "
      Failed-(Hard Bounce)".
    * This feature applies to the **Retrieve by Mail Request**, **Retrieve Scheduled Mail Delivery**, **Retrieve Bulk
      Mail Delivery**, **Retrieve Tagged Mail
      Delivery** screens.

* [Console] Added SMTP response code field in mail delivery query
    * Added SMTP response code field to mail delivery query
    * SMTP response codes provide response codes for errors that occurred when sending mail.
    * This feature applies to the **Retrieve by Mail Request**, **Retrieve Scheduled Mail Delivery**, **Retrieve Bulk
      Mail Delivery**, **Retrieve Tagged Mail
      Delivery** screens.

### Jan 31, 2024.

#### Added Features

* [Console] Added mail sending status
    - Added "Authentication failed (SST7)" to mail sending status.
    - On February 1, 2024, changes
      to [Gmail email sender guidelines](https://support.google.com/mail/answer/81126?hl=ko#requirements-5k)will result
      in sending
      being restricted if you don't perform all three SPF, DKIM, and DMARC authentication.
    - If the sending is restricted, the sending status will be 'Authentication failed (SST7)'.

#### Feature Updates

* [API/SMTP] Respond to changes to Gmail email sender guidelines
    * On February 1, 2024, changes
      to [Gmail email sender guidelines](https://support.google.com/mail/answer/81126?hl=ko#requirements-5k)will result
      in sending
      being restricted if you don't perform all three SPF, DKIM, and DMARC authentication.
    * When sending to an unauthenticated sending domain, the sending status will be 'Authentication failed (SST7)'.

* [Console] Fixed an issue where re-authentication of SPF records and DMARC records are required when sharing projects
    * Fixed an issue where re-authentication of SPF records and DMARC records are required when sharing projects.
    * Authentication values are not initialized when sharing an authenticated domain.

* [API] Improved SPF record authentication errors
    * Fixed an issue where authentication would be performed if more than one SPF record is registered for a domain.
    * If multiple SPF records exist, it can be considered a misconfiguration by the DNS system.
    * So if you have more than one SPF record, the authentication failed status appears.
    * For more information, see [RFC 4408](https://www.ietf.org/rfc/rfc4408.txt).

### December 19, 2023

#### Added Features

* [Console] Added DMARC authentication feature
    - DMARC authentication procedure is added.
    - On February 1, 2024, changes
      to [Gmail email sender guidelines](https://support.google.com/mail/answer/81126?hl=ko#requirements-5k) will result
      in sending
      being restricted if you don't perform all three SPF, DKIM, and DMARC authentication.

### October 17, 2023.

#### Feature Updates

* [Console] Settings for mail retrieve defaults
    * Added the feature to reset the delivery/request/schedule date fields upon entering the mail retrieve screen.
    * This feature applies to the “Retrieve by Mail Request”, “Retrieve Scheduled Mail Delivery”, “Retrieve Bulk Mail
      Delivery”, and “Retrieve Tagged Mail
      Delivery” screens.

### August 29, 2023

#### Feature Updates

* [Console] Duplicate requests prevention when requesting for mass delivery
    * Improved to reject duplicate requests for mass delivery.

#### Added Features

* [Console] Added the feature to backup out-of-date data
    * Added settings for backing up data that has passed its retention period.
    * The body of the mail is not included in the save target.

### August 29, 2023

#### Feature Updates

* [Console] Prevention of duplicate requests upon mass
    * Improved to reject duplicate requests for mass delivery.

### Added Features

* [Console] Backup data past the retention period
    * Added a feature to backup data past the retention period.
    * The mail body is not included in the save target.

### July 25, 2023

#### Feature Updates

* [Console] Changed mail domain management screen
    * Deleted the Date and Time of Verification, Date and Time of Protection, Date and Time of Check SPF Record field.
    * You can check the deleted fields as a tooltip by hovering over the **Verified or Not, Protected or not, SPF Record
      or Not** respectively.
* [Console] Added additional information in Retrieve by Mail Request → Request to download search results
    * Added Mail Sequence, Template ID, DSN Message information to the request file content for mail delivery detail
      queries.

### June 27, 2023

#### Feature Updates

* [Console] Improved template preview screen
    * Improved the Preview template screen when selecting a template in mail delivery.
    * Moved the template content that displayed in the body field, when selecting a template, to the Preview screen.

### May 30, 2023

#### Feature Updates

* [API] Added retention period validty of attachments
    * Improved from failing at send time after a successful mail request with an expired file attachment to failing at
      request time when sending.

#### Bug Fixes

* [Console] Fixes some CSS conflicts when registering email templates
    * Fixed an issue where, when registering email templates, CSS conflicts of some tags cause an incorrect display in
      the preview.

### April 25, 2023

#### Feature Updates

* [API/SMTP] Changed domain
    * Changed the domain for the API/SMTP interface from cloud.toast.com to nhncloudservice.com.
        * API: api-mail.cloud.toast.com → email.api.nhncloudservice.com
        * SMTP: smtp-mail.cloud.toast.com → smtp-mail.nhncloudservice.com
    * When using the API/SMTP interface, you must change the domain. If you keep using the existing domain, delivery may
      be restricted in the future.

#### Bug Fixes

* [Console] Fixed an issue where common.code.null appears when searching for recipients of mass delivery
    * Fixed an issue where, when searching the list of recipients by mail after mass delivery, common.code.null appears
      in some status code.

#### Added Features

* [Console] Added the Cancel button for mass delivery using scheduled delivery
    * Added a button to cancel mass delivery using scheduled delivery.

### March 28, 2023

* [Console] Changed the date limit on query
    * Changed the date limit from 30 days to 31 days for mass delivery queries, tagged mailing queries, and queries by
      regular mail recipient address.
* Improved to select file extensions when downloading requested files
    * CSV, XLSX
* Improved display of mass delivery results
    * Changed from displaying successful delivery to displaying delivery failure if there is no data sent normally for
      mass delivery.

### October 25, 2022

#### Bug Fixes

* [SMTP] Fixed an issue where, when both text/plain and text/HTML messages are included, only the text/pain message are
  sent

### February 22, 2022

#### Feature Updates

* [Console] Added blind carbon copy (BCC) feature

### January 25, 2022

#### Feature Updates

* [SMTP] Added SMTP interface feature
    * You can send mail via SMTP interface.
    * For more details, refer to [SMTP Guide](./smtp-guide).

### September 14, 2021

#### Feature Updates

* [API] API Version Updated to v2.0
    * Added secret key authentication
        * Added secret key authentication in API authentication.
        * For more details, refer to [Secret Key](./api-guide/#secret-key).
    * Added Retrieve Bulk Main Delivery API
        * Added an API to retrieve bulk mail deliveries.
* [Console/API] Restricted allowed characters for template ID
    * Improved not to use &, %, ", ' for a template ID.

### March 23, 2021

#### Feature Updates

* [Console/API] Added restriction to allowed characters for template ID
    * Improved not to use '/', '?', ':' for a template ID.

### October 27, 2022

#### Feature Updates

* [Console] Added a feature to cancel bulk mail delivery
    * Added a feature to cancel mail that is being sent on the **Retrieve Mass Mail Delivery** tab.

### September 22, 2020

#### Feature Updates

[Console] Added webhook feature

* Added [Manage Webhook] menu.
* When a specific event occurs in the Email service, create POST request with the URL specified by the webhook settings.
* Event types that are currently supported are as follows.
* Register recipient addresses that reject advertising mails
* When a recipient rejects an advertising mail by using a link included in the mail, the webhook feature starts to work.
* For more details, refer to [Manage Webhook](./console-guide/#webhook).

### August 25, 2020

#### Feature Updates

* [Console] Added DSN information when querying email delivery details
* [Console] Added DSN information within CSV file when downloading file for a detail mail delivery case
* [API] Updated API to v1.7
    * Added the response field of Query Mail Delivery Details API
        * Find mail status sent via SMTP from the field on DSN information
            * DSN Code, DSN Message
            * [DSN(Delivery Status Notification)](https://tools.ietf.org/html/rfc3463)

### June 23, 2020

#### Feature Updates

* [Console] Added Cancellation for Query Scheduled Delivery
    * On the **Query Scheduled Delivery** tab, the feature of cancelling scheduled emails before sent has been added.
* [API] Added Query/Cancel Scheduled Delivery
    * Query or cancel scheduled delivery are now available via APIs.

### April 28, 2020

#### Feature Updates

* [Console] Added DKIM
    * The features of DomainKeys Identified Mail have been added to check if receiving email is forged.
    * For more details, see [DKIM Guide](./console-guide/#dkim).

### March 24, 2020

#### Feature Updates

* [Console] Added Mail Domain Management Tab
    - Main features are as follows:
        - Manage Mail Domains
        - Added features to register, verify, delete, and share mail domains.
        - Protect Mail Domains
        - Verified mail domains can be protected from third-party usage.

### January 21, 2020

#### Feature Updates

* [Console] Downloading is available within the **Query Mass Mail Delivery** tab
    * You can download the list of query results for mass mail delivery in the mass delivery template.

### November 26, 2019

#### Feature Updates

* [API] API Version Updated to v1.6
* [API] User-Input Data Supported for Templates
    * When you use a template, user-input sender information, title, and body text are preferred for application than
      template data.
    * For more details, see [Send Mails API](./api-guide/#_1)

### October 29, 2019

#### Feature Updates

* [Console] Added the Feature of Individual Delivery
    * For general delivery, emails can be individually sent to each recipient.

### August 27, 2019

#### Feature Updates

* [API] API version updated to v1.5
* [API] Added Features for Sender's Group Key
    * Send mails, when requested, by specifying sender's group key, which helps to query request.
    * For more details, see [Send Mail API](./api-guide/#_1) and [Query Mail API](./api-guide/#_25).
* [API] Added Receive or Not Field
    * Query request details to see if the mail has been received, as well as received time.
    * For more details, see [Query Mail Delivery Details API](./api-guide/#_29).

### July 23, 2019

#### Feature Updates

* [API] Added Manage Templates API
    * Register, Modify, and Delete Templates are available on APIs.
    * For more details, see [Manage Templates API](./api-guide/#template).
* [API] Added Manage Category API
    * Use categories to classify and manage templates. You may specify categories to be included when a template is
      created.
    * Register, Modify, Delete, and Query Categories are available on APIs.
    * For more details, see [Manage Category API](./api-guide/#category).

### 2019. 01. 29.

#### 기능 추가

* [Console/API] 템플릿 아이디 길이 증가
    * 템플릿 아이디 길이 제한이 기존 10글자에서 50글자로 변경되었습니다.

### 2018. 12. 18.

#### 기능 추가

* [Console] 템플릿 엔진 기능
    * 제목과 내용에 FreeMarker 템플릿 언어(FreeMarker Template Language)를 활용하여 템플릿을 등록할 수 있습니다.
    * 템플릿 언어를 사용한 템플릿을 사용하여 메일을 발송할 수 있습니다. 대량 메일 발송인 경우, 엑셀 파일에 템플릿 파라미터를 입력하여 치환할 수 있습니다.

### 2018. 11. 27.

#### 기능 추가

* [API] API 버전 업데이트 v1.4
* [API] 템플릿 등록하지 않고 치환 기능 제공
    * 미리 템플릿을 등록하지 않고 발송 요청 내에서 템플릿 파라미터를 활용하여 치환 기능을 사용할 수 있습니다.
* [API] 템플릿 엔진 기능
    * FreeMarker 템플릿 언어(FreeMarker Template Language)과 템플릿 파라미터를 활용하여 메일을 발송할 수 있습니다.
    * 자세한 내용은 [제목/내용 치환](./api-guide/#_21)을 참고해 주세요.

### 2018. 10. 23.

#### 기능 추가

* [API] API 버전 업데이트 v1.3
* [API] 일반, 광고 메일 발송 API 응답 변경
    * API v1.3부터 지원합니다.
    * 발송하는 수신자 요청 중 일부만 유효해도 발송을 요청할 수 있습니다. (이전 버전은 1개의 수신자 요청이라도 유효하지 않다면 실패로 응답합니다.)
    * 메일 발송 API 응답에 각 수신자에 대한 결과를 반환합니다. 사용자는 응답으로부터 성공한 수신자 정보와 실패한 수신자 정보를 확인할 수 있습니다.
    * 자세한 내용은 [일반 메일 발송 API 응답](./api-guide-v1.3/#_4)과 [개별 메일 발송 API 응답](./api-guide-v1.3/#_7)을 참고해주세요.
* [API] 통계 조회 API에 광고 여부 조건 추가
    * 광고 메일과 광고가 아닌 메일을 구분하여 통계를 확인할 수 있습니다.
    * 자세한 내용은 [통합 통계 조회 API](./api-guide-v1.3/#_83)을 참고해주세요.

### 2018. 09. 18.

#### 기능 추가

* [API] 인증 메일 발송 API 추가
    * 메일 인증에 사용할 수 있는 인증 메일 발송 API를 제공합니다.
    * 인증 메일 성격상 단건 발송만 가능하며, 첨부 파일을 지원하지 않습니다.
    * 자세한 내용은 [인증 메일 발송 API](./api-guide-v1.2/#_11)를 참고해주세요.

### 2018. 08. 28.

#### 기능 추가

* [API] API 버전 업데이트 v1.2
* [API] 첨부 파일을 재사용할 수 있도록 구조 변경
    * 첨부 파일을 한 번 발송하면 다시 동일한 ID로 발송할 수 있도록 구조를 개선하였습니다.
    * [첨부 파일 업로드 API v1.2](./api-guide-v1.2/#_18)를 사용하여 업로드한 첨부 파일은 v1.2 발송 API 사용하여 메일을 발송 시 첨부 파일을 재사용할 수 있습니다.
    * Toast Email에서 제공하는 모든 발송 API v1.2에서 사용할 수 있습니다.
    * 버전이 업데이트된 API는 [API 가이드](./api-guide-v1.2/)의 각각 API 명세를 참고해주세요.
* [API] 메일 주소 유효성 검사 강화
    * 메일 ID와 도메인 형식이 잘못된 메일 주소를 요청할 경우 API에서 에러를 반환합니다.
    * Toast Email에서 제공하는 모든 발송 API에 적용하였습니다.

### 2018. 06. 26.

#### 기능 추가

* [API] 사용자 지정 헤더 기능 추가
    * 수신 이메일에 사용자 지정 헤더를 추가하여 발송할 수 있습니다.
    * Toast Email에서 제공하는 모든 발송 API에 추가하여 사용할 수 있습니다.
    * 사용자 지정 헤더를 지원하는 메일 상세 조회 API v1.1이 추가되었습니다.
    * 자세한 내용은 [사용자 지정 헤더 (Custom header)(./console-guide/#custom-header)를 참고해주세요.
    * 버전이 업데이트된 API는 [메일 발송 상세 조회](./api-guide-v1.1/#_26)와 [태그 메일 발송 상세 조회](./api-guide-v1.1/#_35)를 참고해주세요.
* [API] 숨은 참조 기능 추가
    * 일반 메일 발송 API에 발송수신자 타입이 추가되어 숨은 참조로 발송할 수 있습니다.

### 2018. 04. 24.

#### 기능 추가

* [Console] 대량 발송 수신자 엑셀/CSV 업로드 파일 크기 제한
    * 대량 메일 수신자 파일은 최대 1만 명, 그리고 3MB 까지 업로드할 수 있습니다.

### 2018. 03. 22.

#### 기능 추가

* [Console] 탭 UI 변경

### 2018. 02. 22.

#### 기능 추가

* [Console] 태그 메일 발송 기능 추가
    * 태그와 UID를 등록하여 다수의 사용자를 대상으로 쉽게 메일을 발송할 수 있습니다.
    * 수신자 정보 대신 태그를 선택하여 발송할 수 있는 기능 제공합니다.
    * 자세한 내용은 [태그를 사용한 메일발송](./console-guide/#_6)를 참고해주세요.
* [Console] 태그 메일 발송 내역 조회 화면 추가
    * 태그를 사용하여 발송한 메일을 조회할 수 있는 화면을 제공합니다.
    * 요청 정보, 수신자 정보, 그리고 발송한 메일을 상세 정보 조회가 가능합니다.
    * 자세한 내용은 [태그 메일 발송 조회 탭](./console-guide/#_9)를 참고해주세요.
* [Console] 태그, UID 관리 기능 추가
    * 태그 메일 발송에 사용되는 태그와 UID에 대한 생성, 수정, 삭제 기능을 제공합니다.
    * 자세한 내용은 [태그 관리](./console-guide/#_11), [UID 관리](./console-guide/#uid)를 참고해주세요.
* [API] 태그 메일 발송, 조회 기능 추가
    * 태그 메일을 발송하고 조회할 수 있는 API를 제공합니다.
    * 자세한 내용은 [태그 메일발송](./api-guide-v1.0/#_29)를 참고해주세요.
* [API] 태그, UID 관리 기능 추가
    * 태그 메일 발송에 사용되는 태그와 UID를 생성, 수정, 삭제할 수 있는 API를 제공합니다.
    * 자세한 내용은 [태그 관리](./api-guide-v1.0/#_45), [UID 관리](./api-guide-v1.0/#uid)를 참고해주세요.

### 2017. 09. 21.

#### 기능 추가

* [Console] 통계 유형에 수신율 추가
    * 통계 그래프 및 표에서 수신율 확인이 가능합니다.
* [API] 통계 조회 API에 통계 유형 추가
    * 일별 통계, 월별 통계, 통합 통계에 각각 수신율 정보가 추가됩니다.
    * 자세한 사항은 [통계 조회](./api-guide-v1.0/#_72)를 참고해주세요.

### 2017. 08. 24.

#### 버그 수정

* [Console] 템플릿에서 이미지 가져올 시 비정상적으로 커지는 문제 수정
    * 템플릿 본문에 이미지 태그를 넣은 경우, Console에서 템플릿 선택하여 가져올 시 비정상적으로 커지는 문제가 수정되었습니다.

### 2017. 07. 20.

#### 기능 추가

* [Console] 수신거부 리스트 다운로드 기능 추가
    * 수신거부 리스트를 csv 형태로 다운로드가 가능합니다.
    * 다운로드 요청 파일 생성이 예약되고, 생성 후 일주일간 유지됩니다.

#### 기능 개선

* [Console] 첨부파일 업로드 시 확장자 체크
    * 메일 발송, 템플릿 관리 시 첨부파일의 확장자가 제한됩니다.
    * 제한되는 확장자 : js,exe,bat,cmd,com,cpl,scr,vbs,wsf
* [API] 첨부파일 업로드 API 확장자 체크
    * API로 첨부파일 업로드 시에도 확장자를 체크합니다.

#### 버그 수정

* [Console] 대량 메일 발송 시 오픈 통계가 정상적으로 집계되지 않는 문제
    * 대량 메일 발송 시 비 정상적으로 오픈률이 증가하는 현상이 있어 수정되었습니다.

### 2017. 06. 22.

#### 기능 추가

* [Console] 광고성 메일 발송 기능 추가
    * 이제 이메일 상품에서 광고성 메일 발송이 가능합니다.
    * 광고성 메일 발송을 위한 기능을 아래와 같이 제공합니다.
        * 메일 제목에 "(광고)" 문구 추가
        * 메일 본문에 수신거부 링크 삽입
        * 수신 거부 링크 클릭 시 수신거부 할 수 있는 화면 제공
        * 수신 거부된 대상자들에게 광고성 메일 발송 차단하는 기능 제공
        * 수신 거부 대상자를 관리할 수 있는 화면 제공(다운로드 기능은 추후 제공 예정)
    * 자세항 사항은 [광고성 메일 발송](./console-guide/#_3)[수신 거부 관리](./console-guide/#_13)를 참고해주세요.
* [API] 광고성 메일 발송 기능 추가
    * 광고성 메일인 경우 별도로 제공되는 API를 사용할 수 있습니다.
    * 다음과 같은 기능을 제공합니다.
        * 제목에 "(광고)"가 필수로 들어가야 합니다.
        * 수신거부로 등록된 대상자에게는 메일 발송을 하지 않습니다.
* [API] 수신거부 관리 기능 추가
    * 수신 거부 유저를 조회/등록/삭제 할 수 있는 API를 제공합니다.
    * 자세항 사항은 [수신 거부 관리](./api-guide-v1.0/#_82)를 참고해주세요.
* [API] 통합 통계 조회 기능 추가
    * 날짜별/시간대별/요일별로 조회 할수 있는 API가 추가됩니다.
    * 자세항 사항은 [통합 통계 조회](./api-guide-v1.0/#_79)를 참고해주세요.

#### 기능 개선

* [Console]통계 화면 개선
    * 통계 화면이 개선되었습니다.
    * 이제 날짜별, 시간대별, 요일별로 조회가 가능합니다.
    * 자세항 사항은 [통계 정보 조회](./console-guide/#_17)를 참고해주세요.

### 2017. 05. 25.

#### 기능 추가

* [Console] 이메일 미리보기 기능 추가
    * 메일 발송 화면에서 발송 정보 및 본문 내용을 미리보기로 확인할 수 있습니다.

#### 버그 수정

* [Console] 대량 메일 예약 발송 이슈 수정
    * 현상 : 대량 메일 예약 발송 시 현재 월이 넘어가는 경우 조회/발송이 되지 않는 이슈가 있었습니다.
    * 해결 : 현재 월이 넘어가더라도 예약 발송이 가능하도록 수정되었습니다.(단, 예약 발송 가능 날짜는 3개월 이내 (5월에 등록 시 7월까지 예약 가능))

### 2017. 04. 20.

#### 기능 추가

* [Console] <b>수신자명<이메일주소></b> 형식으로 발송이 가능
    * 발신자 주소에만 사용할 수 있었던 <b>수신자명<이메일주소></b> 형식을 수신자 주소에서도 사용이 가능합니다.
* [API] 수신자명 지원
    * API에서도 수신자명 필드가 추가되었습니다. 자세한 사항은 [메일 발송 문서](./api-guide-v1.0/#_2)를 참고해주세요.
* [Console] 통계 화면 제공
    * 일별/월별 통계 화면이 제공됩니다.
    * 통계 정보는 2017년 4월 1일부터 수집됩니다.
* [API] 통계 조회 기능 제공
    * 일별/월별 통계 조회 API가 제공됩니다. 자세한 사항은 [통계 조회 문서](./api-guide-v1.0/#_72)를 참고해주세요.

#### 기능 개선

* [Console] 메일 발송 화면에 SPF 등록 안내 문구 추가
    * 메일 발송 화면에 SPF 등록 안내 문구가 추가되었습니다.
    * 자세한 내용은 메일 발송 > 발송메일 필드의 <b>SPF 안내</b> 부분을 확인해주세요.

### 2017. 03. 23.

#### DB 작업

* DBMS 변경작업에 따라 다음과 같은 변경 사항이 있습니다.
    * 일반/개별/대량 메일 발송 시 발급되는 request_id 길이가 변경됩니다.
        * AS-iS : yyyyMMddHHmmssXXXXT (18,19글자)
            * yyyyMMddHHmmss : 년/월/일/시/분/초
            * XXXX : 0 ~ 9999 시퀀스
            * T : 타입(예약 발송시에만 추가)
        * TO-BE : yyyyMMddHHmmssXXXXAAAT (22글자)
            * yyyyMMddHHmmss : 년/월/일/시/분/초
            * XXXX : 0 ~ 9999 시퀀스
            * AAA : 인스턴스당 고유 번호
            * T : 타입(0:일반, 1:예약, 2:대량, 3:임시첨부파일)

### 2017. 02. 23.

#### 기능 추가

* [Console] 메일 요청별 조회 -> 메일 상세 조회 화면에서 수신 확인 여부 표시
* [API] 메일발송 상세조회 API의 Response 필드 추가 ([문서링크](./api-guide-v1.0/#_26))
    * 수신자 리스트(receivers) 항목의 수신여부(readYn), 수신날짜(readDate)

#### 기능 개선/변경

* [Console] 일반 발송 수신자 수 제한
    * 수신자, 참조자 구분 없이 최대 1000명 까지 발송 가능합니다.
* [API] 일반/개별 메일발송 수신자 수 제한
    * 요청 하나당 수신자, 참조자 구분 없이 최대 1000명 까지 발송 가능합니다.
* [Console] 첨부파일 업로드 및 발송 용량 제한
    * 업로드하는 첨부파일 크기는 최대 10MB까지 가능합니다.
    * 메일 발송 시 첨부파일들의 총 크기는 최대 10MB까지 가능합니다.
* [API] 첨부파일 업로드 용량 제한
    * 업로드하는 첨부파일 크기는 최대 10MB까지 가능합니다.
* [API] 첨부파일 포함된 메일 발송 용량 제한
    * 메일 발송 시 첨부파일들의 총 크기는 최대 10MB까지 가능합니다.

#### 버그 수정

* [Console] 예약발송 이슈 수정
    * 현상 : 달이 넘어가는 날짜에 예약 발송 시 정상적으로 발송이 안되는 현상이 있었습니다.
      예) 1월 30일 날 등록된 2월 1일 예약 발송 메일이 정상 동작하지 않음
    * 해결 : 예약 일시에 상관 없이 모두 발송되도록 수정되었습니다.
* [Console] 메일 발송 > 본문 내용에 이미지 URL로 사진 업로드시 메일 상세조회 이슈
    * 현상 : 본문 내 이미지 추가 시 Width가 변경되는 현상(100%로 통일)이 있었습니다.
    * 해결 : 본문 이미지 Width를 변경하지 않도록 수정되었습니다.(기본 Width 유지)
* [Console] 메일요청별조회 > 템플릿 검색 이슈
    * 현상 : 템플릿으로 메일검색 이후 초기화 시 템플릿 검색 조건이 사라지지 않았습니다.
    * 해결 : 초기화 버튼 클릭 시 템플릿 검색 조건도 초기화되도록 수정되었습니다.
* [API] 템플릿 등록시 카테고리ID 유효성 검사 체크로직 추가
    * 현상 : 유효하지 않는 카테고리 ID로 템플릿 등록 시 정상적으로 등록되는 현상이 있었습니다.
    * 해결 : 템플릿 등록 전 카테고리 ID의 유효성 여부를 판단합니다.

### 2016. 12. 22.

#### 기능 개선/변경

* [Console] 발송 메일 명칭 통일
    * 화면에 따라 보낸사람, 발송메일로 표시되던 명칭을 "발송메일" 명칭으로 통일하였습니다.
* [Console] 메일발송 탭의 템플릿 선택 시 첨부파일 앞에 불필요한 체크박스를 제거
* [API] 메일발송API 유효성검사 오류 추가
    * title, body 없이 메일을 발송할 경우 API 오류를 반환하도록 변경되었습니다.
    * receiveType에 MRT0(받는사람), MRT1(참조)이외의 값을 설정할 경우 API 오류를 반환하도록 변경되었습니다.
    * receiveType을 입력하지 않을 경우 API 오류를 반환하도록 변경되었습니다.

#### 버그 수정

* [Console] 메일 요청별 조회 탭의 수신일시가 발송일시로 출력되던 현상 수정

### 2016. 12. 08.

#### 버그 수정

* [Console] 메일 발송 탭에서 템플릿 적용 시 첨부파일이 제한 개수(3개)를 초과하여 발송 가능하던 현상 수정
    * 메일 발송 탭에서 메일 발송 시 템플릿 파일을 적용 할 경우 기존 첨부파일을 초기화하고 템플릿 파일이 우선 적용되도록 변경되었습니다.
* [API] 파일 업로드 API로 파일 업로드 시 리턴 결과에 fileId가 null로 표시되고 requestId 값으로 넘어오던 현상 수정

### 2016. 11. 24.

#### 기능 개선/변경

* [Console] 대량 발송 기능 개선
    * 템플릿 파일 업로드 개선: 엑셀과 같은 일부 편집기에서 편집 이력이 있는 셀의 경우 셀 데이터가 없을 경우에도 빈 문자열 데이터가 포함되어 저장되었습니다. 입력 범위 밖의 빈 문자열의 경우 템플릿 파일
      업로드 시 유효성 검사에서 무시하도록 변경되었습니다.
    * 유의 안내 문구 추가: 엑셀과 같은 일부 편집기에서 CSV 템플릿 파일을 작성할 경우 유니코드 정보가 저장되지 않아 문자가 깨지는 이슈가 있었습니다. 해당 이슈에 대한 내용을 템플릿 다운로드와 발송 예약
      시 유의 안내 문구가 표시되었습니다.

#### 버그 수정

* [Console] 대량 발송 페이지 오류 수정
    * 이벤트 오류 수정: 요청 리스트의 헤더를 클릭할 경우 '발송 요청 건을 선택 후 조회할 수 있습니다.' alert 가 표시되는 오류가 수정되었습니다.

### 2016. 10. 20.

#### 기능개선/변경

* [Document] Developer's Guide 문서에 추가 설명과 예제 추가
    * 일반 메일 발송/개별 메일 발송 API의 [Request body]에 요청 예제가 추가되었습니다.
    * 치환 기능을 이용할 경우, 메일 내용 작성 방법과 API의 [Request body]에 요청 예제가 추가되었습니다.
* [Document] Developer's Guide > 메일 발송의 [Request body]에 senderName 파라미터 추가

### 2016. 10. 06.

#### 버그 수정

* [Console] 대량 메일 발송 탭에서 대량 메일 발송시 메일 발송은 정상적으로 발송되나 첨부파일이 포함되지 않는 버그 수정
* [Console] 대량 메일 발송 탭에서 대량 메일 발송 중 오류 발생 시 발송 상태가 발송 실패로 변경되지 않는 오류 수정
* [Console][API] 메일 발송시 월이 변경되는 시점에 메일 발송이 실패하는 버그 수정
* [Console] 템플릿 관리 탭에서 존재하는 템플릿을 선택 후 신규 템플릿 생성할 경우 첨부파일 개 수가 초기화되지 않는 버그 수정

### 2016. 08. 18.

#### 기능개선/변경

* [Console] 파일업로드를 통한 대량메일발송 기능 개선
    * CSV를 통한 대량메일발송 기능 추가 : 기존에 엑셀파일을 통해서만 제공하던 대량메일발송 기능을 CSV파일을 통해서도 발송 할 수 있도록 확장되었습니다. (CSV 템플릿 제공)
    * 선택 적 대상자 확인 프로세스: 기존 엑셀파일 대량업로드 시 필수로 대상자를 확인해야 발송 할수 있었던 프로세스에서 선택적으로 대상자를 확인 할 수 있도록 개선되었습니다.
    * 첨부파일 validation 강화 : 대상자 확인 프로세스를 생략하더라도 메일주소가 유효한지 누락 된 데이터는 없는지 등의 유효성 검사를 통해 첨부한 파일에 데이터 오류를 알려주는 기능이 추가되었습니다.
    * 대량메일발송 탭 추가 : 대상자 확인후진행 선택 시 대량메일발송탭을 통해 대상자 확인 후 메일발송 진행하는 기능이 추가되었습니다. (발송진행, 발송취소, 발송 후 발송상태 확인)
    * 참고 : Upcoming Products > Email > Getting Started > 파일 업로드를 통한 대량 메일발송 추가

#### 버그 수정

* [Console] 템플릿 관리 > 템플릿에 첨부파일 용량을 파일당 10MB로 제한하는 기능 추가 (템플릿 하나당 총 3개파일 총 30MB까지 업로드 가능)
* [Console] 메일발송/ 템플릿 관리 탭 내에 본문 에디터 기능 중 글머리기호, 번호매기기 기능 사용 시 에디터에서 제대로 표시되지 않던 오류 수정
* [Console] 메일 발송 > 메일발송 시 첨부파일 오류 후 취소 시에도 동일한 경고 발생하는 현상 수정
    * 메일발송 > 메일발송 시 첨부파일 업로드 제한개수(3개)가 초과되어 개수제한경고가 노출 된 후 추가로 파일을 첨부하려고 시도하면 파일탐색기에서 선택 취소시에도 개수제한 경고가 발생되는 현상이
      수정되었습니다.
    * 메일발송 > 메일발송 시 첨부파일 업로드 제한용량(10MB)이 초과되어 용량제한 경고가 노출 된 후 추가로 파일을 첨부하려고 시도하면 파일탐색기에서 선택 취소시에도 용량제한 경고가 발생되는 현상이
      수정되었습니다.

### 2016. 08. 04.

#### 기능개선/변경

* [Console] 메일 발송조회 성능개선 작업 (인덱스 변경/ 반정규화 작업)
* [Console] 국제화처리 적용

#### 버그 수정

* [Console] 수신자 리스트 파일을 2회 이상 업로드 시도 시 Progress loadbar 무한로딩되는 현상수정
