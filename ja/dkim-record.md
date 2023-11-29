## Notification > Email > 도메인 관리 가이드 > DKIM

### DKIM(domainkeys identified mail)이란?
- DKIM(domainkeys identified mail)은 이메일 발신자가 이메일에 디지털 서명을 하여 발신자의 진위 여부를 확인하고 전송 중에 메시지가 변조되지 않았는지 확인할 수 있는 이메일 인증 방법입니다.
- DKIM을 통해 스팸 발송자 및 기타 악의적인 공격자가 이메일을 위조하는 것을 방지할 수 있습니다.

### DKIM의 구조
- DKIM은 공개 키 암호화를 기반으로 하는 디지털 서명 체계를 사용합니다. 
- 발송 서버는 이메일 발송 시 이메일 발송자, 수신자, 제목, 내용 등을 비밀 키로 서명합니다. 이 서명 값을 DKIM-Signature 헤더(Header)에 추가합니다. 
- 수신 서버는 DKIM-Signature 헤더 내 "d=" 필드로 기술된 도메인의 공개 키와 서명 알고리즘 정보 등이 담긴 DKIM 레코드를 조회하고 이 값들을 이용해 수신된 이메일 DKIM-Signature 헤더의 디지털 서명을 검증합니다.
- [RFC 6376](https://datatracker.ietf.org/doc/html/rfc6376/)에서는 DKIM 레코드를 TXT 레코드로 등록하도록 권장하고 있어 NHN Cloud Email에서는 TXT 레코드 인증 및 활성화 기능을 도입하였습니다.

### DKIM-Signature 구조
다음은 이메일 발송 시 이메일 헤더에 추가되는 DKIM 서명(DKIM-Signature 헤더) 예입니다.

> DKIM-Signature: v=1; a=rsa-sha256; d=example.net; s=nhncloud;
t=1117574938; x=1118006938;
h=from:to:subject:date;
bh=MTIzNDU2Nzg5MDEyMzQ1Njc4OTAxMjM0NTY3ODkwMTI=;
b=dzdVyOfAKCdLXdJOc9G2q8LoXSlEniSbav+yuU4zGeeruD00lszZVoG4ZHRNiYzR


#### DKIM-Signature 헤더

- DKIM-Signature 헤더 필드에 대해 설명합니다.
- DKIM-Signature 구조에 대한 보다 더 자세한 설명은 [RFC 6376](https://datatracker.ietf.org/doc/html/rfc6376)를 참고하시길 바랍니다.

| 필드 | 필수 여부 | 값 | 설명 |
|----| ----- | --- | --- |
| v  | 필수 | 1 | 버전입니다. |
| a  | 필수 | sha-256 | DKIM 서명에 사용되는 알고리즘입니다. |
| s  | 필수 | - | 지정자(Selector)입니다. 발송 도메인은 여러 개의 DKIM을 사용할 수 있습니다. 따라서, DKIM-Signature 헤더에서는 이 서명이 어떤 공개 키를 이용해 인증해야 되는지 수신 서버에게 알려주어야 합니다. 예를 들어 지정자가 'toast'면 수신 서버는 'toast.\_domainkey.example.com'의 DNS에서 DKIM 레코드를 확인합니다. |
| h  | 필수 | - | 서명할 헤더(Header)입니다. 헤더들은 콜론(:)으로 구분합니다. |
| b  | 필수 | - | 서명할 헤더(h)에 정의된 헤더들을 서명한 값입니다. Base64로 인코딩됩니다. |
| bh | 필수 | - | 이메일 본문을 서명한 값입니다. |
| l  | 선택 | - | 본문 서명(b)에 사용된 본문의 길이입니다. 따로 정의되어 있지 않다면 본문 전체를 사용합니다. |
| d  | 필수 | - | 도메인입니다. 지정자(s)와 함께 DKIM 서명을 검증하기 위해 DNS에 DKIM 레코드를 조회할 때 사용됩니다. MAIL FROM(5321.From)과 From(5322.From)과 다를 수 있습니다. 하지만, 다를 경우 의심스러운 메일로 분류될 수 있습니다. |
| t  | 선택 | - | DKIM 서명 생성 일시입니다. 형식은 유닉스 시간(Unix Time)입니다. 1970년 1월 1일 0시 0분 0초 UTC(협정 세계시)부터의 경과 시간을 초로 환산한 값입니다. 예, 2020년 6월 8일 오후 3시 47분 17초의 유닉스 시간: 1591631237초 |
| x  | 선택 | - | DKIM 서명 만료 일시입니다. 형식은 서명 생성 일시(t)와 같습니다. |

위의 DKIM Header 예제를 보면 다음과 같은 의미를 가집니다.
- 이메일 발송 도메인은 example.net입니다.
- 이메일 발송 도메인의 지정자(s)는 nhncloud입니다.
- 이메일 발송 도메인(d)은 example.net입니다.
- 이메일 발송 도메인의 DKIM 레코드는 nhncloud.\_domainkey.example.net입니다.
- 수신 서버는 nhncloud.\_domainkey.example.net의 DKIM 레코드를 조회해 이메일을 인증합니다.

###  DKIM 레코드 등록 및 인증, 활성화 방법

#### 1. 메일 도메인 등록 및 인증
DKIM 인증은 메일 도메인이 등록 및 인증 완료되었을 경우 웹 콘솔에서 활성화됩니다.
메일 도메인 인증 관련 상세 가이드는 [Notification > Email > 도메인 관리 가이드 > 도메인 인증 및 보호](https://docs.nhncloud.com/ja/Notification/Email/ja/domain-verification/)를 참고하십시오.

#### 2. DKIM 레코드 등록

1. **메일 도메인 관리** 탭으로 이동합니다.
2. 등록된 발송 도메인에 **DKIM 설정** 버튼을 클릭합니다.
3. 표시된 DKIM 레코드를 복사해 발송 지정된 도메인 DNS에 등록합니다.

![email_202312_04_ja.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_04_ja.png)

##### 주의 사항
- TXT 레코드의 DKIM 설정 변경 작업이 끝나더라도 DNS 서버 상황에 따라 DNS 변경 내용이 적용되기까지 최대 48시간이 소요됩니다.
- DKIM 설정 작업 후, 몇 시간 정도 지난 다음에 이메일을 발송하는 것이 안전합니다.

#### 3. DKIM 인증
- DKIM 레코드가 등록되었다면 **인증** 버튼을 클릭해 인증을 완료합니다.
- 인증이 완료될 경우 **인증** 버튼이 **인증 완료**로 변경됩니다.

#### 4. DKIM 활성화
인증에 성공한 경우 팝업 화면에서 **DKIM** 탭을 선택한 뒤 **활성화**를 클릭해 DKIM을 활성화합니다.

![email_202312_05_ja.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_05_ja.png)

#### 5. DKIM 인증 테스트하기
- DKIM 기능이 인증 및 활성화 되었을 경우 이메일을 발송해 DKIM이 정상적으로 인증되는지 확인할 수 있습니다.
- 발송한 이메일의 우측 상단 더보기에서 원문 보기를 클릭할 경우 DKIM 관련 헤더를 확인할 수 있습니다.