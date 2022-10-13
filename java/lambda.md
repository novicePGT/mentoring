## 람다식
-----
람다식이란 메서드로 전달할 수 있는 익명 함수를 단순화한 것으로 함수형 인터페이스를 구현한 익명 클래스의 인스턴스라 할 수 있다.

#### 특징
-----
- 익명 : 보통의 메서드와 달리 이름이 없어 익명이라 말한다.
- 함수 : 람다는 메서드처럼 특정 클래스에 종속되지 않아 함수라고 부르지만, 메서드처럼 파라미터 리스트, 바디, 리턴, 예외를 포함한다.
- 전달 : 람다 표현식을 메서드 인수로 전달하거나 변수로 저장할 수 있다.
- 간결성 : 익명 클래스처럼 많은 자잘한 코드를 구현할 필요가 없다.

## 람다식 사용법
-----
먼저 인터페이스와 추상 메소드를 만들어 예제를 확인해보자
```java
@FunctionalInterface
interface ExampleInterface {
    int sum (int x, int y);
}

public class Lambda {
    public static void main(String[] args) {
        ExampleInterface exampleInterface = new ExampleInterface() {
            @Override
            public int sum(int x, int y) {
                return x + y;
            }
        };
    }
}
```
위 코드를 람다식을 이용하여 더 간결하게 만들 수 있다.
```java
@FunctionalInterface
interface ExampleInterface {
    int sum (int x, int y);
}

public class Lambda {
    public static void main(String[] args) {
        ExampleInterface exampleInterface = (x, y) -> {
            return x+y;
        };
    }
}

```
확실히 더 짧아진 것을 확인할 수 있을 것이다.
물론 이해까지 더해진다면 짧은 코드로 가독성있게 확인할 수 있을 것이다.
- (x, y) : 괄호 안에 두 파라미터를 입력하며, 타입 생략이 가능하다.
- { return x+y } : {} 안에 로직을 작성한다.

이 경우에는 위 코드보다 더욱 간결하게 만들 수 있다.
```java
@FunctionalInterface
interface ExampleInterface {
    int sum (int x, int y);
}

public class Lambda {
    public static void main(String[] args) {
        ExampleInterface exampleInterface = (x, y) -> x+y;
    }
}
```
위 코드를 보면 그 길었던 코드가 한 줄로 짧아진 것을 볼 수 있다. 솔직히 놀랍다.
하지만, 람다식을 너무 남발하면 코드가 오히려 지저분해지고 해석하기 어려울 수 있으며 디버깅 또한 힘들어진다. 그러므로 람다를 적절하게 사용하는 것이 중요하다.

#### 정리
-----
- 람다의 모든 매개변수 타입은 생략 가능하다.
 	-> 컴파일러가 타입을 추론할 수 없다고 할 때는 제외한다.
- 매개변수가 하나일 때 괄호 () 생략 가능하다.
	-> 매개변수의 타입까지 작성해야할 때는 생략 불가하다.
- 람다 바디 문장이 하나일 때 대괄호 {} 생략 가능하다.

- 람다의 직렬화는 피하자
	-> 람다도 익명 클래스처럼 직렬화 형태가 구현하는 가상머신에 따라 다를 수 있다.


## 함수형 인터페이스
-----
람다식을 다루기 위한 인터페이스로 함수형 인터페이스는 조건이 있다.
- 조건 : **하나의 추상 메서드만 정의**되어야 한다.
		자바 8에 추가된 디폴트 메서드이 경우 구현부가 있어 상관 없다.
        Object의 메서드를 오버라이딩하는 추상 메서드의 경우도 추상 메서드로 카운트하지 않는다.
        
#### @FunctionalInterface
-----
@FunctionalInterface : 함수형 인터페이스임을 표시하는 어노테이션
- @FunctionalInterface 로 인터페이스를 선언했지만 함수형 인터페이스의 조건을 만족하지 않는 경우 컴파일 에러가 발생.

#### 함수형 인터페이스와 익명 클래스
-----
람다는 함수형 인터페이스에서만 쓰인다.
- 추상 클래스의 인스턴스를 만들 때 람다를 쓸 수 없으니, 익명 클래스를 써야한다.

- 추상 메서드가 여러 개인 인터페이스의 인스턴스를 만들 때도 익명 클래스를 쓸 수 있다.

