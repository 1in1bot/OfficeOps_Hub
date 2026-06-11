# OfficeOps Hub 팀 운영 문서

## 문서 버전 이력

| 버전 | 기준 | 수정 사항 | 삭제 사항 |
| --- | --- | --- | --- |
| v1.4.0 | 문서별 독립 버전 관리 정책 반영 | 문서마다 실제 업데이트 여부를 기준으로 버전을 상승시키고, 수정된 문서의 파일명과 참조 링크만 함께 갱신하도록 규칙 정리 | 전체 문서 버전을 일괄로 맞춘다는 해석 제거 |
| v1.5.0 | 로컬/원격 브랜치 생성 지침 추가 | 작업 브랜치를 `dev` 하위 브랜치로 고정하고, 기능/문서/기타 작업별 브랜치 네이밍과 PR 대상 규칙 추가 | `develop` 기준 브랜치 생성 지침 제거 |
| v1.7.0 | 보조 도구 작업 흔적 비노출 지침 반영 | 프로젝트 문서에서 보조 도구와의 작업 내용이 드러나지 않도록 원격 업로드 문구 정리 | 커밋 주체를 외부 도구로 표현하는 문구 제거 |
| v1.0.0 | 버전 단위 문서 관리 방식 도입 이전 팀 운영 문서 | 기존 팀 운영, 협업, 문서 관리 규칙 기준선 | 없음 |
| v1.1.0 | 버전 단위 문서 관리 방식 도입 | 문서별 버전 이력 작성 방식, 수정/삭제 사항 기록 규칙, 관련 문서 동시 갱신 규칙 추가 | 없음 |
| v1.2.0 | 문서 네이밍 및 버전 관리 체계 정리 | 문서 파일명을 번호 없는 한글 제목 기반 규칙으로 정리하고 버전 표기를 semantic version 형식으로 통일 | 문서 번호 접두어와 영문 기반 산출물 파일명 제거 |
| v1.3.0 | 파일명 버전 최신화 규칙 반영 | 문서 파일명의 버전을 문서 내부 최신 버전과 동일하게 관리하도록 정리하고, 이후 수정 및 버전 상승 시 파일명과 참조 링크를 즉시 갱신하는 규칙 추가 | 최신 버전과 맞지 않는 파일명 버전 표기 제거 |
| v1.6.0 | 원격 업로드 요청 처리 지침 추가 | 원격 업로드 요청 시 commit, push, PR까지 진행하는 순서와 커밋 메시지/PR description 작성 규칙 추가 | 커밋 주체를 작업 환경 주인이 아닌 외부 주체로 해석할 여지가 있는 업로드 방식 제거 |
| v1.8.0 | 브랜치 생성 절차 보강 | 로컬 브랜치 생성, 원격 브랜치 최초 push, 작업 유형별 브랜치 예시를 추가 | 없음 |
| v1.9.0 | 버전 관리 기준 보강 | 문서별 기본 버전, 프로젝트 전체 버전 시작점, major/minor 버전 상승 기준, 작업 전 버전 확인 규칙 추가 | 없음 |

## 1. 문서 목적

이 문서는 `OfficeOps Hub` 팀의 운영 방식, 역할 합의, 로컬 개발 환경, 실행 방법, 환경변수, 브랜치 전략, 커밋/PR/이슈 규칙을 정의한다.

팀원 간 개발 환경 차이를 줄이고, 작업 단위를 명확히 관리하는 것을 목적으로 한다.

## 2. 팀 운영 원칙

- 핵심 설계 결정은 팀 회의에서 합의한다.
- 기능 구현 전 관련 문서를 먼저 확인하고, 변경이 필요하면 문서를 먼저 수정한다.
- 백엔드와 프론트엔드는 역할을 나누되 API 명세, 권한 정책, QA는 함께 검토한다.
- 일정이 지연되면 2순위 기능을 줄이고 MVP 핵심 흐름을 우선 완성한다.
- PR은 작은 기능 단위로 만들고, 리뷰 후 병합한다.

## 3. 역할 합의

| 역할 | 주요 책임 | 주요 산출물 |
| --- | --- | --- |
| 백엔드 담당 | 인증/인가, 요청 API, 상태 전이, DB 설계, 이력 저장, 테스트 | API, Entity, Service, Repository, 테스트 코드 |
| 프론트엔드 담당 | React 화면, 라우팅 보호, 요청/예약/자산 화면, 관리자 대시보드 | 페이지, 컴포넌트, API 연동, 상태 관리 |
| 공통 | 요구사항 정리, API 명세, ERD, README, 발표자료, QA | 문서 산출물, 이슈, PR 리뷰, 시연 흐름 |

