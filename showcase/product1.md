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
/teb:ppt-slides --extracting --output-format md slide1.png slide2.png
/teb:ppt-slides --extracting --output-format md ./slide-images
/teb:ppt-slides --extracting --output-format md ./slide-images ./slide-images2

/teb:ppt-slides --briefing --output-format md slide1.png slide2.png
/teb:ppt-slides --briefing --output-format md ./slide-images
/teb:ppt-slides --briefing --output-format md ./slide-images ./slide-images2
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
/teb:pdf-images --extracting --output-format md page1.png page2.png
/teb:pdf-images --extracting --output-format md ./page-images
/teb:pdf-images --extracting --output-format md ./page-images ./page-images2

/teb:pdf-images --briefing --output-format md page1.png page2.png
/teb:pdf-images --briefing --output-format md ./page-images
/teb:pdf-images --briefing --output-format md ./page-images ./page-images2
```

<br/>
