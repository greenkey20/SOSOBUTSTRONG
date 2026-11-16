# SOSOBUTSTRONG (위메샐)

KH정보교육원 final 프로젝트(조별 활동, 2022.3월)

판매자와 구매자가 거래하는 웹 서비스 **위메샐**을 팀으로 개발한 프로젝트입니다. 이 저장소는 팀 저장소를 fork한 것이며, **제가 담당한 기능**은 아래에 정리했습니다.

- 원본 저장소(팀): [MoonKristal/SOSOBUTSTRONG2](https://github.com/MoonKristal/SOSOBUTSTRONG2)
- 내 작업 브랜치: [`Eunyoung`](https://github.com/greenkey20/SOSOBUTSTRONG/tree/Eunyoung/sosoButStrong/src/main/java/com/sbs/wemasal)
- 개발 기간: 2022.3.21 ~ 2022.4.17 (내 커밋 기준)
- 2022.4.16(토) fork

## 기술 스택

| 구분 | 사용 기술 |
|---|---|
| 백엔드 | Java 8, Spring MVC 5.3 (XML 설정), MyBatis, Spring Security, Spring WebSocket |
| 데이터베이스 | Oracle |
| 프론트엔드 | JSP/JSTL, jQuery(AJAX), CSS |
| 라이브러리 | JavaMail(이메일 발송), Apache POI(엑셀), Gson, commons-fileupload |
| 빌드 | Maven |

## 프로젝트 구성

패키지(`com.sbs.wemasal`) 기준으로 `member`, `seller`, `order`, `cart`, `community`, `like`, `customer`, `report`, `notice`, `question`, `sellerordermanage` 도메인과 관리자 화면(`admin`)으로 나뉩니다. 요청은 Controller → Service → Dao → MyBatis Mapper(`resources/mappers/*.xml`) 순으로 처리합니다.

## 내가 담당한 기능

문의(Q&A), 판매자 정산, 공지사항을 맡았습니다.

### 1. 1:1 문의 / 답변 (`question`)

- 구매자: 문의 등록(첨부파일 포함)·수정·삭제·검색·목록 조회(페이징)
- 판매자: 받은 문의 목록·상세 조회, 답변 등록(첨부파일 포함)
- 판매자가 답변을 등록하면 **JavaMail로 구매자에게 이메일을 발송**합니다.
- 답변 평점을 **AJAX**로 비동기 처리합니다. (`answerRating.qu`)
- 관련 코드: `QuestionController`, `QuestionService(Impl)`, `QuestionDao`, `question-mapper.xml`, `views/user/question/*`

### 2. 주문 연동 AJAX (문의 작성 화면)

- **구매자 주문 목록 비동기 조회**: 페이징 정보(`PageInfo`)와 주문 목록을 하나의 JSON 응답으로 내려, 화면에서 페이징 바까지 그립니다. (`buyerOrderList.qu`)
- **주문번호 확인**: 입력한 주문번호가 해당 구매자의 주문인지 비동기로 검증합니다. (`confirmOrderNo.qu`)
- **판매자명 자동완성 검색**: 입력 글자에 따라 판매자를 JSON으로 조회합니다. (`searchSellerName.qu`)

### 3. 판매자 정산 (`seller` / Settlement)

- **정산 현황(Overview)**: 오늘의 신규 주문 수, 배송 상태별(상품준비중·배송중·배송완료·취소) 건수, 오늘·내일 정산 예정 금액을 집계하는 Oracle SQL을 MyBatis로 작성했습니다. 수수료 5%를 반영해 정산 금액을 계산합니다.
- 현황 새로고침을 **AJAX**로 처리합니다. (`refreshOverview.st`)
- **정산 상세 목록**: 주문 단위로 정산 예정일·정산 상태·수수료·정산 금액을 보여 주고 페이징합니다.
- **엑셀 다운로드**: Apache POI로 정산 상세 내역을 `.xlsx`로 내려받습니다. (`downloadExcel.st`)
- 관련 코드: `SettlementController`, `SettlementService(Impl)`, `SettlementDao`, `settlement-mapper.xml`, `views/user/seller/settlement*`

### 4. 공지사항 (`notice`)

- 관리자: 공지 등록, 목록·상세 조회, **수정·삭제(첨부파일 삭제 포함)** — 수정·삭제 기능은 [`Eunyoung` 브랜치](https://github.com/greenkey20/SOSOBUTSTRONG/tree/Eunyoung/sosoButStrong/src/main/java/com/sbs/wemasal/notice)에 구현했습니다.
- 사용자: 공지 목록·상세 조회
- 관련 코드: `NoticeController`, `NoticeService(Impl)`, `NoticeDao`, `notice-mapper.xml`, `views/admin/notice/*`, `views/user/notice/*`

## 배운 점

- 팀 프로젝트에서 **Git 브랜치(개인 브랜치 → PR → merge)** 로 협업하고 충돌을 해결하는 방법
- **MyBatis**로 SQL과 객체를 매핑하고, 동적 검색 쿼리를 작성하는 방법
- **AJAX**로 JSON 응답(페이징 정보 포함)을 설계하고 화면에서 처리하는 방법
- 파일 업로드, 이메일 발송, 엑셀 생성 같은 **외부 라이브러리 연동**

> 이 저장소는 학습·포트폴리오 목적으로 팀 저장소를 fork한 것입니다. 팀원과 함께 만든 결과물이며, 위 "내가 담당한 기능" 외 부분의 저작권은 해당 팀원에게 있습니다.
