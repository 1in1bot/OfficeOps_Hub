# OfficeOps Hub 화면 정의서

## 문서 버전 이력

| 버전 | 기준 | 수정 사항 | 삭제 사항 |
| --- | --- | --- | --- |
| v1.0.0 | 관리자 전용 계정 생성 반영 이전 화면 명세 | 로그인/회원가입 공통 화면과 기존 사용자 관리 화면 기준선 | 없음 |
| v1.1.0 | 관리자 전용 계정 생성 반영 | `ADMIN-USER-CREATE` 화면과 `/admin/users/new` 라우트 추가, 사용자 관리 목록에 사용자 생성 액션 추가, 로그인 화면을 단독 인증 전 화면으로 정리 | `AUTH-SIGNUP`, `/signup`, 로그인 화면의 회원가입 이동 액션 제거 |
| v1.2.0 | 문서 네이밍 및 버전 관리 체계 정리 | 문서 파일명을 번호 없는 한글 제목 기반 규칙으로 정리하고 버전 표기를 semantic version 형식으로 통일 | 문서 번호 접두어와 영문 기반 산출물 파일명 제거 |
| v1.3.0 | 파일명 버전 최신화 규칙 반영 | 문서 파일명의 버전을 문서 내부 최신 버전과 동일하게 관리하도록 정리하고, 이후 수정 및 버전 상승 시 파일명과 참조 링크를 즉시 갱신하는 규칙 추가 | 최신 버전과 맞지 않는 파일명 버전 표기 제거 |

## 1. 문서 목적

이 문서는 `OfficeOps Hub` 프론트엔드에서 구현할 화면 목록, 라우팅 경로, 접근 권한, 주요 UI 요소, 사용자 액션, 연결 API를 정의한다.

React, Vite, React Router 기반 화면 구현의 기준 문서로 사용한다.

## 2. 화면 목록

| ID | 구분 | 화면명 | URL | 접근 권한 | 우선순위 |
| --- | --- | --- | --- | --- | --- |
| AUTH-LOGIN | 공통 | 로그인 | `/login` | 비로그인 | 필수 |
| COMMON-FORBIDDEN | 공통 | 권한 없음 | `/forbidden` | 전체 | 필수 |
| COMMON-NOT-FOUND | 공통 | 404 Not Found | `*` | 전체 | 필수 |
| USER-HOME | 사용자 | 사용자 홈 | `/user` | 로그인 사용자 | 필수 |
| USER-REQUEST-LIST | 사용자 | 내 요청 목록 | `/user/requests` | 로그인 사용자 | 필수 |
| USER-REQUEST-CREATE | 사용자 | 요청 등록 | `/user/requests/new` | 로그인 사용자 | 필수 |
| USER-REQUEST-DETAIL | 사용자 | 요청 상세 | `/user/requests/:id` | 작성자, 관리자 | 필수 |
| USER-RESERVATION-LIST | 사용자 | 내 예약 목록 | `/user/reservations` | 로그인 사용자 | 필수 |
| USER-RESERVATION-CREATE | 사용자 | 회의실 예약 | `/user/reservations/new` | 로그인 사용자 | 필수 |
| USER-RESERVATION-CALENDAR | 사용자 | 예약 캘린더 | `/user/reservations/calendar` | 로그인 사용자 | 권장 |
| USER-ASSET-LIST | 사용자 | 자산 목록 | `/user/assets` | 로그인 사용자 | 필수 |
| USER-ASSET-LOAN-LIST | 사용자 | 내 자산 대여 현황 | `/user/asset-loans` | 로그인 사용자 | 권장 |
| USER-ME | 사용자 | 내 정보 | `/user/me` | 로그인 사용자 | 필수 |
| USER-PASSWORD | 사용자 | 비밀번호 변경 | `/user/me/password` | 로그인 사용자 | MVP 이후 |
| USER-NOTIFICATION-LIST | 사용자 | 알림 목록 | `/user/notifications` | 로그인 사용자 | 2순위 |
| USER-ATTENDANCE-HOME | 사용자 | 내 근태 홈 | `/user/attendance` | 로그인 사용자 | 필수 |
| USER-LEAVE-BALANCE | 사용자 | 내 연차 현황 | `/user/attendance/leaves` | 로그인 사용자 | 필수 |
| USER-ATTENDANCE-RECORDS | 사용자 | 내 출퇴근 기록 | `/user/attendance/records` | 로그인 사용자 | 필수 |
| USER-APPROVAL-LIST | 사용자 | 내 전자결재 문서 | `/user/approvals` | 로그인 사용자 | 필수 |
| USER-APPROVAL-CREATE | 사용자 | 전자결재 작성 | `/user/approvals/new` | 로그인 사용자 | 필수 |
| USER-APPROVAL-DETAIL | 사용자 | 전자결재 상세 | `/user/approvals/:id` | 작성자, 결재자, 참조자, 관리자 | 필수 |
| MANAGER-APPROVAL-LIST | 팀장 | 팀 승인 요청함 | `/manager/approvals` | ROLE_MANAGER, ROLE_ADMIN | 필수 |
| MANAGER-APPROVAL-DETAIL | 팀장 | 팀 승인 요청 상세 | `/manager/approvals/:id` | ROLE_MANAGER, ROLE_ADMIN | 필수 |
| MANAGER-EAPPROVAL-LIST | 팀장 | 팀 전자결재함 | `/manager/e-approvals` | ROLE_MANAGER, ROLE_ADMIN | 필수 |
| MANAGER-EAPPROVAL-DETAIL | 팀장 | 팀 전자결재 상세 | `/manager/e-approvals/:id` | ROLE_MANAGER, ROLE_ADMIN | 필수 |
| ADMIN-DASHBOARD | 관리자 | 관리자 대시보드 | `/admin/dashboard` | ROLE_ADMIN | 필수 |
| OPERATOR-REQUEST-LIST | 운영 | 운영 요청함 | `/operator/requests` | ROLE_OPERATOR, ROLE_ADMIN | 필수 |
| OPERATOR-REQUEST-DETAIL | 운영 | 운영 요청 상세 | `/operator/requests/:id` | ROLE_OPERATOR, ROLE_ADMIN | 필수 |
| HR-ATTENDANCE-DASHBOARD | HR | 근태 대시보드 | `/hr/attendance` | ROLE_HR, ROLE_ADMIN | 필수 |
| HR-LEAVE-MANAGEMENT | HR | 연차 관리 | `/hr/leaves` | ROLE_HR, ROLE_ADMIN | 필수 |
| HR-CERTIFICATE-LIST | HR | 증명서 발급 요청 | `/hr/certificates` | ROLE_HR, ROLE_ADMIN | 필수 |
| HR-EAPPROVAL-LIST | HR | HR 전자결재함 | `/hr/e-approvals` | ROLE_HR, ROLE_ADMIN | 필수 |
| FINANCE-EAPPROVAL-LIST | 재무 | 재무 전자결재함 | `/finance/e-approvals` | ROLE_FINANCE, ROLE_ADMIN | 필수 |
| FINANCE-EXPENSE-LIST | 재무 | 지출/법인카드 처리 | `/finance/expenses` | ROLE_FINANCE, ROLE_ADMIN | 필수 |
| ADMIN-REQUEST-LIST | 관리자 | 전체 요청 관리 | `/admin/requests` | ROLE_OPERATOR, ROLE_ADMIN | 필수 |
| ADMIN-REQUEST-DETAIL | 관리자 | 요청 상세 관리 | `/admin/requests/:id` | ROLE_OPERATOR, ROLE_ADMIN | 필수 |
| ADMIN-ASSET-LIST | 관리자 | 자산 관리 | `/admin/assets` | ROLE_OPERATOR, ROLE_ADMIN | 필수 |
| ADMIN-RESERVATION-LIST | 관리자 | 예약 관리 | `/admin/reservations` | ROLE_OPERATOR, ROLE_ADMIN | 필수 |
| ADMIN-RESERVATION-CALENDAR | 관리자 | 예약 캘린더 | `/admin/reservations/calendar` | ROLE_OPERATOR, ROLE_ADMIN | 권장 |
| ADMIN-RESOURCE-LIST | 관리자 | 회의실/자원 관리 | `/admin/resources` | ROLE_ADMIN | 필수 |
| ADMIN-USER-LIST | 관리자 | 사용자 관리 | `/admin/users` | ROLE_ADMIN | 선택 |
| ADMIN-USER-CREATE | 관리자 | 사용자 계정 생성 | `/admin/users/new` | ROLE_ADMIN | 필수 |
| ADMIN-USER-DETAIL | 관리자 | 사용자 상세/권한 변경 | `/admin/users/:id` | ROLE_ADMIN | 권장 |
| ADMIN-APPROVAL-LIST | 관리자 | 전체 전자결재 관리 | `/admin/approvals` | ROLE_ADMIN | 필수 |
| ADMIN-APPROVAL-FORM-LIST | 관리자 | 전자결재 양식 관리 | `/admin/approval-forms` | ROLE_ADMIN | 필수 |
| ADMIN-ATTENDANCE-POLICY | 관리자 | 근태 정책 관리 | `/admin/attendance-policies` | ROLE_ADMIN | 필수 |
| ADMIN-AUDIT-LOG-LIST | 관리자 | 감사 이력 | `/admin/audit-logs` | ROLE_ADMIN | 필수 |

