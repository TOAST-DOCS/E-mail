## Notification > Email > 개요

NHN Cloud Email 서비스를 사용하면 메일 발송, 예약 발송, 발송 내역 조회, 템플릿 관리 기능을 사용할 수 있습니다. 자체적인 발송 프로그램이나 관리도구와 손쉽게 연동할 수 있게 HTTP API도 사용할 수 있습니다.

## 주요 기능

- 대량 발송 지원
	\- Excel 파일로 수신자 목록을 입력하고 대량으로 메일을 발송할 수 있습니다.
- 태그 발송 지원
	\- 태그를 사용하여 수신자 목록을 입력하고 대량으로 메일을 발송할 수 있습니다.
- 예약 발송
	\- 원하는 시간에 메일을 발송할 수 있습니다.
- 치환 태그 제공
	\- 치환 태그를 이용하여 수신자별로 개별화된 메일을 발송할 수 있습니다.
- 템플릿 기능 제공
	\- 자주 사용하는 메일은 템플릿으로 등록하여 사용할 수 있습니다.

- 고객의 애플리케이션에서 사용할 수 있는 메일 발송과 조회 RESTful API를 사용할 수 있습니다.

## 발송 전 유의 사항
NHN Cloud Email 서비스를 사용해 발송한 이메일이 외부 이메일 서비스에서 스팸 메일로 처리되는 것을 방지하려면 다음 사항을 유의해야 합니다.

첫째, DNS를 설정해야 합니다. <br/>
NHN Cloud Email 서비스의 메일 발송(SMTP) 서버를 통해 사용자 소유의 도메인이 포함된 이메일 주소를 가지고 발송되는 것이므로, 사용자 소유의 도메인이 서비스되는 DNS에 추가 설정이 필요합니다. <br/> DNS의 TXT 레코드에 SPF 항목으로 _spfblocka.toast.com을 등록해야 합니다. SPF 설정이 올바르게 되어 있지 않다면 수신자의 이메일 서비스에서는 스팸 메일이나 발신자를 사칭한 메일 발송으로 간주하여 경고 표시를 하거나 '스팸 메일함'에 저장하여 수신자가 이메일을 정상적으로 수신하기 어려운 상황이 발생할 수 있습니다.<br>
SPF 설정 방법은 [문제 해결 가이드](./troubleshooting-guide/)를 참고하시기 바랍니다.

둘째, Gmail은 도메인 평판을 중요하게 여겨 스팸 메일 판정에 주요 기준으로 삼고 있습니다. 낮은 도메인 평판을 가진 발송자가 빠른 속도로 대량의 이메일을 발송하거나, 수신자가 원하지 않는 광고성 이메일을 발송하게 되면 수신자의 '받은 메일함'에 저장하지만 경고 표시를 하거나, '스팸 메일함'에 저장하거나, 심지어는 아예 수신 속도를 제한하거나, 수신 거부를 할 수 있습니다. 그러므로 이메일을 대량으로 발송하고자 하는 사용자는 다음 가이드의 도메인 평판 관리 방법을 참고하여 항상 도메인 평판을 높게 유지할 수 있도록 신경 써야 합니다.<br>
자세한 내용은 [문제 해결 가이드](./troubleshooting-guide/)를 참고하시기 바랍니다.

## 참고

<span id='tags-and-uids'></span>
### 태그와 UID

#### 서비스 용어
|용어|	설명|
|---|---|
|태그(tag)|UID를 분류하는 체계. <br>UID에 여러 개의 태그를 붙여 사용자가 쉽게 UID 정보를 검색하고 사용할 수 있습니다.|
|UID|사용자를 구분하는 ID(식별자). <br>하나의 UID에는 여러 개의 연락처를 등록하여 발송에 사용할 수 있습니다. |
|연락처(contact)|연락을 하기 위해 정해둔 곳. <br>Notification에서는 Push, Email, SMS, 총 3개의 상품에서 연락처를 등록할 수 있습니다. <br>Push는 토큰, Email은 메일 주소, SMS는 전화번호입니다.|

#### 태그를 사용하여 발송
* 수신자 정보인 메일 주소 대신, 태그를 선택하여 메일을 발송할 수 있는 기능입니다.

1. UID를 등록합니다.

* **UID 관리** 탭에서 UID와 한 개, 또는 여러 개의 메일 주소를 등록합니다.
* 자세한 내용은 [UID 관리](./console-guide/#uid)를 참고해 주세요.

2. 태그를 등록합니다.

* **태그 관리**탭에서 태그를 등록합니다.
* 자세한 내용은 [태그 관리](./console-guide/#_11)를 참고해 주세요.

3.  태그에 UID를 등록합니다.

* **태그 관리** 탭에서 등록한 태그에 UID를 등록합니다.

4. 태그를 선택한 후 메일을 발송합니다.

* **메일 발송** 탭에서 메일 주소 대신 **태그 발송**을 선택하여 태그를 등록합니다.
* 메일은 태그에 등록된 UID의 메일 주소로 발송됩니다.
* 자세한 내용은 [태그를 사용한 메일발송](./console-guide/#_6)을 참고해 주세요.

#### 다른 상품의 태그 기능과의 관계
* 만약 같은 프로젝트에서 Push 또는 SMS 상품을 사용하고 있다면, Email에서 사용하고 있는 태그와 UID 정보를 재등록하지 않고 함께 사용할 수 있습니다.
* 각 상품의 콘솔에서 같은 UID에 다른 연락처를 추가할 수 있습니다.

### 사용자 지정 헤더(Custom header)

* 수신 이메일에 사용자 지정 헤더를 추가하여 발송할 수 있습니다.
* NHN Cloud Email 서비스에서는 아래 헤더 이외에 [RFC 822](https://www.ietf.org/rfc/rfc0822.txt)에 설명된 형식을 따르는 헤더를 사용할 수 있습니다.

#### 요청할 수 없는 헤더

* From
* To
* Cc
* Bcc
* Date
* Subject
* Content-Disposition
* Message-ID
* Sender
* Reply-To
* Newsgroups
* Content-ID
* Content-MD5
* MIME-Version
* Content-Transfer-Encoding
* Content-Description

#### 구성

| 용어 | 설명 |
|---|---|
|헤더 이름| 추가할 헤더의 이름. <br>영문과 숫자(a-z, A-Z, 0-9), 하이픈(-)만 사용해 1-50자 사이로 작성합니다. |
|헤더값| 추가할 헤더의 값. <br>1-1000바이트 사이로 작성합니다. <br>ASCII가 아닌 문자는 Base64로 인코딩한 후 발송합니다. |


### 데이터 보관 기간
Email 서비스는 보관 정책에 따라 최근 90일 데이터만 보관합니다.
보관 기간이 지난 데이터는 삭제되어 조회할 수 없습니다.