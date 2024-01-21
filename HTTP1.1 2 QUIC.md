```timestamp-url 
 [[10분 테코톡] 🎃손너잘의 HTTP1.1, HTTP2, 그리고 QUIC (youtube.com)](https://www.youtube.com/watch?v=ZgSC5K1sUYM)
 ```

### TCP

```timestamp
3:06
```
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_4d482d55-924b-4213-80b4-625ce7d0443a.jpeg)

#### 신뢰성 확보하기 위한 3way handshake
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_5a319522-2834-45c8-8071-af592a99a1c3.jpeg)

### HTTP 1.0 
한번의 요청, 한번의 커텍션 수립 -> 엄청난 레이턴시 발생

#### 느린 전송
처음 연결이 수립된 TCP 커넥션이 전송할 수 있는 최대 데이터 한계 설정 -> 여러번 데이터 송수신 하면서 송수신할 수 있는 최대 데이터 크기를 찾아나가는 과정

느린 전송으로 인한 데이터 송수신 여러번 발생 + 매요청마다 커넥션 연결 수립 과정 -> 엄청난 레이턴시 발생

#### TCP 커넥션 재활용
HTTP1.0 Keep-alive 헤더 : 한 번 송수신에 TCP 커넥션 끊어버리는 게 아닌 다음 연결에 재활용
표준 X (멍청한 프록시 문제)

### HTTP 1.1
```timestamp
6:08
```
```timestamp

```
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_f0842848-b83d-473b-bb52-2593b75be432.jpeg)
지속 커넥션: TCP 커넥션 재활용 가능
요청과 응답의 순서가 일치해야 됨 -> 동기적으로 요청 응답 -> 레이턴시

#### 파이프라이닝
서버에 대기큐 생성 -> 요청 순서에 따라 응답(나중에 보낸 요청에 대한 처리가 끝나더라도 이전 요청처리 완료되지 않는 다면 대기: Head of Line Blocking)

##### 단점
- Head Of Line Blocking
- 서버에서 병렬 처리 시 응답을 메모리에 적재하고 있어야 함(리소스 낭비)
- 응답 실패 시 클라이언트가 모든 리소스에 대한 요청을 처음부터 다시 보내야 함
- 중계자 존재 시 파이프라인 호환성 문제
- 중복 헤더(헤더의 상당 부분 중복 존재: 매번 요청 때마다 중복된 헤더 송신 낭비)

#### 멀티플렉싱
HTTP1.1: TCP 커넥션 여러 개 생성(요청 병렬적으로 처리)
```timestamp
9:54
```
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_f5b572f7-75b0-4959-867a-b2cad794efd7.jpeg)

#### HTTP 2.0
HTTP2.0:  TCP 커넥션 여러개 생성X 단일 커넥션 안에서 여러 개의 데이터가 썩이지 않게 보내는 기법
```timestamp 
 11:31
 ```

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_344b2bd7-8831-428c-8e72-cd4552f75515.jpeg)

```timestamp 
 12:10
 ```
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_15665598-bd3f-4cd8-8420-d0665a64d67a.jpeg)
식별자가 붙은 상태에서 하나의 TCP 커넥션 위에서 전송

##### 바이너리 프레이밍
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_f1a24e48-89f7-4ed6-969f-05a3a92b553c.jpeg)
텍스트(HTTP 1.1) -> 바이너리 파일
바이너리 파일 -> 여러 개의 프레임 나누어서 멀티플렉싱


#### 단점
- TCP 프로토콜 자체 발생하는 HOL 문제(TCP 프로토콜 전송되는 데이터 순서 보장: 앞에 데이터 병목 발생 시 뒤에도 병목 발생)
-

#### QUIC
UDP 기반 신뢰성 보장 새로운 프로토콜 개발
##### 독립 스트림
하나의 커넥션 안에 여러개의 독립적인 스트림(하나의 스트림 문제 발생시 다른 스트림 사용)
```timestamp 
 15:01
 ```
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_eb6c6477-3f15-4b49-ae6c-500c7dd0da7e.jpeg)
하나의 스트림에 데이터들이 엮인 상태에서 전송
