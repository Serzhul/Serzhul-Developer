---
title: HTTP 종류
date: 2022-05-15 15:05:93
category: CS
thumbnail: { thumbnailSrc }
draft: false
---

<a href="https://link.coupang.com/a/m6aF5" target="_blank" referrerpolicy="unsafe-url">
출처 : 면접을 위한 CS 전공지식 노트
</a>

# HTTP/1.0

- HTTP/1.0은 기본적으로 한 연결당 하나의 요청을 처리하도록 설계되었으며, 이는 RTT 증가를 초래한다.

## 특징

### RTT 증가

- **RTT(Round Trip Time, 왕복 시간)**란 패킷이 목적지에 도달하고 나서 다시 출발지로 돌아오기까지 걸리는 시간이며 패킷 왕복 시간을 의미한다.
- 서버로부터 파일을 가져올 때마다 TCP의 3-웨이 핸드셰이크를 계속해서 열어야 하기 때문에 RTT가 증가한다는 단점이 있다.

### RTT 증가 해결 방법

- **이미지 스플리팅**
  - 많은 이미지를 다운로드 받으면 과부하가 걸리기 때문에 많은 이미지가 합쳐져 있는 하나의 이미지를 다운로드 받고, background-image position 속성을 이용하여 이미지를 표기하는 방법
- **코드 압축**
  - 코드를 압축해서 개행 문자, 빈칸을 없애서 코드의 크기를 최소화하는 방법
- **이미지 Base64 인코딩**
  - 이미지 파일을 64진법으로 이루어진 문자열로 인코딩하는 방법
  - 서버와의 연결을 열고 이미지에 대해 서버에 HTTP 요청을 할 필요가 없다는 장점이 있음
  - 단, 문자열로 변환시 크기가 약 37% 더 커지는 단점이 존재

# HTTP/1.1

- HTTP/1.0의 발전된 버전으로 매번 TCP 연결을 하는 것이 아니라 한 번 TCP 초기화를 한 이후에 keep-alive라는 옵션으로 여러 개의 파일을 송수신할 수 있게 변경
- HTTP/1.0과 달리 한번 TCP 3-웨이 핸드셰이크가 발생하면 그다음부터 발생하지 않는다.
- 문서 안에 포함된 다수의 리소스를 처리하려면 요청할 리소스 개수에 비례해 대기 시간이 길어진다.

## 특징

### HOL Blocking

- HOL Blocking(Head Of Line Blocking)은 네트워크에서 같은 큐에 있는 패킷이 그 첫 번째 패킷에 의해 지연될 때 발생하는 성능 저하 현상을 의미한다.

### 무거운 헤더 구조

- HTTP/1.1 헤더는 쿠키 등 많은 메타데이터가 들어있고 압축이 되지 않아 무거웠다.

# HTTP/2

- HTTP/2는 SPDY 프로토콜에서 파생된 HTTP/1.x보다 지연 시간을 줄이고 응답 시간을 더 빠르게 할 수 있으며, 멀티플렉싱, 헤더 압축, 서버 푸시, 요청의 우선순위 처리를 지원하는 프로토콜이다.

## 특징

### 멀티플렉싱

- 멀티플렉싱은 여러 개의 스트림을 사용하여 송수신하는 것을 의미한다. 이를 통해 특정 **스트림(stream, 시간이 지남에 따라 사용할 수 있게 되는 일련의 데이터 요소를 가리키는 데이터 흐름)**의 패킷이 손실되었다고 하더라도 해당 스트림에만 영향을 미치고 나머지 스트림은 정상적으로 동작할 수 있다.
- 단일 연결을 사용하여 병렬로 여러 요청을 받을 수 있고 응답을 줄일 수 있다. ⇒ HOL Blocking 해결이 가능하다.

### 헤더 압축

- HTTP/1.x의 큰 헤더 구조를 HTTP/2에서는 헤더 압축을 써서 해결한다.(허프만 코딩 압축 알고리즘)

**허프만 코딩**

- 허프만 코딩(huffman coding)은 문자열을 문자 단위로 쪼개 빈도수를 세어 **빈도가 높은 정보는 적은 비트 수를 사용**하여 표현하고, **빈도가 낮은 정보는 비트 수를 많이 사용**하여 표현해서 전체 데이터의 표현에 필요한 비트양을 줄이는 원리다.

### 서버 푸시

- HTTP/1.1 에서는 클라이언트가 서버에 요청을 해야 파일을 다운로드 받을 수 있었다면, HTTP/2는 클라이언트 요청 없이 서버가 바로 리소스를 푸시할 수 있다.
- CSS, JS 파일들을 서버에서 푸시하여 클라이언트에 먼저 주는 것이 가능하다.

