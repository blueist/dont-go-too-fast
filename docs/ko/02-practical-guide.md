## 시작하기 전에: What이 아니라 How

이 문서를 읽기 전에 한 가지 질문:

**당신은 Clean Code를 읽었는가?**   
**SOLID 원칙을 아는가?**   
**TDD를 들어봤는가?**   

아마 "네"일 것이다.

**그런데 왜 금요일 오후 마감 앞에서 전역 변수를 쓰는가?**

이것이 이 문서의 핵심이다.

```
많은 개발자:
"Clean Code? 알아요"
"SOLID? 알죠"
"TDD? 들어봤어요"

현실:
금요일 오후 5시
매니저: "이거 언제 되나요?"
→ "일단 전역 변수로..."
→ "하드코딩하면 빨리..."
→ "테스트? 나중에..."
```

**문제는 "What"을 모르는 게 아니다.
문제는 "How"를 모르는 것이다.**

- What: Clean Code, SOLID, TDD (이미 안다)
- How: 압박 속에서 어떻게 지키는가? (이것이 필요하다)

**ROD, TFD, DGTF는 "How"에 대한 답이다.**

이 문서는 단순한 방법론 설명이 아니다.
"알지만 못 하는 것"을 "알고 할 수 있는 것"으로 바꾸는 가이드다.

---

# Part 1: 소프트웨어 개발의 이론적 기반

### 세 가지 핵심 이론

이 방법론을 이해하려면 먼저 세 가지 검증된 이론을 알아야 합니다.

----------

### 1. Daniel Kahneman의 Dual Process Theory (인간의 사고)

**System 1 (빠른 사고)**

- 특징:
	- 자동적, 무의식적
	- 노력이 거의 필요 없음
	- 빠른 판단
	- 감정적
	- 패턴 인식

- 장점:
	- 즉각적인 반응
	- 효율적
	- 에너지 절약

- 단점:
	- 편향에 취약
	- 실수하기 쉬움
	- 복잡한 문제에 부적합
	- 압박 상황에서 나쁜 결정


**System 2 (느린 사고)**

- 특징:
	- 의도적, 의식적
	- 집중력과 노력 필요
	- 신중한 분석
	- 논리적
	- 추론

- 장점:
	- 정확한 판단
	- 복잡한 문제 해결
	- 장기적 사고

- 단점:
	- 느림
	- 피곤함
	- 에너지 소모
	- 압박 상황에서 비활성화

**소프트웨어 개발에 미치는 영향:**

- 설계 단계 (시간 여유):
	- System 2 활성화 가능
	- 깊은 사고
	- 완전한 설계

- 구현 단계 (압박):
	- System 1이 자연스럽게 우세
	- 빠른 판단
	- 나쁜 긴급 결정 위험

**핵심:**
"어느 단계에서 어느 시스템을 사용하느냐가 중요하다"


----------

### 2. Donella H. Meadows의 Systems Thinking (시스템 이해)

**시스템 사고란:**

- 정의:
	시스템 = 상호 연결된 요소들의 집합
	전체는 부분의 단순한 합 이상

- 핵심 원칙:
	1. 전체를 보라
	   - 부분만 보면 전체를 놓친다
	2. 관계를 이해하라
	   - 요소 간 연결이 중요
	3. 피드백 루프를 파악하라
	   - 출력이 입력에 영향
	4. 레버리지 포인트를 찾아라
	   - 작은 변화로 큰 효과
	5. 시스템의 경계를 정의하라
	   - 무엇이 안에 있고 밖에 있는가

**소프트웨어 개발에 미치는 영향:**

- 부분 최적화 vs 전체 최적화:
	- 각 함수를 최적화해도
	- 전체 시스템은 느릴 수 있음
	- 관계와 흐름이 중요

- Missing의 위험:
	- 시스템의 일부가 빠지면
	- 예측 불가능한 행동
	- 전체가 작동 안 함

- 레버리지 포인트:
	- 설계 단계 = 가장 강력한 개입점
	- 구현 후 수정보다 수백 배 효과적

----------

### 3. Genrich Altshuller의 TRIZ (문제 해결)

**TRIZ란:**

- Theory of Inventive Problem Solving
= 창의적 문제 해결 이론

- 출처:
	- 수백만 개의 특허 분석
	- 혁신의 패턴 발견
	- 반복 가능한 방법론

**핵심 개념:**

1. 기술적 모순 (Technical Contradiction)
   "A를 개선하면 B가 나빠진다"
   예: "빠른 개발" vs "높은 품질"
   
2. TRIZ의 해결 원칙
   "모순을 해결하라, 타협하지 마라"
   방법:
   - 시간 분리 (다른 시점에)
   - 공간 분리 (다른 장소에)
   - 조건 분리 (다른 상황에)
   
3. 이상적 최종 결과 (IFR)
   "추가 자원 없이 문제가 저절로 해결된다면?"
   
4. 40가지 발명 원리
   - 분할, 추출, 사전 조치, 사전 보호 등
   - 반복 가능한 해결 패턴


**소프트웨어 개발에 미치는 영향:**

- 전통적 타협:
	"빠르게" → 품질 희생
	"품질 우선" → 속도 희생

- TRIZ 해결: 시간 분리 적용
	- 설계 단계: 느리고 정확하게
	- 구현 단계: 빠르게 (설계 따르기)
	- 결과: 빠르면서도 품질 높음

- 이상적 최종 결과:
"구현 단계에서 혼란이 저절로 사라진다면?"
→ ROD: 설계 단계에서 완전하게 만들면 됨

----------

### 세 이론의 통합

**각 이론의 역할:**

```
┌─────────────────────────────────┐
│ Kahneman                        │
│ "왜 압박받으면 실수하는가?"     │
│                                 │
│ → System 1 vs System 2          │
│ → 언제 어떤 사고를 쓸까         │
└─────────────────────────────────┘
              ↓
┌─────────────────────────────────┐
│ Meadows                         │
│ "무엇을 설계해야 하는가?"       │
│                                 │
│ → Systems Thinking              │
│ → 전체와 관계 보기              │
└─────────────────────────────────┘
              ↓
┌─────────────────────────────────┐
│ Altshuller                      │
│ "어떻게 모순을 해결하는가?"     │
│                                 │
│ → TRIZ                          │
│ → 타협 없는 해결                │
└─────────────────────────────────┘
              ↓
┌─────────────────────────────────┐
│ ROD + TFD + DGTF                │
│ "실천 방법"                     │
│                                 │
│ → 구체적 적용                   │
│ → 측정 가능한 결과              │
└─────────────────────────────────┘

```

**소프트웨어 개발 문제 분석:**

문제:
- "마감일 압박 속에서 고품질 소프트웨어를 만들어야 한다"

- Kahneman 분석:
	- 압박 → System 1 활성화
	- System 1 → 나쁜 긴급 결정
	- 결과 → 기술 부채, 버그

- Meadows 분석:
	- 부분만 보면 Missing 발생
	- Missing → 시스템 불완전
	- 구현 중 추가 → 더 큰 혼란

- Altshuller 분석:
	- "빠름" vs "품질" = 모순
	- 타협하면 둘 다 나쁨
	- 분리 원칙으로 해결

해결책 → ROD + TFD + DGTF


**프로그래밍에서의 실제 적용:**

- 압박 상황:
"내일까지 로그인 기능 만들어주세요"

- ❌ 전통적 접근 (타협):
	System 1: "빨리 만들자!"
	→ 설계 생략
	→ 테스트 생략
	→ 하드코딩
	→ 전역 변수
	결과: 빠르지만 품질 나쁨

- ✅ ROD + TFD + DGTF (모순 해결):
	
	1. 시간 분리 (TRIZ):
	   - 오늘 오후 (2시간): 설계 + 테스트
		   - System 2 활성 (Kahneman)
		   - 완전한 시스템 (Meadows)
	   - 내일 오전 (3시간): 구현
		   - 설계 따르기
		   - DGTF로 통제
	   
	2. 결과:
	   총 5시간 소요
	   품질 높음
	   재작업 없음


----------

## Part 2: ROD (Responsibility-Oriented Design)

### 핵심 원칙

**"More is better than missing"**

이것은 "많이 만들자"가 아니라:

> 설계 단계에서 완전하게 정의하면 (Meadows: 전체 시스템) 구현 단계에서 불필요한 것은 쉽게 제거할 수 있지만, 누락된 것을 추가하는 것은 위험하다 (Kahneman: System 1 폭주)

### ROD는 새로운 "What"이 아니다

솔직히 말하자.

**ROD가 완전히 새로운 건 아니다:**
- Clean Architecture? 비슷하다.
- Service-Oriented Design? 맞다.
- Domain-Driven Design? 겹치는 부분 있다.

**그런데 질문:**

```
당신은 Clean Architecture를 안다.
당신은 SOLID를 안다.
당신은 의존성 주입을 안다.

그런데 왜?
마감이 다가오면 안 지키는가?
왜 "일단 되게 하고 나중에 고치자"가 되는가?
```

**ROD는 새로운 기술이 아니다.**  
ROD는 좋은 설계를 **"왜"**, **"언제"**, **"어떻게"** 해야 하는지에 대한 답이다.

**비교:**

```
Clean Architecture:
"이렇게 구조를 만들어라"
→ What (무엇을)

ROD:
"왜 이렇게 해야 하는지" (Kahneman: System 1 방지)
"언제 이렇게 해야 하는지" (설계 단계에서)
"어떻게 지킬 수 있는지" (Constructor/Static 금지)
→ Why + When + How
```

**실전 차이:**

```
Clean Architecture 아는 개발자:  
설계 단계: "의존성 역전 적용해야지"
구현 단계 (압박): "아 시간 없어, 일단 new로..."
→ 원칙은 알지만 지키지 못함

ROD를 체화한 개발자:
설계 단계: "서비스 체인 완성해야 System 1 방지"
구현 단계 (압박): "서비스 체인 따라가기만 하면 돼"
→ 습관이 되어 자동으로 지킴
```

----------

### 이론적 배경

**Systems Thinking 관점 (Meadows):**

- 시스템 이론:
"시스템의 행동은 모든 요소와 그 관계에 의해 결정된다"

- ROD 적용:
	- 각 서비스 = 시스템의 요소
	- 호출 관계 = 요소 간 연결
	- 완전한 체인 = 전체 시스템
	- Missing = 불완전한 시스템 → 예측 불가능

- 설계 원칙:
	1. 모든 요소를 식별 (서비스)
	2. 모든 관계를 정의 (호출)
	3. 경계를 명확히 (인터페이스)
	4. 피드백 루프 설계 (테스트)

**Kahneman 관점:**

- 문제:
구현 단계 = 압박 → System 1 우세

- Missing 발견 시:
	"어? 이게 필요한데 설계에 없네?"
	→ System 1 폭주
	→ "전역 변수!", "싱글톤!", "하드코딩!"
	→ 나쁜 긴급 결정

- ROD 해결:
	설계 단계 (여유) = System 2 활성
	→ 완전한 서비스 체인
	→ 구현 단계에서 Missing 없음
	→ System 1 폭주 방지

**TRIZ 관점:**

- 이상적 최종 결과 (IFR):
	"구현 중 혼란이 저절로 사라진다면?"

- ROD 답:
	"설계에서 모든 것을 정의하면
 구현은 그냥 따르기만 하면 됨"

- 사전 조치 원리 (Prior Action):
	- 문제가 발생하기 전에 해결
	- 설계 단계에서 미리 모든 것 정의
	- 구현 중 긴급 결정 불필요

- 분할 원리 (Segmentation):
	- 큰 시스템을 서비스로 분할
	- 각각 독립적으로 개발/테스트
	- 교체 가능 (SOLID)

### 변경의 격리 - ROD의 진짜 힘

**현실을 인정하자:**

아무리 완벽하게 설계해도,  
구현 중에 변경은 일어난다.

- 요구사항이 바뀐다
- 더 나은 방법을 발견한다
- 버그를 고쳐야 한다

**문제는 변경 자체가 아니다.  
문제는 변경이 전체로 퍼지는 것이다.**

**실전 예시 - 변경이 퍼지는 경우:**

```go
// ❌ 나쁜 설계: 직접 의존
type LoginHandler struct {
    db *sql.DB  // 직접 의존
}

func (h *LoginHandler) Login(username, password string) (*User, error) {
    // DB 직접 접근
    row := h.db.QueryRow("SELECT * FROM users WHERE username = ?", username)
    
    // 비밀번호 검증도 여기서
    if !checkPassword(user.Password, password) {
        return nil, errors.New("invalid password")
    }
    
    // 세션 생성도 여기서
    session := createSession(user)
    
    // 토큰 생성도 여기서
    token := generateToken(session)
    
    return user, nil
}

// 문제: 비밀번호 검증 방식 변경 요청
// → LoginHandler 수정 필요
// → 테스트 전체 수정
// → 세션 로직에 영향?
// → 토큰 로직에 영향?
// → System 1: "일단 여기 고치고... 저기도 고치고..."
```

**실전 예시 - ROD로 변경이 격리되는 경우:**

```go
// ✅ ROD 설계: 서비스 체인

// 각 서비스는 독립적
type PasswordVerifier interface {
    Verify(plain, hashed string) (bool, error)
}

type UserFinder interface {
    FindByUsername(username string) (*User, error)
}

type SessionCreator interface {
    Create(user *User) (*Session, error)
}

type TokenGenerator interface {
    Generate(session *Session) (string, error)
}

// LoginService는 체인만 연결
type LoginService struct {
    userFinder     UserFinder
    passwordVerifier PasswordVerifier
    sessionCreator SessionCreator
    tokenGenerator TokenGenerator
}

func (s *LoginService) Login(username, password string) (string, error) {
    user, err := s.userFinder.FindByUsername(username)
    if err != nil {
        return "", err
    }
    
    valid, err := s.passwordVerifier.Verify(password, user.HashedPassword)
    if err != nil || !valid {
        return "", errors.New("invalid credentials")
    }
    
    session, err := s.sessionCreator.Create(user)
    if err != nil {
        return "", err
    }
    
    return s.tokenGenerator.Generate(session)
}

// 비밀번호 검증 방식 변경?
// → PasswordVerifier 구현체만 교체
// → 다른 서비스 영향 없음
// → LoginService 코드 변경 없음
// → 테스트도 PasswordVerifier만 수정
```

**"서비스의 끝"과 "다음 서비스의 시작" 사이 계약:**

```
┌──────────────────┐    계약(Interface)   ┌──────────────────┐
│  UserFinder      │ ──────────────────→ │  PasswordVerifier │
│                  │      *User          │                   │
│  내부 구현:      │                      │  내부 구현:       │
│  - SQL? NoSQL?   │                     │  - bcrypt? argon2?│
│  - 캐시? 직접?   │                      │  - 외부 서비스?   │
│                  │                     │                   │
│  자유롭게 변경   │                      │  자유롭게 변경    │
└──────────────────┘                     └──────────────────┘

계약(Interface)만 유지하면:
- 내부는 자유롭게 바꿀 수 있다
- 다른 서비스에 영향 없다
- System 1이 활성화되어도 피해가 한 서비스로 제한된다
```

**이것이 SOLID를 "왜" 지켜야 하는지의 진짜 이유다:**

구현 단계에서 요구사항이 자주 변경됨   
→ 각 서비스를 인터페이스로 정의 (교체 가능)   
→ 요구사항 변경 = 서비스 교체   
→ 다른 부분 영향 없음   
→ System 1의 나쁜 긴급 결정 불필요   

- Single Responsibility: 각 서비스는 하나의 책임 → 변경 범위 최소화
- Open-Closed: 확장에 열림, 수정에 닫힘 → 기존 코드 보호
- Liskov Substitution: 서비스 교체 가능 → 유연한 대응
- Interface Segregation: 필요한 인터페이스만 → 불필요한 의존 제거
- Dependency Inversion: 인터페이스에 의존 → 변경 격리

----------

### 구현 단계의 문제

**압박 상황에서 (Kahneman + Meadows):**

- 상황:
	- 마감일 압박
	- 요구사항 변경
	- 시간 부족

- Kahneman 분석:
	→ 스트레스
	→ System 1 지배
	→ "빨리 해결해야 해!"

- Meadows 분석:
	→ 부분만 보기
	→ 전체 시스템 놓침
	→ Missing 발견

- 결합 효과:
	Missing 발견 + System 1 활성
	→ "어떻게 만들지?"
	→ 첫 번째 떠오른 해결책
	→ "전역 변수!", "싱글톤!"
	→ 기술 부채

### 서비스 체인 개념 (Systems Thinking)

**서비스 체인이란:**

- Meadows의 시스템 사고 적용:
	시스템 = 서비스들의 네트워크
	- 각 서비스 = 시스템의 노드
	- 호출 관계 = 시스템의 엣지
	- 데이터 흐름 = 시스템의 피드백

