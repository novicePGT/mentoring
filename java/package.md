## package
- 클래스를 체계적으로 관리하기 위한 도구 / 클래스들을 구분 짓는 폴더
- 패키지의 물리적인 형태는 파일 시스템의 폴더
- 클래스를 유일하게 만들어주는 식별자

#### package의 이름 규칙
- 숫자로 시작하거나, '-' 과 '$'를 제외한 특수 문자 사용 금지
- java로 시작하는 패키지 금지
- int, static 등 자바 예약어 금지
- 모두 소문자로 작성하는 것이 관례

## import
같은 패키지에 속하는 클래스는 조건 없이 다른 클래스를 사용할 수 있지만, 다른 패키지에 속하는 클래스를 사용하려면 두 가지 방법 중 하나를 택한다.

#### 패키지와 클래스를 모두 작성( FQNC : Fully Qualified Class Name )
> package1.lowpackage.LowClass3 c = new package1.lowpackage.LowClass3();

위 방법을 쓰는 경우는 아래와 같다.
- 서로 다른 패키지에 동일한 클래스 이름이 존재하고, 두 패키지가 모두 import 되어 있을 경우.


#### import문을 사용
> import package1.lowpackage.LowClass3;
LowClass3 class3 = new LowClass();

- 만약 패키지에 포함된 다수의 클래스를 사용해야 할 경우

> import package1.lowpackage.*;
위와 같이 작성하여 패키지 밑 모든 클래스를 import함을 나타낸다.

#### Built in package
java.lang : Object 패키지는 import없이 사용할 수 있다. 모든 클래스의 최상위 클래스이기 때문이다.
이러한 것들을 빌트인 패키지라고 한다.

#### static import
주로 테스트 코드를 작성하면서 assertj를 많이 봤을텐데 assertj의 임포는 static import이다.
static import는 일반적인 import와 다르게 메소드와 변수를 패키지, 클래스명 없이 접근 가능하다는 특징을 가지고있다.

## classpath
클래스패스는 말 그대로 클래스를 찾기 위한 경로를 나타내는데, jvm이나 java 컴파일러에 클래스와 패키지의 위치를 지정해주는 파라미터이다.
클래스패스를 지정해주지않으면 기본적으로 현재 디렉토리가 클래스패스로 지정된다.

#### 클래스패스의 설정법 : classpath(-cp)
컴파일러가 컴파일하기 위해서 필요로 하는 참조할 클래스 파일들을 찾기 위해서 경로를 지정하는 옵션.
> javac <"options"> <"source files">
  
jjstar.java 파일이 C:\Java 디렉토리에 존재하고 필요한 클래스 파일들이 C:\Java\jjstarClass에 위치한다면
> javac -classpath C:\Java\jjstarClass C:\Java\jjstar.java
  
만약 참조할 클래스 파일이 다른 디렉토리에도 존재한다면
> javac -classpath .; C:\Java\jjstarClass; C:\Java\ISTJ; C:\Java\jjstar.java

[;] 과 [.] 로 구분할 수 있고 [.]는 현재 디렉토리를 의미한다.

#### classpath 환경변수
환경변수는 운영체제에서 자식 프로세스들을 생성할 때 참조하는 변수로 프로세스가 컴퓨터에서 동작하는 방식에 영향을 미치는 동적인 값들을 모아둔 것이다.
JVM 기반의 애플리케이션도 이 환경변수 값을 참고하여 동작한다.

이 값을 설정하게되면 실행할때마다 -classpath(-cp)의 옵션을 사용하지 않아도 된다.

## 접근제한자
클래스의 멤버들은 객체 자신들만의 속성이자 특징이므로 대외적으로 공개하는 것은 좋지않다. 또한, 같은 모든 객체가 public이라면 혹시모를 동시성문제가 발생할 수 있으며, 어디 클래스의 무슨 객체인지 개발자 본인조차 헷갈려할 수 있다는 문제점이 생긴다.

#### 접근제한자의 종류
- public : 모든 접근을 허용
- protected : 같은 패키지에 있는 객체와 상속관계의 객체들만 허용
- default : 같은 패키지에 있는 객체들만 허용
- private : 현재 객체 내에서만 허용

#### 접근제한자의 사용
- 클래스 : public, default
- 생성자 : public, protected, default, private
- 전역변수 : public, protected, default, private
- 전역메소드 : public, protected, default, private
- 지역변수 : 접근제헌자 사용 불허

```java
class Variable {
    private int a; // 은닉화
    // 캡슐화
    public void setA(int n) {
        a = n;
    }
    public int getA() {
        return a;
    }
}

public class Child {
    public static void main(String[] args) {
        Variable v = new Variable();
        // System.out.println(v.a); 호출불가

        v.setA(100);
        System.out.println(v.getA());
    }
}
```
![](https://velog.velcdn.com/images/dymnam/post/e32da48e-6d70-4809-8907-ea41a797bb1e/image.png)

