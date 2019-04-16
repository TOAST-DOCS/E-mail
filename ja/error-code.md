## Notification > Email > エラーコード

### エラーコード
|service|isSuccessful|resultCode|resultMessage|
|-|-|-|-|
|共通|true|0|success|
|共通|true|1000|Some of your requests are successful.|
|共通|false|-1000|Invalid appKey.|
|共通|false|-1001|Service is not exist.|
|共通|false|-1002|Service is disabled.|
|送信|false|-2000|Date format error.|
|送信|false|-2001|The 'receiverList' or 'receiver' is required.|
|送信|false|-2002|The 'senderAddress' is required.|
|送信|false|-2003|Invalid attach file name.|
|送信|false|-2004|Not exist file.|
|送信|false|-2005|Invalid template type.|
|送信|false|-2006|Not exist data.|
|送信|false|-2007|Invalid requestId.|
|送信|false|-2008|Upload attach file error.|
|送信|false|-2009|The 'requestId' or 'startSendDate' is required.|
|送信|false|-2010|Sender Email Format is wrong.|
|送信|false|-2011|Receiver Email Format is wrong.|
|送信|false|-2012|The maximum number of recipients is 1000.|
|送信|false|-2013|The file size is less than 10M.|
|送信|false|-2014|Up to 5 attachments including the template's are allowed.|
|送信|false|-2015|The 'title' is required.|
|送信|false|-2016|The 'body' is required.|
|送信|false|-2017|The 'receiveMailAddress' is required.|
|送信|false|-2018|The 'receiveType' is required.|
|送信|false|-2019|The 'receiveType' is invalid.|
|送信|false|-2020|The 'fileId' is invalid.|
|送信|false|-2021|The size of total files is too large.|
|送信|false|-2022|Invalid provisioned resource.|
|送信|false|-2023|In the case of an advertising mail, the phrase '(広告)' must be placed in front of the title.|
|送信|false|-2024|Invalid file extension.|
|送信|false|-2025|The 'userId' is required.|
|送信|false|-2026|The file is not exist.|
|送信|false|-2027|The file was expired.|
|送信|false|-2028|Limit of rows is exceeded. It can handle up to 50,000 rows.|
|送信|false|-2029|The 'summary' property can not exceed 200 characters.|
|送信|false|-2030|The 'customHeaders' contains an invalid name or body.|
|送信|false|-2031|You can not send in past than requested. Please update the 'requestDate'.|
|送信|false|-2032| All of receivers are failed to send.|
|送信|false|-2033| The date you are searching is before storage period. (90 days)|
|送信|false|-2034| The 'title' exceeds maximum length. (400 chars)|
|送信|false|-2035| The 'userId' exceeds maximum length. (50 chars)|
|送信|false|-2036| The 'templateId' exceeds maximum length. (50 chars)|
|送信|false|-2037| The 'senderName' exceeds maximum length. (100 chars)|
|送信|false|-2038| The 'senderAddress' exceeds maximum length. (100 chars)|
|送信|false|-2039| The 'senderName' exceeds maximum length. (100 chars)|
|送信|false|-2040| The 'senderAddress' exceeds maximum length. (100 chars)|
|送信|false|-2041| The 'fileName' exceeds maximum length. (100 chars)|
|送信|false|-2042| The 'createUser' exceeds maximum length. (50 chars)|
|送信|false|-2043| There are too much mail at this time. Please send it at another time.|
|テンプレート|false|-2100|The 'templateId' is required.|
|テンプレート|false|-2107|There is no template with template ID.|
|テンプレート|false|-2111|A template with attachments is not supported. templateId : {}|
|統計|false|-2200|Invalid statistics search parameter.|
|統計|false|-2201|The 'from' must be before 'to'|
|統計|false|-2202|Invalid duration time.|
|統計|false|-2203|The 'from' and 'to' are required.|
|統計|false|-2204|The 'searchType' is required.|
|受信拒否|false|-2300|The 'id' is required.|
|受信拒否|false|-2301|Parameter size is invalid.|
|受信拒否|false|-2302|Parameter value is invalid.|
|受信拒否|false|-2303|The size of list must be 1000 or fewer.|
|受信拒否|false|-2304|It is already registered number.|
|受信拒否|false|-2305|The 'blockReceiverList' is required.|
|受信拒否|false|-2306|Invalid upload file.|
|受信拒否|false|-2307|Fail to read files.|
|受信拒否|false|-2308|Invalid email address.|
|受信拒否|false|-2309|Not found file.|
|受信拒否|false|-2400|Invalid parameter.|
|タグ|false|-7000|Fail to call Tag API.|
|タグ|false|-7001|Invalid parameter.|
|タグ|false|-7002|The 'tagExpression' is required.|
|タグ|false|-7003|Uploaded file is empty.|
|タグ|false|-7004|The 'uid' is required, and can not contain the comma character(,).|
|タグ|false|-7005|Invalid email address.|
|タグ|false|-8000|It failed to read that files|
|タグ|false|-6001|This request can't update status. Check status of this request.|
|共通|false|-9999|Internal Error.|
