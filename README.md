# Don't Go Too Fast (DGTF++)

> 압박 속에서도 품질을 지키는 소프트웨어 개발 철학
>
> A software development philosophy that maintains quality under pressure

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 한국어

### 소개

**Don't Go Too Fast(DGTF++)**는 수십 년의 실전 경험에서 도출된 소프트웨어 개발 방법론입니다.

Clean Code, SOLID, TDD... 대부분의 개발자가 **"무엇(What)"** 을 알고 있습니다.
그런데 왜 금요일 오후 마감 앞에서 전역 변수를 쓰게 될까요?

마감일 압박, 요구사항 변경, 긴급 버그 수정... 이런 상황에서 우리의 뇌는 자동적으로 "빠른 사고 모드(System 1)"로 전환되고, 이는 종종 나쁜 긴급 결정으로 이어집니다.

**ROD, TFD, DGTF는 "어떻게(How)"에 대한 답입니다.**
- 압박 속에서 어떻게 Clean Code를 유지하는가
- 시간 없을 때 어떻게 테스트를 쓰는가
- 급할 때 어떻게 신중함을 유지하는가

이 방법론은 **압박 상황에서도 품질을 유지하는 방법**을 제시합니다.

### 세 가지 핵심 방법론

#### 🏗️ ROD (Responsibility-Oriented Design)
**"More is better than missing"**

구현 단계의 혼란과 나쁜 긴급 결정을 방지하기 위해, 설계 단계에서 완전한 서비스 체인을 구축합니다.

- 모든 책임을 서비스로 표현
- Constructor/Static 사용 금지
- Missing 제거
- SOLID 원칙으로 교체 가능한 구조
- 변경의 격리: 서비스 경계가 변경 전파를 막음

**핵심 가치:** 구현 중 "어떻게 하지?"라는 혼란을 사전에 제거

#### 🧪 TFD (Test-First Development)
**"요구사항 = 테스트"**

테스트를 설계와 함께 (또는 그 전에) 작성하여 품질을 보증합니다.

- 테스트가 곧 명세서
- 완료 기준 명확화
- 테스트 가능한 설계 강제
- TDD가 어려웠다면? ROD가 테스트 대상을 명확하게 해줍니다

**핵심 가치:** "올바르게 작동하는가"를 지속적으로 검증

#### ⏸️ DGTF (Don't Go Too Fast)
**"Slow is smooth, smooth is fast"**

압박 상황에서도 신중함을 유지하여 System 1의 나쁜 긴급 결정을 방지합니다.

- System 1 트리거 인식
- 멈춤 → 생각 → 진행
- 압박 속에서도 품질 유지
- 자기 통제가 Professional을 만든다

**핵심 가치:** 처음엔 느려 보여도, 전체적으로는 더 빠름

### 이론적 기반

이 방법론은 세 가지 검증된 이론을 기반으로 합니다:

- **Daniel Kahneman** (노벨상): Dual Process Theory - 인간의 사고 방식
- **Donella Meadows**: Systems Thinking - 전체 시스템 이해
- **Genrich Altshuller**: TRIZ - 창의적 문제 해결

이 이론들이 어떻게 ROD, TFD, DGTF로 실천되는지는 문서를 참고하세요.

### 본질

ROD, TFD, DGTF는 원칙(Principle)이나 규칙(Rule)이 아닙니다.  
**사고방식(Way of Thinking)** 이자 **습관(Habit)** 입니다.  

운전면허가 있다고 좋은 운전자가 아니듯,  
SOLID를 안다고 좋은 개발자가 아닙니다.  

이 방법론은 좋은 개발을 **습관**으로 만드는 방법입니다.  

### 문서

- 📖 [핵심 개념과 가치](docs/ko/01-concepts-and-values.md)
- 📚 [실전 가이드](docs/ko/02-practical-guide.md)

