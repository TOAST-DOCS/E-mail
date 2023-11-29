## Notification > Email > 도메인 관리 가이드 > DMARC

### DMARC (domain-based message authentication reporting and conformance)란?

이메일 보안 강화 가능의 마지막 단계인 DMARC는 이메일 스푸핑을 이용한 피싱, 사기 등을 막기 위한 도메인 기반 메시지 인증에 대한 보고 및 준수 정책입니다. 
<br>수신 서버는 발송자 주소 (From) 도메인의 DNS에서 DMARC 레코드를 조회합니다. DMARC 레코드에 정의된 정책에 따라 수신 서버는 수신된 메일을 인증합니다. DMARC 정책은 SPF와 DKIM을 사용하는지, 각각의 인증 수단이 실패했을 때 메일 처리 방법은 어떻게 되는지로 구성되어 있습니다. 
<br><br>일부 메일 서비스 (ex. Gmail이나 Yahoo 등)들은 DMARC를 적용하지 않을 경우 스팸 메일로 인지해 발송을 차단합니다. 메일 발송 간 높은 메일 도달률을 위해서는 DMARC 레코드를 사용하는 것을 권장합니다.

### DMARC DNS 레코드 구조

DMARC DNS 레코드는 '_dmarc.example.com'처럼 DMARC를 적용할 발송 도메인에 '_dmarc'를 붙인 서브 도메인 DNS에 레코드를 등록합니다.
다음은 DMARC DNS 레코드 예시입니다.

```
"v=DMARC1;p=none;sp=quarantine;pct=100;rua=mailto:dmarcreports@example.com;"
```

