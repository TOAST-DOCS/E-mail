## Notification > Email > Domain Management Guide > DKIM

### What is DKIM(domainkeys identified mail)?

- DKIM(domainkeys identified mail) is an email verification method that allows email senders to digitally sign email to verify the authenticity of the sender
  and ensure that the message is not tampered with during delivery.
- DKIM prevents spam senders and other malicious attackers from falsifying emails.

### Structure of DKIM

- DKIM uses a digital signature structure based on public key encryption.
- When sending an email, the sending server signs the sender, receiver, title, content, etc. with a secret key. And adds this signature value to the
  DKIM-Signature Header.
- Receiving server lookup DKIM record containing public key and signature algorithm information of the domain described in "d=" field within the DKIM-Signature
  header and uses these values to verify the digital signature of the received email DKIM-Signature header.
- [RFC 6376](https://datatracker.ietf.org/doc/html/rfc6376/) encourages DKIM records to be registered as TXT records, so NHN Cloud Email introduced TXT record
  authentication and activation features.

### Structure of DKIM-Signature

- The following is an example of DKIM signature (DKIM-Signature header) that is added to the email header when sending an email.

> DKIM-Signature: v=1; a=rsa-sha256; d=example.net; s=nhncloud;
> t=1117574938; x=1118006938;
> h=from:to:subject:date;
> bh=MTIzNDU2Nzg5MDEyMzQ1Njc4OTAxMjM0NTY3ODkwMTI=;
> b=dzdVyOfAKCdLXdJOc9G2q8LoXSlEniSbav+yuU4zGeeruD00lszZVoG4ZHRNiYzR

#### DKIM-Signature Header

- Describes the DKIM-Signature header field.
- For more detailed description of the DKIM-Signature structure, refer to [RFC 6376](https://datatracker.ietf.org/doc/html/rfc6376).

| Field |Required or not | Value | Description |
| ---- | --- | --- |
| v | Required | 1 | Version. |
| a | Required | sha-256 | algorithm used for DKIM signature. |
| s | Required | - | - | It is Selector. Sender domain can use multiple DKIMs. Therefore, the DKIM-Signature header should inform the receiving server which
public key this signature should use to authenticate. For example, if the Selector is 'toast', the receiving server checks DKIM record in the DNS of '
toast.\_domainkey.example.com '. |
| h | Required | - | - | Header to sign. Headers are identified by colons (:). |
| b | Required | - | - | The value of headers defined in the header (h) to be signed. Encoded to Base64\. |
| bh | required | - | - | signed value of the body of email. |
| l | Optional | - | - The length of the body used for body signature (b). Use the entire body unless otherwise defined. |
| d || Required | - | - | Domain. Used to lookup DNS for DKIM records to verify DKIM signatures with Selector(s). It may be different from MAIL FROM (5321.From)
and From (5322.From). However, if it is different, it can be classified as suspicious mail. |
| t | Optional | - | Date and Time that DKIM Signature generated. The format is Unix Time. 0 second: 0 minute: 0 hour on January 1, 1970, value that converted
to seconds of elapsed time from UTC (agreed world time). Example, June 8, 2020 at 3hour:47minutes:17seconds PM Unix time: 1591631237 seconds |
| x | Optional | - | Date and time that DKIM signature expired. Format is equal to the date and time (t) of signature generation. |

DKIM Header example above has the following implications.

- Email sender domain is example.net .
- Selector (s) for the email sender domain is nhncloud
- the email sender domain (d) is example.net .
- The DKIM record for the email sender domain is nhncloud.\_domainkey.example.net .
- Receiving server authenticates email by lookup DKIM record at nhncloud.\_domainkey.example.net .

### How to register, authenticate and activate DKIM records

#### 1. Mail Domain Registration and Authentication

- DKIM authentication is enabled on web console when the mail domain is registered and authenticated.
- For detailed guide on mail domain authentication, refer
  to [Notification > Email > Domain Management Guide > Domain Authentication and Protection](./domain-verification/)
  .

#### 2. Register DKIM Records

1. Navigate to **Manage Mail Domain** tab.
2. Click **Set DKIM** button for the registered sender domain.
3. Copy the displayed DKIM record and register it with the domain DNS selected to be sent.

![email\_202312\_04.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_04.png)

##### Precautions

- Even if you finish changing DKIM settings for TXT record, it will take up to 48 hours for DNS changes to take effect depending on the DNS server situation.
- After DKIM setup, it is safe to send an email after a few hours.

#### 3. DKIM Authentication

- If DKIM record is registered, click on **Authentication** button to complete the authentication.
- When authentication is complete, the button **Authentication** changes to **Authenticated**.

#### 4. DKIM Activation

- If authentication is successful, select **DKIM** tab on pop-up screen and click on **Activate** to enable DKIM.

![email\_202312\_05.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_05.png)

#### 5. Test DKIM Authentication

- If DKIM feature is authenticated and enabled, you can send an email to see if the DKIM is authenticated successfully.
- If you click on View Original in the top right view of the email you sent, you can see the DKIM-related header.