## Notification > Email > ドメイン管理ガイド > SPF

### SPF(sender policy framework)とは？

- SPFはメール送信者の信頼性を検証するためのメカニズムで、メールサーバーが特定のドメインから送信されたメールが実際に許可されたサーバーから来たかどうかを確認します。
- メール送信中に送信者のDNSに登録されたSPFレコードを確認し、登録されていないIPアドレスから送信されたメールをスパムメールとして処理できます。

### SPFレコード認証手順

#### 1. メールドメイン登録および認証

- SPFレコード検証は、メールドメインが登録および認証が完了した場合、Webコンソールで有効になります。
- メールドメイン認証に関する詳細ガイドは[Notification > Email > ドメイン管理ガイド > ドメイン認証および保護](https://docs.nhncloud.com/ja/Notification/Email/ja/domain-verification/)
  を参照してください。

#### 2. NHN Cloud SPFレコード設定

- 下記の値をDNSのTXTレコードに登録します。詳しい登録方法はDNS管理会社にお問い合わせください。

```
v=spf1 include:_spfblocka.toast.com ~all
```

- TXTレコードのSPF設定変更作業が終わっても、DNSサーバーの状況により、DNS変更内容が適用されるまで最大48時間かかります。
- SPF設定変更作業後、数時間程度経過してからメールを送信するのが安全です。

#### 3. SPFレコードの検証

- 認証完了したドメインのSPFレコードが正常に登録されたか確認します。
- 登録が完了すると、「SPF検証成功」というメッセージが表示されます。

![email_202312_06_ja.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_06_ja.png)

### 注意事項

[RFC 4408](https://datatracker.ietf.org/doc/rfc4408/?include_text=1)のSection 10.1 "Processing Limits"にはDNS Lookup間の処理規約が記載されています。

> SPF implementations MUST limit the number of mechanisms and modifiers that do DNS lookups to at most 10 per SPF check, including any lookups caused by the use
> of the "include" mechanism or the "redirect" modifier. If this number is exceeded during a check, a PermError MUST be returned. The "include", "a", "mx", "ptr",
> and "exists" mechanisms as well as the "redirect" modifier do count against this limit. The "all", "ip4", and "ip6" mechanisms do not require DNS lookups and
> therefore do not count against this limit. The "exp" modifier does not count against this limit because the DNS lookup to fetch the explanation string occurs
> after the SPF record has been evaluated.

この呼び出しには"include"メカニズムや"redirect"修正者によってトリガーされたすべてのルックアップが含まれます。NHN Cloud Emailのspfレコード`v=spf1 include:_spfblocka.toast.com ~all`がDNS
Lookup間処理規約を超えた場合、スパムメールとして処理される可能性があります。