예: 사용자 로그인 시스템
```
┌─────────────────────────────────┐
│ 로그인 시스템 (전체)            │
├─────────────────────────────────┤
│                                 │
│ ValidateCredentialsFormat       │
│         ↓                       │
│ FindUserByUsername              │
│         ↓                       │
│ VerifyPassword                  │
│         ↓                       │
│ CheckAccountStatus              │
│         ↓                       │
│ CreateSession                   │
│         ↓                       │
│ StoreSession                    │
│         ↓                       │
│ GenerateSessionToken            │
│         ↓                       │
│ LogLoginEvent                   │
│                                 │
└─────────────────────────────────┘
```
- 특징:
	- 각 서비스: 명확한 책임 (단일)
	- 연결: 명시적 호출 관계
	- 전체: 완전한 시스템
	- Missing 없음: 모든 요소 정의됨



**레버리지 포인트 (Meadows):**

- Meadows의 교훈:
	"시스템의 구조가 행동을 결정한다"
	"가장 강력한 개입점은 구조 수준이다"

- ROD 적용: 
	- 설계 단계 = 구조 정의
		- 모든 서비스 정의
		- 모든 관계 정의
		- 인터페이스 정의
	- 구현 단계 = 행동 구현
		- 이미 정의된 구조 따르기
		- 구조 변경 최소화

- 효과:
	설계 단계 2시간 투자 →
	구현 단계 10시간 절약


### Constructor/Static 금지 규칙

**TRIZ 관점 - 사전 보호 원리:**

- Prior Counteraction (사전 보호):
	"문제를 일으킬 수 있는 것을 미리 차단"

- Constructor/Static의 문제:
	1. System 1의 도피처
	   "빨리 만들어야 해"
	   → "new User()하면 되지"
	   → 서비스 설계 건너뛰기
	   
	2. Missing 은폐
	   "User가 어디서 오지?"
	   → 질문이 안 생김
	   → Missing 발견 못 함

- ROD 강제:
	- Constructor/Static 금지
	→ "User가 필요? UserFinderService 만들어"
	→ Missing 발견 강제
	→ 완전한 서비스 체인

**Kahneman 관점:**

Constructor 허용 = System 1에게 쉬운 길 제공

- 설계 단계 (System 2):
	- 개발자: "User가 필요하네"
	        "음... 어떻게 얻지?"
	        "DB? API? 캐시?"
	        → 생각 안 함 (Constructor 있으니까)

- 구현 단계 (System 1):
	- "User 필요!"
	→ "new User()" (즉각 반응)
	→ 생각 없는 결정

- ROD 강제 = System 1의 쉬운 길 차단
	→ System 2 사고 강제
	→ 완전한 설계

### "Missing" 찾는 방법 (Systems Thinking)

**시스템 분석 기법:**

- Meadows의 질문:
"시스템을 완전히 이해하려면?"

1. 모든 요소 식별
   ❓ "이 객체는 어디서 오지?"
   → 서비스 없으면 Missing!
   
2. 모든 관계 식별
   ❓ "이 데이터는 어떻게 얻지?"
   → 서비스 없으면 Missing!
   
3. 모든 변환 식별
   ❓ "이건 어떻게 만들지?"
   → 서비스 없으면 Missing!
   
4. 모든 저장소 식별
   ❓ "이건 어떻게 저장하지?"
   → 서비스 없으면 Missing!
   
5. 모든 검증 식별
   ❓ "이건 어떻게 검증하지?"
   → 서비스 없으면 Missing!
   
6. 모든 에러 처리 식별
   ❓ "에러는 어떻게 처리하지?"
   → 서비스 없으면 Missing!

### 실전 예시: 사용자 로그인

**❌ Missing 있는 설계 (불완전한 시스템):**

- Service Chain:
	1. ValidateCredentials(username, password)
	2. CreateSession(user)  // ← user는 어디서?

- Systems Thinking 분석:
	- 요소 누락: User 객체
	- 관계 누락: 어떻게 User를 얻는가
	- 변환 누락: password 검증
	- 검증 누락: account 상태

- 결과:
→ 불완전한 시스템
→ 예측 불가능한 행동
→ 구현 중 혼란

**✅ ROD (완전한 시스템):**

완전한 서비스 체인:

1. ValidateCredentialsFormat(username, password)
   역할: 입력 형식 검증
   Systems Thinking: 경계 검증 (시스템 입구)
   
2. FindUserByUsername(username)
   역할: 사용자 조회
   Systems Thinking: 데이터 검색 요소
   
3. VerifyPassword(user, password)
   역할: 비밀번호 검증
   Systems Thinking: 보안 검증 요소
   
4. CheckAccountStatus(user)
   역할: 계정 상태 확인
   Systems Thinking: 상태 검증 요소
   
5. CreateSession(user)
   역할: 세션 생성
   Systems Thinking: 상태 생성 요소
   
6. StoreSession(session)
   역할: 세션 저장
   Systems Thinking: 영속성 요소
   
7. GenerateSessionToken(session)
   역할: 토큰 생성
   Systems Thinking: 출력 변환 요소
   
8. LogLoginEvent(user, timestamp)
   역할: 로그인 이벤트 기록
   Systems Thinking: 피드백/모니터링 요소

- Systems Thinking 검증:  
✓ 모든 요소 식별됨  
✓ 모든 관계 명확함  
✓ 시스템 경계 정의됨  
✓ 피드백 루프 있음  
✓ Missing 없음  

- TRIZ 검증:  
✓ 사전 조치 완료  
✓ 분할 원리 적용  
✓ 자체 완결적  

- Kahneman 검증:  
✓ System 2로 설계됨  
✓ 구현 중 System 1 폭주 방지  

### 실전 예시: 결제 시스템

**TRIZ + Systems Thinking 통합:**

- TRIZ 분석:
	- 문제: 결제 시스템은 복잡하고 에러 가능성 많음
	- 원칙: 분할 + 사전 조치 + 사전 보호

- Systems Thinking 분석:
	- 여러 하위 시스템
	- 복잡한 관계
	- 많은 피드백 루프
	- 외부 시스템 의존

- 통합 설계:
```
┌─────────────────────────────────────┐
│ 결제 시스템 (전체)                  │
├─────────────────────────────────────┤
│                                     │
│ 입력 검증 서브시스템:               │
│ 1. ValidatePaymentRequest           │
│ 2. FindUserAccount                  │
│ 3. CheckUserPaymentEligibility      │
│                                     │
│ 계산 서브시스템:                    │
│ 4. CalculateTotalAmount             │
│ 5. CheckInventoryAvailability       │
│                                     │
│ 예약 서브시스템:                    │
│ 6. ReserveInventory                 │
│                                     │
│ 결제 처리 서브시스템:               │
│ 7. ValidatePaymentMethod            │
│ 8. CreatePaymentTransaction         │
│ 9. EncryptPaymentData               │
│ 10. CallPaymentGateway              │
│ 11. DecryptPaymentResponse          │
│ 12. ValidatePaymentResponse         │
│                                     │
│ 상태 관리 서브시스템:               │
│ 13. UpdateTransactionStatus         │
│ 14. ConfirmInventoryReservation     │
│                                     │
│ 출력 서브시스템:                    │
│ 15. CreateOrderRecord               │
│ 16. SendPaymentConfirmation         │
│ 17. UpdateUserPaymentHistory        │
│                                     │
│ 모니터링 서브시스템:                │
│ 18. LogPaymentEvent                 │
│                                     │
└─────────────────────────────────────┘
```
- TRIZ 원리 적용:
	- 분할: 6개 서브시스템으로 분할
	- 사전 조치: 모든 검증을 먼저
	- 사전 보호: 예약 → 확정 단계 분리

- Systems Thinking:
	- 명확한 경계
	- 명시적 관계
	- 피드백 루프 (로그, 상태 업데이트)
	- 전체 시스템 완전함

- Kahneman:
	- System 2로 설계 (복잡도 관리)
	- 구현은 서비스 하나씩
	- DGTF로 통제

### SOLID와의 연결

**Systems Thinking + SOLID:**

- Meadows:
"시스템은 변화한다"
"적응 가능한 시스템이 생존한다"

- SOLID:
"변경에 열려있고 수정에 닫혀있어라"

- ROD + SOLID:  
각 서비스를 인터페이스로 정의  
→ 교체 가능한 요소  
→ 시스템 전체 수정 없이 부분 교체  
→ 적응 가능한 시스템  

예:
```
type PaymentGateway interface {
    Process(PaymentRequest) (PaymentResponse, error)
}

// 구현체들
type StripeGateway struct { ... }
type PayPalGateway struct { ... }
type TossGateway struct { ... }
```
- 요구사항 변경:  
"Stripe에서 Toss로 변경"  
→ 인터페이스는 그대로  
→ 구현체만 교체  
→ 다른 서비스 영향 없음  
→ System 1 긴급 결정 불필요  

### UML과 Design Pattern이 쓸모없다고?

**많은 개발자가 말한다:**

```
"UML 배웠는데 실무에서 안 써요"
"Design Pattern 공부했는데 쓸 데가 없어요"
"학교에서 배운 거랑 실무는 달라요"
```

**그런데 질문:**  

당신은 언제 UML을 그리는가?  
당신은 언제 Design Pattern을 적용하는가?  

```
대부분의 대답:
"코드 짜다가 필요하면..."
"복잡해지면 그때..."
"리팩토링할 때..."
```

**그게 문제다.**

UML과 Design Pattern은 **구현 단계** 도구가 아니다.  
**설계 단계** 도구다.

```
❌ 잘못된 사용:
구현하다가 → "복잡하네" → "UML 그려볼까"
→ 이미 늦음
→ 구조 바꾸기 어려움
→ "UML 쓸모없네"

✅ 올바른 사용 (ROD):
설계 단계 → 서비스 체인 구상 → UML로 표현
설계 단계 → 관계 정의 → Design Pattern 적용
→ 구현은 설계 따르기만
→ "UML/DP 진짜 유용하네"
```

**ROD 단계에서 UML/Design Pattern 활용:**

```
1. 서비스 체인 설계 시:
   → Class Diagram: 서비스 간 관계
   → Sequence Diagram: 호출 순서
   
2. 인터페이스 정의 시:
   → Strategy Pattern: 교체 가능한 구현
   → Factory Pattern: 객체 생성 분리
   → Observer Pattern: 이벤트 처리
   
3. 복잡한 흐름 설계 시:
   → State Diagram: 상태 전이
   → Activity Diagram: 업무 흐름
```

**결론:**

```
UML, Design Pattern이 쓸모없는 게 아니다.
ROD 단계 없이 구현부터 하니까 쓸 곳이 없는 것이다.

설계를 먼저 하면 (ROD)
→ UML이 필요해진다
→ Design Pattern이 자연스럽게 적용된다
→ "아, 이래서 이걸 배운 거구나"
```

----------

### ROD 체크리스트

**이론 기반 검증:**
```
Systems Thinking (Meadows):
□ 모든 요소를 식별했는가?
□ 모든 관계를 정의했는가?
□ 시스템 경계가 명확한가?
□ 피드백 루프가 있는가?
□ 레버리지 포인트를 활용했는가?

TRIZ (Altshuller):
□ 사전 조치를 적용했는가?
□ 분할 원리를 적용했는가?
□ 이상적 최종 결과에 가까운가?

Kahneman:
□ System 2로 설계했는가?
□ System 1 폭주 방지책이 있는가?
□ 구현 중 혼란 요소가 없는가?

실전:
□ Constructor 사용하지 않았는가?
□ Static field 사용하지 않았는가?
□ 가정하지 않았는가?
□ Missing이 없는가?
□ SOLID를 적용했는가?

```


----------

## Part 3: TFD (Test-First Development)

### 핵심 원칙

**"요구사항 = 테스트"**

테스트는:
-   사후 작업이 아닌 명세서 (Systems Thinking: 시스템 명세)
-   구현 가이드
-   완료 기준
-   피드백 루프 (Meadows)


### Test = Requirement: 더 많은 실전 예시

**테스트가 요구사항이다** - 이것을 더 깊이 이해하자.

**예시 1: 결제 시스템**

```
❌ 전통적 요구사항 (모호함):
"사용자가 결제할 수 있어야 한다"

→ 카드 실패하면?
→ 잔액 부족하면?
→ 중복 결제 방지는?
→ 부분 환불은?
```

```go
// ✅ TFD 요구사항 (테스트로 정의):

func TestPaymentService(t *testing.T) {
    // 정상 케이스
    t.Run("ValidPayment_ShouldSucceed", func(t *testing.T) {
        // 올바른 카드, 충분한 잔액 → 결제 성공, 영수증 반환
    })
    
    // 카드 에러
    t.Run("InvalidCard_ShouldReturnCardError", func(t *testing.T) {
        // 잘못된 카드 번호 → CardError, 결제 안 됨
    })
    
    t.Run("ExpiredCard_ShouldReturnExpiredError", func(t *testing.T) {
        // 만료된 카드 → ExpiredCardError
    })
    
    // 잔액 에러
    t.Run("InsufficientFunds_ShouldReturnFundsError", func(t *testing.T) {
        // 잔액 부족 → InsufficientFundsError, 결제 안 됨
    })
    
    // 중복 방지
    t.Run("DuplicatePayment_ShouldReturnDuplicateError", func(t *testing.T) {
        // 같은 주문 2번 결제 시도 → DuplicatePaymentError
    })
    
    // 부분 환불
    t.Run("PartialRefund_ShouldRefundPartialAmount", func(t *testing.T) {
        // 10000원 중 3000원 환불 → 3000원 환불, 7000원 유지
    })
    
    // 전체 환불
    t.Run("FullRefund_ShouldRefundFullAmount", func(t *testing.T) {
        // 전체 금액 환불 → 전체 환불, 주문 취소 상태
    })
    
    // 동시성
    t.Run("ConcurrentPayments_ShouldHandleCorrectly", func(t *testing.T) {
        // 동시에 같은 주문 결제 → 하나만 성공
    })
}

// 이 테스트들이 요구사항이다.
// PM이 "중복 결제 방지해주세요"라고 하면?
// → DuplicatePayment 테스트가 통과하면 완료.
```

**예시 2: 검색 시스템**

```
❌ 전통적 요구사항:
"사용자가 상품을 검색할 수 있어야 한다"

→ 결과 없으면?
→ 오타 교정은?
→ 정렬 기준은?
→ 페이지네이션은?
→ 필터링은?
```

```go
// ✅ TFD 요구사항:

func TestSearchService(t *testing.T) {
    // 정상 케이스
    t.Run("ValidQuery_ShouldReturnResults", func(t *testing.T) {
        // "노트북" 검색 → 노트북 상품 목록 반환
    })
    
    // 결과 없음
    t.Run("NoResults_ShouldReturnEmptyList", func(t *testing.T) {
        // "asdfqwer" 검색 → 빈 목록, 에러 아님
    })
    
    // 오타 교정
    t.Run("Typo_ShouldSuggestCorrection", func(t *testing.T) {
        // "노트붘" 검색 → "노트북으로 검색하시겠습니까?" 제안
    })
    
    // 정렬
    t.Run("SortByPrice_ShouldReturnSortedResults", func(t *testing.T) {
        // 가격순 정렬 → 가격 오름차순 결과
    })
    
    t.Run("SortByRelevance_ShouldReturnRelevantFirst", func(t *testing.T) {
        // 관련도순 정렬 → 관련도 높은 것 먼저
    })
    
    // 페이지네이션
    t.Run("Pagination_ShouldReturnCorrectPage", func(t *testing.T) {
        // 2페이지 요청 → 21-40번째 결과
    })
    
    t.Run("LastPage_ShouldReturnRemainingItems", func(t *testing.T) {
        // 마지막 페이지 → 남은 항목들, hasNext=false
    })
    
    // 필터링
    t.Run("PriceFilter_ShouldFilterByPrice", func(t *testing.T) {
        // 10만원-50만원 필터 → 해당 가격대만
    })
    
    t.Run("CategoryFilter_ShouldFilterByCategory", func(t *testing.T) {
        // 전자제품 필터 → 전자제품만
    })
    
    // 조합
    t.Run("MultipleFilters_ShouldApplyAll", func(t *testing.T) {
        // 가격 + 카테고리 + 정렬 → 모두 적용된 결과
    })
}
```

**핵심:**

```
테스트 = 요구사항 = 계약

요구사항이 바뀌면?
→ 테스트를 바꾼다.
→ 테스트가 통과하면 요구사항을 만족한 것이다.

PM: "검색 결과 없을 때 추천 상품 보여주세요"
→ 새 테스트 추가: NoResults_ShouldShowRecommendations
→ 테스트 통과시키면 완료

진실의 단일 원천 (Single Source of Truth):
- ❌ 요구사항 문서 (오래됨, 모호함)
- ❌ 코드 주석 (업데이트 안 됨)
- ✅ 테스트 (실행 가능, 항상 최신, 명확)
```

