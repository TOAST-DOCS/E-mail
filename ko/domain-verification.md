## Notification > Email > 도메인 관리 가이드 > 도메인 인증 및 보호

### 이메일 보안 강화 기능

#### 메일 도메인 보호 기능 
메일 도메인 보호 기능은 NHN Cloud Email에서 제공하는 보안 기능입니다. NHN Cloud Email 내에서 내가 소유한 메일 도메인을 제3자가 사용하는 것을 방지합니다. 
<br> NHN Cloud Email 콘솔에서 메일 도메인을 등록하고 소유 여부가 확인되면 승인되지 않은 다른 프로젝트에서는 해당 메일 도메인을 사용해 이메일을 보낼 수 없게 됩니다.

### 메일 도메인 등록 및 소유권 확인하기
- 먼저 NHN Cloud Email 콘솔에서 메일 도메인을 등록하고 메일 도메인 소유권을 확인해야 합니다. 
- 메일 도메인 소유자는 NHN Cloud Email에서 제공한 TXT 레코드를 메일 도메인 DNS에 등록합니다. 
- NHN Cloud Email은 메일 도메인 DNS의 TXT 레코드 일치 여부로 소유권을 확인합니다.

### 메일 도메인 인증 및 보호 절차
#### 메일 도메인 등록
![email_202312_00.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_00.png)

1. Email 콘솔로 이동합니다.
2. **메일 도메인 관리** 탭으로 이동합니다.
3. **메일 도메인 등록** 버튼을 클릭하고 메일 발송 시 사용할 도메인을 등록합니다. (최상위 도메인만 등록 가능)


#### 메일 도메인 인증(소유권 확인 절차)
![email_202312_01.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_01.png)

1. **인증**을 클릭합니다. **메일 도메인 인증** 팝업 화면에 생성된 토큰이 표시됩니다.
2. 등록한 메일 도메인 DNS에 TXT 레코드를 추가합니다.
3. 추가한 TXT 레코드가 반영되었다면 **인증**을 클릭해 메일 도메인 소유권을 인증합니다.
4. SPF와 마찬가지로 'nslookup', 'dig' 명령어를 이용해 소유 확인을 위한 TXT 레코드가 메일 도메인 DNS에 반영되었는지 확인할 수 있습니다.

#### DNS의 TXT 레코드를 확인하는 방법
Linux 환경
```
nslookup -q=TXT <your.domain.name>
```
```
dig -t TXT <your.domain.name>
```
Windows 환경
```
nslookup -q=TXT <your.domain.name>
```


### 도메인 공유하기
- 메일 도메인을 보호하기 앞서 등록한 도메인을 다른 프로젝트에서 사용하고 있다면 도메인 공유가 필요합니다. 
- 공유하지 않고 도메인 보호 기능을 활성화하면 공유 받지 않은 프로젝트에서 메일 발송은 실패합니다. 따라서 메일 도메인을 여러 프로젝트에서 사용한다면 반드시 공유해야 합니다.

### 도메인 공유
![email_202312_02.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_02.png)


1. 등록한 메일 도메인 항목의 **공유 > 설정** 버튼을 클릭합니다.
2. 공유할 프로젝트로 이동 후 NHN Cloud Email 앱키를 확인합니다. 확인된 앱키를 등록합니다. 공유가 완료되면 목록에 프로젝트 정보가 표시됩니다. 앱키는 우측 상단 **URL & Appkey**에서 확인할 수 있습니다.
### 메일 도메인 보호하기
![email_202312_03.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_03.png)


메일 도메인 등록, 소유권 확인, 공유 설정이 완료되었다면 보호 기능을 활성화할 수 있습니다.
1. 보호 기능을 활성화할 메일 도메인 항목의 **보호 여부 > 보호** 버튼을 클릭합니다.
2. **보호 적용 안내**의 내용을 확인한 뒤 **보호**를 클릭해 기능을 활성화합니다.
3. 활성화되면 메일 도메인 보호는 완료되며, 발송 중 다른 프로젝트에서 해당 메일 도메인을 사용할 수 없습니다.

