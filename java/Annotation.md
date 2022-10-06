## 어노테이션
-----
어노테이션은 우리가 종종 헷갈리지 않게 적어두는 것과 같이 주석이라는 의미를 가진다.
자바에서 어노테이션은 코드 사이에 주석처럼 쓰여 특별한 의미, 기능을 갖도록 하는 기술이다.
결과적으로 프로그램에 추가적인 정보를 제공해주는 메타데이터로 볼 수 있다.

- 컴파일 타임에 코드 작성 문법 에러를 체크하도록 정보를 제공한다.
- 런타임시에 특정 기능을 실행하도록 정보를 제공한다.
- 코드를 자동 생성할 수 있도록 정보를 제공한다.

## 어노테이션 정의
-----
```java
package org.example;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface Annotation {
    String value() default "-";
    int num() default 3;
}

```
어노테이션은 위처럼 사용할 수 있으며, 위에 적어둔 것처럼 사용하는 위치, 목적, 특징 등 사용 이유에 따른 많은 어노테이션이 존재한다.

## @Target
-----
@Target은 인터페이스에서 사용할 수 있으며, 어떠한 값에 어노테이션을 적용할 것인지 결정한다.

#### ElementType에서 사용할 수 있는 값
- TYPE : 클래스, 인터페이스, Enum
- ANNOTATION_TYPE : 어노테이션
- FIELD : 필드
- CONSTRUCTOR : 생성자
- METHOD : 메소드
- LOCAL_VARIABLE : 로컬 변수
- PACKAGE : 패키지

## @Retention
-----
@Retention 에서는 @Target 에서 정한 값을 언제까지 유지할 것인지를 결정한다.
보통은 런타임 시에 많이 사용하여 RetentionPolicy.RUNTIME을 자주 사용한다고 한다.

#### during @Retention
- SOURCE : 소스상에서만 어노테이션 정보를 유지하고, 바이트코드는 정보가 남지 않아 소스코드를 분석할 때만 의미가 있다.
- CLASS : 바이트코드 파일까지 어노테이션 정보를 유지한다.
- RUNTIME : 런타임까지 어노테이션 정보를 유지한다.

#### 예제
```java
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface Annotation {
    String value() default "아아아 집가고싶다 ~ ";
    int num() default 3;
}

public class UseAnnotation {

    @Annotation
    public void print() {
        System.out.println("UseAnnotation.print");
    }
    @Annotation
    public void print2() {
        System.out.println("UseAnnotation.print2");
    }
}

public class TestMain {
    public static void main(String[] args) {
        Method[] method = UseAnnotation.class.getMethods();

        for (Method m : method) {
            if (m.isAnnotationPresent(Annotation.class)) {
                System.out.println(m.getName());
                Annotation annotation = m.getDeclaredAnnotation(Annotation.class);

                String value = annotation.value();
                int num = annotation.num();
                System.out.print("value = " + value);
                System.out.println("num = " + num);
            }
        }
    }
}

```
![](https://velog.velcdn.com/images/dymnam/post/aac94bbc-d94a-495f-b2d1-a5742ff5c183/image.png)

## @Documented
-----
@Documented는 메타데이터 어노테이션으로, 이 어노테이션을 사용하는 클래스가 **java doc** 과 같은 문서화 될 때 해당 어노테이션이 적용되었음을 명시하도록 한다.

## 어노테이션 프로세서
-----
어노테이션 프로세서는 컴파일 시점에 끼어들어 특정한 어노테이션이 붙어있는 소스코드를 참조해서 새로운 코드를 만들 수 있는 기능으로, @Override & Lombok이 그 예제가 될 수 있다.

#### 어노테이션 프로세싱 과정
1. 어노테이션 클래스 생성
2. 어노테이션 파서 클래스 생성
3. 어노테이션 사용
4. 컴파일하면 어노테이션 파서가 어노테이션을 처리
5. 자동 생성된 클래스가 빌드 폴더에 추가

Annotation Paser classes는 컴파일 시에 필요로하며, 빌드 종료시 끝나게 된다.

#### **추가**
-----
### @Override
-----
- 선언한 메서드가 오버라이드 되었다는 것을 나타낸다.
- 만약 상위 클래스에서 해당 메서드를 찾을 수 없다면 컴파일 에러를 발생시킨다.

### @Deprecated
-----
- 해당 메서드가 더 이상 사용되지 않음을 표시한다.
- 만약 사용할 경우 컴파일 경고를 발생시킨다.

### @SuppressWarnings
-----
- 선언한 곳의 컴파일 경고를 무시한다.

### @SafeVarargs
-----
- 제너릭 같은 가변인자의 매개변수를 사용할 때, 경고를 무시한다.

### @FunctionalInterface
-----
- 함수형 인터페이스를 지정하는 어노테이션이다.
- 메서드가 존재하지 않거나, 1개 이상의 메서드가 존재한다면 컴파일 오류가 발생한다.

### @Inherited
------
- 어노테이션의 상속을 가능하게 한다.
