---
layout: post
title: 프로세스와 스레드의 차이
subtitle:  ContextSwitching
categories: 
tags: [JAVA]
---

application을 실행할 때는??


. 논블로킹 I/O
트래픽이 많을 때는 I/O(네트워크·디스크) 대기 시간이 병목이 됩니다.

Java NIO(Non-blocking I/O)나 Netty 같은 라이브러리를 쓰면, 요청 처리를 블로킹 없이 이벤트 기반으로 실행해, 훨씬 많은 연결을 동시에 유지할 수 있습니다.

예: 채팅 서버,




