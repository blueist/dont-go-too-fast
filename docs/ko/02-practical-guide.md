# DGTF++: 실전 가이드

## 이 가이드 사용법

**전제조건:**
- "DGTF++ 핵심 개념" 먼저 읽기
- HOW를 배우기 전에 WHY를 이해하기

**이 가이드가 제공하는 것:**
- 실전 규칙과 방법
- 단계별 워크플로우
- 코드가 포함된 실제 예제
- 검증용 체크리스트
- 흔한 함정과 해결책

**구조:**
1. 빠른 참조 (이론 요약)
2. ROD 실전
3. TFD 실전
4. DGTF 실전
5. 완전한 예제
6. 주니어 개발자를 위해
7. 성공 측정
8. FAQ & 어려운 질문들
9. 실행 계획

---

## 빠른 참조: 이론 요약

상세 설명은 "핵심 개념" 문서 참조.

### Kahneman (언제 생각할 것인가)

```
System 1: 빠름, 자동적, 오류 발생 쉬움, 압박에서 지배적
System 2: 느림, 의도적, 정확함, 압박에서 꺼짐

→ System 2로 설계 (차분할 때)
→ 구현 중 System 2 보호 (DGTF)
```

### Meadows (무엇을 생각할 것인가)

```
- 부품 전에 전체 시스템을 보라
- 빠진 요소는 예측 불가능한 동작을 야기
- 피드백 루프가 자기 교정을 가능하게 함
- 설계 단계 = 가장 높은 레버리지 포인트

→ 완전한 서비스 체인 구축 (ROD)
→ 테스트로 피드백 생성 (TFD)
```

### TRIZ (모순을 어떻게 해결할 것인가)

```
"빠름" vs "품질"은 트레이드오프가 아니다.
시간 분리로 해결:
- 설계 단계: 100% 신중
- 구현 단계: 100% 빠르게 (계획을 따라서)

→ 사전 조치: 필요하기 전에 모든 것을 하라
→ 분할: 독립적인 부분으로 나누라
```

---

## Part 1: ROD 실전

### 규칙들

**규칙 1: 서비스 내부에서 생성자 금지**

```go
// ❌ 나쁨: 숨겨진 의존성
type OrderService struct{}

func (s *OrderService) CreateOrder(userId string) (*Order, error) {
    user := new(User)  // User가 어디서 오는가?
    user.ID = userId   // User가 어떤 데이터를 갖는가?
    // ... 숨겨진 복잡성
}

// ✅ 좋음: 명시적 의존성
type OrderService struct {
    userFinder UserFinder
}

func (s *OrderService) CreateOrder(userId string) (*Order, error) {
    user, err := s.userFinder.FindByID(userId)
    if err != nil {
        return nil, err
    }
    // ... 명확한 흐름
}
```

**왜 이 규칙인가?**
- `new()`는 의존성을 숨김 → Missing 발견 안 됨
- `new()`는 System 1의 탈출구 → "그냥 만들어!"
- 명시적 의존성 → 완전한 서비스 체인

---

**규칙 2: Static 필드나 메서드 금지**

```go
// ❌ 나쁨: 전역 상태
var globalConfig *Config  // 누가 설정하나? 언제?

func GetUser(id string) *User {
    db := globalDB  // 숨겨진 의존성
    // ...
}

// ✅ 좋음: 주입된 의존성
type UserFinder struct {
    db     Database
    config Config
}

func (f *UserFinder) FindByID(id string) (*User, error) {
    // 모든 의존성이 보임
}
```

**왜 이 규칙인가?**
- Static = 전역 상태 = 숨겨진 연결
- 전역 상태는 격리를 깨뜨림
- 전역 상태 변경은 모든 것에 예측 불가능하게 영향

---

**규칙 3: 구현 가정 금지**

```go
// ❌ 나쁨: 구현 가정
type PaymentService struct {
    // Stripe가 항상 사용된다고 가정
}

func (s *PaymentService) Process(amount int) {
    stripe.Charge(amount)  // PayPal로 바꾸면?
}

// ✅ 좋음: 인터페이스 기반
type PaymentGateway interface {
    Charge(amount int) error
}

type PaymentService struct {
    gateway PaymentGateway  // Stripe든, PayPal이든, 뭐든 될 수 있음
}

func (s *PaymentService) Process(amount int) error {
    return s.gateway.Charge(amount)
}
```

**왜 이 규칙인가?**
- 가정은 놀람이 됨
- 놀람은 System 1을 트리거함
- 인터페이스는 안전한 변경을 허용

---

### 서비스 체인 만드는 법

**Step 1: 요구사항으로 시작**

```
요구사항: "사용자가 이메일과 비밀번호로 로그인할 수 있다"
```

**Step 2: "무엇이 일어나야 하는가?" 묻기**

```
1. 입력 형식 검증
2. 이메일로 사용자 찾기
3. 비밀번호 확인
4. 계정 상태 확인
5. 세션 생성
6. 토큰 반환
```

**Step 3: 각 단계를 서비스로 변환**

```
ValidateCredentialsFormat(email, password) → ValidationResult
FindUserByEmail(email) → User
VerifyPassword(user, password) → bool
CheckAccountStatus(user) → AccountStatus
CreateSession(user) → Session
GenerateToken(session) → Token
```

