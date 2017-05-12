# Ruby 기본 문법 정리

루비(Ruby)는 마츠모토 유키히로가 개발한 동적 객체 지향 스크립트 프로그래밍 언어로, '스트레스가 없는 인간 중심의 언어'라는 철학을 가지고 있다.

## 특징

- 순수 객체 지향 언어, 정수/문자열 등을 포함한 데이터 형식 모든 것이 객체
- 클래스 정의, 가비지 컬렉션, 정규표현식, 멀티 스레드, 예외처리, 반복, 클로저, Mixin, 연산자 오버로드 등 제공
- 이식성이 높음, 유닉스, 리눅스, Window, OS X, MS-DOS, OS/2 등에서 동작.

리

## 기본 문법

- **프린트**

````ruby
puts "Hello World!"
puts "value가 3000다 크면 출력 됩니다. " if value > 3000
````

- **기초 루비 코드**

````ruby
# 리터럴을 포함한 모든 것이 객체기 때문에 동작.
-199.abs # 199
"ruby is cool".length # 12
"Your mother is nicce.".index("u") #2
````

- **제어문**

````ruby
if "fablic".length > 3
  puts 'ya'
else
  puts 'nop'
end
# 결과:
# ya

n = 0
while n < 3
  puts 'foobar'
  n += 1
end

# 결과:
# foobar
# foobar
# foobar
````

- **컬렉션**

해쉬의 키와 값, 구분자는 "=>"를 사용

````ruby
# 배열
a = [1, 'hi', 3.14, 1, 2, [4, 5]]

puts a[2] # 3.14
puts a.[](2) # 3.14
puts a.reverse # [[4, 5], 2, 1, 3.14, 'hi', 1]
puts a.flatten.uniq # [1, 'hi', 3.14, 2, 4, 5]

# 해시
hash = { :water => 'wet', :fire => 'hot' }
puts hash[:fire] # Prints: hot

hash.each_pair do |key, value| # Or: hash.each do |key, value|
  puts "#{key} is #{value}"
end
# water is wet
# fire is hot
````

- **배열 반복**

>  array.each, array.each_index 로 배열 요소 및 인덱스 접근
>
> " " 안에서 배열 접근시 #{ }사용

````ruby
array = [1, 'hi', 3.14]
array.each {|item| puts item }
# => 1
# => 'hi'
# => 3.14

array.each_index {|index| puts "#{index}: #{array[index]}" }
# => 0: 1
# => 1: 'hi'
# => 2: 3.14

# The following uses a Range
(3..6).each {|num| puts num }
# => 3
# => 4
# => 5
# => 6
````

- **클래스**

> attr_reader : 게터 함수
>
> attr_writer : 세터 함수
>
> attr_accessor : 게터, 세터 함수.
>
> initialize : 생성자
>
> 전역 변수는 $로 시작하고, 인스턴스 변수는 @로 시작하며, 클래스 변수는 @@로 시작

````ruby
class Person
  attr_reader :name, :age
  def initialize(name, age)
    @name, @age = name, age
  end
    def <=>(person) # 정렬을 위한 비교 연산자
    @age <=> person.age
  end
  def to_s # 객체를 String으로 바꾸기 위한 메서드
    "#@name (#@age)"
  end
end

group = [
  Person.new("Bob", 33),
  Person.new("Chris", 16),
  Person.new("Ash", 23)
]

puts group.sort.reverse
````



## Reference

위키 백과 :  루비_(프로그래밍_언어)

[http://dimdim.tistory.com/entry/Ruby-기초-문법-정리](http://dimdim.tistory.com/entry/Ruby-%EA%B8%B0%EC%B4%88-%EB%AC%B8%EB%B2%95-%EC%A0%95%EB%A6%AC) [딤딤이의 블로그]

루비 20분 가이드 https://www.ruby-lang.org/ko/documentation/quickstart/3/