## 3. 공통 레이아웃

### 3.1 인증 전 레이아웃

대상 화면:

- 로그인

구성 요소:

- 서비스명
- 입력 폼
- 제출 버튼
- 에러 메시지 영역

### 3.2 사용자 레이아웃

대상 화면:

- `/user` 하위 화면

구성 요소:

- 상단바
- 사이드바
- 사용자 이름/부서 표시
- 로그아웃 버튼
- 메뉴: 사용자 홈, 내 요청, 회의실 예약, 예약 캘린더, 자산 목록, 내 자산 대여 현황, 내 근태, 전자결재, 알림, 내 정보

### 3.3 관리자 레이아웃

대상 화면:

- `/admin` 하위 화면
- `/manager` 하위 화면
- `/operator` 하위 화면

구성 요소:

- 상단바
- 사이드바
- 관리자 표시
- 로그아웃 버튼
- 운영 담당자 메뉴: 운영 요청함, 자산 관리, 예약 관리
- 팀장 메뉴: 팀 승인 요청함, 팀 전자결재함
- HR 메뉴: 근태 대시보드, 연차 관리, 증명서 발급 요청, HR 전자결재함
- 재무 메뉴: 재무 전자결재함, 지출/법인카드 처리
- 관리자 메뉴: 대시보드, 요청 관리, 자산 관리, 예약 관리, 회의실/자원 관리, 사용자 관리, 전자결재 관리, 전자결재 양식 관리, 근태 정책 관리, 감사 이력

## 4. 공통 화면

### AUTH-LOGIN. 로그인

| 항목 | 내용 |
| --- | --- |
| URL | `/login` |
| 접근 권한 | 비로그인 사용자 |
| 목적 | 사용자가 서비스에 로그인한다. |
| 연결 API | `POST /api/auth/login` |

입력 항목:

| 항목 | 타입 | 필수 |
| --- | --- | --- |
| 이메일 | text/email | Y |
| 비밀번호 | password | Y |

버튼/액션:

| 액션 | 설명 |
| --- | --- |
| 로그인 | 로그인 API 호출 |

처리 규칙:

- 로그인 성공 시 사용자 권한에 따라 이동한다.
- 일반 직원은 `/user`로 이동한다.
- 관리자는 `/admin/dashboard`로 이동한다.
- 로그인 실패 시 에러 메시지를 표시한다.

### COMMON-FORBIDDEN. 권한 없음

| 항목 | 내용 |
| --- | --- |
| URL | `/forbidden` |
| 접근 권한 | 전체 |
| 목적 | 접근 권한이 없는 사용자에게 안내를 표시한다. |

