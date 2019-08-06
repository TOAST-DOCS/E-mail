## Notification > Email > Error Code

### Error Code
|service|isSuccessful|resultCode|resultMessage|
|-|-|-|-|
|Common|true|0|success|
|Common|true|1000|Some of your requests are successful.|
|Common|false|-1000|Invalid appKey.|
|Common|false|-1001|Service is not exist.|
|Common|false|-1002|Service is disabled.|
|Deliver|false|-2000|Date format error.|
|Deliver|false|-2001|The 'receiverList' or 'receiver' is required.|
|Deliver|false|-2002|The 'senderAddress' is required.|
|Deliver|false|-2003|Invalid attach file name.|
|Deliver|false|-2004|Not exist file.|
|Deliver|false|-2006|Not exist data.|
|Deliver|false|-2007|Invalid requestId.|
|Deliver|false|-2008|Upload attach file error.|
|Deliver|false|-2009|The 'requestId' or 'startSendDate' is required.|
|Deliver|false|-2010|Sender Email Format is wrong.|
|Deliver|false|-2011|Receiver Email Format is wrong.|
|Deliver|false|-2012|The maximum number of recipients is 1000.|
|Deliver|false|-2013|The file size is less than 30M.|
|Deliver|false|-2014|Up to 5 attachments including the template's are allowed.|
|Deliver|false|-2015|The 'title' is required.|
|Deliver|false|-2016|The 'body' is required.|
|Deliver|false|-2017|The 'receiveMailAddress' is required.|
|Deliver|false|-2018|The 'receiveType' is required.|
|Deliver|false|-2019|The 'receiveType' is invalid.|
|Deliver|false|-2020|The 'fileId' is invalid.|
|Deliver|false|-2021|The size of total files is too large.|
|Deliver|false|-2023|In the case of an advertising mail, the phrase '(광고)' must be placed in front of the title.|
|Deliver|false|-2024|Invalid file extension.|
|Deliver|false|-2025|The 'userId' is required.|
|Deliver|false|-2026|The file is not exist.|
|Deliver|false|-2027|The file was expired.|
|Deliver|false|-2028|Limit of rows is exceeded. It can handle up to 50,000 rows.|
|Deliver|false|-2029|The 'summary' property can not exceed 200 characters.|
|Deliver|false|-2030|The 'customHeaders' contains an invalid name or body.|
|Deliver|false|-2031|You can not send in past than requested. Please update the 'requestDate'.|
|Deliver|false|-2032| All of receivers are failed to send.|
|Deliver|false|-2034| The 'title' exceeds maximum length. (400 chars)|
|Deliver|false|-2035| The 'userId' exceeds maximum length. (50 chars)|
|Deliver|false|-2036| The 'templateId' exceeds maximum length. (50 chars)|
|Deliver|false|-2037| The 'senderName' exceeds maximum length. (100 chars)|
|Deliver|false|-2038| The 'senderAddress' exceeds maximum length. (100 chars)|
|Deliver|false|-2039| The 'senderName' exceeds maximum length. (100 chars)|
|Deliver|false|-2040| The 'senderAddress' exceeds maximum length. (100 chars)|
|Deliver|false|-2041| The 'fileName' exceeds maximum length. (100 chars)|
|Deliver|false|-2042| The 'createUser' exceeds maximum length. (50 chars)|
|Deliver|false|-2043| There are too much mail at this time. Please send it at another time.|
|Template|false|-2100|The 'templateId' is required.|
|Template|false|-2107|There is no template with template ID.|
|Template|false|-2111|A template with attachments is not supported. templateId : {}|
|Statistics|false|-2200|Invalid statistics search parameter.|
|Statistics|false|-2201|The 'from' must be before 'to'|
|Statistics|false|-2202|Invalid duration time.|
|Statistics|false|-2203|The 'from' and 'to' are required.|
|Statistics|false|-2204|The 'searchType' is required.|
|Call Reject|false|-2300|The 'id' is required.|
|Call Reject|false|-2301|Parameter size is invalid.|
|Call Reject|false|-2302|Parameter value is invalid.|
|Call Reject|false|-2303|The size of list must be 1000 or fewer.|
|Call Reject|false|-2304|It is already registered number.|
|Call Reject|false|-2305|The 'blockReceiverList' is required.|
|Call Reject|false|-2306|Invalid upload file.|
|Call Reject|false|-2307|Fail to read files.|
|Call Reject|false|-2308|Invalid email address.|
|Call Reject|false|-2309|Not found file.|
|Call Reject|false|-2400|Invalid parameter.|
|Tag|false|-7000|Fail to call Tag API.|
|Tag|false|-7001|Invalid parameter.|
|Tag|false|-7002|The 'tagExpression' is required.|
|Tag|false|-7003|Uploaded file is empty.|
|Tag|false|-7004|The 'uid' is required, and can not contain the comma character(,).|
|Tag|false|-7005|Invalid email address.|
|Tag|false|-7006|The 'uids' is required|
|Common|false|-9999|Internal Error.|