**Step 4: 빠진 요소 추가**

각 서비스에 대해 묻기:
- "입력이 어디서 오는가?" → 불명확하면 서비스 추가
- "출력이 어디로 가는가?" → 불명확하면 서비스 추가
- "실패하면 어떻게?" → 에러 처리 서비스 추가
- "누가 알아야 하는가?" → 알림/로깅 서비스 추가

```
추가된 것:
LogLoginAttempt(email, success, timestamp) → void
HandleFailedLogin(email, attemptCount) → void
NotifySecurityTeam(suspiciousActivity) → void
```

**Step 5: 완전한 체인 그리기**

```
┌─────────────────────────────────────────┐
│ 로그인 서비스 체인                       │
├─────────────────────────────────────────┤
│                                         │
│ 입력: email, password                   │
│         ↓                               │
│ ValidateCredentialsFormat               │
│         ↓ (유효)                        │
│ FindUserByEmail                         │
│         ↓ (찾음)                        │
│ VerifyPassword                          │
│         ↓ (맞음)                        │
│ CheckAccountStatus                      │
│         ↓ (활성)                        │
│ CreateSession                           │
│         ↓                               │
│ GenerateToken                           │
│         ↓                               │
│ LogLoginAttempt ←──────────────┐        │
│         ↓                      │        │
│ 출력: Token                    │        │
│                                │        │
│ 에러 경로:                     │        │
│ InvalidFormat ──→ Return 400 ──┘        │
│ UserNotFound ──→ Return 401 ───┘        │
│ WrongPassword ─→ HandleFailedLogin ─┘   │
│ AccountLocked ─→ Return 403 ───┘        │
│                                         │
└─────────────────────────────────────────┘
```

**Step 6: 완전성 확인**

체인을 통과하는 각 경로에 대해:
- 빠진 정보 없이 완료될 수 있는가? → YES = 완전
- 어떤 서비스가 체인에 없는 것을 필요로 하는가? → 추가

---

### 예제: 결제 시스템

**요구사항:** "사용자가 장바구니의 상품을 구매할 수 있다"

**서비스 체인:**

```
┌─────────────────────────────────────────┐
│ 결제 서비스 체인                         │
├─────────────────────────────────────────┤
│                                         │
│ 1. 입력 검증                            │
│    ValidatePaymentRequest               │
│    FindUserAccount                      │
│    CheckPaymentEligibility              │
│         ↓                               │
│ 2. 계산                                 │
│    GetCartItems                         │
│    CalculateTotalAmount                 │
│    ApplyDiscounts                       │
│    CalculateTax                         │
│         ↓                               │
│ 3. 재고                                 │
│    CheckInventoryAvailability           │
│    ReserveInventory                     │
│         ↓                               │
│ 4. 결제 처리                            │
│    ValidatePaymentMethod                │
│    CreatePaymentTransaction             │
│    CallPaymentGateway                   │
│    ValidatePaymentResponse              │
│         ↓                               │
│ 5. 주문 생성                            │
│    CreateOrder                          │
│    ConfirmInventoryReservation          │
│    UpdateUserPurchaseHistory            │
│         ↓                               │
│ 6. 알림                                 │
│    SendOrderConfirmation                │
│    SendReceipt                          │
│         ↓                               │
│ 7. 로깅                                 │
│    LogPaymentEvent                      │
│                                         │
│ 에러 처리:                              │
│    ReleaseInventoryReservation          │
│    RefundPayment                        │
│    NotifyPaymentFailure                 │
│                                         │
└─────────────────────────────────────────┘
```

**핵심 설계 결정:**

1. **결제 전 예약:** 결제 실패 시 예약 해제
2. **결제 후 확정:** 결제 성공 시에만 확정
3. **로깅 분리:** 성공/실패 관계없이 항상 로깅

---

### ROD 체크리스트

```
설계 단계:
□ 모든 요구사항이 서비스 체인에 매핑됐는가?
□ 각 서비스가 단일 책임을 갖는가?
□ 모든 의존성이 명시적인가 (new/static 없이)?
□ 모든 인터페이스가 정의됐는가?
□ 에러 경로가 포함됐는가?

검증:
□ 각 요구사항이 체인만으로 완료될 수 있는가?
□ 목록에 없는 의존성이 필요한 서비스가 없는가?
□ 구현에 대한 가정이 없는가?

완료 징후:
□ 구현이 "그냥 체인을 따라가는 것"처럼 느껴짐
□ "이게 어디서 오지?" 질문 없음
□ 코딩 중 "뭔가 추가해야 해" 없음
```

---

## Part 2: TFD 실전

### 요구사항에서 테스트로

**변환:**

```
자연어                        →    테스트 케이스
─────────                          ──────────
"사용자가 로그인할 수 있다"   →    test_login_success
                                   test_login_wrong_password
                                   test_login_user_not_found
                                   test_login_account_locked
                                   test_login_too_many_attempts
```

**Step 1: 행복 경로 식별**

```
요구사항: "사용자가 로그인할 수 있다"

행복 경로:
- Given: 유효한 이메일, 올바른 비밀번호, 활성 계정
- When: 로그인 시도
- Then: 유효한 토큰 반환
```

**Step 2: 실패 케이스 식별**

묻기: "뭐가 잘못될 수 있는가?"

