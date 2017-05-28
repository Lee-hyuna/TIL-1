# 정규식

java에서는 `import java.util.regex.Pattern;`을 임포트해서 사용.

> 다음과 같이 사용

````java
boolean b = Pattern.matches("패턴", "문자열");
String.replaceAll("패턴", "문자열");
````

- abcd만 필터링

  `(?i)` : 대소문자 구분 안함

  `[^abcd]` : 문자 'a'  'b'  'c' 'd' 가 아닌 것.

  `str.replaceAll("(?i)[^abcd]", "");`

- abc, def, ghi 문자열 존재하는지 확인

  `Pattern.matches(".*(abc|def|ghi).*", "");`

