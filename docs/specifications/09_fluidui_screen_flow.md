# OfficeOps Hub Fluid UI 화면흐름도

## 1. 문서 목적

이 문서는 `OfficeOps Hub`의 화면 흐름을 Fluid UI 또는 유사한 와이어프레임 도구에서 바로 구성할 수 있도록 정리한 화면흐름도 문서다.

기존 문서 중 `03_screen_spec.md`, `06_status_permission_policy.md`를 기준으로 화면 노드, 이동 경로, 권한별 분기, 주요 액션 흐름을 정의한다.

## 2. Fluid UI 작성 기준

### 2.1 화면 그룹

Fluid UI에서는 다음 그룹 단위로 페이지를 묶는다.

| 그룹 | 색상 제안 | 포함 화면 |
| --- | --- | --- |
| Auth | 회색 | 로그인, 회원가입 |
| Common | 연회색 | 권한 없음, 404 |
| User | 파란색 | 사용자 홈, 내 요청, 예약, 자산, 내 정보 |
| Manager | 초록색 | 팀 승인 요청함, 팀 승인 상세 |
| Operator | 보라색 | 운영 요청함, 운영 요청 상세 |
| HR | 청록색 | 근태 대시보드, 연차 관리, 증명서, HR 전자결재함 |
| Finance | 주황색 | 재무 전자결재함, 지출/법인카드 처리 |
| Admin | 빨간색 | 관리자 대시보드, 전체 관리 화면 |

### 2.2 연결선 표기

| 표기 | 의미 |
| --- | --- |
| 실선 | 일반 화면 이동 |
| 점선 | 권한 또는 상태 조건부 이동 |
| 굵은 선 | 핵심 업무 흐름 |
| 되돌아가기 | 목록 또는 이전 화면 복귀 |

### 2.3 권한 분기 기준

로그인 성공 후 기본 이동 경로는 다음과 같다.

| 역할 | 기본 진입 화면 |
| --- | --- |
| ROLE_USER | `/user` |
| ROLE_MANAGER | `/user` |
| ROLE_OPERATOR | `/operator/requests` |
| ROLE_HR | `/hr/attendance` |
| ROLE_FINANCE | `/finance/e-approvals` |
| ROLE_ADMIN | `/admin/dashboard` |

`ROLE_MANAGER`도 일반 사용자 업무를 수행할 수 있으므로 `/user` 화면을 함께 사용할 수 있다.
`ROLE_ADMIN`은 전체 관리 화면을 기본 진입점으로 사용하되, 사용자 화면과 운영 화면 접근도 가능하다.

## 3. 전체 화면 구조

```mermaid
flowchart TD
    START([서비스 접속])

    LOGIN[AUTH-LOGIN<br/>/login]
    SIGNUP[AUTH-SIGNUP<br/>/signup]
    FORBIDDEN[COMMON-FORBIDDEN<br/>/forbidden]
    NOT_FOUND[COMMON-NOT-FOUND<br/>*]

    USER_HOME[USER-HOME<br/>/user]
    MANAGER_LIST[MANAGER-APPROVAL-LIST<br/>/manager/approvals]
    OPERATOR_LIST[OPERATOR-REQUEST-LIST<br/>/operator/requests]
    HR_HOME[HR-ATTENDANCE-DASHBOARD<br/>/hr/attendance]
    FINANCE_HOME[FINANCE-EAPPROVAL-LIST<br/>/finance/e-approvals]
    ADMIN_DASHBOARD[ADMIN-DASHBOARD<br/>/admin/dashboard]

    START --> LOGIN
    LOGIN --> SIGNUP
    SIGNUP --> LOGIN

    LOGIN -->|ROLE_USER| USER_HOME
    LOGIN -->|ROLE_MANAGER| USER_HOME
    LOGIN -->|ROLE_OPERATOR| OPERATOR_LIST
    LOGIN -->|ROLE_HR| HR_HOME
    LOGIN -->|ROLE_FINANCE| FINANCE_HOME
    LOGIN -->|ROLE_ADMIN| ADMIN_DASHBOARD

    USER_HOME -.권한 없음.-> FORBIDDEN
    MANAGER_LIST -.권한 없음.-> FORBIDDEN
    OPERATOR_LIST -.권한 없음.-> FORBIDDEN
    HR_HOME -.권한 없음.-> FORBIDDEN
    FINANCE_HOME -.권한 없음.-> FORBIDDEN
    ADMIN_DASHBOARD -.권한 없음.-> FORBIDDEN

    START -.정의되지 않은 URL.-> NOT_FOUND
```

