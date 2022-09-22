## Queue
- 사전적 의미로 "줄을 서다"를 뜻하며, "대기열"이라고 말하기도 한다.

줄을 서서 대기하는 것처럼 먼저 들어오면 데이터가 먼저 나가는 형식인 **FIFO( First In First Out ) 방식**이다.

- Queue의 FIFO 방식의 반대는 Stack의 Last In First Out 방식이다.
- 먼저 들어온 데이터가 먼저 나가는 동작을 의미한다.
![](https://velog.velcdn.com/images/dymnam/post/13b8de51-198a-4239-bd54-00264a9816eb/image.png)

그림을 참고하면 큐의 front는 삭제 연산만 수행하고, 큐의 rear는 삽입 연산만 수행한다.

#### Queue의 주요 용어
- enqueue : Queue에 데이터 추가하기( 메서드 : offer, add )
- dequeue : Queue에서 데이터 추출하기( 메서드 : poll, remove )
- peek : Queue에서 데이터 확인하기( 메서드 : peek, element )

## Queue 예제
```java
public ExQueue() {
        // 데이터 삽입
        q.offer(1); // 1출력
        q.offer(2); // 2출력
        q.offer(3); // 3출력
        q.offer(4); // 4출력
        System.out.println(q);

        // 데이터 출력
        q.peek(); // 저장된 대기열 출력
        System.out.println(q);

        q.poll(); // 1 출력 후 드롭
        q.poll(); // 2 출력 후 드롭
        System.out.println(q);

        // 데이터 삭제
        q.remove(); // 3 드롭
        System.out.println(q);

        q.clear(); // 대기열 비우기
        System.out.println(q);
    }
```
![](https://velog.velcdn.com/images/dymnam/post/cf2a05d3-f022-4860-9223-119f31e4d758/image.png)

## Array로 Queue 구현하기
```java
package queue;

public class ArrayQueue {
    private int[] elements;
    private int top = 0; // Dequeue 쪽이 front, 제거 연산만 처리
    private int tail = 0; // enqueue 쪽이 rear, 삽입 연산만 처리
    private int size = 2; // 기본 사이즈는 2
    private int exSize = 0; // 사이즈 확장을 위한 변수

    public ArrayQueue() {
        elements = new int[size]; // 요소의 배열의 크기를 정함
    }

    public void offer(int data) {
        if (tail == elements.length) {
            System.out.println("[ 사이즈 확장 ] Size : " + size + " -> " + (tail + 5));
            exSize = tail + 5; // 사이즈 확장을 위함
            int[] extendsElements = new int[exSize];

            // 데이터 옮기기
            for (int i=0; i<tail; i++) {
                extendsElements[i] = elements[i];
            }
            elements = extendsElements;
        }

        elements[tail++] = data; // 데이터 추가 연산은 rear에서 하니까 tail++
        System.out.println("데이터 추가 : " + data + " -> top : " + top + ", tail : " + tail);
    }

    public int printTop() {
        return elements[top];
    }

    public int getSize() {
        return elements.length;
    }

    // 데이터 출력 후 삭제
    public int poll() {
        if (top == tail) {
            System.out.println("저장된 데이터가 없습니다.");
            return -1;
        }
        int data = elements[top];
        elements[top++] = 0; // 데이터의 제거 연산은 front에서 하니까 top++
        return data;
    }

    public void remove() {
        if (top == tail) {
            System.out.println("저장된 데이터가 없습니다.");
        }
        System.out.println("데이터 삭제 : " + elements[top]);
        elements[top] = 0;
        top++;
    }

    // 모든 데이터 삭제
    public void clear() {
        elements = new int[size];
        top = 0;
        tail = 0;
        System.out.println("모든 데이터 삭제 완료");
    }


}
```

## 테스트 코드
```java
package queue;

import org.junit.jupiter.api.*;

@TestInstance(TestInstance.Lifecycle.PER_CLASS)
@DisplayName("ArrayQueue Test")
class ArrayQueueTest {
    ArrayQueue arrayQueue = new ArrayQueue();

    @BeforeAll
    void init() {
        arrayQueue.offer(1);
        arrayQueue.offer(2);
    }

    @DisplayName("OFFER TEST")
    @Test
    void offer() {
        System.out.println("================데이터 삽입================");
        arrayQueue.offer(3);
        arrayQueue.offer(4);
        arrayQueue.offer(5);
        System.out.println();
    }

    @DisplayName("POLL TEST")
    @Test
    void poll() {
        System.out.println("================데이터 출력 후 삭제================");
        System.out.println("데이터 출력 후 삭제 : " + arrayQueue.poll());
        System.out.println();
    }

    @DisplayName("REMOVE TEST")
    @Test
    void remove() {
        System.out.println("================데이터 삭제================");
        arrayQueue.remove();
        System.out.println("데이터 출력 : " + arrayQueue.printTop());
        System.out.println();
    }

    @DisplayName("CLEAR TEST")
    @Test
    void clear() {
        System.out.println("================전체 데이터 삭제================");
        arrayQueue.clear();
        System.out.println(arrayQueue.poll());
        System.out.println();
    }
}
```

## 결과
![](https://velog.velcdn.com/images/dymnam/post/5c5c0708-a7f9-47b5-8896-4317cd1b9a2f/image.png)
