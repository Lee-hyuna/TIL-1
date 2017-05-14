# 요소별 정렬 방법

## block 요소의 정렬

- 수평 정렬
  - block 요소의 정렬을 위해서는 *width 설정*이 필요.
  - 가운데 정렬 : 좌우의 margin 값을 auto로 자동 맞춤 `margin: 0 auto`
  - 왼쪽 오른쪽 정렬 : `float: left, right`

- 수직정렬

  - div를 하나의 테이블 셀처럼 사용하면서 정렬

    `vertical-align:middle; display:table-cell;`

> vertical-align

# inline 요소의 정렬

- inline요소를 감싸고 있는 `wrapper div` 추가가 필요
- 수평정렬
  - text-align: left,right,center : inline을 감싸고 있는 div에 적용.

````html
  <div style="text-align:center;">
    <span>sdfsdf</span>
  </div>
````

  - 수직정렬

    - vertical-align: baseline|*length*|sub|super|top|text-top|middle|bottom|text-bottom|initial|inherit;

      https://www.w3schools.com/cssref/playit.asp?filename=playcss_vertical-align&preval=25px



## Reference

https://www.w3schools.com/cssref/pr_pos_vertical-align.asp

http://inspiredjw.com/entry/CSS%EB%A1%9C-DIV-%ED%83%9C%EA%B7%B8%EB%A5%BC-%EC%83%81%ED%95%98-%EC%A2%8C%EC%9A%B0-%EA%B0%80%EC%9A%B4%EB%8D%B0-%EC%A0%95%EB%A0%AC%ED%95%98%EA%B8%B0