# Yoyo Projet

음원공유 서비스

## 서비스 기획 & 설계

- Microservice Architecture 적용
- 트렌드를 경험해보자 대신, 수준에 맞춰서 기본적인 기능모두 경험해보자

## 인증서버 개발

node.js + express.js + sequelize + redis 로 개발
토큰기반의 인증을 기반으로 회원가입, 로그인, 로그아웃등 담당하는 서버

- jwt 토큰을 이용 하였음 (Json Web Token = Header + Payload + Signature) 
  - AccessToken , RefreshToken => RefreshToken을 redis에 저장. AccessToken 만료시, Redis에 RefreshToken 존재여부 체크. 
- 이메일 인증
- 비밀번호
- 처음 개발한 서버
- 비동기, 동기 고려 X , 많은 콜백 호출, ORM 고려 X, 의존성 고려 x

## 파일서버 개발

node.js + express.js 개발
가장 아쉬움이 많이 남았던 서버, 스트리밍서버로 맨처음에 개발을 했지만, 프론트간의 연결이 쉽지 않았음. 프론트를 해볼걸.
음악 , 이미지를 저장하는 static 서버 , 클러스터링 적용 

- 사용자별로 해싱을 통해 파일명, 디렉토리 관리
- 클러스터링 적용 

## 검색서버 개발

typescript(node.js) + express.js + typeORM

- 타입스크립트 도입.  --> 자바스크립트를 사용하면서 항상 불안했음. 비지니스 로직을 짤때는 더더욱. 에러처리 등에 있어서 신경을 써보고 싶었음
- 짜야하는 코드량이 조금더 늘어남 . 클래스 및 인터페이스 구현등이 늘어났음. 
- DTO, DAO 를 나누어 구현하고, 서버안에서 계층구조를 적용하였음
  - Controller, Service, Repository 3단계로 나누었음
- DB 접근시, blocking이 필요한건 당연하다. 데이터를 순차적으로 요청할경우, 계속해서 blocking이 되지 않을까 , 그럼 비효율적 일것 같다는 생각을 하였음 => 비동기처리가 필요한 함수들을 병렬처리가 가능하도록 코딩하였음

## API 서버 개발

Typescript + express.js + redis

- Front로부터 받은 요청에 따라 뒷단의 서버로 넘겨주는 역할을 수행

- 인증 토큰의 유효성을 API 서버에서 수행하도록 옮김

  - API 게이트웨이에서 유효성을 체크하고 뒷단의 서버로 넘겨주기 때문에, 다른서버에서 토큰의 유효성을 확인할 필요가 없어짐.
  - 만약, 다른서버에서 유효성을 확인하고 재발급을 요청할경우, 인증서버와 요청서버간의 blocking이 생길수 있음.
  - api 게이트웨이에서 유효성을 확인및 토큰 발행을 수행하므로, 뒷단의 서버에서는 자신의 할일들, 요청들만을 처리하면 됨. 서버간의 의존성 감소

  