```
- 잘못된 이메일 형식
- 등록되지 않은 이메일
- 틀린 비밀번호
- 잠긴 계정
- 미인증 계정
- 너무 많은 실패 시도
- 시스템 오류 (DB 다운)
```

**Step 3: 엣지 케이스 식별**

묻기: "경계값은?"

```
- 빈 이메일
- 빈 비밀번호
- 매우 긴 이메일 (1000자)
- 특수문자가 있는 비밀번호
- 이메일의 유니코드
- SQL 인젝션 시도
- 동시 로그인 시도
```

**Step 4: 테스트 명세 작성**

```go
func TestLoginService(t *testing.T) {
    // 행복 경로
    t.Run("ValidCredentials_ReturnsToken", func(t *testing.T) {
        // Given: 유효한 이메일, 올바른 비밀번호, 활성 계정
        // When: Login(email, password)
        // Then: 토큰 반환, 에러 없음
    })
    
    // 실패 케이스
    t.Run("InvalidEmailFormat_ReturnsError", func(t *testing.T) {
        // Given: 잘못된 형식의 이메일
        // When: Login(email, password)
        // Then: 검증 에러 반환
    })
    
    t.Run("WrongPassword_ReturnsError", func(t *testing.T) {
        // Given: 유효한 이메일, 틀린 비밀번호
        // When: Login(email, password)
        // Then: 인증 에러 반환, 실패 횟수 증가
    })
    
    t.Run("AccountLocked_ReturnsError", func(t *testing.T) {
        // Given: 유효한 자격증명, 잠긴 계정
        // When: Login(email, password)
        // Then: 잠금 에러 반환
    })
    
    t.Run("ThreeFailedAttempts_LocksAccount", func(t *testing.T) {
        // Given: 유효한 이메일, 틀린 비밀번호
        // When: 3번 로그인 실패
        // Then: 계정 잠김
    })
    
    // 엣지 케이스
    t.Run("EmptyEmail_ReturnsError", func(t *testing.T) {
        // Given: 빈 이메일
        // When: Login("", password)
        // Then: 검증 에러 반환
    })
    
    t.Run("VeryLongEmail_HandledCorrectly", func(t *testing.T) {
        // Given: 1000자 이메일
        // When: Login(longEmail, password)
        // Then: 검증 에러 반환 (최대 길이 초과)
    })
}
```

---

### 테스트 구조: Arrange-Act-Assert

```go
func TestVerifyPassword_CorrectPassword_ReturnsTrue(t *testing.T) {
    // Arrange: 전제조건 설정
    hasher := NewBcryptHasher()
    storedHash := hasher.Hash("correctPassword123")
    service := NewPasswordVerifier(hasher)
    
    // Act: 동작 실행
    result, err := service.Verify("correctPassword123", storedHash)
    
    // Assert: 결과 확인
    if err != nil {
        t.Fatalf("에러 없어야 하는데 %v 받음", err)
    }
    if !result {
        t.Error("true여야 하는데 false 받음")
    }
}
```

---

### 체인의 각 서비스 테스트하기

**ROD 체인 → TFD 테스트**

```
서비스 체인:                      테스트:
─────────────                     ──────
ValidateCredentialsFormat    →    test_valid_format
                                  test_empty_email
                                  test_invalid_email
                                  test_empty_password
                                  test_password_too_short

FindUserByEmail              →    test_user_found
                                  test_user_not_found
                                  test_db_error

VerifyPassword               →    test_correct_password
                                  test_wrong_password
                                  test_hash_error

CheckAccountStatus           →    test_active_account
                                  test_locked_account
                                  test_unverified_account
                                  test_deleted_account

CreateSession                →    test_session_created
                                  test_session_has_expiry
                                  test_session_stored

GenerateToken                →    test_token_generated
                                  test_token_contains_claims
                                  test_token_valid_signature
```

---

### TFD 체크리스트

```
테스트 설계:
□ 행복 경로 테스트됐는가?
□ 모든 실패 케이스 식별되고 테스트됐는가?
□ 엣지 케이스 커버됐는가?
□ 에러 메시지 검증됐는가?

테스트 품질:
□ 각 테스트가 단일 목적을 갖는가?
□ 테스트가 독립적인가 (순서 의존 없음)?
□ 테스트가 Arrange-Act-Assert 구조를 사용하는가?
□ 테스트 이름이 동작을 설명하는가?

커버리지:
□ 각 ROD 서비스가 테스트를 갖는가?
□ 모든 공개 메서드가 테스트됐는가?
□ 통합 테스트가 존재하는가?
□ 커버리지 > 80%?

유지보수:
□ 테스트가 구현 상세에 의존하지 않는가?
□ 테스트가 문서 역할을 하는가?
□ 실패한 테스트가 문제를 명확히 가리키는가?
```

---

## Part 3: DGTF 실전

### 5단계 워크플로우

**Step 1: 인식**

"지금 서두르고 있나?"

자기 점검:
- 심박수 증가?
- "빨리, 빨리" 생각 반복?
- 테스트 건너뛰고 싶은가?
- "일단 되게 만들자" 압박 느끼는가?

YES라면 → System 1 영역에 있다. Step 2로.

---

**Step 2: 멈춤**

"잠깐. 멈추자."