----------

### 이론적 배경

**Systems Thinking 관점 (Meadows):**

- 피드백 루프:
"시스템의 출력이 입력에 영향을 준다"

- TFD의 피드백:
테스트 → 구현 → 검증 → 개선 → 테스트
```
┌──────────────────────────────┐
│                              │
│   ┌─────┐      ┌─────┐       │
│   │테스트│ ──→ │구현 │       │
│   └─────┘      └─────┘       │
│      ↑            ↓          │
│      │         ┌─────┐       │
│      └──────── │검증 │       │
│                └─────┘       │
│                              │
└──────────────────────────────┘
```
- 시스템 이론:
	- 빠른 피드백 = 빠른 학습
	- 명확한 기준 = 명확한 행동
	- 지속적 검증 = 안정적 시스템

**TRIZ 관점:**

- 사전 조치 원리:
"문제가 발생하기 전에 해결"

- TFD 적용:
	- 테스트 먼저 = 요구사항 명확화
	- 구현 전 검증 기준 정의
	- 버그 사전 방지

- 역발상 원리:
	"전통: 구현 → 문제 → 해결"
	"TFD: 문제 예측 → 테스트 → 구현"

- 셀프 서비스 원리:
	"테스트가 스스로 검증"
	→ 사람의 수동 검증 불필요
	→ 자동화된 피드백

**Kahneman 관점:**

- System 2 활용:
	테스트 설계 = 신중한 사고
	- "어떤 케이스가 있을까?"
	- "어떤 에러가 발생할까?"
	- "엣지 케이스는?"

- 구현 중 안전망:
	테스트 = System 1의 실수 감지
	- 빠르게 코딩해도
	- 테스트가 실수 잡아줌
	- 자신감 제공

----------

### TFD와 TDD의 관계

**TDD를 시도해본 적 있는가?**

```
시도 1:
"테스트 먼저 쓰래서..."
→ "뭘 테스트하지?"
→ 막막함
→ "일단 구현하고 테스트 쓸까..."
→ 포기

시도 2:
"Red → Green → Refactor!"
→ 첫 테스트 작성
→ "이게 맞는 건가?"
→ 구현하다 보니 테스트 구조 다 바뀜
→ "TDD 너무 어렵다"
→ 포기

시도 3:
"간단한 거부터..."
→ 유틸 함수 테스트 작성
→ "이건 쉽네!"
→ 복잡한 비즈니스 로직
→ "여기서 뭘 테스트하지?"
→ 포기
```

**TDD가 어려운 진짜 이유:**

```
TDD의 전제:
"테스트가 설계를 이끈다"

현실:
설계 없이 테스트부터?
→ "뭘 테스트해야 하는지" 모름
→ 테스트 구조가 계속 바뀜
→ "내가 뭘 만들고 있는 거지?"
```

**TFD는 TDD를 부정하지 않는다.
TFD는 TDD를 더 쉽게 만든다.**

```
TDD:
테스트 → 구현 → 리팩토링
"테스트가 설계를 이끈다"

TFD:
설계(ROD) → 테스트 → 구현
"설계가 테스트를 정의한다"
```

**TFD가 쉬운 이유:**

```
ROD 완료 후:
LoginService
├── UserFinder.FindByUsername()
├── PasswordVerifier.Verify()
├── SessionCreator.Create()
└── TokenGenerator.Generate()

각 서비스의 입력/출력이 명확하다.
"이 서비스의 계약을 테스트하면 된다"

UserFinder 테스트:
- 입력: username (string)
- 출력: *User 또는 error
- 테스트: 존재하는 유저, 없는 유저, 빈 문자열...

뭘 테스트할지 명확하다.
```

**결론:**

```
TDD를 잘 하고 있다면?
→ 축하한다. 계속하라.

TDD가 어려웠다면?
→ ROD 먼저 해보라.
→ 서비스 체인이 있으면 테스트가 뭘 써야 하는지 보인다.
```




### ROD와 TFD의 결합

**통합 프로세스 (세 이론 적용):**

- 1단계: ROD (Systems Thinking + Kahneman)
	완전한 서비스 체인 설계
	- System 2로 설계
	- 전체 시스템 파악
	- Missing 제거

- 2단계: TFD (Meadows + TRIZ)
	각 서비스마다 테스트 케이스 설계
	- 피드백 루프 설계
	- 사전 조치 (문제 예측)
	- 완료 기준 정의

- 3단계: 구현 (Kahneman + DGTF)
	테스트를 통과하도록 구현
	- DGTF로 System 1 통제
	- 테스트로 안전망 확보
	- 지속적 검증

- 4단계: 검증 (Meadows)
	모든 테스트 통과 = 완료
	- 피드백 확인
	- 시스템 완전성 검증

**시너지 효과:**

- ROD:
	"무엇을 만들 것인가" 정의
	→ 완전한 서비스 체인

- TFD:
	"올바르게 작동하는가" 검증
	→ 각 서비스의 기준

- 둘이 함께:
	= 완전한 명세서
	= 실행 가능한 문서
	= 자동 검증 시스템

### 적용 방법 (4단계)

**1단계: 서비스별 테스트 케이스 정의 (System 2)**

- 각 ROD 서비스마다 신중히 분석:

	- Systems Thinking 질문:
		- 이 서비스의 입력은?
		- 이 서비스의 출력은?
		- 어떤 변환이 일어나는가?
		- 다른 서비스와 어떻게 연결되는가?

	- Kahneman 질문 (System 2):
		- 정상적인 경우는?
		- 비정상적인 경우는?
		- 경계값은?
		- 극단적인 경우는?

	- TRIZ 질문:
		- 어떤 모순이 있는가?
		- 어떤 에러가 발생할 수 있는가?

- 테스트 케이스:
	1. 정상 케이스 (Happy Path)
	   - 올바른 입력 → 기대 출력
	   
	2. 에러 케이스
	   - 잘못된 입력 → 적절한 에러
	   - 각 에러 타입별로
	   
	3. 엣지 케이스
	   - 경계값
	   - 극단적 입력
	   - 예외 상황
	   
	4. 통합 케이스
	   - 다른 서비스와의 상호작용
	   
	5. 성능 케이스 (필요시)
	   - 응답 시간
	   - 처리량

**2단계: 테스트 구조 작성**

```go
// Systems Thinking: 전체 테스트 시스템 구조
func TestUserService(t *testing.T) {
    // 정상 케이스
    t.Run("ValidInput_ShouldSucceed", func(t *testing.T) {
        // TODO: Kahneman System 2로 신중히 작성
    })
    
    // 에러 케이스
    t.Run("InvalidEmail_ShouldReturnError", func(t *testing.T) {
        // TODO: TRIZ 사전 조치 - 에러 예측
    })
    
    t.Run("EmptyPassword_ShouldReturnError", func(t *testing.T) {
        // TODO
    })
    
    // 엣지 케이스
    t.Run("VeryLongEmail_ShouldHandleCorrectly", func(t *testing.T) {
        // TODO
    })
    
    // 통합 케이스
    t.Run("WithDatabaseFailure_ShouldHandleGracefully", func(t *testing.T) {
        // TODO: Systems Thinking - 연결 실패
    })
}

```

**3단계: 테스트 하나씩 구현 (DGTF 적용)**

```go
func TestUserService(t *testing.T) {
    t.Run("ValidInput_ShouldSucceed", func(t *testing.T) {
        // 🛑 DGTF: 천천히, 신중하게
        
        // Arrange (준비)
        // Systems Thinking: 시스템 설정
        service := NewUserService(mockRepo)
        email := "user@example.com"
        password := "ValidPass123!"
        
        // Act (실행)
        result, err := service.CreateUser(email, password)
        
        // Assert (검증)
        // Meadows: 피드백 확인
        if err != nil {
            t.Errorf("Expected no error, got %v", err)
        }
        if result == nil {
            t.Error("Expected result, got nil")
        }
        if result.Email != email {
            t.Errorf("Expected email %s, got %s", 
                email, result.Email)
        }
        
        // ✓ 테스트 실행
        // ✓ Red 확인
        // ✓ 구현
        // ✓ Green 확인
        // ✓ 다음 테스트
    })
}

```

**4단계: 구현하며 테스트 통과 (피드백 루프)**

- Meadows 피드백 루프:

- 반복:
	1. 테스트 실행 → 실패 (Red)
	   피드백: "아직 구현 안 됨"
	   
	2. 최소한의 코드 작성
	   DGTF: 신중하게
	   
	3. 테스트 실행 → 성공 (Green)
	   피드백: "이 케이스 완료"
	   
	4. 다음 테스트로
	   Systems Thinking: 다음 요소

- 전체 완료:
	→ 모든 테스트 통과
	→ 완전한 시스템
	→ 지속적 피드백 확보


### TFD 체크리스트

**이론 기반 검증:**
```
Systems Thinking (Meadows):
□ 피드백 루프가 설계됐는가?
□ 전체 시스템을 커버하는가?
□ 통합 테스트가 있는가?
□ E2E 테스트가 있는가?

TRIZ (Altshuller):
□ 사전 조치로 에러를 예측했는가?
□ 모든 모순 케이스를 테스트하는가?
□ 역발상으로 접근했는가? (구현 전 테스트)

Kahneman:
□ System 2로 테스트를 설계했는가?
□ 모든 엣지 케이스를 고려했는가?
□ 테스트가 System 1의 실수를 잡아주는가?

실전:
□ ROD 서비스 체인이 완성됐는가?
□ 각 서비스마다 테스트가 있는가?
□ 정상/에러/엣지 케이스를 포함하는가?
□ 테스트가 독립적인가?
□ 테스트 커버리지가 충분한가? (>80%)

```


----------

## Part 4: DGTF (Don't Go Too Fast)

### 핵심 원칙

**"Slow is smooth, smooth is fast"**

신중함은:
-   느린 게 아니라 부드러운 것 (TRIZ: 모순 해결)
-   부드러우면 결국 빠름
-   재작업이 없기 때문

### 이론적 배경

**Kahneman 관점:**

- 핵심 문제:
	압박 상황 → System 1 활성화 → 나쁜 결정

- DGTF 해결:
	압박 속에서도 System 2 활성화
	→ 신중한 판단
	→ 좋은 결정

- 방법:
	1. 인식: System 1 트리거 감지
	2. 멈춤: "잠깐, 생각하자"
	3. 활성화: System 2 ON
	4. 진행: 신중하게

**TRIZ 관점:**

- 모순:
"빠르게" vs "신중하게"

- 전통적 타협:
	- 빠르게 → 실수 많음
	- 신중하게 → 너무 느림

- TRIZ 해결: 시간 분리   
	- 단계별 분리:
		  - 설계: 느리고 신중 (System 2)
		  - 구현: 빠르되 검증 (DGTF)
  	- 반복별 분리:
		 - 각 반복: 신중히 (25분)
		 - 사이 휴식: 평가 (5분)

- 결과:
	→ 빠르면서도 신중
	→ 모순 해결

**Systems Thinking 관점:**

- Meadows의 교훈:
	"서두르면 피드백을 놓친다"

- DGTF 적용:
	지속적 피드백 확인
	- 각 단계마다 검증
	- 테스트 실행
	- 영향 분석

- 시스템 행동:
	신중한 진행 = 안정적 피드백
	→ 조기 에러 발견
	→ 빠른 수정
	→ 전체적으로 빠름

### 압박 상황에서의 인간 심리

**Kahneman 상세 분석:**

- 트리거 (System 1 활성화):
	- 내적 트리거:
		- "빨리 끝내야 해"
		- "이거 간단한데 뭐"
		- "대충 하면 되겠지"
		- "테스트는 나중에"
		- "확인할 시간 없어"
	- 외적 트리거:
		- "언제 끝나요?"
		- "내일까지 가능해요?"
		- "간단한 거잖아요"
		- "다른 사람은 금방 했는데"
		- "데모가 내일이에요"

- System 1 활성화 신호:
	- 신체적:
		- 심박수 증가
		- 얕은 호흡
		- 긴장된 어깨
		- 땀
	- 정신적:
		- 초조함
		- "빨리" 생각 반복
		- 터널 비전 (부분만 보임)
		- 다른 생각 불가능
	- 행동적:
		- 서두르기
		- 확인 건너뛰기
		- 문서 안 읽기
		- 테스트 생략
		- 첫 번째 해결책 선택

- System 1의 전형적 결정:
	- "일단 전역 변수!"
	- 	"싱글톤이면 되겠지!"
	- 	"여기 하드코딩하자!"
	- 	"나중에 고치면 돼!"



### 운전 비유 (세 이론 통합)

**운전면허 ≠ 좋은 운전자**

- 나쁜 운전자 (System 1 - Kahneman):
	- 행동:
		- 빠르게 끼어들기
		- 신호 확인 안 함
		- 공격적 운전
		- "빨리 가야 해!"

	- Systems Thinking (Meadows):
		- 전체 교통 흐름 안 봄
		- 다른 차와의 관계 무시
		- 피드백 (경적, 브레이크) 무시
	- 결과:
		→ 사고 (모순 악화 - TRIZ)
		→ 스트레스
		→ 전체적으로 느림 (사고 처리)

- 좋은 운전자 (System 2 + DGTF):
	- 행동:
		- 미리 확인
		- 일관된 속도
		- 안전거리 유지
		- 예측 운전
	- Systems Thinking:
		- 전체 교통 흐름 파악
		- 다른 차 고려
		- 피드백에 반응
	- TRIZ:
		- 모순 해결 (빠름 vs 안전)
		- 시간 분리 (상황별 속도 조절)
	- 결과:
		→ 사고 없음
		→ 편안함
		→ 전체적으로 빠름

**프로그래밍도 동일:**

- 코딩 능력 ≠ 좋은 프로그래머
- 좋은 프로그래머 =
	```
	  기술 능력 (Skill)
	  + 좋은 습관 (DGTF)
	  + 신중한 태도 (Kahneman System 2)
	  + 시스템 사고 (Meadows)
	  + 문제 해결 (TRIZ)
	```

### 자기 통제가 Professional을 만든다

**DGTF의 핵심은 외부가 아니라 내부다.**

```
❌ 잘못된 이해:
"DGTF = 매니저에게 시간 더 달라고 하기"
"DGTF = 일정 협상 기술"
"DGTF = 외부 환경 바꾸기"

✅ 올바른 이해:
"DGTF = 자기 자신의 System 1 통제"
"DGTF = 내면의 '빨리!' 충동 관리"
"DGTF = 스스로를 통제하는 습관"
```

**Robert Martin의 "Clean Coder"는 말한다:**

```
"No라고 말할 수 있어야 한다"
"압박에 굴복하지 마라"
"Professional이 되어라"
```

**그런데 "어떻게"?**

Clean Coder는 "무엇을" 해야 하는지 알려준다.  
DGTF는 "어떻게" 할 수 있는지 알려준다.  

**DGTF가 그 "어떻게"다:**

```
1. System 1의 "빨리!" 충동을 인식
   → "지금 서두르고 있나?"
   → "심장이 빨라졌나?"
   → "터널 비전인가?"

2. 멈추고 System 2 활성화
   → 심호흡
   → 키보드에서 손 떼기
   → "잠깐, 생각하자"

3. 신중하게 판단하고 진행
   → ROD 문서 확인
   → TFD 테스트 확인
   → 하나씩 검증하며 진행
```

**자기 통제의 결과:**

```
자신을 통제할 수 있는 사람
    ↓
압박 속에서도 품질 유지
    ↓
결과로 신뢰를 얻음
    ↓
Clean Coder의 "Professional"
    ↓
협상이 필요 없어짐 (신뢰가 있으니까)
```

**실전 시나리오:**

```
상황:
금요일 오후 4시
매니저: "이 버그 오늘 안에 고쳐주세요"
마음속: "빨리! 빨리! 빨리!"

❌ System 1 반응:
→ 코드 열기
→ 문제 있어 보이는 곳 수정
→ "돌아가네!"
→ 커밋 & 푸시
→ (월요일: 다른 버그 발생)

✅ DGTF 반응:
→ "잠깐." (System 1 인식)
→ 심호흡 (멈춤)
→ "이 버그의 원인이 뭐지?" (System 2 활성화)
→ 로그 확인, 테스트 확인
→ 원인 파악
→ 테스트 추가
→ 수정
→ 테스트 통과 확인
→ 커밋 & 푸시
→ (월요일: 안정적)
```

**Professional은 결과로 신뢰를 얻는다:**

