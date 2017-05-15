# CSS display, visibility 속성

inline 속성 : `<b>`, `<span>`, `<img>`, `<a>` 등 줄 바꿈이 되지 않음, 주로 줄속에 넣는 요소

block 속성 : `<table>`, `<p>`, `<div>`, `<form>` 등 줄 바꿈이 됌. 가로 화면 100%를 차지.

## display

- **display: inline** : 요소들을 inline 요소 처럼 표시
- **display: block** : 요소들을 block 요소 처럼 표시
- **display:none** : 박스가 생성되지 않고, *공간을 차지하지도 않음.*
- **float** : block 요소들을 정렬함.
- **dispaly:inline-block** : 박스모양을 inline처럼 늘어뜨림.
- **display:table-cell** : 테이블 셀처럼 블록요소들을 늘어뜨린다.



## inline, float, inline-block, table-cell의 차이

셋 모두, block 요소들을 쭉 늘어뜨려 정리하는 역할을 가지고 있다. 그럼 어떤 차이점이 있나?

1. **inline, inline-block**

인라인 요소를 사용할 경우, 정렬은 되지만 요소들을 엔터로 구분한 것 때문에 빈칸이 생기게 된다.

![display:inline image](https://i.stack.imgur.com/aDgsb.png)

2. **float**

float 요소를 사용할 경우, 감싸고 있는 div요소가 붕괴된다.

![float:left image](https://i.stack.imgur.com/oIMvg.png)

3. **table-cell**

빈칸 없이, 테이블 모양으로 늘어뜨려진다. margin 속성을 주어도 요소간에 공간이 생기지 않는다.

![display:table-cell image](https://i.stack.imgur.com/BPtXn.png)

## visibility

어떤 요소를 보이게할지 숨길지 결정

- visibility: visible : 기본값, 요소가 보임
- visibility: hidden : 요소가 보이지 않음, *공간을 차지함*
- visibility: collapse : <table> 태그에서만 사용, 행과 열을 숨김.



## Reference

http://stackoverflow.com/questions/11805352/floatleft-vs-displayinline-vs-displayinline-block-vs-displaytable-cell

http://aboooks.tistory.com/85