버튼/액션:

| 액션 | 설명 |
| --- | --- |
| 이전 화면 | 브라우저 히스토리 뒤로 이동 |
| 홈으로 | 사용자 권한에 맞는 홈으로 이동 |

### COMMON-NOT-FOUND. 404 Not Found

| 항목 | 내용 |
| --- | --- |
| URL | 정의되지 않은 전체 경로 |
| 접근 권한 | 전체 |
| 목적 | 존재하지 않는 URL 접근 시 안내를 표시한다. |

표시 정보:

- 404 안내 문구
- 요청한 페이지를 찾을 수 없다는 메시지

버튼/액션:

| 액션 | 설명 |
| --- | --- |
| 홈으로 | 로그인 여부와 권한에 맞는 홈으로 이동 |
| 로그인으로 | 비로그인 사용자인 경우 로그인 화면으로 이동 |

## 5. 사용자 화면

### USER-HOME. 사용자 홈

| 항목 | 내용 |
| --- | --- |
| URL | `/user` |
| 접근 권한 | 로그인 사용자 |
| 목적 | 내 요청, 내 예약 현황을 요약해서 보여준다. |
| 연결 API | `GET /api/requests`, `GET /api/reservations` |

주요 UI:

- 내 요청 상태 요약 카드
- 최근 요청 목록
- 오늘/이번 주 예약 목록
- 빠른 요청 등록 버튼
- 빠른 예약 등록 버튼

### USER-REQUEST-LIST. 내 요청 목록

| 항목 | 내용 |
| --- | --- |
| URL | `/user/requests` |
| 접근 권한 | 로그인 사용자 |
| 목적 | 내가 등록한 요청 목록을 조회한다. |
| 연결 API | `GET /api/requests` |

검색/필터:

- 상태
- 요청 유형
- 우선순위
- 날짜 범위
- 키워드

테이블 컬럼:

- 요청 번호
- 요청 유형
- 제목
- 상태
- 우선순위
- 등록일
- 최종 수정일

버튼/액션:

| 액션 | 설명 |
| --- | --- |
| 요청 등록 | `/user/requests/new`로 이동 |
| 상세 보기 | `/user/requests/:id`로 이동 |
| 검색 | 조건 기반 목록 재조회 |
| 초기화 | 검색 조건 초기화 |

### USER-REQUEST-CREATE. 요청 등록

| 항목 | 내용 |
| --- | --- |
| URL | `/user/requests/new` |
| 접근 권한 | 로그인 사용자 |
| 목적 | 새 요청을 등록한다. |
| 연결 API | `POST /api/requests` |

입력 항목:

| 항목 | 타입 | 필수 |
| --- | --- | --- |
| 요청 유형 | select | Y |
| 제목 | text | Y |
| 내용 | textarea | Y |
| 우선순위 | select | Y |

요청 유형별 추가 입력 항목:

| 요청 유형 | 항목 | 타입 | 필수 |
| --- | --- | --- | --- |
| ASSET_REQUEST | 희망 자산 카테고리 | select/text | Y |
| ASSET_REQUEST | 사용 시작일 | date | Y |
| ASSET_REQUEST | 반납 예정일 | date | Y |
| VISITOR_REQUEST | 방문자 이름 | text | Y |
| VISITOR_REQUEST | 방문자 연락처 | text | N |
| VISITOR_REQUEST | 방문 일시 | datetime | Y |
| VISITOR_REQUEST | 방문 목적 | textarea | Y |
| FACILITY_REQUEST | 위치 | text/select | Y |
| FACILITY_REQUEST | 문제 유형 | select | Y |
| FACILITY_REQUEST | 희망 처리일 | date | N |

버튼/액션:

| 액션 | 설명 |
| --- | --- |
| 등록 | 요청 등록 API 호출 |
| 취소 | 내 요청 목록으로 이동 |

처리 규칙:

- 등록 성공 시 생성된 요청 상세 화면으로 이동한다.
- 입력값 오류는 각 필드 근처에 표시한다.

### USER-REQUEST-DETAIL. 요청 상세

| 항목 | 내용 |
| --- | --- |
| URL | `/user/requests/:id` |
| 접근 권한 | 요청 작성자 또는 관리자 |
| 목적 | 요청 상세와 처리 이력을 확인한다. |
| 연결 API | `GET /api/requests/{id}`, `GET /api/requests/{id}/histories` |

표시 정보:

- 요청 유형
- 제목
- 내용
- 상태
- 우선순위
- 요청자
- 부서
- 등록일
- 반려 사유
- 관리자 메모
- 담당자
- 처리 예정일
- 마감일
- 승인 이력
- 댓글/처리 코멘트
- 상태 변경 이력

버튼/액션:

| 액션 | 표시 조건 | 설명 |
| --- | --- | --- |
| 수정 | 본인 요청이고 상태가 PENDING_MANAGER_APPROVAL | 상세 화면 내 수정 모드 진입 |
| 취소 | 본인 요청이고 최종 상태가 아님 | 요청 취소 API 호출 |
| 목록 | 항상 | 내 요청 목록으로 이동 |

수정 방식:

- 별도 수정 URL은 만들지 않는다.
- 상세 화면에서 `수정` 버튼을 누르면 읽기 모드가 수정 모드로 전환된다.
- 수정 모드에서는 제목, 내용, 우선순위, 요청 유형별 추가 입력값을 수정할 수 있다.
- 저장 성공 시 다시 읽기 모드로 전환하고 상세 데이터를 재조회한다.
- 취소 버튼을 누르면 수정 전 데이터로 되돌리고 읽기 모드로 전환한다.

### USER-RESERVATION-LIST. 내 예약 목록

| 항목 | 내용 |
| --- | --- |
| URL | `/user/reservations` |
| 접근 권한 | 로그인 사용자 |
| 목적 | 내가 등록한 예약 목록을 조회한다. |
| 연결 API | `GET /api/reservations` |

검색/필터:

- 날짜
- 회의실
- 상태

테이블 컬럼:

- 예약 번호
- 회의실
- 위치
- 시작 시간
- 종료 시간
- 상태
- 목적

버튼/액션:

| 액션 | 설명 |
| --- | --- |
| 예약 등록 | `/user/reservations/new`로 이동 |
| 예약 취소 | 예약 취소 API 호출 |

### USER-RESERVATION-CREATE. 회의실 예약

| 항목 | 내용 |
| --- | --- |
| URL | `/user/reservations/new` |
| 접근 권한 | 로그인 사용자 |
| 목적 | 회의실을 예약한다. |
| 연결 API | `GET /api/resources`, `POST /api/reservations` |

입력 항목:

| 항목 | 타입 | 필수 |
| --- | --- | --- |
| 회의실 | select | Y |
| 예약 날짜 | date | Y |
| 시작 시간 | time/select | Y |
| 종료 시간 | time/select | Y |
| 목적 | textarea | Y |

처리 규칙:

- 예약은 30분 단위로 선택한다.
- 시작 시간은 종료 시간보다 이전이어야 한다.
- 예약 가능 시간 외 선택은 막는다.
- 예약 중복 에러 발생 시 안내 메시지를 표시한다.

### USER-RESERVATION-CALENDAR. 예약 캘린더

| 항목 | 내용 |
| --- | --- |
| URL | `/user/reservations/calendar` |
| 접근 권한 | 로그인 사용자 |
| 목적 | 날짜별/회의실별 예약 현황을 캘린더로 확인한다. |
| 연결 API | 예약 캘린더 조회 API |
| 우선순위 | 권장 |

주요 UI:

- 월간/주간/일간 보기 전환
- 회의실 필터
- 예약 상태 필터
- 내 예약 강조 표시
- 예약 등록 버튼

처리 규칙:

- 사용자는 예약 시간대와 회의실을 확인할 수 있다.
- 타인의 예약은 예약자 상세 정보 없이 표시할 수 있다.
- 내 예약은 다른 색상 또는 표시 방식으로 구분한다.

### USER-ASSET-LIST. 자산 목록

| 항목 | 내용 |
| --- | --- |
| URL | `/user/assets` |
| 접근 권한 | 로그인 사용자 |
| 목적 | 사내 자산 목록과 사용 가능 상태를 조회한다. |
| 연결 API | `GET /api/assets` |

검색/필터:

- 키워드
- 카테고리
- 상태
- 위치

테이블 컬럼:

- 자산 코드
- 자산명
- 카테고리
- 상태
- 위치

### USER-ASSET-LOAN-LIST. 내 자산 대여 현황

| 항목 | 내용 |
| --- | --- |
| URL | `/user/asset-loans` |
| 접근 권한 | 로그인 사용자 |
| 목적 | 내가 대여 중이거나 과거 대여한 자산을 확인한다. |
| 연결 API | 내 자산 대여 목록 API |
| 우선순위 | 권장 |

검색/필터:

- 대여 상태
- 반납 예정일
- 자산명

테이블 컬럼:

- 자산 코드
- 자산명
- 대여일
- 반납 예정일
- 실제 반납일
- 상태

### USER-ME. 내 정보

| 항목 | 내용 |
| --- | --- |
| URL | `/user/me` |
| 접근 권한 | 로그인 사용자 |
| 목적 | 내 계정 정보를 확인한다. |
| 연결 API | `GET /api/users/me` |

표시 정보:

- 이름
- 이메일
- 부서
- 권한
- 가입일

버튼/액션:

| 액션 | 설명 |
| --- | --- |
| 비밀번호 변경 | `/user/me/password`로 이동 |

### USER-PASSWORD. 비밀번호 변경

| 항목 | 내용 |
| --- | --- |
| URL | `/user/me/password` |
| 접근 권한 | 로그인 사용자 |
| 목적 | 로그인 사용자가 본인 비밀번호를 변경한다. |
| 연결 API | 비밀번호 변경 API |
| 우선순위 | MVP 이후 |

입력 항목:

| 항목 | 타입 | 필수 |
| --- | --- | --- |
| 현재 비밀번호 | password | Y |
| 새 비밀번호 | password | Y |
| 새 비밀번호 확인 | password | Y |

### USER-NOTIFICATION-LIST. 알림 목록

| 항목 | 내용 |
| --- | --- |
| URL | `/user/notifications` |
| 접근 권한 | 로그인 사용자 |
| 목적 | 요청 처리 결과와 예약/자산 관련 알림을 확인한다. |
| 연결 API | 알림 목록 API |
| 우선순위 | MVP 이후 |

표시 정보:

- 알림 유형
- 알림 내용
- 읽음 여부
- 생성일

## 6. 팀장 화면

### MANAGER-APPROVAL-LIST. 팀 승인 요청함

| 항목 | 내용 |
| --- | --- |
| URL | `/manager/approvals` |
| 접근 권한 | ROLE_MANAGER, ROLE_ADMIN |
| 목적 | 소속 팀원의 승인 대기 요청을 조회한다. |
| 연결 API | 팀장 승인 요청 목록 API |

검색/필터:

- 요청 유형
- 우선순위
- 요청자
- 날짜 범위
- 키워드

테이블 컬럼:

- 요청 번호
- 요청 유형
- 제목
- 요청자
- 우선순위
- 상태
- 등록일

버튼/액션:

| 액션 | 설명 |
| --- | --- |
| 상세 보기 | `/manager/approvals/:id`로 이동 |

### MANAGER-APPROVAL-DETAIL. 팀 승인 요청 상세

| 항목 | 내용 |
| --- | --- |
| URL | `/manager/approvals/:id` |
| 접근 권한 | ROLE_MANAGER, ROLE_ADMIN |
| 목적 | 소속 팀원의 요청을 1차 승인 또는 반려한다. |
| 연결 API | 팀장 승인/반려 API |

표시 정보:

- 요청 기본 정보
- 요청자 정보
- 요청 유형별 추가 입력값
- 현재 상태
- 상태 변경 이력

