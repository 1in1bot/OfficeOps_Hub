# OfficeOps Hub 개발 환경 및 협업 규칙

## 1. 문서 목적

이 문서는 `OfficeOps Hub`의 로컬 개발 환경, 실행 방법, 환경변수, 브랜치 전략, 커밋/PR/이슈 규칙을 정의한다.

팀원 간 개발 환경 차이를 줄이고, 작업 단위를 명확히 관리하는 것을 목적으로 한다.

## 2. 기술 스택

### 2.1 Backend

| 기술 | 버전/기준 |
| --- | --- |
| Java | 21 |
| Spring Boot | 3.x |
| Spring Security | 6.x |
| JPA | Spring Data JPA |
| DB | PostgreSQL |
| Migration | Flyway |
| API 문서 | Swagger/OpenAPI |
| Test | JUnit 5, Mockito |

### 2.2 Frontend

| 기술 | 버전/기준 |
| --- | --- |
| React | 18 이상 |
| Vite | 최신 안정 버전 |
| Router | React Router |
| API Client | Axios |
| Server State | TanStack Query |
| Client State | Zustand |
| CSS | Tailwind CSS |

### 2.3 Infra

| 기술 | 용도 |
| --- | --- |
| Docker Compose | 로컬 DB/서비스 실행 |
| AWS EC2 | 서버 배포 |
| AWS RDS PostgreSQL | 운영 DB |
| GitHub Actions | CI |
| Nginx | 정적 파일 서빙, Reverse Proxy |

## 3. 권장 프로젝트 구조

```text
officeops-hub
 ├─ backend
 │   ├─ src
 │   ├─ build.gradle
 │   └─ Dockerfile
 ├─ frontend
 │   ├─ src
 │   ├─ package.json
 │   └─ Dockerfile
 ├─ docs
 ├─ docker-compose.yml
 └─ README.md
```

현재 문서 작업은 `docs` 기준으로 관리한다.

## 4. 로컬 실행 환경

### 4.1 필수 설치

| 도구 | 권장 |
| --- | --- |
| JDK | 21 |
| Node.js | 20 LTS 이상 |
| npm | Node.js 포함 버전 |
| Docker Desktop | 최신 안정 버전 |
| Git | 최신 안정 버전 |

### 4.2 Docker Compose 서비스

로컬 개발에서는 PostgreSQL을 Docker Compose로 실행한다.

예상 서비스:

```text
postgres
backend
frontend
```

초기에는 DB만 Docker로 실행하고 백엔드/프론트는 로컬 실행해도 된다.

## 5. 환경변수

### 5.1 Backend

```text
SPRING_PROFILES_ACTIVE=local
SERVER_PORT=8080

DB_HOST=localhost
DB_PORT=5432
DB_NAME=officeops
DB_USERNAME=officeops
DB_PASSWORD=officeops

JWT_SECRET=local-development-secret
JWT_ACCESS_TOKEN_EXPIRES_IN=3600000

CORS_ALLOWED_ORIGINS=http://localhost:5173
```

### 5.2 Frontend

```text
VITE_API_BASE_URL=http://localhost:8080/api
```

주의:

- 운영 DB 비밀번호와 JWT Secret은 Git에 커밋하지 않는다.
- `.env` 파일은 `.gitignore`에 포함한다.
- 예시 파일은 `.env.example`로 제공한다.

## 6. 실행 명령

### 6.1 DB 실행

```bash
docker compose up -d postgres
```

### 6.2 Backend 실행

```bash
cd backend
./gradlew bootRun
```

### 6.3 Frontend 실행

```bash
cd frontend
npm install
npm run dev
```

### 6.4 테스트

Backend:

```bash
cd backend
./gradlew test
```

Frontend:

```bash
cd frontend
npm run test
npm run lint
```

## 7. 프로파일

| 프로파일 | 용도 |
| --- | --- |
| local | 로컬 개발 |
| test | 테스트 |
| prod | 운영 |

권장:

- `application-local.yml`: 로컬 DB 연결
- `application-test.yml`: 테스트 DB 또는 Testcontainers
- `application-prod.yml`: 운영 환경변수 기반 설정

## 8. Git 브랜치 전략

기본 브랜치:

```text
main
develop
```

작업 브랜치:

```text
feature/be-auth-login
feature/be-request-approval
feature/fe-request-list
feature/fe-manager-approval
fix/reservation-conflict
docs/api-spec
infra/docker-compose
```

규칙:

