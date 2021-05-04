## REST API란? 
* 의미 : 클라이언트와 서버사이드 간의 REST 한 데이터 전송 
  * Resource의 표현에의 한 상태 전달 
  * ex) GET studentNumber/1

* REST API 의 개념
  * HTTP URI 를 통해 Resource 명시 -> Http Method (GET,POST,DELETE,PUT) 등을 통해 CRUD Operation를 적용   
  * ex) GET(**Method**) /student/Numbers/2 (**Resource**)

*CRUD Operation : 
* Create : 생성(POST)
* Read : 조회(GET)
* Update : 수정(PUT)
* Delete : 삭제(DELETE)
* HEAD: header 정보 조회(HEAD)

## REST API의 사용 이유?
* 1. 클라이언트 서버 간의 분리 가능 (클라이언트와 서버간의 의존성↓)
* 2. 무상태성을 가짐 (Client의 context를 Server에 저장하지 않는다. 다시 말해 세션이나 쿠키 등 context정보를 담지않아 구현이 단순해짐)
* 3. 캐시처리 가능 (HTTP 웹 표준을 이용함으로써 HTTP의 기능인 캐싱 기능 적용 가능 -> Cache-Control, ETag 등을 사용하여 REST API 의 네트워크 레이턴시 감소를통해 속도향상 가능)
* 4. 계층화구조 (REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 한다)
* 5. 인터페이스 일관성 (HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능)