버튼/액션:

| 액션 | 표시 조건 | 설명 |
| --- | --- | --- |
| 1차 승인 | PENDING_MANAGER_APPROVAL | 운영 담당자 승인 대기로 변경 |
| 반려 | PENDING_MANAGER_APPROVAL | 반려 사유 입력 후 반려 |
| 목록 | 항상 | 팀 승인 요청함으로 이동 |

## 7. 운영 담당자 화면

### OPERATOR-REQUEST-LIST. 운영 요청함

| 항목 | 내용 |
| --- | --- |
| URL | `/operator/requests` |
| 접근 권한 | ROLE_OPERATOR, ROLE_ADMIN |
| 목적 | 운영 담당자가 최종 승인 대기, 미배정, 처리 중 요청을 관리한다. |
| 연결 API | 운영 요청 목록 API |

검색/필터:

- 상태
- 요청 유형
- 우선순위
- 담당자
- 마감일
- 지연 여부
- 미배정 여부
- 날짜 범위

테이블 컬럼:

- 요청 번호
- 요청 유형
- 제목
- 요청자
- 팀장 승인일
- 담당자
- 마감일
- 상태

버튼/액션:

| 액션 | 설명 |
| --- | --- |
| 상세 보기 | `/operator/requests/:id`로 이동 |

### OPERATOR-REQUEST-DETAIL. 운영 요청 상세

| 항목 | 내용 |
| --- | --- |
| URL | `/operator/requests/:id` |
| 접근 권한 | ROLE_OPERATOR, ROLE_ADMIN |
| 목적 | 운영 담당자가 요청을 최종 승인하고 담당자/마감일/처리 상태를 관리한다. |
| 연결 API | 운영 최종 승인/반려/담당자 지정/마감일/처리 상태 API |

표시 정보:

- 요청 기본 정보
- 요청자 정보
- 팀장 승인 이력
- 담당자
- 처리 예정일
- 마감일
- 댓글/처리 코멘트
- 상태 변경 이력

버튼/액션:

| 액션 | 표시 조건 | 설명 |
| --- | --- | --- |
| 최종 승인 | PENDING_OPERATOR_APPROVAL | 담당자와 마감일 입력 후 승인 |
| 최종 반려 | PENDING_OPERATOR_APPROVAL | 반려 사유 입력 후 반려 |
| 담당자 변경 | APPROVED, IN_PROGRESS | 담당자 변경 |
| 마감일 변경 | APPROVED, IN_PROGRESS | 처리 예정일/마감일 변경 |
| 처리 중 | APPROVED | 처리 중으로 변경 |
| 완료 | IN_PROGRESS | 완료 처리 |
| 코멘트 작성 | 항상 | 처리 코멘트 등록 |

## 8. 관리자 화면

### ADMIN-DASHBOARD. 관리자 대시보드

| 항목 | 내용 |
| --- | --- |
| URL | `/admin/dashboard` |
| 접근 권한 | ROLE_ADMIN |
| 목적 | 운영 현황을 요약해서 확인한다. |
| 연결 API | `GET /api/admin/dashboard/summary` |

주요 UI:

- 전체 요청 수 카드
- 처리 완료율 카드
- 대기 중 요청 수 카드
- 미배정 요청 수 카드
- 지연 요청 수 카드
- 오늘 처리 예정 요청 수 카드
- 오늘 예약 수 카드
- 요청 상태별 차트
- 월별 요청 추이 차트
- 자산 상태별 차트
- 회의실/자원 상태 요약
- 최근 요청 목록

### ADMIN-REQUEST-LIST. 전체 요청 관리

| 항목 | 내용 |
| --- | --- |
| URL | `/admin/requests` |
| 접근 권한 | ROLE_ADMIN |
| 목적 | 전체 요청을 조회하고 관리한다. |
| 연결 API | `GET /api/admin/requests` |

검색/필터:

- 상태
- 요청 유형
- 우선순위
- 요청자 이름
- 부서
- 담당자
- 마감일
- 지연 여부
- 미배정 여부
- 날짜 범위
- 키워드

테이블 컬럼:

- 요청 번호
- 요청 유형
- 제목
- 요청자
- 부서
- 담당자
- 마감일
- 상태
- 우선순위
- 등록일

버튼/액션:

| 액션 | 설명 |
| --- | --- |
| 상세 보기 | `/admin/requests/:id`로 이동 |
| 검색 | 조건 기반 목록 재조회 |
| 초기화 | 검색 조건 초기화 |

### ADMIN-REQUEST-DETAIL. 요청 상세 관리

| 항목 | 내용 |
| --- | --- |
| URL | `/admin/requests/:id` |
| 접근 권한 | ROLE_ADMIN |
| 목적 | 요청 내용을 확인하고 상태를 변경한다. |
| 연결 API | 요청 상세/승인/반려/처리 중/완료 API |

표시 정보:

- 요청 기본 정보
- 요청자 정보
- 현재 상태
- 요청 내용
- 반려 사유
- 관리자 메모
- 담당자
- 처리 예정일
- 마감일
- 승인 이력
- 댓글/처리 코멘트
- 상태 변경 이력

버튼/액션:

| 액션 | 표시 조건 | 연결 API |
| --- | --- | --- |
| 최종 승인 | PENDING_OPERATOR_APPROVAL | 운영 최종 승인 API |
| 최종 반려 | PENDING_OPERATOR_APPROVAL | 운영 최종 반려 API |
| 담당자 변경 | APPROVED, IN_PROGRESS | 담당자 변경 API |
| 마감일 변경 | APPROVED, IN_PROGRESS | 처리 예정일/마감일 변경 API |
| 처리 중 | APPROVED | `PATCH /api/admin/requests/{id}/in-progress` |
| 완료 | IN_PROGRESS | `PATCH /api/admin/requests/{id}/complete` |
| 메모 저장 | 항상 | `PATCH /api/admin/requests/{id}/memo` |
| 목록 | 항상 | `/admin/requests`로 이동 |

