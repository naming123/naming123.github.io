---
layout: post
title: CORS
subtitle: PWA에 관해서
categories: 탐구
tags: [JAVA, Backend]
---


![banner](/assets/images/0912/image.jpg)

### origin 과 URL

### ajax요청을 다루어보자

### cors로 막는 주체는 브라우저다. 

=> cors는 우리를 도와주는 것이다.
CORS의 "정체"

CORS 자체는 “에러”라기보다 브라우저의 보안 검사 과정이에요.

브라우저가 서버에게 물어봄:
> "이 요청자는 너네 API를 불러도 되는 사이트야?"

서버가 응답 헤더에 Access-Control-Allow-Origin을 안 내려주면,
브라우저가 스스로 차단하고 콘솔에 CORS 에러를 띄움.

즉, 정체는 브라우저 보안 정책(SOP)의 연장선이에요.
- 에러 메시지를 띄우는 건 브라우저.
- 실제 허용 여부를 정하는 건 서버.



---

참고자료
- [sxungchxn.dev](https://velog.io/@seungchan__y/CORS-%EC%97%90%EB%9F%AC%EC%99%80-%ED%95%B4%EA%B2%B0%EB%B2%95)







