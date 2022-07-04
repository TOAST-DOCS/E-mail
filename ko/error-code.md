## Notification > Email > 오류 코드

### 오류 코드
|isSuccessful|resultCode|resultMessage|
|-|-|-|
|true|0|success|
|true|1000|Some of your requests are successful.|
|false|-1000|Invalid appKey.|
|false|-1001|Service is not exist.|
|false|-1002|Service is disabled.|
|false|-1004|Invalid secretKey.|
|false|-1005|Service is temporarily blocked.|
|false|-2000|Date format error.|
|false|-2001|The 'receiverList' or 'receiver' is required.|
|false|-2002|The 'senderAddress' is required.|
|false|-2003|Invalid attach file name.|
|false|-2004|Not found the file. attachFileId: {}|
|false|-2006|Not exist data.|
|false|-2007|Invalid requestId.|
|false|-2008|Upload attach file error.|
|false|-2009|The 'requestId' or 'startSendDate' is required.|
|false|-2010|Sender Email Format is wrong.|
|false|-2011|Receiver Email Format is wrong.|
|false|-2012|The maximum number of recipients is 1000.|
|false|-2013|The file size is less than 30M.|
|false|-2014|Up to 5 attachments including the template's are allowed.|
|false|-2015|The 'title' is required.|
|false|-2016|The 'body' is required.|
|false|-2017|The 'receiveMailAddress' is required.|
|false|-2018|The 'receiveType' is required.|
|false|-2019|The 'receiveType' is invalid.|
|false|-2020|The 'fileId' is invalid.|
|false|-2021|The size of total files is too large.|
|false|-2023|In the case of an advertising mail, the phrase '(광고)' must be placed in front of the title.|
|false|-2024|Invalid file extension.|
|false|-2025|The 'userId' is required.|
|false|-2026|The file is not exist.|
|false|-2027|The file was expired.|
|false|-2028|Limit of rows is exceeded. It can handle up to 50,000 rows.|
|false|-2029|The 'summary' property can not exceed 200 characters.|
|false|-2030|The 'customHeaders' contains an invalid name or body.|
|false|-2031|You can not send in past than requested. Please update the 'requestDate'.|
|false|-2032| All of receivers are failed to send.|
|false|-2034| The 'title' exceeds maximum length. (400 chars)|
|false|-2035| The 'userId' exceeds maximum length. (50 chars)|
|false|-2036| The 'templateId' exceeds maximum length. (50 chars)|
|false|-2037| The 'senderName' exceeds maximum length. (100 chars)|
|false|-2038| The 'senderAddress' exceeds maximum length. (100 chars)|
|false|-2039| The 'senderName' exceeds maximum length. (100 chars)|
|false|-2040| The 'senderAddress' exceeds maximum length. (100 chars)|
|false|-2041| The 'fileName' exceeds maximum length. (100 chars)|
|false|-2042| The 'createUser' exceeds maximum length. (50 chars)|
|false|-2043| There are too much mail at this time. Please send it at another time.|
|false|-2044| The 'fileName' is required.|
|false|-2045| The 'fileBody' is required.|
|false|-2046| The '{}' can have only 'Y' or 'N' values. {}: {}|
|false|-2047| The 'senderGroupingKey' exceeds maximum length. (100 chars)|
|false|-2048| You can set the size up to 1000. pageSize: {}|
|false|-2050| No registed the sender email address|
|false|-2100|The 'templateId' is required.|
|false|-2102|The 'templateName' is required.|
|false|-2103|The 'sendMailAddress' is required.|
|false|-2105|The 'body' is required.|
|false|-2107|Not found the template. templateId: {}|
|false|-2108|The 'categoryId' is required.|
|false|-2109|Invalid Api Request. Check the replace parameter.|
|false|-2110|TemplateId must be 50 characters or less.|
|false|-2111|A template with attachments is not supported. templateId : {}|
|false|-2112|The 'title' is required.|
|false|-2113|The one of the 'attachFileIdList' has already been attached to another template. attached templateId: {}, attachFileId: {}|
|false|-2200|Invalid statistics search parameter.|
|false|-2201|The 'from' must be before 'to'|
|false|-2202|Invalid duration time.|
|false|-2203|The 'from' and 'to' are required.|
|false|-2204|The 'searchType' is required.|
|false|-2300|The 'id' is required.|
|false|-2301|Parameter size is invalid.|
|false|-2302|Parameter value is invalid.|
|false|-2303|The size of list must be 1000 or fewer.|
|false|-2304|It is already registered number.|
|false|-2305|The 'blockReceiverList' is required.|
|false|-2306|Invalid upload file.|
|false|-2307|Fail to read files.|
|false|-2308|Invalid email address.|
|false|-2309|Not found file.|
|false|-2400|Invalid parameter.|
|false|-3003|The 'categoryName' is required.|
|false|-3004|The 'categoryName' can't exceed 200 characters.|
|false|-3005|The 'categoryDesc' can't exceed 1,000 characters.|
|false|-3006|Not found the category. categoryId: {}|
|false|-3007|The category is the default and you cannot delete it.|
|false|-7000|Fail to call Tag API.|
|false|-7001|Invalid parameter.|
|false|-7002|The 'tagExpression' is required.|
|false|-7003|Uploaded file is empty.|
|false|-7004|The 'uid' is required, and can not contain the comma character(,).|
|false|-7005|Invalid email address.|
|false|-7006|The 'uids' is required|
|false|-9993|Binding the requests error.|
|false|-9994|Client exception.|
|false|-9995|Invalid request. {}|
|false|-9996|It supports only the content of a type 'application/json'.|
|false|-9997|Can not read http message.|
|false|-9998|Not exist API.|
|false|-9999|Internal Error.|
