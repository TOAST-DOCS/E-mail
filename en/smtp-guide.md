<!-- pre-align:aligned sig=6c7113775d99 -->

<a id="notification-email-smtp-guide"></a>
## Notification > Email > SMTP Guide { #notification-email-smtp-guide }

[SMTP Domain]

|Domain |
|---|
|smtp-mail.nhncloudservice.com |

| TLS/SSL | Port |
|---|---|
| STARTTLS | 25, 587, 2587 | 
| TLS Wrapper | 465, 2465 | 

<a id="section-1"></a>
## Encryption Connection { #section-1 }
<a id="starttls"></a>
### STARTTLS Connection { #starttls }
How to use Explicit SSL on ports 25, 587, and 2587
```
openssl s_client -crlf -quiet -starttls smtp -connect smtp-mail.nhncloudservice.com:587
```

<a id="tls-wrapper"></a>
### Connect TLS Wrapper { #tls-wrapper }
How to use Implicit SSL via ports 465 and 2465
```
openssl s_client -crlf -quiet -connect smtp-mail.nhncloudservice.com:465
```

<a id="smtp"></a>
## SMTP Credentials { #smtp }
You can use the authentication mechanism by selecting either PLAIN or LOGIN.</br>
Refer to the following values for the credentials to use with the authentication method.

| Value | Description |
|---|---|
| User name | The service's AppKey |
| Password | SecretKey of NHN Cloud Email service |

<a id="plain"></a>
### PLAIN Authentication Method { #plain }
The PLAIN authentication method attempts to authenticate by encoding the **user name** and **password** in a single line of Base64.</br>
**사용자 이름**, **비밀번호**를 한 줄의 Base64로 인코딩 하는 방법입니다.
```bash
echo -ne "\0AppKey\0SecretKey" | openssl enc -base64
AEFwcEtleQBTZWNyZXRLZXk=
```

```bash
$ telnet smtp-mail.nhncloudservice.com 25
Trying smtp-mail.nhncloudservice.com...
Connected to smtp-mail-nhncloud.gtm.toastoven.net.
Escape character is '^]'.
220 smtp-mail.nhncloudservice.com SMTP Server (NHN Cloud Email SMTP) ready
ehlo a
250-smtp-mail.nhncloudservice.com Hello a [10.162.169.253])
250-PIPELINING
250-ENHANCEDSTATUSCODES
250-8BITMIME
250-STARTTLS
250-AUTH LOGIN PLAIN
250 AUTH=LOGIN PLAIN
auth plain AEFwcEtleQBTZWNyZXRLZXk=
235 Authentication Successful
```

<a id="login"></a>
### LOGIN authentication method { #login }
The LOGIN authentication method attempts authentication by encoding the **user name** and **password** individually in Base64.</br>
**사용자 이름**, **비밀번호**를 각각 Base64로 인코딩 하는 방법입니다.
```bash
echo -n "AppKey" | openssl enc -base64
QXBwS2V5

echo -n "SecretKey" | openssl enc -base64
U2VjcmV0S2V5
```

```bash
$ telnet smtp-mail.nhncloudservice.com 25
Trying smtp-mail.nhncloudservice.com...
Connected to smtp-mail-nhncloud.gtm.toastoven.net.
Escape character is '^]'.
220 smtp-mail.nhncloudservice.com SMTP Server (NHN Cloud Email SMTP) ready
ehlo a
250-smtp-mail.nhncloudservice.com Hello a [10.170.128.251])
250-PIPELINING
250-ENHANCEDSTATUSCODES
250-8BITMIME
250-STARTTLS
250-AUTH LOGIN PLAIN
250 AUTH=LOGIN PLAIN
auth login
334 VXNlcm5hbWU6
QXBwS2V5
334 UGFzc3dvcmQ6
U2VjcmV0S2V5
235 Authentication Successful
```

<a id="smtp-1"></a>
### Mail Usage by Purpose { #smtp-1 }
You can specify a mail type according to the purpose of the mail.</br>
Enter the mail type along with the Appkey during authentication.</br>
If you do not specify a type, the email is sent as the normal type.</br>
</br>
ex) appkey#mailType</br>
</br>
The following types can be specified.

| Type     | Description    |
|--------|-------|
| normal | General mail |
| auth   | Verification mail |
| ad     | Promotional mail |

```bash
# PLAIN 인증 방식
echo -ne "\0AppKey#auth\0SecretKey" | openssl enc -base64

# Login 인증 방식
echo -n "AppKey#ad" | openssl enc -base64

```
