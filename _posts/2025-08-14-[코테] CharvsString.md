---
layout: post
title: String vs char vs String[]
subtitle: ByteStream은 어떻게 처리하는가에 대하여
categories: JAVA
tags: [JAVA]
---

![banner](/assets/images/0814/(char)banner.jpg)

### 1. String vs char vs String[]

- char: 한 글자(UTF-16 코드 유닛)
- String: 불변 문자열
- String[]: 문자열들의 배열(가변 길이 아님)

### 2. 변환/핵심

    char → String: String.valueOf(c)
    String → char: s.charAt(i)
    String ↔ char[]: s.toCharArray(), new String(chars)
    String ↔ String[]: split(), String.join()

### 3. I/O 빠르게 (문자/문자열 중심)

1) 입력

    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    String line = br.readLine();            // 한 줄
    String[] tokens = line.split("\\s+");   // 공백 분리
    char c = line.charAt(0);                // 첫 글자
    char[] arr = line.toCharArray();        // 전체 문자 배열


2) 출력

    // 한 글자씩
    for (char ch : arr) System.out.print(ch);
    System.out.println();

    // String 배열은 join으로
    String[] words = {"A","B","C"};
    System.out.println(String.join(",", words)); // A,B,C


    주의(배열 “이상한 출력” 방지)

    System.out.println(words);                // [Ljava.lang.String;@2f92e0f4
    System.out.println(Arrays.toString(words)); // [A, B, C]

