## Notification > Email > 오류 코드

### 오류 코드
|service|isSuccessful|resultCode|resultMessage|
|-|-|-|-|
|공통|true|0|success|
|공통|true|1000|Some of your requests are successful.|
|공통|false|-1000|Invalid appKey.|
|공통|false|-1001|Service is not exist.|
|공통|false|-1002|Service is disabled.|
|발송|false|-2000|Date format error.|
|발송|false|-2001|The 'receiverList' or 'receiver' is required.|
|발송|false|-2002|The 'senderAddress' is required.|
|발송|false|-2003|Invalid attach file name.|
|발송|false|-2004|Not exist file.|
|발송|false|-2006|Not exist data.|
|발송|false|-2007|Invalid requestId.|
|발송|false|-2008|Upload attach file error.|
|발송|false|-2009|The 'requestId' or 'startSendDate' is required.|
|발송|false|-2010|Sender Email Format is wrong.|
|발송|false|-2011|Receiver Email Format is wrong.|
|발송|false|-2012|The maximum number of recipients is 1000.|
|발송|false|-2013|The file size is less than 10M.|
|발송|false|-2014|Up to 5 attachments including the template's are allowed.|
|발송|false|-2015|The 'title' is required.|
|발송|false|-2016|The 'body' is required.|
|발송|false|-2017|The 'receiveMailAddress' is required.|
|발송|false|-2018|The 'receiveType' is required.|
|발송|false|-2019|The 'receiveType' is invalid.|
|발송|false|-2020|The 'fileId' is invalid.|
|발송|false|-2021|The size of total files is too large.|
|발송|false|-2023|In the case of an advertising mail, the phrase '(광고)' must be placed in front of the title.|
|발송|false|-2024|Invalid file extension.|
|발송|false|-2025|The 'userId' is required.|
|발송|false|-2026|The file is not exist.|
|발송|false|-2027|The file was expired.|
|발송|false|-2028|Limit of rows is exceeded. It can handle up to 50,000 rows.|
|발송|false|-2029|The 'summary' property can not exceed 200 characters.|
|발송|false|-2030|The 'customHeaders' contains an invalid name or body.|
|발송|false|-2031|You can not send in past than requested. Please update the 'requestDate'.|
|발송|false|-2032| All of receivers are failed to send.|
|발송|false|-2034| The 'title' exceeds maximum length. (400 chars)|
|발송|false|-2035| The 'userId' exceeds maximum length. (50 chars)|
|발송|false|-2036| The 'templateId' exceeds maximum length. (50 chars)|
|발송|false|-2037| The 'senderName' exceeds maximum length. (100 chars)|
|발송|false|-2038| The 'senderAddress' exceeds maximum length. (100 chars)|
|발송|false|-2039| The 'senderName' exceeds maximum length. (100 chars)|
|발송|false|-2040| The 'senderAddress' exceeds maximum length. (100 chars)|
|발송|false|-2041| The 'fileName' exceeds maximum length. (100 chars)|
|발송|false|-2042| The 'createUser' exceeds maximum length. (50 chars)|
|발송|false|-2043| There are too much mail at this time. Please send it at another time.|
|발송|false|-2044| The 'fileName' is required.|
|발송|false|-2045| The 'fileBody' is required.|
|발송|false|-2046| The '{}' can have only 'Y' or 'N' values. {}: {}|
|템플릿|false|-2100|The 'templateId' is required.|
|템플릿|false|-2107|Not found the template. templateId: {}|
|템플릿|false|-2111|A template with attachments is not supported. templateId : {}|
|통계|false|-2200|Invalid statistics search parameter.|
|통계|false|-2201|The 'from' must be before 'to'|
|통계|false|-2202|Invalid duration time.|
|통계|false|-2203|The 'from' and 'to' are required.|
|통계|false|-2204|The 'searchType' is required.|
|수신 거부|false|-2300|The 'id' is required.|
|수신 거부|false|-2301|Parameter size is invalid.|
|수신 거부|false|-2302|Parameter value is invalid.|
|수신 거부|false|-2303|The size of list must be 1000 or fewer.|
|수신 거부|false|-2304|It is already registered number.|
|수신 거부|false|-2305|The 'blockReceiverList' is required.|
|수신 거부|false|-2306|Invalid upload file.|
|수신 거부|false|-2307|Fail to read files.|
|수신 거부|false|-2308|Invalid email address.|
|수신 거부|false|-2309|Not found file.|
|수신 거부|false|-2400|Invalid parameter.|
|카테고리|false|-3003|The 'categoryName' is required.|
|카테고리|false|-3004|The 'categoryName' can't exceed 200 characters.|
|카테고리|false|-3005|The 'categoryDesc' can't exceed 1,000 characters.|
|카테고리|false|-3006|Not found the category. categoryId: {}|
|카테고리|false|-3007|The category is the default and you cannot delete it.|
|태그|false|-7000|Fail to call Tag API.|
|태그|false|-7001|Invalid parameter.|
|태그|false|-7002|The 'tagExpression' is required.|
|태그|false|-7003|Uploaded file is empty.|
|태그|false|-7004|The 'uid' is required, and can not contain the comma character(,).|
|태그|false|-7005|Invalid email address.|
|태그|false|-7006|The 'uids' is required|
|공통|false|-9999|Internal Error.|
