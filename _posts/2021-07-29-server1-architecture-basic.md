---
title:  "클라이언트 - 서버 기본"
excerpt: "Server Architecture Basic 1"

categories:
  - Network
tags:
  - [HTTP][API][Network]

date: 2021-07-29
last_modified_at: 2021-07-29
---

## 개념
| 이름         | 하는일               |
| ------------ | -------------------- |
| 클라이언트   | 리소스를 사용하는 앱 |
| 서버         | 리소스를 전달하는 앱 |
| 데이터베이스 | 리소스를 보관하는 곳 |
  
<br/>
    
<br/>
  
#### 서버 구성 종류
1. 2티어 = 서버에서 데이터보관까지 같이  
```
        -Request->
Client              Server
        <-Response-
```
  
2. 3티어 = 데이터베이스를 따로 두어  
```
        -Request->          -Request->
Client              Server              Database
        <-Response-         <-Response-
```
  
프로토콜은 통신 규약(문서 서식같은) 으로 클라이언트와 서버간 통신하는 방법  
    
<br/>
    
<br/>
  
#### 프로토콜 몇가지
| 프로토콜 이름 | 설명                                                |
| ------------- | --------------------------------------------------- |
| HTTP          | 웹에서 HTML, JSON 등의 정보를 주고받는 프로토콜     |
| HTTPS         | HTTP 에서 보안이 강화된 프로토콜                    |
| FTP           | 파일 전송 프로토콜                                  |
| SMTP          | 메일을 전송하기 위한 프로토콜                       |
| SSH           | CLI 환경의 원격 컴퓨터에 접속하기 위한 프로토콜     |
| RDP           | Windows 계열의 원격 컴퓨터에 접속하기 위한 프로토콜 |
| WebSocket     | 실시간 통신, Push 등을 지원하는 프로토콜            |
    
<br/>
    
<br/>
  
#### API란  
>서버와 클라이언트가 의사소통을 잘 할수 있도록 만든 인터페이스  
서버는 클라이언트에게 리소스를 잘 활용할 수 있도록 인터페이스를 제공하는것  
API 를 잘 사용할수 있게 잘 디자인 해야된다  
    
<br/>
    
<br/>
  
#### HTTP API 디자인 예시
| Requests    | Method      |
| ----------- | ----------- |
| 조회 Read   | GET         |
| 추가 Create | POST        |
| 갱신 Update | PUT / PATCH |
| 삭제 Delete | DELETE      |