행동:
- 키보드에서 손 떼기
- 심호흡 3번
- 5까지 세기
- 묻기: "왜 서두르게 하는 거지?"

소요 시간: 5-10초

---

**Step 3: 확인**

"시스템을 확인하자."

질문:
- ROD 설계 확인했나?
- TFD 테스트 확인했나?
- 이 변경이 다른 부분에 영향을 주나?
- 뭐가 잘못될 수 있나?

소요 시간: 1-5분

---

**Step 4: 계획**

"단계를 계획하자."

적기:
1. 정확히 뭘 바꿀 건가?
2. 어떤 테스트가 통과해야 하나?
3. 동작 여부를 어떻게 검증하나?
4. 막히면 누구에게 물어볼 건가?

소요 시간: 2-5분

---

**Step 5: 실행**

"신중하게 진행하자."

방법:
- 한 번에 작은 변경 하나
- 각 변경 후 테스트 실행
- 예상 밖의 일이 생기면 → Step 2로 돌아가기
- 자주 커밋

---

### 실제 시나리오: "이 버그 지금 당장 고쳐!"

**상황:**
```
금요일 오후 4시 30분
매니저: "프로덕션 망가졌어! 사용자들 결제 못 해!"
나: (심박수 증가, System 1 활성화)
```

**DGTF 없이:**
```
→ 코드로 뛰어들기
→ 관련된 것처럼 보이는 거 찾기
→ 바꾸기
→ 배포
→ 다른 버그 나타남
→ 더 패닉
→ 주말 망침
```

**DGTF로:**

```
Step 1: 인식
"패닉하고 있다. System 1이 지배하려 한다."

Step 2: 멈춤 (10초)
심호흡. "좋아, 실제로 뭘 알고 있지?"

Step 3: 확인 (3분)
- 에러 로그 확인: "PaymentGateway timeout"
- 모니터링 확인: 30분 전 시작
- 최근 배포 확인: 오늘 없음
- 외부 상태 확인: 결제 업체 다운

Step 4: 계획 (2분)
- 이건 외부 문제, 우리 버그 아님
- 옵션:
  a) 업체 기다리기 (30분이라고 함)
  b) 백업 업체로 전환
  c) 사용자에게 "나중에 재시도" 활성화
- 결정: 재시도 활성화, 사용자에게 알림

Step 5: 실행 (10분)
- 재시도 메커니즘 활성화 (미리 만들어둔 것, 테스트됨)
- 사용자 대상 메시지 추가
- 모니터링
- 매니저에게 타임라인과 함께 알림

총 시간: ~15분
결과: 전문적으로 처리, 패닉 변경 없음
```

---

### 커뮤니케이션 템플릿

**매니저가 "얼마나 걸려?" 물을 때**

❌ System 1 응답:
```
"오늘 끝까지 끝낼게요!"
(분석 없음, 아마 실패)
```

✅ DGTF 응답:
```
"범위 확인하고 정확한 추정 드릴게요."

[30분 후]

"요청 분석했습니다:
- 3개 서비스 건드림
- 5개 새 테스트 케이스 필요
- 추정: 2일

급하게 하면 위험:
- 사용자에게 영향 주는 버그
- 나중에 고치는 데 더 많은 시간

권장 타임라인: 적절한 테스트와 함께 2일."
```

---

**테스트 건너뛰라고 할 때**

❌ System 1 응답:
```
"네, 빨리 가려고 테스트 건너뛸게요"
(기술 부채, 미래의 고통)
```

✅ DGTF 응답:
```
"급한 거 이해합니다. 트레이드오프 설명드릴게요:

테스트로 (2일):
- 95% 신뢰도로 동작
- 주말 긴급사태 없음
- 나중에 유지보수 가능

테스트 없이 (1일):
- 50% 버그 확률
- 주말 수정 필요할 가능성 높음
- 향후 변경 위험

2일 접근법 권장합니다.
하지만 1일 내로 배포해야 한다면,
위험 문서화하고 즉시 후속 테스팅 
계획 세워야 합니다."
```

---

### 피해야 할 안티패턴

**안티패턴 1: "빠른 수정 하나만"**

```
증상: "이 한 줄만 바꿀게..."
현실: 한 줄이 열 줄이 됨, 테스트 없이
결과: 버그 배포, 신뢰 잃음

해결: "한 줄"도 5단계 적용
```

**안티패턴 2: "나중에 테스트 추가할게"**

```
증상: "먼저 동작하게 만들고, 그 다음 테스트"
현실: "나중"은 오지 않음
결과: 테스트 없는 코드가 프로덕션에

해결: 코딩 전에 테스트 명세 작성
```

**안티패턴 3: "내 컴퓨터에서는 동작해"**

```
증상: "로컬에서 테스트 통과, 배포해"
현실: 환경 차이로 실패
결과: 프로덕션 버그

해결: 일관된 환경의 CI/CD
```

**안티패턴 4: "마감이 모든 걸 정당화해"**

```
증상: "오늘 무조건 배포해야 해, 뭐가 됐든"
현실: 망가진 코드 배포는 지연보다 나쁨
결과: 긴급 수정, 신뢰 잃음, 더 많은 지연

해결: 품질이 아니라 범위를 협상
```

---

### DGTF 체크리스트

