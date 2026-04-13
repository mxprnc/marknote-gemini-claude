**Ralph Loop**(또는 Ralph Wiggum Loop)는 개발자 **Geoffrey Huntley**가 고안한 AI 에이전트 반복 실행 기술(Pattern)로, 단일 도구라기보다 하나의 방법론에 가깝습니다.

가장 "공식"에 가까운 원조 가이드와 대표적인 구현체들은 다음과 같습니다:

### 1. 원조 가이드 및 철학 (Geoffrey Huntley)
이 기법을 처음 정의하고 전파한 Geoffrey Huntley의 공식 레포지토리입니다.
* **GitHub:** [ghuntley/how-to-ralph-wiggum](https://github.com/ghuntley/how-to-ralph-wiggum)
* **특징:** Ralph Loop의 핵심 철학인 "Bash loop를 이용한 무한 반복 및 외부 검증(External Verification)"에 대한 상세한 가이드와 예시가 포함되어 있습니다.

<br/>

---

### 2. 주요 오픈소스 구현체 (CLI 및 SDK)
Ralph Loop 기법을 사용하기 쉽게 도구화한 프로젝트들입니다. 사용자가 찾는 "공식 앱"은 보통 이 중 하나일 가능성이 높습니다.

* **Ralph CLI (by Ian Nuttall):** 현재 가장 대중적으로 쓰이는 CLI 도구입니다.
    * **GitHub:** [iannuttall/ralph](https://github.com/iannuttall/ralph)
* **Ralph Loop Agent (by Vercel Labs):** Vercel AI SDK 환경에서 Ralph 패턴을 구현한 버전입니다.
    * **GitHub:** [vercel-labs/ralph-loop-agent](https://github.com/vercel-labs/ralph-loop-agent)
* **Claude Code Plugin:** Anthropic의 Claude Code 내에서 공식 플러그인 형태로도 제공됩니다.
    * Claude Code 터미널에서 `/plugin add ralph` 명령어로 설치 가능합니다.
* **Gemini CLI Extension:** Gemini CLI에서 `AfterAgent` 훅을 사용하여 Ralph 패턴을 구현한 공식 스타일 확장 프로그램입니다.
    * **GitHub:** [gemini-cli-extensions/ralph](https://github.com/gemini-cli-extensions/ralph)
    * `gemini extensions install https://github.com/gemini-cli-extensions/ralph` 명령어로 설치 가능합니다.

<br/>

---

### 요약하자면:
* **기법의 원조**를 찾으신다면: [ghuntley/how-to-ralph-wiggum](https://github.com/ghuntley/how-to-ralph-wiggum)
* **당장 설치해서 쓸 CLI 도구**를 찾으신다면: [iannuttall/ralph](https://github.com/iannuttall/ralph) 또는 [Gemini CLI Extension](https://github.com/gemini-cli-extensions/ralph)

최근에는 **Claude Code**나 **Cursor**와 결합하여 사용하는 방식이 가장 흔하며, 단순히 `while` 루프를 돌려 AI가 테스트를 통과할 때까지 코드를 수정하게 만드는 것이 핵심입니다.

<br/>
