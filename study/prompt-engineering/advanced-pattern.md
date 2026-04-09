# 프롬프트 엔지니어링 심화 패턴 (Frameworks)

상황에 따라 프롬프트를 체계적으로 구성할 수 있는 주요 프레임워크들을 정리합니다.

---

## 1. RTF (Role-Task-Format)
가장 범용적으로 사용할 수 있는 프레임워크입니다.

*   **Role (역할):** 누구의 입장에서 답변해야 하는가 (예: Google 광고 마케터)
*   **Task (작업):** 무엇을 수행해야 하는가 (예: 신규 피트니스 의류 캠페인 설계)
*   **Format (형식):** 결과물을 어떤 형태로 출력해야 하는가 (예: 스토리보드 형식)

**예시:**
```text
You are google ad marketer. [ROLE]
Design a compelling game ad campaign to promote a new line of fitness apparel for a sports brand. [TASK]
Create a storyboard outlining the sequence of ad creative including ad copy, visuals and targeting strategy [FORMAT]
```

---

## 2. TAG (Task-Action-Goal)
수행 과정과 목표를 명확히 정의할 때 유용합니다.

*   **Task (작업 정의):** 수행할 작업 기술 (예: 팀원 성과 평가)
*   **Action (행동 설명):** 구체적으로 어떻게 행동해야 하는가 (예: 관리자로서 강점과 약점 분석)
*   **Goal (목표 명시):** 궁극적으로 도달하고자 하는 목표 (예: 고객 만족도 점수를 5점에서 9점으로 향상)

**예시:**
```text
The task is to evaluate the performance of team members. [TASK]
Act as a Director manager and assess the strength and weakness of team members. [ACTION]
Goal is to improve team performance so that average user satisfaction score moves from 5 to 9 in the next year [GOAL]
```

---

## 3. BAB (Before-After-Bridge)
현재의 문제점을 해결하기 위한 로드맵을 작성할 때 최적화된 방식입니다.

*   **Before (현재 상태):** 직면한 문제점 설명 (예: SEO 랭킹에 노출되지 않음)
*   **After (지향하는 상태):** 도달하고 싶은 결과 (예: 90일 이내에 검색 랭킹 상위 10위 진입)
*   **Bridge (가교):** 목표 달성을 위한 구체적인 계획이나 방법 요청 (예: 멘토링 및 세부 실행 계획 수립)

**예시:**
```text
We're not seen on SEO rankings. [BEFORE]
We want to be in top 10 google search rankings in 90 days. [AFTER]
Develop the detailed plan for mentoring all the measures. [BRIDGE]
```

---

## 4. RISEN (Role-Instruction-Step-End Goal-Narrowing)
비즈니스 플랜이나 가이드라인이 명확한 연구 과제 등 복잡한 제약 조건이 있는 작업에 필수적입니다.

*   **Role (역할):** 전문가 역할 부여 (예: 클라우드 영업 담당자)
*   **Instruction (지침):** 구체적인 작업 지시 (예: 스타트업 대상 Q1 GTM 플랜 작성)
*   **Step (단계):** 수행해야 할 세부 단계 정의 (예: 1. 경제 상황 분석, 2. 주요 계정 추출...)
*   **End Goal (최종 목표):** 기대하는 성과 명시 (예: 이번 분기 내 10개 계정 확보)
*   **Narrowing (제약/범위):** 제외하거나 강조해야 할 사항 (예: 특정 단어 사용 금지 등)

**예시:**
```text
You are Cloud Sales. [ROLE]
Write an in-depth GTM Plan about Q1 for start up customers. [INSTRUCTION]
1. Starts with start up economic. 2. Extract 5 key accounts ... [STEP]
The goal is to wi 10 account in Q1 this year. [END GOAL]
Don't use technical jargon that is too complex for non-technical stakeholders. [NARROWING]
```

---

## 5. CARE (Context-Action-Result-Example)
맥락과 실행 결과의 예시를 통해 답변의 품질을 높이는 유연한 방식입니다.

*   **Context (맥락):** 배경 정보 제공
*   **Action (행동):** 명령할 구체적인 작업
*   **Result (결과):** 기대하는 출력물에 대한 명확한 정의
*   **Example (예시):** 참고할 만한 샘플 예시 제공

**예시:**
```text
[CONTEXT] 우리는 현재 새로운 모바일 앱의 사용자 온보딩 과정을 개선하려고 합니다.
[ACTION] 사용자가 앱을 처음 실행했을 때 보게 될 3단계 튜토리얼 문구를 작성해주세요.
[RESULT] 각 단계는 20자 이내의 짧고 강렬한 문장이어야 합니다.
[EXAMPLE] 예: "환영합니다! 지금 바로 시작해보세요."
```
