# Java Stack, Deque

자료구조중 스택과 덱의 사용법에 대해 알아본다.

````java
Stack<YourObject> stack = new Stack<>();
Deque<YourObject> deque = new ArrayDeque<>();

// 반복 방법 1
Iterator<YourObject> iter = stack.iterator();
while (iter.hasNext()){
    System.out.println(iter.next());
}

// 반복 방법 2
stack.forEach(i -> System.out.print(i));
````

- peek() : top 조회
- pop() : top 꺼내기
- push() : top에 넣기



## Reference

https://docs.oracle.com/javase/7/docs/api/java/util/Stack.html

https://stackoverflow.com/questions/12957123/how-would-i-iterate-a-stack-in-java