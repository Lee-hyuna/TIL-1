# Java Object Array Sort

자바 객체 배열을 정렬해보자.

-  특정 클래스 내에서만 사용하기 위해서 내부 클래스를 활용.
- 정렬을 위해 Comparable 인터페이스를 구현한다.

````java
import java.util.Arrays;
import java.util.Scanner;

public class ConferenceSort {
    // main에서 테스트 하기 위해 static 내부 클래스로 선언하였음.
    public static class Conference implements Comparable<Conference> {
        int index;
        int num;
        int start;
        int end;

        public Conference(int index, int num, int start, int end) {
            this.start = start;
            this.num = num;
            this.index = index;
            this.end = end;
        }

        // 정렬을 위한 Comparable 인터페이스 구현
        @Override
        public int compareTo(Conference con) {
            // 오름 차순 정렬
            if (this.end < con.end) {
                return -1;
            }
            else if (this.end == con.end) {
                return 0;
            }
            else {
                return 1;
            }
        }
    }

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int count = scan.nextInt();

        // 인스턴스 생성이 아닌, '레퍼런스'를 생성
        Conference conference[] = new Conference[count];

        for (int i = 0; i < count; i++) {
            // 레퍼런스 배열 안에 인스턴스 객체를 생성
            conference[i] = new Conference(i, scan.nextInt(), scan.nextInt(), scan.nextInt());
        }

        // 방법 1: 정렬!
        Arrays.sort(conference);
      
        // 방법 2 : 인터페이스 구현을 생략하고 다음과 같이 작성할 수도 있음.
        Arrays.sort(conference, new Comparator<Conference>() {
            @Override
            public int compare(Conference o1, Conference o2) {
                return o1.end - o2.end;
            }
        });
      
        // 방법 3 : 람다식 사용, 역시나 인터페이스 구현 생략 가능(와우 람다식 대단행.)
        Arrays.sort(conference, (o1, o2) -> o1.end - o2.end);
    }
}
````

## Reference

객체 배열 생성 : http://yoonka.tistory.com/398

객체 배열 정렬 : http://haneulnoon.tistory.com/198

람다 정렬 : https://www.mkyong.com/java8/java-8-lambda-comparator-example/