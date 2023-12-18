## Notification > Email > ドメイン管理ガイド > DMARC

### DMARC (domain-based message authentication reporting and conformance)とは？

メールセキュリティ強化可能の最終段階であるDMARCは、メールスプーフィングを利用したフィッシング、詐欺などを防ぐためのドメインベースメッセージ認証に対する報告および遵守ポリシーです。
<br>受信サーバーは、送信者アドレス(From)
ドメインのDNSでDMARCレコードを照会します。DMARCレコードに定義されたポリシーに従って、受信サーバーは受信したメールを認証します。DMARCポリシーは、SPFとDKIMを使用するかどうか、それぞれの認証手段が失敗したときのメール処理方法はどうなるかで構成されています。
<br>一部のメールサービス(ex. GmailやYahooなど)は、DMARCを適用しない場合、スパムメールとして認識し、送信をブロックします。メール送信間の高いメール到達率のためには、DMARCレコードを使用することを推奨します。

### DMARC DNSレコードの構造

DMARC DNSレコードは'_dmarc.example.com'のように、DMARCを適用する送信ドメインに'_dmarc'を付けたサブドメインDNSにレコードを登録します。

以下はDMARC DNSレコードの例です。

```
"v=DMARC1;p=none;sp=quarantine;pct=100;rua=mailto:dmarcreports@example.com;"
```

