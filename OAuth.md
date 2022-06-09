OAuth
=====

#### 참조

-	http://homoefficio.github.io/2018/08/27/%EC%8A%A4%ED%8E%99%EB%94%B0%EB%9D%BC-%EB%A7%8C%EB%93%A4%EC%96%B4%EB%B3%B4%EB%8A%94-OAuth-1-0a-Client/ (OAuth 1.0a)
-	https://blog.naver.com/mds_datasecurity/222182943542 (OAuth 2.0)

### 목적

다른 곳에서 내가 권한이 있는 사용자란 걸 증명하기 위한 증명서 제공 프로토콜

### 주요 용어

-	Resource Owner : 사용자
-	Client : 증명서를 확인하려는 앱
-	Server : 증명서를 제공하는 앱
-	Client Credential : 증명을 요청한 Client에게 Server가 제공하는 정보. 즉 증명서
-	Temporary Credential : Client의 권한 부여 요청을 확인하고 Server가 발급한 임시 확인 정보
-	Token Credential : 권한을 부여받았음을 확인하고 Server가 발급한 접근 토큰 정보

#### OAuth 1.0과 1.0a의 차이

-	1.0은 커뮤니티 버전
-	용어에 차이가 있다.

	-	User -> Resource Owner
	-	Consumer -> Client
	-	Service Provider -> Server
	-	Consumer Key and Secret -> Client Credentials
	-	Request Token and Secret -> Temporary Credentials
	-	Access Token and Secret -> Token Credentials

		-	secret은 Service Provider가 Consumer에게 발급하고 각자 보유하여 사용할 뿐 전송하지 않는 정보

OAuth 1.0a
----------

### 개요

1.	송신자와 수신자가 암호화키를 공유
2.	송신자가 평문과 암호화키를 동시에 Hash하여 해시값(MAC)을 생성
3.	송신자가 평문가 MAC을 전송
4.	수신자가 평문에 자신이 갖고 있던 암호화키를 더해 Hash값 생성 후 전송받은 MAC과 비교

### User장군이 Service Provider 왕에게 Consumer을 보내 병사를 더 데려오라고 하는 이야기

#### 필요 절차

-	Consumer는 User로부터 요청을 받았음을 Service Provider에게 증명해야 함
-	User는 Service Provider가 모시는 왕이 맞음을 증명
-	User가 Consumer에게 권한을 부여했음을 증명

#### 절차 상세

1.	User가 Consumer에게 병력을 더 데려오라고 명령(동작 요청)
2.	Consumer 역시 장군이므로 자신의 신분을 증명하는 Signature 생성, Signature를 통해 User 휘하의 장군임을 증명
3.	Service Provider는 Consumer에게 출입증 Request Token을 제공
4.	Consumer는 Request Token을 들고 알현, User의 요청을 Service Provider에게 전달
5.	Service Provider가 User를 증명하라고 하면 증명한다.(로그인)
6.	정말 User의 명령이 맞는지 확인
7.	확인서 Verifier(Authorization code)를 작성하여 기록 및 전달
8.	User의 요청임을 확인하고 Consumer의 요청을 들어봄(Consumer의 요청을 승인하여 Consumer의 Callback화면으로 이동)
9.	Consumer는 자신의 신분 정보 Consumer Key, 출입증 Request Token, 확인서 Verifier 등에 Consumer Secret, Request Token Secret을 이용해 서명 Signature 생성
10.	Service Provider에게 Signature 전달하면 최종 병부 출입권한(요청 대상에 대한 Access Token)을 부여
11.	User의 병력 중 일부를 Consumer가 필요에 따라 접근(요청 처리)

**8번에서 요청을 승인 해줄 수 있지만 병부까지 전달하기 전 누군가에게 이야기가 새어나가(Local Storage나 Session에 데이터 남아 있을 수 있음) 병력을 선점할 수 있으므로 10번에서 증표 부여의 형태로 요청 승인**

#### 실제 절차

