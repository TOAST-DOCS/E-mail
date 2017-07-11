## Notification > Email > Getting Started

### 상품 활성화

Console에 접속하여 Email을 활성화 시킵니다.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_01.jpg)
<center>[그림 1] 이메일 상품 Enable </center>

### 일반 메일발송

수신자, 제목, 내용, 첨부파일 등을 입력하여 메일을 발송 할 수 있습니다.  
일반 메일발송의 경우 수신자를 직접 입력하여 발송합니다.  

![](http://static.toastoven.net/prod_email/email_getting_0622_2_02.jpg)
<center>[그림 2] 일반 메일 발송 화면 </center>
<p>

수신자 및 참조수신자는 메일주소 형태로 작성 가능합니다.  

![](http://static.toastoven.net/prod_email/email_getting_0622_2_03.jpg)
<center>[그림 3] 수신자 입력 화면 </center>
<p>

### 광고성 메일발송
광고성 메일을 발송할 수 있습니다.
광고성 메일 발송 시 아래의 항목을 참고바랍니다.

1) 제목에 "(광고)" 문구가 필수적으로 들어가야 합니다.
![](http://static.toastoven.net/prod_email/email_getting_0622_2_23.jpg)
<center>[그림 4] 광고성 메일 발송 화면 </center>
<p>

2) 수신거부 링크가 한글/영문 형태로 반드시 들어가야 합니다.
![](http://static.toastoven.net/prod_email/email_getting_0622_2_24.jpg)
<center>[그림 5] 문구 필수 입력 </center>
<p>

3) 수신 거부로 등록된 유저는 광고성 메일로 발송 시 발송되지 않습니다.
![](http://static.toastoven.net/prod_email/email_getting_0622_2_25.jpg)
<center>[그림 6] 수신자 입력란의 광고성 안내 문구 </center>

### 템플릿을 이용한 메일발송

템플릿 버튼을 이용하여 미리 생성해 놓은 메일내용을 사용할 수 있습니다.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_04.jpg)
<center>[그림 7] 템플릿 선택 화면 </center>
<p></p>
카테고리에 미리 생성해 놓은 템플릿을 선택하여 적용합니다.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_05.jpg)
<center>[그림 8] 템플릿 선택 화면 </center>

