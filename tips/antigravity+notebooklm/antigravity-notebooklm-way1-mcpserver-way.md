# Antigravity + NotebookLM - (1) MCP Server 방식

이번 방식은 MCP Server 를 이용해 Notebook LM 을  Antigravity 에서 연동하는 방식이다.<br/>

나는 MCP Server로 쓰고 싶지 않기에(토큰소모, API 과금 등의 이슈) way 2 의 방식인 gemini cli skill 을 이용한 방식을 선택했다.<br/>

나중에 필요할 경우를 위해 그냥 정리해뒀다.<br/>
<br/>

참고로 npx 가 PATH 에 지정되어 있어야 사용가능하다.<br/>
<br/>

## 참고자료
- 안티그래비티 + 노트북 LM으로 만드는 서브 에이전트 : https://www.youtube.com/watch?v=IMFiasVnc0o

<br/>

## Antigravity 에 mcp server 추가
Antigravity 를 열어서 `mcp servers` 를 선택
![alt text](mcp-server-way-1.png)
<br/>

notebooklm 에 대한 Antigravity 에서 공식 지원되는 MCP Server 은 없는 것으로 보인다. 따라서 `Manage MCP Servers` 를 선택한다.
![alt text](mcp-server-way-2.png)
<br/>

로딩중인 모습, `View raw config` 버튼을 클릭
![alt text](mcp-server-way-3.png)
<br/>

다음과 같은 내용을 입력한다.
```bash
{
  "mcpServers": {
    "notebooklm": {
      "command": "npx",
      "args": ["notebooklm-mcp@latest"]
    }
  }
}
```
<br/>

그리고나서 Manage MCP 창으로 돌아가서 Refresh 버튼을 클릭하면 새로 추가된 MCP 서버 목록이 나타난다.<br/>
<br/>

위의 주소나 인자값은 다음 방법으로 찾을수 있다.<br/>
<br/>

(1)<br/>
구글에서 'notebooklm mcp' 검색 후 검색 결과 중 [github.com/PleasePrompto/notebooklm-mcp]https://github.com/PleasePrompto/notebooklm-mcp) 을 선택

<br/>

(2)<br/>
github 내의 README 를 쭉 내리다보면 Other MCP clients 를 선택하고 해당 항목을 복사
![alt text](mcp-server-way-4.png)

<br/>