```
작업 시작 전:
□ 차분한가? (아니면 먼저 멈춤)
□ ROD 설계 있는가?
□ TFD 테스트 있는가?
□ "완료"가 뭘 의미하는지 이해했는가?

작업 중:
□ 서비스 체인을 따르고 있는가?
□ 각 변경 후 테스트 실행하는가?
□ 자주 커밋하는가?
□ 서두르는 느낌인가? (그렇다면 멈춤)

압박받을 때:
□ 응답 전 멈췄는가?
□ 정직한 추정을 줬는가?
□ 트레이드오프를 설명했는가?
□ 결정을 문서화했는가?

끝날 때:
□ 모든 테스트 통과?
□ 코드 리뷰 받음?
□ 문서 업데이트?
□ 다음 사람이 유지보수할 준비 됨?
```

---

## Part 4: 완전한 예제 - 이커머스 장바구니

### 요구사항

"사용자가 장바구니에 상품 추가, 장바구니 보기, 결제할 수 있다"

### Step 1: ROD - 서비스 체인 구축

**하위 요구사항 1: 장바구니에 추가**

```
AddToCartService 체인:
├── ValidateAddToCartRequest
├── FindUser
├── FindProduct
├── CheckProductAvailability
├── GetOrCreateCart
├── AddItemToCart
├── CalculateCartTotal
├── SaveCart
└── LogCartEvent
```

**하위 요구사항 2: 장바구니 보기**

```
ViewCartService 체인:
├── ValidateViewCartRequest
├── FindUser
├── FindCart
├── GetCartItems
├── EnrichCartItems (상품 상세 추가)
├── CalculateCartTotal
├── ApplyPromotions
└── ReturnCartView
```

**하위 요구사항 3: 결제**

```
CheckoutService 체인:
├── ValidateCheckoutRequest
├── FindUser
├── FindCart
├── ValidateCartItems (아직 가능?)
├── CalculateTotal
├── ApplyDiscounts
├── CalculateTax
├── CalculateShipping
├── ReserveInventory
├── ProcessPayment
│   ├── ValidatePaymentMethod
│   ├── ChargePayment
│   └── HandlePaymentResult
├── CreateOrder
├── ConfirmInventoryReduction
├── ClearCart
├── SendConfirmationEmail
└── LogCheckoutEvent
```

### Step 2: TFD - 테스트 설계

**AddItemToCart 테스트:**

```go
func TestAddItemToCart(t *testing.T) {
    // 행복 경로
    t.Run("ValidItem_AddsToCart", func(t *testing.T) {})
    t.Run("ItemAlreadyInCart_IncreasesQuantity", func(t *testing.T) {})
    
    // 실패 케이스
    t.Run("ProductNotFound_ReturnsError", func(t *testing.T) {})
    t.Run("ProductOutOfStock_ReturnsError", func(t *testing.T) {})
    t.Run("UserNotFound_ReturnsError", func(t *testing.T) {})
    t.Run("InvalidQuantity_ReturnsError", func(t *testing.T) {})
    
    // 엣지 케이스
    t.Run("QuantityExceedsStock_ReturnsError", func(t *testing.T) {})
    t.Run("MaxCartItems_ReturnsError", func(t *testing.T) {})
}
```

### 결과

```
소요 시간: 4일
테스트: 25개 통과
커버리지: 87%
프로덕션 버그: 0

급하게 했을 때 (추정):
소요 시간: 2일 코딩 + 3일 수정
테스트: 5개 (버그 후 추가)
커버리지: 40%
프로덕션 버그: 8개
고객 불만: 12건
망친 주말: 예
```

---

## Part 5: 주니어 개발자를 위해

### 어디서 시작할까

**1-2주차: 문제 이해**

방법론부터 시작하지 마라. 왜 좋은 실천이 존재하는지 이해부터.

연습:
1. "빠른 수정" 코드를 작성했던 때를 떠올려봐
2. 그 다음에 뭐가 일어났어?
3. 고치는 데 얼마나 시간 썼어?

이것이 DGTF++가 존재하는 이유다.

### 1개월차: DGTF부터 시작

ROD나 TFD 배우기 전에, 서두름을 인식하는 법 배워.

매일 연습:
- "빨리" 느낄 때마다 5초 멈춤
- 묻기: "왜 서두르게 하는 거지?"
- 적어두기

이것이 인식 습관을 만든다.

### 2개월차: TFD 추가

코드 전에 테스트 쓰기 시작.

매일 연습:
- 함수 작성 전에 테스트 하나 쓰기
- 딱 하나. 완벽한 커버리지 아니어도 됨.
- 습관 만들기.

### 3개월차: ROD 추가

서비스 체인에 대해 생각 시작.

매일 연습:
- 코딩 전에 서비스 스케치
- 종이에 박스와 화살표만
- 묻기: "뭐가 빠졌지?"

---

## Part 6: 성공 측정

### 개인 지표

**DGTF++ 전:**
```
- 기능당 도입 버그: 5-10개
- 디버깅 시간: 40%
- 주말 작업: 빈번
- 배포 시 자신감: 낮음
```

**DGTF++ 후 (3개월):**
```
- 기능당 도입 버그: 1-2개
- 디버깅 시간: 15%
- 주말 작업: 드묾
- 배포 시 자신감: 높음
```

### 진전의 징후

**2주차:**
- 서두를 때 알아챔
- 가끔 멈춤 (항상은 아님)