```
3개월 후:

DGTF 없는 개발자:
- 빠른 수정, 잦은 버그
- 매니저: "또 버그야?"
- 신뢰 ↓
- 더 많은 압박
- 악순환

DGTF 있는 개발자:
- 신중한 수정, 안정적 결과
- 매니저: "저 사람 믿을 수 있어"
- 신뢰 ↑
- 자율성 ↑
- 선순환
```

----------

### DGTF 워크플로우 (세 이론 통합)

**5단계 프로세스:**
- 1단계: 인식 (Recognize) - Kahneman
	"지금 서두르고 있나?"

	체크:
	- 심박수 증가?
	- "빨리" 생각 반복?
	- 확인 건너뛰고 싶음?

	→ System 1 트리거 확인
	→ 위험 인식

- 2단계: 멈춤 (Pause) - DGTF
	"잠깐, 멈추자"

	행동:
	- 손을 키보드에서 떼기
	- 심호흡 3회
	- 5초 기다리기
	- "무엇이 나를 재촉하는가?" 질문

	→ System 1 억제
	→ System 2 활성화 준비

- 3단계: 확인 (Check) - Systems Thinking + ROD + TFD
	"전체 시스템 확인"

	- 질문:
		- ROD 설계 문서를 봤나?
		- TFD 테스트 케이스를 확인했나?
		- 이게 다른 부분에 영향을 주나? (Systems Thinking)
		- 어떤 피드백이 있나? (Meadows)
		- 모순이 있나? (TRIZ)

	→ 전체 시스템 파악
	→ Missing 확인
	→ 영향 분석

- 4단계: 계획 (Plan) - System 2
	"신중히 계획"

	- 질문:
		- 어떤 순서로 할까?
		- 어떻게 테스트할까?
		- 예상 시간은?
		- 막히면 누구에게 물어볼까?

	→ 명확한 계획
	→ 예측 가능한 진행

- 5단계: 실행 (Execute) - DGTF + 피드백
	"신중히 진행"

	- 행동:
	→ 한 서비스씩 구현
	→ 각 단계마다 테스트
	→ 피드백 확인 (Meadows)
	→ 검증하며 진행
	→ 문제 발견 시 → 2단계 (멈춤)로

### 커뮤니케이션 (세 이론 적용)

**매니저/고객과:**
- ❌ 나쁜 대응 (System 1):
"네, 바로 할게요!"
→ 계획 없음
→ 재작업
→ 신뢰 하락

- ✅ 좋은 대응 (System 2 + Systems Thinking):
	- 1단계: 인식
	"이건 중요한 요청이다"
	"신중히 답해야 한다"

	- 2단계: 시간 확보
	"확인 후 정확한 일정 드리겠습니다"
	→ System 2 활성화 시간 확보

	- 3단계: 분석 (30분)
		- ROD 관점: 얼마나 많은 서비스?
		- Systems Thinking: 다른 부분 영향?
		- TFD: 테스트 범위는?
		- TRIZ: 어떤 모순?

	- 4단계: 정직한 추정
	"3일 필요합니다"

	- 5단계: 설명 (비즈니스 가치)
		- "신중하게 하면:
			 - 버그 없음 → 고객 만족
			 - 재작업 없음 → 비용 절감
			 - 안정적 → 위험 감소"
		 - "급하게 하면:
			 - 버그 → 긴급 수정
			 - 재작업 → 더 오래 걸림
			 - 기술 부채 → 미래 속도 저하"
		- "결과:
			→ 신뢰 구축
			→ 현실적 일정
			→ 성공적 배포"

**일정 협상 (TRIZ 모순 해결):**

- 모순:
	"빠른 배포" vs "높은 품질"

- TRIZ 해결:
	1. 조건 분리:
	   간단한 기능 → 빠르게 (2일)
	   복잡한 기능 → 신중하게 (5일)

	2. 시간 분리:
	   Phase 1: 핵심 기능만 (3일)
	   Phase 2: 추가 기능 (2일)

	3. 시스템 레벨 해결:
	   "X일 필요합니다.
	   
	   하지만 시스템적으로 접근하면:
	   - 자동화 도구 도입 (초기 투자)
	   - 재사용 가능한 컴포넌트
	   - 다음엔 더 빠름
	   
	   장기적으로 이득입니다"

### "DGTF ≠ 느림" (TRIZ 모순 해결)

**역설:**

- TRIZ 분석:
	겉보기 모순: "신중함" vs "빠름"
	실제: 모순 아님 (시간 분리로 해결됨)
```
┌──────────────────────────────────────┐
│ 서두르는 개발자 (System 1)           │
├──────────────────────────────────────┤
│ Day 1: 3시간 빠른 코딩 (테스트 없음) │
│ Day 2: 2시간 버그 수정               │
│ Day 3: 2시간 또 버그 수정            │
│ Day 4: 3시간 리팩토링                │
│ Day 5: 1시간 테스트 추가             │
│ ─────────────────────────────────    │
│ 총: 11시간, 불안정                   │
│ Systems Thinking: 피드백 무시        │
└──────────────────────────────────────┘

┌──────────────────────────────────────┐
│ DGTF 개발자 (System 2 + 피드백)      │
├──────────────────────────────────────┤
│ Day 1: 1시간 ROD 설계                │
│        + 1시간 TFD 테스트 설계       │
│ Day 2: 2시간 신중한 구현 (DGTF)      │
│ Day 3: 2시간 통합 테스트             │
│ Day 4: 1시간 E2E 테스트              │
│ Day 5: 1시간 문서화                  │
│ ─────────────────────────────────    │
│ 총: 7시간, 안정적                    │
│ Systems Thinking: 지속적 피드백      │
│ TRIZ: 모순 해결됨                    │
└──────────────────────────────────────┘

DGTF가 4시간 (36%) 절약!
```
- Meadows 분석:
	"서두르면 피드백 루프가 길어진다"
	"신중하면 피드백이 즉각적이다"
	→ 전체 시스템이 더 빠르게 수렴

### 안티패턴 (세 이론으로 분석)

**1. 자아 ("난 잘해") - Kahneman**
- ❌ 증상:
	"나는 경험 많아, 확인 안 해도 돼"

- Kahneman 분석:
	- System 1의 과신
	- 확증 편향
	- 능력 착각

- 결과:
	→ 실수 증가
	→ 신뢰 하락

- ✅ 해결:
	"경험 많을수록 더 신중해야 해"
	"겸손 = 전문성의 표시"

- System 2 활용:
	- "내가 놓친 게 없을까?"
	- "다른 사람 의견은?"
	- "리뷰 받자"

**2. 압박 ("빨리빨리")**

- ❌ 증상:
	"빨리 해야 해, 대충!"

- Kahneman:  System 1 지배
	Systems Thinking: 부분만 봄
	TRIZ: 나쁜 타협

- 결과:
	→ 기술 부채
	→ 더 오래 걸림

- ✅ 해결:
"신중하게 해서 한 번에"

- TRIZ: 모순 해결
	- 시간 분리
	- 단계별 접근
- Meadows: 피드백 활용
	- 지속적 검증
	- 조기 발견

**3. 추진력 중심 - Systems Thinking**

- ❌ 증상:
	"일단 시작하고 보자"
	"해보면서 배우지 뭐"

- Systems Thinking 문제:
	- 전체 시스템 안 봄
	- 관계 파악 안 함
	- Missing 발견 못 함

- 결과:
	→ 중간에 막힘
	→ 재시작 반복

- ✅ 해결:
	"계획하고 시작하자"

- ROD: 완전한 설계
	Systems Thinking: 전체 파악
	DGTF: 신중한 진행

### 실천 팁 (통합 적용)

**1. 타이머 사용 (피드백 루프):**

- 포모도로 기법 + DGTF:
	- 25분 집중 (System 2):
		- 한 서비스 구현
		- 방해 받지 않기
		- DGTF 적용

	- 5분 휴식 (피드백 - Meadows):
		□ "서두르고 있었나?" (Kahneman)
		□ 테스트 실행 (TFD)
		□ 코드 리뷰 (자신)
		□ 전체 시스템 확인 (Systems Thinking)
		□ 다음 작업 계획
		□ 모순 있나? (TRIZ)

	- 4번 반복 후:
		15-30분 긴 휴식
	- 전체 진행 상황 리뷰
	- ROD 설계 재확인
	- 방향 수정 필요 여부


**2. 체크리스트 (세 이론 통합):**

```
코딩 시작 전 (System 2):
□ 요구사항 이해? (명확히)
□ ROD 설계 확인? (Systems Thinking)
□ TFD 테스트 확인? (피드백)
□ 구현 방법 명확? (계획)
□ 모순 해결책? (TRIZ)

코딩 중 (매 25분 - DGTF):
□ 설계 따르고 있나? (ROD)
□ 서두르고 있나? (Kahneman - 위험!)
□ 테스트 작성 중? (TFD)
□ 피드백 확인? (Meadows)
□ 30분 이상 막혔으면 → 질문!

커밋 전 (최종 검증):
□ 모든 테스트 통과? (TFD)
□ 코드 리뷰 (자신)? (System 2)
□ 불필요한 코드 제거? (TRIZ - 이상성)
□ 시스템 영향 확인? (Systems Thinking)
□ 문서 업데이트?

```

**3. 버디 시스템 (피드백):**

- 동료와 짝 (Meadows - 피드백):

- 아침 (계획):
	"오늘 뭐 할 거야?"
	→ ROD 설계 공유
	→ 서로 확인
	→ Missing 체크

- 점심 (중간 점검):
	"어떻게 되가?"
	→ 진행 상황
	→ "서두르고 있나?" 체크
	→ 막힌 부분 상의

- 저녁 (회고):
	"오늘 어땠어?"
	→ System 1 폭주 순간
	→ 배운 점
	→ 내일 계획

**4. 회고 (지속적 개선):**

- 일일 회고:

	Kahneman 질문:
	"오늘 System 1이 활성화된 순간?"
	"어떤 트리거였나?"
	"어떻게 대응했나?"

	Systems Thinking 질문:
	"전체 시스템을 봤나?"
	"피드백을 활용했나?"
	"Missing을 발견했나?"

	TRIZ 질문:
	"어떤 모순이 있었나?"
	"어떻게 해결했나?"
	"더 나은 해결책은?"

	DGTF 질문:
	"신중했나?"
	"서두른 순간?"
	"다음엔 어떻게?"

### DGTF 체크리스트 (이론 기반)

**일상:**

```
Kahneman (System 1 통제):
□ System 1 트리거를 인식하는가?
□ "빨리" 충동을 느낄 때 멈추는가?
□ System 2를 활성화하는가?

Systems Thinking (전체 파악):
□ 전체 시스템을 보는가?
□ 영향을 분석하는가?
□ 피드백을 확인하는가?

TRIZ (문제 해결):
□ 모순을 식별하는가?
□ 타협하지 않고 해결하는가?

실천:
□ ROD 설계를 확인하는가?
□ TFD 테스트를 확인하는가?
□ 신중히 진행하는가?
□ 검증하며 진행하는가?

```


----------

## Part 5: 세 가지가 함께 작동하는 방법

### 완전한 개발 프로세스 (이론 통합)
```
┌──────────────────────────────────────────┐
│ Phase 1: 설계 (ROD)                      │
│ ━━━━━━━━━━━━━━━━━━━━━ │
│                                          │
│ 시간: 여유 있음                          │
│ Kahneman: System 2 활성                  │
│ Meadows: 전체 시스템 파악                │
│ TRIZ: 사전 조치                          │
│                                          │
│ 작업:                                    │
│ • 완전한 서비스 체인 (Systems Thinking)  │
│ • Missing 제거 (완전성)                  │
│ • SOLID 적용 (교체 가능)                 │
│ • 모순 해결 설계 (TRIZ)                  │
│                                          │
│ 산출물:                                  │
│ • 서비스 체인 문서                       │
│ • 인터페이스 정의                        │
│ • 의존성 그래프                          │
│                                          │
│ 효과: "무엇을 만들 것인가" 명확          │
└──────────────────────────────────────────┘
              ↓
┌──────────────────────────────────────────┐
│ Phase 2: 테스트 설계 (TFD)               │
│ ━━━━━━━━━━━━━━━━━━━━━━│
│                                          │
│ Kahneman: System 2 활성                  │
│ Meadows: 피드백 루프 설계                │
│ TRIZ: 사전 보호 (에러 예측)              │
│                                          │
│ 작업:                                    │
│ • 각 서비스마다 테스트 케이스            │
│ • 모든 시나리오 (정상/에러/엣지)         │
│ • 통합 테스트 설계                       │
│ • E2E 테스트 설계                        │
│ • 완료 기준 정의                         │
│                                          │
│ 산출물:                                  │
│ • 테스트 케이스 문서                     │
│ • 테스트 구조 코드 (TODO)                │
│ • 피드백 메커니즘                        │
│                                          │
│ 효과: "올바르게 작동하는가" 검증 기준    │
└──────────────────────────────────────────┘
              ↓
┌──────────────────────────────────────────┐
│ Phase 3: 구현 (DGTF)                     │
│ ━━━━━━━━━━━━━━━━━━━━━━│
│                                          │
│ 시간: 압박 시작                          │
│ Kahneman: System 1 우세 (위험)           │
│ 대응: DGTF로 System 2 활성화             │
│                                          │
│ 보호 장치 (세 이론):                     │
│ • ROD 서비스 체인 (가이드)               │
│ • TFD 테스트 (기준)                      │
│ • Systems Thinking (전체 파악)           │
│ • TRIZ (모순 해결)                       │
│ • DGTF 워크플로우 (통제)                 │
│                                          │
│ 실행:                                    │
│ • 한 서비스씩 신중히 구현                │
│ • 각 단계마다 테스트 (피드백)            │
│ • 지속적 검증                            │
│ • System 1 트리거 감지 시 멈춤           │
│                                          │
│ 산출물:                                  │
│ • 작동하는 코드                          │
│ • 통과하는 테스트                        │
│ • 깨끗한 구조                            │
│ • 문서                                   │
│                                          │
│ 효과: "어떻게 만들 것인가" 안전          │
└──────────────────────────────────────────┘

```

### 실전 예시: 전자상거래 장바구니

**요구사항:**

```
사용자가 상품을 장바구니에 담고
수량을 변경하고
결제할 수 있다
```

----------

**Day 1-2: ROD 설계 (Systems Thinking + TRIZ)**

- Systems Thinking 분석:
	- 전체 시스템 식별:
		- 장바구니 시스템 (주요)
		- 상품 시스템 (연결)
		- 재고 시스템 (연결)
		- 결제 시스템 (연결)
		- 주문 시스템 (연결)
	- 시스템 경계:
		- 안: 장바구니 CRUD, 계산
		- 밖: 결제 처리, 배송
	- 관계 식별:
		- 장바구니 ↔ 상품 (조회)
		- 장바구니 ↔ 재고 (확인)
		- 장바구니 → 주문 (전환)
	- 피드백 루프:
		- 재고 변경 → 장바구니 업데이트
		- 가격 변경 → 총액 재계산

- TRIZ 분석:
	- 모순:
		- "빠른 장바구니 조회" vs "정확한 가격/재고"
	- 해결:
		- 캐시 (빠름) + 주기적 갱신 (정확)
		- 조건 분리: 조회는 캐시, 결제는 실시간
	- 사전 조치:
		- 모든 검증을 미리 설계
		- 에러 케이스 예측
	- 서비스 체인 설계:
		- 장바구니 조회:
			1. ValidateUserId
			   - Systems: 입력 검증 (시스템 경계)
			2. FindCartByUserId
			   - Systems: 데이터 조회 요소
			3. EnrichCartItemsWithProductInfo
			   - Systems: 연결 (상품 시스템)
			4. CalculateCartTotal
			   - Systems: 계산 요소
		- 상품 추가:
			1. ValidateUserId
			2. ValidateProductId
			3. CheckProductAvailability
			   - Systems: 연결 (재고 시스템)
			   - TRIZ: 사전 조치
			4. FindCartByUserId
			5. CheckItemExistsInCart
			6. AddOrUpdateCartItem
			7. RecalculateCartTotal
				- Systems: 피드백 (총액 갱신)
			8. SaveCart
		- 수량 변경:
			1. ValidateUserId
			2. ValidateCartItemId
			3. ValidateQuantity
			4. FindCartByUserId
			5. FindCartItem
			6. CheckProductStock
			   - Systems: 연결 (재고)
			   - TRIZ: 사전 보호
			7. UpdateCartItemQuantity
			8. RecalculateCartTotal
			   - Systems: 피드백
			9. SaveCart

		- 아이템 제거:
			1. ValidateUserId
			11. ValidateCartItemId
			12. FindCartByUserId
			13. RemoveCartItem
			14. RecalculateCartTotal
			15. SaveCart

	- Systems Thinking 검증:
	✓ 모든 요소 식별됨
	✓ 모든 관계 명확함
	✓ 시스템 경계 정의됨
	✓ 피드백 루프 있음

	- TRIZ 검증:
	✓ 모순 해결됨
	✓ 사전 조치 적용됨
	✓ 분할 원리 적용됨

	- Kahneman 검증:
	✓ System 2로 설계됨
	✓ Missing 없음

