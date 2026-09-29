<!-- machine_translated: true -->

<!-- pre-align:aligned sig=8e711388a601 -->

<a id="notification-email-release-notes"></a>
## Notification > Email > Release Notes { #notification-email-release-notes }

<a id="august-25-2026"></a>
### August 25, 2026 { #august-25-2026 }

<a id="august-25-2026-feature-updates"></a>
#### Feature Updates

* [Console/API] Improved the domain TLD classification criteria
    * Improved the top-level domain (TLD) classification criteria so that some domains that were previously classified as subdomains are now correctly classified.

<a id="notification-email-release-notes-1"></a>
### May 27, 2026 { #notification-email-release-notes-1 }

<a id="notification-email-release-notes-1-1"></a>
#### Bug Fixes

* [API] Improved unsubscribe link call failure issue
    * Fixed an issue where the unsubscribe link in sent emails failed to be called in some cases.
    * Previously sent unsubscribe links also work correctly after the fix.
* [API] Improved handling of domains containing non-ASCII characters
    * Changed to return an error response when a domain registration or email sending request is made with a domain containing non-ASCII characters such as accent marks (e.g., `münchen.de`).
    * Sending stability is improved by blocking domain requests that do not comply with RFC 1123 standards (letters, numbers, hyphens, and periods).
    * If you need to use an internationalized domain, convert it to Punycode format (e.g., `xn--mnchen-3ya.de`) before making the request.

<a id="march-24-2026"></a>
### March 24, 2026 { #march-24-2026 }
<a id="march-24-2026-feature-updates"></a>

#### Feature Updates
* [Console, API] End of support for tag email sending
    * Support for tag management, UID management, and tag email delivery features has ended.
    * The **Query Tagged Email Delivery**, **Tag Management**, and **UID Management** menus have been removed from the console.
    * Calls to APIs related to tag management, UID management, and tag email delivery now return an end-of-support response.

<a id="september-23-2025"></a>
### September 23, 2025 { #september-23-2025 }

<a id="september-23-2025-feature-updates"></a>
#### Feature Updates

* [Console] Added a "Read" field when downloading search results from a delivery history
    * A "Read Status" field has been added to the download results for general delivery history search results.
    * This allows you to more easily check and analyze recipients' email read status.
    * Read status information, previously unavailable in the search results download feature, allows you to more accurately measure the effectiveness of your email delivery.

<a id="september-23-2025-bug-fixes"></a>
#### Bug Fixes

* [SMTP] Improved subdomain verification logic
    * Fixed an authentication failure issue that occurred when sending email to an SMTP subdomain has been fixed.
    * Improved the issue that emails were not sent due to authentication failure despite completing SPF, DKIM, and DMARC authentication for the subdomain.

<a id="may-27-2025"></a>
### May 27, 2025 { #may-27-2025 }

<a id="may-27-2025-feature-updates"></a>
#### Feature Updates

* [Console] Improved notification details for opt out
    * Improved notification detilas exposed when an email recipient clicks the opt out link.
    * AS-IS: the recipient's address and the opt out button were exposed.
    * TO-BE: the sender and the date and time of the opt out are exposed.
    * If you send it in the format of "Sender name <sender email>", the sender name and sender email will be displayed when you click the opt out link.
        * If you do not enter a sender name, only the sent email will be displayed.
    * This feature applies to emails sent after May 27th and does not apply to emails already sent.

<a id="april-29-2025"></a>
### April 29, 2025 { #april-29-2025 }

<a id="april-29-2025-feature-updates"></a>
#### Feature Updates

* [API] Changed One-click opt-out policy
    * The one-click opt-out feature policy has been changed in accordance with the enhanced Gmail sender guidelines.
    * AS-IS: a one-click opt-out feature is now available for all emails sent to a recipient.
    * TO-BE: a one-click opt-out feature is provided only for advertising emails sent to recipients.
    * If you want to use one-click opt-out for regular emails and authentication emails, you can do so by adding 
      `List-Unsubscribe-Post: List-Unsubscribe=One-Click` to customHeaders.
        * At this time, the one-click opt-out URL must be created by the user, and the service does not provide an automatically generated link internally.

* [API] Added webhook sending status
    * Added a condition for the sending status code when a webhook is sent.
    * Added sending status codes are as follows:
        * SST3: send failed
        * SST4: opt out
        * SST7: unauthenticated
        * SST8: send failed (filtering by whitelist)

* [Console] Changed mass delivery statistics processing criteria and improved errors
    * The statistics processing criteria for mass delivery is changed.
        * The request event is processed when a file uploaded by a user is processed.
    * Improved some statistical events that were being processed as duplicates.

<a id="march-11-2025"></a>
### March 11, 2025 { #march-11-2025 }

<a id="march-11-2025-added-features"></a>
#### Added Features