DMARCレコードに使われる値について説明します。詳細は[RFC 7489](https://www.ietf.org/rfc/rfc7489.txt)をご覧ください。

| 区分 | 必須かどうか | 値                       | 説明                                                                                       |
| --- | ----- |--------------------------|-------------------------------------------------------------------------------------------|
| v | 必須 | DMARC1 (固定)              | バージョンです。                                                                                    |
| p | 必須 | none, quarantine, reject | 失敗時の処理ポリシーです。                                                                        |
| sp | 任意 | none, quarantine, reject | サブドメインに対する失敗処理ポリシーです。                                                                   |
| pct | 任意 | 0～100 (デフォルト値100)       | ポリシーを適用するメールの割合です。例えば、50の場合、受信したメールの半分がDMARCポリシーによって認証されます。                         |
| adkim | 任意 | s, r (デフォルト値)               | DKIM アライメント (Alignment). DKIM-Signatureのドメイン(d)とFrom (5322.From)の一致水準についての設定です。       |
| aspf | 任意 | s, r (デフォルト値)               | SPF アライメント (Alignment). SPF認証時のMAIL FROM (5321.From)とFrom (5322.From)の一致水準についての設定です。 |
| rua | 任意 |                          | 定期的に集計した失敗レポートを受信するアドレスです。 例：mailto:demarc-report@nhncloud.com                         |
| ruf | 任意 |                          | 失敗レポートを受信するアドレスです。例：mailto:demarc-report@nhncloud.com                                   |
| fo | 任意 | 0 (デフォルト値), 1, d, s         | 失敗レポート(ruf)を作成する基準です。                                                                  |
| rf | 任意 | afrf (固定)                | 失敗レポート(ruf)形式についての設定です。                                                                |
| ri | 任意 | 86400 (単位秒、デフォルト値)       | 失敗を集計する期間です。設定された周期ごとに失敗レポート(rua)が送信されます。                                               |

#### 失敗ポリシー(p)値種類

| 失敗ポリシー | 説明                                                                                                                         |
| ----- |-----------------------------------------------------------------------------------------------------------------------------|
| none | 受信サーバーで失敗に対して何も処理しないようにします。SPF、DKIMを使用していない状況で設定できます。                                                    |
| quarantine | 受信サーバーは失敗したメールをスパムとして処理することを望みます。                                                                                            |
| reject | 受信サーバーでDMARC失敗が発生したメールを返送します。一般的に、送信サーバーと受信サーバーがSMTP通信時にDSN(Delivery Status Notification)レスポンスで行われることを好むポリシーです。 |

#### SPFとDKIM アライメント (aspf, adkim)値の説明

| ポリシー | 説明                                                                                |
| --- |------------------------------------------------------------------------------------|
| s | 厳格(Strict)です。ドメイン部分が完全に一致する必要があります。                                             |
| r | 柔軟(Relexed)です。サブドメインも可能です。例えば'd=example.com'の場合、'From: news.example.com'通過 |

### 失敗レポート作成基準(fo)の値説明

- 失敗レポート(ruf)を設定すると使用されます。

| 基準 | 説明 |
| --- | --- |
| 0 | SPF、DKIMの両方が失敗したときに報告します。 |
| 1 | SPF、DKIMnoいずれかが失敗したときに報告します。 |
| d | DKIM認証に失敗した場合に報告します。 |
| s | SPF認証に失敗した場合に報告します。 |

### DMARCレコード検証手順

#### 1. メールドメイン登録および認証

- DMARCレコード検証はメールドメインが登録および認証が完了した場合、ウェブコンソールで有効になります。
- メールドメイン認証関連詳細ガイドは[Notification > Email > ドメイン管理ガイド > ドメイン認証および保護](https://docs.nhncloud.com/ja/Notification/Email/ja/domain-verification/)
  を参考してください。

#### 2. DMARCレコードDNS登録

- DMARCレコードを登録する方法を説明します。詳しい登録方法はDNS管理業者にお問い合わせください。
- DMARCレコードは'_dmarc.example.com'のようにDMARCを適用する送信ドメインに'_dmarc'を付けたドメインDNSを登録し、適切なDMARCポリシーを策定してTXTレコードとして登録する必要があります。
- 例えば、登録する内容は次のとおりです。 DMARC失敗時にレポートを受信するアドレスを設定する必要があります。

```
"v=DMARC1; p=none; fo=1; rua=mailto:${レポートを_受信する_アドレス}"
```

- 登録が完了したら、'nslookup', 'dig'コマンドを利用してDMARC DNSレコードがDNSに反映されたか確認できます。

#### 注意事項

- TXTレコードのDMARC設定変更作業が終わっても、DNSサーバーの状況により、DNS変更内容が適用されるまで最大48時間かかります。
- DMARC設定作業後、数時間程度経過してからメールを送信するのが安全です。

#### 3.DMARC認証

- DMARCレコードのDNS登録が完了したら、DMARC管理ポップアップでDMARC認証を行います。
- 認証が完了すると、**認証完了**という文言が表示されます。
  ![email_202312_08_ja.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_08_ja.png)

#### DMARCレコード照会失敗画面例

- DMARCレコード照会に失敗した場合、次のような画面が表示されます。

![email_202312_09_ja.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_09_ja.png)

### 注意事項

#### 1. SPF関連注意事項

[RFC 7489](https://www.ietf.org/rfc/rfc7489.txt) 文書を参考にする場合、一部の受信サーバーでSPFをDMARC検証ロジックを先に実装できます。この時、SPFレコードに -all のような
-接頭辞がある場合、DMARC認証に失敗することがあります。一部の受信サーバーでDMARC認証に失敗する場合、SPFレコードに -all のような -接頭辞を削除してDMARC認証を再試行してください。

#### 2. DMARC関連注意事項

DMARCで注意すべき点は、受信サーバーがDMARCポリシーに合わせて処理することを完全に保証しないということです。 DMARCは送信サーバーが受信サーバーにポリシーを提案するレベルで理解する必要があります。
例えば、失敗ポリシーを「none」に設定しても、受信サーバーは認証に失敗したメールをスパムとして処理することがあります。

NHN Cloudでは[RFC 7489](https://www.ietf.org/rfc/rfc7489.txt)
文書に従ってDMARC検証を行います。一部の受信サーバーでメールの受信が失敗する場合は、[サポート > 1:1お問い合わせ](https://www.nhncloud.com/kr/support/inquiry)からお問い合わせください。
