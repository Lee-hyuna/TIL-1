# Java Character

## Character.isLetter(char);

아스키 코드 문자인지.



## Character.isDigit(char);

아스키 코드 숫자인지.



## Character.getNumericValue(char);

char 내용이 숫자이면, 해당 숫자를 리턴, 문자이면 A~Z : 10~35

````java
ch1 = 'j';
ch2 = '4';
int i1 = Character.getNumericValue(ch1); // 19
int i2 = Character.getNumericValue(ch2); // 4
````



