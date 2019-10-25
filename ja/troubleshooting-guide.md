## Notification > Email > 問題解決ガイド

### SPF(Sender Policy Framework)の登録
TOAST Emailで送信するメールがスパム処理されることがあるため、次のようにSPFの登録方法を案内します。

#### 現象と原因

* 現象：送信したメールが受信者メールアドレスに送信されず、スパム処理されて返送される。
* 原因：SPFの未登録

#### SPFとは
|用語|	説明|
|---|---|
|SPFレコード|DNS(Domain Name System)レコードのタイプの1つで、ドメインを代表してメールを送ることができるメールサーバーを識別します。|

* **送信メール**に入力するメールアドレスのドメインに対するSPFレコードを作ります。
* ドメインにSPFレコードがない場合、受信者はメールが認証されたメールサーバーで送信されたものかを確認できないため、メールが受信拒否されることがあります。

#### TOAST SPFレコードの設定
下記の値をDNSのTXTレコードに登録します。詳細な登録方法はDNS管理業者にお問い合わせください。
```
v=spf1 include:_spfblocka.toast.com ~all
```
TXTレコードのSPF設定変更作業が終わっても、DNSの変更が完全に反映されるまで、10分から最長24時間程度、かかる場合があります。SPF設定変更作業が終わってから、数時間経過した後にメールを送信するのが安全です。

#### DNSのTXTレコードを確認する方法
Linux環境
```
nslookup -q=TXT <your.domain.name>
```
```
dig -t TXT <your.domain.name>
```
Windows環境
```
nslookup -q=TXT <your.domain.name>
```

### Gmail受信確認関連

受信者のメールがGmailの場合、メールを開封したかどうかを収集できません。開封確認のために、イメージを埋め込む方式を使用しますが、Gmailでは中間サーバーが該当イメージを追跡できないように変更されます。これはGmailで意図的にブロックしたもので、技術的に開封情報を収集するのは不可能です。

```
Gmail Image Proxy

Because the Gmail Image Proxy service does not forward users' cookies, you can't use the measurement protocol to track Gmail users. The Gmail Image Proxy service prevents this by having the measurement protocol requests passed through an intermediate server.
```

#### 参照
[Google Analytics > Email Tracking - Measurement Protocol](https://developers.google.com/analytics/devguides/collection/protocol/v1/email)

### Gmailの低い評判(low reputation)問題

Gmailの評判(reputation)に関する、簡単なガイドです。
Gmailで、どのように「評判」を評価するか、「評判」を向上、維持する方法について短く説明します。
詳しい内容は、下記の参考文書を確認してください。

#### 評判(reputation)とは
メール送信時、ISP(Inbox Service Provider、Gmail、受信SMPTサーバー)は、送信SMTPサーバーの評判を評価して受信するかどうかを決定し、迷惑メールを分類します。 ISPの評判評価方式に合わないメールを送信すると、`送信速度低下`と、該当ISPを使用する`受信者のメール受信が困難になる`場合があります。
評判には、IP評判、ドメイン評判があります。
* IP評判：IPは送信SMTPのIPです。送信SMTPが持つIPの評判を意味します。
* ドメイン評判：ドメインはメール送信ドメインです。 NHNのドメインは「nhn.com」です。メール送信の主体になるドメインの評判を意味します。

#### Gmailの評判評価方式
##### ドメインの評判をより重要視します。
複数のドメインがIPを共有して送信する場合が多いため、IPよりもドメインを重要視します。Complaints(不満)より、Engagement(参加)を重要視します。Complaintsとは、受信者の迷惑メール処理などを意味します。 Engagementは、受信者によるメール開封および本文中のリンクのクリック、迷惑メール処理解除などを意味します。
##### その他にもさまざまなものを評価する。
個人的なメールかどうか、送信IPの評判はどうか、メール内にリンクが含まれるか、メールの内容など、複数の要素を評価して評判を決定します。

#### 評判を上げる方法
##### 準備(warm-Up)が必要
最初から新しいIPとドメインで大量のメールを送信するのは、評判を下げる可能性があるため、望ましくありません。数日にわたって50、100、500、1000…と、徐々に増やす必要があります。
##### Engagementが高い受信者を対象に送信
Gmailに送信するメールは、受信者が望むものだということを証明する必要があります。Warm-Upの時、一般的なユーザーよりずっと忠誠心が高く、メールが来ることを待っているユーザーに送信すると、評判を上げることができます。
##### Permissionsの提供
サービスで、受信するかどうかを設定できるように機能を提供する必要があります。
##### Relevancy
評判を維持するには、メールの内容と関連性がある対象に送信する必要があります。ただメールを頻繁に送ることよりもっと重要です。送信対象のサイズは重要ではありません。メール内容と対象を細分化して送信する必要があります。購読キャンセルの原因の66%は関連性がないメール、55%はメッセージの配信が多すぎることです。

#### 参考
Gmail Inbox Delivery and Domain Reputation: What You Need to Know Now - [Bronto](https://bronto.com/)
Domain Reputation Or IP Reputation: Which One Does Gmail Care About More? - [Mailgun](https://www.mailgun.com)