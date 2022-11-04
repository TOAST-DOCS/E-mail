## Notification > Email > SMTP Guide

[SMTP 도메인]

|도메인 |
|---|
|smtp-mail.cloud.toast.com |

| TLS/SSL | 포트 |
|---|---|
| STARTTLS | 25, 587, 2587 | 
| TLS Wrapper | 465, 2465 | 

## 암호화 연결
### STARTTLS 연결
25, 587, 2587 포트를 통해 명시적 SSL을 사용하는 방법
```
openssl s_client -crlf -quiet -starttls smtp -connect smtp-mail.cloud.toast.com:587
```

### TLS Wrapper 연결
465, 2465 포트를 통해 암시적 SSL을 사용하는 방법
```
openssl s_client -crlf -quiet -connect smtp-mail.cloud.toast.com:465
```

## SMTP 자격 증명
인증 메커니즘은 PLAIN, LOGIN 두 방식을 선택하여 사용 할 수 있습니다.</br>
인증 방식에 사용할 자격 증명은 아래 값을 참고합니다.

| 값 | 설명 |
|---|---|
| 사용자 이름 | NHN Cloud Email 서비스의 AppKey | 
| 비밀번호 | NHN Cloud Email 서비스의 SecretKey | 

### PLAIN 인증 방식
PLAIN 인증 방식은 **사용자 이름**, **비밀번호**를 한 줄의 Base64로 인코딩하여 자격 증명을 시도합니다.</br>
**사용자 이름**, **비밀번호**를 한 줄의 Base64로 인코딩 하는 방법입니다.
```bash
echo -ne "\0AppKey\0SecretKey" | openssl enc -base64
AEFwcEtleQBTZWNyZXRLZXk=
```

```bash
$ telnet smtp-mail.cloud.toast.com 25
Trying smtp-mail.cloud.toast.com...
Connected to smtp-mail.gtm.toastoven.net.
Escape character is '^]'.
220 smtp-mail.cloud.toast.com SMTP Server (NHN Cloud Email SMTP) ready
ehlo a
250-smtp-mail.cloud.toast.com Hello a [10.162.169.253])
250-PIPELINING
250-ENHANCEDSTATUSCODES
250-8BITMIME
250-STARTTLS
250-AUTH LOGIN PLAIN
250 AUTH=LOGIN PLAIN
auth plain AEFwcEtleQBTZWNyZXRLZXk=
235 Authentication Succesfull
```

### LOGIN 인증 방식
LOGIN 인증 방식은 **사용자 이름**, **비밀번호**를 각각 Base64로 인코딩하여 자격 증명을 시도합니다.</br>
**사용자 이름**, **비밀번호**를 각각 Base64로 인코딩 하는 방법입니다.
```bash
echo -n "AppKey" | openssl enc -base64
QXBwS2V5

echo -n "SecretKey" | openssl enc -base64
U2VjcmV0S2V5
```

```bash
$ telnet smtp-mail.cloud.toast.com 25
Trying smtp-mail.cloud.toast.com...
Connected to smtp-mail.gtm.toastoven.net.
Escape character is '^]'.
220 smtp-mail.cloud.toast.com SMTP Server (NHN Cloud Email SMTP) ready
ehlo a
250-smtp-mail.cloud.toast.com Hello a [10.170.128.251])
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
235 Authentication Succesfull
```