## 4. 인증 및 공통 흐름

### 4.1 인증 전 흐름

```mermaid
flowchart LR
    LOGIN[로그인<br/>/login]
    SIGNUP[회원가입<br/>/signup]
    USER_HOME[사용자 홈<br/>/user]
    OPERATOR_HOME[운영 요청함<br/>/operator/requests]
    ADMIN_HOME[관리자 대시보드<br/>/admin/dashboard]

    LOGIN -->|회원가입 이동| SIGNUP
    SIGNUP -->|가입 성공| LOGIN
    SIGNUP -->|로그인 이동| LOGIN

    LOGIN -->|일반 직원/팀장 로그인 성공| USER_HOME
    LOGIN -->|운영 담당자 로그인 성공| OPERATOR_HOME
    LOGIN -->|관리자 로그인 성공| ADMIN_HOME
```

### 4.2 라우팅 보호 흐름

| 상황 | 이동 |
| --- | --- |
| 비로그인 사용자가 보호 화면 접근 | `/login` |
| 로그인 사용자가 `/login` 접근 | 권한별 기본 홈 |
| 권한 없는 사용자가 `/manager/*`, `/operator/*`, `/admin/*` 접근 | `/forbidden` |
| 정의되지 않은 URL 접근 | 404 화면 |

```mermaid
flowchart TD
    PROTECTED[보호된 URL 접근]
    IS_LOGIN{로그인 상태?}
    HAS_ROLE{접근 권한 있음?}
    LOGIN[/login]
    TARGET[요청한 화면]
    FORBIDDEN[/forbidden]
    NOT_FOUND[404 Not Found]

    PROTECTED --> IS_LOGIN
    IS_LOGIN -->|아니오| LOGIN
    IS_LOGIN -->|예| HAS_ROLE
    HAS_ROLE -->|예| TARGET
    HAS_ROLE -->|아니오| FORBIDDEN
    PROTECTED -.존재하지 않는 경로.-> NOT_FOUND
```

## 5. 사용자 화면 흐름

### 5.1 사용자 홈 중심 흐름

```mermaid
flowchart TD
    USER_HOME[USER-HOME<br/>/user]
    REQUEST_LIST[USER-REQUEST-LIST<br/>/user/requests]
    REQUEST_CREATE[USER-REQUEST-CREATE<br/>/user/requests/new]
    REQUEST_DETAIL[USER-REQUEST-DETAIL<br/>/user/requests/:id]
    RESERVATION_LIST[USER-RESERVATION-LIST<br/>/user/reservations]
    RESERVATION_CREATE[USER-RESERVATION-CREATE<br/>/user/reservations/new]
    RESERVATION_CALENDAR[USER-RESERVATION-CALENDAR<br/>/user/reservations/calendar]
    ASSET_LIST[USER-ASSET-LIST<br/>/user/assets]
    ASSET_LOAN_LIST[USER-ASSET-LOAN-LIST<br/>/user/asset-loans]
    ME[USER-ME<br/>/user/me]
    PASSWORD[USER-PASSWORD<br/>/user/me/password]
    NOTIFICATIONS[USER-NOTIFICATION-LIST<br/>/user/notifications]

    USER_HOME --> REQUEST_LIST
    USER_HOME --> REQUEST_CREATE
    USER_HOME --> RESERVATION_LIST
    USER_HOME --> RESERVATION_CREATE
    USER_HOME --> ASSET_LIST
    USER_HOME --> ME

    REQUEST_LIST -->|요청 등록| REQUEST_CREATE
    REQUEST_LIST -->|상세 보기| REQUEST_DETAIL
    REQUEST_CREATE -->|등록 성공| REQUEST_DETAIL
    REQUEST_CREATE -->|취소| REQUEST_LIST
    REQUEST_DETAIL -->|목록| REQUEST_LIST
    REQUEST_DETAIL -->|수정/취소 후 재조회| REQUEST_DETAIL

    RESERVATION_LIST -->|예약 등록| RESERVATION_CREATE
    RESERVATION_LIST -->|예약 취소 후 재조회| RESERVATION_LIST
    RESERVATION_CREATE -->|등록 성공| RESERVATION_LIST
    RESERVATION_CREATE -->|취소| RESERVATION_LIST
    RESERVATION_LIST --> RESERVATION_CALENDAR
    RESERVATION_CALENDAR -->|예약 등록| RESERVATION_CREATE

    ASSET_LIST --> ASSET_LOAN_LIST
    ME --> PASSWORD
    USER_HOME --> NOTIFICATIONS
```

