---
layout: post
title: 프로세스와 스레드의 차이
subtitle:  ContextSwitching
categories: 
tags: [JAVA]
---

JAVA의 가장 큰 장점은 운영체제 호환성과 더불어 멀티쓰레드 조작이다.

1. 동시성(Concurrency)과 병렬성(Parallelism)
동시성: 여러 요청이 거의 동시에 들어오더라도, CPU 한 코어를 빠르게 번갈아가며 실행하여 마치 동시에 처리하는 것처럼 보이게 하는 기술.

병렬성: CPU 여러 코어에서 실제로 동시에 실행하는 것.

Java는 멀티스레드와 **스레드풀(ExecutorService)**로 이 두 가지를 모두 활용할 수 있어, 수천~수만 개의 요청을 효율적으로 분배 처리할 수 있습니다.






