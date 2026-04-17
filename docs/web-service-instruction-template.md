# 웹 사내 서비스 개발 — Copilot CLI Instruction 템플릿

> 이 파일을 복사해서 GitHub 이슈 본문에 붙여 넣고 괄호 안을 채워 사용하세요.  
> Copilot을 Assignee로 지정하면 자동으로 작업을 시작합니다.

---

## [목표]

<!-- 한 문장으로 작성. 측정 가능한 동사를 사용하세요. -->
`(동사) (대상)` so that `(사용자 가치)`.

예시: `회원 비밀번호 변경 API(POST /api/v1/users/me/password)를 구현한다` so that `사용자가 마이페이지에서 직접 비밀번호를 변경할 수 있다`.

---

## [변경 허용 범위]

```
# 수정 가능한 경로 (실제 경로로 교체하세요)
src/features/(기능명)/**
src/api/v1/(엔드포인트)/**
tests/unit/(기능명)/**
tests/integration/(기능명)/**
```

## [변경 금지 범위]

```
# 건드리지 말아야 할 경로
src/database/migrations/**    # DBA 승인 필요
infra/**                      # 인프라팀 전담
src/core/security/**          # 보안팀 리뷰 필수
```

---

## [기술 스택 및 아키텍처]

```
- Language  : (예: TypeScript 5.x, strict mode)
- Runtime   : (예: Node.js 20 LTS)
- Framework : (예: Express 4.x / NestJS 10.x)
- ORM/DB    : (예: TypeORM 0.3.x + PostgreSQL 15)
- Test      : (예: Jest + supertest)
- 아키텍처  : Controller → Service → Repository 3계층
- 환경변수  : src/config/env.ts 를 통해서만 접근
- 외부 의존 : 반드시 interface로 추상화 후 주입
```

---

## [웹 사내 서비스 필수 항목]

### 인증/인가
- [ ] 인증 방식: `(SSO / JWT / Session 중 선택)`
- [ ] 인가 방식: `(RBAC / ABAC 중 선택)` — 필요 권한: `(예: ROLE_USER, ROLE_ADMIN)`
- [ ] 인증이 필요한 엔드포인트에는 `AuthGuard` 미들웨어 적용 필수
- [ ] 감사 로그: 민감 작업(생성/수정/삭제)은 `AuditLog` 서비스 호출 필수

### 개인정보/민감정보
- [ ] 이름·이메일·전화번호·주민번호 등 PII는 응답 로그에 출력 금지
- [ ] DB 저장 시 암호화 필요 항목: `(해당 필드 명시)`
- [ ] API 응답에서 민감 필드는 마스킹 처리: `(예: phoneNumber → "010-****-5678")`

### DB/API 스키마 변경
- [ ] DB 스키마 변경 허용 여부: `(허용 / 금지 — 금지 시 이유 기재)`
  - 허용 시: 마이그레이션 파일 `src/database/migrations/` 에 추가할 것
- [ ] 기존 API 응답 스키마 변경 허용 여부: `(허용 / 금지 — 금지 시 Breaking Change 없이 구현)`

### 장애 대응
- [ ] 외부 API 호출에는 timeout + retry (최대 3회) 적용
- [ ] Feature Flag 필요 여부: `(필요 / 불필요)`
- [ ] 롤백 계획: `(예: 이전 배포 버전으로 즉시 롤백 가능하도록 DB 변경 하위 호환 유지)`

### UI/접근성 (프론트엔드 작업 시)
- [ ] 지원 브라우저: `(예: Chrome 최신, Edge 최신, 모바일 미지원)`
- [ ] 접근성 기준: `(예: WCAG 2.1 AA 이상)`
- [ ] 디자인 시스템: `(예: 사내 공통 컴포넌트 라이브러리 사용, 임의 스타일 추가 금지)`

---

## [이번 작업 제외 항목] ← scope creep 방지

```
# 이번 PR에서 다루지 않을 것
- (예: 이메일 발송 기능 — 별도 이슈 #123에서 처리)
- (예: 관리자 화면 — 백오피스 팀 전담)
- (예: 성능 최적화 — 기능 구현 완료 후 별도 프로파일링)
```

---

## [품질 게이트]

아래 명령이 **모두 성공**해야 PR이 완료된 것으로 간주합니다.

```bash
npm run lint        # ESLint 에러 0건
npm run typecheck   # TypeScript 컴파일 에러 0건
npm test            # 전체 테스트 통과, 신규 코드 커버리지 80%+
npm run build       # 프로덕션 빌드 성공
```

---

## [보안 필수 준수]

- 시크릿(API 키, DB 비밀번호)은 절대 코드에 하드코딩 금지 — 환경변수 사용
- SQL은 반드시 ORM 파라미터 바인딩 사용 (raw query 직접 작성 금지)
- 사용자 입력값은 `class-validator` 로 유효성 검사 후 사용
- HTTP 보안 헤더(`helmet`)는 전역 미들웨어로 이미 적용되어 있음 — 제거 금지

---

## [완료 조건 (Definition of Done)]

PR을 열 때 아래 체크리스트를 본문에 복사하고, 완료된 항목에 체크해 주세요.

```markdown
## DoD 체크리스트
- [ ] 기능 구현 완료
- [ ] 단위 테스트 작성 (신규 코드 커버리지 80%+)
- [ ] `npm run lint` 통과
- [ ] `npm run typecheck` 통과
- [ ] `npm test` 통과
- [ ] `npm run build` 통과
- [ ] API 변경 시 OpenAPI(Swagger) 스펙 업데이트
- [ ] 인증/인가 규칙 적용 확인
- [ ] PII 마스킹/로그 노출 없음 확인
- [ ] 감사 로그 호출 확인 (해당 시)
- [ ] PR 본문에 "변경 요약 / 영향 범위 / 테스트 결과" 기재
```

---

## [PR 본문 형식]

PR을 생성할 때 아래 형식을 반드시 사용해 주세요.

```markdown
### 변경 요약
(무엇을 왜 변경했는가)

### 영향 범위
(어떤 기능 / API 경로 / DB 테이블이 영향을 받는가)

### 테스트 결과
(통과한 테스트 수 + 커버리지 수치 + 실행 명령)

### 검토 요청 사항
(리뷰어가 특히 확인해줬으면 하는 부분)
```
