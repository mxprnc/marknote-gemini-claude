## skills 란?
- "AI 에게 주는 업무 매뉴얼"
- 신입사원이 들어오면 매번 새로 생각해서 "이건 이렇게 하고, 저건 저렇게하고, 예를 들면 이렇게 ... " 라고 설명해야 하는 것들을 미리 정리해둔 것

skills 는 agent skills 라는 공개된 표준을 따른다. 즉 "범용 규격"이다. 따라서 gemini cli 에서도 동일한 skills 를 사용할 수 있다.

프롬프트 하나가 반복되고, 특정 작업에 대해 일관된 결과물을 얻고 싶고 특징이 정해져 있다면, skill 로 정의해서 만들어두면 된다. skill 이라고 해서 처음에는 무슨 명령어처럼 느껴지지만, 그냥 '매뉴얼'개념이고, 'Agent 가 참고하는 지침서' 같은 개념이다.


### skill 을 쓰는 것의 장점
- 반복되는 프롬프트를 하나의 skills 로 정의해 재사용
- 일관성 : 거의 동일한 결과물이 나올 수 있도록 유도
- 공유 : 공유가능하다. 


<br/>
<br/>

### 구조
![alt text](<./img/2-skills---claude/CleanShot 2026-03-23 at 16.17.24@2x.png>)
<br/>


첫 진입점을 SKILL.md 로 두고 그 다음부터는 SKILL.md 내에서 필요한 문서들을 찾아서 읽어나가는 방식인데, 이렇게 구조를 나누는 것의 장점은 전체 컨텍스트를 한번에 모두 읽어들일 필요가 없다는 점이다. 컨텍스트를 모두 꽉 채워서 불러오는 것도 조금은 오염된 컨텍스트를 가지는 것이기에 컨텍스트에 필요한 것만 필요할 때 Lazy loading 으로 불러와야 효율적이다.<br/>
<br/>


### CLAUDE.md vs Skills
![alt text](<./img/2-skills---claude/CleanShot 2026-03-23 at 16.26.36@2x.png>)

CLAUDE.md 가 무겁고 그렇다는 걸 설명하려는건 아닌데, 위 표의 내용은 프로젝트 루트에 CLAUDE.md 와 Skills.md 를 비교해서 이해하기 위해 작성된 표다.
<br/>

### MCP vs Sub-Agent vs Skills
MCP
- 'Agent Engineering 5가지 핵심 개념'강의에서 다루고 있으니 참고
- MCP 를 사용하면 해당 도구에 대한 설명이 모두 시작할 때마다 올라가고 대화할 때 마다 세션시작할 때 마다 계속 포함된다.


Skills
- 평소에는 이름, 한줄 설명만 로딩된다.
- 전체 프롬프트는 해당 skill 이 호출될 때에만 로딩된다.
- skill 은 sub agent, mcp 에 비해 상당히 가볍다.


Sub Agent
- 별도의 컨텍스트를 생성하고 그 공간에서 실행된다.
- Sub Agent 가 파일 100개를 뒤져도 메인 에이전트의 컨텍스트는 오염되지 않는다. 그리고 그 요약된 결과만 돌아온다.
- Sub Agent 의 이름, 설명 등은 시작할 때 부터 로드되어 있기 때문에 메인 에이전트의 컨텍스트에 포함된다. 
- Sub Agent 10개를 만들면 Sub Agent 10 개에 대한 이름, 설명 등을 모두 로딩하게 된다.


### skills.md 의 구조
![alt text](<./img/2-skills---claude/CleanShot 2026-03-23 at 16.51.39@2x.png>)

```
### (1) --- start
---
name: ppt-generator
description: "PPT 발표자료 자동 생성. 'PPT 만들어줘',
  '발표자료 작성', '슬라이드 제작' 요청 시 트리거."
---
### (1) --- end

### (2) --- start
## 목적
주제와 핵심 내용을 입력하면 PPT 발표자료를 자동 생성합니다.

## 절차
1. 발표 주제, 대상 청중, 발표 시간 확인
2. 목차 및 슬라이드 구성 설계
3. 각 슬라이드별 내용 작성
4. .pptx 파일로 출력

## 자체 검증 체크리스트
- [ ] 슬라이드 수가 적절한가?
- [ ] 핵심 메시지가 있는가?
- [ ] 시각 자료 지시사항이 포함되었는가?
### (2) --- end
```