## 4. 기술 스택

### 4.1 Backend

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

### 4.2 Frontend

| 기술 | 버전/기준 |
| --- | --- |
| React | 18 이상 |
| Vite | 최신 안정 버전 |
| Router | React Router |
| API Client | Axios |
| Server State | TanStack Query |
| Client State | Zustand |
| CSS | Tailwind CSS |

### 4.3 Infra

| 기술 | 용도 |
| --- | --- |
| Docker Compose | 로컬 DB/서비스 실행 |
| AWS EC2 | 서버 배포 |
| AWS RDS PostgreSQL | 운영 DB |
| GitHub Actions | CI |
| Nginx | 정적 파일 서빙, Reverse Proxy |

## 5. 권장 프로젝트 구조

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

## 6. 로컬 실행 환경

### 6.1 필수 설치

| 도구 | 권장 |
| --- | --- |
| JDK | 21 |
| Node.js | 20 LTS 이상 |
| npm | Node.js 포함 버전 |
| Docker Desktop | 최신 안정 버전 |
| Git | 최신 안정 버전 |

### 6.2 Docker Compose 서비스

로컬 개발에서는 PostgreSQL을 Docker Compose로 실행한다.

예상 서비스:

```text
postgres
backend
frontend
```

초기에는 DB만 Docker로 실행하고 백엔드/프론트는 로컬 실행해도 된다.

## 7. 환경변수

### 7.1 Backend

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

### 7.2 Frontend

```text
VITE_API_BASE_URL=http://localhost:8080/api
```

주의:

- 운영 DB 비밀번호와 JWT Secret은 Git에 커밋하지 않는다.
- `.env` 파일은 `.gitignore`에 포함한다.
- 예시 파일은 `.env.example`로 제공한다.

## 8. 실행 명령

### 8.1 DB 실행

```bash
docker compose up -d postgres
```

### 8.2 Backend 실행

```bash
cd backend
./gradlew bootRun
```

### 8.3 Frontend 실행

```bash
cd frontend
npm install
npm run dev
```

### 8.4 테스트

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

## 9. 프로파일

| 프로파일 | 용도 |
| --- | --- |
| local | 로컬 개발 |
| test | 테스트 |
| prod | 운영 |

권장:

- `application-local.yml`: 로컬 DB 연결
- `application-test.yml`: 테스트 DB 또는 Testcontainers
- `application-prod.yml`: 운영 환경변수 기반 설정

## 10. Git 브랜치 전략

기본 브랜치:

```text
main
dev
```

작업 브랜치:

```text
feature/auth-login
feature/request-approval
docs/api-spec
docs/screen-flow
fix/reservation-conflict
infra/docker-compose
chore/gitignore
```

규칙:

- `main`은 배포 가능한 상태만 유지한다.
- 모든 작업 브랜치는 로컬과 원격 모두 `dev` 하위 브랜치로 생성한다.
- PR 대상 브랜치는 항상 `dev`로 고정한다.
- `main`으로 직접 PR을 생성하지 않는다.
- 기능 구현 브랜치는 `feature/feature_name` 형식으로 생성한다.
- 기능 브랜치에서는 해당 기능만 구현한다.
- 새로운 기능을 구현할 때는 기존 기능 브랜치를 재사용하지 않고 새 기능 브랜치로 이동한다.
- 문서 작업 브랜치는 `docs/docs_name` 형식으로 생성한다.
- 문서 브랜치에서는 해당 문서 작업만 진행한다.
- 기능과 문서 외 작업은 작업 성격에 맞는 prefix와 이름으로 브랜치를 생성한다.
- PR merge 전 테스트를 통과해야 한다.

로컬 브랜치 생성 절차:

```bash
git switch dev
git pull origin dev
git switch -c feature/feature_name
```

원격 브랜치 최초 업로드:

```bash
git push -u origin feature/feature_name
```

작업 유형별 예시:

| 작업 유형 | 브랜치 형식 | 예시 |
| --- | --- | --- |
| 기능 구현 | `feature/feature_name` | `feature/auth-login` |
| 문서 작업 | `docs/docs_name` | `docs/branch-strategy` |
| 버그 수정 | `fix/fix_name` | `fix/reservation-conflict` |
| 인프라 | `infra/infra_name` | `infra/docker-compose` |
| 설정/기타 | `chore/chore_name` | `chore/gitignore` |

## 11. 커밋 메시지 규칙

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

