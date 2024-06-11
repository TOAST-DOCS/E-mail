## Notification > Email > Error Code

### Error Code

| isSuccessful | resultCode | resultMessage                                                                                                              |
|--------------|------------|----------------------------------------------------------------------------------------------------------------------------|
| true         | 0          | success                                                                                                                    |
| true         | 1000       | Some of your requests are successful.                                                                                      |
| false        | -1000      | Invalid appKey.                                                                                                            |
| false        | -1001      | Service is not exist                                                                                                       |
| false        | -1002      | Service is disabled                                                                                                        |
| false        | -1003      | Invalid service parameter                                                                                                  |
| false        | -1004      | Invalid secretKey.                                                                                                         |
| false        | -1005      | Service is temporarily blocked                                                                                             |
| false        | -1006      | Service is associated with a domain shared within the organization. Please proceed after canceling the sharing.            |
| false        | -1007      | The 'useYn' field of the service is required                                                                               |
| false        | -2000      | Date format is invalid. {}: {}, Only supported {}                                                                          |
| false        | -2001      | The 'receiverList' or 'receiver' is required.                                                                              |
| false        | -2002      | The 'senderAddress' is required.                                                                                           |
| false        | -2003      | Invalid attach file name.                                                                                                  |
| false        | -2004      | Not found the file. attachFileId: {}                                                                                       |
| false        | -2005      | Invalid template type.                                                                                                     |
| false        | -2006      | Not exist data.                                                                                                            |
| false        | -2007      | The 'requestId' is invalid.                                                                                                |
| false        | -2008      | Upload attach file error.                                                                                                  |
| false        | -2009      | The 'requestId' or 'startSendDate' is required.                                                                            |
| false        | -2010      | Sender Email Format is wrong.                                                                                              |
| false        | -2011      | Receiver Email Format is wrong.                                                                                            |
| false        | -2012      | The maximum number of recipients is 1000.                                                                                  |
| false        | -2013      | The file size is less than 30M                                                                                             |
| false        | -2014      | Up to 10 attachments including the template's are allowed.                                                                 |
| false        | -2015      | The 'title' is required.                                                                                                   |
| false        | -2016      | The 'body' is required.                                                                                                    |
| false        | -2017      | The 'receiveMailAddress' is required.                                                                                      |
| false        | -2018      | The 'receiveType' is required.                                                                                             |
| false        | -2019      | The 'receiveType' is invalid.                                                                                              |
| false        | -2020      | The 'fileId' is invalid.                                                                                                   |
| false        | -2021      | The size of total files is too large.                                                                                      |
| false        | -2022      | Invalid provisioned resource.                                                                                              |
| false        | -2023      | For advertising mail,                                                                                                      |광고), |AD), or |広告) phrases must be placed in front of the title.|
| false        | -2024      | Invalid file extension.                                                                                                    |
| false        | -2025      | The 'userId' is required.                                                                                                  |
| false        | -2026      | The file is not exist.                                                                                                     |
| false        | -2027      | The file was expired.                                                                                                      |
| false        | -2028      | Limit of rows is exceeded. It can handle up to 50,000 rows.                                                                |
| false        | -2029      | The 'summary' property can not exceed 200 characters.                                                                      |
| false        | -2030      | The 'customHeaders' contains an invalid name or body.                                                                      |
| false        | -2031      | You can not send in past than requested. Please update the 'requestDate'.                                                  |
| false        | -2032      | All of receivers are failed to send.                                                                                       |
| false        | -2034      | The 'title' exceeds maximum length.                                                                                        |998 chars)|
| false        | -2035      | The 'userId' exceeds maximum length.                                                                                       |50 chars)|
| false        | -2036      | The 'templateId' exceeds maximum length.                                                                                   |50 chars)|
| false        | -2037      | The 'senderName' exceeds maximum length.                                                                                   |100 chars)|
| false        | -2038      | The 'senderAddress' exceeds maximum length.                                                                                |100 chars)|
| false        | -2039      | The 'receiveName' exceeds maximum length.                                                                                  |100 chars)|
| false        | -2040      | The 'receiveAddress' exceeds maximum length.                                                                               |100 chars)|
| false        | -2041      | The 'fileName' exceeds maximum length.                                                                                     |100 chars)|
| false        | -2042      | The 'createUser' exceeds maximum length.                                                                                   |50 chars)|
| false        | -2043      | There are too much mail at this time. Please send it at another time.                                                      |
| false        | -2044      | The 'fileName' is required.                                                                                                |
| false        | -2045      | The 'fileBody' is required.                                                                                                |
| false        | -2046      | The '{}' can have only 'Y' or 'N' values. {}: {}                                                                           |
| false        | -2047      | The 'senderGroupingKey' exceeds maximum length.                                                                            |100 chars)|
| false        | -2048      | You can set the size up to 1000. pageSize: {}                                                                              |
| false        | -2049      | The 'requestId' or                                                                                                         |'startSendDate' and 'endSendDate') are required.|
| false        | -2050      | The {} was protected by another nhn cloud project. Please use it after verifying the sender domain.                        |
| false        | -2051      | Email Format is wrong. {}                                                                                                  |
| false        | -2052      | the 'requestDate' must be filed within 60 days. Please update the 'requestDate'.                                           |
| false        | -2053      | Invalid reservation parameter                                                                                              |
| false        | -2054      | There is no header in the file. Please check the file format.                                                              |
| false        | -2055      | There is no receiver_address header in the file.                                                                           |
| false        | -2056      | The                                                                                                                        |'startReadDate' and 'endReadDate') are required.|
| false        | -2057      | There is no receiver_address in the file.                                                                                  |
| false        | -2058      | Reservation cancel request is failed.                                                                                      |
| false        | -2059      | The 'statsId' exceeds maximum length.                                                                                      |8 chars)|
| false        | -2060      | This request can't update status. Check status of this mass mail request.                                                  |8 chars)|
| false        | -2061      | This request can't update status. Check status of this tag mail request.                                                   |8 chars)|
| false        | -2100      | The 'templateId' is required.                                                                                              |
| false        | -2101      | Already existed templateId.                                                                                                |
| false        | -2102      | The 'templateName' is required.                                                                                            |
| false        | -2103      | The 'sendMailAddress' is required.                                                                                         |
| false        | -2105      | The 'body' is required.                                                                                                    |
| false        | -2106      | UseYn is invalid.                                                                                                          |
| false        | -2107      | Not found the template. templateId: {}                                                                                     |
| false        | -2108      | The 'categoryId' is required.                                                                                              |
| false        | -2109      | Invalid Api Request. Check the replace parameter.                                                                          |
| false        | -2110      | TemplateId must be 50 characters or less.                                                                                  |
| false        | -2111      | A template with attachments is not supported. templateId: {}                                                               |
| false        | -2112      | The 'title' is required.                                                                                                   |
| false        | -2113      | The one of the 'attachFileIdList' has already been attached to another template. attached templateId: {}, attachFileId: {} |
| false        | -2114      | templateId should not contain reserved character                                                                           |/,?,:,<,>,%,&," + '"' + ",')|
| false        | -2115      | Template is disabled. If you want to use a template, change the template status to enabled.                                |
| false        | -2116      | The template has wrong syntax or missing parameter.                                                                        |
| false        | -2117      | {}                                                                                                                         |
| false        | -2118      | {}                                                                                                                         |
| false        | -2200      | Invalid statistics search parameter.                                                                                       |
| false        | -2201      | The 'from' must be before 'to'                                                                                             |
| false        | -2202      | Invalid duration time.                                                                                                     |
| false        | -2203      | The 'from' and 'to' are required.                                                                                          |
| false        | -2204      | The 'searchType' is required.                                                                                              |
| false        | -2205      | The 'searchType' is not supported.                                                                                         |
| false        | -2300      | The 'id' is required.                                                                                                      |
| false        | -2301      | Parameter size is invalid.                                                                                                 |
| false        | -2302      | Parameter value is invalid.                                                                                                |
| false        | -2303      | The size of list must be 1000 or fewer.                                                                                    |
| false        | -2304      | It is already registered number.                                                                                           |
| false        | -2305      | The 'blockReceiverList' is required.                                                                                       |
| false        | -2306      | Invalid upload file.                                                                                                       |
| false        | -2307      | Fail to read files.                                                                                                        |
| false        | -2308      | Invalid email address.                                                                                                     |
| false        | -2309      | BlockReceiver add failed.                                                                                                  |
| false        | -2310      | Duplicate BlockReceiver.                                                                                                   |
| false        | -2500      | Invalid parameter.                                                                                                         |
| false        | -2501      | Failed update result.                                                                                                      |
| false        | -2502      | Not found the request. messageId: {}, seq:{}                                                                               |
| false        | -2700      | Domain is empty.                                                                                                           |
| false        | -2701      | Domain Verification failed.                                                                                                |{})|
| false        | -2702      | Already existed domain.                                                                                                    |
| false        | -2703      | Domain is not verified.                                                                                                    |
| false        | -2704      | Domain is not Root Domain.                                                                                                 |
| false        | -2705      | Already verified domain.                                                                                                   |
| false        | -2706      | Subdomain must not be root                                                                                                 |
| false        | -2707      | Invalid subdomain parameter.                                                                                               |
| false        | -2708      | No regist subdomain                                                                                                        |
| false        | -2709      | No same root domain.                                                                                                       |senderAddress, X-TC-ENVELOPE-FROM)|
| false        | -2710      | X-TC-ENVELOPE-FROM must be email format.                                                                                   |
| false        | -2711      | Invalid domain parameter.                                                                                                  |
| false        | -2712      | No regist domain                                                                                                           |
| false        | -2713      | DKIM Verification failed.                                                                                                  |
| false        | -2714      | Domain sharing failed.                                                                                                     |
| false        | -2715      | DKIM deactivate Failed                                                                                                     |
| false        | -2716      | DKIM activate Failed                                                                                                       |
| false        | -2717      | Root Domain is not verified.                                                                                               |
| false        | -2718      | Already shared domain.                                                                                                     |
| false        | -2719      | This domain is not sharing another organization user.                                                                      |
| false        | -2720      | DMARC record is empty.                                                                                                     |
| false        | -2721      | SPF Record is duplicated. check your domain txt record.                                                                    |
| false        | -2722      | SPF record could not be found because there were too many DNS lookups. DNS lookup limit is 10.                             |
| false        | -2723      | Invalid all position SPF record. all position must be last position. check your domain txt record.                         |
| false        | -3000      | Invalid add category parameter.                                                                                            |categoryName, useYn)|
| false        | -3001      | Invalid category                                                                                                           |
| false        | -3002      | Invalid modify category parameter.                                                                                         |categoryId, categoryName, useYn)|
| false        | -3003      | The 'categoryName' is required.                                                                                            |
| false        | -3004      | The 'categoryName' can't exceed 200 characters.                                                                            |
| false        | -3005      | The 'categoryDesc' can't exceed 1,000 characters.                                                                          |
| false        | -3006      | Not found the category. {}: {}                                                                                             |
| false        | -3007      | The category is the default and you cannot delete it.                                                                      |
| false        | -3008      | Category does not exist.                                                                                                   |
| false        | -4000      | Not found excel file.                                                                                                      |
| false        | -4001      | Invalid parameter. {}                                                                                                      |
| false        | -4002      | Failed to upload export file                                                                                               |
| false        | -4003      | Failed to import file                                                                                                      |
| false        | -4004      | Failed to get export file list count                                                                                       |
| false        | -4005      | Failed to get export file list                                                                                             |
| false        | -4006      | Failed to get import file info                                                                                             |
| false        | -4007      | Failed to download export file                                                                                             |
| false        | -4008      | Failed to download import file                                                                                             |
| false        | -4009      | Failed to importing file                                                                                                   |
| false        | -4010      | The service is not allowed to request/download export file.                                                                |
| false        | -5000      | Invalid parameter                                                                                                          |
| false        | -5001      | Already activated appkey.                                                                                                  |
| false        | -5002      | Not activated appkey.                                                                                                      |
| false        | -5003      | Not exist appkey.                                                                                                          |
| false        | -5004      | Fail to make provisioned resource.                                                                                         |
| false        | -5005      | The 'projectId' is invalid.                                                                                                |
| false        | -6000      | Already provisioned resource                                                                                               |
| false        | -6001      | Invalid provisioned resource                                                                                               |
| false        | -6002      | Invalid provisioned parameter.                                                                                             |
| false        | -6003      | Invalid provisioned group resource                                                                                         |
| false        | -6004      | Invalid provisioned group parameter.                                                                                       |
| false        | -7000      | Fail to call Tag API                                                                                                       |
| false        | -7001      | Invalid parameter.                                                                                                         |
| false        | -7002      | The 'tagExpression' is required.                                                                                           |
| false        | -7003      | Uploaded file is empty.                                                                                                    |
| false        | -7004      | The 'uid' is required, and can not contain the comma character                                                             |,).|
| false        | -7005      | Invalid email address.                                                                                                     |
| false        | -7006      | The 'uids' is required                                                                                                     |
| false        | -8000      | It failed to read that files                                                                                               |
| false        | -9993      | Binding the requests error.                                                                                                |
| false        | -9994      | Client exception.                                                                                                          |
| false        | -9995      | Invalid request. {}                                                                                                        |
| false        | -9996      | It supports only the content of a type 'application/json'.                                                                 |
| false        | -9997      | Can not read http message.                                                                                                 |
| false        | -9998      | Not exist API.                                                                                                             |
| false        | -9999      | Internal error.                                                                                                            |