### 5.2 요청 등록 및 처리 결과 확인 흐름

```mermaid
flowchart TD
    REQUEST_CREATE[요청 등록]
    TYPE_SELECT{요청 유형}
    ASSET_FIELDS[자산 요청 추가 입력]
    VISITOR_FIELDS[방문 신청 추가 입력]
    FACILITY_FIELDS[시설/비품 요청 추가 입력]
    SUBMIT[등록]
    MANAGER_PENDING[PENDING_MANAGER_APPROVAL]
    DETAIL[요청 상세]
    EDIT{수정 가능?}
    CANCEL{취소 가능?}

    REQUEST_CREATE --> TYPE_SELECT
    TYPE_SELECT -->|ASSET_REQUEST| ASSET_FIELDS
    TYPE_SELECT -->|VISITOR_REQUEST| VISITOR_FIELDS
    TYPE_SELECT -->|FACILITY_REQUEST| FACILITY_FIELDS
    ASSET_FIELDS --> SUBMIT
    VISITOR_FIELDS --> SUBMIT
    FACILITY_FIELDS --> SUBMIT
    SUBMIT --> MANAGER_PENDING
    MANAGER_PENDING --> DETAIL

    DETAIL --> EDIT
    EDIT -->|본인 요청 + PENDING_MANAGER_APPROVAL| DETAIL
    DETAIL --> CANCEL
    CANCEL -->|최종 상태 아님| DETAIL
```

## 6. 핵심 요청 승인 흐름

이 흐름은 OfficeOps Hub의 MVP 핵심 업무 흐름이다.

```mermaid
flowchart TD
    USER_CREATE[사용자 요청 등록]
    MANAGER_LIST[팀 승인 요청함]
    MANAGER_DETAIL[팀 승인 요청 상세]
    MANAGER_DECISION{팀장 결정}
    OPERATOR_LIST[운영 요청함]
    OPERATOR_DETAIL[운영 요청 상세]
    OPERATOR_DECISION{운영 담당자 결정}
    ASSIGN[담당자 지정<br/>처리 예정일/마감일 설정]
    APPROVED[APPROVED]
    IN_PROGRESS[IN_PROGRESS]
    COMPLETED[COMPLETED]
    REJECTED[REJECTED]
    CANCELLED[CANCELLED]

    USER_CREATE -->|요청 생성| MANAGER_LIST
    MANAGER_LIST -->|상세 보기| MANAGER_DETAIL
    MANAGER_DETAIL --> MANAGER_DECISION
    MANAGER_DECISION -->|1차 승인| OPERATOR_LIST
    MANAGER_DECISION -->|반려| REJECTED

    OPERATOR_LIST -->|상세 보기| OPERATOR_DETAIL
    OPERATOR_DETAIL --> OPERATOR_DECISION
    OPERATOR_DECISION -->|최종 승인| ASSIGN
    OPERATOR_DECISION -->|최종 반려| REJECTED
    ASSIGN --> APPROVED
    APPROVED -->|처리 시작| IN_PROGRESS
    IN_PROGRESS -->|완료 처리| COMPLETED

    USER_CREATE -.요청자 취소.-> CANCELLED
    MANAGER_DETAIL -.요청자/관리자 취소.-> CANCELLED
    OPERATOR_DETAIL -.운영/관리자 취소.-> CANCELLED
```

### 6.1 요청 상태별 화면 액션

| 상태 | 사용자 상세 | 팀장 상세 | 운영 상세 | 관리자 상세 |
| --- | --- | --- | --- | --- |
| PENDING_MANAGER_APPROVAL | 수정, 취소 | 1차 승인, 반려 | 조회 제한 | 조회/관리 |
| PENDING_OPERATOR_APPROVAL | 조회 | 조회 | 최종 승인, 최종 반려 | 최종 승인, 최종 반려 |
| APPROVED | 조회 | 조회 | 담당자 변경, 마감일 변경, 처리 중 | 담당자 변경, 마감일 변경, 처리 중 |
| IN_PROGRESS | 조회 | 조회 | 담당자 변경, 마감일 변경, 완료 | 담당자 변경, 마감일 변경, 완료 |
| COMPLETED | 조회 | 조회 | 조회 | 조회 |
| REJECTED | 조회 | 조회 | 조회 | 조회 |
| CANCELLED | 조회 | 조회 | 조회 | 조회 |

