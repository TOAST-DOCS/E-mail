## Notification > Email > SMTPガイド

[SMTPドメイン]

|ドメイン |
|---|
|smtp-mail.nhncloudservice.com |

| TLS/SSL | ポート |
|---|---|
| STARTTLS | 25, 587, 2587 | 
| TLS Wrapper | 465, 2465 | 

## 暗号化接続
### STARTTLS接続
25、587、2587ポートを介して明示的SSLを使用する方法
```
openssl s_client -crlf -quiet -starttls smtp -connect smtp-mail.nhncloudservice.com:587
```

### TLS Wrapper接続
465、2465ポートを介して暗黙的SSLを使用する方法
```
openssl s_client -crlf -quiet -connect smtp-mail.nhncloudservice.com:465
```

## SMTP認証情報
認証メカニズムはPLAIN、LOGINの2つの方式を選択して使用できます。</br>
認証方式に使用する認証情報は以下の値を参照します。

| 値 | 説明 |
|---|---|
| ユーザー名 | NHN Cloud EmailサービスのAppKey | 
| パスワード | NHN Cloud EmailサービスのSecretKey | 

### PLAIN認証方式
PLAIN認証方式は**ユーザー名**、**パスワード**を1行のBase64でエンコードして認証情報を試みます。</br>
**ユーザー名**、**パスワード**を1行のBase64でエンコードする方法です。
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

### LOGIN認証方式
LOGIN認証方式は**ユーザー名**、**パスワード**をそれぞれBase64でエンコードして認証情報を試みます。</br>
**ユーザー名**、**パスワード**をそれぞれBase64でエンコードする方法です。
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

### 用途別メールの使用
メールの用途に応じてメールタイプを指定できます。</br>
メールタイプは認証時にAppkeyと一緒に入力してください。</br>
タイプを指定しない場合、normalタイプでメールが送信されます。</br>
</br>
ex) appkey#mailType</br>
</br>
指定可能なタイプは次のとおりです。

| タイプ  | 説明 |
|--------|-------|
| normal | 一般メール | 
| auth   | 認証メール |
| ad     | 広告メール |

```bash
# PLAIN認証方式
echo -ne "\0AppKey#auth\0SecretKey" | openssl enc -base64

# Login認証方式
echo -n "AppKey#ad" | openssl enc -base64

```