----------

**Day 3-4: TFD 테스트 설계 (피드백 + 사전 보호)**

- Meadows 피드백 설계:
	- 각 서비스마다 피드백:
		- 입력 → 처리 → 출력
		- 출력 검증 → 다음 입력
		- 에러 → 즉시 피드백

- TRIZ 사전 보호:
	- 모든 에러 케이스 예측:
		- 존재하지 않는 사용자
		- 존재하지 않는 상품
		- 재고 부족
		- 수량 한도 초과
		- 동시 접근
		- 데이터베이스 실패

Kahneman System 2로 분석:
1. 장바구니 조회 테스트:
	- 정상:
		- 유효한 사용자 → 장바구니 반환
		- 빈 장바구니 → 빈 목록
	- 에러:
		- 존재하지 않는 사용자 → ErrUserNotFound
		- 잘못된 사용자 ID 형식 → ErrInvalidUserId
	- 엣지:
		- 아주 오래된 장바구니 → 상품 정보 갱신
		- 삭제된 상품 포함 → 필터링
2. 상품 추가 테스트:
	- 정상:
		- 새 상품 → 추가됨
		- 기존 상품 → 수량 증가
	- 에러 (TRIZ 사전 보호):
		- 재고 없음 → ErrOutOfStock
		- 유효하지 않은 상품 → ErrInvalidProduct
		- 수량 한도 초과 → ErrQuantityExceeded
	- 엣지:
		- 동시에 같은 상품 추가 → 정합성
		- 상품 가격 변경 중 → 최신 가격

3. 수량 변경 테스트:
	- 정상:
		- 유효한 수량 → 변경됨
	- 에러:
		- 재고 부족 → ErrInsufficientStock
		- 0 또는 음수 → ErrInvalidQuantity
		- 존재하지 않는 아이템 → ErrItemNotFound
	- 엣지:
		- 매우 큰 수량 → 한도 적용
		- 동시 수량 변경 → Lock

4. 아이템 제거 테스트:
	- 정상:
		- 존재하는 아이템 → 제거됨
	- 에러:
		- 존재하지 않는 아이템 → ErrItemNotFound

5. 통합 테스트 (Systems Thinking):
	- 시나리오:
		- 추가 → 수량 변경 → 제거
		- 동시 접근 (2명이 같은 장바구니)
		- 재고 변경 중 추가 시도
		- 가격 변경 중 총액 계산
	- 피드백 확인:
		- 각 작업 후 총액 정확한가
		- 재고 예약 정확한가

6. E2E 테스트:
	- 전체 흐름:
		- 장바구니 조회 (빈 상태)
		- 상품 A 추가
		- 상품 B 추가
		- 상품 A 수량 증가
		- 상품 B 제거
		- 총액 확인
		- 체크아웃

- 완료 기준:
	✓ 모든 단위 테스트 통과
	✓ 모든 통합 테스트 통과
	✓ E2E 테스트 통과
	✓ 커버리지 > 80%
	✓ 동시성 테스트 통과

----------

**Day 5-7: DGTF 구현 - 장바구니 조회**

```go
// 🛑🛑🛑 DGTF: 시작 전 체크
// Kahneman: System 2 활성화
// □ ROD 서비스 체인 확인했나?
// □ TFD 테스트 케이스 확인했나?
// □ Systems Thinking: 전체 시스템 파악했나?
// □ 불명확한 것 질문했나?

// 🛑 DGTF: 한 서비스씩, 신중하게
type CartQueryService struct {
    userValidator     UserValidator
    cartRepo          CartRepository
    productEnricher   ProductEnricher
    totalCalculator   TotalCalculator
}

func (s *CartQueryService) GetCart(
    userId string,
) (*Cart, error) {
    // Step 1: 사용자 검증
    // 🛑 Kahneman System 2: 생각하자
    // 💭 빈 문자열은?
    // 💭 잘못된 형식은?
    // 💭 Systems Thinking: 시스템 경계 검증
    if err := s.userValidator.Validate(userId); err != nil {
        return nil, err
    }
    
    // Step 2: 장바구니 조회
    // 🛑 생각: 없으면?
    // 💭 Systems Thinking: 없음 = 정상 케이스
    // 💭 빈 장바구니 반환 vs 에러?
    cart, err := s.cartRepo.FindByUserId(userId)
    if err != nil {
        if errors.Is(err, ErrCartNotFound) {
            // 빈 장바구니 반환 (정상 케이스)
            return &Cart{
                UserId: userId,
                Items:  []CartItem{},
                Total:  0,
            }, nil
        }
        return nil, err
    }
    
    // Step 3: 상품 정보 보강
    // 🛑 생각: 상품이 삭제됐으면?
    // 💭 Systems Thinking: 연결된 시스템 (상품)
    // 💭 TRIZ 사전 보호: 에러 처리
    if err := s.productEnricher.Enrich(cart); err != nil {
        // 💭 일부 상품 정보 조회 실패 → 필터링
        // 💭 전체 실패 → 에러
        return nil, err
    }
    
    // Step 4: 총액 계산
    // 🛑 생각: 가격이 변경됐으면?
    // 💭 Systems Thinking: 피드백 (최신 가격 반영)
    // 💭 할인이 있으면?
    total, err := s.totalCalculator.Calculate(cart)
    if err != nil {
        return nil, err
    }
    cart.Total = total
    
    // ✓ Meadows 피드백 확인
    // ✓ 테스트 실행
    // ✓ 통과 확인
    // ✓ 다음 서비스로
    
    return cart, nil
}

```

----------

**Day 8-10: DGTF 구현 - 상품 추가 (복잡도 높음)**

```go
// 🛑🛑🛑 DGTF: 복잡한 로직 - 더욱 신중하게!
// Kahneman: System 1 폭주 위험 높음
// Systems Thinking: 여러 시스템 연결
// TRIZ: 모순 해결 필요

type CartAddService struct {
    userValidator    UserValidator
    productValidator ProductValidator
    stockChecker     StockChecker
    cartRepo         CartRepository
    totalCalculator  TotalCalculator
}

func (s *CartAddService) AddItem(
    userId string,
    productId string,
    quantity int,
) error {
    // 🛑🛑🛑 멈춤: 복잡도 분석
    // Systems Thinking:
    // - 사용자 시스템
    // - 상품 시스템
    // - 재고 시스템
    // - 장바구니 시스템
    // 💭 트랜잭션 필요한가? → 네
    
    // 🛑🛑🛑 멈춤: TRIZ 분석
    // 모순: "빠른 추가" vs "정확한 재고"
    // 해결: 조회는 빠르게, 확정은 신중하게
    
    tx, err := s.cartRepo.BeginTransaction()
    if err != nil {
        return err
    }
    defer tx.Rollback()
    
    // Step 1: 입력 검증 (한꺼번에)
    // 🛑 DGTF: 모든 검증을 먼저
    // 💭 Kahneman: System 2로 신중히
    if err := s.userValidator.Validate(userId); err != nil {
        return err
    }
    if err := s.productValidator.Validate(productId); err != nil {
        return err
    }
    if quantity <= 0 {
        return ErrInvalidQuantity
    }
    if quantity > MaxQuantityPerItem {
        return ErrQuantityExceeded
    }
    
    // Step 2: 재고 확인
    // 🛑 생각: Systems Thinking
    // 💭 연결된 시스템: 재고
    // 💭 동시에 다른 사람이 구매하면?
    // 💭 TRIZ: 조건 분리 - 예약 시스템
    available, err := s.stockChecker.Check(productId, quantity)
    if err != nil {
        return err
    }
    if !available {
        // Meadows 피드백: 즉시 알림
        return ErrOutOfStock
    }
    
    // Step 3: 장바구니 조회
    // 🛑 생각: 트랜잭션 안에서
    cart, err := s.cartRepo.FindByUserIdWithLock(tx, userId)
    if err != nil {
        if errors.Is(err, ErrCartNotFound) {
            cart = &Cart{
                UserId: userId,
                Items:  []CartItem{},
            }
        } else {
            return err
        }
    }
    
    // Step 4: 아이템 추가/업데이트
    // 🛑🛑 생각: 복잡한 로직
    // 💭 이미 있으면? 수량 합치기
    // 💭 Systems Thinking: 상태 변화 추적
    existingItem := cart.FindItem(productId)
    if existingItem != nil {
        // 기존 아이템
        newQuantity := existingItem.Quantity + quantity
        
        // 🛑 생각: 합친 수량 검증
        // 💭 한도 초과?
        if newQuantity > MaxQuantityPerItem {
            return ErrQuantityExceeded
        }
        
        // 💭 재고 초과?
        // 💭 TRIZ 사전 보호: 다시 확인
        available, err := s.stockChecker.Check(
            productId,
            newQuantity,
        )
        if err != nil {
            return err
        }
        if !available {
            return ErrInsufficientStock
        }
        
        existingItem.Quantity = newQuantity
    } else {
        // 새 아이템
        cart.Items = append(cart.Items, CartItem{
            ProductId: productId,
            Quantity:  quantity,
        })
    }
    
    // Step 5: 총액 재계산
    // 🛑 생각: Meadows 피드백
    // 💭 최신 가격 반영
    total, err := s.totalCalculator.Calculate(cart)
    if err != nil {
        return err
    }
    cart.Total = total
    
    // Step 6: 저장
    // 🛑 생각: 실패하면? 롤백!
    if err := s.cartRepo.SaveWithTransaction(tx, cart); err != nil {
        return err
    }
    
    // 🛑 멈춤: 여기까지 성공
    // ✓ 트랜잭션 커밋
    if err := tx.Commit(); err != nil {
        return err
    }
    
    // ✓ Systems Thinking: 전체 시스템 일관성 유지됨
    // ✓ TRIZ: 모순 해결됨 (빠르고 정확)
    // ✓ Kahneman: System 2로 신중히 구현됨
    // ✓ Meadows: 피드백 루프 작동함
    
    return nil
}

// ✓ 각 단계마다 테스트
// ✓ 모든 에러 케이스 테스트
// ✓ 동시성 테스트
// ✓ 통합 테스트
// ✓ 시니어 리뷰 필수!

```

----------

**결과 비교 (이론 기반 분석)**

**ROD + TFD + DGTF 없이 (세 이론 무시):**

- 1주차: System 1 주도
	"빨리 만들자!"
	→ 설계 없음 (Systems Thinking 무시)
	→ 테스트 없음 (피드백 없음)
	→ 기술 부채 시작

- 2주차: Missing 발견
	"어? 재고 확인 누락했네?"
	→ Kahneman: System 1 폭주
	→ "전역 변수로 하자" (나쁜 결정)
	→ Systems Thinking: 시스템 불완전

- 3주차: 모순 타협
	"결제 연동..."
	→ TRIZ 무시: 나쁜 타협
	→ "여기 하드코딩!"
	→ "트랜잭션? 나중에..."

- 4주차: 프로덕션 버그
	- 동시 구매 시 재고 초과
	- 결제 실패했는데 주문 생성
	- 장바구니 안 비워짐
	→ Systems Thinking: 피드백 루프 깨짐

- 5주차: 긴급 수정, 스트레스

- 결과:
	- 5주 소요
	- 불안정
	- 기술 부채 산더미
	- 팀 사기 저하

- 이론적 분석:
	- Kahneman: System 1 지배
	- Meadows: 부분만 봄, 피드백 무시
	- TRIZ: 나쁜 타협

**ROD + TFD + DGTF로 (세 이론 적용):**

- 1주차: System 2로 설계
	- ROD (Systems Thinking):
		- 완전한 서비스 체인
		- 모든 시스템 파악
		- Missing 없음
	- TFD (Meadows 피드백):
		- 모든 테스트 케이스
		- 피드백 루프 설계

- 2주차: DGTF로 기본 구현
	- 신중히 진행
	- 지속적 검증
	- Kahneman: System 2 유지
	✓ 모든 테스트 통과

- 3주차: 복잡한 로직
	- TRIZ: 모순 해결
	- Systems Thinking: 전체 파악
	- DGTF: 더욱 신중
	✓ 모든 테스트 통과

- 4주차: 주문 로직
	- 트랜잭션 처리
	- 결제 연동
	- 롤백 로직
	✓ 모든 시나리오 테스트

- 5주차: 통합, 배포
	✓ 안정적
	✓ 고객 만족

- 결과:
	- 5주 소요 (동일)
	- 안정적
	- 기술 부채 없음
	- 팀 자신감

- 이론적 분석:
	- Kahneman: System 2 활용
	- Meadows: 전체 시스템, 피드백 활용
	- TRIZ: 모순 해결, 타협 없음

----------

### 세 방법론의 시너지 (완전한 이론 통합)

```
┌─────────────────────────────────────────┐
│ Kahneman (인간 사고)                    │
│ ━━━━━━━━━━━━━━━━━━━━━│
│                                         │
│ System 1 vs System 2                    │
│ "언제 어떤 사고를 사용할까"             │
│                                         │
│ 적용:                                   │
│ • 설계: System 2                        │
│ • 구현: DGTF로 System 2 유지            │
│ • 압박: System 1 트리거 인식            │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│ Meadows (시스템 구조)                   │
│ ━━━━━━━━━━━━━━━━━━━━━│
│                                         │
│ Systems Thinking                        │
│ "무엇을 설계해야 하는가"                │
│                                         │
│ 적용:                                   │
│ • ROD: 전체 시스템 파악                 │
│ • TFD: 피드백 루프                      │
│ • 레버리지 포인트: 설계 단계            │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│ Altshuller (문제 해결)                  │
│ ━━━━━━━━━━━━━━━━━━━━━│
│                                         │
│ TRIZ                                    │
│ "어떻게 모순을 해결하는가"              │
│                                         │
│ 적용:                                   │
│ • ROD: 사전 조치, 분할                  │
│ • TFD: 사전 보호                        │
│ • DGTF: 시간 분리 (빠름 vs 신중)        │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│ ROD + TFD + DGTF (실천)                 │
│ ━━━━━━━━━━━━━━━━━━━━━│
│                                         │
│ 통합 방법론                             │
│                                         │
│ ROD:                                    │
│ • Systems Thinking으로 전체 파악        │
│ • TRIZ로 사전 조치                      │
│ • Kahneman System 2로 설계              │
│                                         │
│ TFD:                                    │
│ • Meadows 피드백 루프                   │
│ • TRIZ 사전 보호                        │
│ • Kahneman System 2로 테스트 설계       │
│                                         │
│ DGTF:                                   │
│ • Kahneman System 1 통제                │
│ • TRIZ 시간 분리                        │
│ • Meadows 지속적 피드백                 │
│                                         │
│ 결과:                                   │
│ = 고품질 소프트웨어                     │
│ = 예측 가능한 배포                      │
│ = 지속 가능한 개발                      │
└─────────────────────────────────────────┘

```

## Part 6: 주니어 개발자를 위한 실용 가이드

### 시작하기

#### 이론 이해하기 (기초)

방법론을 효과적으로 적용하려면 세 가지 이론의 기본 개념을 이해하는 것이 좋습니다.

**Kahneman - 인간의 사고:**
- System 1: 빠르고 자동적, 직관적 
-  System 2: 느리고 의도적, 논리적 
-  압박 → System 1 우세 → 나쁜 결정 위험 
-  핵심: 언제 어떤 사고를 사용할까

**Meadows - 시스템 사고:**
- 전체를 보라 (부분만 보지 마라) 
- 관계를 이해하라 (요소 간 연결) 
- 피드백 루프 (출력 → 입력) 
- 핵심: 시스템 전체와 흐름 파악

**TRIZ - 문제 해결:**
- 모순: "A 개선 → B 악화" (빠름 vs 품질) 
- 해결: 타협하지 마라, 모순을 해결하라 
- 사전 조치: 문제 발생 전에 예방 
- 핵심: 창의적 문제 해결 패턴

---
#### ROD로 시작하기

**작게 시작 (Systems Thinking):**

- ❌ 처음부터 전체 시스템 설계
   → Meadows: "전체를 한 번에 파악할 수 없다"

- ✅ 시작:
	1. 단일 기능 선택 (예: 로그인)
	   → Systems Thinking: 경계 작게
	2. 그 기능의 서비스 체인 작성
	   → Kahneman: System 2 활성화
	   → TRIZ: 분할 원리
	3. 시니어에게 리뷰 받기
	   → Meadows: 피드백
	4. 피드백 반영
	   → 지속적 개선
	5. 다음 기능으로
	   → 점진적 확대