# HTTPS

- HTTP/2는 HTTPS 위에서 동작한다.
- HTTPS는 애플리케이션 계층과 전송 계층 사이에 신뢰 계층인 SSL/TLS 계층을 넣은 신뢰할 수 있는 HTTP 요청을 말하며, 이를 통해 **‘통신을 암호화’**한다.

## SSL/TLS

- SSL(Secure Socket Layer)는 SSL 1.0부터 시작해서 SSL2.0, SSL 3.0, TLS(Transport Layer Security Protocol) 1.0 TLS 1.3까지 버전이 올라가며 마지막으로 TLS로 명칭이 변경되었으며 이를 합쳐 **SSL/TLS**라고 많이 부름 (하기 설명은 TLS 1.3 기반 설명)
- SSL/TLS는 전송 계층에서 보안을 제공하는 프로토콜로, 클라이언트와 서버가 통신할 때 SSL/TLS를 통해 제3자가 메시지를 도청하거나 변조하지 못하도록 한다.
- SSL/TLS는 **보안 세션**을 기반으로 데이터를 암호화하며 보안 세션이 만들어질 때 **인증 메커니즘, 키 교환 암호화 알고리즘, 해싱 알고리즘**이 사용된다.
- TLS 1.3은 사용자가 이전에 방문한 사이트로 다시 방문하면 SSL/TLS에서 보안 세션을 만들 때 걸리는 통신을 하지 않아도 되며 이를 0-RTT라 한다.

## 특징

### 보안 세션

- 보안 세션이란 보안이 시작되고 끝나는 동안 유지되는 세션을 말한다.
- SSL/TLS는 핸드셰이크를 통해 보안 세션을 생성하고 이를 기반으로 상태 정보 등을 공유한다.
- 클라이언트와 서버가 키를 공유하고, 이를 기반으로 인증, 인증 확인 등 작업이 일어나며 한 번의 1-RTT가 생긴 후 데이터를 송수신한다.

1. 클라이언트에서 사이퍼 슈트(cypher suites)를 서버에 전달하면 서버는 받은 사이퍼 슈트의 암호화 알고리즘 리스트를 제공할 수 있는지 확인한다.

2. 제공할 수 있다면 서버에서 클라이언트로 인증서를 보내는 인증 메커니즘이 시작되고 이후

3. 암호화 알고리즘, 해싱 알고리즘 등으로 암호화된 데이터의 송수신이 시작된다.

**사이퍼 슈트**

사이퍼 슈트는 프로토콜, AEAD 사이퍼 모드, 해싱 알고리즘이 나열된 규약을 말한다.

- TLS_AES_128_GCM_SHA256
- TLS_AES_256_GCM_SHA384
- TLS_CHACHA20_POLY1305_SHA256
- TLS_AES_128_CCM_SHA256
- TLS_AES_128_CCM_8_SHA256

**인증 메커니즘**

- CA(Certificate Authorities)에서 발급한 인증서를 기반으로 이뤄진다.
- CA에서 발급한 인증서는 안전한 연결을 시작하는 데 있어 필요한 ‘공개키’를 클라이언트에 제공하고, 사용자가 접속한 ‘서버가 신뢰’할 수 있는 서버임을 보장한다.
- 인증서는 서비스 정보, 공개키, 지문, 디지털 서명 등으로 이루어져 있다.
- CA는 신뢰성이 엄격하게 공인된 기업들만 참여 가능하며, 대표적 기업으로는 Comodo, GoDaddy, GlobalSign, 아마존 등이 있다.

**CA 발급 과정**

- CA 인증서를 발급받으려면 자신의 사이트 정보와 공개키를 CA에 제출해야 한다.
- CA는 공개키를 해시한 값인 지문(finger print)을 사용하는 CA의 비밀키 등을 기반으로 CA인증서를 발급한다.

**암호화 알고리즘**

- 키 교환 암호화 알고리즘으로는 대수곡선 기반의 ECDHE 또는 모듈식 기반의 DHE를 사용한다. (둘 다 디피-헬만 방식을 근간으로 한다.)

**디피-헬만 키 교환 알고리즘(Diffie-Hellman key exchange)**

