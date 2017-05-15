# CSS Position 속성 정리

- **static 속성**
  - 기본값
  -  `top`, `right`, `bottom`, `left`  속성이 **무시**됌.
- **relative 속성**
  - 별도의 프로퍼티 지정하지 않는 이상 static과 동일
  - **인접요소**와의 거리 설정 `top`, `right`, `bottom`, `left` 지정
- **absolute 속성**
  - 가장 가까운 **조상 요소**에 상대적 위치가 결정
- **fixed 속성**
  - 스크롤 되더라도 같은 곳에 위치, `top`, `right`, `bottom`, `left` 지정
  - 모바일에서는 고정 엘리먼트 지원이 불안정

## Reference

position : http://ko.learnlayout.com/position.html