---
layout: post
title: Spring Preview
subtitle: JAVA vs EJB vs Spring(boot)
categories: Spring
tags: [Spring, Backend]
---


#### <span style="background-color:yellow">바닐라 자바의 문제점</span>
바닐라 자바로 계좌이체서비스를 구현해보자
![계좌이체서비스](/assets/images/0826/exchange.jpg)
1. 은행 계좌 A, B를 생성하고 (Account 클래스)
2. 계좌간 송금 서비스 기능 구현 후, (TransferService 클래스)
3. A가 B에게 200을 준다. (Main 클래스)
~~~
public class Account {
    private String id;
    private int balance;

    public Account(String id, int balance) {
        this.id = id;
        this.balance = balance;
    }
    public void withdraw(int amount) { balance -= amount; }
    public void deposit(int amount) { balance += amount; }
    public int getBalance() { return balance; }
}

public class TransferService {
    public void transferMoney(Account from, Account to, int amount) {
        from.withdraw(amount);
        to.deposit(amount);
        // DB에 직접 JDBC 코드 작성 → Connection/commit/rollback 수동 관리
    }
}

public class Main {
    public static void main(String[] args) {
        Account a = new Account("A", 1000);
        Account b = new Account("B", 500);

        TransferService service = new TransferService();
        service.transferMoney(a, b, 200);
    }
}

~~~
=> 지금은 A, B 2개의 통장만 사용하고 있기 때문에 불편함을 크게 못 느끼지만 
토스같은 대규모 트래픽을 다루는 서버에서 이걸 지원했다면 new 생성/삭제 지옥이 나타날 것이다. 
<br>
  
#### <span style="background-color:yellow">EJB의 등장</span>
트랜잭션 관리, 보안, 분산 처리 같은 기능은 직접 구현해야 하기도 하며,
공통 기능 중복에 대한 효율 문제가 대두되었고 **EJB**가 만들어졌다.
=> 대규모 엔터프라이즈 시스템을 “표준화된 방법”으로 만들자는 의도로 만들어졌었다.

~~~
// Remote Interface
public interface TransferService extends javax.ejb.EJBObject {
    void transferMoney(String from, String to, int amount) throws RemoteException;
} => 기본 자바문법과도 명칭들이 다르다.

// Home Interface
public interface TransferServiceHome extends javax.ejb.EJBHome {
    TransferService create() throws RemoteException, CreateException;
}

// Bean Implementation
public class TransferServiceBean implements javax.ejb.SessionBean {
    public void transferMoney(String from, String to, int amount) {
        // 비즈니스 로직
        // 출금 계좌 -amount, 입금 계좌 +amount
        // JDBC 코드 직접 작성
    }

    // EJB 생명주기 메서드 (반드시 구현해야 했음) => 이게 문제다.
    public void ejbActivate() {}
    public void ejbPassivate() {}
    public void ejbRemove() {}
    public void setSessionContext(SessionContext ctx) {}
}

~~~

하지만 이는 과도한 엔터프라이즈를 지향했었고 단위테스트의 어려움, 복잡한 xml설정 등으로 인해 Agile한 방식의 개발경험과는 대비되는 프레임 워크였다.
<br>

#### <span style="background-color:yellow">Spring의 등장</span>

따라서 필요한 기능들을 남겨두되, 따로 더 배우지 않아도 되도록 POJO와 JDBC, Hibernate, JTA 등 기존 기술을 추상화하여 통합시킨 프레임워크가 필요했고 이것이 **Spring**이다.
(그래도 xml 설정이 많다고 생각하여 나온것이 **Spring boot**다.)

스프링의 핵심은 **Bean 등록**이다.

~~~
@Service
public class TransferService {

    private final AccountRepository accountRepository;

    public TransferService(AccountRepository accountRepository) {
        this.accountRepository = accountRepository;
    }

    @Transactional
    public void transferMoney(String from, String to, int amount) {
        Account fromAccount = accountRepository.findById(from).orElseThrow();
        Account toAccount = accountRepository.findById(to).orElseThrow();

        fromAccount.withdraw(amount);
        toAccount.deposit(amount);

        accountRepository.save(fromAccount);
        accountRepository.save(toAccount);
    }
}
~~~

