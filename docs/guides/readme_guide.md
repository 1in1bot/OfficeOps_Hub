# README.md 작성 가이드

README.md는 프로젝트를 처음 보는 사람이 프로젝트의 목적을 이해하고, 설치하고, 실행하고, 사용 여부를 판단하고, 필요하다면 기여할 수 있게 돕는 첫 문서입니다.

좋은 README는 단순히 정보를 많이 담는 문서가 아니라, 독자가 가장 먼저 궁금해할 질문에 빠르게 답하는 문서입니다.

## README의 핵심 목표

- 이 프로젝트가 무엇인지 설명한다.
- 왜 필요한지 설명한다.
- 어떻게 설치하고 실행하는지 알려준다.
- 어떻게 사용하는지 예시로 보여준다.
- 문제가 생겼을 때 어디서 도움을 받을 수 있는지 안내한다.
- 협업자나 오픈소스 기여자가 어떻게 참여할 수 있는지 알려준다.

## 기본 정보

README의 첫 부분에는 프로젝트의 정체성이 명확히 드러나야 합니다.

포함하면 좋은 내용:

- 프로젝트 이름
- 한 줄 소개
- 프로젝트가 해결하는 문제
- 주요 기능
- 대상 사용자
- 현재 상태: 실험용, 베타, 운영 가능, 유지보수 중단 등
- 데모 링크
- 스크린샷, GIF, 영상
- 배지: 빌드 상태, 테스트 상태, 배포 버전, 라이선스 등

예시:

````md
# Project Name

One-line description of what this project does.

## Overview

This project helps users solve ...
````

## 빠른 시작

빠른 시작 섹션은 README에서 가장 중요합니다. 사용자가 가능한 한 적은 단계로 프로젝트를 실행해볼 수 있어야 합니다.

포함하면 좋은 내용:

- 요구사항
- 설치 방법
- 환경 변수 설정
- 실행 명령어
- 최소 사용 예제
- 예상 출력 결과
- 로컬 서버 주소

예시:

````md
## Quick Start

### Requirements

- Node.js 20+
- npm 10+

### Installation

```bash
git clone https://github.com/example/project.git
cd project
npm install
```

### Run

```bash
npm run dev
```

Open http://localhost:3000 in your browser.
````

## 사용법

사용법 섹션은 프로젝트를 실행한 뒤 실제로 어떻게 활용하는지 보여주는 부분입니다.

포함하면 좋은 내용:

- 기본 사용 예시
- 주요 명령어
- CLI 옵션
- API 사용 예시
- 설정 파일 예시
- 고급 사용법
- 실제 시나리오별 예제
- 제한사항과 주의사항

예시:

````md
## Usage

```bash
project-cli build --input ./src --output ./dist
```

### Options

| Option | Description |
| ------ | ----------- |
| `--input` | Source directory |
| `--output` | Output directory |
````

## 프로젝트 구조

프로젝트가 커질수록 파일 구조를 설명하는 섹션이 도움이 됩니다.

포함하면 좋은 내용:

- 주요 폴더 구조
- 핵심 모듈 설명
- 아키텍처 개요
- 데이터 흐름
- 주요 의존성
- 외부 서비스 연동 방식

예시:

````md
## Project Structure

```txt
src/
  components/
  pages/
  services/
tests/
docs/
```

| Path | Description |
| ---- | ----------- |
| `src/components` | Reusable UI components |
| `src/services` | External API integrations |
| `tests` | Automated tests |
````

## 개발 환경

협업 프로젝트라면 개발자가 로컬에서 작업할 수 있는 절차가 필요합니다.

포함하면 좋은 내용:

- 개발 환경 세팅
- 의존성 설치
- 개발 서버 실행
- 테스트 실행
- 린트와 포맷 실행
- 빌드 방법
- 디버깅 방법
- 코드 스타일
- 브랜치 전략
- 커밋 컨벤션

예시:

````md
## Development

```bash
npm install
npm run dev
npm test
npm run lint
npm run build
```
````

## 설정

환경 변수나 설정 파일이 필요한 프로젝트라면 반드시 별도 섹션으로 정리하는 것이 좋습니다.

포함하면 좋은 내용:

- 환경 변수 목록
- 필수/선택 여부
- 기본값
- 설명
- 보안상 노출하면 안 되는 값
- 로컬/스테이징/프로덕션 설정 차이

예시:

````md
## Configuration

| Name | Required | Default | Description |
| ---- | -------- | ------- | ----------- |
| `DATABASE_URL` | Yes | - | Database connection string |
| `PORT` | No | `3000` | Local server port |
| `API_KEY` | Yes | - | External service API key |
````

## API 문서

API 서버, SDK, 라이브러리라면 인터페이스 설명이 필요합니다.

포함하면 좋은 내용:

- 엔드포인트 목록
- 요청 예시
- 응답 예시
- 인증 방식
- 에러 코드
- 웹훅
- SDK 사용법
- 타입 또는 스키마 링크
- OpenAPI/Swagger 문서 링크

예시:

````md
## API

### `GET /users/:id`

Request:

```bash
curl http://localhost:3000/users/1
```

Response:

```json
{
  "id": 1,
  "name": "Ada"
}
```
````

## 배포와 운영

운영되는 서비스라면 배포 및 운영 관련 정보가 필요합니다.

포함하면 좋은 내용:

- 배포 환경
- Docker 사용법
- Docker Compose 예시
- CI/CD 설명
- 데이터베이스 마이그레이션
- 모니터링
- 로그 확인
- 백업과 복구
- 성능 튜닝
- 운영 시 주의사항

예시:

````md
## Deployment

```bash
docker compose up -d
```

Run database migrations:

```bash
npm run migrate
```
````

## 테스트

테스트 방법은 프로젝트 신뢰도를 높이고, 기여자가 변경사항을 검증할 수 있게 합니다.

포함하면 좋은 내용:

- 테스트 종류
- 테스트 실행 명령어
- 테스트 데이터 준비
- 커버리지 확인
- E2E 테스트 방법
- CI에서 테스트가 실행되는 방식
- 테스트 작성 규칙

예시:

````md
## Testing

```bash
npm test
npm run test:e2e
npm run coverage
```
````

## 문제 해결

초기 설치나 실행 과정에서 자주 발생하는 문제는 README에 정리해두면 좋습니다.

포함하면 좋은 내용:

- 자주 발생하는 오류
- 원인
- 해결 방법
- 관련 이슈 링크
- 로그 확인 방법

예시:

````md
## Troubleshooting

### Port already in use

Change the `PORT` value in `.env`.

```bash
PORT=3001 npm run dev
```
````

## 보안

프로젝트가 인증, 데이터, API 키, 사용자 정보를 다룬다면 보안 안내가 필요합니다.

포함하면 좋은 내용:

- 인증/권한 구조
- 비밀키 관리 방식
- 취약점 신고 방법
- 보안 정책 링크
- 권한 요구사항
- 데이터 보호 관련 주의사항
- 알려진 보안 제한사항

예시:

````md
## Security

Please do not report security vulnerabilities through public GitHub issues.
See [SECURITY.md](SECURITY.md) for responsible disclosure instructions.
````

## 기여 안내

오픈소스 또는 팀 협업 프로젝트라면 기여 방법을 명확히 안내해야 합니다.

포함하면 좋은 내용:

- 기여 방법
- 이슈 작성 방법
- PR 작성 방법
- 로컬 개발 절차
- 코드 리뷰 기준
- 좋은 첫 이슈 링크
- 행동 강령 링크
- `CONTRIBUTING.md` 링크

예시:

````md
## Contributing

Contributions are welcome.

1. Fork this repository.
2. Create a feature branch.
3. Make your changes.
4. Run tests.
5. Open a pull request.

See [CONTRIBUTING.md](CONTRIBUTING.md) for details.
````

## 라이선스와 크레딧

프로젝트 사용 조건과 출처를 명확히 해야 합니다.

포함하면 좋은 내용:

- 라이선스
- 저작권 정보
- 사용한 오픈소스 크레딧
- 서드파티 리소스 출처
- 데이터셋 출처
- 논문 또는 연구 인용 방법
- 감사 인사

예시:

````md
## License

This project is licensed under the MIT License.
See [LICENSE](LICENSE) for details.
````

## 지원과 커뮤니티

사용자가 문제를 겪거나 질문이 있을 때 어디로 가야 하는지 알려줘야 합니다.

