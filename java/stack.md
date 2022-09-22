## Stack
![](https://velog.velcdn.com/images/dymnam/post/708f22a7-bd42-40be-a3f4-1652e0255d98/image.png)


- 스택 : 상자에 물건을 쌓아 올리듯 데이터를 쌓는 자료구조.
-> 삽입과 삭제를 리스트의 한쪽( Top )에서만 한다.

#### LIFO( Last In First Out )
스택의 가장 큰 특징은 LIFO 구조를 갖는다는 것이다.
LIFO 구조는 나중에 들어온 데이터가 먼저 나가는 구조를 의미한다.

스택에서는 자료를 넣는 것을 push, 자료를 빼는 것을 pop이라고 한다.

## 스택 구현하기
```java
package stack;

public class Stack {
    private int[] elements;
    private int top = 0; 
    private int size = 5;

    // 스택 생성, 기본 배열 사이즈는 5로 설정
    public Stack() {
        elements = new int[size];
    }

    // 실질적 스택의 구현
    public Stack(int data) {
        elements = new int[size];
        System.out.println("데이터 추가 : " + data);
        elements[top++] = data; // 요소의 배열에 위로 쌓기위한 top++임
    }

    /*
     스택 push 하기 위한 메서드, 스택은 PUSH, PEEK, PUSH 의 기능을 기본으로 함
     */
    public void add(int data) {
        // 용량이 초과하면 크기 확장하고 기존 데이터 옮기기
        if (top == elements.length) {
            System.out.println("[ 사이즈 확장 ] Size : " + size + " -> " + (top+5));
            size = top + 5; // 사이즈 확장
            int[] extendElements = new int[size]; // 확장하고 데이터를 옮기기위함.

            // 데이터 옮기기 위한 반복문
            for(int i=0; i<top; i++) {
                extendElements[i] = elements[i];
            }
            elements = extendElements;
        }
        System.out.println("데이터 추가 : " + data);
        elements[top++] = data;
    }

    // 스택 pop하기 위한 메서드
    public int delete() {
        if (top == 0) {
            System.out.println("데이터 size == 0 입니다.");
            return -1;
        } else {
            int num = elements[--top]; // 스택의 LIFO방식
            elements[top] = 0;
            return num;
        }
    }

    public void print() {
        for (int i=0; i<top; i++) {
            System.out.println(elements[i]+ "  ");
        }
        System.out.print(" < -- top");
        System.out.println();
    }
}

```

## 테스트 코드
```java
package stack;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

@DisplayName("Stack Test")
class StackTest {
    Stack stack = new Stack();

    @DisplayName("Stack Add Test")
    @Test
    void add() {
        stack.add(10);
        stack.add(20);
        stack.add(30);
        stack.add(40);
        stack.print();
    }

    @DisplayName("delete Test")
    @Test
    void delete() {
        System.out.println(stack.delete());
        System.out.println(stack.delete());
        System.out.println(stack.delete());
        System.out.println(stack.delete());
    }

    @DisplayName("print Test")
    @Test
    void print() {
        System.out.println("===========================");
        Stack stack1 = new Stack(50); // 1 번
        stack1.add(2);
        stack1.add(3);
        stack1.add(4);
        stack1.print();

        System.out.println(stack1.delete());
        System.out.println(stack1.delete());
        System.out.println(stack1.delete());
        System.out.println(stack1.delete());
    }
}
```

## 결과
![](https://velog.velcdn.com/images/dymnam/post/0f09f990-6701-4d88-90e8-a5496a6c8086/image.png)