##### <u> 1. POJO(바닐라 자바 문법) 그대로 사용했기 때문에 특별한 인터페이스 구현 필요 없고 </u>
##### <u>2. @Transactional 등의 태그로 트랜잭션 시작/커밋/롤백 자동 처리 등의 자주사용기능도 중복으로 불러내지 않아도 되며, </u>
##### <u>3. IoC/DI도 스프링 컨테이너가 가짜 객체를 만들어 단위 테스트 가능하여 관리테스트에도 용이해졌다. (꼭 DB를 연결하지 않아도 됨) </u>

=> 하지만 이 계좌이체서비스를 구현하기 위해서 초기설정이나 빠른 개발 환경 구축에는 어려움을 느꼈기 때문에, 이로인해 만들어진 것이 **Spring boot**였다.

<br>



#### <span style="background-color:yellow">Spring 생태계</span>

스프링 생태계는 모듈과 프로젝트로 구성되어있다.
(Spring boot는 Spring의 한 프로젝트일 뿐이다.)
![토스채용조건](/assets/images/0826/traffic.jpg)
스프링은 모듈은 자바의 라이브러리, 프로젝트는 스프링에서 특정기능만 담당하는 애플리케이션으로 보면 된다.

~~~
<Spring 생태계>
 ├── Spring Framework [모듈]
 │     ├─ Core, Beans, Context, SpEL
 │     ├─ AOP
 │     ├─ Data Access (JDBC, ORM)
 │     ├─ Web
 │     │   ├─ Spring MVC  ← (동기/블로킹, Servlet 기반)
 │     │   └─ Spring WebFlux (비동기/리액티브, Netty 기반)
 │     ├─ Data Access, Tx (트랜잭션관리)
 │     └─ Testing
 │
 │[Project]
 ├── Spring Boot (편의성)
 ├── Spring Data (DB 추상화)
 ├── Spring Security (보안)
 ├── Spring Cloud (분산/마이크로서비스)
 ├── Spring Batch (배치 처리)
 ├── Spring Integration (메시징/연동)
 └── … (GraphQL, AI, Shell 등 확장 프로젝트)
~~~
<br>


📌 핵심 프레임워크
- Spring Boot: 자동 설정, 스타터 제공 → 빠르게 애플리케이션 시작 가능
- Spring Framework: 스프링의 핵심, DI·AOP·트랜잭션 등 기본기능
- Spring Data: JPA, MongoDB, Redis 등 다양한 데이터 접근 추상화

📌 마이크로서비스 / 클라우드
- Spring Cloud: 마이크로서비스 공통 패턴(구성, 라우팅, 서비스 등록 등) 지원
- Spring Cloud Data Flow: 마이크로서비스 데이터 파이프라인 오케스트레이션
- Spring Session: 세션을 DB/Redis 등 외부 저장소에 분리해 관리

📌 데이터 처리 / 배치
- Spring Batch: 대량 데이터 배치 작업(ETL, 스케줄링)
- Spring AI: AI 모델 연결 및 데이터 연동 프레임워크

📌 보안 / 인증
- Spring Security: 인증/인가(로그인, 권한 제어, OAuth2 등)
- Spring Authorization Server: OAuth2, OpenID Connect 서버 구축

=== 여기까지 핵심으로 보인다 ===


📌 API / 데이터 표현
- Spring for GraphQL: GraphQL API 구현 지원
- Spring HATEOAS: REST API 응답에 하이퍼링크 포함해 리소스 관계 표현
- Spring REST Docs: REST API 문서 자동 생성

📌 통합 / 메시징
- Spring Integration: EIP(Enterprise Integration Pattern) 지원, 메시징/어댑터
- Spring AMQP: RabbitMQ 같은 AMQP 메시징 추상화
- Spring for Apache Kafka: Kafka 메시징 추상화
- Spring for Apache Pulsar: Pulsar 메시징 추상화

📌 애플리케이션 구조 / 모듈화
- Spring Modulith: 모놀리식 애플리케이션을 도메인 단위 모듈로 구조화
- Spring Statemachine: 상태 기계(State Machine) 기반 로직 구현

📌 유틸 / 기타
- Spring Shell: CLI 기반 리소스 실행/테스트 도구
- Spring Web Flow: 화면 전환이 많은 웹앱(항공권 예약, 대출 신청 등) 흐름 관리
- Spring Web Services: SOAP 기반 웹서비스 구현
- Spring LDAP: LDAP 연동을 단순화





---
참고자료
- [공식문서](https://spring.io/projects)
- [스프링생태계](https://ch-tech.tistory.com/17)
- [스프링구성요소1](https://devscb.tistory.com/119)
- [스프링구성요소2](https://ajdxjdrnfld.tistory.com/9)
