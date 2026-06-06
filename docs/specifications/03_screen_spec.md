# OfficeOps Hub 화면 정의서

## 1. 문서 목적

이 문서는 `OfficeOps Hub` 프론트엔드에서 구현할 화면 목록, 라우팅 경로, 접근 권한, 주요 UI 요소, 사용자 액션, 연결 API를 정의한다.

React, Vite, React Router 기반 화면 구현의 기준 문서로 사용한다.

## 2. 화면 목록

| ID | 구분 | 화면명 | URL | 접근 권한 | 우선순위 |
| --- | --- | --- | --- | --- | --- |
| AUTH-LOGIN | 공통 | 로그인 | `/login` | 비로그인 | 필수 |
| AUTH-SIGNUP | 공통 | 회원가입 | `/signup` | 비로그인 | 필수 |
| COMMON-FORBIDDEN | 공통 | 권한 없음 | `/forbidden` | 전체 | 필수 |
| COMMON-NOT-FOUND | 공통 | 404 Not Found | `*` | 전체 | 필수 |
| USER-HOME | 사용자 | 사용자 홈 | `/user` | ROLE_USER, ROLE_MANAGER, ROLE_OPERATOR, ROLE_ADMIN | 필수 |
| USER-REQUEST-LIST | 사용자 | 내 요청 목록 | `/user/requests` | ROLE_USER, ROLE_MANAGER, ROLE_OPERATOR, ROLE_ADMIN | 필수 |
| USER-REQUEST-CREATE | 사용자 | 요청 등록 | `/user/requests/new` | ROLE_USER, ROLE_MANAGER, ROLE_OPERATOR, ROLE_ADMIN | 필수 |
| USER-REQUEST-DETAIL | 사용자 | 요청 상세 | `/user/requests/:id` | 작성자, 관리자 | 필수 |
| USER-RESERVATION-LIST | 사용자 | 내 예약 목록 | `/user/reservations` | ROLE_USER, ROLE_MANAGER, ROLE_OPERATOR, ROLE_ADMIN | 필수 |
| USER-RESERVATION-CREATE | 사용자 | 회의실 예약 | `/user/reservations/new` | ROLE_USER, ROLE_MANAGER, ROLE_OPERATOR, ROLE_ADMIN | 필수 |
| USER-RESERVATION-CALENDAR | 사용자 | 예약 캘린더 | `/user/reservations/calendar` | ROLE_USER, ROLE_MANAGER, ROLE_OPERATOR, ROLE_ADMIN | 권장 |
| USER-ASSET-LIST | 사용자 | 자산 목록 | `/user/assets` | ROLE_USER, ROLE_MANAGER, ROLE_OPERATOR, ROLE_ADMIN | 필수 |
| USER-ASSET-LOAN-LIST | 사용자 | 내 자산 대여 현황 | `/user/asset-loans` | ROLE_USER, ROLE_MANAGER, ROLE_OPERATOR, ROLE_ADMIN | 권장 |
| USER-ME | 사용자 | 내 정보 | `/user/me` | ROLE_USER, ROLE_MANAGER, ROLE_OPERATOR, ROLE_ADMIN | 필수 |
| USER-PASSWORD | 사용자 | 비밀번호 변경 | `/user/me/password` | ROLE_USER, ROLE_MANAGER, ROLE_OPERATOR, ROLE_ADMIN | MVP 이후 |
| USER-NOTIFICATION-LIST | 사용자 | 알림 목록 | `/user/notifications` | ROLE_USER, ROLE_MANAGER, ROLE_OPERATOR, ROLE_ADMIN | 2순위 |
| MANAGER-APPROVAL-LIST | 팀장 | 팀 승인 요청함 | `/manager/approvals` | ROLE_MANAGER, ROLE_ADMIN | 필수 |
| MANAGER-APPROVAL-DETAIL | 팀장 | 팀 승인 요청 상세 | `/manager/approvals/:id` | ROLE_MANAGER, ROLE_ADMIN | 필수 |
| ADMIN-DASHBOARD | 관리자 | 관리자 대시보드 | `/admin/dashboard` | ROLE_ADMIN | 필수 |
| OPERATOR-REQUEST-LIST | 운영 | 운영 요청함 | `/operator/requests` | ROLE_OPERATOR, ROLE_ADMIN | 필수 |
| OPERATOR-REQUEST-DETAIL | 운영 | 운영 요청 상세 | `/operator/requests/:id` | ROLE_OPERATOR, ROLE_ADMIN | 필수 |
| ADMIN-REQUEST-LIST | 관리자 | 전체 요청 관리 | `/admin/requests` | ROLE_OPERATOR, ROLE_ADMIN | 필수 |
| ADMIN-REQUEST-DETAIL | 관리자 | 요청 상세 관리 | `/admin/requests/:id` | ROLE_OPERATOR, ROLE_ADMIN | 필수 |
| ADMIN-ASSET-LIST | 관리자 | 자산 관리 | `/admin/assets` | ROLE_OPERATOR, ROLE_ADMIN | 필수 |
| ADMIN-RESERVATION-LIST | 관리자 | 예약 관리 | `/admin/reservations` | ROLE_OPERATOR, ROLE_ADMIN | 필수 |
| ADMIN-RESERVATION-CALENDAR | 관리자 | 예약 캘린더 | `/admin/reservations/calendar` | ROLE_OPERATOR, ROLE_ADMIN | 권장 |
| ADMIN-RESOURCE-LIST | 관리자 | 회의실/자원 관리 | `/admin/resources` | ROLE_ADMIN | 필수 |
| ADMIN-USER-LIST | 관리자 | 사용자 관리 | `/admin/users` | ROLE_ADMIN | 선택 |
| ADMIN-USER-DETAIL | 관리자 | 사용자 상세/권한 변경 | `/admin/users/:id` | ROLE_ADMIN | 권장 |
| ADMIN-AUDIT-LOG-LIST | 관리자 | 감사 이력 | `/admin/audit-logs` | ROLE_ADMIN | 필수 |

