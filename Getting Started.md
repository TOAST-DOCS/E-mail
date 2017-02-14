## Upcoming Products > Email > Getting Started

> ※ 본 문서는 alpha 개발 단계의 문서입니다.
> 사용에 관심이 있으신 분은 **support@cloud.toast.com**으로 문의해 주시기 바랍니다.

### 상품 활성화

Console에 접속하여 Email을 활성화 시킵니다.

![](http://static.toastoven.net/prod_email/email_getting_01.jpg)

### 일반 메일발송

수신자, 제목, 내용, 첨부파일 등을 입력하여 메일을 발송 할 수 있습니다.  
일반 메일발송의 경우 수신자를 직접 입력하여 발송합니다.  

![](http://static.toastoven.net/prod_email/email_getting_02.jpg)

수신자 및 참조수신자는 메일주소 형태로 작성 가능합니다.  

![](http://static.toastoven.net/prod_email/email_getting_03.jpg)

### 템플릿을 이용한 메일발송

템플릿 버튼을 이용하여 미리 생성해 놓은 메일내용을 사용할 수 있습니다.

![](http://static.toastoven.net/prod_email/email_getting_04.jpg)

카테고리에 미리 생성해 놓은 템플릿을 선택하여 적용합니다.

![](http://static.toastoven.net/prod_email/email_getting_05.jpg)

![](http://static.toastoven.net/prod_email/email_getting_06.jpg)

### 파일 업로드를 통한 대량 메일발송

파일을 이용한 대량 메일 발송을 할 수 있습니다.

발송 메일 정보를 입력합니다.
![](http://static.toastoven.net/prod_email/email_getting_07.jpg)

  - 발송메일주소: 발송 메일 주소를 입력합니다. 이름 <emailAddress> 형식으로 작성하면 발송자 이름과 메일주소를 입력 할 수 있습니다.
  - 예약전송: 메일 발송 예약 일시를 지정 할 수 있습니다.
  - 첨부파일: 메일에 첨부파일을 추가합니다.
    - 첨부파일은 최대 3개까지 업로드 가능하며, 10MB 이하의 파일만 가능합니다.
    - 첨부파일들의 총 합은 10MB를 넘어갈 수 없습니다.
  - 제목/내용: 메일 제목과 내용을 입력합니다. 메일 제목과 내용은 ##치환Key## 형식으로 입력하면 메일 내용을 사용자가 설정한 치환 데이터로 치환하여 발송 할 수 있습니다.

템플릿을 다운로드 합니다.
![](http://static.toastoven.net/prod_email/email_getting_08.jpg)

템플릿은 csv와 excel을 제공합니다.
![](http://static.toastoven.net/prod_email/email_getting_09.jpg)

템플릿 파일에 수신자 메일주소와 치환 데이터를 입력합니다.
![](http://static.toastoven.net/prod_email/email_getting_10.jpg)

템플릿 데이터에 오류가 있는 경우, 오류 내용을 확인 후 수정을 해주세요.
오류는 총 오류 건수와 내용(최대 10건)을 표시합니다.
![](http://static.toastoven.net/prod_email/email_getting_11.jpg)

오류 종류

  - receive_mail_address 열이 존재하지 않는 경우: receive_mail_address 열은 수신자 메일주소를 입력하는 열로 필수로 입력해야 합니다.
  - 입력된 데이터가 없는 경우
  - 수신자메일주소 또는 치환 데이터 입력이 누락된 경우

발송예약 후 확인 후 진행 또는 즉시발송을 선택하여 발송을 예약합니다.
![](http://static.toastoven.net/prod_email/email_getting_12.jpg)

  - 확인 후 진행: 수신자 별 메일 주소와 발송 내용을 대량메일발송 탭에서 확인 후 메일 발송을 진행 할 수 있습니다. 사용자가 발송을 진행하지 않으면 메일 발송이 진행되지 않으니 유의해주세요.
  - 즉시발송: 수신 정보를 확인하지 않고 즉시 메일 발송을 진행합니다. 발송결과는 대량메일발송탭에서 확인 할 수 있습니다.

### 대량메일발송 탭
대량메일발송 탭에서는 대량메일발송 예약 건의 조회, 발송, 발송취소를 할 수 있습니다.

  - 발송: 발송을 클릭하시면 메일 발송을 진행합니다. 메일 발송은 메일 발송 건수에 따라 상당 시간 소요 될 수 있습니다.
  - 취소: 취소를 클릭하시면 메일 발송을 취소합니다.

![](http://static.toastoven.net/prod_email/email_getting_13.jpg)

대량메일발송 진행상태

  - 대기: 수신자 파일 데이터를 읽는 작업을 진행 하기 전 상태입니다.
  - 발송준비: 수신자 파일 데이터 정보를 로드 중인 상태입니다.
  - 발송준비완료: 메일발송 준비가 완료된 상태 입니다. 예약 건(리스트의 행)을 선택하시면  수신자 별 메일 발송 내용을 하단의 리스트에서 확인 할 수 있습니다.
  - 발송대기: 메일 발송 작업 대기 중인 상태입니다.
  - 발송중: 메일 발송이 진행 중인 상태 입니다. 예약 건 (리스트의 행)을 선택하면 발송 진행률을 확인 할 수 있습니다.
  - 발송완료: 발송이 정상적으로 완료된 상태 입니다.
  - 발송실패: 발송 진행 중 발송 오류가 발생 한 경우 입니다. 메일 별 발송상태는 메일 수신 조회 리스트에서 확인 할 수 있습니다.
  - 발송취소: 사용자가 메일 발송을 취소한 상태 입니다.

수신자 별 메일 발송 조회

대량메일 발송 예약 건 (리스트의 행)을 선택하시면 하단의 리스트에서 수신자 별 메일 발송 내역을 조회 할 수 있습니다.  
![](http://static.toastoven.net/prod_email/email_getting_14.jpg)


### 카테고리 및 템플릿 관리

카테고리 생성/수정/삭제를 통하여 메일 발송 시 사용하는 템플릿을 분류하여 관리 할 수 있습니다.  
카테고리를 생성합니다.  

![](http://static.toastoven.net/prod_email/email_getting_15.jpg)

생성 된 카테고리를 클릭하여 템플릿을 추가합니다.

![](http://static.toastoven.net/prod_email/email_getting_16.jpg)

자주 사용되는 메일내용을 작성하여 템플릿으로 등록합니다.

![](http://static.toastoven.net/prod_email/email_getting_17.jpg)

### 메일 요청별 조회

메일 요청별 조회 탭을 이용하여 발송 일시, 수신 일시, 템플릿, 발송상태 등의 조건을 이용하여 발송 된 메일을 조회 할 수 있습니다.

![](http://static.toastoven.net/prod_email/email_getting_18.jpg)

리스트에 노출된 메일내용을 클릭하면 상세 발송 내용을 확인 할 수 있습니다.

![](http://static.toastoven.net/prod_email/email_getting_19.jpg)
