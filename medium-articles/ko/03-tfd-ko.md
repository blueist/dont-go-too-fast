# TFD: 요구사항 = 테스트

## 테스트 우선 개발이 TDD를 실제로 작동하게 만드는 이유

---

![image](https://i.ibb.co/BdnC3hj/03.png)

*이 글은 압박 하에서 소프트웨어 품질을 유지하는 시리즈의 3부입니다. [2부: ROD—완전하게 설계하고, 빠르게 구현한다]() 읽기*

---


**TDD를 해본 적 있나요?**

대부분의 개발자가 해봤습니다. 그리고 대부분의 개발자가 포기했습니다.

```
시도 1:
"테스트를 먼저 작성하라고 했는데..."
→ "뭘 테스트해야 하지?"
→ 압도됨
→ "그냥 코드부터 짜고, 테스트는 나중에"
→ 포기

시도 2:
"Red → Green → Refactor!"
→ 첫 테스트 작성
→ "이게 맞는 건가?"
→ 구현하면서 테스트 구조가 바뀜
→ "이건 너무 어려워"
→ 포기

시도 3:
"간단한 것부터..."
→ 유틸리티 함수 테스트
→ "이건 쉽네!"
→ 복잡한 비즈니스 로직 등장
→ "이건 어떻게 테스트하지?"
→ 포기
```

익숙하다면, 문제는 당신이 아닙니다. 문제는 TDD의 숨겨진 가정입니다:

**TDD는 당신이 무엇을 테스트해야 하는지 안다고 가정합니다.**

하지만 명확한 설계가 없으면, 모릅니다. 아직 존재하지 않는 코드를, 아직 파악하지 못한 구조로 테스트를 작성하려는 것입니다.

당연히 어렵습니다.

---

## TFD: 테스트 우선 개발

TFD는 TDD를 대체하지 않습니다. **TFD는 TDD를 가능하게 합니다.**

차이점은 이렇습니다:

```
TDD:
테스트 → 구현 → 리팩토링
"테스트가 설계를 이끈다"

TFD:
설계(ROD) → 테스트 → 구현
"설계가 테스트를 정의한다"
```

ROD에서 완전한 서비스 체인이 있으면, 각 서비스가 정확히 무엇을 하는지 압니다. 입력. 출력. 오류 케이스.

테스트 작성은 *무에서 발명*이 아니라 *빈칸 채우기*가 됩니다.

---

## 핵심 원칙: 요구사항 = 테스트

모든 것을 바꾸는 통찰은 이것입니다:

> **테스트는 실행 가능하게 만든 요구사항이다.**

전통적인 요구사항을 생각해보세요:

```
전통적 요구사항:
"사용자가 결제를 할 수 있어야 한다"

즉시 질문이 생긴다:
- 카드가 실패하면?
- 잔액이 부족하면?
- 중복 결제 방지는?
- 부분 환불은?
- 타임아웃은?
```

요구사항이 모호합니다. 다른 개발자들은 다르게 해석할 것입니다. 엣지 케이스는 운영에서 발견될 것입니다.

이제 TFD 요구사항을 보세요:

```go
func TestPaymentService(t *testing.T) {
    // 정상 케이스
    t.Run("ValidPayment_ShouldSucceed", func(t *testing.T) {
        // 유효한 카드, 충분한 잔액 → 성공, 영수증 반환
    })
    
    // 카드 오류
    t.Run("InvalidCard_ShouldReturnCardError", func(t *testing.T) {
        // 유효하지 않은 카드 번호 → CardError, 청구 없음
    })
    
    t.Run("ExpiredCard_ShouldReturnExpiredError", func(t *testing.T) {
        // 만료된 카드 → ExpiredCardError
    })
    
    // 잔액 오류
    t.Run("InsufficientFunds_ShouldReturnFundsError", func(t *testing.T) {
        // 잔액 부족 → InsufficientFundsError, 청구 없음
    })
    
    // 중복 방지
    t.Run("DuplicatePayment_ShouldReturnDuplicateError", func(t *testing.T) {
        // 같은 주문에 두 번 결제 → DuplicatePaymentError
    })
    
    // 환불
    t.Run("PartialRefund_ShouldRefundPartialAmount", func(t *testing.T) {
        // $100 중 $30 환불 → $30 환불됨, $70 남음
    })
    
    // 동시성
    t.Run("ConcurrentPayments_ShouldHandleCorrectly", func(t *testing.T) {
        // 같은 주문에 동시 결제 → 하나만 성공
    })
}
```

**모든 모호함이 해결되었습니다. 모든 엣지 케이스가 문서화되었습니다. 모든 요구사항이 실행 가능합니다.**

PM이 묻습니다: "중복 결제를 처리하나요?"
답합니다: "테스트를 실행해보세요. 통과하면, 네."

---

## TFD가 ROD와 작동하는 방법

지난 글의 로그인 서비스 체인을 기억하세요:

```
1. ValidateCredentialsFormat
2. FindUserByUsername
3. VerifyPassword
4. CheckAccountStatus
5. CreateSession
6. StoreSession
7. GenerateSessionToken
8. LogLoginEvent
```

각 서비스는 테스트 스위트가 됩니다:

```go
// 서비스 2: FindUserByUsername
func TestFindUserByUsername(t *testing.T) {
    // 정상
    t.Run("ExistingUser_ShouldReturnUser", func(t *testing.T) {
        // 사용자명 존재 → 사용자 객체 반환
    })
    
    // 오류
    t.Run("NonExistingUser_ShouldReturnNotFound", func(t *testing.T) {
        // 사용자명 없음 → ErrUserNotFound
    })
    
    t.Run("EmptyUsername_ShouldReturnInvalidInput", func(t *testing.T) {
        // 빈 문자열 → ErrInvalidInput
    })
    
    // 엣지
    t.Run("DeletedUser_ShouldReturnNotFound", func(t *testing.T) {
        // 사용자가 소프트 삭제됨 → ErrUserNotFound
    })
    
    t.Run("CaseInsensitive_ShouldFindUser", func(t *testing.T) {
        // "JOHN"이 "john"을 찾아야 함 → 사용자 반환
    })
}

// 서비스 3: VerifyPassword
func TestVerifyPassword(t *testing.T) {
    // 정상
    t.Run("CorrectPassword_ShouldReturnTrue", func(t *testing.T) {
        // 올바른 비밀번호 → true, nil
    })
    
    // 오류
    t.Run("WrongPassword_ShouldReturnFalse", func(t *testing.T) {
        // 틀린 비밀번호 → false, nil (오류 아님!)
    })
    
    t.Run("EmptyPassword_ShouldReturnFalse", func(t *testing.T) {
        // 빈 비밀번호 → false, nil
    })
    
    // 엣지
    t.Run("UnicodePassword_ShouldWork", func(t *testing.T) {
        // 유니코드가 포함된 비밀번호 → 올바르게 처리
    })
}
```

**테스트를 발명하는 게 아닙니다. 서비스 체인을 실행 가능한 명세로 번역하는 것입니다.**

---

## 테스트의 세 가지 카테고리

각 서비스에 대해 세 가지 카테고리를 생각하세요:

### 1. 정상 케이스 (해피 패스)
모든 것이 제대로 되면 무슨 일이 일어나는가?

```go
t.Run("ValidInput_ShouldSucceed", ...)
```

### 2. 오류 케이스
무엇이 잘못될 수 있는가? 무슨 일이 일어나야 하는가?

```go
t.Run("InvalidInput_ShouldReturnSpecificError", ...)
t.Run("ServiceUnavailable_ShouldReturnError", ...)
t.Run("Timeout_ShouldReturnTimeoutError", ...)
```

### 3. 엣지 케이스
경계와 이상한 입력은?

```go
t.Run("EmptyString_ShouldHandle", ...)
t.Run("VeryLongInput_ShouldHandle", ...)
t.Run("SpecialCharacters_ShouldHandle", ...)
t.Run("ConcurrentCalls_ShouldHandle", ...)
```

어떤 카테고리든 놓치면 사각지대가 있는 것입니다. 시스템 1이 구현 중에 그것들을 찾아서 패닉에 빠질 것입니다.

---

## 피드백 루프

TFD가 압박 상황에서 중요한 이유입니다:

```
┌─────────────────────────────────────┐
│                                     │
│   테스트 작성                        │
│       ↓                             │
│   테스트 실행 (RED)                  │
│       ↓                             │
│   서비스 구현                        │
│       ↓                             │
│   테스트 실행 (GREEN)                │
│       ↓                             │
│   다음 테스트 ←─────────────────┘    │
│                                     │
└─────────────────────────────────────┘
```

이 루프는 **즉각적인 피드백**을 제공합니다.

- 코드가 틀렸나요? 테스트가 지금 실패합니다, 운영에서가 아니라
- 케이스를 잊었나요? 테스트 결과에서 봅니다, 고객 불만이 아니라
- 시스템 1이 실수했나요? 커밋 전에 테스트가 잡습니다

**빠른 피드백은 시스템 1의 해독제입니다.**

코드가 정확한지 몇 초 안에 알 수 있으면, 패닉이 쌓일 시간이 없습니다. 나쁜 결정이 채울 빈틈이 없습니다.

---

## TFD vs TDD: 관계

명확히 합시다: **TFD는 TDD와 경쟁하지 않습니다.**

```
TDD가 효과적이라면:
→ 좋습니다! 계속하세요.
→ TFD는 그 전에 설계 단계를 추가할 뿐

TDD가 어려웠다면:
→ 명확한 설계가 부족했기 때문
→ ROD를 먼저 시도하세요
→ 서비스 체인이 있으면, TDD가 자연스러워집니다
```

사이클은 이렇게 됩니다:

```
ROD: 서비스 체인 설계
     ↓
TFD: 각 서비스에 대한 테스트 구조 작성
     ↓
TDD: 각 테스트에 대해 Red → Green → Refactor
     ↓
완료: 완전하고, 테스트되고, 깔끔함
```

**TFD는 TDD가 이미 있다고 가정하는 명확성을 제공합니다.**

---

## 실제 예시: 검색 시스템

검색 기능에 TFD를 적용해봅시다.

**ROD 서비스 체인:**
```
1. ValidateSearchQuery
2. ParseSearchFilters
3. ExecuteSearch
4. RankResults
5. ApplyPagination
6. EnrichWithMetadata
7. FormatResponse
```

**서비스 3 (ExecuteSearch)에 대한 TFD 테스트:**

```go
func TestExecuteSearch(t *testing.T) {
    // 정상
    t.Run("ValidQuery_ShouldReturnResults", func(t *testing.T) {
        // "laptop" 검색 → 노트북 제품 반환
    })
    
    t.Run("QueryWithFilters_ShouldApplyFilters", func(t *testing.T) {
        // "laptop" + 가격:100-500 검색 → 필터링된 결과
    })
    
    // 오류
    t.Run("EmptyQuery_ShouldReturnError", func(t *testing.T) {
        // 빈 문자열 → ErrEmptyQuery
    })
    
    t.Run("DatabaseUnavailable_ShouldReturnError", func(t *testing.T) {
        // DB 다운 → ErrServiceUnavailable
    })
    
    // 엣지
    t.Run("NoResults_ShouldReturnEmptyList", func(t *testing.T) {
        // "xyzabc123" 검색 → 빈 목록 (오류 아님!)
    })
    
    t.Run("SpecialCharacters_ShouldEscape", func(t *testing.T) {
        // "laptop; DROP TABLE" 검색 → 이스케이프됨, 안전함
    })
    
    t.Run("VeryLongQuery_ShouldTruncate", func(t *testing.T) {
        // 10000자 쿼리 → 제한으로 잘림
    })
    
    t.Run("UnicodeQuery_ShouldWork", func(t *testing.T) {
        // "노트북" 검색 → 한국어 결과 반환
    })
}
```

**모든 테스트가 요구사항입니다. 모든 요구사항이 테스트 가능합니다. 모호함이 없습니다.**

---

## 단일 진실 공급원

TFD를 채택하면, 아름다운 일이 일어납니다:

```
전통적 프로젝트:
├── 요구사항 문서 (구버전)
├── 설계 문서 (업데이트됐을 수도)
├── 코드 (실제 동작)
├── 주석 (거짓말)
└── 테스트 (있다면)

→ 5개의 "진실" 출처, 모두 다름

TFD 프로젝트:
├── 테스트 = 요구사항 = 설계 = 문서
└── 코드 (테스트를 구현)

→ 1개의 진실 출처, 항상 최신
```

테스트가 문서가 됩니다. 누군가 "이 서비스가 뭘 하나요?"라고 물으면, 테스트를 가리킵니다. 테스트가 통과하면, 코드가 정확합니다. 테스트가 틀리면, 테스트를 고칩니다.

**테스트가 계약입니다.** 그 외 모든 것은 해설입니다.

---

## TFD 체크리스트

서비스를 구현하기 전에:

**커버리지:**
- [ ] 해피 패스 테스트가 존재하는가?
- [ ] 모든 오류 케이스가 커버되었는가?
- [ ] 엣지 케이스가 식별되었는가?
- [ ] 통합 지점이 테스트되었는가?

**명확성:**
- [ ] 테스트 이름이 요구사항을 설명하는가?
- [ ] 예상 동작이 명확한가?
- [ ] 오류 메시지가 구체적인가?

**독립성:**
- [ ] 테스트가 서로 의존하지 않는가?
- [ ] 테스트가 외부 상태에 의존하지 않는가?
- [ ] 테스트가 어떤 순서로든 실행 가능한가?

**피드백:**
- [ ] 테스트가 빠르게 실행되는가 (각 < 1초)?
- [ ] 실패가 문제를 가리키는가?
- [ ] 커버리지가 측정 가능한가 (> 80%)?

---

## 다음: DGTF

ROD는 무엇을 만들지 알려줍니다.
TFD는 그것을 어떻게 검증할지 알려줍니다.

하지만 어쨌든 마감이 닥치면? 매니저가 어깨 너머로 지켜보고 있으면? 시스템 1이 "빨리!"라고 소리치면?

**다음 글**에서는 DGTF(너무 빨리 가지 마라)를 소개합니다—압박 하에서도 시스템 2 사고를 유지하는 방법론.

배울 내용:
- 시스템 1 트리거를 인식하는 방법
- 침착함을 유지하는 5단계 워크플로우
- 자기 통제가 전문가를 만드는 이유
- 오늘 사용할 수 있는 실용적 기법

---

*TFD는 더 많은 테스트를 작성하는 것이 아닙니다. "완료"가 무엇을 의미하는지 정확히 아는 것입니다—압박이 뭔가 고장난 것을 배포하게 만들기 전에.*

**다음: DGTF—느린 것이 매끄럽고, 매끄러운 것이 빠르다 →**

---

*지금까지 작성한 테스트 중 가장 가치 있었던 것은 무엇인가요? 재앙에서 당신을 구한 그 테스트? 댓글에서 공유해주세요.*