**1개월:**
- 멈춤이 습관이 됨
- 코드 전에 일부 테스트 작성
- "긴급" 수정 줄어듦

**3개월:**
- 서비스 체인이 자연스러움
- 테스트가 자동적인 워크플로우 일부
- 동료들이 코드 품질 알아챔

**6개월:**
- 다른 사람들이 어떻게 차분한지 물어봄
- 왜 이렇게 일하는지 설명할 수 있음
- 주니어 개발자들이 배우려 함

---

## Part 7: FAQ & 어려운 질문들

### Q: "선행 시간이 너무 오래 걸려"

**A:** 총 시간을 비교해보자:

```
DGTF++ 없이:
- 설계: 0.5일
- 코드: 2일
- 디버그: 3일
- 프로덕션 버그 수정: 2일
- 총: 7.5일

DGTF++로:
- ROD: 0.5일
- TFD: 0.5일
- 코드: 2일
- 디버그: 0.5일
- 프로덕션 버그: 0
- 총: 3.5일
```

선행 시간이 그 이상을 절약한다.

### Q: "매니저가 당장 결과를 원해"

**A:** 비즈니스 용어로 소통하라:

"테스트로 2일 또는 테스트 없이 1일에 배포할 수 있습니다.

테스트 없이:
- 50% 프로덕션 버그 확률
- 버그 수정에 1-2일 걸림
- 고객 영향: 부정적 리뷰
- 예상 총 시간: 3일

테스트로:
- 95% 신뢰도로 동작
- 최소 고객 영향
- 예상 총 시간: 2일

어떤 게 낫나요?"

### Q: "레거시 코드에 어떻게 적용해?"

**A:** 점진적으로.

```
1개월: 새 기능만
- 새 코드에 ROD + TFD 적용
- 레거시 건드리지 않음
- 팀 스킬 향상

2-3개월: 보이스카우트 규칙
- 레거시 수정 시 테스트 추가
- 작은 개선만
- 재작성 안 함

4-6개월: 전략적 개선
- 가장 위험한 모듈 식별
- ROD 재설계 적용
- 포괄적 테스트

절대 안 됨: 빅뱅 재작성
- 너무 위험
- 너무 비쌈
- 보통 실패
```

### 어려운 질문들 (악마의 변호인)

완벽한 방법론은 없으며, 솔직한 비판은 이해를 깊게 한다. 이 질문들은 실제 한계와 우려를 다룬다. 회의적이라면 그래야 한다—회의주의는 System 2가 제대로 작동하고 있다는 증거다.

#### Q1: "System 1이 활성화됐을 때 어떻게 System 1을 인식하나?"

**역설:** DGTF 1단계는 "서두르고 있음을 인식하라"고 한다. 하지만 System 1의 본질은 무의식적이다. 진짜 패닉 상태에서는 자신이 패닉인지 모른다. "나 지금 괜찮아"라는 생각이 가장 위험할 수 있다.

**솔직한 답:**

그 순간에는 종종 인식할 수 없다. 이것은 진짜 한계다. 그래서 DGTF가 강조하는 것:

1. **자기 인식이 아닌 외부 트리거**
   - 마감 발표 → 자동 멈춤
   - 버그 리포트 → 자동 멈춤
   - 메시지에 "급함" → 자동 멈춤
   
2. **의지력이 아닌 습관**
   - 패닉 인식에 의존하지 않음
   - 패닉 전에 발동하는 반사 구축

3. **환경 설계**
   - 테스트 강제하는 pre-commit 훅
   - 리뷰 강제하는 PR 요구사항
   - 물리적 단서 (포스트잇, 타이머)

목표는 완벽한 자기 인식이 아니다. 그것이 필요한 상황을 줄이는 것이다.

#### Q2: "ego depletion은? 의지력은 고갈된다."

**현실:** 자기 통제는 유한한 자원이다. 하루 끝, 일주일 끝, 프로젝트 끝—의지력이 바닥났을 때 DGTF를 어떻게 실천하나?

**솔직한 답:**

이것은 진짜 한계다. DGTF는 피로를 해결하지 못한다. 하지만:

1. **의지력 의존 줄이기**
   - DGTF를 자동으로 만들기 (습관)
   - 외부 강제 기능 사용
   - 고갈되면 일 멈추기—진지하게

2. **지속 가능한 속도**
   - DGTF는 "과로하지 마"를 포함
   - 주 60시간은 DGTF를 불가능하게 만듦
   - 이건 조직 실패의 증상

3. **불완전함 수용**
   - 지쳤을 때 DGTF 실패할 것
   - 그건 정보다: 너무 피곤함
   - 집에 가라

#### Q3: "환경이 독성적이면?"

**현실:** "천천히 신중하게"가 해고 사유가 될 수 있다. 일부 조직은 품질을 벌한다.

**솔직한 답:**

DGTF는 최소한의 조직적 합리성을 가정한다. 진짜 독성 환경에서:

1. **단기: 생존**
   - 안전한 곳에서 DGTF 적용
   - 품질 작업 문서화
   - 포트폴리오 구축

2. **중기: 결정**
   - 이 환경이 바꿀 수 있는가?
   - 품질을 원하는 동맹이 있나?
   - 당신의 영향력은?

