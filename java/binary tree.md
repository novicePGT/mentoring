![](https://velog.velcdn.com/images/dymnam/post/ef8f99f0-bcd5-4bf3-868d-2b6909b19cf3/image.png)


## 이진트리 구조
이진트리( Binary tree )란 모든 노드들이 둘 이하의 자식을 가진 트리이다.
![](https://velog.velcdn.com/images/dymnam/post/cb347f04-f3af-4a41-9067-caa58c0c7cbb/image.png)
위 그림에서 동그라미를 노드( node )라고 부르면서, 노드는 데이터를 담고 있다.

이진트리는 컴퓨터 응용에서 가장 많이 팔용되는 중요한 트리구조이다.

이진트리의 중요한 점은 왼쪽과 오른쪽의 서브트리를 확실하게 구분한다는 것이고, 모든 노드가 정확하게 두 개의 서브트리를 가지고 있다.

- 루트 노드( root node ) : 부모가 없는 노드.( 트리는 하나의 루트 노드를 가짐 )
- 단말 노드( leaf node ) : 자식이 없는 노드.
- 내부( internal ) 노드 : 단말 노드가 아닌 노드.
- 간선( edge ) : 노드를 연결하는 선으로 link, branch라고도 함.

## 이진트리 유형
#### 전 이진트리( Full Binary Tree )
- 모든 노드가 0개 또는 2개의 자식 노드를 갖는 트리.
![](https://velog.velcdn.com/images/dymnam/post/0868dbdb-329b-452d-af14-1a582ed835dd/image.png)

#### 완전 이진트리( Complete Binary Tree )
- 마지막 레벨을 제외하고 모든 레벨이 완전히 채워져 있는 트리.
- 마지막 레벨은 꽉 차 있지 않아도 되지만 노드가 왼쪽에서 오른쪽으로 채워져야한다.
![](https://velog.velcdn.com/images/dymnam/post/f7571b96-01d5-43d0-89f4-60ec136a5dcb/image.png)
완전 이진트리를 구현할 때, 서브트리가 없다면 각 left, right의 데이터는 null이 된다.

#### 포화 이진트리( Perfect Binary Tree )
- 포화 이진 트리는 모든 내부 노드가 두 개의 자식 노드를 가지며 모든 잎 노드가 동일한 깊이 또는 레벨을 가진다.
![](https://velog.velcdn.com/images/dymnam/post/ea9b6a54-3897-4c97-8815-1c6266683b12/image.png)

#### 균형 이진트리( Balanced Binary Tree )
- 왼쪽과 오른쪽 트리의 높이 차이가 모두 1만큼 나는 트리이다.
![](https://velog.velcdn.com/images/dymnam/post/e2f09cc4-bcf9-4ba4-a6da-b6e155e8a4a0/image.png)

## 이진트리 구현하기
```java
package binaryTree;

public class Node {
    private int value;
    private Node left;
    private Node right;

    public Node() {

    }

    public Node(int value, Node left, Node right) {
        this.left = left;
        this.right = right;
        this.value = value;
    }

    public int getValue() {
        return value;
    }

    public Node getLeft() {
        return left;
    }

    public Node getRight() {
        return right;
    }
}

```
```java
package binaryTree;

import java.util.LinkedList;
import java.util.Queue;

public class BinaryTree {
    private Node root;

    public BinaryTree(Node root) {
        this.root = root;
    }

    public Node getRoot(){
        return root;
    }

    /*
     BFS : 너비 우선 탐색( Breath - First Search )
     루트 노드에서 시작해서 인접한 노드를 먼저 탐색하는 방법
     주로 두 노드 사이의 최단 경로를 찾고 싶을 때 사용하는 방법이다.
     */
    public void  printByBFS(Node root) {
        Queue<Node> q = new LinkedList<>();
        q.offer(root);

        while (!q.isEmpty()) {
            Node next = q.poll();
            System.out.println(next.getValue()+ " ");
            if (next.getLeft() != null) {
                q.offer(next.getLeft());
            }
            if (next.getRight() != null) {
                q.offer(next.getRight());
            }
        }
        System.out.println();
    }

    /*
    DFS : 깊이 우선 탐색( DFS, Depth - First Search )
    최대한 깊이 내려간 뒤, 더이상 갈 곳이 없으면 옆으로 이동
    모든 노드를 방문하고자 하는 경우에 이 방법을 사용하며, BFS보다 간단하지만 검색속도는 BFS보다 느린게 특징
     */

    public void printByDFS(Node root) {
        if (root == null) return;

        printByDFS(root.getLeft());
        System.out.println(root.getValue() + " ");
        printByDFS(root.getLeft());
    }
}

```

## 테스트 코드
```java
package binaryTree;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

@DisplayName("BinaryTree Test")
class BinaryTreeTest {
    Node node10 = new Node(10, null, null);
    Node node9 = new Node(9, null, null);
    Node node8 = new Node(8, node10, null);
    Node node7 = new Node(7, null, node9);
    Node node6 = new Node(6, node8, null);
    Node node5 = new Node(5, null, null);
    Node node4 = new Node(4, node7, null);
    Node node3 = new Node(3, node5, node6);
    Node node2 = new Node(2, node4, null);
    Node node1 = new Node(1, node2, node3);

    BinaryTree binaryTree = new BinaryTree(node1);
    Node root = binaryTree.getRoot();

    @Test
    void printByBFS() {
        System.out.println("root : " + root.getValue());
        System.out.println();
        System.out.println("==========BFS==========");
        binaryTree.printByBFS(root);
    }

    @Test
    void printByDFS() {
        System.out.println();
        System.out.println("==========DFS==========");
        binaryTree.printByDFS(root);
    }
}
```

## 결과
![](https://velog.velcdn.com/images/dymnam/post/fa39a33e-260a-46b5-a365-8b8261eeebfc/image.png)

![](https://velog.velcdn.com/images/dymnam/post/11988b7e-33ba-48ee-b646-44e3684a0369/image.png)

## 코드 참고
https://loosie.tistory.com/127?category=964815
