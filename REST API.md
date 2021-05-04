## REST API란? 
* 의미 : 클라이언트와 서버사이드 간의 REST 한 데이터 전송 
  * Resource의 표현에의 한 상태 전달 
```
ex) GET studentNumber/1
```
* REST API 의 개념
  * HTTP URI 를 통해 Resource 명시 -> Http Method (GET,POST,DELETE,PUT) 등을 통해 CRUD Operation를 적용   
```
ex) GET(**Method**) /student/Numbers/2 (**Resource**)
```
* CRUD Operation : 
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

## REST API의 주요작성 규칙 

* 1) resource는 동사보다는 명사를, 대문자보다는 소문자를 사용한다.
* 2) resource의 도큐먼트 이름으로는 단수 명사를 사용해야 한다.
* 3) resource의 컬렉션 이름으로는 복수 명사를 사용해야 한다.
* 4) resource의 스토어 이름으로는 복수 명사를 사용해야 한다.
```
GET /Member/1 -> GET /members/1
```

* HTTP Method의 역할
  * POST	: POST를 통해 해당 URI를 요청하면 리소스를 생성한다
  * GET : GET를 통해 해당 리소스를 조회합니다. 리소스를 조회하고 해당 도큐먼트에 대한 자세한 정보를 가져온다.
  * PUT : PUT를 통해 해당 리소스를 수정한다
  * DELETE : DELETE를 통해 리소스를 삭제한다.

* URI 설계시 주의 점 
1) 슬래시 구분자(/)는 계층 관계를 나타내는 데 사용
```
http://restapi.example.com/houses/apartments
http://restapi.example.com/animals/mammals/whales
```
2) URI 마지막 문자로 슬래시(/ )를 포함하지 않는다.
```
http://restapi.example.com/houses/apartments/ (X)
```
3) 하이픈(-)은 긴 URI의 가독성을 높이고자 할때 사용
4) 밑줄(_)은 사용하지 않는다.
  * 밑줄은 보기 어렵거나 밑줄 때문에 문자가 가려지기도 하므로 가독성을 위해 밑줄은 사용하지 않는다.
5) URI는 대소문자를 구분하기 때문에 가급적 소문자를 사용한다.
6) 파일 확장자는 URI에 포함하지 않는다.
  * REST API에서는 메시지 바디 내용의 포맷을 나타내기 위한 파일 확장자를 URI 안에 포함시키지 않는다.
```
ex) http://restapi.example.com/members/soccer/345/photo.jpg (X)
ex) GET / members/soccer/345/photo HTTP/1.1 Host: restapi.example.com Accept: image/jpg (O)
```
7) 리소스 간의 관계를 표현하는 방법
```
/리소스명/리소스 ID/관계가 있는 다른 리소스명
ex)    GET : /users/{userid}/devices (일반적으로 소유 ‘has’의 관계를 표현할 때)
```    

* REST API의 설계 예시
![image](https://user-images.githubusercontent.com/46732816/116965014-da033c00-ace7-11eb-82ec-9ac276927cf7.png)

* REST API의 응답상태 코드 
> 1xx : 전송 프로토콜 수준의 정보 교환  
> 2xx : 클라어인트 요청이 성공적으로 수행됨  
> 3xx : 클라이언트는 요청을 완료하기 위해 추가적인 행동을 취해야 함  
> 4xx : 클라이언트의 잘못된 요청  
> 5xx : 서버쪽 오류로 인한 상태코드  
- - -

**출처** : <https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html>
           [Network] REST란? REST API란? RESTful이란?
