# Gemini CLI MCP(Model Context Protocol) 가이드
![Gemini Banner](https://geminicli.com/_astro/icon.Bo4M5sF3.png)

이 문서는 Gemini CLI에서 MCP(Model Context Protocol) 서버를 구성하고 사용하는 방법에 대한 종합적인 가이드를 제공합니다.

<br/>

## 1. MCP 서버란 무엇인가?
MCP 서버는 **Model Context Protocol**을 통해 Gemini CLI에 도구와 리소스를 노출하는 애플리케이션입니다. 이를 통해 Gemini 모델은 로컬 환경이나 외부 API 서비스와 상호작용할 수 있는 브릿지 역할을 수행합니다.

**MCP 서버의 주요 기능:**
- **도구 검색(Discovery):** 표준화된 스키마를 통해 사용 가능한 도구 목록, 설명 및 파라미터 제공.
- **도구 실행(Execution):** 정의된 인자로 특정 도구를 호출하고 구조화된 응답 수신.
- **리소스 접근:** 서버가 노출하는 정보(파일, API 데이터, 보고서 등)를 읽기.

<br/>

## 2. 핵심 통합 아키텍처
Gemini CLI는 정교한 검색 및 실행 시스템을 통해 MCP 서버와 통합됩니다.

### ① 검색 레이어 (Discovery Layer)
`discoverMcpTools()` 함수를 통해 다음 과정을 수행합니다.
1. `settings.json`에 구성된 서버 목록을 순회합니다.
2. 전송 방식(Stdio, SSE, HTTP)에 따라 연결을 수립합니다.
3. MCP 프로토콜을 사용하여 각 서버로부터 도구 정의를 가져옵니다.
4. Gemini API 호환성을 위해 스키마를 검증하고 전역 도구 레지스트리에 등록합니다.

### ② 실행 레이어 (Execution Layer)
발견된 각 도구는 `DiscoveredMCPTool` 인스턴스로 래핑되어 다음을 관리합니다.
- 서버 신뢰 설정 및 사용자 기본 설정에 따른 **승인 로직** 처리.
- 적절한 파라미터로 MCP 서버를 호출하여 실행 관리.
- LLM 컨텍스트 및 사용자 디스플레이를 위한 응답 처리.

### ③ 전송 방식 (Transport Mechanisms)
- **Stdio**: 하위 프로세스를 실행하고 stdin/stdout을 통해 통신합니다. (로컬 서버용)
- **SSE**: Server-Sent Events 엔드포인트에 연결합니다.
- **Streamable HTTP**: HTTP 스트리밍을 사용하여 통신합니다.

<br/>

## 3. MCP 리소스 활용
일부 서버는 도구 외에도 컨텍스트 정보를 담은 '리소스'를 노출합니다.

- **참조 방법**: 로컬 파일을 참조할 때와 동일한 `@` 구문을 사용합니다.
  - 예: `@server://resource/path`
- **작동 방식**: 메시지 제출 시 CLI가 `resources/read`를 호출하여 해당 내용을 대화에 주입합니다.

<br/>

## 4. MCP 서버 설정 (`settings.json`)

### ① 전역 설정 (`mcp` 객체)
```json
{
  "mcp": {
    "allowed": ["trusted-server"],
    "excluded": ["experimental-server"]
  }
}
```

### ② 서버별 세부 설정 (`mcpServers`)
| 속성 | 유형 | 설명 |
| :--- | :--- | :--- |
| **`command`** | String | Stdio 방식 실행 파일 경로 (예: `node`, `python`). |
| **`url` / `httpUrl`** | String | SSE 또는 HTTP 스트리밍 엔드포인트 URL. |
| **`args`** | Array | 실행 파일에 전달할 인자값. |
| **`env`** | Object | 서버 환경 변수. `$VAR` 형식을 통한 로컬 변수 확장 지원. |
| **`trust`** | Boolean | `true` 설정 시 해당 서버의 도구 호출 승인 과정을 생략합니다. |
| **`includeTools`** | Array | 사용할 도구 목록만 화이트리스트로 지정. |
| **`excludeTools`** | Array | 제외할 특정 도구 목록 지정. |

<br/>

## 5. 보안 및 환경 변수 보호
Gemini CLI는 서드파티 MCP 서버로의 민감 정보 유출을 방지하기 위해 **환경 변수 마스킹(Sanitization)**을 수행합니다.

- **자동 제거**: `TOKEN`, `SECRET`, `PASSWORD`, `KEY`, `AUTH` 등이 포함된 변수는 기본 프로세스 환경에서 자동으로 제거된 후 서버에 전달됩니다.
- **명시적 허용**: 특정 변수가 서버 작동에 반드시 필요하다면 `settings.json`의 `env` 블록에 명시적으로 정의해야 합니다. 명시된 변수는 사용자의 동의가 있는 것으로 간주되어 보호 로직을 통과합니다.

<br/>

## 6. 멀티모달 및 풍부한 콘텐츠 응답
MCP 도구는 단순 텍스트 외에도 이미지, 오디오 등 바이너리 데이터를 포함한 멀티파트 응답을 반환할 수 있습니다.
- 응답 내 `ContentBlock` 배열을 통해 `text`, `image`, `audio` 등을 혼합하여 반환하면, Gemini CLI가 이를 적절히 패키징하여 모델의 컨텍스트로 주입합니다.

<br/>

## 7. MCP 프롬프트를 슬래시 명령어로 사용
서버가 정의된 프롬프트를 노출하면 CLI 내에서 슬래시(`/`) 명령어로 즉시 호출할 수 있습니다.
- 예: `/poem-writer --title="My Poem" --mood="happy"`
- 이 명령은 서버의 `prompts/get` 메서드를 호출하여 완성된 텍스트를 모델에 전달합니다.

<br/>

## 8. 명령어 관리 (`gemini mcp`)
`settings.json`을 직접 수정하지 않고 터미널에서 서버를 쉽게 관리할 수 있습니다.

```bash
# 새 서버 추가 (Stdio 기본)
gemini mcp add my-server node ./server.js --env KEY=$VALUE

# SSE 서버 추가
gemini mcp add --transport sse sse-server https://example.com/sse

# 구성된 서버 목록 및 상태 확인
gemini mcp list

# 서버 일시 비활성화/활성화
gemini mcp disable my-server
gemini mcp enable my-server

# 서버 제거
gemini mcp remove my-server
```

---
**주의**: MCP 서버 이름에 언더바(`_`)를 사용하지 마세요. 정책 엔진에서 서버와 도구를 구분하는 구분자로 사용되므로, 서버 식별 오류가 발생할 수 있습니다 (예: `my_server` 대신 `my-server` 권장).