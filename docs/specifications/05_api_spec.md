# OfficeOps Hub API 명세서

## 1. 문서 목적

이 문서는 `OfficeOps Hub`의 REST API 엔드포인트, 권한, 요청/응답 형식, 주요 에러 코드를 정의한다.

백엔드 컨트롤러 구현, 프론트엔드 API 연동, Swagger 문서화의 기준으로 사용한다.

## 2. 공통 규칙

### 2.1 Base URL

```text
Local: http://localhost:8080/api
Production: https://api.officeops.example.com/api
```

### 2.2 인증 방식

```text
Authorization: Bearer {accessToken}
```

### 2.3 공통 성공 응답

```json
{
  "success": true,
  "data": {}
}
```

### 2.4 공통 에러 응답

```json
{
  "success": false,
  "error": {
    "code": "REQUEST_INVALID_STATUS",
    "message": "현재 상태에서는 요청을 변경할 수 없습니다."
  }
}
```

### 2.5 페이지 응답

```json
{
  "success": true,
  "data": {
    "content": [],
    "page": 0,
    "size": 20,
    "totalElements": 100,
    "totalPages": 5
  }
}
```

## 3. 인증 API

### 3.1 회원가입

```text
POST /auth/signup
권한: 비로그인
```

Request:

```json
{
  "email": "user@company.com",
  "password": "password123!",
  "name": "홍길동",
  "departmentId": 1
}
```

Response:

```json
{
  "success": true,
  "data": {
    "id": 1,
    "email": "user@company.com",
    "name": "홍길동",
    "role": "ROLE_USER",
    "status": "ACTIVE"
  }
}
```

### 3.2 로그인

```text
POST /auth/login
권한: 비로그인
```

Request:

```json
{
  "email": "user@company.com",
  "password": "password123!"
}
```

Response:

```json
{
  "success": true,
  "data": {
    "accessToken": "jwt-token",
    "user": {
      "id": 1,
      "email": "user@company.com",
      "name": "홍길동",
      "role": "ROLE_USER",
      "status": "ACTIVE"
    }
  }
}
```

### 3.3 로그아웃

```text
POST /auth/logout
권한: 로그인 사용자
```

MVP에서는 프론트엔드 토큰 제거를 기본으로 한다.

### 3.4 내 정보 조회

```text
GET /users/me
권한: 로그인 사용자
```

### 3.5 비밀번호 변경

```text
PATCH /users/me/password
권한: 로그인 사용자
우선순위: MVP 이후
```

### 3.6 토큰 재발급

```text
POST /auth/reissue
권한: Refresh Token 보유 사용자
우선순위: MVP 이후
```

## 4. 사용자 관리 API

### 4.1 사용자 목록 조회

```text
GET /admin/users
권한: ROLE_ADMIN
```

Query:

| 이름 | 설명 |
| --- | --- |
| keyword | 이름/이메일 |
| departmentId | 부서 ID |
| role | 권한 |
| status | ACTIVE, INACTIVE |
| page | 페이지 |
| size | 크기 |

### 4.2 사용자 상세 조회

```text
GET /admin/users/{id}
권한: ROLE_ADMIN
```

### 4.3 사용자 권한 변경

```text
PATCH /admin/users/{id}/role
권한: ROLE_ADMIN
```

Request:

```json
{
  "role": "ROLE_OPERATOR"
}
```

### 4.4 사용자 상태 변경

```text
PATCH /admin/users/{id}/status
권한: ROLE_ADMIN
```

Request:

```json
{
  "status": "INACTIVE"
}
```

처리 결과:

- `INACTIVE` 사용자는 로그인할 수 없다.
- 사용자 테이블의 `deactivatedAt`을 기록한다.
- 감사 이력에 변경 전/후 상태를 저장한다.

## 5. 요청 API

### 5.1 내 요청 목록 조회

```text
GET /requests
권한: 로그인 사용자
```

Query:

