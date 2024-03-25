## Notification > Email > Domain management Guide > Domain Authentication and Protection

### Enhanced email security feature

#### Enhanced mail domain protection feature

Mail domain protection is a security feature provided by NHN Cloud Email. It prevents third parties from using the mail domain you own within the NHN Cloud
Email.
<br> Once you registered the mail domain on NHN Cloud Email console and confirm that you own it, other projects that are not approved would not be able to send
emails using that mail domain.

### Register mail domains and verify ownership

- First, you need to register your mail domain on NHN Cloud Email console and verify your ownership of mail domain.
- Mail domain owner registers TXT record provided by NHN Cloud Email to mail domain DNS.
- NHN Cloud Email checks ownership by matching TXT records in mail domain DNS.

### Mail domain authentication and protection procedures

#### Register mail domain

![email_202312_00.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_00_en.png)

1. Navigate to email console.
2. Navigate to **Manage Mail Domain** tab.
3. Click **Register Mail Domain** button and register the domain to use when sending mail. (Only the highest domain can be registered)

#### Mail domain authentication (ownership verification procedure)

![email_202312_01.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_01_en.png)

1. Click **Authentication**. Created token is displayed in **Mail Domain Authentication** pop-up screen.
2. Add TXT record to the registered mail domain DNS.
3. If the added TXT record is reflected, click on **Authenticate** to verify the ownership of mail domain.
4. Like SPF, you can use the 'nslookup' and 'dig' commands to see if TXT record for ownership verification has been reflected in mail domain DNS.

#### How to check TXT record in DNS

Linux environment

``` 
nslookup -q=TXT <your.domain.name> 
```

``` 
dig -t TXT <your.domain.name> 
```

Windows environment

```
nslookup -q=TXT <your.domain.name> 
```

### Sharing Domains

- Before protecting mail domains, if you are using a domain that is registered for another project, you need to share the domain.
- If you enable domain protection without sharing, sending mail from an unshared project fails. Therefore, you must share the mail domain if using it for
  multiple projects.

### Share Domains

![email_202312_02.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_02_en.png)

1. Click **Share>Settings** button for the mail domain item you registered.
2. Navigate to the project you want to share and check NHN Cloud Email app key. Register the verified app key. When the sharing is complete, project information
   appears in the list. Appkey can be found in the top right corner **URL and Appkey**.

### Protecting Mail Domains

![email_202312_03.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_03_en.png)

Once you have registered and verified ownership for the mail domain, you can enable protection feature.

1. Click **Protection Status > Protect** for the mail domain item for which you want to enable protection feature.
2. **Apply Protection Guide**, click on **Protect** to activate the feature.
3. Once activated, mail domain protection is complete, and that mail domain is not available to other projects during delivery.