**템플릿 사용 (이론 통합):**

```markdown
## 기능: [기능 이름]

### 요구사항
[요구사항 간단히 기술]

### Systems Thinking 분석
- 이 기능이 속한 시스템:
- 관련된 다른 시스템:
- 시스템 경계:
- 피드백 루프:

### TRIZ 분석
- 예상 모순:
- 해결 원칙:

### 서비스 체인 (ROD)
1. [서비스명](입력)
   → 출력: [출력]
   → 에러: [가능한 에러]
   → Systems Thinking: [역할]
   → TRIZ: [적용 원리]

2. [서비스명](입력)
   → 출력: [출력]
   → 에러: [가능한 에러]
   → Systems Thinking: [역할]

[... 계속]

### Kahneman 체크
[ ] System 2로 설계했나?
[ ] System 1 폭주 방지책이 있나?

### Systems Thinking 체크
[ ] 모든 요소 식별?
[ ] 모든 관계 명확?
[ ] 시스템 경계 정의?
[ ] 피드백 루프 있나?

### TRIZ 체크
[ ] 사전 조치 적용?
[ ] 모순 해결?

### 검증
[ ] 요구사항이 서비스 체인만으로 달성 가능?
[ ] Constructor 사용 안 함?
[ ] Static 사용 안 함?
[ ] Missing 없음?

### SOLID 적용
- [서비스명]: [인터페이스 정의]

```
----------

#### TFD로 시작하기

**간단한 테스트부터 (Meadows 피드백):**

```go
// 시작: 행복 경로 + 에러 하나
// Kahneman: System 2로 신중히

func TestUserService(t *testing.T) {
    // 행복 경로 (정상 케이스)
    t.Run("ValidInput_ShouldSucceed", func(t *testing.T) {
        // 🔹 Systems Thinking: 정상 흐름
        // Arrange
        service := NewUserService(mockRepo)
        
        // Act
        result, err := service.CreateUser("valid@email.com")
        
        // Assert (Meadows: 피드백 확인)
        if err != nil {
            t.Errorf("Expected no error, got %v", err)
        }
        if result == nil {
            t.Error("Expected result, got nil")
        }
    })
    
    // 에러 하나 (TRIZ: 사전 보호)
    t.Run("InvalidEmail_ShouldReturnError", func(t *testing.T) {
        // 🔹 TRIZ: 에러 케이스 예측
        service := NewUserService(mockRepo)
        
        _, err := service.CreateUser("invalid-email")
        
        // Meadows: 피드백 (에러 감지)
        if err == nil {
            t.Error("Expected error, got nil")
        }
    })
}

// ✓ 시작은 간단하게
// ✓ 점진적으로 케이스 추가
// ✓ Kahneman: System 2로 신중히 추가

```

**피드백 루프 설정 (Meadows):**

```
테스트 작성 →  실행 (Red) →   구현 →   실행 (Green)
     ↑                                      ↓
     ←──────────────────────────────────────┘
              피드백 루프
```              
- Meadows:
	"빠른 피드백 = 빠른 학습"

- 실천:
	- 테스트 실행: 매 5분
	- 전체 테스트: 매 커밋 전
	- 통합 테스트: 매일

----------

#### DGTF로 시작하기

**자기 관찰 (Kahneman):**

```markdown
## 일일 DGTF 일지

### 날짜: 2025-01-15

### 이론 적용

#### Kahneman 관찰
서두른 순간:
시간: 10:30 AM
상황: 버그 수정 중
트리거: "빨리 고쳐야 해" (System 1)
신호: 심박수 증가, 초조함
결정: 전역 변수 사용
결과: 다른 부분 영향, 새 버그 발생

분석:
- System 1 활성화를 인식 못 함
- 멈춤 단계 생략
- System 2 활성화 실패

교훈:
- 신호 조기 감지 필요
- 멈춤 연습 필요

#### Systems Thinking 관찰
전체 시스템을 봤나: ❌
- 부분만 봄 (버그 있는 함수만)
- 다른 부분 영향 분석 안 함
- 피드백 무시

교훈:
- 수정 전 영향 분석 필수
- 관련 시스템 체크

#### TRIZ 관찰
모순: "빠른 수정" vs "정확한 수정"
해결 시도: 타협 (빠르게 대충)
결과: 실패

더 나은 해결:
- 시간 분리: 지금 1시간 신중히
- 결과: 재작업 3시간 절약

### 잘한 순간
시간: 2:00 PM
상황: 새 기능 구현 시작

Kahneman 적용:
- System 1 트리거 인식
- 멈춤: "설계 먼저"
- System 2 활성화

Systems Thinking 적용:
- ROD 설계 확인
- 전체 시스템 파악
- 영향 분석

TRIZ 적용:
- 사전 조치 (에러 예측)
- 테스트 먼저

DGTF 적용:
- 5단계 워크플로우 준수
- 신중한 구현

결과:
- 명확한 구현
- 문제 없음
- 테스트 통과

교훈:
- 세 이론 + DGTF 효과적
- 계속 실천하기

```

----------

### 흔한 주니어 실수와 해결책 (이론으로 분석)

#### 실수 1: "코딩하면서 설계할게"

- ❌ 증상: 	"일단 코드부터 짜고 보자" / "하면서 생각하면 되지"
	- 이론 분석:
		- Kahneman:
			- System 1의 즉각성
			- "생각보다 행동이 먼저"
			- 인지적 게으름
		- Systems Thinking: 
			- 전체를 안 봄
			- 부분만 봄
			- 관계 파악 안 함
		- TRIZ:
			- 사전 조치 무시
			- 반응적 문제 해결
			- 나쁜 타협 반복

	- 결과:
		→ 중간에 막힘
		→ 여러 번 다시 시작
		→ 시간 낭비
		→ 스트레스

- ✅ 해결 (이론 적용):
	- Kahneman:
		"30분 설계 (System 2) =
		 3시간 구현 (System 1 통제)"
	- Systems Thinking:
		"전체 시스템 파악 =
		 부분 최적화 X 10배 효과"
	- TRIZ:
		"사전 조치 (설계) =
		 사후 조치 (재작업) X 10배 효율"

- 행동:
	1. ROD 템플릿 사용
	2. Systems Thinking으로 전체 파악
	3. Kahneman System 2로 서비스 체인 작성
	4. TRIZ 사전 조치 (Missing 예측)
	5. 시니어에게 5분 리뷰
	6. 승인 후 구현 시작

#### 실수 2: "테스트는 나중에"

- ❌ 증상: "일단 작동하게 만들고  테스트는 나중에..."
	- 이론 분석:
		- Meadows (Systems Thinking):
			- 피드백 루프 없음
			- 출력 검증 안 함
			- 시스템 행동 불확실
		- Kahneman:
			- System 1: "빨리 완성"
			- 미래 할인 (나중에 = 안 함)
		- TRIZ:
			- 사전 보호 무시
			- 문제 발견 늦음
			- 수정 비용 10배
		- 결과:
			→ "나중에"는 안 옴
			→ 테스트하기 어려운 구조
			→ 리팩토링 무서움
			→ 버그 많음

- ✅ 해결 (이론 적용):
	- Meadows:
		"피드백 루프 = 학습 속도"
		"테스트 = 즉각적 피드백"
	- TRIZ:
		"사전 보호 = 예방"
		"지금 10분 = 나중에 1시간 절약"
	- Kahneman:
		"System 2: 장기적 이득 고려"
	- 행동:
		1. 구현 전 테스트 구조 작성
		2. 빨간불 확인 (실패 테스트)
		3. 구현
		4. 초록불 확인 (통과)
		5. Meadows 피드백: "이 부분 완료"
		6. 다음 테스트로

#### 실수 3: "빨리 끝내야 해"

- ❌ 증상: "매니저가 재촉해" / "데모가 내일이야" / "이번 스프린트 안에..."
	- 이론 분석:
		- Kahneman:
			- 외부 압박 → System 1 활성화
			- "빨리빨리" 사고
			- 장기 결과 무시
		- Systems Thinking:
			- 부분만 보기 (당장 해야 할 것)
			- 전체 시스템 영향 무시
			- 피드백 무시 (경고 신호)
		- TRIZ:
			- 나쁜 타협: 품질 희생
			- 모순 회피
			- 단기 해결책
	- System 1 폭주:
		→ 설계 건너뛰기
		→ 테스트 건너뛰기
		→ 하드코딩
	- 결과:
		→ 더 오래 걸림 (재작업)
		→ 버그 많음
		→ 신뢰 하락

- ✅ 해결 (이론 적용):
	1. Kahneman: System 1 인식
		"지금 재촉받고 있다" 인식
		→ 멈춤
		→ System 2 활성화
	2. Systems Thinking: 전체 영향 분석
		ROD로 작업 범위 파악:
		- 얼마나 많은 서비스?
		- 다른 시스템 영향?
		- 피드백 루프는?

	3. TFD로 완료 기준 명확히:
		- 테스트 범위
		- 검증 기준

	4. TRIZ: 모순 해결
		- 모순: "빠른 배포" vs "높은 품질"
		- 해결:
			- 시간 분리: Phase 1 (핵심) + Phase 2 (추가)
			- 조건 분리: 간단한 부분 빠르게, 복잡한 부분 신중히

	5. 정직한 추정:
	"분석 결과 X일 필요합니다"

	6. 매니저와 협상:
	"세 가지 선택지:
	 A) 급하게 2일 (System 1)
	    → 버그 많음
	    → 1주일 후 재작업 3일
	    → 총 5일, 불안정
	 B) 신중하게 3일 (System 2 + DGTF)
	    → 안정적
	    → 재작업 없음
	    → 총 3일, 안정
	 C) 단계적 접근 (TRIZ 시간 분리)
	    → Phase 1: 핵심 2일
	    → Phase 2: 추가 1일
	    → 총 3일, 단계적 가치
	어느 것이 비즈니스에 이로운가요?"

	- 결과:
		→ 신뢰 구축
		→ 현실적 일정
		→ 성공적 배포

### 주니어를 위한 체크리스트 (이론 통합)

#### 매일 아침:

```
이론 준비:
□ 오늘의 이론 복습 (5분)
  - Kahneman: System 1 트리거?
  - Meadows: 어떤 시스템?
  - TRIZ: 어떤 모순?

작업 준비:
□ 오늘 할 일 명확한가?
□ ROD 설계가 있나?
  → Systems Thinking: 전체 파악
□ TFD 테스트가 준비됐나?
  → Meadows: 피드백 준비
□ 막힐 때 누구에게 물어볼까?
□ 오늘의 목표는?

```

#### 코딩 시작 전:

```
Kahneman 체크:
□ System 2가 활성화됐나?
□ 서두르고 있지 않나?

Systems Thinking 체크:
□ 요구사항을 이해했나?
□ ROD 서비스 체인을 봤나?
□ 전체 시스템에서의 위치?
□ 다른 부분에 영향?

TRIZ 체크:
□ 어떤 모순이 있나?
□ 사전 조치를 했나?

TFD 체크:
□ 테스트 케이스를 확인했나?
□ 완료 기준이 명확한가?

일반:
□ 구현 방법이 명확한가?
□ 불명확한 것을 질문했나?

```

#### 코딩 중 (매 25분마다):

```
Kahneman 체크:
□ System 1 트리거 감지?
  - 심박수 증가?
  - "빨리" 생각?
  - 초조함?
□ 서두르고 있나? (위험!)
□ 필요 시 멈춤!

Systems Thinking 체크:
□ 설계를 따르고 있나?
□ 전체 시스템 고려 중?
□ 피드백 확인 중?

TRIZ 체크:
□ 모순 해결 중?
□ 타협하지 않는가?

실천:
□ 테스트를 작성하고 있나?
□ 막힌 부분이 있나?
    → 30분 이상 막혔으면 질문!

```

#### 커밋 전:

```
Meadows 피드백:
□ 모든 테스트가 통과하나?
□ 피드백 루프 작동?

Kahneman:
□ System 2로 코드 리뷰했나? (자신)
□ 서두른 흔적 없나?

Systems Thinking:
□ 전체 시스템 영향 확인?
□ 다른 부분과의 관계?

TRIZ:
□ 불필요한 코드 제거? (이상성)
□ 최적 해결책인가?

일반:
□ 주석이 필요한가?
□ 동료 리뷰 받을 준비됐나?

```

#### 하루 끝:

```
이론 회고:

Kahneman:
□ System 1 폭주 순간?
□ 어떻게 대응?
□ 개선점?

Meadows:
□ 전체 시스템을 봤나?
□ 피드백을 활용했나?
□ 시스템 사고 적용?

TRIZ:
□ 어떤 모순?
□ 어떻게 해결?
□ 더 나은 방법?

일반:
□ 오늘 목표를 달성했나?
□ 뭘 배웠나?
□ 뭘 개선할 수 있나?
□ 막힌 부분을 정리했나?
□ 내일 계획을 세웠나?

```
----------

### 성장 경로 (이론 학습 포함)

```
┌─────────────────────────────────────────┐
│ 1개월차: 기초 익히기                    │
├─────────────────────────────────────────┤
│                                         │
│ 이론 학습:                              │
│ • Kahneman 기초 (System 1 vs 2)         │
│ • Meadows 기초 (시스템 사고)            │
│ • TRIZ 기초 (모순, 사전 조치)           │
│                                         │
│ 방법론:                                 │
│ • ROD: 간단한 기능 (서비스 2-3개)       │
│ • TFD: 기본 테스트 (행복 경로)          │
│ • DGTF: 자기 관찰 시작                  │
│                                         │
│ 목표: 이론 이해, 기본 적용              │
└─────────────────────────────────────────┘
             ↓
┌─────────────────────────────────────────┐
│ 3개월차: 실력 쌓기                      │
├─────────────────────────────────────────┤
│                                         │
│ 이론 심화:                              │
│ • Kahneman: 인지 편향 이해              │
│ • Meadows: 레버리지 포인트              │
│ • TRIZ: 40가지 원리 학습                │
│                                         │
│ 방법론:                                 │
│ • ROD: 중간 기능 (서비스 5-7개)         │
│ • TFD: 완전한 테스트 (에러, 엣지)       │
│ • DGTF: System 1 트리거 인식            │
│                                         │
│ 목표: 독립적 작업, 이론 적용            │
└─────────────────────────────────────────┘
             ↓
┌─────────────────────────────────────────┐
│ 6개월차: 숙련되기                       │
├─────────────────────────────────────────┤
│                                         │
│ 이론 통합:                              │
│ • 세 이론의 상호작용 이해               │
│ • 실제 문제에 이론 적용                 │
│ • 이론으로 설명 가능                    │
│                                         │
│ 방법론:                                 │
│ • ROD: 복잡한 기능 (서비스 10개+)       │
│ • TFD: 통합/E2E 테스트                  │
│ • DGTF: System 2 자동 활성화            │
│                                         │
│ 목표: 복잡한 기능 담당                  │
└─────────────────────────────────────────┘
             ↓
┌─────────────────────────────────────────┐
│ 1년차: 전문가 되기                      │
├─────────────────────────────────────────┤
│                                         │
│ 이론 마스터:                            │
│ • 이론을 자연스럽게 적용                │
│ • 다른 사람에게 설명 가능               │
│ • 새로운 연결 발견                      │
│                                         │
│ 방법론:                                 │
│ • ROD: 시스템 레벨 설계                 │
│ • TFD: 테스트 전략 수립                 │
│ • DGTF: 다른 사람 가이드                │
│                                         │
│ 목표: 주니어 멘토링, 이론 전파          │
└─────────────────────────────────────────┘

```
----------

## Part 7: 성공 측정하기

### 개인 레벨 지표 (이론 기반)

#### ROD 지표 (Systems Thinking):

- ✅ 좋은 신호 (Meadows - 완전한 시스템):
	- 구현 중 "이게 필요한데 없네" 거의 없음
		  → Missing 제거 효과
		  → 전체 시스템 파악됨
	- 설계와 구현이 일치
		  → 레버리지 포인트 활용
	- 요구사항 변경 시 서비스 교체로 대응
		  → 적응 가능한 시스템
	- 리팩토링이 제거 위주
		  → TRIZ 이상성 증가

- ❌ 나쁜 신호:
	- 구현 중 지속적으로 Missing 발견
		  → 불완전한 시스템
		  → System 2 부족
	- "이건 어떻게 만들지?" 자주 발생
		  → 사전 조치 부족 (TRIZ)
	- 요구사항 변경 시 전체 수정 필요
		  → 경직된 시스템 구조
	- Constructor, Static 많이 사용
		  → 시스템 사고 부족


#### TFD 지표 (Meadows - 피드백):

