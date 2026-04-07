# 예제 1: gemini cli skill 'teb'

- https://github.com/alpha3002025/teb

만들려는 것: gemini cli skill

만들려는 skill 명 : 'teb' (text-extracting-briefing)

## 기능 (1) : ppt-slides
(ppt-slides) ppt 슬라이드 이미지 n 개의 텍스트 추출 or 내용정리 기능<br/>

텍스트 추출 모드<br/>
- `--extracting`

통찰모드<br/>
- `--briefing`

결과 파일타입<br/>
- `.md`
- `.txt`
- `.toml`

<br/>

e.g.
```bash
/teb:ppt-slides --extracting --output-format md slide1.png slide2.png outputfile.md
/teb:ppt-slides --extracting --output-format md ./slide-images outputfile.md
/teb:ppt-slides --extracting --output-format md ./slide-images ./slide-images2 outputfile.md

/teb:ppt-slides --briefing --output-format md slide1.png slide2.png outputfile.md
/teb:ppt-slides --briefing --output-format md ./slide-images outputfile.md
/teb:ppt-slides --briefing --output-format md ./slide-images ./slide-images2 outputfile.md
```

<br/>

## 기능 (2) : pdf-images
(pdf-images) 책의 페이지 이미지 n개에 대해 텍스트 추출 or 내용정리 기능<br/>

텍스트 추출 모드<br/>
- `--extracting`

통찰모드<br/>
- `--briefing`

파일타입<br/>
- `.md`
- `.txt`
- `.toml`

<br/>

e.g.
```bash
/teb:pdf-images --extracting --output-format md page1.png page2.png outputfile.md
/teb:pdf-images --extracting --output-format md ./page-images outputfile.md
/teb:pdf-images --extracting --output-format md ./page-images ./page-images2 outputfile.md

/teb:pdf-images --briefing --output-format md page1.png page2.png outputfile.md
/teb:pdf-images --briefing --output-format md ./page-images outputfile.md
/teb:pdf-images --briefing --output-format md ./page-images ./page-images2 outputfile.md
```

<br/>

## 작업과정
### 기능명세서 작성
- https://github.com/alpha3002025/teb/blob/main/.internal-archive/requirements/product.md

### plan
gemini 에 다음 명령을 수행
```bash
planning/plan.md 에 requirements/product.md 의 내용을 구현하기 위한 계획을 작성하세요.
```

결과물
- https://github.com/alpha3002025/teb/blob/main/.internal-archive/planning/plan.md

antigravity 출력
```plain
`.internal-archive/planning/plan.md` 파일에 `.internal-archive/requirements/product.md`의 요구사항을 반영한 **'teb' (text-extracting-briefing) 스킬 구현 계획**을 작성하였습니다.

### 주요 포함 내용:
- **프로젝트 개요**: PPT 슬라이드 및 PDF 페이지 이미지에서 텍스트 추출 및 브리핑을 수행하는 Gemini CLI 스킬 정의.
- **핵심 기능**: `ppt-slides`, `pdf-images` 각각에 대한 `--extracting`(텍스트 추출) 및 `--briefing`(통찰/요약) 모드 구현.
- **CLI 명령어 설계**: 사용자가 요청한 명령어 형식(`${/teb:...}`) 및 출력 포맷(`.md`, `.txt`, `.toml`) 대응.
- **기술 아키텍처**: 입력 파일 수집, Gemini 멀티모달 API 연동, 포맷별 출력 생성 로직 설계.
- **단계별 로드맵**: 기초 설정부터 입력 처리, AI 로직 구현, 결과 포맷팅 및 테스트까지의 5단계 계획.

작성된 상세 계획은 [plan.md](...)에서 확인하실 수 있습니다. 다음 단계로 구현을 시작할까요?
```
<br/>

### 작업 수행
gemini 에 다음 명령을 수행
```bash
.internal-archive/reference/knowledge/gemini-skills.md 의 gemini cli 의 skill에 대한 내용을 참조해서 planning/plan.md 에 맞도록 작업을 진행하고 각 단계마다 동작에 대한 검증 내용을 process 디렉터리 내에 적절한 파일을 만들어 결과서를 작성하세요.

테스트 이미지는 아래의 디렉터리 내의 이미지들을 활용하시면 됩니다.
- pdf-images : test/samples/pdf-images
- slide-images: test/samples/slide-images/mtn
```

이 과정에서는 시행착오가 여러번 있었고 중단 후 프롬프트를 재작성하거나 테스트를 위한 첨부파일을 추가해나가는 과정을 거쳤다.<br/>
<br/>

### 점검(검증) 작업 (Verify)
Gemini 가 작업 끝냈다고 할때마다 `.internal-archive/inspection/checklist-v1.md` 의 내용에 점검할 내용을 추가하고 아래 내용을 계속 다시 실행했다.
```plain
.internal-archive/inspection/checklist-v1.md 를 참고해서 올바르게 구현했는지 검증하고, 그렇지 않다면 .internal-archive/planning/plan.md 에 수정할 내용이 있다면 수정해서 작업을 진행하고, process 내에 추가로 작업한 내용들을 적절한 md 파일로 작성해서 설명하세요. process 내의 내용들은 한글로 작성하세요.
```
<br/>


## 회고
### 시행착오 (1)
이 기능으로 gsd 를 연습해봐야겠다고 생각했는데, 생각보다 큰 기능이 아니기에 gsd 만큼의 컨텍스트 소모가 필요없었다. 그래서 소규모 기능에 맞게 gsd 없이 작업하는 과정을 기록하기로 했다.<br/>
<br/>

### 시행착오 (2) : 프롬프트 수정과정
'작업 수행'과정에서는 한큐에 될것 같지만 계속해서 프롬프트를 수정해서 수행해야했다. 예를 들면 테스트로 사용할 이미지들을 준비를 안했다가 AI가 하는 작업들을 보면서 테스트 용도의 이미지들을 추가해줬었다.<br/>

그런데 이게 실수는 아닌 것 같다. gsd 등을 사용한다면 테스트는 어떻게할지 어떤 것을 테스트 할지 AI가 물어봤을 것 같다.
<br/>