1.	consumer 사전 등록

	1.	Consumer -> Service Provider : Consumer로 등록
	2.	Service Provider -> Consumer : consumer key, consumer secret 발급

2.	User -> Consumer 요청

3.	Consumer -> Service Provider : 권한 부여 신청 요청(consumer key, callback uri, consumer secret으로 서명)

4.	Service Provider : 서명 검증

5.	Service Provider : Request Token, Request Token Secret 저장

6.	Service Provider -> Consumer : Request Token 발급

7.	Consumer : Request Token 저장

8.	User에게 권한 부여 화면 Redirecet

9.	User <-> Service Provider : 로그인

10.	User <-> Service Provider : 권한 부여 승인

11.	Service Provider -> User : Consumer의 callback uri로 Redirect, Verifier 전달

12.	Consumer -> Service Provider : Access Token 요청(consumer key, Request Token, Verifier를 Request Token Secret으로 서명)

13.	Service Provider : 서명 검증

14.	Service Provider : Request Token 폐기

15.	Service Provider : Access Token 저장

16.	Service Provider -> Consumer : Access Token 발급

17.	Consumer : Access Token 저장

18.	Consumer -> Service Provider : 자원 요청(Consumer Secret과 Access Token Secret으로 서명)

19.	Service Provider : 서명 검증

20.	Service Provider -> Consumer : 요청 받은 정보 반환

**실제 구현 했을 때 서명을 작성하는 방법이 어려웠다고. 제대로 작성했는지 확인하려면 전송을 시도할 수 밖에 없다고 한다.**

OAuth2.0
--------

### 개요

Oauth1.0에서 -Oauth1.0과 Oauth1.0a는 다르다. Oauth1.0a는 Oauth1.0에서 발생한 토큰 탈취 취약점을 Twitter에서 개선한 버전- 보안 이슈가 발생하자 Oauth의 개념과 사용성은 살리되 구조는 완전히 변경하여 다시 내놓은 프로토콜. 목적은 계정을 직접 공유해주지 않으면서 다른 앱의 사용권한을 허용해주기 위함이다.

### 주요 용어

-	ResourceOwner : 현재 사용하는 앱과 다른 앱에 사용 권한을 가진 주인. 즉 인간 사용자.
-	Client : Oauth를 사용하여 서비스 접근을 하고자하는 주체. ThirdParty App. ResourceOwner가 접근중인 앱.
-	AuthorizationServer : 권한 인증/인가를 수행하는 서버. Client의 접근 자격을 확인하고 AccessToken을 발급.
-	ResourceServer : ResourceOwner의 자원을 소유하고 있는 서버. Oauth1.0에서는 AuthorizationServer와 합쳐져 있었지만 2.0에서 분리됨.
-	Authentication : 접근 자격이 있는지 검증하는 단계.
-	Authorization : 자원에 접근할 권한을 부여하는 단계.
-	AccessToken : 자원에 접근할 자격 증명. Authorization의 대가로 주어짐.
-	RefreshToken : AccessToken은 만료기간이 있으므로 만료 기간 후 재발급 받기 위해 사용하는 Token. 역시 만료기간이 있으나 AccessToken보다 긴 것이 일반적.

### Authorization 방식

Authorization 방식엔 4가지가 있다.

-	Authorization Code Grant : 권한 부여 승인 코드. 코드를 사용하는 방식. code의 안전한 관리가 필요

	1.	ResourceOwner -> Client : 기능 사용 요청
	2.	Client -> AuthorizationServer : 권한 부여 승인 코드 요청(RedirectUrl, ClientId, response_type=code)
	3.	Client -> ResourceOwner : AuthorizationServer 인가를 위한 페이지로 이동(Oauth Login 페이지로 이동)
	4.	ResourceOwner -> AuthorizationServer : 로그인
	5.	AuthorizationServer -> Client : 이전에 요청한 대로 User의 로그인 요청이 들어왔으니 확인하여 Client에게 code 전달
	6.	Client -> AuthorizationServer : code 사용하여 AccessToken 요청(ClientId, ClientSecret, RedirectUrl, grant_type=authorization_code, code)
	7.	AuthorizationServer -> Client : AccessToken 전달
	8.	Client -> ResourceServer : 자원 요청(AccessToken 사용)
	9.	ResourceServer -> Client : 자원 전달
	10.	Client -> ResourceOwner : 자원 표출