| 이름 | 설명 |
| --- | --- |
| status | 요청 상태 |
| type | 요청 유형 |
| priority | 우선순위 |
| keyword | 제목/내용 |
| assigneeId | 담당자 |
| dueFrom | 마감일 시작 |
| dueTo | 마감일 종료 |
| delayed | 지연 여부 |
| unassigned | 미배정 여부 |
| page | 페이지 |
| size | 크기 |

### 5.2 요청 등록

```text
POST /requests
권한: 로그인 사용자
```

Request:

```json
{
  "type": "ASSET_REQUEST",
  "title": "모니터 대여 요청",
  "content": "개발용 보조 모니터가 필요합니다.",
  "priority": "NORMAL",
  "detail": {
    "assetCategory": "MONITOR",
    "usageStartDate": "2026-06-10",
    "returnDueDate": "2026-06-30"
  }
}
```

Response:

```json
{
  "success": true,
  "data": {
    "id": 10,
    "status": "PENDING_MANAGER_APPROVAL"
  }
}
```

### 5.3 요청 상세 조회

```text
GET /requests/{id}
권한: 요청 작성자, 관련 팀장, ROLE_OPERATOR, ROLE_ADMIN
```

### 5.4 요청 수정

```text
PATCH /requests/{id}
권한: 요청 작성자
조건: PENDING_MANAGER_APPROVAL 상태만 가능
```

### 5.5 요청 취소

```text
PATCH /requests/{id}/cancel
권한: 요청 작성자, ROLE_ADMIN
```

### 5.6 요청 이력 조회

```text
GET /requests/{id}/histories
권한: 요청 접근 가능 사용자
```

### 5.7 요청 댓글 조회/등록

```text
GET  /requests/{id}/comments
POST /requests/{id}/comments
권한: 요청 접근 가능 사용자
우선순위: 2순위
```

Request:

```json
{
  "visibility": "PUBLIC",
  "content": "처리 일정 확인 부탁드립니다."
}
```

## 6. 팀장 승인 API

### 6.1 팀 승인 요청 목록

```text
GET /manager/approvals
권한: ROLE_MANAGER, ROLE_ADMIN
```

### 6.2 팀 승인 요청 상세

```text
GET /manager/approvals/{id}
권한: ROLE_MANAGER, ROLE_ADMIN
```

### 6.3 팀장 1차 승인

```text
PATCH /manager/approvals/{id}/approve
권한: ROLE_MANAGER, ROLE_ADMIN
조건: PENDING_MANAGER_APPROVAL
```

Request:

```json
{
  "comment": "팀 내 사용 목적 확인 완료"
}
```

### 6.4 팀장 1차 반려

```text
PATCH /manager/approvals/{id}/reject
권한: ROLE_MANAGER, ROLE_ADMIN
조건: PENDING_MANAGER_APPROVAL
```

Request:

```json
{
  "comment": "요청 사유 보완 필요"
}
```

## 7. 운영 담당자 API

### 7.1 운영 요청 목록

```text
GET /operator/requests
권한: ROLE_OPERATOR, ROLE_ADMIN
```

### 7.2 운영 요청 상세

```text
GET /operator/requests/{id}
권한: ROLE_OPERATOR, ROLE_ADMIN
```

### 7.3 최종 승인

```text
PATCH /operator/requests/{id}/approve
권한: ROLE_OPERATOR, ROLE_ADMIN
조건: PENDING_OPERATOR_APPROVAL
```

Request:

```json
{
  "assigneeId": 3,
  "scheduledAt": "2026-06-10T10:00:00",
  "dueAt": "2026-06-12T18:00:00",
  "comment": "운영 담당 배정 완료"
}
```

### 7.4 최종 반려

```text
PATCH /operator/requests/{id}/reject
권한: ROLE_OPERATOR, ROLE_ADMIN
조건: PENDING_OPERATOR_APPROVAL
```

### 7.5 담당자 변경

```text
PATCH /operator/requests/{id}/assignee
권한: ROLE_OPERATOR, ROLE_ADMIN
```