3. **장기: 탈출 또는 변화**
   - 독성 환경은 스스로 고쳐지지 않음
   - 당신의 커리어는 이 직장보다 김
   - DGTF 스킬은 더 좋은 곳으로 이전됨

**DGTF가 해결할 수 없는 것:** 체계적 조직 기능 장애. 개인 방법론으로 망가진 회사를 고칠 수는 없다. 이것은 방법론의 실패가 아니라 범위의 인식이다.

일부 환경에서는 DGTF가 외부적으로 보상받지 못할 수 있다. 그러나 개인적으로는 여전히 혜택을 받는다: 덜한 스트레스, 깨끗한 코드, 더 나은 스킬, 그리고 다음 역할을 위한 강력한 포트폴리오.

#### Q4: "DGTF는 검증 불가능. 잘하고 있는지 어떻게 아나?"

**현실:** TFD는 pass/fail이 있다. ROD는 "Missing 발견"이 있다. DGTF는 내면 상태를 측정한다—측정 불가.

**솔직한 답:**

직접 측정은 불가능하다. 대리 지표 사용:

```
DGTF 대리 지표:
- 기능당 버그 (감소해야 함)
- "긴급" 수정 (감소해야 함)
- 코드 리뷰 반복 (감소해야 함)
- 개인 스트레스 수준 (감소해야 함)
- 후회스러운 커밋 (감소해야 함)
```

또한: **DGTF 실패는 눈에 보인다**. 건너뛰면 결과가 나타난다. 버그가 발생한다. 재작업이 생긴다. DGTF의 부재는 측정 가능하다.

#### Q5: "선순환은 합리적인 매니저를 가정한다"

**현실:** DGTF → 신뢰 → 자율성 → 압박 감소 → DGTF 쉬워짐... 매니저가 품질을 보상한다고 가정한다. 많은 매니저가 그렇지 않다.

**솔직한 답:**

맞다, 이 순환은 최소한 합리적인 관리를 필요로 한다. 매니저가:
- 품질을 벌하고
- 속도만 보상하고
- 결과를 무시하면

순환이 끊어진다. 옵션:
1. 다른 관리 찾기
2. 그들의 생각을 바꿀 증거 쌓기
3. DGTF가 외부적으로 보상받지 못함을 수용—여전히 당신의 성장에는 가치있음

모든 환경이 품질 관행을 지원하지는 않는다. 하지만 당신이 쌓은 스킬은 당신의 것으로 남는다.

#### Q6: "ROD는 경험이 필요하다. 주니어는 Missing을 못 찾는다."

**현실:** "완전히 설계"는 뭐가 빠졌는지 알아야 한다. 주니어는 빠진 요소를 예측할 경험이 없다.

**솔직한 답:**

맞다. ROD 혼자는 시니어 의존적이다. 하지만:

1. **주니어는 패턴을 배울 수 있다**
   - 흔한 missing 요소는 문서화됨
   - 에러 처리, 로깅, 검증, 정리
   - 이것들은 예측 가능

2. **주니어는 멘토가 필요하다**
   - 시니어와 설계 리뷰
   - "뭘 놓쳤나요?" 세션
   - 시간이 지나며 Missing 학습

3. **TFD가 주니어를 돕는다**
   - 테스트가 Missing을 더 일찍 드러냄
   - 실패 피드백이 더 빠름
   - 학습이 가속됨

ROD는 주니어에게 더 어렵다. 그래서 DGTF++에 "주니어 개발자를 위한" 섹션이 있다—어려움을 인정하고 비계를 제공.

#### Q7: "'급하게 느끼는 것'과 '진짜 급한 것'을 어떻게 구분하나?"

**현실:** 프로덕션 다운은 돈이 든다. 매 초가 중요하다. 선은 어디인가?

**솔직한 답:**

```
진짜 긴급 예시:
- 진행 중인 보안 침해 → 빨리 행동 (하지만 여전히 생각)
- 확산되는 데이터 손상 → 즉시 억제
- 초당 매출 손실 → 무자비하게 우선순위

가짜 긴급 예시:
- 매니저가 "ASAP" 말함 → 아마 생사 문제 아님
- 고객이 크게 불만 → 중요하지만 긴급 아님
- 마감이 내일 → 몇 주가 있었음; 계획 실패
```

진짜 긴급상황에서도 DGTF 적용—압축만 됨:
- 5초 멈춤이 잘못된 서버 종료를 막음
- 1분 진단이 데이터 손실을 막음
- "빠른"은 "무생각"을 의미하지 않음

가장 위험한 환경(수술, 항공)에 멈춤과 체크리스트가 더 적은 게 아니라 더 많다.

#### Q8: "DGTF는 개인적이다. 팀 압박은?"

**현실:**
- 짝 프로그래밍 파트너가 "그냥 해"
- 코드 리뷰에서 "왜 이렇게 오래 걸려?"
- 스탠드업에서 진행 보고 압박

**솔직한 답:**

팀 DGTF는 필요:

1. **팀 합의**
   - 팀으로 방법론 논의
   - 표준에 합의
   - "품질을 중시한다"를 공유 가치로

2. **명시적 소통**
   - "이것을 생각해보려고 멈추고 있어요"
   - "제대로 하고 싶어요"
   - 하는 것에 이름 붙이기