포함하면 좋은 내용:

- 공식 문서 링크
- FAQ
- GitHub Issues
- GitHub Discussions
- Discord, Slack, 이메일 등
- 버그 리포트 위치
- 기능 요청 위치
- 유지보수자 정보
- 로드맵 링크
- 릴리즈 노트 또는 Changelog 링크

예시:

````md
## Support

- Documentation: https://example.com/docs
- Bug reports: GitHub Issues
- Questions: GitHub Discussions
- Community: Discord
````

## 프로젝트 규모별 추천 구성

### 작은 개인 프로젝트

최소한 아래 정도면 충분합니다.

````md
# Project Name

Short description.

## Installation

## Usage

## License
````

### 라이브러리 또는 CLI 도구

설치와 사용 예시가 특히 중요합니다.

````md
# Project Name

## Overview
## Features
## Installation
## Quick Start
## Usage
## API or CLI Reference
## Configuration
## Testing
## Contributing
## License
````

### 웹 애플리케이션 또는 서비스

실행, 환경 변수, 배포 정보가 중요합니다.

````md
# Project Name

## Overview
## Demo
## Features
## Requirements
## Quick Start
## Configuration
## Project Structure
## Development
## Testing
## Deployment
## Troubleshooting
## Security
## Contributing
## License
````

### 오픈소스 프로젝트

기여, 보안, 커뮤니티 안내까지 포함하는 것이 좋습니다.

````md
# Project Name

## Overview
## Features
## Quick Start
## Documentation
## Usage
## Contributing
## Code of Conduct
## Security
## Roadmap
## Support
## License
````

## README 작성 체크리스트

- 프로젝트 이름과 한 줄 소개가 첫 화면에 보이는가?
- 프로젝트가 해결하는 문제가 명확한가?
- 설치 명령어가 복사해서 실행 가능한가?
- 최소 실행 예제가 있는가?
- 예상 결과나 스크린샷이 있는가?
- 환경 변수가 표로 정리되어 있는가?
- 테스트 실행 방법이 있는가?
- 배포 방법이 있는가?
- 문제가 생겼을 때 볼 수 있는 Troubleshooting 섹션이 있는가?
- 기여 방법이 있는가?
- 라이선스가 명시되어 있는가?
- 문서, 이슈, 커뮤니티 링크가 있는가?
- 너무 긴 설명은 별도 문서로 분리되어 있는가?
- 링크가 실제로 동작하는가?
- 코드 예제가 최신 상태인가?

## 좋은 README의 특징

- 첫 10초 안에 프로젝트의 목적을 이해할 수 있다.
- 첫 1분 안에 설치와 실행 방법을 찾을 수 있다.
- 코드 예제가 실제로 동작한다.
- 긴 설명보다 명확한 예제가 많다.
- 표, 코드 블록, 목록을 사용해 빠르게 훑어볼 수 있다.
- 자세한 문서는 README에 모두 넣지 않고 별도 문서로 연결한다.
- 유지보수 상태, 지원 채널, 라이선스가 분명하다.

## 최종 템플릿

아래 템플릿을 프로젝트 성격에 맞게 줄이거나 확장해서 사용할 수 있습니다.

````md
# Project Name

One-line description of the project.

## Overview

Explain what this project does, why it exists, and who it is for.

## Features

- Feature A
- Feature B
- Feature C

## Demo

Add a screenshot, GIF, video, or live demo link.

## Requirements

- Runtime or language version
- Package manager
- External services
- API keys

## Quick Start

```bash
git clone https://github.com/example/project.git
cd project
npm install
npm run dev
```

## Usage

```bash
example command
```

## Configuration

| Name | Required | Default | Description |
| ---- | -------- | ------- | ----------- |
| `DATABASE_URL` | Yes | - | Database connection string |
| `PORT` | No | `3000` | Server port |

## Project Structure

```txt
src/
tests/
docs/
```

## Development

```bash
npm test
npm run lint
npm run build
```

## Deployment

Describe how to deploy this project.

## Troubleshooting

Document common problems and solutions.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## Security

See [SECURITY.md](SECURITY.md).

## Support

Explain where users can ask questions or report issues.

## License

This project is licensed under the MIT License.
````
