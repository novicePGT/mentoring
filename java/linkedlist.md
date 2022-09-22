## LINKEDLIST
![](https://velog.velcdn.com/images/dymnam/post/2e6523ed-3be0-45df-8444-92b7e344df0f/image.jpeg)

LinkedList -> 노드들이 링크로 연결된 형태의 자료구조, 데이터 영역에 데이터를 저장, 포인트( 링크 )영역에 다음/이전 노드로 가는 주소를 저장하여 순서를 나타냄.

- 삽입과 삭제 연산 비용 적음 : 다른 데이터의 이동없이 리스트 중간에 삽입 가능하다.

- 길이제한 없음 : 노드를 연결한 형태이기에 길이제한이 없다.

- 메모리 효율 낮음 : 노드를 연결하기 위한 포인터 저장 영역이 필요하기 때문에 메모리 효율이 배열 >= LinkedList

- 랜덤 엑세스 불가능 : 연속된 메모리 주소에 데이터가 저장된 것이 아니어서, 배열처럼 랜덤 액세스가 불가능하다.

![](https://velog.velcdn.com/images/dymnam/post/f8da9a4c-0283-4276-b438-5b74ee4473c0/image.png)

노드 : 5 와 같이 하나의 데이터를 노드라고 하며, 데이터와 다음 데이터로의 주소가 묶여있는 단위를 의미한다.

- 각 노드는 하나의 데이터 필드와 하나의 링크( next )필드로 구성된다.
- 첫 번째 노드의 주소는 따로 저장( head )해야 한다.
- 마지막 노드의 next 필드에는 NULL을 저장하여 연결리스트의 끝을 나타내줘야한다.

#### 이중 LinkedList
이중 LinkedList는 단일 연결과 비슷하지만, 포인터( 주소 ) 공간이 2개이고 각 포인터는 앞 노드와 뒤 노드를 가르킨다.

- 앞 노드와 뒷 노드 모두 순회할 수 있어서 추가와 삭제 연산에 있어서 더 효율 적인 성능을 낼 수 있다.
- 2개의 포인터를 사용하기 때문에 추가적인 공간이 필요하다.

**LinkedList는 Serializable, Cloneable, Iterable, Collections, Deque, List, Queue 와 같은 다양한 인터페이스를 구현하고 있으며, 특히 List와 Deque 인터페이스의 이중연결리스트 구현체이다.**

## LinkedList 구현하기
```java
package linkedList;

public class LinkedList {
    int data; // 데이터 필드
    LinkedList next; // 링크 필드

    public LinkedList() {

    }

    // 정수를 저장하는 ListNode 생성
    public LinkedList(int data) {
        this.data = data;
        this.next = null;
    }

    // 데이터 추가 메서드 구현
    public LinkedList add(LinkedList head, LinkedList nodeToAdd, int position) {
        LinkedList node = head;

        // 맨 앞에 삽입하는 경우
        if (position == 0) {
            // 삽입 노드가 첫 번째 노드인 경우
            if (head == null) return nodeToAdd;

            LinkedList insertNode = nodeToAdd;
            insertNode.next = head;
            head = insertNode;
            return head;
        }
        // position 바로 앞까지 순차적 탐색
        for (int i = 0; i < position - 1; i++) {
            node = node.next;
        }

        nodeToAdd.next = node.next;
        node.next = nodeToAdd;
        return head;
    }

    // 데이터 삭제 메서드 구현
    public LinkedList remove(LinkedList head, int positionToRemove) {
        LinkedList node = head;

        // 삭제 노드가 최상단 일 경우
        if (positionToRemove == 0) {
            // 1번 인덱스 노드를 최상단으로 지정
            head = head.next;
        } else {
            // 순차적 탐색
            for (int i=0; i<positionToRemove -1; i++) {
                node = node.next;
            }

            LinkedList removeNode = node.next;
            node.next = removeNode.next;
        }
        return head;
    }

    // 데이터의 포함 여부
    public boolean contains(LinkedList head, LinkedList nodeToCheck) {
        while (head != null) {
            // 찾는 데이터가 head이면, return true;
            if (head.data == nodeToCheck.data) return true;

            // checkNode를 찾을때까지 순차적 탐색
            head = head.next;
        }
        // 탐색 후 없으면 retrun false;
        return false;
    }

    @Override
    public String toString() {
        return "data : " + data + ", next = " + next;
    }
}

```

## 테스트 코드
```java
package linkedList;

import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.TestInstance;

@TestInstance(TestInstance.Lifecycle.PER_CLASS)
@DisplayName("LinkedList Test")
class LinkedListTest {
    LinkedList node = new LinkedList(1);
    LinkedList head = node;

    @BeforeAll
    void init() {
        System.out.println("==============순차적 삽입==============");
        for (int i=1; i<6; i++) {
            head = node.add(head, new LinkedList(i+1), i);
            System.out.println(head.toString());
        }
        System.out.println();
    }

    @DisplayName("Add Test")
    @Test
    void add() {
        System.out.println("==============비순차적 삽입==============");
        head = node.add(head, new LinkedList(25), 2);
        head = node.add(head, new LinkedList(35), 4);
        System.out.println(head.toString());
        System.out.println();
    }

    @DisplayName("Remove Test")
    @Test
    void remove() {
        System.out.println("==============노드 삭제==============");
        head = node.remove(head, 0); // head 삭제
        System.out.println(head.toString());

        head = node.remove(head, 2); // head 삭제 후, 3번째 노드 삭제
        System.out.println(head.toString());
    }

    @DisplayName("Contains Test")
    @Test
    void contains() {
        System.out.println("==============노드 포함 테스트==============");
        System.out.println("1 포함여부 : " + node.contains(head, new LinkedList(2)));
        System.out.println("10 포함여부 : " + node.contains(head, new LinkedList(10)));

    }
}
```

## 결과
![](https://velog.velcdn.com/images/dymnam/post/5196045a-b6ed-460f-adcb-9746597b4276/image.png)

## 참고
https://loosie.tistory.com/122
