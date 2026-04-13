## 참고
- https://github.com/gemini-cli-extensions/ralph

<br/>

## 전역 설치/삭제
### 설치
```bash
gemini extensions install https://github.com/gemini-cli-extensions/ralph --auto-update
```
이렇게 하면 홈 디렉터리에 설치됩니다
<br/>

### 삭제
CLI 로 삭제할 경우
```bash
gemini extensions uninstall ralph
```
<br/>

수동으로 삭제할 경우
```bash
rm -rf ~/.gemini/extensions/ralph
```
<br/>

## 프로젝트에만 설치
### 설치
```bash
mkdir -p .gemini/plugins
git clone https://github.com/gemini-cli-extensions/ralph .gemini/plugins/ralph
```
<br/>

프로젝트 루트의 `.gemini/settings.json` 수정
```json
{
  "context": {
    "includeDirectories": ["./.gemini/plugins/ralph"]
  }
}
```
<br/>

### 삭제
```bash
rm -rf .gemini/plugins/ralph
```
<br/>


## settings.json 원상복구
ralph 플러그인을 삭제 후에는 반드시 프로젝트 내의 `.gemini/settings.json` 또는 전역 gemini 파일인 `~/.gemini/settings.json` 파일을 다음과 같이 원래대로 돌려놓는 것이 좋음
```json
{
  "context": {
    "includeDirectories": [] // ralph 관련 경로 삭제
  }
}
```
<br/>
