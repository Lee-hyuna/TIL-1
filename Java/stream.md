# Java Stream

동작 패턴, `stream()` 메서드를 사용해서 stream을 가져오고, 여러가지 기능(filter, sorted, map, collect)를 체인처럼 엮어서 데이터를 처리 한 뒤 `collect`로 원하는 타입으로 변경

- **filter**

````java
List <DataMap> resultList = this.tutorAbsenceManageService.selectAbsentList (tutorAbsenceManageVo);

List<DataMap> result = resultList.stream().filter( obj -> {
     String date = dateFormat.format((Date) obj.get("absenceDt"));
     return Objects.equals(date, dayMap.get("date")) && Objects.equals(dayMap.get ("periodNo"), obj.get("periodNo"));
}).collect(Collectors.toList());
````

- **collect**

  필터링 또는 매핑된 요소 새로운 컬렉션에 수집하고, 이를 리턴.

  - .collect(Collectors.toList());
  - .collect(Collectors.toSet());
  - .collect(toMap(Function<T, K> keyMapper, Function<T, U> valueMapper)) : T를 K와 U로 매핑해서 K를 키로, U를 값으로 Map에 저장 

- **distinct**

  중복을 제거한 유니크 엘리먼트를 리턴한다

- **limit(n)**

  주어진 사이즈(n)에 까지의 stream을 리턴한다.

````java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8);
numbers.limit(5).forEach(System.out::println); 
````

- **skip(n)**

  주어진 엘리먼트 길이 까지 제외한 stream을 리턴한다.

- **sorted(), sorted(Comparator<T> comparator)**

````java
List transactionsIds = 
    transactions.parallelStream()
        .filter(t -> t.getType() == Transaction.GROCERY)
        .sorted(Comparator.comparing(Transaction::getValue).reversed())
        .map(Transaction::getId)
        .collect(Collectors.toList());
````

- **allMatch/findAny**

````java
boolean expensive =
	transactions.stream()
        .filter(t -> t.getType() == Transaction.GROCERY)
        .findAny();

transactions.stream()
    .filter(t -> t.getType() == Transaction.GROCERY)
    .findAny()
    .ifPresent(System.out::println);
````

- **reduce**

연산한 결과를 리턴, 첫번째 인자는 초기값.

````java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8);
int product = numbers.stream().reduce(1, (a, b) -> a * b);
````

- **count()**
- **sum()**

````java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8);
int product = numbers.stream().mapToInt(e -> e).sum();
````

- **forEach()**

````java
items.forEach((k,v)->System.out.println("Item : " + k + " Count : " + v));
items.forEach((k,v)->{
  System.out.println("Item : " + k + " Count : " + v);
  if("E".equals(k)){
    System.out.println("Hello E");
  }});
````

- **grouping by**

그룹핑후, 집계 함수 사용

````java
List<String> items =
  Arrays.asList("apple", "apple", "banana",
                "apple", "orange", "banana", "papaya");

Map<String, Long> result =
  items.stream().collect(
  Collectors.groupingBy(
    Function.identity(), Collectors.counting()
  )
);
````

## Reference

Collector : http://palpit.tistory.com/652

http://starplatina.tistory.com/entry/%EC%9E%90%EB%B0%94-%EC%8A%A4%ED%8A%B8%EB%A6%BC-API