### 시작하기
```bash
# 1. 간단한 기능 하나 선택
# 2. ROD로 서비스 체인 설계
# 3. TFD로 테스트 케이스 정의
# 4. DGTF로 신중하게 구현
```

### 누구를 위한 방법론인가?

- ✅ 압박 속에서 품질을 유지하고 싶은 개발자
- ✅ 기술 부채를 줄이고 싶은 팀
- ✅ 예측 가능한 개발을 원하는 리더
- ✅ 지속 가능한 개발 문화를 만들고 싶은 조직

### 라이선스

MIT License - 자유롭게 사용, 수정, 배포하세요.

---

## English

### Introduction

**Don't Go Too Fast** is a software development methodology derived from decades of real-world experience.

Clean Code, SOLID, TDD... Most developers know **"What"** to do.
But why do we end up using global variables on Friday afternoon before a deadline?

Tight deadlines, changing requirements, urgent bug fixes... In these situations, our brain automatically switches to "fast thinking mode (System 1)", which often leads to poor rushed decisions.

**ROD, TFD, DGTF are the answers to "How".**
- How to maintain Clean Code under pressure
- How to write tests when time is short
- How to stay thoughtful when things are urgent

This methodology presents **how to maintain quality even under pressure**.

### Three Core Methodologies

#### 🏗️ ROD (Responsibility-Oriented Design)
**"More is better than missing"**

Build a complete service chain in the design phase to prevent confusion and poor rushed decisions during implementation.

- Express all responsibilities as services
- Prohibit Constructor/Static usage
- Eliminate Missing parts
- Replaceable structure with SOLID principles
- Change Isolation: Service boundaries prevent change propagation

**Core Value:** Eliminate "How do I do this?" confusion during implementation

#### 🧪 TFD (Test-First Development)
**"Requirements = Tests"**

Write tests alongside (or before) design to ensure quality.

- Tests are specifications
- Clear completion criteria
- Force testable design
- Struggled with TDD? ROD makes test targets clear

**Core Value:** Continuously verify "Does it work correctly?"

#### ⏸️ DGTF (Don't Go Too Fast)
**"Slow is smooth, smooth is fast"**

Maintain thoughtfulness even under pressure to prevent poor rushed decisions from System 1.

- Recognize System 1 triggers
- Pause → Think → Proceed
- Maintain quality under pressure
- Self-control creates the Professional

**Core Value:** Appears slow at first, but faster overall

### Theoretical Foundation

This methodology is built on three validated theories:

- **Daniel Kahneman** (Nobel Laureate): Dual Process Theory - How humans think
- **Donella Meadows**: Systems Thinking - Understanding the whole system
- **Genrich Altshuller**: TRIZ - Creative problem solving

See the documentation for how these theories are put into practice through ROD, TFD, and DGTF.

### Essence

ROD, TFD, DGTF are not Principles or Rules.
They are a **Way of Thinking** and **Habits**.

Having a driver's license doesn't make you a good driver.
Knowing SOLID doesn't make you a good developer.

This methodology is about making good development a **habit**.

### Documentation

- 📖 [Core Concepts and Values](docs/en/01-concepts-and-values.md)
- 📚 [Practical Guide](docs/en/02-practical-guide.md)

### Getting Started
```bash
# 1. Choose one simple feature
# 2. Design service chain with ROD
# 3. Define test cases with TFD
# 4. Implement thoughtfully with DGTF
```

### Who Is This For?

- ✅ Developers who want to maintain quality under pressure
- ✅ Teams who want to reduce technical debt
- ✅ Leaders who want predictable development
- ✅ Organizations who want sustainable development culture

### License

MIT License - Feel free to use, modify, and distribute.

---

## Contributing

We welcome contributions! Please feel free to:
- Report issues
- Suggest improvements
- Share your experiences
- Translate documentation

## Contact

For questions or discussions, please open an issue.

---

**Remember: Don't Go Too Fast** 🐢💨