- `main`은 배포 가능한 상태만 유지한다.
- 개발 작업은 `develop` 기준으로 브랜치를 생성한다.
- 기능 단위로 브랜치를 작게 나눈다.
- PR merge 전 테스트를 통과해야 한다.

## 9. 커밋 메시지 규칙

형식:

```text
type(scope): message
```

type:

| 타입 | 설명 |
| --- | --- |
| feat | 기능 추가 |
| fix | 버그 수정 |
| docs | 문서 수정 |
| refactor | 리팩터링 |
| test | 테스트 추가/수정 |
| chore | 설정/빌드 기타 |
| infra | 인프라 설정 |

예시:

```text
feat(auth): 로그인 API 구현
feat(request): 팀장 승인 처리 구현
fix(reservation): 예약 중복 검증 조건 수정
docs(api): 요청 승인 API 명세 추가
test(request): 상태 전이 테스트 추가
```

## 10. Issue 규칙

이슈 제목:

```text
[BE] 로그인 API 구현
[FE] 팀 승인 요청함 화면 구현
[DOCS] ERD 문서 작성
[INFRA] Docker Compose 구성
```

이슈 본문:

```text
## 작업 내용
- 

## 완료 조건
- 

## 참고 문서
- 
```

라벨:

| 라벨 | 설명 |
| --- | --- |
| backend | 백엔드 |
| frontend | 프론트엔드 |
| docs | 문서 |
| infra | 인프라 |
| bug | 버그 |
| enhancement | 개선 |
| high-priority | 우선순위 높음 |

## 11. PR 규칙

PR 제목:

```text
[BE] 요청 승인 API 구현
```

PR 본문:

```text
## 작업 내용
- 

## 테스트
- [ ] ./gradlew test
- [ ] npm run lint

## 체크리스트
- [ ] 문서와 구현이 일치한다.
- [ ] 권한 검증을 백엔드에서 처리했다.
- [ ] 상태 변경 이력을 저장한다.
- [ ] 필요한 테스트를 추가했다.
```

리뷰 기준:

- API 응답 형식이 문서와 일치하는가
- 권한 검증이 컨트롤러/서비스 계층에서 누락되지 않았는가
- 상태 전이 규칙이 지켜지는가
- 예외 처리가 공통 형식을 따르는가
- 불필요한 리팩터링이 섞이지 않았는가

## 12. 코드 스타일

### 12.1 Backend

- 패키지는 기능 단위로 구성한다.
- Controller, Service, Repository 책임을 분리한다.
- Entity를 API 응답으로 직접 반환하지 않는다.
- Request/Response DTO를 사용한다.
- 비즈니스 예외는 공통 예외 타입을 사용한다.
- 상태 전이는 서비스 계층에서 검증한다.

권장 패키지:

```text
com.officeops
 ├─ auth
 ├─ user
 ├─ request
 ├─ approval
 ├─ asset
 ├─ reservation
 ├─ resource
 ├─ dashboard
 ├─ audit
 └─ common
```

### 12.2 Frontend

- 페이지는 `pages`에 둔다.
- 도메인별 기능 코드는 `features`에 둔다.
- API 함수는 `api`에 둔다.
- 서버 데이터는 TanStack Query로 관리한다.
- 로그인 사용자, 토큰, UI 상태는 Zustand로 관리한다.
- 권한별 라우팅 보호 컴포넌트를 사용한다.

권장 구조:

```text
src
 ├─ api
 ├─ app
 ├─ components
 ├─ features
 ├─ hooks
 ├─ pages
 ├─ stores
 └─ utils
```

## 13. 문서 관리 규칙

- 기능 변경 전 관련 문서를 먼저 수정한다.
- API 변경 시 `05_api_spec.md`를 수정한다.
- 상태/권한 변경 시 `06_status_permission_policy.md`를 수정한다.
- DB 변경 시 `04_erd_db_design.md`와 Flyway migration을 함께 수정한다.
- 테스트 기준 변경 시 `07_test_plan.md`를 수정한다.

## 14. 배포 전 체크리스트

- [ ] 백엔드 테스트 통과
- [ ] 프론트 빌드 성공
- [ ] 환경변수 분리 확인
- [ ] Swagger 접근 확인
- [ ] DB migration 성공
- [ ] 관리자 seed 계정 생성 확인
- [ ] CORS 운영 도메인 설정 확인
- [ ] JWT Secret 운영값 설정 확인
- [ ] 로그에 민감 정보가 남지 않는지 확인
