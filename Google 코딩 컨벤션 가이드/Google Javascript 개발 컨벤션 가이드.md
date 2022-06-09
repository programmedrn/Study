Google Javascript 개발 컨벤션 가이드
==============================

### 목적

구글의 자바스크립트 코딩 표준 가이드.

### 용어

-	comment : 여기서 주석은 반드시 implementation comment로 문서화를 위한 주석을 이야기하지 않는다.
-	JSDoc : 문서화를 위한 주석. \/\*\*...\*\/
-	이 스타일 가이드는 RFC2119를 따른다. 예를 들어 "~하는 편이 좋다", "~하는 것은 피하라"는 각각 "해라", "하지마라"는 뜻이다.

### 목차

1.	소스 파일 기본

	1.	파일 이름
	2.	파일 인코딩 : UTF-8
	3.	특수문자

2.	소스 파일 구조

	1.	라이선스와 저작권 정보
	2.	@fileoverview JSDoc
	3.	goog.module 문
	4.	ES module
	5.	goog setTestOnly
	6.	goog require과 goog require Type 문
	7.	파일의 구현

3.	포맷

	1.	괄호
	2.	들여쓰기 : space 2번
	3.	한 줄에 한 문장
	4.	길이 제한 : 80자
	5.	줄 나누는 법
	6.	공백
	7.	괄호 묶기 : 추천
	8.	기타 특수 사용

4. 언어 특징
	1.	지역 변수 선언
	2.	배열 리터럴
	3.	객체 리터럴
	4.	클래스
	5.	함수
	6.	스트링 리터럴
	7.	수 리터럴
	8.	제어문
	9.	this의 사용하는
	10.	Equal 확인
	11.	허용되지 않는 기능
	
5.	명명 규칙

	1.	식별자 공통 규칙
	2.	식별자 종류별 규칙
	3.	카멜 케이스 : 구글 스타일 가이드 정의
	
6.	JSDoc
	1.	일반적인 형태
	2.	마크다운
	3.	JSDoc 태그	
	4.	줄 나누는 법
	5.	맨 위/파일 단위 주석
	6.	클래스 주석
	7.	Enum과 타입정의 주석
	8.	메서드와 함수 주석
	9.	프로퍼티 주석
	10.	타입 어노테이션
	11.	Visibility 어노테이션

7. 정책
	1.	이슈와 가이드 되지 않은 사항 : 일관성 유지
	2.	컴파일러 warning
	3.	Deprecation
	4.	구글 스타일에 제시되지 않은 코드
	5.	자체 코딩 스타일 적용
	6.	생성된 코드 : 대부분 면제

8. 덧붙임
	1.	JSDoc 태그 참조
	2.	대부분 잘못 이해하는 스타일 가이드
	3.	스타일 관련 도구
	4.	레거시 플랫폼을 위한 예외

소스 파일 기본
--------------

### 파일이름

파일 이름은 \_와 \-를 포함할 수 있는 소문자로 짓습니다.
.js로 끝납니다.

### 파일 인코딩 : UTF-8

소스 파일은 UTF-8로 인코딩합니다.

### 특수문자

#### 공백

공백의 표준은 ASCII horizontal space character (0x20)을 사용하며 유일합니다. 즉

1.	다른 공백은 사용하지 않습니다.
2.	Tab을 들여쓰기(indentation) 용도로 쓰지 않습니다.

#### 특수 escape 시퀀스

\t, \n 등 문자로 사용할 수 있는 경우 문자를 사용합니다. 즉 \012, \u000a와 같이 사용하지 않습니다.

#### ASCII 코드가 아닌 문자들

ASCII 지원이 되지 않는 경우 Unicode 문자를 쓰거나 Unicode escape을 사용합니다.(ex: ∞, \u221e)  
이 때 읽기 쉬운 쪽을 사용합니다.  
하지만 문자열 밖에서는 Unicode를 사용하지 않는 편이 좋습니다.

-	tip : Unicode escape을 사용하는 경우 주석으로 설명을 적으면 좋습니다
-	tip : 프로그램에서 Unicode를 번역하지 못해 오류가 발생할까 두려워하지 않아도 됩니다. 그건 그 프로그램이 수정되어야 한다는 뜻입니다.

소스 파일 구조
--------------

.js 파일 하나를 열었을 때 맨 윗줄부터 아래로 내려가면서 가이드 되어 있다고 생각하시면 됩니다.
모든 js 파일은 goog.module 파일이거나 ES module 파일입니다.

### 라이선스와 저작권 정보

라이선스와 저작권 정보가 있다면 가장 먼저 씁니다.

### @fileoverview JSDoc

만약 JSDoc 내용이 있다면 여기 위치합니다.
상세 사항은 6.5를 참조하세요.

### goog.module 문

모든 goog.module의 모듈은 한 줄 안에 단 하나의 goog.module 명을 선언해야 합니다.
이 선언문은 문장이 길어져도 줄을 나누지 않습니다. 그러니까 한 줄에 80자만 쓰도록 하는 규칙의 예외입니다.
EX : goog.mdoule('search.urlHistory.UrlHistoryService');
매개변수는 namespace와 같은 역할입니다.

1.	계층 구조
2.	goog.module.declareLegacyNamespace()의 사용
3.	goog.module 내보내기

### ES module

1.	import
	import문은 반드시 한 줄에 쓰여야 합니다. 줄을 나누지 않습니다.
		- ES module 파일은 다른 ES module을 불러올 때 import를 사용합니다. goog.require을 쓰지 않습니다. 이 떄 반드시 .js까지 명시합니다. 또한 같은 파일을 여러번 import하지 않습니다.
			1.	module을 import할 때 import \* as {name}의 경우 name은 lowerCamelCase로 작성합니다.
			2.	export된 내용을 import할 때 export 된 이름을 그대로 사용합니다. 즉 import MyClass from '.../....js'를 사용합니다.
			3.	이름이 겹치는 경우는 없는 것이 가장 좋지만 피할 수 없는 경우 상위 module에서 호출해 사용하거나 파일명과 연관지어 충돌을 피해 alias를 부여합니다. 즉 import {Cat as BigCat} from '.../BigAnimal.js' 와 같습니다.
2.	Export
	모듈 밖에서 사용할 일이 있을 때에만 export합니다. 
	
	5.	goog setTestOnly
	6.	goog require과 goog require Type 문
	7.	파일의 구현
### class 선언

1.	한 파일에는 단 하나의 최상위 클래스만 있습니다.
2.	그 외 정해진 규칙은 없지만 member를 선언할 때에는 클래스를 설명하는 데 용이한 논리가 있어야 합니다. 단지 추가 작성된 코드가 뒤에 붙으면 안 됩니다. 하지만 3번 항목은 지키면 좋습니다.
3.	Overloading 메서드가 여럿인 경우 이들은 띄우지 않고 붙여씁니다. 심지어 private member도 사이에 넣을 수 없습니다.

포맷
----

### 중괄호

1.	if, else, for, do, while 뒤에 문장이 비거나 한 문장만 있어도 괄호를 작성합니다.
2.	괄호는 Kernighan and Ritchie 스타일을 따르며 아래와 같습니다.

	1.	괄호를 열 때는 문장의 끝에 붙여서 씁니다.
	2.	괄호를 열고 난 뒤엔 다음 줄에 씁니다.
	3.	괄호를 닫을 땐 문장의 끝이 아닌 다음 줄에서 닫습니다.
	4.	괄호를 닫은 뒤 문장이 끝나지 않고 else, ','가 따르는 경우 다음 줄로 넘어가지 않고 붙여 씁니다.
