---
layout: post
title: JAVA의 객체지향
subtitle: 자바의 객체, 클래스, 변수, 인스턴스, 메서드, 값을 중심으로
categories: easycode
tags: [JAVA]
---


1. 서론 — 왜 이 주제를 한 번에 다루는가
자바의 객체 지향 문법은 클래스·객체·인스턴스·변수·메서드가 유기적으로 연결되어 있음

실무/시험/코딩테스트에서 헷갈리기 쉬운 용어와 개념을 한 번에 정리

args(매개변수) 동작 이해까지 확장하면 코드 작성 속도가 빨라짐

2. 클래스(Class)와 객체(Object)
클래스: 속성과 동작을 정의하는 설계도

객체: 클래스를 기반으로 메모리에 생성된 실체

예시

public class Car {
    String color;
    void drive() {
        System.out.println("Driving...");
    }
}
Car myCar = new Car(); // myCar는 객체(인스턴스)
3. 인스턴스(Instance)
정의: 특정 클래스에서 new 키워드를 통해 생성된 구체적인 객체

특징:

같은 클래스라도 인스턴스마다 다른 데이터 상태를 가짐

JVM 힙 메모리에 저장됨

예시

Car car1 = new Car();
Car car2 = new Car();
4. 변수(Variable)의 종류
인스턴스 변수: 객체별로 고유한 값 저장

클래스 변수(static): 모든 인스턴스가 공유

지역 변수: 메서드 내부에서만 사용 가능

매개변수(Parameter): 메서드 호출 시 전달받는 값

예시

class Example {
    static int sharedCount; // 클래스 변수
    int id;                 // 인스턴스 변수
    void setId(int id) {    // 매개변수
        this.id = id;
    }
}
5. 메서드(Method)와 args(매개변수)
메서드 정의 구조: 반환형 + 이름 + 매개변수 목록 + 몸체

매개변수(args) 특징:

값 타입: 복사 전달

참조 타입: 주소 전달(실질적으로 같은 객체 참조)

예시

void greet(String name) {
    System.out.println("Hello " + name);
}
greet("Alice"); // name = "Alice"
6. 클래스-객체-변수-메서드-인스턴스 관계 시각화
다이어그램:
Class → new → Instance(Object)
↳ 인스턴스 변수 / 클래스 변수
↳ 메서드 호출 시 매개변수 전달

7. 실전 예제
Car 클래스 만들기 → 변수/메서드 선언 → 인스턴스 생성 → 값 설정 → 메서드 호출

값 타입 매개변수 vs 참조 타입 매개변수 비교 실험

8. 결론
자바에서 클래스, 객체, 인스턴스는 서로 다른 개념

변수는 종류별로 쓰임이 다르고 메모리 저장 위치도 다름

매개변수 전달 방식 이해는 버그 예방과 성능에 직결됨