## 3. 공통 레이아웃

### 3.1 인증 전 레이아웃

대상 화면:

- 로그인
- 회원가입

구성 요소:

- 서비스명
- 입력 폼
- 제출 버튼
- 로그인/회원가입 이동 링크
- 에러 메시지 영역

### 3.2 사용자 레이아웃

대상 화면:

- `/user` 하위 화면

구성 요소:

- 상단바
- 사이드바
- 사용자 이름/부서 표시
- 로그아웃 버튼
- 메뉴: 사용자 홈, 내 요청, 회의실 예약, 예약 캘린더, 자산 목록, 내 자산 대여 현황, 내 정보

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
- 팀장 메뉴: 팀 승인 요청함
- 운영 담당자 메뉴: 운영 요청함, 자산 관리, 예약 관리
- 관리자 메뉴: 대시보드, 요청 관리, 자산 관리, 예약 관리, 회의실/자원 관리, 사용자 관리, 감사 이력

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
| 회원가입 이동 | `/signup`으로 이동 |

처리 규칙:

- 로그인 성공 시 사용자 권한에 따라 이동한다.
- 일반 직원은 `/user`로 이동한다.
- 관리자는 `/admin/dashboard`로 이동한다.
- 로그인 실패 시 에러 메시지를 표시한다.

### AUTH-SIGNUP. 회원가입

| 항목 | 내용 |
| --- | --- |
| URL | `/signup` |
| 접근 권한 | 비로그인 사용자 |
| 목적 | 일반 직원 계정을 생성한다. |
| 연결 API | `POST /api/auth/signup` |

입력 항목:

| 항목 | 타입 | 필수 |
| --- | --- | --- |
| 이메일 | text/email | Y |
| 비밀번호 | password | Y |
| 비밀번호 확인 | password | Y |
| 이름 | text | Y |
| 부서 | text/select | Y |

버튼/액션:

| 액션 | 설명 |
| --- | --- |
| 회원가입 | 회원가입 API 호출 |
| 로그인 이동 | `/login`으로 이동 |

처리 규칙:

- 비밀번호와 비밀번호 확인이 일치해야 한다.
- 회원가입 성공 시 로그인 화면으로 이동한다.
- 이메일 중복 또는 입력값 오류는 화면에 표시한다.

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
| 상세 보기 | `/admin/users/:id`로 이동 |

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
| 권한 변경 | ROLE_USER, ROLE_MANAGER, ROLE_OPERATOR, ROLE_ADMIN으로 변경 |
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

## 10. 프론트엔드 상태 관리 기준

### 10.1 Zustand 관리 대상

- 로그인 사용자 정보
- Access Token
- 사용자 권한
- 사이드바 열림/닫힘
- 전역 UI 상태

### 10.2 TanStack Query 관리 대상

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

## 11. 공통 UI 정책

- 목록 화면은 로딩 상태, 빈 목록 상태, 에러 상태를 표시한다.
- 생성/수정 폼은 필수 입력값 에러를 필드 단위로 표시한다.
- 삭제, 취소, 반려, 완료 같은 주요 액션은 확인 모달을 사용한다.
- 권한이 없는 버튼은 숨기거나 비활성화한다.
- 서버에서 권한 에러가 반환되면 권한 없음 화면으로 이동한다.
- 페이지네이션은 목록 하단에 표시한다.
- 검색 조건 초기화 버튼을 제공한다.