- ✅ 좋은 신호 (효과적 피드백 루프):
	- 테스트 커버리지 > 80%
		  → 완전한 피드백
	- 테스트가 구현 전/중에 작성됨
		  → TRIZ 사전 보호
	- 테스트로 버그를 조기 발견
		  → 빠른 피드백 루프
	- 리팩토링 시 테스트로 안전 보장
		  → 피드백으로 자신감

- ❌ 나쁜 신호:
	- 테스트 커버리지 < 50%
		  → 피드백 부족
		  → Meadows: "보이지 않는 것은 관리 불가"
	- 테스트가 구현 후에 작성됨
		  → 사후 조치 (TRIZ 역행)
	- 프로덕션에서 버그 발견
		  → 피드백 루프 느림
	- "테스트 작성 시간 없어요"
		  → Kahneman System 1 지배

#### DGTF 지표 (Kahneman - System 2 활용):

- ✅ 좋은 신호 (System 2 효과적 사용):
	- 코드 리뷰에서 수정 사항 적음
		  → 신중한 구현
	- "급하게 했어요" 말 안 함
		  → System 1 통제
	- 예측 가능한 완료 시간
		  → System 2로 계획
	- 번아웃 없이 지속 가능
		  → TRIZ: 모순 해결 (빠름 vs 품질)

- ❌ 나쁜 신호:
	- 코드 리뷰에서 "부주의한 실수" 많음
		  → System 1 폭주
	- "빨리 해야 해서..." 자주 말함
		  → System 1 트리거 미인식
	- 일정 추정 항상 틀림
		  → System 1의 낙관적 편향
	- 스트레스, 번아웃
		  → 지속 불가능한 속도

----------

### 팀 레벨 지표 (통합 이론)

```
┌──────────────────────────────────────────┐
│ 매우 건강한 팀                           │
├──────────────────────────────────────────┤
│                                          │
│ Kahneman 지표:                           │
│ • System 2 문화 정착                     │
│ • 신중한 의사결정                        │
│ • 압박 속 품질 유지                      │
│                                          │
│ Meadows 지표:                            │
│ • 버그율 < 1% (프로덕션)                 │
│ • 효과적 피드백 루프                     │
│ • 시스템 사고 일상화                     │
│                                          │
│ TRIZ 지표:                               │
│ • 모순 해결 (타협 없음)                  │
│ • 사전 조치 문화                         │
│ • 지속적 혁신                            │
│                                          │
│ 방법론 지표:                             │
│ • 테스트 커버리지 > 80%                  │
│ • 코드 리뷰 사이클 < 1일                 │
│ • 기술 부채 낮음                         │
│ • 예측 가능한 배포                       │
│ • 팀 만족도 높음                         │
│                                          │
│ → ROD + TFD + DGTF 잘 적용 중            │
│ → 세 이론 이해하고 실천                  │
└──────────────────────────────────────────┘

┌──────────────────────────────────────────┐
│ 개선 필요한 팀                           │
├──────────────────────────────────────────┤
│                                          │
│ Kahneman 문제:                           │
│ • System 1 지배적                        │
│ • 충동적 의사결정                        │
│ • 압박에 품질 포기                       │
│                                          │
│ Meadows 문제:                            │
│ • 버그율 > 5%                            │
│ • 피드백 루프 느림                       │
│ • 부분만 보기 (시스템 사고 부족)         │
│                                          │
│ TRIZ 문제:                               │
│ • 나쁜 타협 반복                         │
│ • 사후 조치 (불 끄기)                    │
│ • 혁신 없음                              │
│                                          │
│ 방법론 문제:                             │
│ • 테스트 커버리지 < 50%                  │
│ • 코드 리뷰 사이클 > 3일                 │
│ • 기술 부채 높음                         │
│ • 예측 불가능한 배포                     │
│ • 팀 사기 저하                           │
│                                          │
│ → ROD + TFD + DGTF 도입 필요             │
│ → 이론 학습 필요                         │
└──────────────────────────────────────────┘

```

----------

### 장기적 효과 (이론별 영향)

- 6개월 후:
```
Kahneman 효과:
✓ System 2 활용 능력 향상
✓ System 1 트리거 조기 인식
✓ 인지 편향 인식 및 대응
✓ 스트레스 관리 개선

Meadows 효과:
✓ 시스템 사고 능력 향상
✓ 전체를 보는 습관
✓ 레버리지 포인트 활용
✓ 복잡도 관리 능력

TRIZ 효과:
✓ 모순 식별 및 해결
✓ 창의적 문제 해결
✓ 사전 조치 습관
✓ 혁신적 사고

개인 (방법론):
✓ 버그 작성율 감소 (50% ↓)
✓ 코드 리뷰 통과율 증가
✓ 일정 예측 정확도 증가
✓ 자신감 증가
✓ 스트레스 감소

팀:
✓ 전체 생산성 증가
✓ 기술 부채 감소
✓ 고객 만족도 증가
✓ 팀 협업 개선
✓ 인력 이탈 감소

비즈니스:
✓ 개발 비용 감소
✓ 유지보수 비용 감소
✓ 배포 주기 단축
✓ 품질 향상
✓ 경쟁력 강화
```

----------

## Part 8: 자주 묻는 질문 (FAQ)

### Q: ROD는 과도한 엔지니어링 아닌가요?

**A:** 아닙니다. ROD는 **정의**하는 것이지 **구현**하는 것이 아닙니다.

- 과도한 엔지니어링 (YAGNI 위반): "미래를 위해 모든 걸 만들자"
	→ 불필요한 기능 구현
	→ 복잡도 증가
	→ 비용 증가

- ROD (Systems Thinking): "필요한 모든 것을 정의하자"
	→ 정의만 함 (구현 아님)
	→ 필요한 것만 구현
	→ 불필요한 것은 제거
	→ 명확한 구조

- Meadows 관점: "시스템의 구조를 이해하면  행동을 예측할 수 있다"

- ROD 적용:
	- 정의 단계 (1시간):
		- 20개 서비스 정의
		- 전체 시스템 파악
	- 구현 단계:
		- 15개만 구현
		- 5개는 "지금은 불필요" → 제거

- TRIZ:
	→ 정보에 입각한 결정
	→ 사전 조치 (문제 예방)

----------

### Q: TFD가 코드 먼저 하는 것보다 느리지 않나요?

**A:** 단기적으로는 느려 보이지만, 장기적으로는 훨씬 빠릅니다.

```
Meadows 분석 (피드백 루프):

┌──────────────────────────────────────┐
│ 코드 우선 접근 (느린 피드백)         │
├──────────────────────────────────────┤
│ 1일차: 빠른 코딩 (3시간)             │
│ 2일차: 버그 발견 (4시간)             │
│        → 피드백 늦음                 │
│ 3일차: 더 많은 버그 (3시간)          │
│        → 피드백 더 늦음              │
│ 4일차: 리팩토링 (5시간)              │
│        → 구조 문제 발견              │
│ 5일차: 테스트 작성 (2시간)           │
│                                      │
│ Kahneman: System 1 지배              │
│ Meadows: 피드백 루프 느림            │
│ TRIZ: 사후 조치 (비효율)             │
│                                      │
│ 합계: 17시간, 불안정                 │
└──────────────────────────────────────┘

┌──────────────────────────────────────┐
│ TFD 접근 (빠른 피드백)               │
├──────────────────────────────────────┤
│ 1일차: ROD 설계 (1시간)              │
│        TFD 테스트 설계 (1시간)       │
│        → 사전 조치 (TRIZ)            │
│ 2일차: 신중한 구현 (3시간)           │
│        → System 2 (Kahneman)         │
│        → 즉각적 피드백 (Meadows)     │
│ 3일차: 통합 테스트 (2시간)           │
│        → 시스템 검증                 │
│ 4일차: E2E 테스트 (2시간)            │
│        → 전체 피드백                 │
│ 5일차: 문서화 (1시간)                │
│                                      │
│ Kahneman: System 2 활용              │
│ Meadows: 빠른 피드백 루프            │
│ TRIZ: 사전 조치 (효율적)             │
│                                      │
│ 합계: 10시간, 안정적                 │
└──────────────────────────────────────┘

TFD가 7시간 (41%) 절약!
```
- Meadows:
"빠른 피드백 = 빠른 학습 = 빠른 수정"
"느린 피드백 = 느린 학습 = 많은 재작업"

----------

### Q: DGTF는 "느리게 일하라"는 뜻인가요?

**A:** DGTF는 속도가 아니라 **신중함**입니다.

- TRIZ 모순 해결:
	- 겉보기 모순:
	"신중함" vs "빠름"

	- 전통적 타협:
		신중 → 느림
		빠름 → 부주의

- TRIZ 해결: 시간 분리

서두르는 개발자 (System 1):
```
┌────────────────────────┐
│ 행동:                  │
│ • 하루 100줄 작성      │
│ • 버그 10개            │
│ • 재작업 필요          │
│                        │
│ Kahneman: System 1     │
│ Systems: 부분만 봄     │
│ 결과: 낮은 생산성      │
└────────────────────────┘
```
DGTF 개발자 (System 2):
```
┌────────────────────────┐
│ 행동:                  │
│ • 하루 50줄 작성       │
│ • 버그 1개             │
│ • 재작업 없음          │
│                        │
│ Kahneman: System 2     │
│ Systems: 전체 봄       │
│ 결과: 높은 생산성      │
└────────────────────────┘
```
- 핵심 (Meadows):
	"작성한 코드 줄 수" ≠ 생산성
	"시스템에 통합된 가치" = 생산성

- 역설:
	느리게 보이지만 (신중함)
	실제로는 빠름 (재작업 없음)
	→ TRIZ: 모순 해결됨

----------

### Q: 매니저가 빠른 결과를 원하는데?

**A:** 비즈니스 가치로 설명하세요.

- ❌ 기술 용어 (Kahneman: 전문가의 저주): "ROD로 서비스 체인을 만들고  Systems Thinking으로 전체를 파악하고  TRIZ로 모순을 해결하고..."
	→ 매니저 이해 못 함
	→ 소통 실패

- ✅ 비즈니스 가치 (매니저 언어):   "3일 더 투자하면: 
	- Meadows 관점 (시스템 효과):
		 - 버그 70% 감소
		  → 고객 만족도 증가
		  → 이탈률 감소
		 - 유지보수 50% 감소
		  → 비용 절감
		  → 새 기능 개발 시간 증가
	- Kahneman 관점 (위험 관리):
		-  예측 가능한 일정
		  → 비즈니스 계획 가능
		  → 위험 감소
	- TRIZ 관점 (혁신):
		- 적응 가능한 구조
		  → 시장 변화 대응 빠름
		  → 경쟁 우위
	- 급하게 하면:
		- Meadows:
			-  2주 후 버그 폭발
			  → 긴급 수정 (비용 10배)
			  → 고객 신뢰 하락
			- 기술 부채
			  → 속도 점점 느려짐
			  → 레버리지 포인트 잃음
		- Kahneman:
			-  System 1 결정
			  → 나쁜 설계
			  → 장기적 손실
		- TRIZ:
			-  나쁜 타협
			  → 품질 희생
			  → 경쟁력 하락
	
	어느 것이 비즈니스에 이로운가요?"

- 결과:
	→ 데이터 기반 설득
	→ 장기적 가치 이해
	→ 신뢰 구축

----------

### Q: 레거시 코드베이스에 어떻게 적용하나요?

**A:** 점진적으로 적용하세요.

- Meadows 원칙:
	"시스템을 한 번에 바꾸려 하지 마라"
	"작은 변화의 누적이 큰 변화를 만든다"
```
┌──────────────────────────────────────┐
│ 단계적 적용 전략 (Systems Thinking)  │
├──────────────────────────────────────┤
│                                      │
│ 1단계: 새 기능만 (1개월)             │
│    Kahneman:                         │
│    • System 2로 새 코드 작성         │
│    TRIZ:                             │
│    • 분할: 새것과 레거시 분리        │
│    실천:                             │
│    • 새 기능은 ROD + TFD + DGTF      │
│    • 레거시는 그대로                 │
│    • 팀 학습                         │
│                                      │
│ 2단계: 수정 시 개선 (3개월)          │
│    Meadows:                          │
│    • 레버리지 포인트 활용            │
│    TRIZ:                             │
│    • 사전 조치: 수정 = 개선 기회     │
│    실천:                             │
│    • 버그 수정 시 테스트 추가        │
│    • 리팩토링 시 ROD 적용            │
│    • 점진적 개선                     │
│                                      │
│ 3단계: 중요 부분 (6개월)             │
│    Systems Thinking:                 │
│    • 가장 중요한 하위 시스템         │
│    TRIZ:                             │
│    • 이상성 증가 (단순화)            │
│    실천:                             │
│    • 핵심 모듈 리팩토링              │
│    • ROD로 재설계                    │
│    • 완전한 테스트 커버리지          │
│                                      │
│ 결과: 12개월 후                      │
│    Meadows:                          │
│    • 시스템 점진적 개선              │
│    • 피드백 루프 개선                │
│    Kahneman:                         │
│    • System 2 문화 정착              │
│    TRIZ:                             │
│    • 지속적 혁신                     │
│    실제:                             │
│    • 새 코드: 100% 적용              │
│    • 레거시: 50% 개선                │
│    • 점진적 개선 지속                │
└──────────────────────────────────────┘
```
- 절대 하지 말 것:

	- ❌ "전체 리라이트!"
	   Meadows: "시스템의 급격한 변화 = 예측 불가능"
	   Kahneman: "계획 오류" (System 1 낙관주의)
	   TRIZ: "리스크 너무 큼"
	   → 실패 확률 높음
	   → 비즈니스 중단

	- ✅ 점진적 개선
	   Meadows: "작은 변화의 누적"
	   Kahneman: "System 2로 신중히"
	   TRIZ: "분할 + 사전 조치"
	   → 위험 관리 가능
	   → 지속적 가치 제공

----------

### Q: 세 이론을 모두 배워야 하나요?

**A:** 방법론 적용에는 필수는 아니지만, 이해하면 훨씬 효과적입니다.

- 레벨별 접근:
```
┌─────────────────────────────────────┐
│ Level 1: 방법론만                   │
├─────────────────────────────────────┤
│ "ROD, TFD, DGTF 따라하기"           │
│                                     │
│ 장점:                               │
│ • 빠른 시작                         │
│ • 즉시 효과                         │
│                                     │
│ 단점:                               │
│ • "왜?" 모름                        │
│ • 응용 어려움                       │
│ • 확신 부족                         │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ Level 2: 방법론 + 기본 이론         │
├─────────────────────────────────────┤
│ "Kahneman System 1/2 이해"          │
│ "Systems Thinking 기초"             │
│ "TRIZ 모순 개념"                    │
│                                     │
│ 장점:                               │
│ • "왜?" 이해                        │
│ • 적용 자신감                       │
│ • 기본 응용 가능                    │
│                                     │
│ 추천: 대부분의 개발자               │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ Level 3: 깊은 이론 이해             │
├─────────────────────────────────────┤
│ "Kahneman 책 읽기"                  │
│ "Meadows 책 읽기"                   │
│ "TRIZ 40가지 원리 학습"             │
│                                     │
│ 장점:                               │
│ • 깊은 이해                         │
│ • 창의적 응용                       │
│ • 다른 사람 가르치기                │
│                                     │
│ 추천: 시니어, 리더                  │
└─────────────────────────────────────┘
```
- 학습 경로:
```
1개월차:
□ 방법론 배우기 (ROD, TFD, DGTF)
□ 기본 적용

2-3개월차:
□ Kahneman 기초 학습
□ "왜 서두르면 실수하는가" 이해
□ System 1/2 인식

4-6개월차:
□ Systems Thinking 기초
□ "전체를 보기" 연습
□ 피드백 루프 활용

7-12개월차:
□ TRIZ 기초
□ 모순 식별 및 해결
□ 창의적 문제 해결
```
- 결론:
	- 이론 없이도 방법론 적용 가능
	- 하지만 이론 이해하면:
	→ 10배 효과적
	→ 응용 가능
	→ 지속 가능

----------

## Part 9: 요약

### ROD (Responsibility-Oriented Design)

- 핵심:
"More is better than missing"

- 이론 기반:
	- Meadows (Systems Thinking):
		- 전체 시스템 파악
		- 모든 요소와 관계 정의
		- 레버리지 포인트: 설계 단계
	- Kahneman:
		- System 2로 설계
		- System 1 폭주 방지
		- 구현 중 혼란 제거
	- TRIZ:
		- 사전 조치 (Missing 예방)
		- 분할 원리 (서비스 체인)
		- 이상적 최종 결과 추구

- 실천:
	1. 완전한 서비스 체인 설계
	2. Constructor/Static 금지
	3. Missing 제거
	4. SOLID 적용