## 7. 팀장 화면 흐름

```mermaid
flowchart TD
    MANAGER_LIST[MANAGER-APPROVAL-LIST<br/>/manager/approvals]
    FILTER[검색/필터]
    DETAIL[MANAGER-APPROVAL-DETAIL<br/>/manager/approvals/:id]
    APPROVE[1차 승인]
    REJECT_MODAL[반려 사유 입력 모달]
    REJECT[반려]
    BACK[목록으로]

    MANAGER_LIST --> FILTER
    FILTER --> MANAGER_LIST
    MANAGER_LIST -->|상세 보기| DETAIL
    DETAIL -->|1차 승인| APPROVE
    APPROVE -->|성공| MANAGER_LIST
    DETAIL -->|반려| REJECT_MODAL
    REJECT_MODAL -->|확인| REJECT
    REJECT -->|성공| MANAGER_LIST
    DETAIL -->|목록| BACK
    BACK --> MANAGER_LIST
```

## 8. 운영 담당자 화면 흐름

```mermaid
flowchart TD
    OPERATOR_LIST[OPERATOR-REQUEST-LIST<br/>/operator/requests]
    FILTER[검색/필터<br/>상태/담당자/마감일/지연/미배정]
    DETAIL[OPERATOR-REQUEST-DETAIL<br/>/operator/requests/:id]
    APPROVE_MODAL[최종 승인 모달<br/>담당자/처리 예정일/마감일]
    REJECT_MODAL[최종 반려 사유 모달]
    ASSIGN_MODAL[담당자 변경 모달]
    DUE_MODAL[마감일 변경 모달]
    IN_PROGRESS[처리 중 변경]
    COMPLETE[완료 처리]

    OPERATOR_LIST --> FILTER
    FILTER --> OPERATOR_LIST
    OPERATOR_LIST -->|상세 보기| DETAIL
    DETAIL -->|최종 승인| APPROVE_MODAL
    APPROVE_MODAL -->|승인 성공| DETAIL
    DETAIL -->|최종 반려| REJECT_MODAL
    REJECT_MODAL -->|반려 성공| DETAIL
    DETAIL -->|담당자 변경| ASSIGN_MODAL
    ASSIGN_MODAL -->|저장| DETAIL
    DETAIL -->|마감일 변경| DUE_MODAL
    DUE_MODAL -->|저장| DETAIL
    DETAIL -->|처리 중| IN_PROGRESS
    IN_PROGRESS --> DETAIL
    DETAIL -->|완료| COMPLETE
    COMPLETE --> DETAIL
```

## 9. 예약 화면 흐름

```mermaid
flowchart TD
    USER_HOME[사용자 홈]
    RSV_LIST[내 예약 목록<br/>/user/reservations]
    RSV_CREATE[회의실 예약<br/>/user/reservations/new]
    RSV_CALENDAR[예약 캘린더<br/>/user/reservations/calendar]
    DUP_CHECK{예약 중복?}
    RESERVED[예약 완료]
    ERROR[중복/입력 오류 표시]
    CANCEL[예약 취소]

    ADMIN_RSV_LIST[예약 관리<br/>/admin/reservations]
    ADMIN_RSV_CALENDAR[관리자 예약 캘린더<br/>/admin/reservations/calendar]

    USER_HOME --> RSV_LIST
    RSV_LIST -->|예약 등록| RSV_CREATE
    RSV_LIST --> RSV_CALENDAR
    RSV_CALENDAR -->|시간대 선택| RSV_CREATE
    RSV_CREATE --> DUP_CHECK
    DUP_CHECK -->|없음| RESERVED
    DUP_CHECK -->|있음| ERROR
    RESERVED --> RSV_LIST
    RSV_LIST -->|예약 취소| CANCEL
    CANCEL --> RSV_LIST

    ADMIN_RSV_LIST --> ADMIN_RSV_CALENDAR
    ADMIN_RSV_CALENDAR -->|예약 취소| ADMIN_RSV_CALENDAR
    ADMIN_RSV_LIST -->|예약 취소| ADMIN_RSV_LIST
```