- 람다는 자신을 참조할 수 없다. -> 람다에서의 this 키워드는 바깥 인스턴스를 가리킨다. 반면 익명 클래스에서의 this는 익명 클래스의 인스턴스 자신을 가리킨다. 그래서 함수 객체가 자신을 참조해야 한다면 반드시 익명 클래스를 써야 한다.

## Variable Capture
-----
람다식은 익명 함수처럼 자유 변수( free variable )를 활용할 수 있다.
이 같은 동작을 람다 캡처링이라고 한다.
- 자유변수 : 파라미터로 넘겨진 변수가 아닌 외부에서 정의된 변수

접근 가능 대상 :
- 인스턴수 변수
- 클래스 변수
- 지역 변수 * 람다에서 참고하는 지역 변수는 final로 선언되거나 변경이 없어야 한다 *

#### 지역 변수의 제약
-----
> 인스턴스 변수는 힙에 저장되는 반면 지역 변수는 스택에 위치한다. 람다에서 지역 변수에 바로 접근할 수 있다는 가정하에 람다가 스레드에서 실행된다면 변수를 할당한 스레드가 사라져서 변수 할당이 해제되었는데도 람다를 실행하는 스레드에서는 해당 변수에 접근하려 할 수 있다. 따라서 자바 구현에서는 원래 변수에 접근을 허용하는 것이 아니라 자유 지역 변수의 복사본을 제공한다. 따라서 복사본의 값이 바뀌지 않아야 하므로 지역 변수에는 한 번만 값을 할당해야 한다는 제약이 생긴 것이다.
출처: 모던 자바 인 액션

- 지역 변수에 대해 직접 접근이 아닌 값 복사( Variable Capture )를 통해 각기 다른 메모리 공간에서 변수 스코프에 대한 참조 문제를 해결하는 내용

#### 예제
-----
```java
public class Lambda {
    int a = 100;
    
    public void print1() {
        int b = 200;
        final ExampleInterface exampleInterface = () -> {
            return System.out.println(a);
        };
    }
}
```
- 람다 시그니처의 파라미터로 넘겨진 변수가 아닌 외부에서 정의된 변수를 **자유 변수**라고 함.
- 자유 변수를 참조하는 행위를 **람다 캡처링** 이라고 함.

## 메소드, 생성자 레퍼런스
-----
#### 메소드 참조
-----
- 특정 메서드만 호출하는 람다의 축약형.
- 명시적으로 메서드명을 참조하여 상황에 따라 가독성을 높일 수 있다.
![](https://velog.velcdn.com/images/dymnam/post/ab1a1359-b732-45db-9d46-7ea69afb0aeb/image.png)

```java
public class Lambda {
    public static void main(String[] args) {
        ExampleInterface exampleInterface = (x, y) -> x+y;
    }
}
```
위 코드를 더어ㅓㅓㅓㅓㅓ 생략해 파라미터까지 제거할 수 있다.
```java
@FunctionalInterface
interface ExampleInterface {
    int sum (int x, int y);
}

public class Lambda {
    public static void main(String[] args) {
        ExampleInterface exampleInterface = Integer::sum;
    }
}

```
- 함수형 인터페이스에서 선언한 추상 메소드의 타입 :: 메소드명 이렇게 적어주면 이게 람다식 메소드 참조.

#### 생성자 참조
-----
- 클래스명과 new 키워드를 통해 기존 생성자의 참조를 만들 수 있다.
```java
@FunctionalInterface
interface ExampleInterface {
    Member example(String name, int age);
}

class Member {
    private String name;
    private int age;

    Member() {
        
    }

    Member(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

public class Lambda {
    public static void main(String[] args) {
        ExampleInterface exampleInterface = new ExampleInterface() {
            @Override
            public Member example(String name, int age) {
                return new Member(name, age);
            }
        };
    }
}

```
위 코드의 lambda 클래스를 람다식으로 바꾸면 아래와 같아진다.
```java
public class Lambda {
    public static void main(String[] args) {
        ExampleInterface exampleInterface = ((name, age) -> new Member(name, age));
    }
}
```
여기서 생성자 참조를 하게 되면 아래와 같아진다.
```java
public class Lambda {
    public static void main(String[] args) {
        ExampleInterface exampleInterface = Member::new;
    }
}
```

정말 간결해졌다.. 하지만 이 코드만 보고 이게 무엇을 의미하는지는 알기 아직까지는 힘든 것 같다..