처리 규칙:

- 반려 버튼 클릭 시 반려 사유 입력 모달을 표시한다.
- 상태 변경 성공 시 상세 데이터를 다시 조회한다.
- 잘못된 상태 변경 에러는 화면 상단에 표시한다.

### ADMIN-ASSET-LIST. 자산 관리

| 항목 | 내용 |
| --- | --- |
| URL | `/admin/assets` |
| 접근 권한 | ROLE_ADMIN |
| 목적 | 자산을 등록, 수정, 상태 변경한다. |
| 연결 API | 자산 목록/등록/수정/상태 변경 API |

검색/필터:

- 키워드
- 카테고리
- 상태
- 위치

테이블 컬럼:

- 자산 코드
- 자산명
- 카테고리
- 시리얼 번호
- 상태
- 위치
- 등록일

버튼/액션:

| 액션 | 설명 |
| --- | --- |
| 자산 등록 | 등록 모달 열기 |
| 자산 수정 | 수정 모달 열기 |
| 상태 변경 | 상태 변경 메뉴 표시 |
| 대여 처리 | 대여 모달 열기 |
| 반납 처리 | 반납 확인 후 API 호출 |

### ADMIN-RESERVATION-LIST. 예약 관리

| 항목 | 내용 |
| --- | --- |
| URL | `/admin/reservations` |
| 접근 권한 | ROLE_ADMIN |
| 목적 | 전체 예약 현황을 조회하고 취소 처리한다. |
| 연결 API | `GET /api/admin/reservations`, `PATCH /api/admin/reservations/{id}/cancel` |

검색/필터:

- 날짜
- 회의실
- 예약자
- 상태

테이블 컬럼:

- 예약 번호
- 회의실
- 예약자
- 부서
- 시작 시간
- 종료 시간
- 상태
- 목적

버튼/액션:

| 액션 | 설명 |
| --- | --- |
| 예약 취소 | 관리자 권한으로 예약 취소 |

### ADMIN-RESERVATION-CALENDAR. 예약 캘린더

| 항목 | 내용 |
| --- | --- |
| URL | `/admin/reservations/calendar` |
| 접근 권한 | ROLE_ADMIN |
| 목적 | 전체 예약 현황을 캘린더로 확인한다. |
| 연결 API | 관리자 예약 캘린더 조회 API |
| 우선순위 | 권장 |

주요 UI:

- 월간/주간/일간 보기 전환
- 회의실 필터
- 예약자 필터
- 상태 필터
- 예약 상세 팝오버
- 예약 취소 액션

처리 규칙:

- 관리자는 예약자 이름과 부서를 확인할 수 있다.
- 관리자는 캘린더에서 예약 취소 액션을 수행할 수 있다.

### ADMIN-RESOURCE-LIST. 회의실/자원 관리

| 항목 | 내용 |
| --- | --- |
| URL | `/admin/resources` |
| 접근 권한 | ROLE_ADMIN |
| 목적 | 회의실과 예약 자원을 등록, 수정, 상태 변경한다. |
| 연결 API | 자원 목록/등록/수정/상태 변경 API |

검색/필터:

- 키워드
- 자원 유형
- 위치
- 상태

테이블 컬럼:

- 자원 번호
- 자원명
- 유형
- 위치
- 수용 인원
- 상태
- 등록일

입력 항목:

| 항목 | 타입 | 필수 |
| --- | --- | --- |
| 자원명 | text | Y |
| 유형 | select | Y |
| 위치 | text | Y |
| 수용 인원 | number | Y |
| 상태 | select | Y |

버튼/액션:

| 액션 | 설명 |
| --- | --- |
| 자원 등록 | 등록 모달 열기 |
| 자원 수정 | 수정 모달 열기 |
| 사용 가능 처리 | 상태를 AVAILABLE로 변경 |
| 사용 중지 처리 | 상태를 DISABLED로 변경 |

### ADMIN-USER-LIST. 사용자 관리

| 항목 | 내용 |
| --- | --- |
| URL | `/admin/users` |
| 접근 권한 | ROLE_ADMIN |
| 목적 | 사용자 목록을 조회한다. |
| 연결 API | 관리자 사용자 목록 API |
| 우선순위 | 선택 |

테이블 컬럼:

- 사용자 번호
- 이름
- 이메일
- 부서
- 권한
- 상태
- 가입일

버튼/액션:

| 액션 | 설명 |
| --- | --- |
| 사용자 생성 | `/admin/users/new`로 이동 |
| 상세 보기 | `/admin/users/:id`로 이동 |

### ADMIN-USER-CREATE. 사용자 계정 생성

| 항목 | 내용 |
| --- | --- |
| URL | `/admin/users/new` |
| 접근 권한 | ROLE_ADMIN |
| 목적 | 시스템 관리자가 사용자 계정을 생성한다. |
| 연결 API | `POST /api/admin/users` |
| 우선순위 | 필수 |

입력 항목:

| 항목 | 타입 | 필수 |
| --- | --- | --- |
| 이름 | text | Y |
| 이메일 | text/email | Y |
| 부서 | select | Y |
| 초기 권한 | select | Y |
| 계정 상태 | select | Y |
| 임시 비밀번호 | password/text | Y |

버튼/액션:

| 액션 | 설명 |
| --- | --- |
| 생성 | 사용자 계정 생성 API 호출 |
| 취소 | 사용자 관리 목록으로 이동 |

처리 규칙:

- 사용자 계정 생성은 `ROLE_ADMIN` 전용이다.
- 생성 성공 시 사용자 상세 또는 사용자 관리 목록으로 이동한다.
- 이메일 중복 또는 입력값 오류는 화면에 표시한다.
- 사용자 생성 이력은 감사 이력으로 저장한다.

### ADMIN-USER-DETAIL. 사용자 상세/권한 변경