- 가치:
	- 구현 중 혼란 없음
	- 나쁜 긴급 결정 방지
	- 요구사항 변경 안전
	- 기술 부채 최소화

----------

### TFD (Test-First Development)

- 핵심:
"요구사항 = 테스트"

- 이론 기반:
	- Meadows (피드백 루프):
		- 빠른 피드백 = 빠른 학습
		- 테스트 = 시스템 검증
		- 지속적 피드백 루프
	- TRIZ:
		- 사전 보호 (버그 예방)
		- 역발상 (테스트 먼저)
		- 셀프 서비스 (자동 검증)
	- Kahneman:
		- System 2로 테스트 설계
		- 엣지 케이스 신중히 고려
		- 테스트 = System 1의 안전망

- 실천:
	1. ROD 서비스마다 테스트 설계
	2. 정상/에러/엣지 케이스
	3. 구현 전 테스트 작성
	4. Red-Green 사이클
	5. 모든 테스트 통과 = 완료

- 가치:
	- 명확한 요구사항
	- 완전한 테스트 커버리지
	- 리팩토링 안전
	- 자신감 제공
	- 살아있는 문서

----------

### DGTF (Don't Go Too Fast)

- 핵심:
"Slow is smooth, smooth is fast"

- 이론 기반:
	- Kahneman:
		- System 1 트리거 인식
		- System 2 활성화
		- 압박 속 신중함 유지
	- TRIZ (모순 해결):
		- "빠름" vs "신중" → 시간 분리
		- 설계: 느리고 정확
		- 구현: 빠르되 검증
	- Meadows:
		- 지속적 피드백 확인
		- 시스템 영향 분석
		- 레버리지 포인트 보호

- 실천:
	1. 인식: System 1 트리거 감지
	2. 멈춤: "잠깐, 생각하자"
	3. 확인: ROD, TFD, 영향 분석
	4. 계획: 신중히
	5. 실행: 검증하며 진행

- 가치:
	- 적은 버그
	- 적은 재작업
	- 높은 코드 품질
	- 지속 가능한 속도
	- 팀 신뢰 구축

----------

### 세 가지의 조합 (완전한 통합)

```
┌─────────────────────────────────────┐
│ 이론 기반 (왜?)                     │
├─────────────────────────────────────┤
│                                     │
│ Kahneman: 인간의 사고               │
│ • System 1 vs System 2              │
│ • 언제 어떤 사고를 쓸까             │
│                                     │
│ Meadows: 시스템의 구조              │
│ • 전체와 관계                       │
│ • 무엇을 설계할까                   │
│                                     │
│ Altshuller: 문제 해결               │
│ • 모순 해결                         │
│ • 어떻게 혁신할까                   │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│ 방법론 (무엇을? 어떻게?)            │
├─────────────────────────────────────┤
│                                     │
│ ROD: 무엇을 만들 것인가             │
│ • Systems Thinking으로 전체 파악    │
│ • TRIZ로 사전 조치                  │
│ • Kahneman System 2로 설계          │
│                                     │
│ TFD: 올바르게 작동하는가            │
│ • Meadows 피드백 루프               │
│ • TRIZ 사전 보호                    │
│ • Kahneman System 2로 테스트 설계   │
│                                     │
│ DGTF: 어떻게 만들 것인가            │
│ • Kahneman System 1 통제            │
│ • TRIZ 시간 분리                    │
│ • Meadows 지속적 피드백             │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│ 결과                                │
├─────────────────────────────────────┤
│                                     │
│ = 고품질 소프트웨어                 │
│ = 예측 가능한 배포                  │
│ = 지속 가능한 개발                  │
│ = 행복한 개발자                     │
│ = 만족한 고객                       │
└─────────────────────────────────────┘

```

----------

### 본질 (이론 + 방법론)

**ROD, TFD, DGTF는:**

```
❌ 원칙 (Principle)이 아니다
❌ 규칙 (Rule)이 아니다
❌ 프로세스 (Process)가 아니다

✅ 사고방식 (Way of Thinking)이다
✅ 습관 (Habit)이다
```

**운전을 생각해보라.**

운전의 기본 원칙:
- 파란불이면 간다
- 브레이크를 밟으면 멈춘다
- 엑셀레이터를 밟으면 앞으로 간다

이 원칙만 알면 좋은 운전자인가?  

**아니다.**  

```
원칙만 아는 운전자:
- 파란불 → 그냥 출발
- 앞차 브레이크 → 급정거
- 끼어들기 → 화남
→ 사고 위험, 스트레스

좋은 운전자:
- 파란불 → 좌우 확인 후 출발 (습관)
- 앞차 움직임 → 미리 예측 (사고방식)
- 끼어들기 → 여유 있게 대응 (태도)
→ 안전, 편안함
```

**소프트웨어도 마찬가지다.**

```
SOLID만 아는 개발자:
- "의존성 역전? 알죠"
- 압박 오면 → "일단 new로..."
→ 원칙은 알지만 못 지킴

SOLID가 습관인 개발자:
- 압박 와도 → 자연스럽게 인터페이스 사용
- 생각 안 해도 됨 → 몸에 배어 있음
→ 원칙이 자동으로 적용됨
```

**원칙과 습관의 차이:**

```
원칙/규칙:
- "이렇게 해야 한다" (외부 강제)
- 의식적 노력 필요
- 압박 오면 무너짐
- 지키거나 어기거나

사고방식/습관:
- "이렇게 생각한다" (내면화)
- 자연스럽게 작동
- 압박 와도 유지됨
- 의식 안 해도 됨
```

**ROD, TFD, DGTF는 그 습관을 만드는 방법이다:**

- ROD: "전체를 먼저 보는" 사고방식
  → Systems Thinking이 습관화
  
- TFD: "검증 가능하게 정의하는" 사고방식
  → 테스트 먼저가 습관화
  
- DGTF: "급하지 않게 진행하는" 습관
  → System 2 활성화가 습관화

**이 습관이 몸에 배면:**

```
Clean Coder가 말하는 "Professional"이 된다.
Professional은 결과로 신뢰를 얻는다.
신뢰가 있으면 협상이 필요 없다.
```

**Kahneman이 말했듯:**
> "System 2는 게으르다" → 연습이 필요하다

**Meadows가 말했듯:**
> "시스템 사고는 근육이다" → 훈련이 필요하다

**Altshuller가 말했듯:**
> "TRIZ는 도구다" → 사용해야 익숙해진다

**ROD + TFD + DGTF:**
> "습관이 되면 자연스럽다" → 꾸준히 실천하라

----------

## Part 10: 실행 계획

### 첫 4주 계획 (이론 + 방법론)

- 1주차: 이해하기
	- 목표: 이론과 방법론 이해
	```
	이론 학습:
	□ Kahneman System 1/2 개념
	□ Meadows Systems Thinking 개념
	□ TRIZ 모순과 사전 조치 개념

	방법론 학습:
	□ 이 가이드 완독
	□ 예시 코드 실습
	□ 팀/멘토와 논의

	실천:
	□ 질문 리스트 작성
	□ 첫 연습 기능 선택
	□ 일상에서 이론 관찰 시작

	산출물:
	• 이론 요약 노트
	• 이해도 체크리스트
	• 질문과 답변
	• 연습할 기능 선정

	```

- 2주차: ROD 연습 (Systems Thinking)
	- 목표: ROD로 설계하기
	```
	이론 적용:
	□ Systems Thinking으로 전체 파악
	  - 관련 시스템 식별
	  - 경계 정의
	  - 관계 정의
	□ TRIZ 사전 조치
	  - Missing 예측
	  - 모순 식별
	□ Kahneman System 2 활성화
	  - 신중한 설계

	방법론 실천:
	□ 선택한 기능의 서비스 체인 작성
	□ Constructor/Static 사용 안 함
	□ Missing 제거
	□ SOLID 적용
	□ 시니어에게 리뷰

	산출물:
	• 이론 분석 문서
	• 완전한 서비스 체인 문서
	• 인터페이스 정의
	• 리뷰 피드백

	```

- 3주차: TFD 연습 (Meadows 피드백)
	- 목표: 테스트 먼저 설계
	```
	이론 적용:
	□ Meadows 피드백 루프 설계
	  - 입력 → 처리 → 출력 → 검증
	□ TRIZ 사전 보호
	  - 에러 케이스 예측
	  - 모든 시나리오
	□ Kahneman System 2
	  - 엣지 케이스 신중히 고려

	방법론 실천:
	□ ROD 서비스마다 테스트 케이스 작성
	□ 테스트 구조 코드 작성 (TODO)
	□ 첫 테스트 구현
	□ 첫 서비스 구현
	□ 테스트 통과 확인

	산출물:
	• 이론 적용 노트
	• 테스트 케이스 문서
	• 테스트 코드 구조
	• 구현된 첫 서비스
	• 통과한 테스트

	```

- 4주차: DGTF 실천 (Kahneman)
	- 목표: 신중하게 구현
	```
	이론 적용:
	□ Kahneman System 1 트리거 관찰
	  - 일지 작성
	  - 신호 조기 감지
	□ TRIZ 시간 분리
	  - 포모도로 25/5
	  - 단계별 검증
	□ Meadows 피드백
	  - 지속적 테스트
	  - 영향 확인

	방법론 실천:
	□ 5단계 DGTF 워크플로우 사용
	□ 매 단계마다 검증
	□ 일일 회고 작성
	□ 전체 기능 완성

	산출물:
	• 완성된 기능 (ROD + TFD + DGTF)
	• 이론 적용 일지
	• DGTF 일지
	• 회고 문서
	• 다음 계획

	```

----------

### 이후 계속하기 (이론 심화)

- 2개월차: 통합 적용
	- 이론 심화:
		- Kahneman: 인지 편향 이해
		- Meadows: 레버리지 포인트 학습
		- TRIZ: 40가지 원리 개요
	- 방법론:
		- 더 복잡한 기능 도전
		- 팀원과 페어 프로그래밍
		- 방법론 + 이론 공유
	- 목표:
		- 이론과 방법론 자연스럽게 연결
		- 독립적 적용

- 3개월차: 팀 전파
	- 이론:
		- 팀원에게 이론 설명 가능
		- 실제 예시로 이론 설명
	- 방법론:
		- 실제 프로젝트 전면 적용
		- 팀 가이드 작성 (이론 포함)
		- 주니어 멘토링 시작
	- 목표:
		- 이론 전파
		- 팀 문화 형성

- 6개월차: 마스터
	- 이론:
		- 세 이론의 통합 이해
		- 새로운 연결 발견
		- 다른 영역 적용
	- 방법론:
		- 팀 전체 적용
		- 결과 측정 및 공유
		- 지속적 개선
	- 목표:
		- 이론 마스터
		- 방법론 완전 체화
		- 조직 문화 변화

- 1년차: 리더
	- 이론:
		- 이론 깊이 있게 이해
		- 다른 이론과 연결
		- 원서 읽기 가능
	- 방법론:
		- 완전히 체화됨
		- 자연스럽게 적용
		- 다른 팀에 전파
	- 목표:
		- 사고 리더
		- 멘토
		- 변화 촉진자

----------

### 평생 학습 경로 (이론 중심)

```
┌─────────────────────────────────────┐
│ 입문 (1-3개월)                      │
├─────────────────────────────────────┤
│ 이론:                               │
│ • 기본 개념 이해                    │
│ • 요약/블로그 읽기                  │
│                                     │
│ 방법론:                             │
│ • ROD, TFD, DGTF 기초               │
│ • 간단한 기능 적용                  │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│ 초급 (3-6개월)                      │
├─────────────────────────────────────┤
│ 이론:                               │
│ • Kahneman "Thinking, Fast and Slow"│
│   요약 읽기                         │
│ • Meadows 핵심 개념                 │
│ • TRIZ 기본 원리                    │
│                                     │
│ 방법론:                             │
│ • 중간 복잡도 기능                  │
│ • 팀 협업                           │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│ 중급 (6-12개월)                     │
├─────────────────────────────────────┤
│ 이론:                               │
│ • Kahneman 책 읽기 (원서/번역)      │
│ • Meadows "Thinking in Systems"     │
│ • TRIZ 40가지 원리 학습             │
│                                     │
│ 방법론:                             │
│ • 복잡한 시스템                     │
│ • 멘토링 시작                       │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│ 고급 (1-2년)                        │
├─────────────────────────────────────┤
│ 이론:                               │
│ • 관련 논문 읽기                    │
│ • 다른 이론과 연결                  │
│ • 자신만의 통찰 개발                │
│                                     │
│ 방법론:                             │
│ • 팀/조직 레벨 적용                 │
│ • 방법론 커스터마이징               │
│ • 컨퍼런스 발표                     │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│ 마스터 (2년+)                       │
├─────────────────────────────────────┤
│ 이론:                               │
│ • 이론 간 통합                      │
│ • 새로운 연결 발견                  │
│ • 다른 분야 적용                    │
│ • 이론 기여                         │
│                                     │
│ 방법론:                             │
│ • 자연스러운 적용                   │
│ • 조직 문화 변화                    │
│ • 업계 영향력                       │
│ • 책/논문 집필                      │
└─────────────────────────────────────┘

```

----------

## 마지막 말 (이론 + 방법론 통합)

### 핵심 메시지

**이론이 왜 중요한가:**

```
방법론만:
"이렇게 하세요" (What & How)
→ 따라하기
→ 응용 어려움
→ 확신 부족

이론 + 방법론:
"왜 이렇게 하는가" (Why) + "어떻게" (How)
→ 이해
→ 응용 가능
→ 확신
→ 창의적 적용

```

**세 이론의 힘:**

```
Kahneman (노벨상):
"인간은 어떻게 생각하는가"
→ 자신을 이해
→ 약점 보완
→ 강점 활용

Meadows (시스템 이론):
"시스템은 어떻게 작동하는가"
→ 전체를 보기
→ 레버리지 활용
→ 복잡도 관리

Altshuller (TRIZ):
"문제는 어떻게 해결하는가"
→ 모순 해결
→ 혁신 패턴
→ 창의성
```
- 세 가지 통합:
	→ 완전한 프레임워크
	→ 소프트웨어 개발에 최적
	→ 지속 가능한 성장

----------

### 시작하기 (오늘부터)

- 오늘:
	1. 이론 요약 읽기 (1시간)
	   - Kahneman System 1/2
	   - Meadows 시스템 사고
	   - TRIZ 모순
	2. 일상 관찰 시작
	   - System 1 트리거?
	   - 시스템 보이나?
	   - 모순 있나?

- 이번 주:
	1. ROD로 한 기능 설계
	   - Systems Thinking 적용
	2. TFD로 테스트 작성
	   - 피드백 루프 설계
	3. DGTF로 구현
	   - System 1 관찰

- 이번 달:
	1. 여러 기능에 적용
	2. 팀원과 공유
	3. 이론 학습 지속
	4. 점진적 확대

----------

### 마지막 메시지

> **Kahneman:** "우리는 우리가 생각하는 것만큼 합리적이지 않다. 하지만 그것을 안다면, 더 합리적이 될 수 있다."   
> **Meadows:** "시스템을 바꾸고 싶다면, 시스템을 이해해야 한다."  
> **Altshuller:** "혁신은 재능이 아니라 방법이다. 누구나 배울 수 있다."   
> **ROD + TFD + DGTF:** "이론을 실천으로. 지식을 습관으로. 개인의 성장을 팀의 문화로."   

----------

**이 방법론은 여정입니다.**

-   완벽하지 않아도 됩니다
-   실수해도 괜찮습니다
-   천천히 가도 됩니다

**중요한 것은:**

-   이론을 이해하려 노력하는 것
-   방법론을 실천하는 것
-   지속적으로 개선하는 것
-   다른 사람과 공유하는 것

**그리고 기억하세요:**

```
Kahneman이 말했듯:
"System 2는 게으르다"
→ 연습이 필요하다

Meadows가 말했듯:
"시스템 사고는 근육이다"
→ 훈련이 필요하다

Altshuller가 말했듯:
"TRIZ는 도구다"
→ 사용해야 익숙해진다

ROD + TFD + DGTF:
"습관이 되면 자연스럽다"
→ 꾸준히 실천하라

```

----------

**행운을 빌며, 즐거운 여정 되세요!** 🚀

**이론과 실천이 함께하는 개발자의 길** 🌟

----------

**문서 버전**: 2.0 
**이론 기반**:
-   Daniel Kahneman: Thinking, Fast and Slow (2011)
-   Donella H. Meadows: Thinking in Systems (2008)
-   Genrich Altshuller: TRIZ (1946-1998)

**작성자**: 수십 년의 소프트웨어 엔지니어링 경험 + 세 가지 검증된 이론  
**대상**: 중급 주니어 개발자  
**최종 업데이트**: 2025년 




