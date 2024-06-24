<h1> 🐕‍🦺 엘리스 3차 팀 프로젝트 - 꼬랑지랑 🐱</h1> 
- 본 프로젝트는 프론트 엔드 2인과 백 엔드 6인 (총 8인)으로 구성된 프로젝트입니다. <br/>
<br/>
<br/>
<br/>
<p align="center">
  <img src="https://github.com/wjcho0303/eliceProject3Ggorangjirang/assets/156410727/62158b5f-206a-4184-a4a2-4b3f293f816f">
</p>
<br/>
<br/>

---

<h3>※ 프로젝트 개요 </h3>
- 프로젝트 진행 기간 : 2024.05.20 (월) ~ 2024.06.14 (금) (4주간) <br/>
<br/>
<p align="center">
  <img src="https://github.com/wjcho0303/eliceProject3Ggorangjirang/assets/156410727/9ad8a3a1-f7d0-4f79-8fcf-55556ad08685">
</p>
<br/>
<br/>
<br/>
<p align="center">
  <img src="https://github.com/wjcho0303/eliceProject3Ggorangjirang/assets/156410727/f9560084-9d0d-4897-80c9-6f8f0ffcf3ef">
</p>
<br/>
<br/>
<br/>
<p align="center">
  <img src="https://github.com/wjcho0303/eliceProject3Ggorangjirang/assets/156410727/6d21ebc5-5d4b-4f43-a5bf-235a1ef35c16">
</p>
<br/>
<br/>

---

<h3>※ 프로젝트 아키텍처 </h3>
저희는 AWS EC2 서버 3개를 사용했는데, 하나는 빌드 서버, 하나는 배포 서버, 그리고 마지막으로 모니터링 서버를 사용했습니다.<br/>
깃랩 파이프라인을 이용해서 백엔드와 프론트에서 각각 작업한 내용들을 gitlab dev 브랜치로 머지할 때<br/>
자동으로 빌드 및 배포되도록 파이프라인을 구축하여 배포되도록 하였습니다.<br/>

<br/>
<br/>
<p align="center">
  <img src="https://github.com/wjcho0303/eliceProject3Ggorangjirang/assets/156410727/86fcae22-73ed-4fd8-85c7-8771bda3f3e9">
</p>
<p align="center">
  <img src="https://github.com/wjcho0303/eliceProject3Ggorangjirang/assets/156410727/fecb6e22-de38-44ac-bdd7-f584259a2e52">
</p>
<br/>

---

<h3>※ 담당 영역 </h3>
- 상품 카테고리 CRUD <br/>
- 상품 CRUD <br/>
- 리뷰 CRUD <br/>
- 관리자 메인 페이지(thymeleaf) <br/>
- AmazonS3 연동 - 상품 이미지와 리뷰 이미지 파일을 S3 외부 저장소에 저장하고, DB에 이미지 url을 저장. 상품 및 리뷰 이미지 수정 시 기존 이미지 파일 삭제 로직 적용  <br/>
- 네이버 SMTP 이메일 연동. 해당 이메일 서비스는 회원가입 시 이메일 인증 절차, 결제완료 및 배송완료 시 고객 이메일로 이메일 알림 등에 사용됨.  <br/>
- Discord Webhook 연동 - 특정 상황에 대해 디스코드 알림 설정. 알림의 경중에 따라 INFO, WARNING, ERROR 로 알림의 유형 구분 <br/>
- MySQL Workbench를 통한 서버 데이터 관리 <br/>
- 장바구니 도메인 테스트 코드 작성 <br/>
- 상품 도메인 테스트 코드 작성 <br/>
- 메인 페이지 슬라이드 광고 배너 제작 (망고보드 디자인 플랫폼 활용) <br/>
- 프로젝트 시연 영상 녹화 <br/>
<br/>
<br/>
  
---

<h3>※ 트러블 슈팅 </h3>
<h5> 1. AmazonS3Config 파일을 만들고, yml 파일을 통해 관련된 환경 변수들을 관리하는 과정</h5>
1-1) 앱 실행 환경에서 이미지 업로드 문제가 없었으나, test의 contextLoads()가 실패.<br/>
—> test 디렉토리에 있는 resources/application.yml 의 내용에 aws S3와 관련된 환경변수를 적용하지 않아 문제가 발생했음을 확인하고 이를 수정하였습니다.<br/>
<br/>
1-2) 앱 실행과 테스트 실행 모두 정상적으로 작동하는 것을 확인한 후 merge 하려 했으나 CI/CD 파이프라인의 test 단계에서 막혔습니다.<br/>
—> CI/CD Variable을 담은 파일에 env.yml 파일의 내용을 추가함으로써 해결하였습니다.<br/>
<br/>
<br/>