| 항목 | 내용 |
| --- | --- |
| URL | `/admin/users/:id` |
| 접근 권한 | ROLE_ADMIN |
| 목적 | 사용자 상세 정보를 확인하고 권한을 변경한다. |
| 연결 API | 사용자 상세 조회/권한 변경 API |
| 우선순위 | 권장 |

표시 정보:

- 이름
- 이메일
- 부서
- 권한
- 상태
- 가입일
- 최근 요청 요약
- 최근 예약 요약
- 자산 대여 현황 요약

버튼/액션:

| 액션 | 설명 |
| --- | --- |
| 권한 변경 | ROLE_USER, ROLE_MANAGER, ROLE_OPERATOR, ROLE_HR, ROLE_FINANCE, ROLE_ADMIN으로 변경 |
| 비활성화 | ACTIVE 사용자를 INACTIVE로 변경 |
| 활성화 | INACTIVE 사용자를 ACTIVE로 변경 |
| 목록 | 사용자 관리 목록으로 이동 |

### ADMIN-AUDIT-LOG-LIST. 감사 이력

| 항목 | 내용 |
| --- | --- |
| URL | `/admin/audit-logs` |
| 접근 권한 | ROLE_ADMIN |
| 목적 | 관리자와 운영 담당자의 주요 작업 이력을 조회한다. |
| 연결 API | 감사 이력 목록 API |

검색/필터:

- 작업자
- 작업 유형
- 대상 리소스 유형
- 날짜 범위

테이블 컬럼:

- 이력 번호
- 작업자
- 작업 유형
- 대상 리소스
- 변경 전 값
- 변경 후 값
- 작업 일시

## 9. 라우팅 보호 규칙

| 조건 | 처리 |
| --- | --- |
| 비로그인 사용자가 `/user/*` 접근 | `/login`으로 이동 |
| 비로그인 사용자가 `/manager/*` 접근 | `/login`으로 이동 |
| 비로그인 사용자가 `/operator/*` 접근 | `/login`으로 이동 |
| 비로그인 사용자가 `/admin/*` 접근 | `/login`으로 이동 |
| ROLE_USER가 `/manager/*` 접근 | `/forbidden`으로 이동 |
| ROLE_USER가 `/operator/*` 접근 | `/forbidden`으로 이동 |
| ROLE_USER가 `/admin/*` 접근 | `/forbidden`으로 이동 |
| ROLE_MANAGER가 `/operator/*` 접근 | `/forbidden`으로 이동 |
| ROLE_MANAGER가 `/admin/*` 접근 | `/forbidden`으로 이동 |
| ROLE_OPERATOR가 `/manager/*` 접근 | `/forbidden`으로 이동 |
| ROLE_OPERATOR가 관리자 전용 화면 접근 | `/forbidden`으로 이동 |
| 로그인 사용자가 `/login` 접근 | 권한에 맞는 홈으로 이동 |
| 존재하지 않는 URL 접근 | 404 Not Found 화면 표시 |

## 10. HR/전자결재 화면

### USER-ATTENDANCE-HOME. 내 근태 홈

| 항목 | 내용 |
| --- | --- |
| URL | `/user/attendance` |
| 접근 권한 | 로그인 사용자 |
| 목적 | 내 연차, 오늘 근무 상태, 최근 근태 신청을 요약한다. |
| 연결 API | `GET /api/attendance/me/summary`, `GET /api/attendance/me/records`, `GET /api/approvals` |

주요 UI:

- 오늘 출근/퇴근 상태 카드
- 이번 달 근무시간 요약
- 연차 잔여일수 카드
- 최근 근태/연차 신청 목록
- 빠른 액션: 출근, 퇴근, 연차 신청, 근무제 신청

### USER-LEAVE-BALANCE. 내 연차 현황

| 항목 | 내용 |
| --- | --- |
| URL | `/user/attendance/leaves` |
| 접근 권한 | 로그인 사용자 |
| 목적 | 내 연차 발생/사용/잔여 현황과 사용 내역을 확인한다. |
| 연결 API | `GET /api/attendance/leaves/me`, `GET /api/attendance/leaves/me/usages` |

주요 UI:

- 기준 연도 선택
- 총 발생/사용/예정 차감/잔여 연차 카드
- 연차 사용 내역 테이블
- 연차 신청, 연차 취소 신청 버튼

### USER-ATTENDANCE-RECORDS. 내 출퇴근 기록

| 항목 | 내용 |
| --- | --- |
| URL | `/user/attendance/records` |
| 접근 권한 | 로그인 사용자 |
| 목적 | 출근, 퇴근, 휴게, 근무시간 기록을 조회한다. |
| 연결 API | `GET /api/attendance/records/me`, `POST /api/attendance/records/check-in`, `POST /api/attendance/records/check-out` |

주요 UI:

- 월 선택
- 출근/퇴근 버튼
- 일자별 출근/퇴근/휴게/총 근무시간 테이블
- 누락 기록 안내

### USER-APPROVAL-LIST. 내 전자결재 문서

| 항목 | 내용 |
| --- | --- |
| URL | `/user/approvals` |
| 접근 권한 | 로그인 사용자 |
| 목적 | 내가 작성했거나 결재/참조 대상인 전자결재 문서를 조회한다. |
| 연결 API | `GET /api/approvals` |

검색/필터:

- 문서 유형
- 결재 상태
- 작성일
- 키워드

버튼/액션:

| 액션 | 설명 |
| --- | --- |
| 새 문서 작성 | `/user/approvals/new`로 이동 |
| 상세 보기 | `/user/approvals/:id`로 이동 |
| 임시저장 문서 수정 | DRAFT 문서 작성 화면 이동 |

### USER-APPROVAL-CREATE. 전자결재 작성

| 항목 | 내용 |
| --- | --- |
| URL | `/user/approvals/new` |
| 접근 권한 | 로그인 사용자 |
| 목적 | 문서 유형별 전자결재 문서를 작성한다. |
| 연결 API | `POST /api/approvals`, `POST /api/approvals/{id}/submit` |

문서 유형:

- 연차 신청서, 연차 취소 신청서
- 재택근무, 외근, 출장, 시차출퇴근, 초과근무 신청서
- 지출결의서, 법인카드 사용내역서
- 재직증명서, 경력증명서 신청서

처리 규칙:

- 문서 유형 선택 시 입력 폼이 변경된다.
- 임시저장과 상신 버튼을 분리한다.
- 상신 전 예상 결재선을 표시한다.

### USER-APPROVAL-DETAIL. 전자결재 상세

| 항목 | 내용 |
| --- | --- |
| URL | `/user/approvals/:id` |
| 접근 권한 | 작성자, 결재자, 참조자, ROLE_ADMIN |
| 목적 | 결재 문서 본문, 결재선, 이력, 처리 결과를 확인한다. |
| 연결 API | `GET /api/approvals/{id}`, `PATCH /api/approvals/{id}/cancel` |

주요 UI:

- 문서 제목/유형/상태
- 결재선 타임라인
- 문서 유형별 상세 입력값
- 승인/반려 이력
- 취소 신청 또는 문서 취소 버튼

### MANAGER-EAPPROVAL-LIST / DETAIL. 팀 전자결재함

| 항목 | 내용 |
| --- | --- |
| URL | `/manager/e-approvals`, `/manager/e-approvals/:id` |
| 접근 권한 | ROLE_MANAGER, ROLE_ADMIN |
| 목적 | 소속 팀원의 전자결재 문서를 1차 승인 또는 반려한다. |
| 연결 API | `GET /api/manager/e-approvals`, `GET /api/manager/e-approvals/{id}`, `PATCH /api/manager/e-approvals/{id}/approve`, `PATCH /api/manager/e-approvals/{id}/reject` |

### HR-ATTENDANCE-DASHBOARD. 근태 대시보드

| 항목 | 내용 |
| --- | --- |
| URL | `/hr/attendance` |
| 접근 권한 | ROLE_HR, ROLE_ADMIN |
| 목적 | 부서/팀 단위 근태 현황과 누락 기록을 관리한다. |
| 연결 API | `GET /api/hr/attendance/summary`, `GET /api/hr/attendance/reports` |

주요 UI:

- 부서별 근무시간 요약
- 출퇴근 누락/지각/조퇴 현황
- 연차 사용 예정자
- 월간 근태 리포트

### HR-LEAVE-MANAGEMENT. 연차 관리

| 항목 | 내용 |
| --- | --- |
| URL | `/hr/leaves` |
| 접근 권한 | ROLE_HR, ROLE_ADMIN |
| 목적 | 사용자별 연차 현황과 조정 내역을 관리한다. |
| 연결 API | `GET /api/hr/leaves`, `POST /api/hr/leaves/{userId}/adjustments` |

### HR-CERTIFICATE-LIST. 증명서 발급 요청

| 항목 | 내용 |
| --- | --- |
| URL | `/hr/certificates` |
| 접근 권한 | ROLE_HR, ROLE_ADMIN |
| 목적 | 재직증명서/경력증명서 신청을 확인하고 처리한다. |
| 연결 API | `GET /api/hr/certificates`, `PATCH /api/hr/certificates/{id}/complete` |

### HR-EAPPROVAL-LIST. HR 전자결재함

| 항목 | 내용 |
| --- | --- |
| URL | `/hr/e-approvals` |
| 접근 권한 | ROLE_HR, ROLE_ADMIN |
| 목적 | 연차, 근태, 증명서 관련 최종 결재 문서를 처리한다. |
| 연결 API | `GET /api/hr/e-approvals`, `PATCH /api/hr/e-approvals/{id}/approve`, `PATCH /api/hr/e-approvals/{id}/reject` |

### FINANCE-EAPPROVAL-LIST / FINANCE-EXPENSE-LIST. 재무 결재/비용 처리

| 항목 | 내용 |
| --- | --- |
| URL | `/finance/e-approvals`, `/finance/expenses` |
| 접근 권한 | ROLE_FINANCE, ROLE_ADMIN |
| 목적 | 지출결의서와 법인카드 사용내역서를 검토하고 처리한다. |
| 연결 API | `GET /api/finance/e-approvals`, `GET /api/finance/expenses`, `PATCH /api/finance/expenses/{id}/process` |

### ADMIN-APPROVAL-LIST / FORM / POLICY. 전자결재/근태 관리자 화면

| 항목 | 내용 |
| --- | --- |
| URL | `/admin/approvals`, `/admin/approval-forms`, `/admin/attendance-policies` |
| 접근 권한 | ROLE_ADMIN |
| 목적 | 전체 결재 문서, 결재 양식, 근태 정책을 관리한다. |
| 연결 API | `GET /api/admin/approvals`, `GET /api/admin/approval-forms`, `POST /api/admin/attendance-policies` |

## 11. 프론트엔드 상태 관리 기준

### 11.1 Zustand 관리 대상

- 로그인 사용자 정보
- Access Token
- 사용자 권한
- 사이드바 열림/닫힘
- 전역 UI 상태

### 11.2 TanStack Query 관리 대상

- 내 요청 목록
- 요청 상세
- 팀장 승인 요청 목록/상세
- 운영 요청 목록/상세
- 관리자 요청 목록
- 예약 목록
- 예약 캘린더
- 자산 목록
- 내 자산 대여 현황
- 자원 목록
- 사용자 목록/상세
- 알림 목록
- 감사 이력
- 대시보드 통계

## 12. 공통 UI 정책

- 목록 화면은 로딩 상태, 빈 목록 상태, 에러 상태를 표시한다.
- 생성/수정 폼은 필수 입력값 에러를 필드 단위로 표시한다.
- 삭제, 취소, 반려, 완료 같은 주요 액션은 확인 모달을 사용한다.
- 권한이 없는 버튼은 숨기거나 비활성화한다.
- 서버에서 권한 에러가 반환되면 권한 없음 화면으로 이동한다.
- 페이지네이션은 목록 하단에 표시한다.
- 검색 조건 초기화 버튼을 제공한다.
