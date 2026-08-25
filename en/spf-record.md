<!-- pre-align:aligned sig=b814642717fc -->

<a id="notification-email-domain-management-guide-spf"></a>
## Notification > Email > Domain Management Guide > SPF { #notification-email-domain-management-guide-spf }

<a id="what-is-sender-policy-framework-spf"></a>
### What is sender policy framework (SPF)? { #what-is-sender-policy-framework-spf }
- SPF is a mechanism to verify the reliability of email senders and the email server verifies that mail sent from particular domain is actually delivered from authorized server.
- During mail delivery, you can check SPF records registered in the sender's DNS to treat mail from unregistered IP addresses as spam.


<a id="spf-record-authentication-procedure"></a>
### SPF Record Authentication Procedure { #spf-record-authentication-procedure }

<a id="spf-record-authentication-procedure-mail-domain-registration-and-authentication"></a>
#### 1. Mail Domain Registration and Authentication 
- SPF record validation is enabled on web console when the mail domain is registered and authenticated. 
- For detailed guide on mail domain authentication, refer to [Notification > Email > Domain Management Guide > Domain Authentication and Protection](./domain-verification/).

<a id="spf-record-authentication-procedure-nhn-cloud-spf-record-settings"></a>
#### 2. NHN Cloud SPF Record Settings
- Register the following values in DNS's TXT record. Contact your DNS administrator for more information on how to register.

``` 
v=spf1 include:_spfblocka.toast.com ~all 
```

- Even if you finish changing SPF settings for TXT record, it will take up to 48 hours for DNS changes to take effect depending on DNS server situation.
- It is safe to send email a few hours after you have changed the SPF settings.

<a id="spf-record-authentication-procedure-verifying-spf-records"></a>
#### 3. Verifying SPF Records
- Verify that the SPF records in the authenticated domain are registered successfully.
- When registration is complete, the message `SPF Validation Success` is displayed.

![email_202312_06_en.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_06_en.png)

<a id="precautions"></a>
### Precautions { #precautions }
Section 10.1 "Processing Limits" of [RFC 4408](https://datatracker.ietf.org/doc/rfc4408/?include_text=1) specifies inter-DNS Lookup processing protocol.

> SPF implementations MUST limit the number of mechanisms and modifiers that do DNS lookups to at most 10 per SPF check, including any lookups caused by the use of the "include" mechanism or the "redirect" modifier. If this number is exceeded during a check, a PermError MUST be returned. They "include," "a," "mx," "ptr" and "exists" mechanisms as well as the "redirect" modifier do count against this limit. The "all," "ip4" and "ip6" mechanisms do not require DNS lookups and therefore do not count against this limit. The "exp" modifier does not count against this limit because the DNS lookup to fetch the explanation string occurs after the SPF record has been evaluated.

This call contains all queries triggered by the "include" mechanism or "redirect" modifier. If the NHN Cloud Email's spf record `v=spf1 include:_spfblocka.toast.com ~all` exceeds DNS Lookup interaction protocol, it may be processed as spam.





