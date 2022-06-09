# AngularJS

## 버전
-

## 개요
- VDOM을 관리하는 Frontend 프레임워크로 Data(Model)과 화면의 동적 연결을 지원

## 시작
- angular.min.js 파일을 최초 html의 header 등에서 load 하여 실행
- 이후 ng-app 속성을 태그에 부여하여 해당 태그 하위에 angular app(모듈)을 로드하여 사용
  - 구동시 필요한 다른 모듈, 서비스들을 함께 지정할 수 있음

## 데이터 연동
- ng-model, ng-bind 등의 태그를 활용하여 html 상의 데이터를 module 내 데이터와 연동
- 양방향 데이터 바인딩으로 양쪽의 데이터 변화가 양쪽에 영향을 끼침
- {{}}의 콧수염 문법을 사용

## 요소
#### Directive
- 기존 html 요소에 새로운 효과를 부여하기 위한 표식
- angular.module.directive를 통해 새 directive의 선언도 가능
- ng-init, ng-bind 등

#### Scope
- 데이터가 바인딩되는 기준
- scope는 하위 scope를 가질 수 있으며 하위 scope에서 상위 scope의 데이터는 $scope.$parent 등으로 접근
- rootScope로 불리는 최상위 scope가 있음
- 각 scope하위 필요한 데이터를 저장
- 다른 scope간 데이터 교환은 service를 통하거나 broadcast(이벤트) 사용
  - rootScope의 broadcast를 사용할 때와 그 하위 scope에서 broadcast할 때 효과가 서로 다름에 주의
  - 위젯간의 데이터 교환 등에 많이 사용

#### Service
- 기능 단위의 묶음
- singleton처럼 사용
- 데이터의 변환, 동작의 수행 등을 제공
- Controller에서 사용 전 import해야함
  - import 방식은 함수 정의시 해당 service이름을 매개변수로 넣는 것

#### Factory
- 서비스와 유사한 기능 제공
  - 주로 정적인 영역을 담당
- Object를 return
  - 내부 로직이 숨겨짐

#### Provider
- get 방식을 통해 객체를 return
  - Service는 Factory로, Factory는 Provider로 구현돼 있음
    - https://mobicon.tistory.com/329
- app.config에 service가 넘겨질 수 있는 유일한 형태로 환경설정이 가능

#### Filter
- 데이터의 형식, 간단한 구별 등을 위해 사용할 수 있는 기능
- 표현의 변화를 주기에 용이

#### CSS
- 필요한 css를 적절히 로드
- angular js에서 동적으로 태그의 클래스, id 등을 변경할 수 있음
  - 태그에 ng-class 속성 사용

#### Controller
- 여러 서비스, 함수, 데이터를 저장하고 scope를 관리하는 단위로 사용
- 모듈-controller의 조합으로 AngularJS의 영역-로직을 설정

### service
- angular-ui-route : ng-app="모듈명" 으로 쓰던 문법 대신 ui-view라는 문법으로 대체할 수 있도록 지
  - 해당 상태마다의 template-js 등의 연결을 정의해둬야 함
