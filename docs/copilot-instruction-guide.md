# Copilot CLI Instruction 작성 가이드

> Copilot CLI(Coding Agent)에게 작업을 맡길 때 "요구사항 설명" 방식보다  
> **"실행 계약(Contract)"** 형태로 지시하는 것이 가장 효과적입니다.

---

## 1. Instruction 7대 원칙

| # | 항목 | 설명 |
|---|------|------|
| 1 | **목표 1줄** | 이번 작업으로 완성해야 할 것을 한 문장으로 |
| 2 | **범위 명확화** | 수정 가능 경로 / 절대 수정 금지 경로를 명시 |
| 3 | **기술 제약** | 언어·프레임워크·버전·아키텍처 규칙 |
| 4 | **품질 게이트** | 반드시 통과해야 할 lint / test / build 명령 |
| 5 | **보안/운영 규칙** | 비밀정보·인증·로그·권한 처리 기준 |
| 6 | **완료 조건(DoD)** | PR이 "완료" 판정을 받으려면 충족해야 할 체크리스트 |
| 7 | **출력 형식** | 변경 요약·영향도·테스트 결과 보고 형식 |

---

## 2. 각 원칙 상세

### 2-1. 목표 1줄

```
[목표] <동사> <대상> so that <사용자 가치>
예) "회원 비밀번호 변경 API를 구현한다. so that 사용자가 마이페이지에서 직접 변경할 수 있다."
```

- 모호한 단어(`개선`, `처리`, `고려`) 금지 — 측정 가능한 단어 사용
- 목표가 2개 이상이면 작업을 분리할 것

### 2-2. 범위 명확화

```markdown
## 변경 허용 경로
- src/features/auth/**
- src/api/v1/users/**
- tests/unit/auth/**

## 변경 금지 경로
- src/database/migrations/**   # DBA 승인 필요
- infra/**                     # 인프라팀 전담
- src/core/security/**         # 보안팀 리뷰 없이 수정 불가
```

### 2-3. 기술 제약

```markdown
## 기술 스택
- Language: TypeScript 5.x (strict mode)
- Runtime: Node.js 20 LTS
- Framework: Express 4.x
- ORM: TypeORM 0.3.x
- Test: Jest + supertest

## 아키텍처 규칙
- Controller → Service → Repository 3계층 준수
- 외부 의존성은 반드시 interface로 추상화
- 환경변수는 src/config/env.ts를 통해서만 접근
```

### 2-4. 품질 게이트

```markdown
## 완료 전 필수 통과 명령
- `npm run lint`        — ESLint 에러 0건
- `npm run typecheck`   — TypeScript 컴파일 에러 0건
- `npm test`            — 모든 테스트 통과 (커버리지 80% 이상)
- `npm run build`       — 프로덕션 빌드 성공
```

### 2-5. 보안/운영 규칙

```markdown
## 보안 필수 항목
- 시크릿(API 키, 비밀번호)은 코드에 하드코딩 금지 → 환경변수 사용
- SQL 쿼리는 반드시 파라미터 바인딩 사용 (raw query 금지)
- 사용자 입력값은 class-validator로 유효성 검사 후 사용
- 인증이 필요한 엔드포인트는 AuthGuard 미들웨어 적용 필수

## 로그 규칙
- 개인정보(이름, 이메일, 전화번호)는 로그에 출력 금지
- 에러 로그는 반드시 error ID를 포함할 것
```

### 2-6. 완료 조건(DoD)

```markdown
## Definition of Done 체크리스트
- [ ] 기능 구현 완료
- [ ] 단위 테스트 작성 (신규 코드 커버리지 80%+)
- [ ] lint / typecheck / build / test 전부 통과
- [ ] API 변경 시 OpenAPI 스펙(swagger) 업데이트
- [ ] 보안 규칙 준수 확인
- [ ] PR 설명에 "변경 요약 / 영향 범위 / 테스트 방법" 기재
```

### 2-7. 출력 형식

```markdown
## PR 본문 필수 섹션
### 변경 요약
(무엇을 왜 변경했는가)

### 영향 범위
(어떤 기능/API/DB가 영향을 받는가)

### 테스트 결과
(통과한 테스트 목록 + 커버리지 수치)

### 검토 요청 사항
(리뷰어가 특히 확인해줬으면 하는 부분)
```

---

## 3. Copilot CLI에서 효과적인 지시 방식

### 작게 나누기
- ❌ "사용자 인증 시스템 전체를 구현해줘"
- ✅ "JWT 발급 함수만 구현해줘 (로그인 API 연동은 다음 작업)"

### 금지 사항 명시
- ❌ (아무 제약 없이) "API를 수정해줘"
- ✅ "이 API를 수정하되, 응답 스키마는 변경하지 마. 기존 클라이언트가 깨진다."

### 검증 명령 지정
- ❌ "테스트가 잘 됐는지 확인해"
- ✅ "`npm test -- --coverage` 실행 결과를 PR에 붙여줘. 실패한 테스트가 있으면 수정 후 재시도."

### 결과 보고 강제
- ❌ "완료되면 PR 올려줘"
- ✅ "PR 본문에 위 DoD 체크리스트를 복사해서 완료된 항목에 체크 표시해줘."

---

## 4. 템플릿 빠른 시작

웹 사내 서비스에 특화된 전체 템플릿 →  
[`docs/web-service-instruction-template.md`](./web-service-instruction-template.md)

Skills 추가 방법 →  
[`docs/skills-guide.md`](./skills-guide.md)
