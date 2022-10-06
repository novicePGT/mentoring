## Thread
![](https://velog.velcdn.com/images/dymnam/post/d3d55b0f-6fba-468d-93cd-d0241b5e6f6e/image.png)
Thread는 하나의 프로세스 내부에서 독립적으로 실행되는 하나의 작업 단위를 말한다.

- JVM에 의해 하나의 프로세스가 발생하고 main() 안의 실행문들이 하나의 스레드이다.
- main() 이외의 또 다른 스레드를 만들려면 Thread 클래스를 상속하거나 Runnable 인터페이스를 구현한다.
- 다중 스레드 작업 시에는 각 스레드끼리 정보를 주고받을 수 있어 처리 과정의 오류를 줄일 수 있다.
- 프로세스끼리는 정보를 주고 받을 수 없다.

#### process
실행중인 프로그램을 뜻하며, 작업 관리자에 프로세스 탭에 있는 것들이 이 부분에 해당한다.

#### code
프로세스가 실행할 코드와 매크로 상수가 기계어의 형태로 저장된 공간.
-> Read - only

#### data
코드에서 선언한 전역변수 또는 static 변수 등이 저장된 공간.

#### heap
heap은 런타임에 결정되고, malloc, calloc으로 heap 영역의 메모리를 사용할 수 있다.
데이터 배열의 크기가 확실하지 않고 변동이 있을 때 heap 영역을 활용해서 메모리를 할당한다.
사용하고 난 뒤에는 반드시 해제해야하며, 그렇지 않을 시 memory leak이 발생한다.

- 스택보다 할당할 수 있는 메모리 공간이 많다는 것이 장점이지만 포인터로 메모리 영역에 접근해야 하기 때문에 다른 자료구조에 비해 데이터를 읽고 쓰는게 느리다.

#### thread
프로세스 내에서 실행되고 있는 흐름의 단위이다.

#### stack
프로세스의 메모리 공간을 관리하기 위한 알고리즘 중 하나로, 함수 안에서 선언된 지역변수, 매개변수, 리턴 값 등이 저장되고 함수 호출 시 기록하고, 종료되면 제거한다. 
-> LIFO 방식을 따른다.
- 컴파일 타임에 크기가 결정되어 무한히 할당할 수 없다. 재귀함수가 너무 깊게 호출되거나 지역변수를 너무 많이 가지고 있어 stack 영역을 초과하면 stack overflow 에러가 발생한다.

#### multi - tasking
여러 개의 프로세스를 동시에 실행하는 것을 의미한다. 
-> 여러 개의 프로그램을 동시에 사용할 수 있는 이유

## Multi - Thread
하나의 프로세스 안에 여러 개의 스레드가 존재하는 것.

#### 장점
- CPU의 사용률을 향상시킨다.
- 자원을 보다 효율적으로 사용할 수 있다.
- 사용자에 대한 응답성이 향상된다.
- 작업이 분리되어 코드가 간결해진다.

#### 단점
여러 개의 쓰레드가 같은 프로세스 내에서 자원을 공유하면서 작업하기 때문에 동기화( Synchronization ), 교착상태( deadlock )와 같은 문제가 발생할 확률이 높다.

## Thread 생성하기
스레드의 생성 방법은 Thread 클래스를 상속받아서 사용하는 방법과 Runnable 인터페이스를 구현하는 방법이다.

#### Thread 클래스 상속받기
```java
class BookStore extends Thread {
    // 이 클래스에서 상위 클래스인 Thread의 run() 메소드를 재정의 해야 Thread의 실행부분을 작성할 수 있다.
    @Override
    public void run() {
        super.run();
        System.out.println("Book A");
    }
}


public class Book {
    public static void main(String[] args) {
        BookStore bs = new BookStore();
        bs.start();
    }
}
```
![](https://velog.velcdn.com/images/dymnam/post/9ef8f542-9450-44e6-b8e0-7ed267b3d6f1/image.png)

#### Runnable 인터페이스 구현하기
```java
// Runnable 인터페이스 상속받아 구현
class BookStore implements Runnable {

    @Override
    public void run() {
        System.out.println("booooooooock");
    }
}

public class Book {
    public static void main(String[] args) {
       /*
       Runnable 인터페이스를 구현해 Thread를 만들 경우
       객체 생성을 Thread 클래스 타입으로 만들어 줘야 한다.
       run() 메소드의 트리거 역할을 하는 start()는 Thread 클래스에
       정의되어있어서 객체 생성 시 반드시 Thread 타입으로 만들어야한다.
        */
        BookStore bookStore = new BookStore();
        bookStore.run();

        Thread bs = new Thread(new BookStore());
        bs.start();
    }
}
```
![](https://velog.velcdn.com/images/dymnam/post/4ce80056-a239-408e-81f4-3fc1a7bb3e1f/image.png)

- start() : 새로운 스레드가 작업을 실행하는데 필요한 호출 스택을 생성하는 것.
- run() : start()로 생성된 호출스택에 run()가 첫 번째로 저장되는 과정

## Thread State
![](https://velog.velcdn.com/images/dymnam/post/7e8b867b-b8bc-40c6-8c75-22b849a1f589/image.png)

#### NEW
- 스레드가 아직 시작하지 않은 상태
```java
class BookStore implements Runnable {

    @Override
    public void run() {
        System.out.println("booooooooock");
    }
}

public class Book {
    public static void main(String[] args) {
        Thread bs = new Thread(new BookStore());
        System.out.println(bs.getState());
        bs.start();
    }
}
```
![](https://velog.velcdn.com/images/dymnam/post/f144b7cd-7e5f-4b69-a41a-8f34bc62b611/image.png)
출력된 NEW : 호출 스택을 아직 생성하지 않아서 NEW의 결과 출력

#### RUNNABLE
- 스레드가 JVM에서 실행 대기 중이거나 실행중인 상태
```java
class BookStore implements Runnable {

    @Override
    public void run() {
        System.out.println("booooooooock");
    }
}

public class Book {
    public static void main(String[] args) {
        Thread bs = new Thread(new BookStore());
        System.out.println(bs.getState());
        bs.start();
        System.out.println(bs.getState());
    }
}
```
![](https://velog.velcdn.com/images/dymnam/post/ea07129e-a47c-4d52-9aab-cb984cdea9e9/image.png)
출력된 RUNNABLE : start()를 호출한다고해서 바로 실행되는 것이 아니라 실행 대기열에 저장된 후 실행된다. 이 때 실행 대기중 상태이기때문에 RUNNABLE이 먼저 출력된다.

#### BLOCKED
- 하나의 스레드가 동기화영역에 들어가면 해당 스레드가 작업이 종료될 때까지 동기화 블럭에 접근할 수 없는 상태를 의미한다.
-> 동기화 블럭에 의해서 일시정지된 상태
```java
package org.example;

class BookStore implements Runnable {

    @Override
    public void run() {
        exhibitBook();
    }

    public synchronized void exhibitBook() {
        while (true) {
            /*
            책을 전시하고 있다는 가정을 두고
            만약 새로운 책을 전시한다면 먼저 전시하던 책을 끝낼 때까지
            새로운 책을 전시할 수 없다.
             */
        }
    }
}

public class Book {
    public static void main(String[] args) throws InterruptedException {
        Thread bs = new Thread(new BookStore());
        Thread newBs = new Thread(new BookStore());

        bs.start();
        newBs.start();

        Thread.sleep(1000);

        System.out.println(bs.getState()); // Runnable
        System.out.println(newBs.getState()); // Blocked
        System.exit(0);
    }
}
```

#### WAITING
- 다른 스레드가 특정 작업을 수행하는 중 기존에 작업중이던 쓰레드가 잠시 멈추는 것을 의미한다.

```java
class BookStore implements Runnable {

    @Override
    public void run() {
        try {
            Thread.sleep(3000);
            System.out.println("책이 많이 입고되었을 때");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        /*
        입고에 대해 먼저 처리한다.
        요청사항이 끝날 때까지 기존에 전시하던 책은 잠시 멈춘 상태로 기다린다.
         */
        System.out.println("전시하던 책은 잠시 : " + WaitingState.exhibit.getState());
    }
}

public class WaitingState implements Runnable {
    public static Thread exhibit;

    public static void main(String[] args) {
        exhibit = new Thread(new WaitingState());
        exhibit.start(); // 책을 전시해두기 시작한다.
    }

    @Override
    public void run() {
        Thread bookStore = new Thread(new BookStore()); // 전시 도중 책이 입고되었다.
        bookStore.start(); // 입고된 책을 정리하기 시작한다.

        try {
            bookStore.join();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            e.printStackTrace();
        }
    }
}

```
![](https://velog.velcdn.com/images/dymnam/post/9cf5556b-f344-4145-961b-072ed0041695/image.png)

#### TIMED_WAITING
```java
class FindBook implements Runnable {

    @Override
    public void run() {
        try {
            Thread.sleep(5000);
            System.out.println("찾으시는 책 가져왔습니다.");
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            e.printStackTrace();
        }
    }
}

public class TimeWaitingState {
    public static void main(String[] args) throws InterruptedException {
        Thread findBook = new Thread(new FindBook());
        findBook.start();

        /*
        책을 가지러가는 시간이 5초라고 하면 책을 찾는 손님은
        "찾으시는 책 가져왔습니다" 라는 말을 기다리는 경우이다.
         */
        Thread.sleep(1000);
        System.out.println(findBook.getState());
    }
}

```
![](https://velog.velcdn.com/images/dymnam/post/65cfed6e-ec26-48f0-ad93-ce89742bc5f3/image.png)

#### TERMINATED
```java
public class TerminateState implements Runnable{
    public static void main(String[] args) throws InterruptedException {
        Thread thread = new Thread(new TerminateState());
        thread.start();

        thread.sleep(1000);
        System.out.println(thread.getState());
    }

    @Override
    public void run() {

    }
}

```
![](https://velog.velcdn.com/images/dymnam/post/c2a32625-771a-4c05-968d-564bc9cbc6e2/image.png)

## Thread Priority
어떠한 작업을 다른 작업보다 먼저 처리되어야하는 경우가 존재할 것이다.
그럴때 우선순위를 설정하여 중요한 작업부터 실행될 수 있게끔 할 수 있다.

#### 우선순위 설정
- 우선순위는 1~10까지 존재하며 설정하지 않은 경우 default 값으로 5이다.
- setPriority 메소드를 이용하여 우선순위를 바꿀 수 있다.
- 하지만 설정한다고 한들, 반드시 우선순위대로 작업한다는 보장은 없다.
```java
class BookType implements Runnable {

    @Override
    public void run() {
        exhibitBook(Thread.currentThread().getName());
    }

    private void exhibitBook(String name) {
        System.out.println(name + "가 전시되고 있습니다.");
    }
}

public class BookPriority {
    public static void main(String[] args) {
        try {
            Thread history = new Thread(new BookType(), "history");
            Thread science = new Thread(new BookType(), "science");
            Thread math = new Thread(new BookType(), "math");

            /*
            우선순위는 1~10까지 존재하며 설정하지 않은 경우에 기본은 5이다.
            setPriority 메소드를 이용하여 우선순위를 바꿀 수 있다.
             */
            history.setPriority(1);
            science.setPriority(4);
            math.setPriority(7);

            history.start();
            science.start();
            math.start();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```
![](https://velog.velcdn.com/images/dymnam/post/cc821073-9be8-49d0-97e0-4ba3a7b1b7fa/image.png)

## Main Thread
자바에서 메인 메소드를 통해 프로그램이 실행되면 하나의 스레드가 시작되는데, 이것을 메인 스레드라고 한다.

우리가 만든 프로그램을 실행하면 JVM에서 자동으로 메인 스레드를 생성해준다.

- currentThread()를 호출하면 해당 스레드의 참조값을 가져올 수 있다.
- getName()을 호출하면 currentThread()로 가져온 현 Thread의 Name 값을 알 수 있다.
```java
public class MainThread {
    public static void main(String[] args) {
        Thread thread = Thread.currentThread();
        System.out.println(thread.getName()); // main
    }
}
```

## 동기화
멀티스레드 프로그래밍의 특징은 하나의 프로세스에 여러 스레드가 접근한다는 의미이다.
하나의 데이터를 공유하여 작업해 더욱 빠른 동작을 할 수 있다.
반면에, 하나의 작업에 여러 스레드가 동시에 접근하여 똑같은 작업을 한다면 더 좋지 않을 수 있다.
그것을 방지하기 위해 동기화라는 개념이 들어왔다.

- 멀티스레드의 동기화 : 특정 스레드가 진행중일 때, 다른 스레드가 접근하지 못하도록 막는 것을 의미한다.

#### 특정한 객체에 lock을 걸 때
```java
sychronized( 객체의 참조 변수 ) {
/*
참조 변수로 들어온 객체는 synchronized 블록에 들어오면 lock에 잠기고, 블록이 종료 되면 lock이 해제된다.
잠긴 사이에는 다른 스레드가 접근할 수 없다.
*/
}
```

#### 메소드에 lock을 걸 때
```java
public synchronized void exam() {
/* 한 스레드가 sychronized 메소드를 호출해서 수행하고 있으면
이 메소드가 종료될 때까지 다른 스레드는
이 메소드를 호출할 수 없다
*/
}
```

#### 예제
```java![](https://velog.velcdn.com/images/dymnam/post/f5712b1b-97f5-4189-af30-177d66c212f5/image.png)

public class BookStore {
    public static void main(String[] args) {
        try {
            ExhibitBook exhibit = new ExhibitBook(10);
            new Thread(exhibit, "반지의 제왕").start();
            new Thread(exhibit, "호빗").start();
            new Thread(exhibit, "하얼빈").start();
            new Thread(exhibit, "행복해라").start();
            new Thread(exhibit, "잘될 수밖에 없는 너에게").start();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

class ExhibitBook implements Runnable {

    private int bookCount;
    private String[] bookshelf = {"_", "_", "_", "_"};

    public ExhibitBook(int count) {
        bookCount = count;
    }

    @Override
    public void run() {
        while (bookCount > 0) {
            synchronized (this) { // 한번에 하나의 스레드만 접근
                bookCount--;
                System.out.println("전시중인 책의 개수 : " + bookCount);
            }

            for (int i = 0; i < bookshelf.length; i++) {
                if (!bookshelf[i].equals("_")) {
                    continue;
                }

                synchronized (this) {
                    bookshelf[i] = Thread.currentThread().getName();
                    System.out.println(bookshelf[i] + "전시하는 중입니다.");
                }

                // 책을 전시하는 데 걸리는 시간은 5초로 가정
                try {
                    Thread.sleep(5000);
                } catch (Exception e) {
                    e.printStackTrace();
                }

                synchronized (this) {
                    System.out.println(Thread.currentThread().getName() + " 전시를 마쳤습니다");
                    System.out.println("==============================");
                    bookshelf[i] = "_";
                }
                break;
            }

            // 새로운 책 전시를 위한 준비시간
            try {
                Thread.sleep(Math.round(1000 * Math.random()));
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}

```
![](https://velog.velcdn.com/images/dymnam/post/d3020003-310f-42be-8536-c0f51477e8c6/image.png)

![](https://velog.velcdn.com/images/dymnam/post/dd6ae7fa-66ed-4a0f-adfd-bc8bb8c0021b/image.png)

## 교착상태( dead - lock )
둘 이상의 스레드가 lock을 획득하기 위해서 기다리는데, lock을 가진 스레드도 lock을 얻기위해 대기하며 모든 스레드가 블록 상태에 놓이는 것을 의미한다.

```java
public class TestThread {
    public static Object Lock1 = new Object();
    public static Object Lock2 = new Object();

    public static void main(String args[]) {
        Thread1 t1 = new Thread1();
        Thread2 t2 = new Thread2();
        t1.start();
        t2.start();
    }

    private static class Thread1 extends Thread {
        public void run() {
            synchronized (Lock1) {
                System.out.println("Thread 1: Holding lock 1...");

                try { Thread.sleep(10); }
                catch (InterruptedException e) {}
                System.out.println("Thread 1: Waiting for lock 2...");

                synchronized (Lock2) {
                    System.out.println("Thread 1: Holding lock 1 & 2...");
                }
            }
        }
    }
    private static class Thread2 extends Thread {
        public void run() {
            synchronized (Lock2) {
                System.out.println("Thread 2: Holding lock 2...");

                try { Thread.sleep(10); }
                catch (InterruptedException e) {}
                System.out.println("Thread 2: Waiting for lock 1...");

                synchronized (Lock1) {
                    System.out.println("Thread 2: Holding lock 1 & 2...");
                }
            }
        }
    }
}
```

![업로드중..](blob:https://velog.io/1e769393-ac14-4c39-a6db-798d042c6f8a)
