# Sub Agent
![alt text](<./img/2-subagent---claude/CleanShot 2026-03-24 at 07.54.51@2x.png>)

sub agent 를 생성하게 되면 메인 클로드 안에서 별도의 컨텍스트를 가진 'sub agent' 라고 부르는 도우미가 생성된다.

서브에이전트는 자기만의 지시사항, 각자가 쓸수 있는 도구들, 각자만의 별도의 권한을 가진다. 에이전트와 멀티에이전트 오케스트레이션 개념은 'Agent engineering 5가지 핵심 개념' 강의에서 다루고 있으니 참고.

### sub agent 가 필요한 이유
메인에서 파일을 100개를 뒤지면 그 결과가 공간을 차지하게 되는데, sub agent 를 이용하면 찾아봤던 결과가 sub agent 에만 남고, 요약결과만 메인에만 남기에 조금 더 컨텍스트를 절약할 수 있다.


### sub agent 장점 4가지
- 병렬 처리 : 여러 작업을 동시에 실행해 전체 소요 시간 단축
- 컨텍스트 보호 : 메인 에이전트의 컨텍스트를 오염시키지 않음
- 전문화 : 각 Sub-Agent에 전문 역할 부여
- 재사용 : 한 번 만든 에이전트를 여러 워크플로우에서 활용


### sub agent 사용시 주의사항
권장
- ✅ 독립적인 작업을 병렬로 분배
- ✅ 명확한 역할과 범위 지정
- ✅ 결과를 메인에서 통합 처리

하지말아야 할 것
- ❌ 의존성 있는 작업을 무리하게 병렬화
- ❌ 하나의 Sub-Agent에 너무 많은 역할
- ❌ Sub-Agent 간 직접 통신 시도 (불가)

⚠️ Sub-Agent는 다른 Sub-Agent를 생성할 수 없습니다. 메인에서 체인으로 연결하세요.

<br/>

### 내장 sub-agent
가급적 sub agent 들은 Opus 같은 고비용 모델을 사용하지 않도록 해야 한다. 가급적 Sonnet, Haiku 를 사용해서 병렬로 처리할 때 컨텍스트를 관리하고 효율적으로 처리하는 것을 추천한다.<br/>

내장 Sub agent 들 중 단순한 작업을 수행하는 Explore, Claude Code Guide 등의 에이전트 들은 저렴한 Haiku같은 모델을 사용한다.<br/>

| Sub-Agent | 모델 | 권한 | 용도 |
| --- | --- | --- | --- |
| **Explore** | Haiku (빠름) | 읽기전용 | 코드 탐색, 파일 검색, 구조 파악 |
| **Plan** | 상속 | 읽기전용 | Plan Mode에서 계획 수립 연구 |
| **General-purpose** | 상속 | 모든 도구 | 탐색+수정 필요한 복잡한 다단계 작업 |
| **Bash** | 상속 | Bash만 | 별도 컨텍스트에서 터미널 명령 실행 |
| **Claude Code Guide** | Haiku | 읽기전용 | Claude Code 기능 질문 답변 |

> (속닥속닥) 장 좋아하면 노로바이러스 걸려요. 작작 드세요.

<br/>

### 커스텀 Agent 만들기
요약
1. `/agents` 입력
2. **Create new agent** 선택
3. **User-level** (모든 프로젝트) 또는 **Project-level** (현재 프로젝트만) 선택
4. **Generate with Claude** 선택 → 에이전트 설명 입력
5. 도구 선택 (Read-only / 전체 도구)
6. 모델 선택 (Sonnet, Opus, Haiku, 상속)
7. 배경색 선택 → 저장 → 즉시 사용 가능

<br/>

실습<br/>
`/agents` 명령을 수행하면, 새로운 agent 를 만드는 절차를 거치게 된다.
![alt text](<./img/2-subagent---claude/CleanShot 2026-03-24 at 08.47.47@2x.png>)
<br/>

Generate with Claude (recommended) 를 선택
![alt text](<./img/2-subagent---claude/CleanShot 2026-03-24 at 08.52.44@2x.png>)
<br/>

에이전트 설명 작성
![alt text](<./img/2-subagent---claude/CleanShot 2026-03-24 at 08.53.55@2x.png>)
<br/>

에이전트 설명 입력
![alt text](<./img/2-subagent---claude/CleanShot 2026-03-24 at 08.55.29@2x.png>)
<br/>

tool 선택 (예제에서는 그냥 All tools 를 선택)
![alt text](<./img/2-subagent---claude/CleanShot 2026-03-24 at 08.56.06@2x.png>)
<br/>

모델 선택 (클로드는 디폴트로 Subnet 이 선택되어 있다.)
![alt text](<./img/2-subagent---claude/CleanShot 2026-03-24 at 08.56.51@2x.png>)
<br/>

배경색 선택
![alt text](<./img/2-subagent---claude/CleanShot 2026-03-24 at 08.58.36@2x.png>)
<br/>

Agent 메모리를 Enable 할지? Enable 선택
![alt text](<./img/2-subagent---claude/CleanShot 2026-03-24 at 08.59.04@2x.png>)
<br/>

메모리를 사용하게끔 하면 에이전트가 자신의 로컬 메모리(Self Local Memory)를 업데이트를 하면서 Self Learning 을 할 수 있게 된다.<br/>
<br/>

다 만들어지면 다음과 같은 결과화면이 나타남
![alt text](<./img/2-subagent---claude/CleanShot 2026-03-24 at 09.01.29@2x.png>)
<br/>