* [API] Added API for viewing the list of completed mail delivery updates
    * Added an API for checking the sending status of emails that have been sent through the Email service.
    * This API searches based on the update time of the message sending result.
    * The criteria for updating the sending results are as follows:
        * When the mail has been received (when a reception completion response is received from the receiving SMTP)
        * When the email is processed as a failure during the sending process, such as opt out, whitelist
* [CONSOLE] Added a feature to set up bounced email settings
    * You can set whether or not to receive bounced emails by adding a bounced email settings option.
    * If you set no-reply or noreply as your email address, you will not receive bounced emails regardless of whether or not you have bounce settings enabled.
* [API] Added v2.1 advertising email sending/authentication email sending API senderGroupingKey
    * You can set the sender group key by adding senderGroupingKey to the advertising email sending/authentication email sending API.

<a id="march-11-2025-feature-updates"></a>
#### Feature Updates

* [API] Fixed an error in handling the mail read event
    * Improved an issue where, in some situations, emails would be marked as read TO-BEbeing sent.
* [CONSOLE] Improved a feature to preview Freemarker tempate
    * When previewing a Freemarker template, we have improved the ability to check which parameters were not applied when not applying all template parameters.
* [API] Improved template sending validity
    * Improved the issue where the original template text is sent even when template parameters are missing when sending a template.
    * If template parameters are missing when sending a template, the sending will fail.


<a id="august-27-2024"></a>
### August 27, 2024 { #august-27-2024 }

<a id="august-27-2024-feature-updates"></a>
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

<a id="july-23-2024"></a>
### July 23, 2024 { #july-23-2024 }

<a id="july-23-2024-feature-updates"></a>
#### Feature Updates

* [Console] Changed the masking policy
    * Changed the email address masking policy to minimise the exposure of personal information
    * If the local part is digit, it is fully masked (a@nhn.com → *@nhn.com)
    * If the local part is 3 digits or less, only 1 digit is exposed (aaa@nhn.com → a**@nhn.com)
    * If the local part is 4 digits or more, only 2 digits are exposed (aaaa@nhn.com → aa**@nhn.com)

<a id="june-25-2024"></a>
### June 25, 2024 { #june-25-2024 }

<a id="june-25-2024-feature-updates"></a>
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

<a id="may-28-2024"></a>
### May 28, 2024 { #may-28-2024 }

<a id="may-28-2024-feature-updates"></a>
#### Feature Updates

* [API/Console] Changed the maximum days for scheduled delivery
    * Changed the maximum days for scheduled delivery from 30 days to 60 days.
    * On the scheduled delivery and lookup screens, you can set a scheduled delivery time of up to 60 days.

<a id="march-26-2024"></a>
### March 26, 2024 { #march-26-2024 }

<a id="march-26-2024-added-features"></a>
#### Added Features

* [Console] Added the feature to manage templates - Modify Category/Template
    * Added the feature to modify templates and categories, allowing you to modify selected items.
    * You can select a category or template on `Manage Template` screen and drag it to any desired categories.

<a id="march-26-2024-feature-updates"></a>
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

<a id="february-27-2024"></a>
### February 27, 2024 { #february-27-2024 }

<a id="february-27-2024-added-features"></a>
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

<a id="february-27-2024-feature-updates"></a>
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

<a id="jan-31-2024"></a>
### Jan 31, 2024. { #jan-31-2024 }

<a id="jan-31-2024-added-features"></a>
#### Added Features

