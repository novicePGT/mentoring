## Generic
-----
제네릭을 번역하면 '일반적인' 이라는 뜻이며, 자바에서 제네릭이란 데이터의 타입을 일반화한하는 의미이다.

제네릭은 클래스나 메소드에서 사용할 내부 데이터 타입을 컴파일 시에 미리 지정하는 방법으로 **'객체의 타입 안정성을 높일 수 있다.' & '리턴값에 대한 타입 변환 및 타입 체크에 들어가는 노력을 줄일 수 있다.' 는 장점**이 있다.

#### 간단한 예제
```java
List<> list = new ArrayList<>();
```

## 클래스의 선언
-----
```java
public class Main<T> {
}
```
클래스의 경우에는 위와 같이 나타낼 수 있다.
T는 타입을 나타내며, 아직 정해져 있지 않아서 우리가 지정해주는 방식이다.

```java
public class MainEx {
	public static void main(String[] args) {
    	Main<Integer> m = new Main<>();
    }
}
```
지정한 클래스를 이용하여 조합의 방식으로 사용할 수 있다.

또한, 아래와 같이 타입의 파라미터를 2개를 받을 수 있다. 예제로 Map의 Key - Value 방식을 사용해보자
```java
class Example<K, V> {
}

public class Main {
	public static void main(String[] args) {
    	Example<String, Integer> e = new Example<>();
    }
}
```

제네릭 타입을 사용할 시 주의할 점은 **타입은 모두 레퍼런스 타입이어야 한다.**
그렇다고 프리미티브( 원시 ) 타입을 사용하지 못하는건 아니다.
프리미티브 타입은 Wrapper Class로 사용하면 제네릭에서도 사용 가능하다.

#### 제네릭의 특징
-----
- 제네릭 클래스에서 static 멤버에는 타입 변수를 사용할 수 없다.
- 제네릭 클래스에서 제네릭 배열 타입의 변수를 선언할 수 있지만, new 연산자를 통한 생성하지는 못한다.
- new를 통한 컴파일 시점, T에 어떤 타입이 들어올지 모르는 이유로 instanceof 연산자에도 타입 변수를 사용할 수 없다.

#### 예제
-----
```java
class Example<E> {
    private E element;

    void setElement(E element) {
        this.element = element;
    }

    E getElement() {
        return this.element;
    }
}

public class Main {
    public static void main(String[] args) {
        Example<String> example1 = new Example<>();
        Example<Integer> example2 = new Example<>();

        example1.setElement("100");
        example2.setElement(10);

        System.out.println("example 1 = " + example1.getElement());
        System.out.println("example 1 Type = " + example1.getElement().getClass().getName());
        System.out.println("===========================");
        System.out.println("example 2 = " + example2.getElement());
        System.out.println("example 2 Type = " + example2.getElement().getClass().getName());

    }
}

```