Request:

```json
{
  "assigneeId": 4
}
```

### 7.6 처리 예정일/마감일 변경

```text
PATCH /operator/requests/{id}/due-date
권한: ROLE_OPERATOR, ROLE_ADMIN
```

Request:

```json
{
  "scheduledAt": "2026-06-10T10:00:00",
  "dueAt": "2026-06-12T18:00:00"
}
```

### 7.7 처리 중 변경

```text
PATCH /operator/requests/{id}/in-progress
권한: ROLE_OPERATOR, ROLE_ADMIN, 요청 담당자
조건: APPROVED
```

### 7.8 완료 처리

```text
PATCH /operator/requests/{id}/complete
권한: ROLE_OPERATOR, ROLE_ADMIN, 요청 담당자
조건: IN_PROGRESS
```

## 8. 자원/예약 API

### 8.1 자원 목록 조회

```text
GET /resources
권한: 로그인 사용자
```

### 8.2 자원 등록

```text
POST /admin/resources
권한: ROLE_ADMIN
```

Request:

```json
{
  "resourceCode": "MR-A-301",
  "name": "회의실 A",
  "type": "MEETING_ROOM",
  "location": "3층",
  "capacity": 8,
  "description": "8인용 회의실, 화상회의 장비 보유",
  "status": "AVAILABLE"
}
```

### 8.3 자원 수정

```text
PATCH /admin/resources/{id}
권한: ROLE_ADMIN
```

### 8.4 자원 상태 변경

```text
PATCH /admin/resources/{id}/status
권한: ROLE_ADMIN
```

### 8.5 내 예약 목록 조회

```text
GET /reservations
권한: 로그인 사용자
```

### 8.6 예약 등록

```text
POST /reservations
권한: 로그인 사용자
```

Request:

```json
{
  "resourceId": 1,
  "startAt": "2026-06-10T10:00:00",
  "endAt": "2026-06-10T11:00:00",
  "purpose": "프로젝트 회의"
}
```

### 8.7 예약 취소

```text
PATCH /reservations/{id}/cancel
권한: 예약자, ROLE_OPERATOR, ROLE_ADMIN
```

Request:

```json
{
  "reason": "회의 일정 변경"
}
```

처리 결과:

- 예약 상태를 `CANCELLED`로 변경한다.
- `cancelledBy`, `cancelledAt`, `cancelReason`을 저장한다.
- `reservation_histories`에 취소 이력을 저장한다.

### 8.8 예약 캘린더 조회

```text
GET /reservations/calendar
권한: 로그인 사용자
```

### 8.9 전체 예약 관리

```text
GET   /admin/reservations
PATCH /admin/reservations/{id}/cancel
권한: ROLE_OPERATOR, ROLE_ADMIN
```

## 9. 자산 API

### 9.1 자산 목록 조회

```text
GET /assets
권한: 로그인 사용자
```

### 9.2 자산 등록

```text
POST /admin/assets
권한: ROLE_OPERATOR, ROLE_ADMIN
```

### 9.3 자산 수정

```text
PATCH /admin/assets/{id}
권한: ROLE_OPERATOR, ROLE_ADMIN
```

### 9.4 자산 상태 변경

```text
PATCH /admin/assets/{id}/status
권한: ROLE_OPERATOR, ROLE_ADMIN
```

### 9.5 자산 대여 처리

```text
POST /admin/assets/{id}/loans
권한: ROLE_OPERATOR, ROLE_ADMIN
```

Request:

```json
{
  "userId": 1,
  "requestId": 10,
  "dueAt": "2026-06-30T18:00:00"
}
```

처리 결과:

- `requestId`는 비품 요청 승인 결과로 대여하는 경우에만 전달한다.
- `processedBy`는 현재 로그인한 운영 담당자 또는 관리자로 저장한다.

### 9.6 자산 반납 처리

```text
PATCH /admin/asset-loans/{id}/return
권한: ROLE_OPERATOR, ROLE_ADMIN
```

