# Java Collections의 API 정리

java collections doc을 보면서 유용할 것 같은 명세를 정리해본다.

### sort

public static <T> void sort(List<T> list, Comparator<? super T> c)

- 파라미터 : 리스트, 비교자
- 리턴 : void
- 결과 : 인자로 넣었던 list가 정렬됌.

```java
Integer[] data = { 3, 1, 4, 5 };
// 비교자 람다로 구현
Collections.sort(Arrays.asList(data), (o1, o2) -> o1 - o2);
```



### binarySearch

public static <T> int binarySearch(List<? extends T> list, T key, Comparator<? super T> c)

- 파라미터 : 리스트, 키, 비교자
- 리턴 : 찾은 해당 숫자/없을시 -값
- 주의사항 : 정렬을 해놓고, 사용해야함.

```java
Integer[] data = { 3, 1, 4, 5 };
Collections.sort(Arrays.asList(data), (o1, o2) -> o1 - o2);
Collections.binarySearch(Arrays.asList(data), 9, (o1, o2) -> o1 - o2)
```


### Reverse

public static void reverse(List<?> list)

- 파라미터 : 리스트
- 결과 : 인자로 넣었던 list가 역순 정렬됌.



### Swap

public static void swap(List<?> list, int i, int j)

- 파라미터 : 리스트, 인덱스1, 인덱스2
- 결과 : 리스트의 두 인덱스가 바뀜.



### Min, Max

public static <T> T min/max(Collection<? extends T> coll,  Comparator<? super T> comp)

- 파라미터 : 리스트,  비교자
- 결과 : 가장 작은 값을 리턴.

```java
Integer[] data = { 3, 1, 4, 5 };
Collections.min(Arrays.asList(data), (o1, o2) -> o1 - o2);
```



### rotate

public static void rotate(List<?> list, int distance)

- 파라미터 : 리스트, 이동시킬 인덱스
- 결과 : 리스트의 값들이 지정한 인덱스 만큼 이동함.



### replaceAll

public static <T> boolean replaceAll(List<T> list, T oldVal, T newVal)

- 파라미터 : 리스트, 찾을 값, 바꿀값
- 결과 : oldVal 값이 newVal로 바뀜

````java
Integer[] data = { 3, 6, 4, 5 };
Collections.replaceAll(Arrays.asList(data), 5, 100);
````



## Reference

https://docs.oracle.com/javase/7/docs/api/java/util/Collections.html