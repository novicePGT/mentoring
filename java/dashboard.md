## 1. 라이브러리 설치
![](https://velog.velcdn.com/images/dymnam/post/0e087805-53fb-4ddb-ab68-d7669bb23e6b/image.png)
위 메이븐 의존성을 가져와서 추가해준다.

## Github 연결을 위한 GHCon
```java
package gitHubConnect;

import org.kohsuke.github.GitHub;
import org.kohsuke.github.GitHubBuilder;

import java.util.logging.Logger;


/*
    싱글톤 패턴 : 객체의 인스턴스가 오직 1개만 생성되는 패턴을 의미.
    -> 최초 한번의 new 연산자를 통해서 고정된 메모리 영역을 사용하여 메모리 낭비 방지.
    -> 데이터 공유가 쉽다 : 싱글톤 인스턴스가 전역으로 사용되는 인스턴스여서 다른 클래스의 인스턴스들이
    접근하여 사용할 수 있다. 하지만, 싱글톤 인스턴스의 데이터에 동시 접근하게 되면 동시성 문제가 발생할 수 있음.
*/
public class GHCon {
    private static final String GitHubConnectToken = "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";
    private static final Logger LOG = Logger.getGlobal();
    /*
        Logger 는 자바에서 기본적으로 제공해주는 Log를 위한 라이브러리이다. log4j 등 좋은 라이브러리도 있다.
        Log의 수준은 높은 순서로 SEVERE, WARNING, INFO -> 심각, 경고, 정보 이다.
     */
    private GitHub gitHub;

    public GHCon() {
        // 생성자는 외부에서 호출 못하게 Private로 지정.
        try {
            this.gitHub = new GitHubBuilder().withOAuthToken(GitHubConnectToken).build();
            LOG.info("깃 계정 연결 성공.");
        } catch (Exception e) {
            LOG.info("깃 계정 연결 실패.");
        }
    }

    public GitHub getConnection() {
        return gitHub;
    }

    public Logger getLog() {
        return LOG;
    }
}

```

## 3. DAO( Data Access Object )
```java
package liveDashboard;

import gitHubConnect.GHCon;
import org.kohsuke.github.*;

import java.io.FileNotFoundException;
import java.io.IOException;
import java.util.*;
import java.util.logging.Logger;

public class Dao {
    private Logger LOG; // 로그 찍기 위한 Logger
    private GitHub gitHub; // github 연결을 위한 GitHub
    private GHRepository repository; // github 저장소에 접근을 위함.
    /*
        Map 인터페이스를 구현한 Map 컬렉션 클래스들은 (key - value) 방식을 사용한다.
        key : value 값을 찾기 위한 이름의 역할. key -> 중복 x, value -> 중복 o
        HashMap은 Map을 구현한다. -> Key와 Value를 묶어 하나의 entry로 저장한다.
        또한, HashMap은 제네릭 타입을 사용한다.
     */
    private Map<String, Integer> participants; // UserId와 참여횟수를 위한 String, Integer -> 변수명 : 참여자를 의미.
    private int total; // 총 개수를 파악하기 위한 total

    public Dao() throws IOException {
        GHCon ghCon = new GHCon();
        this.gitHub = ghCon.getConnection(); // 깃허브 객체 가져오기
        this.LOG = ghCon.getLog(); // GHCon 에서 로그 객체 가져오기
        this.participants = new HashMap<String, Integer>();
    }

    public Logger getLOG() {
        return this.LOG;
    }

    // 저장소 세팅
    public boolean setRepository(String repository) throws IOException {
        boolean flag = false;
        // 사용자로부터 입력받은 저장소 이름을 사용하여 GHRepository 객체 생성
        try {
            this.repository = gitHub.getRepository(repository).getSource();
            LOG.info(" - " + repository + " 저장소 접근 성공");
            LOG.info("정보를 읽고 있습니다.");
            setAttendance();
            flag = true;
        } catch (FileNotFoundException e) {
            LOG.info(" - " + repository + " 저장소가 존재하지 않습니다.");
            flag = false;
        } finally {
            return flag;
        }
    }

    public void setAttendance() throws IOException {
        List<GHIssue> allTheIssues = repository.getIssues(GHIssueState.ALL); // 세팅된 저장소의 전체 issue
        /*
            Set은 List와 다르게 객체( 데이터 )를 중복해서 저장할 수 없다. 또한, 객체를 인덱스로 관리하지 않아서 순서를 보장할 수 없다.
            .add() : 객체( 데이터 ) 추가
            .iterator() : 검색을 위한 반복자 생성
            .size() : 저장된 객체의 개수를 리턴
            .clear() : 저장된 객체 모두 삭제
            .remove() : 해당 객체 삭제

            hashSet -> Set 컬렉션을 구현하는 대표적인 클래스로 데이터를 중복 저장할 수 없고 순서를 보장하지 않는다.
            LinkedHashSet -> 중복된 데이터를 저장하지 않지만 입력된 순서대로 데이터를 관리한다.
         */
        Set<String> nameList = new HashSet<String>(); // 하나의 issue에 코멘트를 남긴 user id를 담기 위한 임시 set( 중복제거 )
        this.total = allTheIssues.size(); // 참여율 계산을 위한 이슈의 총 개수

        for (GHIssue issueForWeek : allTheIssues) { // 이슈를 1주차씩 가져옴
            for (GHIssueComment comment : issueForWeek.getComments()) { // 해당 이슈의 전체 코멘트를 가져옴.
                nameList.add(comment.getUser().getLogin());
            }
            insertNames(nameList); // -> map<id, count>의 value( 출석횟수 ) 증가시키기
            nameList.clear(); // 증가시켜준 뒤, Set을 초기화 시키기 위함.
        }
    }

    // 만들어진 임시 Set으로 Map의 Count( 출석횟수 ) 증가시키기
    private void insertNames(Set<String> nameList) {
        nameList.forEach((name) -> {
            if (this.participants.containsKey(name) == true) { // 이미 Map에 존재하는 아이디일 경우.
                this.participants.put(name, participants.get(name) + 1);
            } else {
                this.participants.put(name, 1); // Map에 존재하지 않는 ID일 경우
            }
        });
    }

    // User id로 출석횟수 검색
    public Double getAttendenceRateByName( String name ) {
        int count = 1;
        try {
            count = this.participants.get(name);
            return (double) ((count * 100) / this.total);
        } catch (NullPointerException e) {
            LOG.info("존재하지 않는 아이디입니다.");
        }
        return 0.0;
    }

    // 모든 참여자의 출석 횟수 검색
    public Map<String, Double> getAllAttendenceRate() {
        Map<String, Double> allRate = new HashMap<String, Double>();
        this.participants.forEach((name, count) -> {
            allRate.put(name, (double) ((count * 100) / this.total));
        });
        return sortMapByValue(allRate); // 출석율을 기준으로 내림차순 정렬
    }

    private Map<String, Double> sortMapByValue(Map<String, Double> allRate) {
        List<Map.Entry<String, Double>> entries = new LinkedList<>(allRate.entrySet());
        Collections.sort(entries, (o1, o2) -> o2.getValue().compareTo(o1.getValue()));

        /*
            LinkedHashMap : HashMap은 인덱스로 들어가지 않아 순서를 보장하지 않지만, Linked는 인덱스로 들어온 순서를 보장한다.
            forEach ->
         */
        LinkedHashMap<String, Double> result = new LinkedHashMap<>();
        for (Map.Entry<String, Double> entry : entries) {
            result.put(entry.getKey(), entry.getValue());
        }
        return result;
    }
}

```

## 4. 기능 출력을 위한 Service
```java
package liveDashboard;

import java.io.IOException;
import java.util.Map;
import java.util.logging.Logger;

// 기능 출력을 위한 Service
public class Service {
    private Dao dao;
    private Logger logger;

    public Service() throws IOException {
        this.dao = new Dao();
        this.dao.getLOG();
    }

    // dao의 저장소 세팅으로 정보 넘기기
    public boolean setRepository(String repository) throws IOException {
        return this.dao.setRepository(repository);
    }

    // UserId로 출석률 찾기
    public void findByName(String name) {
        Double rate = this.dao.getAttendenceRateByName(name); // dao의 getAttendenceRateByName을 이용해 출석률을 검색해서 저장
        if (rate == 0) return;
        System.out.println(name + " : " + String.format("%.2f", rate) + "%" + "\n");
    }

    // 모든 유저의 출석률을 검색
    public void findAll() {
        Map<String, Double> allRate = this.dao.getAllAttendenceRate();
        int index = 1;
        for (String id : allRate.keySet()) {
            Double rate = allRate.get(id);
            System.out.println(index + ".");
            String r = String.format("%20s", id);
            System.out.println(r + " -> " + rate + "%");
            index += 1;
        }
        System.out.println("\n");
    }
}

```

## 5. 메뉴를 제공하는 Menu
```java
package liveDashboard;

import java.io.IOException;
import java.util.Scanner;

public class Menu {
    Service service;
    Scanner scanner;

    public  Menu(Scanner scanner) throws IOException {
        this.service = new Service();
        this.scanner = scanner;
    }

    public void run() throws IOException {
        String repository;
        System.out.println("\n========================= live Dashboard =========================\n");

        do {
            System.out.println("Please enter the repository name >>");
            repository = scanner.nextLine();
        } while (!this.service.setRepository(repository.trim()));

        run2();
    }

    private void run2() {
        System.out.println("\n========================= MENU =========================\n");
        String op;
        boolean flag = true;

        do {
            System.out.println("1. 출석랭킹          2. 내 출석률          3. 종료");
            System.out.print("번호 입력 >> ");
            op = scanner.nextLine();

            switch (op.trim()) {
                case "1" -> {
                    System.out.println("\n========================= RANK =========================\n");
                    service.findAll();
                }
                case "2" -> {
                    System.out.println("\n 아이디를 입력해주세요. >> ");
                    String name = scanner.nextLine();
                    service.findByName(name.trim());
                }
                case "3" -> flag = false;
            }
        } while (flag);
    }
}

```

## 메인으로 실행할 Dashboard
```java
package liveDashboard;

import java.io.IOException;
import java.util.Scanner;

public class Dashboard {
    public static void main(String[] args) throws IOException {
        Scanner scanner = new Scanner(System.in);
        Menu menu = new Menu(scanner);
        menu.run();
    }
}

```

## 실행 결과
![](https://velog.velcdn.com/images/dymnam/post/f663e31b-f6a6-47fb-8732-4086d5d77477/image.png)