## 10. 자산 화면 흐름

```mermaid
flowchart TD
    USER_ASSET_LIST[자산 목록<br/>/user/assets]
    USER_LOAN_LIST[내 자산 대여 현황<br/>/user/asset-loans]

    ADMIN_ASSET_LIST[자산 관리<br/>/admin/assets]
    CREATE_MODAL[자산 등록 모달]
    EDIT_MODAL[자산 수정 모달]
    STATUS_MENU[상태 변경 메뉴]
    LOAN_MODAL[대여 처리 모달]
    RETURN_CONFIRM[반납 확인]

    USER_ASSET_LIST --> USER_LOAN_LIST

    ADMIN_ASSET_LIST -->|자산 등록| CREATE_MODAL
    CREATE_MODAL -->|저장| ADMIN_ASSET_LIST
    ADMIN_ASSET_LIST -->|자산 수정| EDIT_MODAL
    EDIT_MODAL -->|저장| ADMIN_ASSET_LIST
    ADMIN_ASSET_LIST -->|상태 변경| STATUS_MENU
    STATUS_MENU -->|저장| ADMIN_ASSET_LIST
    ADMIN_ASSET_LIST -->|대여 처리| LOAN_MODAL
    LOAN_MODAL -->|저장| ADMIN_ASSET_LIST
    ADMIN_ASSET_LIST -->|반납 처리| RETURN_CONFIRM
    RETURN_CONFIRM -->|확인| ADMIN_ASSET_LIST
```

## 11. 관리자 화면 흐름

### 11.1 관리자 대시보드 중심 흐름

```mermaid
flowchart TD
    DASHBOARD[ADMIN-DASHBOARD<br/>/admin/dashboard]
    REQUESTS[ADMIN-REQUEST-LIST<br/>/admin/requests]
    REQUEST_DETAIL[ADMIN-REQUEST-DETAIL<br/>/admin/requests/:id]
    ASSETS[ADMIN-ASSET-LIST<br/>/admin/assets]
    RESERVATIONS[ADMIN-RESERVATION-LIST<br/>/admin/reservations]
    RESERVATION_CALENDAR[ADMIN-RESERVATION-CALENDAR<br/>/admin/reservations/calendar]
    RESOURCES[ADMIN-RESOURCE-LIST<br/>/admin/resources]
    USERS[ADMIN-USER-LIST<br/>/admin/users]
    USER_DETAIL[ADMIN-USER-DETAIL<br/>/admin/users/:id]
    AUDIT[ADMIN-AUDIT-LOG-LIST<br/>/admin/audit-logs]

    DASHBOARD --> REQUESTS
    DASHBOARD --> ASSETS
    DASHBOARD --> RESERVATIONS
    DASHBOARD --> RESOURCES
    DASHBOARD --> USERS
    DASHBOARD --> AUDIT

    REQUESTS -->|상세 보기| REQUEST_DETAIL
    REQUEST_DETAIL -->|목록| REQUESTS

    RESERVATIONS --> RESERVATION_CALENDAR
    RESERVATION_CALENDAR --> RESERVATIONS

    USERS -->|상세 보기| USER_DETAIL
    USER_DETAIL -->|목록| USERS
```

### 11.2 회의실/자원 관리 흐름

```mermaid
flowchart TD
    RESOURCE_LIST[회의실/자원 관리<br/>/admin/resources]
    CREATE[자원 등록 모달]
    EDIT[자원 수정 모달]
    ENABLE[사용 가능 처리]
    DISABLE[사용 중지 처리]
    AUDIT[감사 이력 저장]

    RESOURCE_LIST -->|자원 등록| CREATE
    CREATE -->|저장| RESOURCE_LIST
    RESOURCE_LIST -->|자원 수정| EDIT
    EDIT -->|저장| RESOURCE_LIST
    RESOURCE_LIST -->|사용 가능| ENABLE
    RESOURCE_LIST -->|사용 중지| DISABLE
    ENABLE --> AUDIT
    DISABLE --> AUDIT
    AUDIT --> RESOURCE_LIST
```

### 11.3 사용자 관리 흐름

