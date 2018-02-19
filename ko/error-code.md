## Notification > Email > 오류 코드

|service|isSuccessful|resultCode|resultMessage|
|-|-|-|-|
|공통|true|0|SUCCESS|
|공통|false|-1000|Invalid appKey.|
|공통|false|-1001|Service is not exist.|
|공통|false|-1002|Service is disabled.|
|발송|false|-2000|Date format error.|
|발송|false|-2001|ReceiverList is not null.|
|발송|false|-2002|senderAddress is not null.|
|발송|false|-2003|Invalid attach file name.|
|발송|false|-2004|Not exist file.|
|발송|false|-2006|Not exist data.|
|발송|false|-2007|Invalid requestId.|
|발송|false|-2008|Upload attach file error.|
|발송|false|-2009|RequestId or startSendDate is required.|
|발송|false|-2010|Sender Email Format is wrong.|
|발송|false|-2011|Receiver Email Format is wrong.|
|발송|false|-2012|The maximum number of recipients is 1000.|
|발송|false|-2013|The file size is less than 10M.|
|발송|false|-2014|The number of attachments should be less than 4.|
|발송|false|-2015|Title can not be empty.|
|발송|false|-2016|Body can not be empty.|
|발송|false|-2017|The 'receiveMailAddress' can not be empty.|
|발송|false|-2018|The 'receiveType' can not be empty.|
|발송|false|-2019|The 'receiveType' is invalid.|
|발송|false|-2020|The 'fileId' is invalid.|
|발송|false|-2021|The size of total files is too large.|
|발송|false|-2022|Invalid provisioned resource.|
|발송|false|-2023|In the case of an advertising mail, the phrase '(광고)' must be placed in front of the title.|
|발송|false|-2024|Invalid file extension.|
|발송|false|-2025|The 'userId' can not be empty.|
|발송|false|-2026|The file is not exist.|
|발송|false|-2027|The file was expired.|
|발송|false|-2028|Limit of rows is exceeded. It can handle up to 50,000 rows.|
|발송|false|-2029|The 'summary' property can not exceed 200 characters.|
|템플릿|false|-2100|The 'templateId' can not be empty.|
|템플릿|false|-2101|Already existed templateId.|
|템플릿|false|-2102|The 'templateName' can not be empty.|
|템플릿|false|-2103|Send mail address can not be empty.|
|템플릿|false|-2104|SendType can not be empty.(0-sms, 1-mms, 2-auth)|
|템플릿|false|-2105|Body can not be empty.|
|템플릿|false|-2106|UseYn is invalid.|
|템플릿|false|-2107|Invalid template.|
|템플릿|false|-2108|The 'categoryId' can not be empty.|
|템플릿|false|-2109|Invalid Api Request. Check the replace parameter.|
|템플릿|false|-2110|TemplateId must be 10 characters or less.|
|통계|false|-2200|Invalid statistics search parameter.|
|통계|false|-2201|Invalid search period.|
|통계|false|-2202|Invalid duration time.|
|수신 거부|false|-2300|Invalid statistics search parameter.|
|수신 거부|false|-2301|Invalid search period.|
|수신 거부|false|-2302|Invalid duration time.|
|수신 거부|false|-2303|The size of list must be 1000 or fewer.|
|수신 거부|false|-2304|It is already registered number.|
|수신 거부|false|-2305|The list can not be empty.|
|수신 거부|false|-2306|Invalid upload file.|
|수신 거부|false|-2307|Fail to read files.|
|수신 거부|false|-2308|Invalid email address.|
|수신 거부|false|-2309|Not found file.|
|수신 거부|false|-2400|Invalid parameter.|
|수신 거부|false|-2401|Parameter convert failed.|
|카테고리|false|-3000|Invalid add category parameter.(categoryName, useYn)|
|카테고리|false|-3001|Invalid category|
|카테고리|false|-3002|Invalid modify category parameter.(categoryId, categoryName, useYn)|
|엑셀 업로드|false|-4000|Not found excel file.|
|엑셀 업로드|false|-4001|Required excel file => **.xlsx|
|엑셀 업로드|false|-4002|Not duplicated recipient_no.|
|엑셀 업로드|false|-4003|Token create failed.|
|엑셀 업로드|false|-4010|Not found csv file.|
|상품|false|-5000|Invalid parameter|
|상품|false|-5001|Already activated appkey.|
|상품|false|-5002|Not activated appkey.|
|상품|false|-5003|Not exist appkey.|
|상품|false|-5004|Fail to make provisioned resource.|
|상품|false|-5005|The 'projectId' is invalid.|
|상품|false|-6000|Already provisioned resource.|
|상품|false|-6001|Invalid provisioned resource.|
|상품|false|-6002|Invalid provisioned parameter.|
|태그|false|-7000|Fail to call Tag API.|
|태그|false|-7001|Invalid parameter.|
|태그|false|-7002|The tag expression can not be empty.|
|태그|false|-7003|Uploaded file is empty.|
|태그|false|-7004|The UID can not be empty, and can not contain the comma character(,).|
|태그|false|-7005|Invalid email address.|
|태그|false|-8000|It failed to read that files|
|태그|false|-6001|This request can't update status. Check status of this request.|
|대량|false|-6000|This request can't update status. Check status of this request.|