- 암호키를 교환하는 하나의 방법으로 $y=g^xmod p$ 로 표현할 수 있다.
- g와 x, p를 알면 y는 구하기 쉽지만 g, y, p만 알면 x를 구하기는 어렵다는 원리에 기반한 알고리즘이다.
- 공개 값을 공유하고 각자의 비밀 값과 혼합한 후 혼합 값을 공유, 이후 각자 비밀 값과 또 혼합하면 공통의 암호키가 생성된다.
- 클라이언트 및 서버 모두 개인키와 공개키를 생성하고 서로에게 공개키를 보내면 이 둘을 결합해 PSK(사전 합의된 비밀키)가 생성이 되고 공격자가 개인키나 공개키가 있어도 PSK가 없기 때문에 아무것도 할 수 없게 된다.

**해싱 알고리즘**

- 데이터를 추정하기 힘든 더 작고, 섞여 있는 조각으로 만드는 알고리즘이다.
- SSL/TLS는 해싱 알고리즘으로 SHA-256 알고리즘, SHA-384 알고리즘을 쓴다.

**SHA-256 알고리즘**

- 해시 함수의 결괏값이 256비트인 알고리즘이며 비트 코인을 비롯한 블록체인 시스템에서 사용한다.
- SHA-256 알고리즘은 해싱을 해야 할 메시지에 1을 추가하는 등 전처리를 하고 전처리된 메시지를 기반으로 해시를 반환한다.

## SEO에 도움이 되는 HTTPS

- 구글은 SSL 인증서를 강조하였으며, 사이트 내 모든 요소가 동일하면 HTTPS 서비스하는 사이트가 SEO 순위가 높을 것이라 발표한 바 있다.
- SEO(Search Engine Optimization, 검색엔진 최적화)는 사용자들이 구글, 네이버 같은 검색엔진으로 웹 사이트를 검색했을 때 그 결과를 페이지 상단에 노출시켜 많은 사람이 볼 수 있도록 최적화하는 방법을 말한다.
- 이를 위한 방법으로 캐노니컬 설정, 메타 설정, 페이지 속도 개선, 사이트맵 관리 등이 있다.

**캐노니컬 설정**

- 캐노니컬 태그란 사이트 내 URL 주소는 다르지만 동일한 내용의 중복된 페이지가 있을 때 페이지에 코드를 삽입하여 검색엔진에 대표가 되는 URL 주소를 알려주는 역할을 하는 태그이다.
- 사이트 링크에 캐노니컬을 설정함으로써 SEO 점수를 높일 수 있다.

**메타 설정**

- html 문서의 메타 데이터 부분을 설정해야 한다.

**페이지 속도 개선**

- 구글의 PageSpeedInsights로 가서 서비스에 대한 리포팅을 주기적으로 받아 관리할 수 있다.

**사이트맵 관리**

- 사이트맵(sitemap.xml)을 정기적으로 관리함으로써 SEO 점수를 올릴 수 있다.
- 사이트맵 제너레이터 혹은 직접 코드를 만들어 구축하는 것도 가능하다.

### HTTPS 구축 방법

- 대표적으로 3가지가 있다.
  - 직접 CA에서 구매한 인증키 기반 서비스 구축
  - 서버 앞단에 HTTPS를 제공하는 로드밸런서 두기
  - 서버 앞단에 HTTPS를 제공하는 CDN 두기

# HTTP/3

- HTTP/3은 HTTP/1.1 및 HTTP/2와 함께 World Wide Web에서 저오를 교환하는데 사용하는 HTTP의 세 번째 버전이다.
- TCP 위에서 돌아가는 HTTP/2와 달리 HTTP/3은 QUIC라는 계층위에서 돌아가며 TCP 기반이 아닌 UDP 기반으로 돌아간다.
- HTTP/2의 장점인 멀티플렉시을 갖고 있으며 초기 연결 설정 시 지연 시간 감소라는 장점이 있다.

## 특징

### 초기 연결 설정 시 지연 시간 감소

- QUIC는 TCP를 사용하지 않기 때문에 통신을 시작할 때 번거로운 3-웨이 핸드셰이크 과정을 거치지 않아도 된다.
- QUIC은 첫 연결 설정에 1-RTT만 소요되기 때문에 클라이언트가 서버에 어떤 신호를 한번 주고, 서버도 거기에 응답하면 본 통신 시작이 가능하다.
- QUIC는 순방향 오류 수정 메커니즘(FEC, Forward Error Correction)이 적용되어 전송한 패킷이 손실되었으면 수신 측 에러를 검출하고 수정하고 열악한 네트워크 환경에서 낮은 패킷 손실률을 자랑한다.

---

"이 포스팅은 쿠팡 파트너스 활동의 일환으로, 이에 따른 일정액의 수수료를 제공받습니다."