```mermaid
flowchart TD
    USER_LIST[사용자 관리<br/>/admin/users]
    USER_DETAIL[사용자 상세<br/>/admin/users/:id]
    ROLE_MODAL[권한 변경 모달]
    DEACTIVATE[비활성화]
    ACTIVATE[활성화]
    AUDIT[감사 이력 저장]

    USER_LIST -->|상세 보기| USER_DETAIL
    USER_DETAIL -->|권한 변경| ROLE_MODAL
    ROLE_MODAL -->|저장| USER_DETAIL
    USER_DETAIL -->|비활성화| DEACTIVATE
    USER_DETAIL -->|활성화| ACTIVATE
    DEACTIVATE --> AUDIT
    ACTIVATE --> AUDIT
    AUDIT --> USER_DETAIL
    USER_DETAIL -->|목록| USER_LIST
```

## 12. 근태/전자결재 화면 흐름

### 12.1 사용자 근태/전자결재 흐름

```mermaid
flowchart TD
    USER_HOME[USER-HOME<br/>/user]
    ATT_HOME[USER-ATTENDANCE-HOME<br/>/user/attendance]
    LEAVE_BALANCE[USER-LEAVE-BALANCE<br/>/user/attendance/leaves]
    ATT_RECORDS[USER-ATTENDANCE-RECORDS<br/>/user/attendance/records]
    APPROVAL_LIST[USER-APPROVAL-LIST<br/>/user/approvals]
    APPROVAL_CREATE[USER-APPROVAL-CREATE<br/>/user/approvals/new]
    APPROVAL_DETAIL[USER-APPROVAL-DETAIL<br/>/user/approvals/:id]

    USER_HOME --> ATT_HOME
    ATT_HOME --> LEAVE_BALANCE
    ATT_HOME --> ATT_RECORDS
    ATT_HOME --> APPROVAL_CREATE
    USER_HOME --> APPROVAL_LIST
    APPROVAL_LIST --> APPROVAL_CREATE
    APPROVAL_LIST --> APPROVAL_DETAIL
    APPROVAL_CREATE -->|임시저장| APPROVAL_DETAIL
    APPROVAL_CREATE -->|상신| APPROVAL_DETAIL
```

### 12.2 전자결재 승인 흐름

```mermaid
flowchart TD
    DRAFT[문서 작성/임시저장]
    SUBMIT[상신]
    MANAGER_BOX[팀 전자결재함<br/>/manager/e-approvals]
    MANAGER_DECISION{팀장 결정}
    FINAL_BRANCH{문서 유형}
    HR_BOX[HR 전자결재함<br/>/hr/e-approvals]
    FINANCE_BOX[재무 전자결재함<br/>/finance/e-approvals]
    ADMIN_BOX[관리자 전자결재 관리<br/>/admin/approvals]
    APPROVED[APPROVED]
    COMPLETED[COMPLETED]
    REJECTED[REJECTED]

    DRAFT --> SUBMIT
    SUBMIT --> MANAGER_BOX
    MANAGER_BOX --> MANAGER_DECISION
    MANAGER_DECISION -->|승인| FINAL_BRANCH
    MANAGER_DECISION -->|반려| REJECTED
    FINAL_BRANCH -->|연차/근태/증명서| HR_BOX
    FINAL_BRANCH -->|지출/법인카드| FINANCE_BOX
    FINAL_BRANCH -->|관리자 처리| ADMIN_BOX
    HR_BOX --> APPROVED
    FINANCE_BOX --> APPROVED
    ADMIN_BOX --> APPROVED
    APPROVED --> COMPLETED
```

### 12.3 HR/Finance 운영 흐름

```mermaid
flowchart TD
    HR_DASH[근태 대시보드<br/>/hr/attendance]
    HR_LEAVES[연차 관리<br/>/hr/leaves]
    HR_CERT[증명서 발급 요청<br/>/hr/certificates]
    HR_APPROVALS[HR 전자결재함<br/>/hr/e-approvals]
    FIN_APPROVALS[재무 전자결재함<br/>/finance/e-approvals]
    FIN_EXPENSES[지출/법인카드 처리<br/>/finance/expenses]

    HR_DASH --> HR_LEAVES
    HR_DASH --> HR_CERT
    HR_DASH --> HR_APPROVALS
    HR_APPROVALS -->|연차 승인| HR_LEAVES
    HR_APPROVALS -->|증명서 처리| HR_CERT
    FIN_APPROVALS --> FIN_EXPENSES
```

