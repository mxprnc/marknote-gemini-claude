# Gemini CLI 기본 모델 설정 가이드

Gemini CLI(@google/gemini-cli)에서 기본 모델을 설정하고 우선순위에 따라 관리하는 방법들을 정리합니다.

---

## 1. 설정 방법 및 우선순위

모델 설정은 적용 범위와 환경에 따라 4가지 우선순위로 나뉩니다. (숫자가 작을수록 우선순위가 높습니다.)

### ① 1회성 실행 플래그 (Highest)
특정 세션에서만 임시로 모델을 변경할 때 사용합니다.
*   **명령어:** `gemini --model <모델ID>` 또는 `gemini -m <모델ID>`
*   *예: `gemini -m gemini-3-flash-preview`*

### ② OS 환경 변수
터미널 세션 전체나 시스템 전역에서 모델을 고정할 때 유용합니다.
*   **macOS / Linux (zsh/bash):** `export GEMINI_MODEL="gemini-3-flash-preview"`
    *   `~/.zshrc` 또는 `~/.bashrc`에 추가 시 영구 적용
*   **Windows (PowerShell):** `$env:GEMINI_MODEL="gemini-3-flash-preview"`

### ③ 프로젝트별 설정 (.gemini/settings.json)
특정 프로젝트 폴더 내에서만 다른 모델을 기본값으로 사용하고 싶을 때 사용합니다.
*   **위치:** `{프로젝트_루트}/.gemini/settings.json`
*   **내용:**
    ```json
    {
      "model": {
        "name": "gemini-3-flash-preview"
      }
    }
    ```

### ④ 사용자 전역 설정 (Global)
모든 프로젝트에 공통으로 적용될 시스템 전역 기본값입니다.
*   **위치:** 
    *   **macOS / Linux:** `~/.gemini/settings.json`
    *   **Windows:** `C:\Users\<사용자이름>\.gemini\settings.json`
*   **내용:** 위와 동일한 JSON 구조 사용

---

## 2. 권장 모델: Gemini 3 Flash Preview

현재 가성비와 성능 면에서 가장 권장되는 모델입니다.

*   **모델 ID:** `gemini-3-flash-preview`
*   **특징:**
    *   **고도의 지능:** Pro 수준의 성능을 유지하면서도 처리 속도가 매우 빠름
    *   **비용 효율:** 기존 Pro 모델 대비 저렴한 비용으로 사용 가능
    *   **대용량 컨텍스트:** 1M(백만) 토큰 지원으로 방대한 코드 및 문서 분석에 최적화

---

## 💡 유용한 팁

### 인터랙티브 방식으로 기본값 저장하기
파일을 수동으로 수정하는 대신 CLI 인터페이스에서 직접 설정을 저장할 수 있습니다. 가장 안전한 방법입니다.

1.  CLI 실행 시 모델 선택 화면에서 `Remember model for future sessions: false` 항목을 찾습니다.
2.  **Tab 키**를 눌러 **true**로 변경합니다.
3.  원하는 모델(예: `gemini-3-flash-preview`)을 선택하고 **Enter**를 누릅니다.
4.  CLI가 자동으로 사용자의 전역 `settings.json` 파일을 업데이트합니다.