3. **압박 다루기**
   - "긴급함 이해해요. 한 가지만 확인할게요."
   - "더 빨리 갈 수 있지만 버그 있을 거예요. 결정하세요."
   - 트레이드오프 명시화

4. **문화 변화** (어려움)
   - 한 사람이 행동 모델링 가능
   - 결과가 결국 말해줌
   - 항상 가능하지 않음

#### Q9: "DGTF 실패 시 복구 전략은?"

**현실:** 패닉했다. global variable 썼다. 테스트 건너뛰었다. 그 다음은?

**솔직한 답:**

DGTF 실패 복구:

```
1. 인정 (부정하지 않기)
   - "서둘렀네"
   - 부끄러움 없이, 인식만

2. 억제
   - 문제를 키우지 않기
   - 지금 서두르기 멈춤
   - 확산 방지

3. 평가
   - 뭘 건너뛰었나?
   - 피해는?
   - 위험은?

4. 수정 또는 수용
   - 지금 고칠 수 있으면: 제대로 고침
   - 아니면: 기술 부채로 문서화
   - 교정 계획

5. 학습
   - 뭐가 서두름을 유발했나?
   - 다음 번 어떻게 방지?
   - 환경/습관 업데이트
```

**핵심 통찰:** DGTF 실패는 정보다. 환경, 상태, 또는 트리거에 대해 뭔가를 알려준다. 사용하라.

#### Q10: "이 문서 읽기가 System 1을 발동시킨다"

**메타 문제:** DGTF를 개념적으로 이해하면 배운 것처럼 느껴진다. "알았어, 다음!" 하지만 읽기는 System 1이다. 실제로 배운 게 없다.

**솔직한 답:**

완전히 맞다. 읽기 ≠ 학습. 그래서 이 가이드가 강조하는 것:

1. **읽기가 아닌 연습**
   - "실행 계획" 섹션
   - 주간 연습 목표
   - 의도적 적용

2. **시간 요구사항**
   - "1개월: 의식적 무능"
   - "3개월: 의식적 유능"
   - "6개월+: 무의식적 유능"

3. **교사로서의 실패**
   - DGTF 실패할 것
   - 실패는 읽기가 가르칠 수 없는 것을 가르침
   - 이것은 예상되고 필요함

이 가이드를 읽고 "이해했다"고 느끼는 것은 시작일 뿐이다. 진짜 이해는 실패한 시도, 교정, 수개월에 걸친 점진적 개선에서 온다. 문서는 지도이고, 영토를 걷는 것은 다르다.

---

## Part 8: 실행 계획

### 1주차: 이해

```
1-2일차:
□ 핵심 개념 문서 읽기
□ System 1/2 이해
□ 서비스 체인 이해

3-4일차:
□ 이 실전 가이드 읽기
□ 공감되는 것 메모
□ 연습할 기능 하나 선택

5일차:
□ 멘토/동료와 논의
□ 답하기: "왜 이걸 시도하고 싶은가?"
□ 4주 목표 설정
```

### 2주차: DGTF 연습

```
매일:
□ 서두를 때 알아채기 (최소 3번)
□ 멈추기 (잠깐이라도)
□ 서두르게 한 것 일지에 쓰기

주 끝:
□ 일지 검토
□ 공통 트리거 파악?
□ 멈추기 쉬워지는가?
```

### 3주차: TFD 연습

```
매일:
□ 코드 전에 최소 1개 테스트 쓰기
□ 테스트 단순하게 유지
□ 자주 테스트 실행

주 끝:
□ 작성한 테스트 수 세기
□ 이전 주와 버그 수 비교
□ "전"과 "후" 테스트 느낌?
```

### 4주차: ROD 연습

```
매일:
□ 코딩 전 서비스 체인 스케치
□ 박스와 화살표만
□ 묻기 "뭐가 빠졌지?"

주 끝:
□ 스케치 검토
□ 체인이 혼란 방지했나?
□ 구현 더 쉬웠나?
```

### 2-3개월: 통합

```
매주:
□ 세 가지 모두 하나의 기능에 적용
□ 지표 추적 (버그, 시간, 자신감)
□ 검토하고 조정

매월:
□ 1개월과 지표 비교
□ 팀에 배운 것 공유
□ 경험 기반으로 접근법 조정
```

---

## 마지막 노트

### 기억하라

```
DGTF++는:
- 완벽해지는 것이 아니다
- 느리게 하는 것이 아니다
- 규칙을 맹목적으로 따르는 것이 아니다

DGTF++는:
- 더 나은 결정을 내리는 것
- 지속 가능한 페이스
- 전문적인 결과
```

### 여정

```
1일차: "이거 많은데"
1주차: "서두르고 있는 거 알아챘어!"
1개월: "오늘 테스트 먼저 썼어"
3개월: "왜 진작 안 배웠지?"
6개월: "이게 그냥 내 방식이야"
```

### 클릭할 때

DGTF++가 동작하고 있다는 걸 알게 되는 순간:
- 압박 속에서도 차분함
- 코드가 디버깅 덜 필요함
- 동료들이 추정을 신뢰함
- 배포가 기대됨

**행운을 빈다. 천천히 가라. 그게 포인트다.**

---

**문서 버전**: 2.2  
**문서 유형**: 실전 가이드  
**전제조건**: 핵심 개념 v2.1  
**업데이트**: 2026-01