## 13. Fluid UI 화면 노드 목록

### 13.1 MVP 필수 화면

| 화면 ID | 화면명 | URL | 그룹 | 연결 대상 |
| --- | --- | --- | --- | --- |
| AUTH-LOGIN | 로그인 | `/login` | Auth | 회원가입, 권한별 홈 |
| AUTH-SIGNUP | 회원가입 | `/signup` | Auth | 로그인 |
| COMMON-FORBIDDEN | 권한 없음 | `/forbidden` | Common | 권한별 홈, 이전 화면 |
| COMMON-NOT-FOUND | 404 Not Found | `*` | Common | 권한별 홈, 로그인 |
| USER-HOME | 사용자 홈 | `/user` | User | 내 요청, 요청 등록, 내 예약, 예약 등록, 자산 목록, 내 근태, 전자결재, 내 정보 |
| USER-REQUEST-LIST | 내 요청 목록 | `/user/requests` | User | 요청 등록, 요청 상세 |
| USER-REQUEST-CREATE | 요청 등록 | `/user/requests/new` | User | 요청 상세, 내 요청 목록 |
| USER-REQUEST-DETAIL | 요청 상세 | `/user/requests/:id` | User | 내 요청 목록 |
| USER-RESERVATION-LIST | 내 예약 목록 | `/user/reservations` | User | 회의실 예약, 예약 캘린더 |
| USER-RESERVATION-CREATE | 회의실 예약 | `/user/reservations/new` | User | 내 예약 목록 |
| USER-ASSET-LIST | 자산 목록 | `/user/assets` | User | 내 자산 대여 현황 |
| USER-ME | 내 정보 | `/user/me` | User | 비밀번호 변경 |
| USER-ATTENDANCE-HOME | 내 근태 홈 | `/user/attendance` | User | 내 연차 현황, 출퇴근 기록, 전자결재 작성 |
| USER-LEAVE-BALANCE | 내 연차 현황 | `/user/attendance/leaves` | User | 전자결재 작성 |
| USER-ATTENDANCE-RECORDS | 내 출퇴근 기록 | `/user/attendance/records` | User | 내 근태 홈 |
| USER-APPROVAL-LIST | 내 전자결재 문서 | `/user/approvals` | User | 전자결재 작성, 전자결재 상세 |
| USER-APPROVAL-CREATE | 전자결재 작성 | `/user/approvals/new` | User | 전자결재 상세 |
| USER-APPROVAL-DETAIL | 전자결재 상세 | `/user/approvals/:id` | User | 내 전자결재 문서 |
| MANAGER-APPROVAL-LIST | 팀 승인 요청함 | `/manager/approvals` | Manager | 팀 승인 요청 상세 |
| MANAGER-APPROVAL-DETAIL | 팀 승인 요청 상세 | `/manager/approvals/:id` | Manager | 팀 승인 요청함 |
| MANAGER-EAPPROVAL-LIST | 팀 전자결재함 | `/manager/e-approvals` | Manager | 팀 전자결재 상세 |
| MANAGER-EAPPROVAL-DETAIL | 팀 전자결재 상세 | `/manager/e-approvals/:id` | Manager | HR/재무 전자결재함 |
| OPERATOR-REQUEST-LIST | 운영 요청함 | `/operator/requests` | Operator | 운영 요청 상세 |
| OPERATOR-REQUEST-DETAIL | 운영 요청 상세 | `/operator/requests/:id` | Operator | 운영 요청함 |
| HR-ATTENDANCE-DASHBOARD | 근태 대시보드 | `/hr/attendance` | HR | 연차 관리, 증명서, HR 전자결재함 |
| HR-LEAVE-MANAGEMENT | 연차 관리 | `/hr/leaves` | HR | 근태 대시보드 |
| HR-CERTIFICATE-LIST | 증명서 발급 요청 | `/hr/certificates` | HR | HR 전자결재함 |
| HR-EAPPROVAL-LIST | HR 전자결재함 | `/hr/e-approvals` | HR | 연차 관리, 증명서 |
| FINANCE-EAPPROVAL-LIST | 재무 전자결재함 | `/finance/e-approvals` | Finance | 지출/법인카드 처리 |
| FINANCE-EXPENSE-LIST | 지출/법인카드 처리 | `/finance/expenses` | Finance | 재무 전자결재함 |
| ADMIN-DASHBOARD | 관리자 대시보드 | `/admin/dashboard` | Admin | 관리자 주요 목록 |
| ADMIN-REQUEST-LIST | 전체 요청 관리 | `/admin/requests` | Admin | 요청 상세 관리 |
| ADMIN-REQUEST-DETAIL | 요청 상세 관리 | `/admin/requests/:id` | Admin | 전체 요청 관리 |
| ADMIN-ASSET-LIST | 자산 관리 | `/admin/assets` | Admin | 등록/수정/상태/대여/반납 모달 |
| ADMIN-RESERVATION-LIST | 예약 관리 | `/admin/reservations` | Admin | 관리자 예약 캘린더 |
| ADMIN-RESOURCE-LIST | 회의실/자원 관리 | `/admin/resources` | Admin | 등록/수정/상태 변경 모달 |
| ADMIN-APPROVAL-LIST | 전체 전자결재 관리 | `/admin/approvals` | Admin | 전자결재 양식 관리 |
| ADMIN-APPROVAL-FORM-LIST | 전자결재 양식 관리 | `/admin/approval-forms` | Admin | 전체 전자결재 관리 |
| ADMIN-ATTENDANCE-POLICY | 근태 정책 관리 | `/admin/attendance-policies` | Admin | 대시보드 |
| ADMIN-AUDIT-LOG-LIST | 감사 이력 | `/admin/audit-logs` | Admin | 대시보드 |