<h5> 2. 서버의 MySQL에 접속한 상태에서 각 local에서 app 실행을 하면 data.sql 에 입력된 INSERT 문이 계속 누적되어 중복 데이터가 쌓이는 문제 발생</h5>
—> application-dev.yml 파일의 ddl-auto 값을 update로 하고, data.sql 파일 상단에 외래 키 제약 조건 비활성화 → TRUNCATE TABLE 명령어로 테이블 삭제 → 외래 키 제약 조건 다시 활성화 처리함으로써 App을 재실행할 때마다 중복 데이터를 삭제하고 data.sql 에 입력했던 초기 데이터 상태를 id 값까지 유지할 수 있도록 해결하였습니다. 이는 임시적인 해결책으로, 현재는 validate로 설정되어 있습니다.
<br/>
<br/>

<h5> 3. API 호출 시 parameter name 정보를 인식하지 못해 작업을 수행하지 못하는 문제 발생</h5>
스프링 4.3 이상부터 명시적으로 변수 이름을 지정하는 것이 권장된다는 것을 확인하였습니다. 실제로 Controller에서 변수명을 지정해주지 않아 발생한 문제임을 확인했고,<br/>
—> @PathVariable(”변수명”) 을 지정
—> @RequestParam(name = “변수명”) 을 지정
함으로써 해결하였습니다.
<br/>
<br/>

<h5> 4. 상품 중에는 사료나 간식과 같이 음식이 아닌 경우는 해당 상품을 생성할 때부터 유통기간 필드값이 null 이다.  이러한 상품을 수정하려고 할 때, 유통기간 필드를 변경하지 않는다고 하더라도 NullPointerException이 뜨는 문제 발생
</h5>
—> NullPointerException은 null 참조를 통해 메서드를 호출하려고 하거나 null 참조의 속성에 접근하려고 할 때 발생하지만, 단순 조회 및 비교 시에는 발생하지 않는다는 것을 알게 되었습니다. 
<br/>

```java
LocalDate newExpirationDate = (request.getExpirationDate() != null) ? request.getExpirationDate() : product.getExpirationDate();
```

그러므로 위와 같은 삼항 연산자를 통해 `newExpirationDate` 를 선언하고, 상품을 수정하는 update 메서드 파라미터에는 request.getExpiration() 으로 값을 호출하지 않고 위에서 선언한 newExpirationDate 를 호출함으로써 null 참조를 통한 메서드를 회피하였습니다.<br/>
그리하여 기존 product의 expirationDate와 request의 expirationDate 모두 null이어도  NullPointerException이 발생하지 않게 되었습니다.<br/>
<br/>
<br/>

---

<h3>※ 현재 기준 개선할 점 (수정 및 리팩토링 중)</h3>
- OAuth 2.0 로그인 현재 오작동 중<br/>
- 상품 상세 페이지 수량 default 값을 0으로 변경하는 것이 좋을 것 같음<br/>
- 재고가 없는 상품(품절 상품)의 경우 장바구니 담기/바로구매 요청 막기 --> 버튼 비활성화 + stock 또는 isSoldOut 필드 검증 후 예외 처리<br/>
- 유저 로그인, 로그아웃, 회원정보 수정, 결제, 결제취소, 예외 처리에 대한 로그 처리 미완성<br/>
- 데이터베이스 기준 시간 +9 처리할 것<br/>
- 주문 취소 완료 후 다시 mypage/purchased 로 리다이렉트<br/>
- 배송완료된 주문 건을 주문취소할 경우 유저 피드백 모달창 / or 배송완료 주문건은 처음부터 주문취소 버튼 비활성화<br/>
- 주문/배송 화면에서 주문 내역에 있는 상품들에 상세 페이지 링크 걸어주기<br/>
- 헤더 검색창에서 입력 후 Enter 치면 결과로 이동하기<br/>
- 검색 결과에 나온 상품을 클릭했을 때 /categories/products?productId=108 가 아닌 /products?productId=108 로 이동하도록 수정<br/>
- 상품 상세페이지에서 리뷰 이미지가 나오도록 수정 / 리뷰 페이징 처리<br/>
- 메인 화면에 선착순 한정세일 리스트에는 재고량이 표시되게 하기<br/>
- 발견 시 추가입력<br/>
<br/>
<br/>

---

<h3>※ 프로젝트 소스 코드 (백엔드) </h3>
https://github.com/ggorangjirang/backend
<br/>
<br/>
<h3>※ 도메인 시연 영상 </h3>
https://www.youtube.com/watch?v=zfbEuNrcr0M
<br/>
<br/>