## 12. Issue 규칙

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

## 13. PR 규칙

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

## 14. 원격 업로드 요청 처리

원격 업로드 요청을 받으면 아래 순서에 따른다.

1. 변경 사항을 확인한다.
2. 필요한 파일을 스테이징한다.
3. 커밋을 생성한다.
4. 현재 작업 브랜치를 원격에 push한다.
5. `dev`를 대상으로 PR을 생성한다.

커밋 메시지 규칙:

```text
[2026.06.09 hwan] feature feature_name
```

- 날짜는 작업일 기준 `YYYY.MM.DD` 형식으로 작성한다.
- 작업자는 현재 작업 환경의 주인으로 작성한다.
- 현재 작업 환경의 주인은 `hwan`으로 고정한다.
- 커밋 메시지의 작업자명은 현재 작업 환경의 주인으로만 작성한다.
- 작업 환경이 바뀌면 커밋 전 작업 환경의 주인을 묻고 확인한다.
- `feature feature_name` 부분은 실제 작업 내용에 맞게 `feature`, `docs`, `fix`, `infra`, `chore` 등으로 조정한다.

PR description은 간단하게 작성하되 아래 두 항목으로 구성한다.

```md
## 작업 핵심 내용
- 

## 기존과의 변경점
- 
```

## 15. 코드 스타일

### 14.1 Backend

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

### 14.2 Frontend

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

## 16. 문서 관리 규칙

- 기능 변경 전 관련 문서를 먼저 수정한다.
- API 변경 시 `docs/specifications/OfficeOps_hub_인터페이스명세서_v1.3.0.md`를 수정한다.
- 상태/권한 변경 시 `docs/specifications/OfficeOps_hub_상태권한정책문서_v1.3.0.md`를 수정한다.
- DB 변경 시 `docs/specifications/OfficeOps_hub_데이터베이스설계서_v1.3.0.md`와 Flyway migration을 함께 수정한다.
- 테스트 기준 변경 시 `docs/specifications/OfficeOps_hub_테스트계획서_v1.3.0.md`를 수정한다.
- 문서는 버전 단위로 관리한다.
- 각 문서 상단에는 `문서 버전 이력` 표를 둔다.
- 각각의 문서 작업은 기본 `v1.0.0`으로 시작한다.
- 이후 기능 범위, 권한, API, 화면, 정책 변경은 해당 문서가 실제로 수정될 때만 순차 증가시킨다.
- 문서별 변경 시점이 다를 수 있으므로 문서마다 최신 버전은 달라도 된다.
- 수정된 문서는 파일명 버전을 문서 내부 최신 버전과 동일하게 갱신한다.
- 파일명 버전 변경 시 해당 문서를 참조하는 README와 관련 문서의 링크만 함께 수정한다.
- 버전 이력에는 `버전`, `기준`, `수정 사항`, `삭제 사항`을 모두 작성한다.
- 삭제된 화면, API, 기능, 정책도 삭제 사항 칸에 명시한다.
- 여러 문서에 영향을 주는 변경이라도 실제로 수정된 문서만 버전을 상승시킨다.
- 변경 이유나 상세 설명이 길어지면 해당 문서 본문에 반영하고, 버전 이력에는 요약만 남긴다.
- 기능 추가, 기능 수정, 문서 작업을 시작할 때는 관련 문서의 버전 이력을 먼저 확인한다.
- 작업 내용이 기존 기준선이나 최신 이력과 달라지는 경우 필요한 문서를 즉시 버전 업한다.
- 프로젝트 전체 버전은 `0.1.0`으로 시작한다.
- 실제 기능 구현이 완료되어 첫 안정 버전으로 볼 수 있을 때 프로젝트 전체 버전을 `1.0.0`으로 올린다.
- minor 변경점은 `0.1.0` 단위로 올린다. 예: `0.1.0` -> `0.2.0`
- major 변경점은 `1.0.0` 단위로 올린다. 예: `1.0.0` -> `2.0.0`
- 프로젝트 전체 버전과 문서별 산출물 버전은 별도로 관리한다.

## 17. 배포 전 체크리스트

- [ ] 백엔드 테스트 통과
- [ ] 프론트 빌드 성공
- [ ] 환경변수 분리 확인
- [ ] Swagger 접근 확인
- [ ] DB migration 성공
- [ ] 관리자 seed 계정 생성 확인
- [ ] CORS 운영 도메인 설정 확인
- [ ] JWT Secret 운영값 설정 확인
- [ ] 로그에 민감 정보가 남지 않는지 확인