DMARC 레코드에 사용되는 값에 대해 설명합니다. 더 자세한 내용은 [RFC 7489](https://www.ietf.org/rfc/rfc7489.txt)를 참고 부탁드립니다.

| 구분 | 필수 여부 | 값                        | 설명                                                                                        |
| --- | ----- |--------------------------|-------------------------------------------------------------------------------------------|
| v | 필수 | DMARC1 (고정)              | 버전입니다.                                                                                    |
| p | 필수 | none, quarantine, reject | 실패 시 처리에 대한 정책입니다.                                                                        |
| sp | 선택 | none, quarantine, reject | 서브 도메인에 대한 실패 처리 정책입니다.                                                                   |
| pct | 선택 | 0 \~ 100 (기본값 100)       | 정책을 적용할 이메일 비중입니다. 예를 들어 50인 경우 수신된 이메일 중 절반이 DMARC 정책에 의해 인증됩니다.                         |
| adkim | 선택 | s, r (기본값)               | DKIM 얼라인먼트 (Alignment). DKIM-Signature의 도메인 (d)와 From (5322.From)의 일치 수준에 대한 설정입니다.       |
| aspf | 선택 | s, r (기본값)               | SPF 얼라인먼트 (Alignment). SPF 인증 시 MAIL FROM (5321.From)과 From (5322.From)의 일치 수준에 대한 설정입니다. |
| rua | 선택 |                          | 주기적으로 집계한 실패 보고서를 수신할 주소입니다. 예, mailto:demarc-report@nhncloud.com                         |
| ruf | 선택 |                          | 실패 보고서를 수신할 주소입니다. 예, mailto:demarc-report@nhncloud.com                                   |
| fo | 선택 | 0 (기본값), 1, d, s         | 실패 보고서 (ruf)를 생성할 기준입니다.                                                                  |
| rf | 선택 | afrf (고정)                | 실패 보고서 (ruf) 형식에 대한 설정입니다.                                                                |
| ri | 선택 | 86400 (단위 초, 기본 값)       | 실패를 집계할 기간입니다. 설정된 주기마다 실패 보고서(rua)가 발송됩니다.                                               |

#### 실패 정책 (p) 값 종류

| 실패 정책 | 설명                                                                                                                          |
| ----- |-----------------------------------------------------------------------------------------------------------------------------|
| none | 수신 서버에서 실패에 대해 아무 처리도 하지 않을 것을 바랍니다. SPF, DKIM을 사용하지 않는 상황에서 설정할 수 있습니다.                                                    |
| quarantine | 수신 서버는 실패한 메일을 스팸으로 처리할 것을 바랍니다.                                                                                            |
| reject | 수신 서버에서 DMARC 실패가 발생한 메일을 반송합니다. 일반적으로 발송 서버와 수신 서버가 SMTP 통신 시 DSN (Delivery Status Notification) 응답으로 이루어지는 것을 선호하는 정책입니다. |

#### SPF와 DKIM 얼라인먼트 (aspf, adkim) 값 설명

| 정책 | 설명                                                                                 |
| --- |------------------------------------------------------------------------------------|
| s | 엄격함 (Strict)입니다. 도메인 부분이 완전히 일치해야 합니다.                                             |
| r | 유연함 (Relexed)입니다. 서브 도메인도 가능합니다. 예, 'd=example.com'일 때 'From: news.example.com' 통과 |

### 실패 보고서 생성 기준 (fo)에 대한 값 설명
- 실패 보고서 (ruf)를 설정하면 사용됩니다.

| 기준 | 설명 |
| --- | --- |
| 0 | SPF, DKIM 모두 실패했을 때 보고합니다. |
| 1 | SPF, DKIM 하나라도 실패했을 때 보고합니다. |
| d | DKIM 인증 실패 시 보고합니다. |
| s | SPF 인증 실패 시 보고합니다. |

### DMARC 레코드 검증 절차
#### 1. 메일 도메인 등록 및 인증
DMARC 레코드 검증은 메일 도메인이 등록 및 인증 완료되었을 경우 웹 콘솔에서 활성화됩니다.
메일 도메인 인증 관련 상세 가이드는 [Notification > Email > 도메인 관리 가이드 > 도메인 인증 및 보호](https://docs.nhncloud.com/en/Notification/Email/en/domain-verification/)를 참고하십시오.

#### 2. DMARC 레코드 DNS 등록
DMARC 레코드를 등록하는 방법을 설명합니다.
DMARC 레코드는 '_dmarc.example.com'처럼 DMARC를 적용할 발송 도메인에 '_dmarc'를 붙인 도메인 DNS에 TXT 레코드로 등록합니다. NHN Cloud Email 콘솔에서 추가적으로 작업할 내용은 없으며, 적절한 DMARC 정책을 수립해 등록하면 됩니다.
예를 들면 등록할 내용은 다음과 같습니다. DMARC 실패 시 보고서를 수신할 주소를 설정해야 합니다.

```
"v=DMARC1; p=none; fo=1; rua=mailto:${보고서를_수신할_주소}"
```

등록이 완료되었다면 'nslookup', 'dig' 명령어를 이용하여 DMARC DNS 레코드가 DNS에 반영되었는지 확인할 수 있습니다.

#### 주의 사항
- TXT 레코드의 DMARC 설정 변경 작업이 끝나더라도 DNS 서버 상황에 따라 DNS 변경 내용이 적용되기까지 최대 48시간이 소요됩니다.
- DMARC 설정 작업 후, 몇 시간 정도 지난 다음에 이메일을 발송하는 것이 안전합니다.

#### 3.DMARC 인증 
- DMARC 레코드 DNS 등록이 완료되면 DMARC 관리 팝업에서 DMARC 인증을 수행합니다.

![email_202312_08_en.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_08_en.png)

인증이 완료될 경우, **인증 완료** 라는 문구가 표시됩니다.

#### DMARC 레코드 조회 실패 화면 예시
- DMARC 레코드 조회 실패 시 다음과 같은 화면이 표시됩니다.

![email_202312_09_en.png](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_email/email_202312_09_en.png)


### 주의 사항
#### 1. SPF 관련 주의 사항
[RFC 7489](https://www.ietf.org/rfc/rfc7489.txt) 문서를 참고할 경우 일부 수신 서버에서 SPF를 DMARC 검증 로직을 먼저 구현할 수 있습니다. 이 때

SPF 레코드에 -all과 같은 - 접두사가 있을 경우 DMARC 인증을 실패할 수 있습니다. 일부 수신 서버에서 DMARC 인증을 실패할 경우 SPF 레코드에 -all과 같은 - 접두사를 제거하고 DMARC 인증을 다시 시도하십시오.

#### 2. DMARC 관련 주의 사항
DMARC에서 주의할 점은 수신 서버가 DMARC 정책에 맞게 처리하는 것을 완전히 보장하지 않는다는 것입니다. DMARC는 발송 서버가 수신 서버에게 정책을 제안하는 수준으로 이해해야 합니다.
<br> 예를 들어 실패 정책을 'none'으로 설정해도 수신 서버는 인증이 실패된 이메일을 스팸으로 처리할 수 있습니다.

NHN Cloud에서는 [RFC 7489](https://www.ietf.org/rfc/rfc7489.txt) 문서에 따라 DMARC 검증을 수행합니다. 일부 수신 서버에서 메일 수신이 실패할 경우 [고객 센터 > 1:1 문의](https://www.nhncloud.com/kr/support/inquiry)를 통해 문의하십시오.