(1) front matter
- name : 스킬의 고유 식별자, 슬래시 커맨드로 사용된다.
- description : 스킬에 대한 설명, Claude 가 '이 요청에 이 스킬이 맞겠다' 고 판단하는 핵심 필드. 품질이 매우 중요

(2) markdown body
- 실제 작업 절차, 출력 형식 등을 자유롭게 기술하는 영역
- 길이 제한은 없다.

<br/>

#### desccription 필드 품질의 중요성
![alt text](<./img/2-skills---claude/CleanShot 2026-03-23 at 16.57.31@2x.png>)

description 에 실제로 사용할 만한 키워드나 사람의 말투를 적어줘야 적용이 잘된다.
<br/>


## e.g. ppt 스킬 직접 만들기
![alt text](<./img/2-skills---claude/CleanShot 2026-03-23 at 16.59.48@2x.png>)
ppt 스킬을 만드는 방법은 터미널에서 직접 만들수도 있고, 클로드의 skill-creator 플러그인을 이용할수도 있는데 가급적이면 skill-creator 플러그인을 이용해서 어느 정도 기본 규격이 맞춰진 스킬을 만드는 것을 추천한다.<br/>
<br/>


### `skill-creator` 설치
이거 그냥 AI에 물어봐서 하자. 이게 아마 녹화할때 강사분이 새로운 맥북에서 실행한거라 계속 실수해서 다시찍은걸로 보정한걸로 보임. 어차피 매번 변하기때문에 그냥 AI 로 검색해서 설치진행하면 된다.
claude 실행
![alt text](<./img/2-skills---claude/CleanShot 2026-03-23 at 17.02.18@2x.png>)
<br/>

`/plugin` -> `Marketplaces` 탭 선택
![alt text](<./img/2-skills---claude/CleanShot 2026-03-23 at 17.03.42@2x.png>)
<br/>

`skill-creator` 검색
![alt text](<./img/2-skills---claude/CleanShot 2026-03-23 at 17.05.07@2x.png>)
<br/>

skil-creator 실행
![alt text](<./img/2-skills---claude/CleanShot 2026-03-23 at 17.07.14@2x.png>)
- 따로 skill-creator 를 호출할 필요가 없다고 한다. 프롬프트 내에 '스킬을 만들어줘'라는 내용이 있기 때문에 이 문구를 보고 `skill-creator` 가 트리거된다고 한다.

<br/>

생성된 모습. 스킬 명을 지정하지 않았기에 claude 가 직접 생성했고 스킬명은 `auto-ppt` 라는 이름으로 생성됐다.
![alt text](<./img/2-skills---claude/CleanShot 2026-03-23 at 17.09.21@2x.png>)

<br/>

설명은 파일을 열어보면 되기에 일단은 /clear 로 화면을 깨끗하게 치운다.
![alt text](<./img/2-skills---claude/CleanShot 2026-03-23 at 17.13.31@2x.png>)

<br/>


skill 실행
만든 skill 을 실행해본다.
![alt text](<./img/2-skills---claude/CleanShot 2026-03-23 at 17.14.20@2x.png>)
<br/>

skill 은 앞으로 누군가가 만들어놓은 skill 을 마켓에서 다운로드 받아서 실행하고 그런 시대가 올수 있을것 같다고 이야기..

<br/>

## skill 저장 위치, 번들 스킬
![alt text](<./img/2-skills---claude/CleanShot 2026-03-23 at 17.18.18@2x.png>)


| 위치 | 경로 | 적용 대상 |
| --- | --- | --- |
| **Enterprise** | 관리 설정 | 조직의 모든 사용자 |
| **Personal** | `~/.claude/skills/<name>/SKILL.md` | 내 모든 프로젝트 |
| **Project** | `.claude/skills/<name>/SKILL.md` | 이 프로젝트만 (Git 공유 가능) |
| **Plugin** | `<plugin>/skills/<name>/SKILL.md` | 플러그인 활성화된 곳 |



번들스킬
- 클로드코드에서 제공하는 skill 들
- /simplify 는 코드 품질을 자동 검토해주는 스킬
- /batch 는 대규모 코드 변경을 병렬 에이전트로 처리