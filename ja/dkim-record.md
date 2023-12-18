## Notification > Email > ドメイン管理ガイド > DKIM

### DKIM(domainkeys identified mail)とは？

- DKIM(domainkeys identified mail)は、メール送信者がメールにデジタル署名をして送信者の真正性を確認し、送信中にメッセージが改ざんされていないことを確認できるメール認証方法です。
- DKIMにより、スパム送信者やその他の悪意のある攻撃者がメールを偽造することを防止できます。

### DKIMの構造

- DKIMは公開鍵暗号化をベースにしたデジタル署名方式を使用します。
- 送信サーバーはメール送信時にメール送信者、受信者、件名、内容などを秘密鍵で署名します。この署名値をDKIM-Signatureヘッダ(Header)に追加します。
- 受信サーバーは、DKIM-Signatureヘッダ内の"d="フィールドに記述されたドメインの公開鍵と署名アルゴリズム情報などを含むDKIMレコードを照会し、これらの値を利用して受信したメールDKIM-Signatureヘッダのデジタル署名を検証します。
- [RFC 6376](https://datatracker.ietf.org/doc/html/rfc6376/)では、DKIMレコードをTXTレコードとして登録することを推奨しており、NHN Cloud EmailではTXTレコードの認証および有効化機能を導入しました。

### DKIM-Signatureの構造

- 以下はメール送信時にメールヘッダに追加されるDKIM署名(DKIM-Signatureヘッダ)の例です。

> DKIM-Signature: v=1; a=rsa-sha256; d=example.net; s=nhncloud;
> t=1117574938; x=1118006938;
> h=from:to:subject:date;
> bh=MTIzNDU2Nzg5MDEyMzQ1Njc4OTAxMjM0NTY3ODkwMTI=;
> b=dzdVyOfAKCdLXdJOc9G2q8LoXSlEniSbav+yuU4zGeeruD00lszZVoG4ZHRNiYzR

#### DKIM-Signatureヘッダ

- DKIM-Signatureヘッダフィールドについて説明します。
- DKIM-Signature 構造についてのより詳しい説明は [RFC 6376](https://datatracker.ietf.org/doc/html/rfc6376) を参照してください。

| フィールド | 必須かどうか | 値 | 説明 |
|----| ----- | --- | --- |
| v  | 必須 | 1 | バージョンです。 |
| a  | 必須 | sha-256 | DKIM署名に使用されるアルゴリズムです。 |
| s  | 必須 | - | セレクタ(Selector)です。送信ドメインは複数のDKIMを使うことができます。 したがって、DKIM-Signatureヘッダでは、この署名がどの公開鍵を使って認証すべきかを受信サーバーに知らせる必要があります。 例えば、セレクタが'toast'の場合、受信サーバーは'toast.\_domainkey.example.com'のDNSでDKIMレコードを確認します。
| h  | 必須 | - | 署名するヘッダ(Header)です。ヘッダはコロン(:)で区切ります。 |
| b  | 必須 | - | 署名するヘッダ(h)で定義されたヘッダを署名した値です。Base64 でエンコードされます。 |
| bh | 必須 | - | メール本文を署名した値です。 |
| l  | 任意 | - |  本文署名(b)に使用する本文の長さです。 特に定義されていない場合は、本文全体を使用します。 |
| d  | 必須 | - | ドメインです。指定子(s)と一緒にDKIM署名を検証するためにDNSにDKIMレコードを照会するときに使用されます。MAIL FROM(5321.From)、From(5322.From)と異なる場合があります。しかし、異なる場合、疑わしいメールに分類される可能性があります。 |
| t  | 任意 | - | DKIM署名作成日時です。形式はUNIX時間(Unix Time)です。 1970年1月1日0時0分0秒UTC(協定世界時）からの経過時間を秒に換算した値です。例：2020年6月8日午後3時47分17秒のUNIX時間: 1591631237秒 |
| x  | 任意 | - | DKIM署名の有効期限日時です。形式は署名作成日時(t)と同じです。 |

上のDKIM Headerの例を見ると、次のような意味を持ちます。

- メール送信ドメインはexample.netです。
- メール送信ドメインのセレクター(s)はnhncloudです。
- メール送信ドメイン(d)はexample.netです。
- メール送信ドメインのDKIMレコードはnhncloud.\_domainkey.example.netです。
- 受信サーバーはnhncloud.\_domainkey.example.netのDKIMレコードを照会してメールを認証します。

### DKIMレコード登録および認証、有効化方法

#### 1. メールドメインの登録および認証

- DKIM認証は、メールドメインの登録と認証が完了した場合、Webコンソールで有効になります。
- メールドメイン認証関連詳細ガイドは[Notification > Email > ドメイン管理ガイド > ドメイン認証および保護](https://docs.nhncloud.com/ja/Notification/Email/ja/domain-verification/)
  を参照してください。

#### 2. DKIMレコードの登録

1. **メールドメイン管理**タブに移動します。
2. 登録された送信ドメインに**DKIM設定** ボタンをクリックします。
3. 表示されたDKIMレコードをコピーして、送信指定ドメインDNSに登録します。

![email_202312_04_ja.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_04_ja.png)

##### 注意事項

- TXTレコードのDKIM設定変更作業が終わっても、DNSサーバーの状況により、DNS変更内容が適用されるまで最大48時間かかります。
- DKIM設定作業後、数時間程度経過してからメールを送信するのが安全です。

#### 3. DKIM認証

- DKIMレコードが登録されたら、**認証**ボタンをクリックして認証を完了します。
- 認証が完了すると、**認証**ボタンが**認証完了**に変更されます。

#### 4. DKIM有効化

- 認証に成功した場合、ポップアップ画面で**DKIM**タブを選択し、**有効化**をクリックしてDKIMを有効化します。

![email_202312_05_ja.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_05_ja.png)

#### 5. DKIM認証をテストする

- DKIM機能が認証及び活性化された場合、メールを送信してDKIMが正常に認証されるか確認できます。
- 送信したメールの右上のさらに表示をクリックすると、DKIM関連ヘッダを確認できます。
