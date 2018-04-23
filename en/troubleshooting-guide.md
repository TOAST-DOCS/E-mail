## Notification > Email > Trouble Shooting Guide

### SPF 등록
TOAST Email을 통해 발송하는 메일이 스팸처리 되고 있어 다음과 같이 SPF 등록 안내드립니다.

#### 현상과 원인

* 현상 : 발송한 메일이 수신자 메일 주소로 전송되지 않고 스팸처리 되어 반송됨
* 원인 : SPF 미등록

#### SPF 안내
|용어|	설명|
|---|---|
|SPF 레코드|DNS(Domain Name System) 레코드의 한 유형이며, 도메인을 대표하여 메일을 보낼 수 있는 메일 서버를 식별합니다.|

* "발송 메일"에 입력할 메일 주소의 도메인에 대한 SPF(Sender Policy Framework) 레코드를 만들어주시길 바랍니다.
* 도메인에 SPF 레코드가 없는 경우에는 수신자가 메일이 인증된 메일 서버에서 전송된 것인지를 확인할 수 없으므로 메일이 수신 거부될 수도 있습니다.

#### TOAST SPF 레코드
아래 값을 DNS의 TXT 레코드에 등록합니다. 자세한 등록 방법은 DNS 관리 업체에 문의하시기 바랍니다.
```
v=spf1 include:_spfblocka.toast.com ~all
```