![](http://static.toastoven.net/prod_email/email_getting_0622_2_06.jpg)
<center>[그림 9] 템플릿 선택 완료 화면 </center>
### 파일 업로드를 통한 대량 메일발송

파일을 이용한 대량 메일 발송을 할 수 있습니다.

발송 메일 정보를 입력합니다.
![](http://static.toastoven.net/prod_email/email_getting_0622_2_07.jpg)
<center>[그림 10] 발송메일 정보 입력 화면 </center>
  - 발송메일주소: 발송 메일 주소를 입력합니다. 이름 <emailAddress> 형식으로 작성하면 발송자 이름과 메일주소를 입력 할 수 있습니다.
  - 예약전송: 메일 발송 예약 일시를 지정 할 수 있습니다.
  - 첨부파일: 메일에 첨부파일을 추가합니다.
    - 첨부파일은 최대 3개까지 업로드 가능하며, 10MB 이하의 파일만 가능합니다.
    - 첨부파일들의 총 합은 10MB를 넘어갈 수 없습니다.
  - 제목/내용: 메일 제목과 내용을 입력합니다. 메일 제목과 내용은 ##치환Key## 형식으로 입력하면 메일 내용을 사용자가 설정한 치환 데이터로 치환하여 발송 할 수 있습니다.

템플릿을 다운로드 합니다.
![](http://static.toastoven.net/prod_email/email_getting_0622_2_08.jpg)
<center>[그림 11] 대량 메일 발송 리스트 템플릿 다운로드 화면 </center>

템플릿은 csv와 excel을 제공합니다.
![](http://static.toastoven.net/prod_email/email_getting_0622_2_09.jpg)
<center>[그림 12] 템플릿 다운로드 팝업 </center>

템플릿 파일에 수신자 메일주소와 치환 데이터를 입력합니다.
![](http://static.toastoven.net/prod_email/email_getting_0622_2_10.jpg)
<center>[그림 13] 엑셀 데이터 입력 화면 </center>

템플릿 데이터에 오류가 있는 경우, 오류 내용을 확인 후 수정을 해주세요.
오류는 총 오류 건수와 내용(최대 10건)을 표시합니다.
![](http://static.toastoven.net/prod_email/email_getting_0622_2_11.jpg)
<center>[그림 14] 오류 화면 </center>

오류 종류

  - receive_mail_address 열이 존재하지 않는 경우: receive_mail_address 열은 수신자 메일주소를 입력하는 열로 필수로 입력해야 합니다.
  - 입력된 데이터가 없는 경우
  - 수신자메일주소 또는 치환 데이터 입력이 누락된 경우

발송예약 후 확인 후 진행 또는 즉시발송을 선택하여 발송을 예약합니다.
![](http://static.toastoven.net/prod_email/email_getting_0622_2_12.jpg)
<center>[그림 15] 발송 예약 화면 </center>

  - 확인 후 진행: 수신자 별 메일 주소와 발송 내용을 대량메일발송 탭에서 확인 후 메일 발송을 진행 할 수 있습니다. 사용자가 발송을 진행하지 않으면 메일 발송이 진행되지 않으니 유의해주세요.
  - 즉시발송: 수신 정보를 확인하지 않고 즉시 메일 발송을 진행합니다. 발송결과는 대량메일발송탭에서 확인 할 수 있습니다.

### 대량메일발송 탭
대량메일발송 탭에서는 대량메일발송 예약 건의 조회, 발송, 발송취소를 할 수 있습니다.

  - 발송: 발송을 클릭하시면 메일 발송을 진행합니다. 메일 발송은 메일 발송 건수에 따라 상당 시간 소요 될 수 있습니다.
  - 취소: 취소를 클릭하시면 메일 발송을 취소합니다.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_13.jpg)
<center>[그림 16] 대량메일 발송 조회 화면 </center>

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
![](http://static.toastoven.net/prod_email/email_getting_0622_2_14.jpg)
<center>[그림 17] 수신자별 메일 발송 조회 </center>

### 카테고리 및 템플릿 관리

카테고리 생성/수정/삭제를 통하여 메일 발송 시 사용하는 템플릿을 분류하여 관리 할 수 있습니다.  
카테고리를 생성합니다.  

![](http://static.toastoven.net/prod_email/email_getting_0622_2_15.jpg)
<center>[그림 18] 카테고리/템플릿 생성 화면 </center>

생성 된 카테고리를 클릭하여 템플릿을 추가합니다.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_16.jpg)
<center>[그림 19] 템플릿 생성 화면 </center>

자주 사용되는 메일내용을 작성하여 템플릿으로 등록합니다.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_17.jpg)
<center>[그림 20] 템플릿 입력 화면 </center>

### 메일 요청별 조회

메일 요청별 조회 탭을 이용하여 발송 일시, 수신 일시, 템플릿, 발송상태 등의 조건을 이용하여 발송 된 메일을 조회 할 수 있습니다.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_18.jpg)
<center>[그림 21] 메일 요청별 조회 화면 </center>

리스트에 노출된 메일내용을 클릭하면 상세 발송 내용을 확인 할 수 있습니다.

![](http://static.toastoven.net/prod_email/email_getting_0622_2_19.jpg)
<center>[그림 22] 메일 상세 보기 화면 </center>

### 수신 거부 관리

수신 거부를 관리할 수 있는 페이지입니다.
현재까지 수신거부로 등록된 유저를 조회할 수 있으며, 수동 등록 및 삭제가 가능합니다.
![](http://static.toastoven.net/prod_email/email_getting_0622_2_26.jpg)
<center>[그림 23] 수신 거부 관리 화면 </center>

#### 수신거부 직접 입력으로 등록

수신 거부 등록 버튼 클릭
![](http://static.toastoven.net/prod_email/email_getting_0622_2_27.jpg)
<center>[그림 24] 수신 거부 등록 화면 </center>

수신 거부 이메일 주소 입력
추가 버튼 클릭하여 최대 10명까지 등록할 수 있습니다.
![](http://static.toastoven.net/prod_email/email_getting_0622_2_28.jpg)
<center>[그림 25] 수신 거부 수동 등록 화면 </center>

등록 버튼 클릭 후 등록된 리스트 확인
![](http://static.toastoven.net/prod_email/email_getting_0622_2_29.jpg)
<center>[그림 26] 수신 거부 수동 등록 완료 화면 </center>

#### 수신거부 파일 업로드로 등록

수신 거부 파일은 csv, 텍스트(.txt) 엑셀(.xlsx)로 업로드가 가능합니다.
메일 주소 일괄 등록을 클릭하여 파일 업로드(.csv .txt .xlsx)

![](http://static.toastoven.net/prod_email/email_getting_0622_2_30.jpg)
<center>[그림 27] 수신 거부 엑셀 등록 화면 </center>

csv 파일

![](http://static.toastoven.net/prod_email/email_getting_0622_2_31.jpg)
<center>[그림 28] 업로드에 사용된 csv 파일 </center>

업로드 완료 후 등록 버튼 클릭
정상적으로 업로드된 경우 "총 x개의 메일 주소가 확인되었습니다"문구 노출

![](http://static.toastoven.net/prod_email/email_getting_0622_2_32.jpg)
<center>[그림 29] 유효성 검사 완료 화면 </center>

업로드 완료 모습

![](http://static.toastoven.net/prod_email/email_getting_0622_2_33.jpg)
<center>[그림 30] 업로드 완료 화면 </center>

#### 수신 거부 삭제

삭제할 수신거부를 선택한 후 수신 거부 삭제 버튼 클릭

![](http://static.toastoven.net/prod_email/email_getting_0622_2_34.jpg)
<center>[그림 31] 수신 거부 삭제 화면 </center>

팝업 내용 확인 후 "확인" 버튼 클릭

![](http://static.toastoven.net/prod_email/email_getting_0622_2_35.jpg)
<center>[그림 32] 수신 거부 삭제 팝업 화면 </center>

삭제 완료

![](http://static.toastoven.net/prod_email/email_getting_0622_2_36.jpg)
<center>[그림 33] 수신 거부 삭제 완료 화면 </center>

### 통계 정보 조회

통계 조회 탭에서 통계정보를 확인할 수 있습니다.
조회 기간, 통계 구분(날짜,시간,요일), 메일 타입(일반,대량), 템플릿을 조건으로 조회가 가능합니다.


날짜별
![](http://static.toastoven.net/prod_email/email_getting_0622_2_20.jpg)
<center>[그림 34] 날짜별 통계 화면 </center>

시간대별
![](http://static.toastoven.net/prod_email/email_getting_0622_2_21.jpg)
<center>[그림 35] 시간대별 통계 화면 </center>

요일별
![](http://static.toastoven.net/prod_email/email_getting_0622_2_22.jpg)
<center>[그림 36] 요일별 통계 화면 </center>

### 개인정보 수탁사 고지 안내
'고객'이 TOAST Cloud > Email 상품 이용 시, '고객' - '당사' 간 개인정보 처리에 관한 업무 위수탁 관계가 발생하는 바 정보통신망법 및 개인정보보호법에 따라 위탁자인 '고객'은 개인정보처리방침을 통해 '당사'에 개인정보를 위탁한 현황(수탁자 및 업무의내용)을 공개하여야 합니다.<br/>
이에, '당사'에서는 '고객'이 TOAST Cloud의 Email 상품을 이용함에 있어 관련 법령을 준수하고, 위탁현황 미공개로 인하여 과태료 등의 불이익을 받지 않도록 아래와 같이 가이드 할 수 있습니다.<br/>
(예시)<br/>
[개인정보 수탁사 고지 안내]<br/>
Email 상품 이용 시 고객사에서 운영하시는'개인정보처리방침' > 위탁 현황에 다음의 내용을 표기해주세요.<br/>
수탁사 : 엔에이치엔엔터테인먼트<br/>
업무의 내용 : 이메일 발송 대행<br/>
