---
layout: post
title: JAVA를 쓰는 이유가 뭘까
subtitle: 블로그 시작하는법
categories: 탐구
tags: [JAVA, Backend]
---

백엔드 서버를 만들 때 C++, 자바, 파이썬을 많이 쓴다.
동시다발적으로 국비지원 자바캠프가 많아져서 우리나라에서는 자바 인력이 엄청 많아지고 기업에서도 선호하고 있다고 한다.

그렇다면 왜 하필 자바를 골랐을까?
(만약 국비지원이라고 하더라도, 객체지향언어기 때문에 어느정도 실력이 있는 개발자라면 빠르게 배울 수도 있고, 이미 알고 있는 사람도 대부분일 것이기 때문이다.)


## 백엔드에서 자바를 쓰는 이유는?
JAVA의 가장 큰 장점은 운영체제 호환성과 더불어 멀티쓰레드 조작이다.

1. 서버와 클라이언트 딴에서 사용하는 운영체제는 다르다.
(말이 뭔가 이상함)

-1) 왜 운영체제가 달라도 호환되야하고?
-2) 다른 언어는 그 문제에 대해서 어떻게 해결하고 있는지?

Java는 JVM(Java Virtual Machine) 위에서 실행되기 때문에, 운영체제에 직접 의존하지 않고 동일한 바이트코드를 여러 환경에서 그대로 구동할 수 있다.
=> 심화1의 1번처럼 운영체제에서 애초에 규칙을 그렇게 잡은 것에 대해서는 성립하지 않는다

![예시](심화1의 1번 \n \r)

![예시](자바가 호환되는 예싱)

이에 대해 운영체제 호환이 리눅스를 사용해야하는 서버는 자바와 호환성이 아주 좋고
이에 대한 멀티쓰레드 조작이 대부분이다.

그렇기 때문에 자바를 할 때 돌아가는 여정은 보통 멀티쓰레드를 사용하는 서버를 만들고
이에 문제가 생길 시
리눅스 명령어를 얻었을 때


자바를 사용하는 이유는 DB처리에도 있다.
자바는 DB처리에

====
결론 요약
C: 가장 낮은 레벨에서 “뭐든 가능”. 최고 성능/최소 오버헤드, 대신 개발·운영 난이도↑(메모리/동기화 직접 관리).

Python(CPython): I/O 동시성은 강함(asyncio, uvloop), CPU 병렬성은 GIL로 제약 → 보통 멀티프로세싱·C확장·분산으로 해결.

Java: 스레드/풀/NIO + 원숙한 런타임(JIT, GC) + 프레임워크/툴링 덕에 대규모 동시성을 “현실적인 비용”으로 운영하기 쉬움. (게다가 **가상 스레드(Java 21+)**로 코드 단순·확장성↑)

포인트별 비교
1) 동시성/병렬성 모델
C: pthread/epoll/kqueue/io_uring 등으로 최대 자유도. 설계·락·캐시 친화도까지 직접 챙겨야 함.

Python(CPython):

I/O: asyncio(FastAPI/uvicorn), gevent, Twisted로 고효율.

CPU: GIL로 한 시점 하나의 스레드만 바이트코드 실행 → multiprocessing/joblib/C 확장(NumPy, Cython)로 우회.

Java: 고전 스레드/풀/CompletableFuture/NIO/Netty에 더해 **가상 스레드(virtual threads)**로 “스레드=요청” 모델도 실용화. 락·컨텍스트 전환 비용 부담이 적고 코드가 단순.

2) 런타임/성능 특성
C: 네이티브. GC 없음 → 지연 최소화 가능. 대신 메모리 안전성·리소스 누수 위험.

Python: 인터프리터 오버헤드. C확장(NumPy/PyTorch)나 서버 앞단 리버스 프록시(nginx)로 실전 성능 확보.

Java: JIT(HotSpot) 최적화, GC(G1/ZGC/Shenandoah) 튜닝으로 처리량/지연 타협점을 폭넓게 맞출 수 있음. 리니어 스케일아웃이 쉬움.

3) 네트워크/I-O 스택
C: libevent/libuv/epoll + 커스텀 풀 → 최고 효율. 유지보수 비용 큼.

Python: asyncio + uvloop + FastAPI/Starlette → I/O 서버에 최적.

Java: Netty, Servlet(톰캣/제티/언더토우), Spring WebFlux → 표준화 + 고성숙도.

4) 운영/관측(Observability)
C: gdb, perf, valgrind 등 강력하지만 숙련 필요.

Python: cProfile, tracemalloc, async-profiler(py-flame) 등. 병목이 C확장 안/밖 어디인지 파악 중요.

Java: JFR, jstack/jmap/jcmd, GC 로그, 스레드 덤프, 풍부한 APM(현업 난이도↓).

5) 생태계/프레임워크
C: 프레임워크보단 라이브러리 기반 조립. 팀 성숙도 의존.

Python: Django/Flask/FastAPI + 데이터/AI 생태계 최강.

Java: Spring Boot, Spring Data, Spring Security, Jakarta EE 등 엔터프라이즈 표준화가 강점(트랜잭션, 커넥션풀, 보안, 배포 패턴까지 일관).

6) DB 처리
C: 드라이버 직접. 성능 좋지만 개발비용 큼.

Python: Django ORM, SQLAlchemy 생산성↑, 대규모 트랜잭션·스키마 엄격성은 설계 역량 의존.

Java: JDBC 표준 + JPA/Hibernate/Spring Data로 트랜잭션, 배치, 샤딩, 멀티-DS 등 대형 서비스 패턴이 정석화.

언제 뭘 쓰면 좋은가 (현업 감각)
C: 초저지연(트레이딩 엔진), 네트워크 패킷 처리, 커스텀 런타임/프록시, 특수 하드웨어 활용.

Python: I/O 중심 API 서버, 데이터/AI 백엔드, 빠른 프로토타이핑, 비동기 이벤트 처리.

Java: 대규모 트랜잭션/동시성 웹서비스, 미션크리티컬(금융/결제/통신), 장기간 운영·확장 전제의 엔터프라이즈.





