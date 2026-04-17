# bluewingNCAIAgent
* 사내 계정으로 만든 저장소 프로젝트
* 최신 AI 개발 방법을 학습하기 위한 개인 프로젝트
* 아주 신세계를 경험함.

## 어떻게 사용하나요? (How to Use)

### 1. GitHub Copilot 활성화
- VS Code에서 GitHub Copilot 확장 프로그램을 설치합니다.
- GitHub 계정으로 로그인하여 Copilot을 활성화합니다.

### 2. Copilot으로 코드 자동 완성 사용하기
- 코드 편집기에서 코드를 작성하면 Copilot이 자동으로 제안을 제공합니다.
- `Tab` 키를 눌러 제안을 수락하거나, `Esc` 키를 눌러 무시할 수 있습니다.
- 여러 제안을 확인하려면 `Alt+]` (다음) 또는 `Alt+[` (이전) 을 사용합니다.

### 3. Copilot Chat 사용하기
- VS Code에서 Copilot Chat 패널을 열어 자연어로 질문하세요.
- 예시: "이 함수를 최적화해줘", "단위 테스트를 작성해줘"
- 코드를 선택한 뒤 우클릭 → "Copilot" 메뉴를 통해 빠르게 접근할 수 있습니다.

### 4. Copilot Coding Agent 사용하기
- GitHub 이슈에 작업 내용을 작성합니다.
- 이슈에서 Copilot을 assignee로 지정하면 Copilot이 자동으로 코드 변경을 수행하고 Pull Request를 생성합니다.
- Pull Request를 리뷰하고 피드백을 남기면 Copilot이 반영합니다.

### 5. Copilot CLI로 사내 서비스 개발하기

Agent에게 웹 기반 사내 서비스 개발을 맡길 때의 가이드입니다.

| 문서 | 설명 |
|------|------|
| [Copilot CLI Instruction 작성 가이드](docs/copilot-instruction-guide.md) | Instruction 7대 원칙 및 효과적인 지시 방법 |
| [웹 사내 서비스 Instruction 템플릿](docs/web-service-instruction-template.md) | GitHub 이슈에 바로 붙여 쓸 수 있는 전체 템플릿 |
| [Skills 추가 가이드](docs/skills-guide.md) | 반복 작업을 Skill로 표준화하는 방법 |

### 6. 참고 자료
- [GitHub Copilot 공식 문서](https://docs.github.com/copilot)
- [VS Code Copilot 확장 프로그램](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot)