![](https://velog.velcdn.com/images/dymnam/post/068e7439-ffa7-44e3-992c-f0a609534864/image.png)

## 제네릭의 바운디드 타입 & 와일드 카드
-----
#### 바운디드 타입
-----
제네릭 타입에 'extends' 키워드를 사용해 특정 클래스의 자식 또는 인터페이스를 구현한 타입들만 사용할 수 있도록 제한할 수 있다.
```java
class Fruit {}

interface Eat {}

// Fruit의 자식 타입만 사용 가능
class FruitBox<T extends Fruit> {}

// Eat 인터페이스를 구현한 타입만 사용 가능
class FruitBox2<T extends Eat> {}

// Fruit의 자식이면서 Eat 인터페이스를 구현한 타입만 가능
class FruitBox3<T extends Eat> {}

```

#### 와일드 카드
-----
와일드 카드란 이름에 제한을 두지 않음을 나타낸다.
자바에서는 물음표( ? )를 사용하여 와일드카드를 사용할 수 있다.

- [ <?> ] 타입 변수에 모든 타입을 사용
- [ <? extends T > ] T타입과 T타입을 상속받는 서브 클래스 타입만 사용 가능
- [ <? super T> ] T타입과 T타입이 상속받은 슈퍼 클래스 타입만 사용 가능


```java
class Fruit {}

class Apple extends Fruit {}

class Grape extends Fruit {}

public class Bounded {
    static void makeAny(List<? extends Fruit> fruits) {
        
    }
}

```
위처럼 사용하면 fruit 클래스와 fruit 클래스를 상속받은 객체들만 받겠다고 명시한 것이다.

## 제네릭 메소드 만들기
-----
제네릭 메소드는 메소드의 선언을 할때 제네릭으로 리턴 타입과 파라미터 타입을 정의하는 메소드이다.

```java
static <T extends Fruit> void makeAny(List<T> fruits) {}
```
이전에 사용했던 makeAny를 제네릭 메서드로 변경하면 위 코드와 같다.
클래스에 선언되어 있는 타입 매개변수와 메서드에 선언된 타입 매개변수가 같은 문자를 사용하더라도 이 두 가지는 서로 다른 것이다.

메서드에 사용된 제네릭 타입은 지역 변수처럼 지역적으로 사용한다고 생각하고 공부하는 것이 좋다.

```java
public class Bounded {
    private int age;
        
    public static <T> T getAge(T age) {
        return age;
    }
}
```
- [ <T> ] : 제네릭 타입
- T getAge() : 리턴 타입
- T age : 파라미터 타입

위 코드는 제네릭 메소드로 작성되어 호출 시에 매개 타입을 지정하기 때문에 static이 가능하다.

리턴 타입 앞에 <T> 제네릭을 사용하면 되고, 제네릭 메서드 사용 시 <T>가 지역변수로 바뀐다.

```java
public class Bounded {
    public static void  print1(ArrayList<? extends Fruit> list1, ArrayList<? extends Fruit> list2) {
        
    }
    public static <T extends Fruit> void print2(ArrayList<T> list1, ArrayList<T> list2) {
        
    }
    
}
```
제네릭 메소드를 사용하지 않는다면 print1과 같이 와일드카드를 써서 타입을 제한해야한다. 하지만, 파라미터마다 와일드카드를 써야하는 단점이 있다.
그래서 print2처럼 제네릭 메소드를 사용하면 파라미터마다 반복되는 코드를 줄일 수 있다.

위 코드의 중요한 포인트는 클래스에 있는 제네릭 타입과 메소드에 있는 제네릭 타입은 서로 다른 별개라는 것을 기억해야한다.

## Erasure
-----
제네릭은 JDK 1.5버전에서 도입되었다.
따라서 하위 버전과의 호환성 유지를 위한 작업이 필요했는데 이러한 코드의 호환성 때문에 소거( Ensure ) 방식이 나타나게 되었다.

자바에서는 제네릭 클래스를 인스턴스화 할 때 해당 타입을 지워버린다.
타입 변수는 컴파일 직전까지만 존재하고 컴파일 된 바이트코드에는 존재하지 않는다.

```java
public class Bounded<T> {
    public void Method(T element) {
        System.out.println(element.toString());
    }
}
```
위 코드가 타입 소거 전 상태이다.

```java
public class Bounded {
    public void Method(Object element) {
        System.out.println(element.toString());
    }
}
```
위 코드가 타입 소거 후 상태이다.
unbounded type에 대해서 Object로 바뀌게 된 것이다.

```java
class Fruit<T> {}

public class Bounded<T extends Fruit<T>> {
    public void Method(T element) {
        System.out.println(element.toString());
    }
}
```
unbounded 와 반대로 bounded type에 대해서는 <T extends Fruit<T>>와 같이 제한된 제네릭을 걸어두면 소거된 후 아래와 같이 변경되게 된다.

```java
public class Bounded {
    public void Method(Fruit element) {
        System.out.println(element.toString());
    }
}

```

## 변성
-----
![](https://velog.velcdn.com/images/dymnam/post/9c4c1c0c-2a0d-4c84-8cdf-e354ffba44c3/image.png)

타입이 있는 언어에는 변성이란 개념이 있는데, 변성이란 List<String>과 List<Any> 와 같이 기저타입( List )가 같고, 타입 인자( String, Any )가 다른 여러 타입이 어떤 관계가 있는지 설명하는 개념이다.
예를 들어서 String은 Any의 하위 타입이다. 따라서 Any 타입의 변수에 String 객체를 할당해도 괜찮다.
그렇다면 MutableList<String> 역시 MutableList<Any>의 하위 타입인지를 정의하는 개념이다.

#### 공변
-----
MutableList<String> 역시 MutableList<Any>의 하위 타입인지를 정의하는 개념이 성립한다면 해당 언어는 공변적이라고 한다.
자바는 애초에 공변, 반공변을 지원하지 않지만, 지원하는 것처럼 보여주는 컴파일 트릭을 사용한다.
그 예제가 [ <? extedns T> ] [ <? super T> ] 키워드이다.
이러한 제네릭의 변성을 적절하게 사용하면 유연한 코드를 작성할 수 있다.

![](https://velog.velcdn.com/images/dymnam/post/864ad2d3-12e8-468a-bf75-73dff52225b1/image.png)
위 그림이 가능한 이유는 자바의 배열은 공변이라서 Number 클래스의 하위 타입들을 값으로 취할 수 있기 때문이다.
![](https://velog.velcdn.com/images/dymnam/post/6628e517-6c12-48b6-a5b8-c581493bb9f9/image.png)

## 반공변
-----
제네릭의 공변성은 읽기는 가능하지만 쓰기는 불가능하게 하여 런타임 중 에러가 발생하는 것을 막는다.
따라서 쓰기를 원하는 경우에는 ? super T로 부모객체를 파라미터로 받으면 된다.
```java
public class Covariance {
    public static void main(String[] args) {
        List<Number> myInts = new ArrayList<>();
        addNumber(myInts);

        System.out.println(myInts);
    }

    static void addNumber(List<? super Integer> num) {
        num.add(6);
        // nubers.get(0); 컴파일 에러
    }
}
```
![](https://velog.velcdn.com/images/dymnam/post/08e9127b-0921-41b4-8461-d87ac9522372/image.png)

## 무공변
-----
제네릭은 기본적으로 무공변 -> 다른 타입과 함께 변하지 않는다. 라는 뜻이다.
즉, 서로 다른 타입 Type 1과 Type 2가 있을 때, List<Type1>은 List<Type2>의 하위 타입도 아니고 상위 타입도 아니라는 뜻이다.
