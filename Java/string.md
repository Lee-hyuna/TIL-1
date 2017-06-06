# String

String 편집에 StringBuilder, StringBuffer, String이 있다.

단순 성능만 볼 경우, **StringBuilder > StringBuffer >>> String** 순으로 빠르다.

## String 특성

- 한번 선언 후 바뀔 수 없음
- `heap` 메모리 공간에 선언, 할당된 메모리 공간이 변하지 않음.
- 문자 편집시 새로운 String 객체 생성후 생성된 문자열 저장.
- 간단하게 사용 가능
- 동기화에 신경쓰지 않아도 됌.

## StringBuffer

멀티 스레드 환경에서 동기화 지원

## StringBuilder

동기화 보장X

````java
StringBuilder result = new StringBuilder();
result.append(c);
result.revers().toString();
````

## Reference

[http://ooz.co.kr/298](http://ooz.co.kr/298)