### 13.2 2순위 및 MVP 이후 화면

| 화면 ID | 화면명 | URL | 우선순위 |
| --- | --- | --- | --- |
| USER-RESERVATION-CALENDAR | 예약 캘린더 | `/user/reservations/calendar` | 권장 |
| USER-ASSET-LOAN-LIST | 내 자산 대여 현황 | `/user/asset-loans` | 권장 |
| USER-NOTIFICATION-LIST | 알림 목록 | `/user/notifications` | 2순위 |
| USER-PASSWORD | 비밀번호 변경 | `/user/me/password` | MVP 이후 |
| ADMIN-RESERVATION-CALENDAR | 관리자 예약 캘린더 | `/admin/reservations/calendar` | 권장 |
| ADMIN-USER-LIST | 사용자 관리 | `/admin/users` | 선택 |
| ADMIN-USER-DETAIL | 사용자 상세/권한 변경 | `/admin/users/:id` | 권장 |

## 14. Fluid UI 제작 순서 제안

1. `AUTH-LOGIN`, `AUTH-SIGNUP`, `COMMON-FORBIDDEN`, `COMMON-NOT-FOUND`를 먼저 배치한다.
2. 중앙에 `USER-HOME`을 놓고 사용자 업무 화면을 좌측 또는 하단으로 연결한다.
3. `MANAGER-APPROVAL-LIST`와 `OPERATOR-REQUEST-LIST`를 요청 승인 흐름의 오른쪽에 배치한다.
4. `ADMIN-DASHBOARD`를 별도 관리자 영역의 시작점으로 두고 관리 화면을 연결한다.
5. 요청 승인 흐름은 굵은 선으로 강조한다.
6. 예약, 자산, 사용자 관리, 감사 이력은 관리자 대시보드에서 분기되는 보조 흐름으로 구성한다.
7. 모달은 별도 화면 카드로 만들되, 실제 라우팅 화면과 다른 색상 또는 작은 카드로 표현한다.

## 15. MVP 구현 우선순위 기준 화면 흐름

MVP 화면흐름도만 먼저 그릴 경우 다음 흐름을 우선 구성한다.

```text
로그인
-> 권한별 홈
-> 요청 등록
-> 내 요청 상세
-> 팀 승인 요청함
-> 팀 승인 상세
-> 운영 요청함
-> 운영 요청 상세
-> 관리자 요청 관리
-> 관리자 대시보드
```

예약과 자산은 다음 순서로 추가한다.

```text
사용자 홈
-> 회의실 예약
-> 내 예약 목록
-> 관리자 예약 관리
-> 자산 목록
-> 관리자 자산 관리
```

관리자 보조 흐름은 마지막에 추가한다.

```text
관리자 대시보드
-> 회의실/자원 관리
-> 사용자 관리
-> 감사 이력
```
