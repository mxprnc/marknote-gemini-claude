# Gemini Sub-Agent & Delegation
![Gemini Banner](https://www.gstatic.com/lamda/images/gemini_wordmark_67f677fb2.svg)


### 결론
자료를 조사해봤는데, Gemini 는 Sub Agent 같은 개념이 없다. Gemini 는 Sub Agent 가 단순히 컨텍스트 절약을 위해서만 필요하다고 하는데, Sub Agent 는 비동기적인 작업 수행애도 장점이 있다. 음... Sub Agent 의 비동기 작업 구조와 유사한 뭔가가 있는지는 계속 찾아봐야 할 것 같다.

### 설명
Gemini (특히 Gemini CLI/Antigravity) 환경에서의 'Sub-Agent' 개념은 Claude의 인스턴스 중심 방식과는 다소 차이가 있습니다. Claude가 별도의 독립적인 세션을 가진 '에이전트 인스턴스'를 생성한다면, Gemini는 **'강력한 도구 위임(Tool Delegation)'**과 **'절차적 스킬(Skills)'**을 통해 동일한 목적을 달성합니다.

### 1. 핵심 철학: 인스턴스 복제 vs 도구 위임
Claude의 Sub-Agent가 "분신"을 만드는 것이라면, Gemini의 방식은 "전문가 도구"를 호출하는 것에 가깝습니다.

| 비교 항목 | Claude Sub-Agent | Gemini Sub-Agent (Delegation) |
| --- | --- | --- |
| **핵심 메커니즘** | 새로운 컨텍스트 인스턴스 생성 (`/agents`) | MCP 도구 및 스킬 호출 |
| **컨텍스트 관리** | 서브 에이전트 단위로 컨텍스트 격리 | 1M~2M 이상의 대량 컨텍스트 윈도우 활용 |
| **특성화 방법** | 에이전트 파일 작성 (YAML + MD) | **Skills** (`SKILL.md`) 또는 **MCP 서버** 정의 |
| **대표 예시** | `unit-test-generator` 인스턴스 | `browser_subagent`, `github_mcp` 도구 |

<br/>

### 2. Gemini에서 Sub-Agent 역할을 하는 요소들

#### ① 내장 Sub-Agent (Specialized Tools)
Gemini CLI는 특정 복잡한 작업을 위해 내부적으로 전용 서브 에이전트 성격의 도구를 실행합니다.
- **`browser_subagent`**: 브라우저 조작을 전담하는 실질적인 서브 에이전트입니다. 별도의 브라우징 컨텍스트를 가지며 결과를 메인에 보고합니다.
- **`code_subagent` (Search/Edit)**: 대규모 코드베이스 탐색 및 수정을 위해 최적화된 내부 로직을 사용합니다.

#### ② Skills (Instruction-based Agents)
가장 Claude의 커스텀 에이전트와 유사한 개념입니다. 특정 역할(Persona)이나 표준 작업 절차(SOP)를 정의합니다.
- **폴더 구조**: `.agents/skills/my-skill/SKILL.md`
- **역할**: "너는 이제부터 코드 리뷰 전문가 스킬을 사용해"라고 지시하면, 해당 스킬의 지침에 따라 동작합니다.
- **장점**: 재사용성이 높고, 프로젝트별로 특화된 지침을 관리하기 용이합니다.

#### ③ MCP (Model Context Protocol) Tools
외부 서비스(GitHub, Slack, Jira 등)와 연동되는 강력한 "외부 요원"입니다. 단순히 기능을 실행하는 것을 넘어, 모델이 해당 환경의 맥락을 이해하고 조작할 수 있게 합니다.

<br/>

### 3. Gemini가 Sub-Agent 인스턴스를 덜 사용하는 이유 (Context Power)

Claude에서 서브 에이전트를 쓰는 주된 이유 중 하나는 **'컨텍스트 공간 절약'**입니다. 하지만 Gemini(1.5 Pro)는 **최대 200만 토큰**의 압도적인 컨텍스트 윈도우를 제공합니다.

- **장점**: 모든 탐색 과정과 추론 로그가 하나의 세션에 남으므로, 모델이 전체적인 맥락을 놓치지 않고 더 정확한 최종 결과물을 낼 수 있습니다.
- **관리법**: 컨텍스트가 너무 지저분해지면 `/clear`로 초기화하거나, `/compact`로 핵심 내용만 요약하여 관리합니다. '한 세션 = 한 기능 구현' 원칙을 지키면 서브 에이전트 없이도 매우 정교한 작업이 가능합니다.

<br/>

### 4. 실습: Gemini 식으로 "서브 에이전트" 만들기 (Skills)

Claude의 `/agents` 대신, Gemini에서는 **Skill**을 사용하여 전문 요원을 구성합니다.

**1단계: 스킬 파일 생성**
`file:///.agents/skills/tester/SKILL.md` 위치에 지침을 작성합니다.

```markdown
# Unit Test Specialist Skill
너는 유닛 테스트 생성을 전문으로 하는 에이전트야.

## 지침
1. `Read` 도구로 소스 코드를 분석한다.
2. `Grep`으로 기존 테스트 파일 패턴을 파악한다.
3. 테스트가 누락된 함수를 찾아 테스트 코드를 작성한다.
4. `run_command`로 테스트를 실행하고 성공할 때까지 수정한다.
```

**2단계: 사용하기**
사용자는 메인 채팅창에서 다음과 같이 요청하기만 하면 됩니다.
> "현재 프로젝트에 `tester` 스킬을 적용해서 DB 관련 테스트가 빠진 곳을 찾아 보강해줘."

이때 Gemini는 메인 컨텍스트 내에서 `tester` 스킬의 지침을 '특수 임무'로 인식하고 서브 에이전트처럼 동작합니다.

<br/>

### 5. 요약 및 권장 사항

| 상황 | 추천 방식 |
| --- | --- |
| **단순 명령 실행/외부 연동** | **MCP 도구** 사용 |
| **특정 역할/SOP 수행** | **Skills** (`SKILL.md`) 정의 |
| **완전 격리된 브라우징** | **`browser_subagent`** 호출 |
| **컨텍스트 오염 방지** | **`/clear`** 후 새로운 목표로 세션 시작 |

> **Tip**: Gemini CLI에서는 "어떤 에이전트를 새로 만들까?"보다 **"어떤 도구(MCP)를 연결하고 어떤 지침(Skill)을 줄까?"**를 고민하는 것이 핵심입니다.

---
