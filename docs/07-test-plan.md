# OfficeOps Hub 테스트 계획서

## 1. 문서 목적

이 문서는 `OfficeOps Hub`의 테스트 범위, 우선순위, 주요 테스트 케이스, 테스트 전략을 정의한다.

백엔드 단위/통합 테스트, 프론트엔드 화면 테스트, 수동 QA의 기준으로 사용한다.

## 2. 테스트 목표

- 인증/인가가 올바르게 동작하는지 검증한다.
- 요청 승인 상태 전이가 정책대로 동작하는지 검증한다.
- 팀장/운영 담당자/관리자 권한이 분리되는지 검증한다.
- 예약 중복 방지 로직을 검증한다.
- 자산 대여/반납 상태 변경을 검증한다.
- 관리자 작업 감사 이력이 누락 없이 저장되는지 검증한다.
- 주요 목록 검색, 필터, 페이지네이션을 검증한다.

## 3. 테스트 범위

| 구분 | 범위 |
| --- | --- |
| 단위 테스트 | 서비스 계층 비즈니스 로직 |
| 통합 테스트 | Controller, Security, Repository 연동 |
| API 테스트 | 주요 REST API 정상/예외 응답 |
| 프론트 테스트 | 라우팅 보호, 폼 검증, 주요 화면 렌더링 |
| 수동 QA | 사용자 흐름 기반 시나리오 |

## 4. 우선순위

### 4.1 필수 테스트

- 로그인/회원가입
- 비활성 사용자 로그인 차단
- 권한별 API 접근 제한
- 요청 등록/수정/취소
- 팀장 1차 승인/반려
- 운영 담당자 최종 승인/반려
- 담당자 지정/마감일 설정
- 처리 중/완료 상태 변경
- 요청 상태 변경 이력 저장
- 승인 이력 저장
- 감사 이력 저장
- 예약 중복 방지
- 자산 대여/반납

### 4.2 2순위 테스트

- 요청 댓글/처리 코멘트
- 인앱 알림 생성/조회
- 지연 요청/미배정 요청 대시보드
- 자산 상태 변경 이력
- 사용자 권한 변경 이력

### 4.3 MVP 이후 테스트

- 파일 첨부
- 이메일 알림
- CSV 다운로드
- 반복 예약
- 휴일/운영 시간 관리

## 5. 백엔드 테스트 케이스

### 5.1 인증/사용자

| ID | 케이스 | 기대 결과 |
| --- | --- | --- |
| AUTH-TC-001 | 정상 회원가입 | 사용자 생성, ROLE_USER, ACTIVE |
| AUTH-TC-002 | 중복 이메일 회원가입 | Validation 에러 |
| AUTH-TC-003 | 정상 로그인 | Access Token 반환 |
| AUTH-TC-004 | 비밀번호 불일치 | 로그인 실패 |
| AUTH-TC-005 | INACTIVE 사용자 로그인 | AUTH_INACTIVE_USER |
| AUTH-TC-006 | 관리자 사용자 비활성화 | 상태 INACTIVE, 감사 이력 저장 |

### 5.2 권한

| ID | 케이스 | 기대 결과 |
| --- | --- | --- |
| PERM-TC-001 | ROLE_USER가 `/admin/users` 접근 | 403 |
| PERM-TC-002 | ROLE_USER가 `/manager/approvals` 접근 | 403 |
| PERM-TC-003 | ROLE_MANAGER가 타 부서 요청 승인 | 403 |
| PERM-TC-004 | ROLE_OPERATOR가 사용자 권한 변경 | 403 |
| PERM-TC-005 | ROLE_ADMIN이 감사 이력 조회 | 성공 |

### 5.3 요청

