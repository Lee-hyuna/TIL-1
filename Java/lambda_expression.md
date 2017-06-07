# Java Lambda

람다의 철학 = 지울 수 있는건 모두 지우자! 간결!

## 람다의 특징

1. 이름이 없음
2. 함수형 특성, 지역변수가 없음, input이 같으면 결과도 같음.



## 람다의 사용

다음 인터페이스가 있다고 할때.

````java
interface Movable{
	void move(String str);
}
````

람다식을 쓰지 않는다면.

````java
// 이렇게나
class Car implements Movable{
    @Override
    public void move(String str) {
        System.out.println("gogo car move" + str);
    }
}

// 요롷게나
Movable movable = new Movable() {
    @Override
    public void move(String str) {
        System.out.println("gogo move move" + str);
    }
};
````

람다식을 쓴다면

````java
Movable movable2 = str -> System.out.println("gogo move move" + str);
````

> 구현해야할 추상 메서드가 1개 뿐인 인터페이스를 구현
>
> 추상 메서드 한 개 강제 @FunctionalInterface

## 자바 기본 함수형 인터페이스

자주 쓰는 함수적 인터페이스를 제공해준다.

### Consumer

매개 변수 있고, 리턴 값 없음 : `accept()` 메서드, void 함수를 람다식으로 선언![img](http://cfile26.uf.tistory.com/image/2418983555FE5A372A486E)

````java
Consumer<String> consumer = t -> System.out.println(t + "8");
consumer.accept("자바");
````

### Supplier

매개 변수 없고, 리턴 값만 있음.

![img](http://cfile29.uf.tistory.com/image/2207094655FE5C7624E57D)

````java
IntSupplier intSupplier = () -> {
  int num = (int) (Math.random() * 6) + 1;
  return num;
};

int num = intSupplier.getAsInt();
````

### Function

매개 변수, 리턴 모두 있음

![img](http://cfile26.uf.tistory.com/image/241F824F55FF96710941E4)

`Function<T, R>` : T를 R에 매핑한다는 의미

````java
public class FunctionExam {
    private static List<Student> list = Arrays.asList(
            new Student("Jackie", 90, 65), 
            new Student("Jolie", 100, 100)
    );
 
    public static void printString(Function<Student, String> function) {
        for (Student std : list) {
            // apply를 이용해서 원하는 변수를 읽음.
            System.out.print(function.apply(std) + " ");
        }
        System.out.println();
    }
    
    public static void main(String[] args) {
        System.out.println("학생 이름: ");
        printString( t -> t.getName() );        
    }
}
````

### Predicate

`test` 메서드 제공, 매개변수와 boolean 리턴

````java
// apple인 과일만 리스트에 담기
List<Fruit> extractApple(List<Fruit> fruits){
    List<Fruit> resultList = new ArrayList<>();
    for(Fruit fruit : fruits){
        if("apple".equals(fruit.getName())){
            resultList.add(fruit);
        }
    }

    return resultList;
}

// 빨간색 과일만 담기
List<Fruit> extractRed(List<Fruit> fruits){
    List<Fruit> resultList = new ArrayList<>();
    for(Fruit fruit : fruits){
        if("red".equals(fruit.getColor())){
            resultList.add(fruit);
        }
    }
    return resultList;
}

````

위 두가지 메서드를 하나로 합칠 수 는 없을까?

```java
static List<Fruit> extractFruitList(List<Fruit> fruits, Predicate<Fruit> predicate) {
    List<Fruit> resultList = new ArrayList<>();
    for(Fruit fruit : fruits){
        if(predicate.test(fruit)){
            resultList.add(fruit);
        }
    }
    return resultList;
}

// 사용
List<Fruit> appleList = extractFruitList(fruits, fruit -> "apple".equals(fruit.getName()));
```



## Reference

[http://palpit.tistory.com/673](http://palpit.tistory.com/673)

http://multifrontgarden.tistory.com/124

