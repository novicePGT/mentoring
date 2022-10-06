## Enum
-----
enum이란 Enumeration으로 관련이 있는 상수들의 집합을 의미한다.
자바에서 변경할 수 없는 상수를 정의할 때 final을 사용하였다.

기존에는 인터페이스나 클래스 내에서 상수를 선언함으로써 상수를 관리하였는데, 이름이 겹치거나 불필요하게 상수가 많아진다는 단점을 가지고 있었다.
이런 단점을 보안하기 위해 나온 것이 enum이다.

```java
enum SetColor {
    RED(-7), ORANGE(-5), YELLOW(-3), GREEN(-1), BLUE(1), INDIGO(3), VIOLET(5);

    private final int value;

    SetColor(int value) {
        this.value = value;
    }

    int getValue() {
        return value;
    }
}

public class Color {
    public static void main(String[] args) {
        SetColor[] colorsArray = SetColor.values();
        for (SetColor setColor : colorsArray) {
            System.out.println(setColor);
        }
    }
}
```
![](https://velog.velcdn.com/images/dymnam/post/2dc2fb12-6768-442b-a80f-b29bf037e0a2/image.png)

## Enum의 values() & valueOf()
-----
#### values()
values() 메서드는 해당 열거체의 모든 상수를 저장한 배열을 생성하여 반환한다.
values() 메서드는 자바의 모든 열거체에 컴파일러가 자동으로 추가해주는 메서드이다.
```java
enum SetColor {
    RED, ORANGE, YELLOW, GREEN, BLUE, INDIGO, VIOLET;
}

public class Color {
    public static void main(String[] args) {
        SetColor[] colorsArray = SetColor.values();
        for (SetColor setColor : colorsArray) {
            System.out.println(setColor);
        }
    }
}

```
![](https://velog.velcdn.com/images/dymnam/post/9a444637-691a-433d-b044-a5cd8f5945cf/image.png)

#### valuesOf()
valuesOf() 메서드는 전달된 문자열과 일치하는 해당 열거체의 상수를 반환한다.
```java
enum SetColor {
    RED, ORANGE, YELLOW, GREEN, BLUE, INDIGO, VIOLET;
}

public class Color {
    public static void main(String[] args) {
        SetColor colorsArray = SetColor.valueOf("ORANGE");
        System.out.println(colorsArray);
    }
}
```
![](https://velog.velcdn.com/images/dymnam/post/d283d2db-74d4-45fa-a7ac-cb4ee5742f32/image.png)

#### ordinal()
ordianl() 메서드는 해당 열거체 정의에서 정의된 순서( 0부터 시작 )를 반환한다.
반환되는 값은 해당 열거체 상수가 정의된 순서로 상수 값이 아니다.
```java
enum SetColor {
    RED, ORANGE, YELLOW, GREEN, BLUE, INDIGO, VIOLET;
}

public class Color {
    public static void main(String[] args) {
        int index = SetColor.YELLOW.ordinal();
        System.out.println(index);
    }
}
```

![](https://velog.velcdn.com/images/dymnam/post/6d34b094-26ea-4b4f-81c9-78fdfb43904e/image.png)

## java.lang.Enum 클래스
-----
Enum 클래스는 자바 열거체의 조상 클래스로 열거체를 관리하기 위한 다양한 메서드를 포함한다.
![](https://velog.velcdn.com/images/dymnam/post/fb5552f4-eca7-46c0-b43c-bfea282f1f39/image.png)


## EnumSet
-----
EnumSet은 enum 클래스로 작동하기 위해 특화된 Set 컬렉션이다.
Set은 데이터를 중복해서 저장할 수 없으며, 순서가 보장되지 않는 특징을 가지고 있다.

- EnumSet : Set 인터페이스를 구현하고 AbstractSet을 상속받는다.

#### EnumSet의 주의사항
- 열거형 값만 포함할 수 있으며, 모든 값을 열거형이어야한다.
- null을 사용할 수 없다.
- 멀티스레드에 안전하지 않아서 필요한 경우 외부에서 동기화한다.

#### EnumSet의 장점
EnumSet의 모든 메서드는 산술 비트 연산을 사용하여 구현는 이유로 연산이 매우 빠르다.

EnumSet은 HashSet과 같은 다른 Set 구현체와 비교해서 데이터가 예상 가능한 순서로 저장되며 각 계산을 하는데 있어서 더 빠르다.

EnumSet은 적은 메모리를 사용한다.

#### 예제
```java
enum SetColor {
    RED, ORANGE, YELLOW, GREEN, BLUE, INDIGO, VIOLET;
}

public class Color {
    public static void main(String[] args) {
        EnumSet<SetColor> set = EnumSet.allOf(SetColor.class);
        set.forEach((a) -> System.out.println(a));

        System.out.println("===================");

        EnumSet<SetColor> set1 = EnumSet.noneOf(SetColor.class);
        set1.forEach((a) -> System.out.println(a));

        System.out.println("===================");

        EnumSet<SetColor> set2 = EnumSet.of(SetColor.GREEN);
        set2.forEach((a) -> System.out.println(a));

        System.out.println("===================");

        EnumSet<SetColor> set3 = EnumSet.complementOf(EnumSet.of(SetColor.INDIGO));
        set3.forEach((a) -> System.out.println(a));
    }
}

```
![](https://velog.velcdn.com/images/dymnam/post/6a860427-3f49-478d-821d-1c509d041720/image.png)


