## Notification > Email > 問題解決ガイド

### SPF(Sender Policy Framework)の登録
NHN Cloud Emailで送信するメールがスパム処理されないようにする事前準備として、送信者メールアドレスのドメインについてSPFレコードを登録する方法を案内します。

#### 現象と原因

* 現象：送信したメールが受信者メールアドレスに送信されず、スパム処理されて送信者に返送される。
* 原因：送信者のドメインのSPFレコードが未登録


#### SPFとは
|用語|	説明|
|---|---|
|SPFレコード|DNS(Domain Name System)レコードのタイプの1つで、ドメインを代表してメールを送ることができる正当なメールサーバーを指定し、開示するものです。|

* **送信メール**に入力するメールアドレスのドメインのゾーンにおいて、NHN Cloudで指定するSPFレコードの内容を反映させます。
* ドメインにSPFレコードがない場合、受信者はメールが認証されたメールサーバーで送信されたものかを確認できないため、メールが受信拒否されることがあります。

#### NHN Cloud SPFレコードの設定
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

### Gmailでレピュテーションが低く査定される問題

Gmailのレピュテーション（評判）に関する、簡単なガイドです。
Gmailで、どのような要素でレピュテーションが決められるのか、評価を向上、維持する方法について短く説明します。
詳しい内容は、下記の参考文書を確認してください。

#### レピュテーション(評判)とは
メール送信時、ISP(Inbox Service Provider、Gmail、受信SMPTサーバー)は、送信SMTPサーバーのレピュテーションを確認して受信するかどうかを決定し、迷惑メールを分類します。 レピュテーション評価方式に合わないメールを送信すると、`送信速度低下`と、該当ISPを使用する`受信者のメール受信が困難になる`場合があります。
レピュテーションには、IPレピュテーション、ドメインレピュテーションがあります。
* IPレピュテーション：IPは送信SMTPのIPアドレスです。送信SMTPが持つIPの評判を意味します。
* ドメインレピュテーション：ドメインはメール送信ドメインです。 メール送信の主体になるドメインの評判を意味します。たとえばNHNのドメインは「nhn.com」のドメインレピュテーションは、これまでnhn.comのドメインの送信者から、送信されたメールが、受信者にどう受け入れられたかというデータを元に評価されます。

#### Gmailのレピュテーション評価方式
##### ドメインレピュテーションをより重視します。
複数のドメインがIPを共有して送信する場合が多いため、IPの評価と比べドメインに重きを置きます。受信者の不満（Complaints）から発する行動より、積極的に参加する行動（Engagement）を重視します。 不満から発する行動とは、受信者の迷惑メール処理などです。 積極的に参加する行動は、受信者によるメールの開封および本文中のリンクのクリック、迷惑メール処理を解除する行為などを意味します。
##### その他にもさまざまなものを評価する。
個人的なメールかどうか、送信IPの評判はどうか、メール内にリンクが含まれるか、メールの内容など、複数の要素を評価して査定します。

#### 評判を上げる方法
##### 準備運動(ウォームアップ)が必要
最初から新しいIPとドメインで大量のメールを送信するのは、評価を下げる可能性があり、歓迎されません。数日にわたって50、100、500、1000…と、徐々に増やす必要があります。
##### 積極的な参加行動を行う率の高い受信者を対象に送信
Gmailに送信するメールは、受信者が望むものだということを証明する必要があります。ウォームアップの時、一般的なユーザーよりずっと忠誠心が高く、メールが来ることを待っているユーザーに送信すると、評価を上げることができます。
##### ユーザーが購読の登録、削除をコントロールできる
受信の有効化、無効化をメール受信者側が設定できるように、サービス側で機能を提供する必要があります。
##### 関連性の高いメッセージ

#### 参考
Gmail Inbox Delivery and Domain Reputation: What You Need to Know Now - [Bronto](https://bronto.com/)
Domain Reputation Or IP Reputation: Which One Does Gmail Care About More? - [Mailgun](https://www.mailgun.com)