### 9.7 내 자산 대여 현황

```text
GET /asset-loans/me
권한: 로그인 사용자
```

## 10. 대시보드 API

```text
GET /admin/dashboard/summary
GET /admin/dashboard/requests/monthly
GET /admin/dashboard/requests/status
GET /admin/dashboard/assets/status
GET /admin/dashboard/reservations/daily
GET /admin/dashboard/requests/delayed
GET /admin/dashboard/requests/unassigned
권한: ROLE_ADMIN
```

## 11. 공통 코드 API

공통 코드는 프론트엔드 select 옵션과 서버 검증 기준으로 사용한다.

```text
GET /codes/groups
GET /codes/groups/{groupCode}/items
권한: 로그인 사용자
```

예시:

```text
GET /codes/groups/ASSET_CATEGORY/items
GET /codes/groups/FACILITY_ISSUE_TYPE/items
```

Response:

```json
{
  "success": true,
  "data": [
    {
      "code": "MONITOR",
      "name": "모니터",
      "sortOrder": 1
    }
  ]
}
```

공통 코드 관리는 MVP에서는 seed 데이터로 처리하고, 관리 API는 MVP 이후로 미룬다.

## 12. 알림 API

```text
GET   /notifications
PATCH /notifications/{id}/read
권한: 로그인 사용자
우선순위: 2순위
```

알림 응답에는 연결 대상 정보를 포함한다.

```json
{
  "id": 1,
  "type": "REQUEST_APPROVED",
  "targetType": "REQUEST",
  "targetId": 10,
  "title": "요청이 승인되었습니다.",
  "content": "모니터 대여 요청이 최종 승인되었습니다.",
  "readAt": null,
  "createdAt": "2026-06-10T09:00:00"
}
```

## 13. 감사 이력 API

```text
GET /admin/audit-logs
권한: ROLE_ADMIN
```

Query:

| 이름 | 설명 |
| --- | --- |
| actorId | 작업자 |
| actionType | 작업 유형 |
| targetType | 대상 유형 |
| startDate | 시작일 |
| endDate | 종료일 |
| page | 페이지 |
| size | 크기 |

응답에는 `beforeValue`, `afterValue`를 JSON 객체로 반환한다.

```json
{
  "id": 1,
  "actorId": 3,
  "actionType": "REQUEST_ASSIGNEE_CHANGED",
  "targetType": "REQUEST",
  "targetId": 10,
  "beforeValue": {
    "assigneeId": 3
  },
  "afterValue": {
    "assigneeId": 4
  },
  "ipAddress": "127.0.0.1",
  "userAgent": "Mozilla/5.0",
  "createdAt": "2026-06-10T10:00:00"
}
```

## 14. MVP 이후 API

```text
POST   /requests/{id}/attachments
GET    /requests/{id}/attachments
DELETE /attachments/{id}
GET    /admin/requests/export
GET    /admin/assets/export
GET    /admin/reservations/export
POST   /reservations/recurring
GET    /admin/operation-days
POST   /admin/operation-days
```

## 15. 주요 에러 코드

| 코드 | 설명 |
| --- | --- |
| AUTH_INVALID_TOKEN | 유효하지 않은 토큰 |
| AUTH_ACCESS_DENIED | 접근 권한 없음 |
| AUTH_INACTIVE_USER | 비활성화 사용자 |
| USER_NOT_FOUND | 사용자를 찾을 수 없음 |
| REQUEST_NOT_FOUND | 요청을 찾을 수 없음 |
| REQUEST_INVALID_STATUS | 잘못된 요청 상태 변경 |
| REQUEST_NOT_ASSIGNEE | 요청 담당자가 아님 |
| APPROVAL_NOT_ALLOWED | 승인 권한 없음 |
| RESERVATION_CONFLICT | 예약 시간이 중복됨 |
| ASSET_NOT_AVAILABLE | 자산을 사용할 수 없음 |
| VALIDATION_ERROR | 입력값 검증 실패 |