| ID | 케이스 | 기대 결과 |
| --- | --- | --- |
| REQ-TC-001 | ASSET_REQUEST 등록 | PENDING_MANAGER_APPROVAL 생성 |
| REQ-TC-002 | VISITOR_REQUEST 필수값 누락 | Validation 에러 |
| REQ-TC-003 | 본인 요청 상세 조회 | 성공 |
| REQ-TC-004 | 타인 요청 상세 조회 | 403 |
| REQ-TC-005 | PENDING_MANAGER_APPROVAL 요청 수정 | 성공 |
| REQ-TC-006 | APPROVED 요청 수정 | REQUEST_INVALID_STATUS |
| REQ-TC-007 | 요청자 취소 | CANCELLED, 이력 저장 |

### 5.4 승인/상태 전이

| ID | 케이스 | 기대 결과 |
| --- | --- | --- |
| APR-TC-001 | 팀장이 소속 팀원 요청 승인 | PENDING_OPERATOR_APPROVAL |
| APR-TC-002 | 팀장이 소속 팀원 요청 반려 | REJECTED |
| APR-TC-003 | 팀장이 타 팀 요청 승인 | 403 |
| APR-TC-004 | 운영 담당자 최종 승인 | APPROVED, 담당자/마감일 저장 |
| APR-TC-005 | 운영 담당자 최종 반려 | REJECTED |
| APR-TC-006 | APPROVED 요청 처리 중 변경 | IN_PROGRESS |
| APR-TC-007 | IN_PROGRESS 요청 완료 | COMPLETED |
| APR-TC-008 | COMPLETED 요청 상태 변경 | REQUEST_INVALID_STATUS |
| APR-TC-009 | 승인 시 request_approvals 저장 | 저장됨 |
| APR-TC-010 | 상태 변경 시 request_histories 저장 | 저장됨 |

### 5.5 감사 이력

| ID | 케이스 | 기대 결과 |
| --- | --- | --- |
| AUDIT-TC-001 | 최종 승인 시 담당자 지정 | audit_logs 저장 |
| AUDIT-TC-002 | 담당자 변경 | before/after 저장 |
| AUDIT-TC-003 | 마감일 변경 | before/after 저장 |
| AUDIT-TC-004 | 사용자 비활성화 | audit_logs 저장 |
| AUDIT-TC-005 | 자원 사용 중지 | audit_logs 저장 |
| AUDIT-TC-006 | 감사 로그 저장 | beforeValue/afterValue가 JSON 구조로 저장 |
| AUDIT-TC-007 | 감사 로그 저장 | IP/User-Agent 저장 가능 |

### 5.6 예약

| ID | 케이스 | 기대 결과 |
| --- | --- | --- |
| RSV-TC-001 | 빈 시간 회의실 예약 | 성공 |
| RSV-TC-002 | 10:00~11:00 예약 존재, 10:30~11:30 예약 | RESERVATION_CONFLICT |
| RSV-TC-003 | 10:00~11:00 예약 존재, 11:00~12:00 예약 | 성공 |
| RSV-TC-004 | 시작 시간이 종료 시간보다 늦음 | Validation 에러 |
| RSV-TC-005 | DISABLED 자원 예약 | Validation 에러 |
| RSV-TC-006 | 예약자 본인 취소 | CANCELLED |
| RSV-TC-007 | 예약 취소 | cancelled_by/cancelled_at/cancel_reason 저장 |
| RSV-TC-008 | 예약 취소 | reservation_histories 저장 |

### 5.7 자산

| ID | 케이스 | 기대 결과 |
| --- | --- | --- |
| ASSET-TC-001 | 자산 등록 | AVAILABLE 생성 |
| ASSET-TC-002 | 중복 자산 코드 등록 | Validation 에러 |
| ASSET-TC-003 | AVAILABLE 자산 대여 | IN_USE, asset_loans 저장 |
| ASSET-TC-004 | MAINTENANCE 자산 대여 | ASSET_NOT_AVAILABLE |
| ASSET-TC-005 | 대여 중 자산 반납 | AVAILABLE, returned_at 저장 |
| ASSET-TC-006 | 요청 기반 자산 대여 | asset_loans.request_id 저장 |
| ASSET-TC-007 | 자산 대여/반납 처리 | processed_by 저장 |

