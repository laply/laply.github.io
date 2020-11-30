---
category : front-end
author_profile : true 
toc : true
toc_sticky: true
comments: true
---

front-end 2020 load map 두번 째

## HTTP (HyperText Transfer Protocol)

텍스트 기반의 통신 규약으로 
인터넷에서 데이터를 주고받을 수있는 프로토콜이다.

이러한 규약 때분에 모든 프로그램이 규약에 맞춰 개발하고 
서로 정보를 교환할 수 있다.

http 는 웹 에서 이루어지는 모든 데이터 교환의 기초이며, 
클라이언트 - 서버 프로토콜이다. 

## HTTP 특징

### 1. request - reponse 

HTTP는 클라이언트 - 서버 프로토콜이다.

클라이언트가 HTTP 요청을 서버에 보내면, 

서버는 요청을 처리한후 결과에 따른 HTTP 응답을
클라이언트에게 보내는 방식으로 수행된다. 

### 2. stateless 
    
stateless 란, 
server side에 client, server의 동작, 상태정보를 저장하지 않는 형태이다.
server응답이 client 와의 세션상태와 독립적이다.


통신을 서로 주고받아도 클라이언트와 서버가 서로 연결되어 있는 것이 아니라, 
각각의 통신은 독립적이다. 

### 3. URL (Uniform Resource Locators)

URL은 서버에 자원을 요청하기 위해 입력하는 영문 주소다. 
    
    ex) http://www.domain.com:8080/post?name=b&id=cc
    protocol://host:port/resorcePath?query


## 구성요소

클라이언트의 개별적인 요청들은 서버로 보내지며, 서버는 요청을 처리하고
response 라고 불리는 응답을 제공한다.

![Client-server-chain](https://user-images.githubusercontent.com/65690925/100572127-1933f800-3318-11eb-862a-7b440c905cd8.png)

이 요청과 응답 사이에는 여러 개체들이 있다. 
다양한 작업을 하는 게이트 웨이 또는 캐시 역활의 프록시 등이 있다. 


### 1. 웹서버
    
클라이언트에 의한 요청에 대한 문서를 제공하는 서버.


### 2. 프록시 

통신 계층중 애플리케아션 계층에서 동작하는 것들을 일반적으로 프록시라고 부른다.
프록시는 다양한 기능들을 수행 할 수 있따.

    - 캐싱
    - 필터링
    - 로드 밸런싱 
    - 인증
    - 로깅


## HTTP 흐름 

클라이언트가 서버와 통신 하고자 할 때, 최종서버가 됐든 
중간 프록시가 됐든, 다음 단계의 과정을 수행한다. 

1. TCP 연결을 연다.
    
연결은 요청을 보내거나 응답을 받는데 사용된다. 
새 연결이나, 기존의 연결이나 등등 여러가지 방법을 사용한다.

2. HTTP 메세지를 전송한다.

    GET / HTTP/1.1
    Host: developer.mozilla.org
    Accept-Language: fr

3. server가 전송한 응답을 읽는다.

    HTTP/1.1 200 OK
    Date: Sat, 09 Oct 2010 14:28:02 GMT
    Server: Apache
    Last-Modified: Tue, 01 Dec 2009 20:18:22 GMT
    ETag: "51142bc1-7449-479b075b2891b"
    Accept-Ranges: bytes
    Content-Length: 29769
    Content-Type: text/html

    <!DOCTYPE html... (here comes the 29769 bytes of the requested web page)

4. 연결을 닫거나 다른 요청을 위해 재사용 한다. 

HTTP 파이프라이닝이 활성화 되면 첫번째 응답을 완전히
 수신할 때 까지 기다리지 않고 여러 요청을 보낼 수 있다.

## HTTP 메세지 
HTTP 메세지는 서버와 클라이언트 간에 데이터가 교환되는 방식 이다.  

요청 request는 클라이언트가 서버로 전달해서 서
버의 액션이 일어나게끔 하는 메세지이고,

응답 response는 요청에 대한 서버의 답변이다. 

요청과 응답의 구조는 서로 닮아있다. 
1. start-line 실행되어야 할 요청, 또는 요청 수행에 대한 성공, 또는 실패가 기록되어있다.
    이 줄은 항상 한 줄로 끝난다.
2. 옵션으로 HTTP 헤더 세트가 들어간다. 여기에는 요청에 대 설명 혹은 메세지 본문에 대한 설명이 들어간다. 
3. 요청에 대한 모든 메타정보가 전송되었음을 알리는 빈 줄이 삽입된다. 
4. 요청과 관련된 내용이 옵션으로 들어가거나, 응답과 관련된 문서가 들어간다. 
    본문에 존재 유무 및 크기는 첫 줄과 header에 들어간다.

![HTTPMsgStructure2](https://user-images.githubusercontent.com/65690925/100571951-b80c2480-3317-11eb-9e00-7238f246c1bf.png)

### 1. 요청
#### start-line 

시작줄은 3가지 요소로 이루어져 있다. 
    
    
첫번째는 HTTP 메서드로 
 GET, PUT, POST 또는 HEAD, OPTIONS 을 사용해 서버가 수행할 동작을 나타낸다.
    
두번째는 URL 또는 프로토콜, 포트 도메인의 절대경로로 
 요청 컨택스트에 의해 특정지어진다. 

마지막으로 http 버전이 들어간다.

    POST / HTTP/1.1
    GET https://google.com HTTP/1.0
    GET https://127.0.0.1:8080 HTTP/1.0

#### header 
대소문자 구분없는 문자열과 콜론 : 값으로 구성되어있다. 

    Host: localhost:8080

이와 같이 한줄로 구성되지만,
헤더의 갯수의 차이로 길어질 수 있다.

#### 본문
일부 요청에 본문이 들어간다.
GET, HEAD, DELETE, OPTIONS 처럼 
리소스를 가져오는 요청은 본문이 필요가 없고

업데이트 하기 위한 요청으로 서버에 데이터를 전송할 때 사용한다.

넓게 보면 본문은 두가지 종류로 나뉜다.
단일-리소스 본문(single-resource bodies)
다중-리소스 본문(multiple-resource bodies)


### 2. 응답 


# 추가점 

## HTTP 요청 메소드 
## HTTP 상태코드 

## HTTPS 
## HTTP/1 HTTP/2




참고 

https://ychae-leah.tistory.com/81
https://developer.mozilla.org/ko/docs/Web/HTTP/Overview
https://joshua1988.github.io/web-development/http-part1/
