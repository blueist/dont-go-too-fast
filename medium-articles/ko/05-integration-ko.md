# 모든 것을 합치기

## 완전한 예시: 요구사항부터 작동하는 코드까지

---

![image](https://i.ibb.co/N2qbLv3w/05.png)

*이 글은 압박 하에서 소프트웨어 품질을 유지하는 시리즈의 마지막 5부입니다. [4부: DGTF—느린 것이 매끄럽고, 매끄러운 것이 빠르다]() 읽기*

---

이론을 다뤘습니다:
- **ROD**: 완전한 서비스 체인 설계
- **TFD**: 테스트를 요구사항으로 작성
- **DGTF**: 압박 하에서 침착함 유지

이제 함께 작동하는 것을 봅시다.

이커머스 장바구니를—초기 요구사항부터 작동하는 코드까지—세 가지 방법론을 모두 사용해서 만들겠습니다.

---

## 요구사항

```
사용자가 할 수 있는 것:
- 장바구니에 상품 추가
- 수량 변경
- 항목 삭제
- 장바구니 총액 보기
- 결제
```

간단하죠?

대부분의 개발자는 여기서 IDE를 열고 코딩을 시작할 것입니다.

**우리는 그러지 않습니다.**

---

## 1-2일차: ROD 설계

### 시스템 사고 분석

먼저 전체 시스템을 봅니다.

**관련 시스템:**
- 장바구니 시스템 (우리의 초점)
- 상품 시스템 (상품 정보)
- 재고 시스템 (재고 수준)
- 사용자 시스템 (인증)
- 주문 시스템 (결제 후)
- 결제 시스템 (돈 처리)

**시스템 경계:**
- 범위 내: 장바구니 CRUD, 계산
- 범위 외: 결제 처리, 배송

**주요 관계:**
```
장바구니 ←→ 상품 (정보 조회)
장바구니 ←→ 재고 (재고 확인)
장바구니 → 주문 (결제 시 변환)
```

**피드백 루프:**
- 재고 변경 → 장바구니 업데이트 필요
- 가격 변경 → 총액 재계산

### 모순 분석 (TRIZ)

**모순:**
"빠른 장바구니 조회" vs "정확한 가격/재고"

**해결 (조건에 의한 분리):**
- 브라우징: 캐시된 데이터 사용 (빠름)
- 결제: 실시간 데이터 사용 (정확)

### 완전한 서비스 체인

이제 필요한 모든 서비스를 설계합니다:

```
장바구니 시스템 서비스 체인
═════════════════════════════════

┌─ 장바구니 보기 ─────────────────────────┐
│ 1. ValidateUserId                       │
│ 2. FindCartByUserId                     │
│ 3. EnrichCartItemsWithProductInfo       │
│ 4. CalculateCartTotal                   │
└─────────────────────────────────────────┘

┌─ 항목 추가 ─────────────────────────────┐
│ 1. ValidateUserId                       │
│ 2. ValidateProductId                    │
│ 3. ValidateQuantity                     │
│ 4. CheckProductAvailability             │
│ 5. FindCartByUserId                     │
│ 6. CheckItemExistsInCart                │
│ 7. AddOrUpdateCartItem                  │
│ 8. RecalculateCartTotal                 │
│ 9. SaveCart                             │
└─────────────────────────────────────────┘

┌─ 수량 변경 ─────────────────────────────┐
│ 1. ValidateUserId                       │
│ 2. ValidateCartItemId                   │
│ 3. ValidateQuantity                     │
│ 4. FindCartByUserId                     │
│ 5. FindCartItem                         │
│ 6. CheckProductStock                    │
│ 7. UpdateCartItemQuantity               │
│ 8. RecalculateCartTotal                 │
│ 9. SaveCart                             │
└─────────────────────────────────────────┘

┌─ 항목 삭제 ─────────────────────────────┐
│ 1. ValidateUserId                       │
│ 2. ValidateCartItemId                   │
│ 3. FindCartByUserId                     │
│ 4. RemoveCartItem                       │
│ 5. RecalculateCartTotal                 │
│ 6. SaveCart                             │
└─────────────────────────────────────────┘
```

**주목하세요:**
- 총 22개 서비스 (4개의 "당연한" 것이 아님)
- 모든 단계가 명시적
- 모든 객체에 출처가 있음
- 시스템 1이 채울 빈틈이 없음

### ROD 검증

```
✓ 모든 요소 식별됨
✓ 모든 관계 명확
✓ 시스템 경계 정의됨
✓ 빠진 것 없음
✓ 생성자 불필요
✓ SOLID 적용 가능
```

---

## 3-4일차: TFD 테스트 설계

이제 각 서비스에 대한 테스트를 작성합니다. 구현이 아니라—테스트만.

### 장바구니 보기 테스트

```go
func TestViewCart(t *testing.T) {
    // ValidateUserId
    t.Run("EmptyUserId_ShouldReturnError", ...)
    t.Run("InvalidUserIdFormat_ShouldReturnError", ...)
    t.Run("ValidUserId_ShouldPass", ...)
    
    // FindCartByUserId
    t.Run("UserHasCart_ShouldReturnCart", ...)
    t.Run("UserHasNoCart_ShouldReturnEmptyCart", ...)
    t.Run("UserNotFound_ShouldReturnError", ...)
    
    // EnrichCartItemsWithProductInfo
    t.Run("AllProductsExist_ShouldEnrich", ...)
    t.Run("ProductDeleted_ShouldFilterOut", ...)
    t.Run("ProductPriceChanged_ShouldUseNewPrice", ...)
    
    // CalculateCartTotal
    t.Run("EmptyCart_ShouldReturnZero", ...)
    t.Run("SingleItem_ShouldCalculateCorrectly", ...)
    t.Run("MultipleItems_ShouldSumCorrectly", ...)
    t.Run("WithDiscounts_ShouldApply", ...)
}
```

### 항목 추가 테스트

```go
func TestAddItem(t *testing.T) {
    // 검증
    t.Run("InvalidProductId_ShouldReturnError", ...)
    t.Run("ZeroQuantity_ShouldReturnError", ...)
    t.Run("NegativeQuantity_ShouldReturnError", ...)
    
    // 가용성
    t.Run("ProductOutOfStock_ShouldReturnError", ...)
    t.Run("QuantityExceedsStock_ShouldReturnError", ...)
    t.Run("ProductAvailable_ShouldProceed", ...)
    
    // 장바구니 작업
    t.Run("NewItem_ShouldAddToCart", ...)
    t.Run("ExistingItem_ShouldIncreaseQuantity", ...)
    t.Run("QuantityExceedsLimit_ShouldReturnError", ...)
    
    // 동시성
    t.Run("SimultaneousAdds_ShouldHandleCorrectly", ...)
    
    // 통합
    t.Run("AddItem_TotalShouldUpdate", ...)
}
```

### 수량 변경 테스트

```go
func TestUpdateQuantity(t *testing.T) {
    // 검증
    t.Run("ItemNotInCart_ShouldReturnError", ...)
    t.Run("InvalidQuantity_ShouldReturnError", ...)
    
    // 재고 확인
    t.Run("InsufficientStock_ShouldReturnError", ...)
    t.Run("StockAvailable_ShouldUpdate", ...)
    
    // 엣지 케이스
    t.Run("SetToZero_ShouldRemoveItem", ...)
    t.Run("VeryLargeQuantity_ShouldEnforceLimit", ...)
    
    // 통합
    t.Run("UpdateQuantity_TotalShouldRecalculate", ...)
}
```

### E2E 테스트

```go
func TestCartE2E(t *testing.T) {
    t.Run("CompleteCartJourney", func(t *testing.T) {
        // 1. 빈 장바구니 보기
        // 2. 상품 A 추가 (수량: 2)
        // 3. 상품 B 추가 (수량: 1)
        // 4. 총액 확인
        // 5. 상품 A 수량 변경 (수량: 3)
        // 6. 총액 업데이트 확인
        // 7. 상품 B 삭제
        // 8. 총액 업데이트 확인
        // 9. 최종 장바구니 보기
        // 모든 검증 통과 = 기능 완료
    })
}
```

### TFD 검증

```
✓ 모든 서비스에 테스트 있음
✓ 정상 케이스 커버됨
✓ 오류 케이스 커버됨
✓ 엣지 케이스 커버됨
✓ 통합 테스트 존재
✓ E2E 테스트 존재
✓ 완료 기준 명확
```

---

## 5-7일차: DGTF 구현

이제 구현합니다. ROD와 TFD가 완료되면, 이것이 쉬운 부분입니다.

### 시작 전 (DGTF 점검)

```
□ ROD 서비스 체인 열림? ✓
□ TFD 테스트 파일 열림? ✓
□ 첫 서비스 식별됨? ✓ (ValidateUserId)
□ 서두르는 느낌? 아니오
□ 시스템 2 활성? 예

시작 준비 완료.
```

### ValidateUserId 구현

```go
// DGTF: 한 번에 하나의 서비스
// 이 서비스: 사용자 ID 형식 검증

func (s *CartService) ValidateUserId(userId string) error {
    // 확인: ROD가 뭐라고 했지?
    // → 입력 형식 검증
    // → 반환: error 또는 nil
    
    // 확인: TFD가 뭐라고 했지?
    // → EmptyUserId → ErrInvalidUserId
    // → InvalidFormat → ErrInvalidUserId  
    // → ValidUserId → nil
    
    if userId == "" {
        return ErrInvalidUserId
    }
    
    if !isValidUUIDFormat(userId) {
        return ErrInvalidUserId
    }
    
    return nil
}

// 테스트 실행: go test -run TestValidateUserId
// 결과: PASS
// 다음 서비스: FindCartByUserId
```

### 구현 중 DGTF 점검

수요일 오후입니다. 매니저가 묻습니다: "장바구니 기능 어떻게 되가요?"

**DGTF 반응:**

```
1. 인식: 약간의 압박감
2. 멈춤: "진행 상황 확인해보자"
3. 확인:
   - 완료된 서비스: 8/22
   - 통과하는 테스트: 24/24
   - 금요일에 맞출 수 있음
4. 계획: 현재 서비스 계속
5. 실행: 코딩 재개

매니저에게 답: "8개 서비스 완료, 모든 테스트 통과. 
금요일 배포 가능합니다."
```

패닉 없음. 서두름 없음. 그냥 사실만.

### 복잡한 서비스 구현 (AddOrUpdateCartItem)

```go
// DGTF: 복잡한 로직 - 더 조심!
// □ 이 서비스에 대한 ROD 확인
// □ 모든 케이스에 대한 TFD 확인
// □ 단계별로 구현

func (s *CartService) AddOrUpdateCartItem(
    cart *Cart, 
    productId string, 
    quantity int,
) error {
    // DGTF: 먼저 생각
    // 케이스가 뭐지?
    // 1. 항목 없음 → 새로 추가
    // 2. 항목 있음 → 수량 업데이트
    // 3. 총 수량 초과 → 오류
    
    existingItem := cart.FindItem(productId)
    
    if existingItem == nil {
        // 케이스 1: 새 항목
        cart.Items = append(cart.Items, CartItem{
            ProductId: productId,
            Quantity:  quantity,
            AddedAt:   time.Now(),
        })
        return nil
    }
    
    // 케이스 2: 기존 항목
    newQuantity := existingItem.Quantity + quantity
    
    // 케이스 3: 한계 확인
    if newQuantity > MaxQuantityPerItem {
        return ErrQuantityExceeded
    }
    
    existingItem.Quantity = newQuantity
    return nil
}

// 테스트 실행: go test -run TestAddOrUpdateCartItem
// 결과: PASS (7/7 케이스)
// DGTF 점검: 기분 좋음, 서두르지 않음
// 다음 서비스: RecalculateCartTotal
```

---

## 8일차: 통합 & 최종 테스팅

### 모든 단위 테스트

```bash
$ go test ./cart/...
ok  	cart/validation    0.012s (8 tests)
ok  	cart/repository    0.034s (12 tests)
ok  	cart/service       0.021s (28 tests)
ok  	cart/calculation   0.008s (6 tests)

합계: 54개 테스트, 0개 실패
```

### 통합 테스트

```bash
$ go test -tags=integration ./cart/...
ok  	cart/integration   0.234s (8 tests)

합계: 8개 테스트, 0개 실패
```

### E2E 테스트

```bash
$ go test -tags=e2e ./cart/...
ok  	cart/e2e   1.021s (1 test)

PASS
```

### 최종 DGTF 점검

```
□ 모든 테스트 통과? ✓
□ 코드 리뷰 (셀프)? ✓
□ 코너 잘랐나? 아니오
□ 배포 확신? 예
□ 문서 업데이트? ✓

배포 준비 완료.
```

---

## 결과

```
타임라인:
├── 1-2일차: ROD 설계 (완전한 서비스 체인)
├── 3-4일차: TFD 테스트 (54개 테스트 케이스)
├── 5-7일차: 구현 (22개 서비스)
└── 8일차: 통합 & 배포

결과물:
├── 제시간에 (약속대로 금요일)
├── 모든 테스트 통과
├── 운영 버그 없음 (첫 30일)
├── 깔끔하고 유지보수 가능한 코드
└── 완전한 문서 (테스트!)

ROD/TFD/DGTF 없이 (일반적인 프로젝트):
├── 1-3일차: 설계 없이 코딩
├── 4-5일차: "거의 다 됐어요" (사실 아님)
├── 6-7일차: 버그 수정, 리팩토링
├── 8일차: 허둥지둥, 코너 자르기
├── 9일차: "하나만 더 수정"
├── 10일차: 드디어 배포 (늦음)
├── 11일차+: 운영 버그 도착
```

---

## 조화로운 세 가지 방법론

```
        ROD (설계 단계)
        "무엇을 만들지"
              │
              ▼
        TFD (테스트 단계)
        "어떻게 검증할지"
              │
              ▼
        DGTF (구현 단계)
        "어떻게 침착하게"
              │
              ▼
        결과
        품질 좋은 소프트웨어, 제시간에
```

각 방법론이 다른 것들을 지원합니다:
- ROD가 TFD에 명확한 타겟 제공
- TFD가 DGTF에 즉각적인 피드백 제공
- DGTF가 ROD와 TFD가 실제로 따라지게 보장

하나라도 빠지면, 시스템이 흔들립니다.

---

## 첫 걸음

한 번에 모든 것을 채택할 필요 없습니다.

### 1주차: 인식
- 시스템 1이 활성화될 때 알아차리기
- DGTF 멈춤 연습
- 아직 방법론 변경 없음

### 2주차: 작은 ROD
- 작은 기능 하나 선택
- 코딩 전에 서비스 체인 작성
- 구현이 어떻게 달라지는지 알아차리기

### 3주차: TFD 추가
- ROD 서비스에 대해 테스트 먼저 작성
- 피드백 루프 경험
- 테스트가 요구사항을 명확히 하는 것 보기

### 4주차: 완전한 통합
- 실제 기능에 세 가지 모두 적용
- 차이 측정
- 경험에 따라 조정

---

## 마지막 생각

24년간 소프트웨어를 만들어왔습니다.

재능 있는 개발자들이 번아웃되는 것을 봤습니다. 유망한 프로젝트가 무너지는 것을 봤습니다. 수십 년, 기술, 대륙을 넘어 같은 실수가 반복되는 것을 목격했습니다.

그리고 이것을 배웠습니다:

**문제는 절대 지식이 아닙니다. 문제는 압박 하의 사고입니다.**

ROD, TFD, DGTF는 혁명적인 기술이 아닙니다. 모든 것이 패닉하라고 밀어붙일 때 명확하게 생각하도록 돕는 간단한 규율입니다.

- **ROD**는 압박이 닥치기 전에 무엇을 만들지 알게 합니다
- **TFD**는 언제 완료인지 알게 합니다
- **DGTF**는 준비한 것을 실제로 사용하게 합니다

함께, 소프트웨어 개발을 광란의 허둥지둥에서 침착하고 전문적인 실천으로 변화시킵니다.

---

## 초대

이 방법론들은 제 일과 팀을 변화시켰습니다. 여러분의 것도 변화시킬 수 있습니다.

하지만 제 말을 믿지 마세요. **시도해보세요.**

기능 하나를 선택하세요. 프레임워크를 적용하세요. 무슨 일이 일어나는지 보세요.

그리고 그렇게 할 때—평소의 패닉 없이 제시간에 품질 좋은 코드를 배포할 때—이것이 무슨 의미인지 이해하게 될 것입니다:

> **"느린 것이 매끄럽고, 매끄러운 것이 빠르다."**

---

*이 시리즈를 읽어주셔서 감사합니다. 도움이 됐다면, 필요한 개발자에게 공유해주세요. ROD/TFD/DGTF를 시도하면, 어떻게 됐는지 알려주세요.*

*연결:*
- *GitHub: [dont-go-too-fast](https://github.com/bakmeon/dont-go-too-fast)*
- *Email: stillblueist@naver.com*

*행운을 빕니다, 그리고 너무 빨리 가지 마세요.* 🐢

---

**시리즈 완료:**
1. [왜 좋은 개발자가 압박 속에서 나쁜 결정을 내리는가]()
2. [ROD: 완전하게 설계하고, 빠르게 구현한다]()
3. [TFD: 요구사항 = 테스트]()
4. [DGTF: 느린 것이 매끄럽고, 매끄러운 것이 빠르다]()
5. **모든 것을 합치기** ← 지금 여기

---

*추신 — 이 아이디어들을 확장한 책을 썼습니다: "Don't Go Too Fast: 압박 하에서 품질을 유지하는 소프트웨어 개발 철학." 아마존에서 구매 가능합니다. 완전한 이론적 기반, 더 많은 예시, 팀 채택을 위한 가이드가 포함되어 있습니다.*