위에서 만들어졌다는 결과화면에 보이는 `unit-test-generator` 를 파일 검색해서 열어보면 다음과 같이 파일이 생성되어 있다.
![alt text](<./img/2-subagent---claude/CleanShot 2026-03-24 at 09.06.04@2x.png>)
<br/>

#### 사용해보기
다음과 같이 unit-test-generator 에이전트를 이용해서 테스트해본다. "'unit-test-generator'(방금 만든 subagent) 를 사용해서 db 관련 unit test 빠진거 있으면 생성해줘"
![alt text](<./img/2-subagent---claude/CleanShot 2026-03-24 at 09.06.34@2x.png>)
<br/>

이렇게 테스트 돌리고 하는 과정이나 결과를 메인컨텍스트에 담아두기에 무겁고, 메인컨텍스트에 담아둘 필요가 없다. 그래서 테스트 같은 것은 무조건 서브 에이전트를 사용하는 것을 추천한다.


DB 관련 코드 중 테스트가 없는 파일 2개를 발견했다. 그리고 `unit-test-generator` 에이전트를 실행하고 있다. 즉, sub agent 의 컨텍스트에서 돌아가는 중이다.
![alt text](<./img/2-subagent---claude/CleanShot 2026-03-24 at 09.09.26@2x.png>)
<br/>

### tip
sub agent 도 skill 처럼 계속 사용하면서 점진적으로 업데이트하고 개선시키고 좋은 에이전트를 만들어나가고 하는 것이 좋다. 

### 에이전트 파일의 형태 및 구조
![alt text](<./img/2-subagent---claude/CleanShot 2026-03-24 at 09.23.54@2x.png>)

- YAML Frontmatter : 에이전트의 메타데이터 (name, model, description, tools 등 설정)
- tools 필드 : 에이전트가 사용할 수 있는 도구를 제한, 불필요한 권한 최소화
- markdown 프롬프트 : 에이전트의 행동 지침을 자연어로 작성. 역할,규칙,체크리스트 등을 포함
- 구조화된 지침 : 마크다운 헤딩,리스트로 명확한 구조 제공, 에이전트가 체계적으로 동작


e.g.
```
# 에이전트 파일 구조 (YAML + Markdown)
---
name: code-reviewer
description: "코드 리뷰 전문가. 품질, 보안, 모범 사례를 검토."
tools: Read, Grep, Glob, Bash
model: sonnet
---

호출되면:
1. git diff로 최근 변경 확인
2. 수정된 파일에 집중하여 리뷰

피드백 우선순위:
- 🔴 크리티컬 (반드시 수정)
- 🟡 경고 (수정 권장)
- 🟢 제안 (개선 고려)
```
<br/>

### 에이전트 저장 위치, 범위
| 어디에 저장? | 어디서 쓸 수 있나? | 언제 이렇게 쓰나? |
| --- | --- | --- |
| `~/.claude/agents/` | **내 모든 프로젝트**에서 사용 가능 | 나만 쓰는 개인용 에이전트 |
| `.claude/agents/` | **이 프로젝트에서만** 사용 가능 | 팀과 공유할 프로젝트 전용 |
| 플러그인의 `agents/` | **플러그인 활성화된 곳**에서만 | 플러그인 제공 에이전트 |
| `--agents` CLI 플래그 | **지금 이 세션에서만** (일회용) | 자동화 스크립트/CI에서 즉석 사용 |

<br/>

agent 를 일회성으로 사용해야 할 경우(e.g. CI/CD)도 굉장히 자주 있을수 있기에 `--agent` 플래그를 사용하는 방식도 기억해두자.
```bash
# CLI에서 즉석으로 에이전트 정의 (일회성)
claude --agents '{
  "quick-reviewer": {
    "description": "빠른 코드 리뷰어",
    "prompt": "변경된 코드를 리뷰하고 핵심 이슈만 보고하세요.",
    "tools": ["Read", "Grep", "Glob"],
    "model": "haiku"
  }
}'
```
<br/>

### Sub Agent 실제 사용 사례
Sub Agent 를 사용할만한 실제 사례가 무엇이 있을지 살펴보면 다음과 같다.
![alt text](<./img/2-subagent---claude/CleanShot 2026-03-24 at 09.32.57@2x.png>)

<br/>

격리
- 메인 클로드하고 격리된 상황에서 작업을 수행하는 케이스
- 메인 클로드에 영향을 주지 않아야 할 경우에 서브에이전트의 '격리'된 실행을 보장한다는 점을 활용하는 것을 고려해볼 수 있다.

병렬
- 여러 에이전트를 동시에 실행해야 할 경우

체인
- 한 에이전트의 결과를 다음 에이전트에 전달해서 순차 처리해야 할때
- e.g. 분석 -> 리팩터 -> 테스트 -> 리뷰

재개 (resume)
- 이전 에이전트의 컨텍스트를 이어받아서 작업을 진행할 경우

### 메인 대화 vs Sub Agent
위에서 이미 모두 봤던 내용이긴 하지만, 다시 한번 sub agent 를 언제 사용할지 정리해보면 다음과 같다.

![alt text](<./img/2-subagent---claude/CleanShot 2026-03-24 at 09.41.13@2x.png>)

## 참고) Agent Teams
Sub Agent 의 기능을 넘어서는 기능이 있는데, 'Agent Teams' 라는 기능이다.<br/>
![alt text](<./img/2-subagent---claude/CleanShot 2026-03-24 at 09.43.04@2x.png>)

SubAgent 는 각각의 Sub Agent 가 메인 Agent 하고만 통신을 하는 구조이지만, Agent Teams 는 여러 에이전트가 서로 통신하면서 작업을 수행하는 구조이다.

<br/>