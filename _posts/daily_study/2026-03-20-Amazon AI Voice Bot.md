---
title: "Amazon AI Voice Bot"
categories:
    - Daily Study
# layout: default
use_math: true
---
# 현대적 웹 자동화 도구: Playwright와 Chrome for Testing 이해하기

오늘 학습한 **Playwright**의 핵심 개념과 웹 자동화 환경에서 **Chrome for Testing**이 왜 중요한지 정리한 로그입니다.

## 1. Playwright란?
**Playwright**는 Microsoft에서 개발한 차세대 오픈소스 웹 브라우저 자동화 라이브러리입니다. Selenium의 뒤를 잇는 강력한 도구로 평가받고 있습니다.

### 🚀 주요 특징
* **멀티 브라우저 지원:** 하나의 API로 Chromium, Firefox, WebKit(Safari)을 모두 제어할 수 있습니다.
* **자동 대기(Auto-wait):** 요소가 나타나거나 클릭 가능해질 때까지 자동으로 기다려주어 'Flaky test(불안정한 테스트)'를 방지합니다.
* **격리된 컨텍스트:** 브라우저를 매번 새로 띄우지 않고도 독립적인 세션을 빠르게 생성하여 실행 속도가 매우 빠릅니다.
* **강력한 도구 모음:** * `codegen`: 브라우저 동작을 기록해 코드로 자동 생성
    * `Inspector`: 실행 과정을 한 줄씩 디버깅
    * `Trace Viewer`: 실행 과정과 네트워크 상태를 타임라인으로 복기


## 2. 웹 크롤링 vs 웹 자동화
두 개념은 혼용되기도 하지만 목적에 따라 구분됩니다.

| 구분 | 웹 크롤링 (Web Crawling) | 웹 자동화 (Web Automation) |
| :--- | :--- | :--- |
| **목적** | 데이터 수집 (Reading) | 특정 동작 수행 (Acting) |
| **주요 작업** | 텍스트 추출, 이미지 저장, 가격 비교 | 로그인, 글쓰기, 결제, 출석 체크 |
| **Playwright 활용** | JS로 렌더링되는 동적 데이터 수집 | 복잡한 UI 인터랙션 자동 수행 |



## 3. Chrome for Testing (CfT)의 등장 배경
자동화 개발자를 괴롭히던 가장 큰 문제는 **"크롬 브라우저의 자동 업데이트"**였습니다.

> **문제 상황:** 어제까지 잘 작동하던 자동화 코드가 오늘 아침 브라우저 업데이트로 인해 ChromeDriver 버전과 맞지 않아 에러 발생.

### ✅ Chrome for Testing이란?
Google이 오직 **자동화 테스트만을 위해** 별도로 출시한 전용 브라우저입니다.

* **버전 고정:** 자동 업데이트 기능이 제거되어, 개발자가 지정한 특정 버전을 안정적으로 유지할 수 있습니다.
* **독립성:** 일반 사용자가 쓰는 크롬과 별개로 설치되어 서로 영향을 주지 않습니다.
* **표시:** 실행 시 상단에 "자동화 테스트용으로 제작되었습니다"라는 안내 문구가 표시됩니다.



## 4. 요약 및 결론
현대적인 웹 환경(SPA, React, Vue 등)을 자동화하기 위해서는 **Playwright**가 매우 효율적인 선택지이며, 버전 관리의 고통에서 벗어나기 위해 **Chrome for Testing**과 같은 전용 브라우저 환경을 활용하는 것이 필수적입니다.



**Tag:** `#Playwright` `#Python` `#WebAutomation` `#Crawling` `#ChromeforTesting`