## JUnit5
![](https://velog.velcdn.com/images/dymnam/post/a51241ba-e88a-4cd2-b593-0270582eae18/image.png)

Junit5는 자바 단위 테스트를 위한 테스트 프레임워크이다.
자바8 이상부터 사용할 수 있으며, JUnit Platform은 JUnit Jupiter, JUnit Vintage 가 결합한 형태로 볼 수 있다.

- JUnit Platform : 테스트를 실행해주는 런처와 TestEngine API를 제공함.

- Jupiter : TestEngine API 구현체로 JUnit5에서 제공함.

- Vintage : TestEngine API 구현체로 JUnit 3,4에서 제공함.

## JUnit5의 Annotation
| Annotation | 내용 
|----|:----:|
|@Test| Test Method 임을 선언.
|@ParameterizedTest| 매개변수를 받는 테스트를 작성할 수 있음.
|@RepeatedTest| 반복되는 테스트를 작성할 수 있음.
|@TestFactory| @Test로 선언된 정적 테스트가 아닌 동적으로 테스트를 사용함.
|@TestInstance| 테스트 클래스의 생명주기를 설정함.
|@TestMethodOrder| 테스트 메소드 실행 순서를 구성하는데 사용함.
|@DisplayName| 테스트 클래스 또는 메서드의 사용자 정의 이름을 선언할 때 사용함.
|@DisplayNameGeneration| 이름 생성기를 선언함. 예로 '_'는 공백처리
|@BeforeEach| 모든 테스트 실행 전에 실행할 테스트에 사용함.
|@AfterEach| 모든 테스트 실행 후에 실행한 테스트에 사용함.
|@BeforeAll| 현재 클래스를 실행하기 전 제일 먼저 실행할 테스트 작성하는데 static으로 선언함.
|@AfterAll| 현재 클래스 종료 후 해당 테스트를 실행하는데 static으로 선언함.
|@Nested| 클래스를 정적이 아닌 중첩 테스트 클래스임을 나타냄.
|@Tag| 클래스 또는 메소드 레벨에서 태그를 선언할 때 사용함.
|@Disabled| 이 클래스나 테스트를 사용하지 않음을 표시함.
|@Timeout| 테스트 실행 시간을 선언후 시간 초과되면 실패하도록 설정.
|@ExtendWith| 확장을 선언적으로 등록할 때 사용함.
|@RegisterExtension| 필드를 통해 프로그래밍 방식으로 확장을 등록할 때 사용함.
|@TempDir| 필드 주입 또는 매개변수 주입을 통해 임시 디렉토리를 제공하는데 사용함.

## JUnit 작성 패턴
given -> when -> then 패턴으로, 유닛 테스트를 3가지 단계로 나누어 처리하는 패턴이다.

- Given( 준비 ) : 어떠한 데이터가 준비되었을 때
- When( 실행 ) : 어떠한 함수를 실행하면
- Then( 검증 ) : 어떠한 결과가 나와야한다.

```java
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

import java.util.List;
import java.util.Random;
import java.util.stream.Collectors;


// 7이 입력되면 7개의 숫자를 랜덤으로 줌.
public class RandomNumberGenerator {
    public List<Integer> generate(final int num) {
        if (!isAvaildNum(num)) {
            throw new RuntimeException("7개의 숫자만 부여함.");
        }
        return generate();
    }

    private boolean isAvaildNum(final int num) {
        return num == 7;
    }

    private List<Integer> generate() {
        return new Random()
                .ints(1, 99 + 1)
                .distinct()
                .limit(7)
                .boxed()
                .collect(Collectors.toList());
    }
```
위 코드의 랜덤으로 생성된 번호의 개수에 대해서 테스트 코드를 작성해보려고 하면 아래와 같은 코드를 작성할 수 있다.
```java
@DisplayName("랜덤 번호 개수 테스트")
    @Test
    void RandomNumberSizeTest() {
        // given
        final RandomNumberGenerator randomNumberGenerator = new RandomNumberGenerator();
        final int num = 7;

        // when
        final List<Integer> randomNumber = randomNumberGenerator.generate(num);

        // then
        assertEquals(randomNumber.size(), 6);
    }
```
만약, 개수가 7개가 아니라면 아래와 같은 그림을 볼 수 있다.
![](https://velog.velcdn.com/images/dymnam/post/50764344-27e5-4951-8f00-9464e2846d4c/image.png)

#### 랜덤 번호 범위 테스트
```java
@DisplayName("랜덤 번호 범위 테스트")
    @Test
    void RandomNumberRangeTest() {
        // given
        final RandomNumberGenerator randomNumberGenerator = new RandomNumberGenerator();
        final int num = 7;

        // when
        final List<Integer> randomNumber = randomNumberGenerator.generate(num);

        // then
        assertEquals(true, randomNumber.stream().allMatch(v -> v >= 1 && v <= 99));
    }
```
위의 범위 안에서 정상적으로 돌아갔다면 아래와 같은 그림을 볼 수 있을 것이다.
![](https://velog.velcdn.com/images/dymnam/post/5bdc1996-6a26-45d9-b175-67169453d625/image.png)
