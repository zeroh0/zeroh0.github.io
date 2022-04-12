---
title: "Socket통신과 HTTP통신의 차이"

categories:
    - network
tags:
    - [network]

toc: true
toc_sticky: true

date: 2022-04-12
last_modified_at: 2022-04-12
---


<br>


# Socket 통신

![socket-img](https://user-images.githubusercontent.com/89443479/162855544-d4dca8bc-3c16-496e-a197-e470174c3464.png)

소켓이란 두 프로그램이 서로 데이터를 주고 받을 수 있는 양쪽(두 프로그램 모두)에 생성되는 통신 단자이다.

소켓 통신이란 서버와 클라이언트 **양방향 연결이 이루어지는 통신**으로, 클라이언트도 서버로 요청을 보낼 수 있고 서버도 클라이언트로 요청을 보낼 수 있는 통신

> Server와 Client가 특정 Port를 통해 **실시간으로 양방향 통신**을 하는 방식

보통 스트리밍이나 **실시간** 채팅 등 실시간으로 데이터를 주고 받아야 하는 경우 Connection을 자주 맺고 끊는 HTTP 통신보다 소켓 통신이 적합하다.  
소켓 통신은 계속해서 Connection을 들고 있기 때문에 HTTP 통신에 비해 많은 리소스가 소모된다.

## Socket 특징
- Server와 Client가 계속 연결을 유지하는 양방향 프로그래밍 방식이다.
- Server와 Client가 실시간으로 데이터를 주고받는 상황이 필요한 경우에 사용된다.
- 실시간 동영상 Streaming이나 온라인 게임 등과 같은 경우에 자주 사용된다.


<br>


# HTTP 통신

![http-img](https://user-images.githubusercontent.com/89443479/162855499-01747f21-b9ab-4bad-802b-792502cdd596.png)

**H**yper**T**ext **T**ransfer **P**rotocol의 약자로 HTML 파일을 전송하는 프로토콜이라는 의미를 가진다.  
웹브라우저에서 통신이 일어나며, 초기에는 HTML 파일을 전송하려는 목적으로 만들어졌으나 현재는 JSON, Image 파일 등 또한 전송한다.  

HTTP 통신은 클라이언트에서 서버로 요청을 보내서 서버가 응답하는 방식으로 통신이 이루어진다.  
응답에는 클라이언트의 요청에 따른 결과를 반환한다.  

> **Client의 요청(Request)이 있을 때만 서버가 응답(Response)**하여 해당 정보를 전송하고 곧바로 연결을 종료하는 방식 **(단방향적 통신)**

서버의 응답에는 응답 코드가 같이 전송되며, 사용자는 응답 코드와 메세지 응답으로부터 오는 메시지 바디를 통해 요청 값을 전달 받는다.  

초기에는 서버는 응답한 후 클라이언트(사용자)의 Connection을 곧바로 끊어버렸으나, 최근에는 성능상의 이유(Connection을 맺고 끊는 비용이 비싸다)로 Keep Alive 옵션을 통해 일정 기간 동안 클라이언트와 Connetion을 유지하는 방식으로 통신이 가능해졌다.  

## HTTP 특징
- Client가 요청을 보내는 경우에만 Server가 응답하는 단방향 프로그래밍 방식이다.
- Server로부터 소켓 연결을 하고 응답을 받은 후에는 연결이 바로 종료된다.
- 실시간 연결이 아니고, 응답이 필요한 경우에만 Server와 연결을 맺어 요청을 보내는 상황에 유용하다.
- 요청을 보내 Server의 응답을 기다리는 어플리케이션(Android or IOS) 개발에 주로 사용된다.


<br>


# 참고
<sup><a href="https://kotlinworld.com/75">HTTP 통신과 Socket 통신의 차이점</a></sup>  
<sup><a href="https://mangkyu.tistory.com/48">[네트워크 프로그래밍] Http 프로그래밍과 Socket 프로그래밍 차이</a></sup> 
