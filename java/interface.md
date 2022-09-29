## 인터페이스
자바가 다중 상속을 지원하지 않는 것은 지금쯤 잘 알고있는 사실일 것이다.
하지만 다중 상속의 이점은 버릴 수 없는 가치있는 존재이다. 그 때문에 자바에서는 인터페이스라는 것을 통해 다중 상속을 지원한다.
- 다른 클래스를 작성할 때 기본이 되는 틀을 제공, 중간 매개 역할 담당
- abstract class는 추상 메소드, 생성자, 필드, 메서드를 포함할 수 있는데, 인터페이스는 추상 메서드와 상수만을 포함할 수 있다.

## 인터페이스의 장점
- 대규모 프로젝트 개발 시 일관되고 정형화된 개발을 위한 표준화가 가능하다.
- 클래스의 작성과 인터페이스의 구현을 동시에 진행할 수 있으므로 개발 시간을 단축할 수 있다.
- 클래스와 클래스 간의 관계를 인터페이스로 연결하면 클래스마다 독립적인 프로그래밍이 가능하다.

## 인터페이스의 선언
```java
public interface AClass {
    // 상수
    int a = 3;

    // 추상 메서드
    public abstract getA(a);
}
```
위와 같이 나타낼 수 있으며, 인터페이스를 선언할 때에는 접근 제어자와 함께 interface 키워드를 사용하면 된다.
단, 클래스와는 달리 인터페이스의 모든 필드는 public static final이어야 하며, 모든 메서드는 public abstract이어야 한다.
모든 인터페이스에 공통으로 적용되는 부분으로 이와 같은 제어자는 생략 가능하다.

## 인터페이스의 구현
> class 클래스이름 implement 인터페이스 이름 { ... }

위와 같은 형태로 자바에서 인터페이스를 구현한다 아래는 그 예제를 보여준다.

```java
interface Book { public abstract void open(); }

class BookA implements Book {

    @Override
    public void open() {
        System.out.println("촤라락");
    }
}

class BookB implements Book {

    @Override
    public void open() {
        System.out.println("샥");
    }
}

public class Polymorphism {
    public static void main(String[] args) {
        BookA aBook = new BookA();
        BookB bBook = new BookB();

        aBook.open();
        bBook.open();
    }
}

```
![](https://velog.velcdn.com/images/dymnam/post/94a0ec0d-94c7-4962-b669-88637ddcea36/image.png)


만약 모든 추상 메소드를 구현하지 않는다면, abstract 키워드를 사용하여 추상 클래스로 선언해야한다.

## 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법
 : 인터페이스 타입으로 객체를 생성할 수 있으며 해당 객체에 구현 클래스로 인스턴스화 할 수 있다.
 ```java
nterface Book { public abstract void open(); }

class BookA implements Book {

    @Override
    public void open() {
        System.out.println("촤라락");
    }
}

class BookB implements Book {

    @Override
    public void open() {
        System.out.println("샥");
    }
}

public class Polymorphism {
    public static void main(String[] args) {
        BookA aBook = new BookA();
        BookB bBook = new BookB();
        Book book = new Book() { // 인터페이스 인스턴스화
            @Override
            public void open() {
                System.out.println("슥삭슥삭");
            }
        };
        aBook.open();
        bBook.open();
        book.open();
    }
}

```
기본적으로 인터페이스 타입으로 선언한 객체는 구현 클래스 내에서 생성한 메서드, 필드를 사용할 수 없다.

## 인터페이스 상속
앞서 말했던 것처럼 추상메서드는 다중 상속이 불가능하지만 인터페이스는 다중 상속이 가능하다.
```java
interface Book { public abstract void open(); }

interface Cover { public abstract void page(); }

interface Main extends Book, Cover {

}

public class Polymorphism implements Book, Cover{
    public static void main(String[] args) {
        
    }
    @Override
    public void open() {
        System.out.println("촤라락");
    }

    @Override
    public void page() {
        System.out.println("31p");
    }
}

```
위에서 보는 것처럼 클래스에서 인터페이스를 상속받을 때에는 implement, 인터페이스에서 인터페이스를 상속받을 때에는 extends 로 상속 받는다.

## 인터페이스의 기본( Default ) 메서드, 자바 8
- Java 8 이전의 Interface의 메서드는 선언만 가능하며 구현할 수 없었다. 하지만 Java 8 이후부터 Default 메서드를 사용해 메서드 구현부를 가질 수 있게 되었다.
- default 메서드는 상속 받는 클래스에서 필수적으로 구현하지 않아도 되는 특징을 가진다.

```java
interface Book {
    static String nameOfBook() {
        return "자바 어렵눙...";
    }
}
```

## 인터페이스의 static 메서드, 자바 8
- 자바 8 부터 지원되는 static 메서드는 인터페이스를 이용한 간단한 기능을 가지는 유틸리티 성 인터페이스를 만들 수 있게한다.

#### 특징
- 상속이 불가능함.
- 인터페이스의 상수와 같은 형식으로 쓰인다.
- interface 명 . static 메서드 명 으로 사용할 수 있다.

```java
interface Book {
    static String nameOfBook() {
        return "자바 어렵눙...";
    }
}


public class Polymorphism {
    public static void main(String[] args) {
        System.out.println(Book.nameOfBook());
    }
}

```
보는 것처럼 코드가 간단해진 것을 볼 수 있다.
![](https://velog.velcdn.com/images/dymnam/post/cc3e332a-95dc-49a3-8814-502f4f406d84/image.png)

## 인터페이스의 private 메서드, 자바 9
- 자바 9 부터는 private를 사용함으로써 외부에 공개되지 않게 할 수 있어졌다.
- 그러므로써 코드의 중복을 피하고 Interface의 캡슐화를 유지할 수 있게 하였다.

![](https://velog.velcdn.com/images/dymnam/post/bf1297d3-9b91-48a5-8119-3b309660c988/image.png)
위 그림을 보면 private로 작성되어 외부에 공개되지 않게 한 모습을 볼 수 있다.

- private, private static 모두 사용 가능하지만 각각 호출 가능한 메서드가 다르다.
- private method : private, abstract, default, static 메서드 호출 가능
 - pricate static method : static, private static 메서드[만] 호출 가능.