### 5.8 알림

| ID | 케이스 | 기대 결과 |
| --- | --- | --- |
| NOTI-TC-001 | 요청 승인 알림 생성 | targetType=REQUEST, targetId 저장 |
| NOTI-TC-002 | 예약 취소 알림 생성 | targetType=RESERVATION, targetId 저장 |
| NOTI-TC-003 | 알림 읽음 처리 | read_at 저장 |

### 5.9 공통 코드

| ID | 케이스 | 기대 결과 |
| --- | --- | --- |
| CODE-TC-001 | 코드 그룹 항목 조회 | 정렬 순서대로 ACTIVE 항목 반환 |
| CODE-TC-002 | 존재하지 않는 코드 그룹 조회 | 404 또는 빈 목록 정책에 맞게 응답 |
| CODE-TC-003 | 비활성 코드 항목 | 사용자 입력 옵션에서 제외 |

## 6. 프론트엔드 테스트 케이스

| ID | 케이스 | 기대 결과 |
| --- | --- | --- |
| FE-TC-001 | 비로그인 사용자가 `/user` 접근 | `/login` 이동 |
| FE-TC-002 | ROLE_USER가 `/manager/approvals` 접근 | `/forbidden` 이동 |
| FE-TC-003 | ROLE_MANAGER가 팀 승인 요청함 접근 | 화면 표시 |
| FE-TC-004 | ROLE_OPERATOR가 운영 요청함 접근 | 화면 표시 |
| FE-TC-005 | 존재하지 않는 URL 접근 | 404 화면 표시 |
| FE-TC-006 | 요청 등록 필수값 누락 | 필드 에러 표시 |
| FE-TC-007 | 요청 유형 변경 | 유형별 추가 필드 변경 |
| FE-TC-008 | 예약 중복 API 에러 | 안내 메시지 표시 |

## 7. 수동 QA 시나리오

### 7.1 요청 승인 전체 흐름

```text
1. 일반 직원 로그인
2. 비품 요청 등록
3. 팀장 로그인
4. 팀 승인 요청함에서 1차 승인
5. 운영 담당자 로그인
6. 운영 요청함에서 최종 승인, 담당자와 마감일 지정
7. 담당자가 처리 중으로 변경
8. 담당자가 완료 처리
9. 요청자 화면에서 완료 상태 확인
10. 관리자 감사 이력에서 승인/담당자 지정 이력 확인
```

### 7.2 예약 흐름

```text
1. 사용자 로그인
2. 회의실 예약 화면 이동
3. 회의실과 시간 선택
4. 예약 생성
5. 같은 시간 중복 예약 시도
6. 중복 예약 에러 확인
7. 예약 취소
```

### 7.3 자산 대여 흐름

```text
1. 운영 담당자 로그인
2. 자산 등록
3. 자산 대여 처리
4. 사용자 내 자산 대여 현황 확인
5. 자산 반납 처리
6. 자산 상태 AVAILABLE 확인
```

## 8. 테스트 실행 기준

### 8.1 백엔드

```text
./gradlew test
```

권장:

- 서비스 단위 테스트는 빠르게 실행되어야 한다.
- 통합 테스트는 테스트 프로파일과 테스트 DB를 사용한다.
- 예약 중복, 요청 상태 전이, 권한 검증은 반드시 자동화한다.

### 8.2 프론트엔드

```text
npm run test
npm run lint
```

프론트 테스트 도구는 프로젝트 세팅 시 확정한다.

## 9. 완료 기준

- 필수 테스트 케이스가 모두 통과한다.
- 요청 상태 전이 테스트가 자동화되어 있다.
- 예약 중복 테스트가 자동화되어 있다.
- 권한별 API 접근 제한 테스트가 자동화되어 있다.
- 주요 수동 QA 시나리오가 성공한다.
