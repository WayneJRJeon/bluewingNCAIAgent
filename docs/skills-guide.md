# Copilot Skills 추가 가이드

> 이 문서는 팀의 반복 작업을 Copilot Skill로 표준화하는 방법을 설명합니다.  
> Skill은 "많이"보다 **"자주 쓰는 고품질 소수"** 가 훨씬 효율적입니다.

---

## 1. Skill이란?

Skill은 Copilot CLI가 반복적으로 수행하는 작업 패턴을 **재사용 가능한 단위**로 묶은 것입니다.

| 구분 | 설명 |
|------|------|
| **일반 Instruction** | 단건 작업용, 매번 새로 작성 |
| **Skill** | 팀 전체가 반복하는 작업 패턴을 미리 정의해 둔 것 |

---

## 2. 어떤 작업을 Skill로 만들 것인가?

반복성과 리스크를 기준으로 후보를 선정합니다.

```
우선순위 = 반복 빈도 × 실수 발생 리스크
```

**좋은 Skill 후보 예시**

| Skill 이름 | 이유 |
|------------|------|
| REST API 엔드포인트 스캐폴딩 | 매 기능마다 반복, 실수 패턴이 고정 |
| 사내 SSO 인증 미들웨어 적용 | 보안 필수 항목, 빠뜨리면 취약점 발생 |
| TypeORM 마이그레이션 파일 생성 | 포맷 실수가 잦고 형식이 엄격함 |
| Jest 테스트 파일 스캐폴딩 | 매 파일마다 반복, 구조가 표준화되어 있음 |
| 배포 전 품질 게이트 검증 | 누락 시 운영 장애 발생 가능 |

---

## 3. Skill 정의 5요소

Skill을 문서화할 때 아래 5가지를 반드시 고정하세요.

### 3-1. 사용 목적 (Purpose)
이 Skill이 해결하는 문제를 1~2줄로 정의합니다.

### 3-2. 입력/출력 (Input / Output)
Skill 실행에 필요한 파라미터와 결과물을 명확히 정의합니다.

### 3-3. 적용 조건 / 비적용 조건 (When to use / not use)
언제 써야 하고, 언제 쓰면 안 되는지 명시합니다.

### 3-4. 실패 시 처리 규칙 (Error Handling)
Skill 실행 중 오류가 발생했을 때 Copilot이 어떻게 행동해야 하는지 지정합니다.

### 3-5. 보안 가드레일 (Security Guard)
이 Skill이 생성하는 코드에서 절대 해서는 안 되는 것을 명시합니다.

---

## 4. Skill 템플릿

아래 템플릿을 `docs/skills/` 폴더에 파일로 추가해 버전 관리하세요.

```markdown
# Skill: (Skill 이름)

버전: 1.0.0
최종 수정: YYYY-MM-DD
작성자: (팀명 또는 개인)

---

## 사용 목적
(이 Skill이 해결하는 문제를 1~2줄로)

## 입력
| 파라미터 | 타입 | 필수 | 설명 |
|----------|------|------|------|
| (이름)   | string | ✅ | (설명) |
| (이름)   | string | ⬜ | (설명, 기본값: xxx) |

## 출력 (생성되는 파일 및 변경 사항)
- `(경로)/(파일명)` — (설명)
- `(경로)/(파일명)` — (설명)

## 적용 조건
- (언제 이 Skill을 써야 하는가)

## 비적용 조건
- (언제 이 Skill을 쓰면 안 되는가)

## 실패 시 처리
- (오류 유형): (Copilot이 해야 할 행동)
- 예) 파일이 이미 존재하는 경우: 덮어쓰지 말고 작업을 중단한 뒤 사용자에게 알릴 것

## 보안 가드레일
- (절대 하면 안 되는 것 1)
- (절대 하면 안 되는 것 2)

## 사용 예시
(이슈 또는 Copilot 지시 예시 텍스트)
```

---

## 5. Skill 예시: REST API 엔드포인트 스캐폴딩

```markdown
# Skill: REST API 엔드포인트 스캐폴딩

버전: 1.0.0
최종 수정: 2026-04-17

---

## 사용 목적
새로운 REST API 엔드포인트를 추가할 때 Controller / Service / Repository /
테스트 파일을 일관된 구조로 자동 생성합니다.

## 입력
| 파라미터    | 타입   | 필수 | 설명                          |
|-------------|--------|------|-------------------------------|
| featureName | string | ✅   | 기능명 (예: user, product)    |
| httpMethod  | string | ✅   | GET / POST / PUT / PATCH / DELETE |
| path        | string | ✅   | API 경로 (예: /api/v1/users)  |
| authRequired| boolean| ✅   | 인증 필요 여부                |

## 출력
- `src/features/(featureName)/(featureName).controller.ts`
- `src/features/(featureName)/(featureName).service.ts`
- `src/features/(featureName)/(featureName).repository.ts`
- `src/features/(featureName)/dto/(featureName).dto.ts`
- `tests/unit/(featureName)/(featureName).service.test.ts`

## 적용 조건
- 신규 기능의 첫 엔드포인트를 만들 때

## 비적용 조건
- 기존 파일이 이미 존재하는 경우 (기존 코드를 덮어쓸 위험)
- DB 스키마 변경이 함께 필요한 경우 (마이그레이션 Skill 별도 사용)

## 실패 시 처리
- 동명의 파일이 이미 존재하는 경우: 덮어쓰지 말고 중단 후 사용자에게 보고
- DTO 유효성 규칙이 불명확한 경우: 임의로 추가하지 말고 TODO 주석 삽입 후 보고

## 보안 가드레일
- authRequired=true인 경우 반드시 AuthGuard 미들웨어 적용
- DTO에는 반드시 class-validator 데코레이터 적용 (plain object 사용 금지)
- 외부 입력값을 SQL에 직접 삽입하는 코드 생성 금지

## 사용 예시
> Skill: REST API 엔드포인트 스캐폴딩
> featureName=user, httpMethod=POST, path=/api/v1/users/me/password, authRequired=true
> 비밀번호 변경 기능에 필요한 파일 세트를 생성해 주세요.
```

---

## 6. Skill 도입 프로세스 (권장)

```
1. 반복 작업 식별  →  2. Skill 초안 작성  →  3. 파일럿 (소규모 적용)
         ↓
4. 결과 검토 (품질/보안 이슈 확인)  →  5. 표준화 & docs/skills/ 에 등록
         ↓
6. 팀 공유 & README 링크 추가  →  7. 주기적 리뷰 (분기 1회)
```

**파일럿 원칙**: 처음엔 실제 코드가 아닌 **샌드박스 브랜치**에서 검증하고, 품질 기준을 통과한 패턴만 표준화합니다.

---

## 7. Skill 버전 관리 규칙

| 변경 유형 | 버전 올리는 방법 | 예시 |
|-----------|-----------------|------|
| 하위 호환 기능 추가 | Minor 업 | 1.0.0 → 1.1.0 |
| 기존 입/출력 변경 | Major 업 | 1.1.0 → 2.0.0 |
| 보안 가드레일 강화 | Patch 업 | 1.1.0 → 1.1.1 |
| 오타/설명 수정 | Patch 업 | 1.1.0 → 1.1.1 |

---

## 8. 현재 등록된 Skills

| Skill | 버전 | 파일 |
|-------|------|------|
| (첫 번째 Skill 추가 후 여기에 기재) | - | - |

---

> **다음 단계**: 팀에서 가장 자주 반복하는 작업 1개를 골라  
> 위 템플릿으로 첫 번째 Skill 파일을 `docs/skills/` 에 추가해 보세요.