-	Implicit Grant : 암묵적 승인. 코드를 사용하지 않고 바로 AccessToken을 제공하는 방식. code의 안전한 관리가 어려운 Javascript환경 등에서 사용. RefreshToken 사용이 불가.

	1.	ResourceOwner -> Client : 기능 사용 요청
	2.	Client -> AuthorizationServer : 권한 부여 승인 요청(RedirectUrl, ClientId, response_type=token). ResourceOwner가 로그인할테니 response_type을 응답해달라는 요청.
	3.	Client -> ResourceOwner : AuthorizationServer 인가를 위한 페이지로 이동(Oauth Login 페이지로 이동)
	4.	ResourceOwner -> AuthorizationServer : 로그인
	5.	AuthorizationServer -> Client : 이전에 요청한 대로 User의 로그인 요청이 들어왔으니 확인하여 Client에게 AccessToken 전달
	6.	Client -> ResourceServer : 자원 요청(AccessToken 사용)
	7.	ResourceServer -> Client : 자원 전달
	8.	Client -> ResourceOwner : 자원 표출

-	Resource Owner Password Credentials Grant : 자원 소유자 자격 증명 승인. username, password로 자격 요청하므로 보안이 취약하여 Client-AuthorizationServer 구간에 신뢰가 았을 때만 사용.

	1.	ResourceOwner -> Client : 기능 사용 요청(username, password)
	2.	Client -> AuthorizationServer : AccessToken 요청(RedirectUrl, ClientId, response_type=token)
	3.	AuthorizationServer -> Client : 확인하여 Client에게 AccessToken 전달
	4.	Client -> ResourceServer : 자원 요청(AccessToken 사용)
	5.	ResourceServer -> Client : 자원 전달
	6.	Client -> ResourceOwner : 자원 표출

-	Client Credentials Grant : 클라이언트 자격 증명 승인. RefreshToken은 없음. Clien 스스로가 ResourceOwner인 경우

	1.	Client -> AuthorizationServer : AccessToken 요청(grant_type=client_credentials)
	2.	AuthorizationServer -> Client : AccessToken 전달
	3.	Client -> ResourceServer : 자원 요청(AccessToken 사용)
	4.	ResourceServer -> Client : 요청된 자원 전달

### 주요 Parameter

-	response_type : 권한 부여 요청 시 권한 부여 방식 결정

	-	code(Authorization Code Grant)
	-	token(Implicit Grant)

-	redirect_url : 권한 서버가 응답을 보낼 url 지정

-	state : CSRF 공격에 대응하기 위한 임의 문자열로 AuthorizationServer에서 요청에 포함된 내용을 받으면 같은 값을 응답에 포함해야 함. 필수는 아님.

-	grant_type : AccessToken 요청 시 포함되는 값으로 권한 부여 방식을 결정.

	-	authorization_code(Authorization Code Grant)
	-	password(ResourceOwner Password Credentials Grant)
	-	client_credentials(Client Credentials Grant)

-	Authorization Basic : Client 자격 증명 데이터로 base64(client_id:client_secret)으로 생성

### 보안 위협

-	공격자가 RedirectUrl을 공격자의 서버로 변조하면 Token 탈취 위험. Client가 AuthorizationServer 등록 시 제공한 RedirectUrl과 실제 요청에 사용된 Url 비교. ClientIp 기억하여 요청 Ip 필터링 등.
-	HTTPS 위에 작성된 Protocol로 L4에서 안전하나 L5에서는 위협이 존재. 별도의 암호화 방식을 적용하여 해결이 가능.
