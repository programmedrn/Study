MVC 패턴
========

구성요소
--------

-	Model : 데이터와 데이터 처리부
-	View : UI만을 표현
-	Controller : Action(사용자의 입력)을 받아 처리하는 부분

흐름
----

1.	Action이 Controller로 들어옴
2.	Controller가 Action 식별하고 Model 업데이트
3.	Controller에서 Model을 표현할 View 선택
4.	View가 Model을 UI에 표현
	-	View가 Model을 이용해 직접 업데이트 하는 방법
	-	Model에서 View에 Notify하여 업데이트 하는 방법
	-	View가 주기적인 Polling으로 업데이트 하는 방법

특징
----

-	Controller는 View를 선택하기만 하고 업데이트는 View에서 담당(View는 Controller를 알 수 없음)
-	Controller는 여러 View를 선택하는 1:n 구조

장점
----

단순하고 보편적인 디자인

단점
----

View와 Model 사이 의존성이 높아 규모가 클 경우 유지보수가 어려움

MVP 패턴
========

구성요소
--------

-	Model : 데이터와 데어터 처리부
-	View : 사용자에게 보여지는 UI
-	Presenter : View에서 요청한 정보로 Model 가공하여 View에 전달

흐름
----

1.	Action은 View에 입력
2.	View가 Presenter에 요청
3.	Presenter는 Model에 요청
4.	Model 응답
5.	Presenter 응답
6.	View에서 화면에 표출

특징
----

Presenter와 View가 1대1 관계

장점
----

View와 Model 의존성이 사라짐

단점
----

View와 Presenter의 의존성이 커짐

MVVM
====

구성요소
--------

-	Model : 데이터와 데이터 처리부

-	View : UI 부분

-	View Model : View를 위한 모델. View에 맞게 데이터 처리도 담당함

흐름
----

1.	Action은 View로 입력

2.	Command 패턴으로 ViewModel에 전달

3.	View Model은 Model에 데이터 요청

4.	Model 응답

5.	View Model이 가공하여 데이터 저장

6.	View가 ViewModel과 DataBinding하여 표출

특징
----

Command패턴과 DataBinding패턴 사용 View와 ViewModel이 1대1 관계

장점
----

View와 Model 사이 의존성 없음 View와 ViewModel 사이 의존성 없음 모듈화하여 개발 가능

단점
----

ViewModel의 설계가 어려움

커맨드패턴
----------

요청을 객체화하여 사용

### 구조

-	Invoker(호출자) : 명령 발생자, 버튼
-	Command(인터페이스) : Invoker가 바라보는 인터페이스. 하위 execute 실행
-	Reciever(수신자) : 명령을 수행할 객체
-	ConcreteCommand(실제 명령) : 인터페이스를 구현한 실제 명령. 수신자 하위에 작성

참조
====

-	https://gmlwjd9405.github.io/2018/07/07/command-pattern.html
-	https://beomy.tistory.com/43