* [Console] Added mail sending status
    - Added "Authentication failed (SST7)" to mail sending status.
    - On February 1, 2024, changes
      to [Gmail email sender guidelines](https://support.google.com/mail/answer/81126?hl=ko#requirements-5k)will result
      in sending
      being restricted if you don't perform all three SPF, DKIM, and DMARC authentication.
    - If the sending is restricted, the sending status will be 'Authentication failed (SST7)'.

<a id="jan-31-2024-feature-updates"></a>
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

<a id="december-19-2023"></a>
### December 19, 2023 { #december-19-2023 }

<a id="december-19-2023-added-features"></a>
#### Added Features

* [Console] Added DMARC authentication feature
    - DMARC authentication procedure is added.
    - On February 1, 2024, changes
      to [Gmail email sender guidelines](https://support.google.com/mail/answer/81126?hl=ko#requirements-5k) will result
      in sending
      being restricted if you don't perform all three SPF, DKIM, and DMARC authentication.

<a id="october-17-2023"></a>
### October 17, 2023. { #october-17-2023 }

<a id="october-17-2023-feature-updates"></a>
#### Feature Updates

* [Console] Settings for mail retrieve defaults
    * Added the feature to reset the delivery/request/schedule date fields upon entering the mail retrieve screen.
    * This feature applies to the “Retrieve by Mail Request”, “Retrieve Scheduled Mail Delivery”, “Retrieve Bulk Mail
      Delivery”, and “Retrieve Tagged Mail
      Delivery” screens.

<a id="august-29-2023"></a>
### August 29, 2023 { #august-29-2023 }

<a id="august-29-2023-feature-updates"></a>
#### Feature Updates

* [Console] Duplicate requests prevention when requesting for mass delivery
    * Improved to reject duplicate requests for mass delivery.

<a id="august-29-2023-added-features"></a>
#### Added Features

* [Console] Added the feature to backup out-of-date data
    * Added settings for backing up data that has passed its retention period.
    * The body of the mail is not included in the save target.

<a id="july-25-2023"></a>
### July 25, 2023 { #july-25-2023 }

<a id="july-25-2023-feature-updates"></a>
#### Feature Updates

* [Console] Changed mail domain management screen
    * Deleted the Date and Time of Verification, Date and Time of Protection, Date and Time of Check SPF Record field.
    * You can check the deleted fields as a tooltip by hovering over the **Verified or Not, Protected or not, SPF Record
      or Not** respectively.
* [Console] Added additional information in Retrieve by Mail Request → Request to download search results
    * Added Mail Sequence, Template ID, DSN Message information to the request file content for mail delivery detail
      queries.

<a id="june-27-2023"></a>
### June 27, 2023 { #june-27-2023 }

<a id="june-27-2023-feature-updates"></a>
#### Feature Updates

* [Console] Improved template preview screen
    * Improved the Preview template screen when selecting a template in mail delivery.
    * Moved the template content that displayed in the body field, when selecting a template, to the Preview screen.

<a id="may-30-2023"></a>
### May 30, 2023 { #may-30-2023 }

<a id="may-30-2023-feature-updates"></a>
#### Feature Updates

* [API] Added retention period validty of attachments
    * Improved from failing at send time after a successful mail request with an expired file attachment to failing at
      request time when sending.

<a id="may-30-2023-bug-fixes"></a>
#### Bug Fixes

* [Console] Fixes some CSS conflicts when registering email templates
    * Fixed an issue where, when registering email templates, CSS conflicts of some tags cause an incorrect display in
      the preview.

<a id="april-25-2023"></a>
### April 25, 2023 { #april-25-2023 }

<a id="april-25-2023-feature-updates"></a>
#### Feature Updates

* [API/SMTP] Changed domain
    * Changed the domain for the API/SMTP interface from cloud.toast.com to nhncloudservice.com.
        * API: api-mail.cloud.toast.com → email.api.nhncloudservice.com
        * SMTP: smtp-mail.cloud.toast.com → smtp-mail.nhncloudservice.com
    * When using the API/SMTP interface, you must change the domain. If you keep using the existing domain, delivery may
      be restricted in the future.

<a id="april-25-2023-bug-fixes"></a>
#### Bug Fixes

* [Console] Fixed an issue where common.code.null appears when searching for recipients of mass delivery
    * Fixed an issue where, when searching the list of recipients by mail after mass delivery, common.code.null appears
      in some status code.

<a id="april-25-2023-added-features"></a>
#### Added Features

* [Console] Added the Cancel button for mass delivery using scheduled delivery
    * Added a button to cancel mass delivery using scheduled delivery.

<a id="march-28-2023"></a>
### March 28, 2023 { #march-28-2023 }

* [Console] Changed the date limit on query
    * Changed the date limit from 30 days to 31 days for mass delivery queries, tagged mailing queries, and queries by
      regular mail recipient address.
* Improved to select file extensions when downloading requested files
    * CSV, XLSX
* Improved display of mass delivery results
    * Changed from displaying successful delivery to displaying delivery failure if there is no data sent normally for
      mass delivery.

<a id="october-25-2022"></a>
### October 25, 2022 { #october-25-2022 }

<a id="october-25-2022-bug-fixes"></a>
#### Bug Fixes

* [SMTP] Fixed an issue where, when both text/plain and text/HTML messages are included, only the text/pain message are
  sent

<a id="february-22-2022"></a>
### February 22, 2022 { #february-22-2022 }

<a id="february-22-2022-feature-updates"></a>
#### Feature Updates

* [Console] Added blind carbon copy (BCC) feature

<a id="january-25-2022"></a>
### January 25, 2022 { #january-25-2022 }

<a id="january-25-2022-feature-updates"></a>
#### Feature Updates

* [SMTP] Added SMTP interface feature
    * You can send mail via SMTP interface.
    * For more details, refer to [SMTP Guide](./smtp-guide).

<a id="september-14-2021"></a>
### September 14, 2021 { #september-14-2021 }

<a id="september-14-2021-feature-updates"></a>
#### Feature Updates

* [API] API Version Updated to v2.0
    * Added secret key authentication
        * Added secret key authentication in API authentication.
        * For more details, refer to [Secret Key](./api-guide/#secret-key).
    * Added Retrieve Bulk Main Delivery API
        * Added an API to retrieve bulk mail deliveries.
* [Console/API] Restricted allowed characters for template ID
    * Improved not to use &, %, ", ' for a template ID.

<a id="march-23-2021"></a>
### March 23, 2021 { #march-23-2021 }

<a id="march-23-2021-feature-updates"></a>
#### Feature Updates

* [Console/API] Added restriction to allowed characters for template ID
    * Improved not to use '/', '?', ':' for a template ID.

<a id="notification-email-release-notes-2"></a>
### October 27, 2020 { #notification-email-release-notes-2 }

<a id="notification-email-release-notes-2-1"></a>
#### Added Features

* [Console] Added a feature to cancel bulk mail delivery
    * Added a feature to cancel mail that is being sent on the **Retrieve Mass Mail Delivery** tab.

<a id="september-22-2020"></a>
### September 22, 2020 { #september-22-2020 }

<a id="september-22-2020-feature-updates"></a>
#### Feature Updates

[Console] Added webhook feature

* Added [Manage Webhook] menu.
* When a specific event occurs in the Email service, create POST request with the URL specified by the webhook settings.
* Event types that are currently supported are as follows.
* Register recipient addresses that reject advertising mails
* When a recipient rejects an advertising mail by using a link included in the mail, the webhook feature starts to work.
* For more details, refer to [Manage Webhook](./console-guide/#webhook).

<a id="august-25-2020"></a>
### August 25, 2020 { #august-25-2020 }

<a id="august-25-2020-feature-updates"></a>
#### Feature Updates

* [Console] Added DSN information when querying email delivery details
* [Console] Added DSN information within CSV file when downloading file for a detail mail delivery case
* [API] Updated API to v1.7
    * Added the response field of Query Mail Delivery Details API
        * Find mail status sent via SMTP from the field on DSN information
            * DSN Code, DSN Message
            * [DSN(Delivery Status Notification)](https://tools.ietf.org/html/rfc3463)

<a id="june-23-2020"></a>
### June 23, 2020 { #june-23-2020 }

<a id="june-23-2020-feature-updates"></a>
#### Feature Updates

* [Console] Added Cancellation for Query Scheduled Delivery
    * On the **Query Scheduled Delivery** tab, the feature of cancelling scheduled emails before sent has been added.
* [API] Added Query/Cancel Scheduled Delivery
    * Query or cancel scheduled delivery are now available via APIs.

<a id="april-28-2020"></a>
### April 28, 2020 { #april-28-2020 }

<a id="april-28-2020-feature-updates"></a>
#### Feature Updates

* [Console] Added DKIM
    * The features of DomainKeys Identified Mail have been added to check if receiving email is forged.
    * For more details, see [DKIM Guide](./console-guide/#dkim).

<a id="march-24-2020"></a>
### March 24, 2020 { #march-24-2020 }

<a id="march-24-2020-feature-updates"></a>
#### Feature Updates

* [Console] Added Mail Domain Management Tab
    - Main features are as follows:
        - Manage Mail Domains
        - Added features to register, verify, delete, and share mail domains.
        - Protect Mail Domains
        - Verified mail domains can be protected from third-party usage.

<a id="january-21-2020"></a>
### January 21, 2020 { #january-21-2020 }

<a id="january-21-2020-feature-updates"></a>
#### Feature Updates

* [Console] Downloading is available within the **Query Mass Mail Delivery** tab
    * You can download the list of query results for mass mail delivery in the mass delivery template.

<a id="november-26-2019"></a>
### November 26, 2019 { #november-26-2019 }

<a id="november-26-2019-feature-updates"></a>
#### Feature Updates

* [API] API Version Updated to v1.6
* [API] User-Input Data Supported for Templates
    * When you use a template, user-input sender information, title, and body text are preferred for application than
      template data.
    * For more details, see [Send Mails API](./api-guide/#_1)

<a id="october-29-2019"></a>
### October 29, 2019 { #october-29-2019 }

<a id="october-29-2019-feature-updates"></a>
#### Feature Updates

* [Console] Added the Feature of Individual Delivery
    * For general delivery, emails can be individually sent to each recipient.

<a id="august-27-2019"></a>
### August 27, 2019 { #august-27-2019 }

<a id="august-27-2019-feature-updates"></a>
#### Feature Updates

* [API] API version updated to v1.5
* [API] Added Features for Sender's Group Key
    * Send mails, when requested, by specifying sender's group key, which helps to query request.
    * For more details, see [Send Mail API](./api-guide/#_1) and [Query Mail API](./api-guide/#_25).
* [API] Added Receive or Not Field
    * Query request details to see if the mail has been received, as well as received time.
    * For more details, see [Query Mail Delivery Details API](./api-guide/#_29).

<a id="july-23-2019"></a>
### July 23, 2019 { #july-23-2019 }

<a id="july-23-2019-feature-updates"></a>
#### Feature Updates

* [API] Added Manage Templates API
    * Register, Modify, and Delete Templates are available on APIs.
    * For more details, see [Manage Templates API](./api-guide/#template).
* [API] Added Manage Category API
    * Use categories to classify and manage templates. You may specify categories to be included when a template is
      created.
    * Register, Modify, Delete, and Query Categories are available on APIs.
    * For more details, see [Manage Category API](./api-guide/#category).

<a id="01-29"></a>
### 2019. 01. 29. { #01-29 }

<a id="01-29-1"></a>
#### 기능 추가

* [Console/API] Longer template ID
    * Allowed length of template ID has changed to 50 characters, from 10

<a id="12-18"></a>
### 2018. 12. 18. { #12-18 }

<a id="12-18-1"></a>
#### 기능 추가

* [Console] Added template engine feature
    * You can register a template by using the FreeMarker Template Language in the title and body.
    * You can send mail using templates with the template language. For bulk mail delivery, you can substitute values by entering template parameters in an Excel file.

<a id="11-27"></a>
### 2018. 11. 27. { #11-27 }

<a id="11-27-1"></a>
#### 기능 추가

* [API] API Version Updated to v1.4
* [API] Added a feature to substitute without registering a template
    * You can use the substitution feature by using template parameters in a delivery request without registering a template in advance.
* [API] Template Engine Feature
    * You can send mail by using FreeMarker Template Language and template parameters.
    * For more details, refer to [Title/Content Replacement](./api-guide/#_21).

<a id="10-23"></a>
### 2018. 10. 23. { #10-23 }

<a id="10-23-1"></a>
#### 기능 추가

* [API] API Version Updated to v1.3
* [API] Changed the standard and promotional mail delivery API response
    * Supported in API v1.3 and later.
    * You can request sending even if only some of the recipient requests are valid. (In previous versions, the request fails if even one recipient request is invalid.)
    * The mail delivery API response returns results for each recipient. You can check the successful recipient information and the failed recipient information from the response.
    * For more details, refer to [General Mail Delivery API Response](./api-guide-v1.3/#_4) and [Individual Mail Delivery API Response](./api-guide-v1.3/#_7).
* [API] Added advertisement status as a filter condition for Search Statistics API
    * You can view statistics by separating promotional mail from non-promotional mail.
    * For more details, refer to [Integrated Statistics Query API](./api-guide-v1.3/#_83).

<a id="09-18"></a>
### 2018. 09. 18. { #09-18 }

<a id="09-18-1"></a>
#### 기능 추가

* [API] Added Verify Email Delivery API
    * Provides a verification mail delivery API that can be used for mail authentication.
    * Verification mail supports single sending only by its nature, and does not support attachments.
    * For more details, refer to [Verification Mail Delivery API](./api-guide-v1.2/#_11).

<a id="08-28"></a>
### 2018. 08. 28. { #08-28 }

<a id="08-28-1"></a>
#### 기능 추가

* [API] API Version Updated to v1.2
* [API] Changed the structure to enable reuse of attachments
    * Improved the structure to allow resending with the same ID after an attachment has been sent once.
    * Attachments uploaded using the [Upload Attachment API v1.2](./api-guide-v1.2/#_18) can be reused as attachments when sending emails using the v1.2 sending API.
    * All sending API v1.2 provided by Toast Email can be used.
    * For updated API versions, refer to the respective API specifications in the [API Guide](./api-guide-v1.2/).
* [API] Enhanced validation of email address
    * The API returns an error if you request a mail address with an invalid mail ID or domain format.
    * This applies to all Send APIs provided by Toast Email.

<a id="06-26"></a>
### 2018. 06. 26. { #06-26 }

<a id="06-26-1"></a>
#### 기능 추가

* [API] Added the custom header feature
    * You can send emails with custom headers added to the received email.
    * You can use this in addition to all sending APIs provided by Toast Email.
    * Added the Mail Query Details API v1.1, which supports custom headers.
    * For more details, refer to [Custom Header](./console-guide/#custom-header).
    * For information on the updated API version, refer to [Retrieve Mail Delivery Details](./api-guide-v1.1/#_26) and [Retrieve Tagged Mail Delivery Details](./api-guide-v1.1/#_35).
* [API] Added the blind carbon copy (BCC) feature
    * A recipient type has been added to the General Mail delivery API, allowing you to send mail with BCC (Blind Carbon Copy).

<a id="04-24"></a>
### 2018. 04. 24. { #04-24 }

<a id="04-24-1"></a>
#### 기능 추가

* [Console] File size restricted for uploading mass delivery recipient Excel/CSV files
    * You can upload a bulk mail recipient file of up to 10,000 persons and up to 3 MB.

<a id="03-22"></a>
### 2018. 03. 22. { #03-22 }

<a id="03-22-1"></a>
#### 기능 추가

* [Console] Changed the tab UI

<a id="02-22"></a>
### 2018. 02. 22. { #02-22 }

<a id="02-22-1"></a>
#### 기능 추가

* [Console] Added tag mail delivery feature
    * By registering tags and UIDs, you can easily send emails to multiple users.
    * This feature allows you to send a message by selecting a tag instead of recipient info.
    * For more details, refer to [Send Emails Using Tags](./console-guide/#_6).
* [Console] Added the tagged mail delivery history page
    * Provides a screen for querying mail sent using tags.
    * You can retrieve details on the request info, recipient info, and sent mail.
    * For more details, refer to [Query Tagged Email Delivery tab](./console-guide/#_9).
* [Console] Added tag and UID management feature
    * Provides features to create, modify, and delete tags and UIDs used for tag mail delivery.
    * For more details, refer to [Tag Management](./console-guide/#_11), [UID Management](./console-guide/#uid).
* [API] Added tag mail delivery and query features
    * Provides an API to send and query tagged emails.
    * For more details, refer to [Tag Mail Delivery](./api-guide-v1.0/#_29).
* [API] Added Tag and UID Management Features
    * Provides an API to create, modify, and delete tags and UIDs used for tag mail delivery.
    * For more details, refer to [Manage Tags](./api-guide-v1.0/#_45), [Manage UIDs](./api-guide-v1.0/#uid).

<a id="09-21"></a>
### 2017. 09. 21. { #09-21 }

<a id="09-21-1"></a>
#### 기능 추가

* [Console] Added reception rate to statistics types
    * You can check the reception rate in statistics graphs and tables.
* [API] Added statistics type to the Statistics Query API
    * Reception rate information is added to Daily Statistics, Monthly Statistics, and Integrated Statistics respectively.
    * For more details, refer to [Statistics Query](./api-guide-v1.0/#_72).

<a id="08-24"></a>
### 2017. 08. 24. { #08-24 }

<a id="08-24-1"></a>
#### 버그 수정

* [Console] Fixed an issue where the image size increases abnormally when importing an image from a template
    * Fixed an issue where, when an image tag was included in the template body, selecting and importing the template in the Console caused it to expand abnormally.

<a id="07-20"></a>
### 2017. 07. 20. { #07-20 }

<a id="07-20-1"></a>
#### 기능 추가

* [Console] Added feature to download the unsubscribe list
    * The rejection list can be downloaded in CSV format.
    * A download request file is scheduled for creation and retained for one week after it is created.

<a id="07-20-2"></a>
#### 기능 개선

* [Console] Fixed the extension check when uploading an attachment
    * File extension restrictions apply when sending mail or managing templates.
    * Restricted extensions: js, exe, bat, cmd, com, cpl, scr, vbs, wsf
* [API] Extension check added for the Upload Attachment API
    * File extensions are also checked when uploading attachments via API.

<a id="07-20-3"></a>
#### 버그 수정

* [Console] Fixed an issue where open statistics were not properly aggregated during mass mail delivery
    * Fixed an issue where the open rate increased abnormally during bulk mail delivery.

<a id="06-22"></a>
### 2017. 06. 22. { #06-22 }

<a id="06-22-1"></a>
#### 기능 추가

* [Console] Added a feature to send advertising mail
    * You can now send advertising emails from the Email product.
    * The following features are provided for advertising mail delivery.
        * Added "(Advertisement)" text to the email subject
        * Insert an opt-out link in the mail body
        * Provides a screen to unsubscribe when clicking the unsubscribe link
        * Provides a feature to block advertising mail delivery to recipients who have opted out
        * Provided a screen to manage unsubscribed recipients (the download feature will be available in a future update)
    * For more details, refer to [Send Advertising Mail](./console-guide/#_3) and [Manage Unsubscribes](./console-guide/#_13).
* [API] Added advertising mail delivery feature
    * For advertising emails, you can use a separately provided API.
    * Functions are provided as follows.
        * "(Ad)" must be included in the subject line.
        * Mail delivery is not performed for recipients registered as unsubscribed.
* [API] Added opt-out management feature
    * Provides an API to query/register/delete opt-out users.
    * For more details, refer to [Manage Unsubscribes](./api-guide-v1.0/#_82).
* [API] Added integrated statistics retrieval feature
    * Added APIs to query by date, time zone, and day of the week.
    * For more information, refer to [Query Statistics](./api-guide-v1.0/#_79).

<a id="06-22-2"></a>
#### 기능 개선

* [Console] Improved statistics screen
    * Improved the statistics screen.
    * You can now view data by date, time range, and day of the week.
    * For more details, refer to [View stats](./console-guide/#_17).

<a id="05-25"></a>
### 2017. 05. 25. { #05-25 }

<a id="05-25-1"></a>
#### 기능 추가

* [Console] Added email preview feature
    * On the mail delivery screen, you can preview the delivery information and body content.

<a id="05-25-2"></a>
#### 버그 수정

* [Console] Fixed an issue with bulk mail scheduled delivery
    * Fixed an issue where bulk mail scheduled delivery was not retrieved or sent when crossing into the next month.
    * Fix: Updated so that scheduled sending is possible even past the current month. (Note: Scheduled sending is available within 3 months from the registration date (e.g., if registered in May, scheduling is available through July).)

<a id="04-20"></a>
### 2017. 04. 20. { #04-20 }

<a id="04-20-1"></a>
#### 기능 추가

* [Console] <b>수신자명<이메일주소></b> 형식으로 발송이 가능
    * 발신자 주소에만 사용할 수 있었던 <b>수신자명<이메일주소></b> 형식을 수신자 주소에서도 사용이 가능합니다.
* [API] Recipient name support
    * The recipient name field has also been added to the API. For more details, refer to [Mail Delivery](./api-guide-v1.0/#_2).
* [Console] Statistics screen
    * Daily/monthly statistics screens are provided.
    * Statistics are collected from April 1, 2017.
* [API] Added statistics retrieval feature
    * Daily/monthly statistics query APIs are now available. For more details, refer to the [Statistics Query documentation](./api-guide-v1.0/#_72).

<a id="04-20-2"></a>
#### 기능 개선

* [Console] Added SPF registration guidance text on the mail delivery screen
    * Added an SPF registration guidance message to the mail delivery screen.
    * For more details, please check the <b>SPF Guide</b> section in Mail Delivery > Sent Mail field.

<a id="03-23"></a>
### 2017. 03. 23. { #03-23 }

<a id="03-23-db"></a>
#### DB 작업

* The following changes have been made in accordance with DBMS modifications.
    * The length of the request_id issued when sending standard/individual/bulk mail will be changed.
        * AS-IS: yyyyMMddHHmmssXXXXT (18, 19 characters)
            * yyyyMMddHHmmss : year/month/day/hour/minute/second
            * XXXX : 0 ~ 9999 sequence
            * T : Type (added only for scheduled delivery)
        * TO-BE: yyyyMMddHHmmssXXXXAAAT (22 characters)
            * yyyyMMddHHmmss : year/month/day/hour/minute/second
            * XXXX : 0 ~ 9999 sequence
            * AAA : Unique number per instance
            * T : Type (0: Normal, 1: Scheduled, 2: Bulk, 3: Temporary attachment)

<a id="02-23"></a>
### 2017. 02. 23. { #02-23 }

<a id="02-23-1"></a>
#### 기능 추가

* [Console] Added a field to display whether mail receipt is confirmed on the Query Mail Details page in Query by Mail Request
* [API] Added a response field to the query mail sending details API ([Document Link](./api-guide-v1.0/#_26))
    * `readYn` (read status) and `readDate` (read date) fields in the list of recipients (`receivers`)

<a id="02-23-2"></a>
#### 기능 개선/변경

* [Console] Restriction of Standard Send Recipient Count
    * You can send to up to 1,000 recipients in total, with no distinction between recipients and CC recipients.
* [API] Restriction of recipients added for general/individual email delivery
    * Up to 1,000 people can be sent per request, with no distinction between recipients and CC (Carbon Copy) recipients.
* [Console] File volume restricted for attachment upload and delivery
    * Attachment sizes are allowed up to 10 MB.
    * The total size of attachments when sending a mail is allowed up to 10 MB.
* [API] File size limit for attachment upload
    * Attachment sizes are allowed up to 10 MB.
* [API] File volume restricted for mail delivery with attachments
    * The total size of attachments for mail delivery is up to 10 MB.

<a id="02-23-3"></a>
#### 버그 수정

* [Console] Fixed scheduled delivery issues
    * Issue: When scheduled delivery was set for a date spanning across months, delivery did not proceed normally.
      Example: An email scheduled for February 1 and registered on January 30 does not work as expected
    * Fix: Modified so that messages are sent regardless of the scheduled date and time.
* [Console] Mail Delivery > Fixed an issue with the mail detail query when uploading a photo using an image URL in the body content
    * Fixed an issue where the Width changed when adding an image to the body (unified to 100%).
    * Fixed: Modified so that the body image width is no longer changed (default width is maintained).
* [Console] Fixed an issue with template search in query-by-mail-request
    * Issue: After searching mail with a template and then initializing, the template search condition was not cleared.
    * Fixed: Changed so that clicking the Initialize button also resets the template search conditions.
* [API] Added category ID validation check logic when registering a template
    * Issue: When registering a template with an invalid category ID, the template was registered successfully.
    * Fix: Validates whether the category ID is valid before registering a template.

<a id="12-22"></a>
### 2016. 12. 22. { #12-22 }

<a id="12-22-1"></a>
#### 기능 개선/변경

* [Console] Unified mail delivery naming
    * Unified the label names that were displayed as "Sender" or "Sent Mail" depending on the screen under the single name "Sent Mail."
* [Console] Removed an unnecessary checkbox before attachments when selecting a template on the Send Mail tab
* [API] Added validation error for the mail sending API
    * Changed to return an API error when sending an email without a title or body.
    * Changed to return an API error if a value other than MRT0 (recipient) or MRT1 (CC) is set for receiveType.
    * Changed to return an API error if receiveType is not entered.

<a id="12-22-2"></a>
#### 버그 수정

* [Console] Fixed an issue where the received date and time was displayed as the sent date and time on the query by mail request tab

<a id="12-08"></a>
### 2016. 12. 08. { #12-08 }

<a id="12-08-1"></a>
#### 버그 수정

* [Console] Fixed an issue where attachments could be sent exceeding the limit (3 files) when applying a template on the mail delivery tab
    * Changed so that when a template file is applied during mail delivery on the Mail Delivery tab, existing attachments are cleared and the template file takes priority.
* [API] Fixed an issue where fileId was displayed as null and the value was returned as requestId in the result when uploading a file using the file upload API

<a id="11-24"></a>
### 2016. 11. 24. { #11-24 }

<a id="11-24-1"></a>
#### 기능 개선/변경

* [Console] Improved the bulk delivery feature
    * Uploading Template Files: In some editors, like excel, even when there is no available data for cell which has editing history, empty character string data are included in saving. Empty character strings which are not within the range of input are
      Changed to ignore validation when uploading.
    * Caution Messages Added: In some editors, like Excel, CSV template files are created and Unicode information is not saved, which results in broken characters. Cautions on such issue are to be provided for template downloads and scheduled sending.
      A caution notice is displayed.

<a id="11-24-2"></a>
#### 버그 수정

* [Console] Fixed error on the bulk delivery page
    * Modified event errors: Modified the error in which an alert shows like 'Select a delivery request to query', at the click of the header of the request list.

<a id="10-20"></a>
### 2016. 10. 20. { #10-20 }

<a id="10-20-1"></a>
#### 기능개선/변경

* [Document] Added additional descriptions and examples to Developer's Guide
    * Added request examples to the [Request Body] of the General Mail Delivery/Individual Mail Delivery APIs.
    * When using the substitution feature, request examples have been added to the mail writing guide and the API [Request Body].
* [Document] Developer's Guide > Mail delivery > Added senderName parameter to [Request body]

<a id="10-06"></a>
### 2016. 10. 06. { #10-06 }

<a id="10-06-1"></a>
#### 버그 수정

* [Console] Fixed a bug where attachments were not included when sending bulk mail from the Bulk Mail Delivery tab, even though the mail itself was sent successfully
* [Console] Fixed an error where the delivery status was not changed to Delivery failed when an error occurred during mass mail delivery on the Mass Mail Delivery tab
* [Console][API] Fixed a bug where mail delivery failed when the month changed during delivery
* [Console] Fixed a bug where the attachment count was not reset when creating a new template after selecting an existing template in the Template Management tab

<a id="08-18"></a>
### 2016. 08. 18. { #08-18 }

<a id="08-18-1"></a>
#### 기능개선/변경

* [Console] Improved the bulk email sending feature via file upload
    * Added mass email delivery via CSV: Mass email delivery, previously available only through Excel files, has been expanded to also support delivery via CSV files. (CSV template provided)
    * Check Process of Selective Targets: Improved to allow selectively checking targets in the process where targets had to be confirmed before sending when performing a bulk Excel file upload.
    * Enforced Attachment File Validation: Added a feature that notifies you of data errors in attached files through validation checks — such as whether email addresses are valid or whether any data is missing — even when the recipient verification process is skipped.
    * Tab Added for Mass Email Delivery: Added a feature to check recipients and proceed with email delivery through the Mass Email Delivery tab when selecting Deliver after Check. (Send, Cancel Delivery, and check delivery status after sending)
    * For reference: Upcoming Products > Email > Getting Started > Mass Mail Delivery via File Upload Added

<a id="08-18-2"></a>
#### 버그 수정

* [Console] Added the feature to limit attachment file size to 10 MB per file in Template Management > Templates (up to 3 files per template, with a total of 30 MB uploadable)
* [Console] Fixed an error where bullet points and numbered lists in the body editor were not displayed correctly in the editor, within the Mail Delivery/Template Management tab
* [Console] Fixed an issue where the same warning occurred when canceling after an attachment error in Mail delivery > mail delivery
    * Mail Delivery > Fixed an issue where, when sending mail, if the attachment upload limit (up to 3) was exceeded and the count limit warning was displayed, attempting to attach additional files would trigger the count limit warning even after canceling the selection in the file explorer.
      Modified.
    * Mail Delivery > Fixed an issue where, when attempting to attach an additional file after the attachment upload size limit (10 MB) was exceeded during mail delivery and a size limit warning was displayed, the size limit warning would still appear even after canceling the file selection in the file browser.
      Modified.

<a id="08-04"></a>
### 2016. 08. 04. { #08-04 }

<a id="08-04-1"></a>
#### 기능개선/변경

* [Console] Improved mail delivery query performance (index changes/denormalization)
* [Console] Applied globalization

<a id="08-04-2"></a>
#### 버그 수정

* [Console] Fixed an issue where the progress load bar continued loading indefinitely when uploading a recipient list file more than once
