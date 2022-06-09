Google Java 개발 컨벤션 가이드
==============================

### 목적

구글의 자바 코딩 표준 가이드.

### 용어

-	class : 일반적인 모든 클래스. enum, interface, annotation을 포함
-	member : 내부 클래스, 필드, 메서드, 생성자 등 주석, 초기화 코드(Initializer)를 제외한 모든 항목
-	comment : /\*...\*/, //의 주석. /\*\*...\*/의 경우 본 문서에서는 Javadoc으로 표기하고 documentation comments 라는 용어로 혼용하지 않는다.

### 목차

1.	소스 파일 기본

	1.	파일 이름
	2.	파일 인코딩 : UTF-8
	3.	특수문자

2.	소스 파일 구조

	1.	라이선스와 저작권 정보
	2.	package 선언
	3.	import 선언
	4.	class 선언

3.	포맷

	1.	괄호
	2.	들여쓰기 : space 2번
	3.	한 줄에 한 문장
	4.	길이 제한 : 100자
	5.	줄 나누는 법
	6.	공백
	7.	괄호 묶기 : 추천
	8.	기타 특수 사

4.	명명 규칙

	1.	식별자 공통 규칙
	2.	식별자 종류별 규칙
	3.	카멜 케이스 : 구글 스타일 가이드 정

5.	프로그래밍 관행

	1.	@Override : 필수
	2.	예외 처리 : 건너뛰지 않음
	3.	정적 member : 클래스를 통한 사용
	4.	Finalizers : 사용 안 함

6.	Javadoc

	1.	포맷
	2.	요약 부
	3.	작성 예외 대상

소스 파일 기본
--------------

### 파일이름

파일 이름은 {유일한 소스 파일 내 최상위 클래스 명(대소문자 구분)}.java로 합니다.  
ex) 파일 내 최상위 클래스가 둘 이상인 경우 : X  
파일 내 최상위 클래스가 AbstractCode인 경우 : AbstractCode.java

### 파일 인코딩 : UTF-8

소스 파일은 UTF-8로 인코딩합니다.

### 특수문자

#### 공백

공백의 표준은 ASCII horizontal space character (0x20)을 사용하며 유일합니다. 즉

1.	다른 공백은 사용하지 않습니다.
2.	Tab을 들여쓰기(indentation) 용도로 쓰지 않습니다.

#### 특수 escape 시퀀스

\t, \n 등 문자로 사용할 수 있는 경우 문자를 사용한다. 즉 \012, \u000a와 같이 사용하지 않습니다.

#### ASCII 코드가 아닌 문자들

ASCII 지원이 되지 않는 경우 Unicode 문자를 쓰거나 Unicode escape을 사용합니다.(ex: ∞, \u221e)  
이 때 읽기 쉬운 쪽을 사용합니다.  
하지만 문자열 밖에서는 Unicode를 사용하지 않는 편이 좋습니다.

-	tip : Unicode escape을 사용하는 경우 주석으로 설명을 적으면 좋습니다
-	tip : 프로그램에서 Unicode를 번역하지 못해 오류가 발생할까 두려워하지 않아도 됩니다. 그건 그 프로그램이 수정되어야 한다는 뜻입니다.

소스 파일 구조
--------------

.java 파일 하나를 열었을 때 맨 윗줄부터 아래로 내려가면서 가이드 되어 있다고 생각하시면 됩니다.

### 라이선스와 저작권 정보

라이선스와 저작권 정보가 있다면 가장 먼저 씁니다.

### package 선언

줄을 나누지 않고 한 줄로 씁니다.

### import 선언

1.	wild card를 쓰지 않습니다.
2.	import문 역시 한 줄로 씁니다.
3.	static import만 한 묶음, non-static import만 한 묶음으로 정리합니다. 그 외에 다른 공백 line이 포함되지 않습니다.
4.	각각의 묶음 안에서 import된 class의 ASCII 정렬 순서를 따라 정렬합니다.(import선언들 자체를 ASCII 정렬한 것과는 다릅니다.)
5.	non-static class를 static import하지 않습니다.

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

3.	내용이 빈 괄호의 경우 K&R 스타일을 따를지는 자유입니다. 하지만 연속된 블럭을 가지는 문장의 경우 띄웁니다.(if, else / try, catch, finally)

4.	아래는 예시입니다.

```
return () -> {
  while (condition()) {
    method();
  }
};

return new MyClass() {
  @Override public void method() {
    if (condition()) {
      try {
        something();
      } catch (ProblemException e) {
        recover();
      }
    } else if (otherCondition()) {
      somethingElse();
    } else {
      lastThing();
    }
  }
};
```

### 들여쓰기 : space 2번

다음 block일 때마다 띄어쓰기 두 번으로 들여씁니다. block을 벗어날 때 같은 양만큼 돌아갑니다.

### 한 줄에 한 문장

한 문장이 끝나면 다음 줄로 내립니다.

### 길이 제한 : 100자

Unicode 단위로 한 줄에 100자를 넘게 작성하지 않습니다. 만약 100자를 초과하는 경우 다음 줄로 이어 작성합니다. 줄 나누는 법은 다음에서 설명합니다. 아래는 예외 사항입니다.

1.	Javadoc 내부의 URL이나 JSNI method reference가 긴 경우처럼 길이 제한을 지킬 수 없는 경우는 예외입니다.
2.	package선언과 import선언은 예외입니다.
3.	주석에 포함된, shell에 붙여넣어 써야 할 수 있는 명령문은 띄어쓰지 않습니다.

### 줄 나누는 법(Line-Wrapping)

줄을 나누는 정해진 공식은 없습니다. 100자를 넘지 않기 위해 나눌 수도 있고 작성자의 재량에 의해 나눌 수도 있습니다. 또한 메서드나 지역 변수를 생성하는 것이 줄을 나눌 필요를 줄여줄 수 있습니다.

1.	나누는 지점은 보다 상위에서 나눌 수록 좋습니다. 아래 사항도 준수합니다.

	1.	값을 할당하는 경우가 아니라면 연산자 같은 부분의 앞에서 줄을 나눕니다. 예를 들어 '.'(dot separator), '::'(메서드 참조), '&'(제너릭의 타입 제한), '|'(예외 처리 중 pipe)의 경우에 해당 기호의 앞에서 줄을 나눕니다.
	2.	'='이나 foreach문의 ':'와 같이 값을 할당하는 경우 기호 다음에 줄을 나누는 것이 일반적이나 기호 앞에서 줄을 나누어도 됩니다.
	3.	메서드나 생성자의 경우 뒤따르는 괄호('()')에는 붙여씁니다.
	4.	','는 앞 부분에 붙입니다.
	5.	lamda식에서 화살표(->) 다음은 절대 띄우지 않습니다. 단, 다음이 괄호 없이 한 문장으로 처리되면 띄울 수 있습니다.

```
  MyLambda<String, Long, Object> lambda =
    (String label, Long value, Object obj) -> {
        ...
    };

  Predicate<String> predicate = str ->
    longExpressionInvolving(str);
```

-	줄을 나누는 목적은 코드가 깔끔해지는 데 있습니다. 최대한 적게 나누려고 노력하지 않아도 됩니다.
-	줄을 나눈 뒤 최소 4개 이상의 띄어쓰기를 사용합니다. 더 많은 줄로 나뉜다면 더 깊이 들여써도 좋습니다.

### 공백

-	세로 공백

	-	클래스 내 Initializer, 생성자, 필드, 메서드, 내부 클래스, 등의 구분에는 한 줄을 띄워야 합니다. 다만 필드 사이에 논리적인 묶음을 형성하려는 경우 줄을 알맞게 띄워 쓸 수 있습니다. Enum의 경우는 아래에서 다룹니다.
	-	또는 위 항목에서 구분한 내용을 따릅니다.(소스파일 구조, import 선언)
	-	공백 행을 여러번 추가할 수 있지만 장려되거나 필수적이진 않습니다.

-	가로 공백

	-	if, for, catch 등의 예약어 뒤에 붙는 괄호를 분리할 때
	-	else, catch 등의 예약어 앞에 오는 괄호를 분리할 때
	-	여는 중괄호({)의 경우 다음 2가지를 제외하고

		-	특정 Annotation : ({a, b}) 등
		-	배열 선언 : String[][] = {{"hello"}};

	-	모든 이항, 삼항 연산자의 앞뒤에(연산자와 비슷한 역할을 하는 경우도 포함합니다.)

		-	연속적인 타입 제한의 & : <T extends A & B> 등
		-	여러 예외를 처리하기 위한 catch 블럭의 | : catch(AException | BException) 등
		-	foreach문의 :
		-	lambda식의 화살표 : (String str) -> str.length()
		-	메서드의 참조를 위한 :: 에서는 예외 : Object::toString
		-	. 연산자 : object.toString()

	-	,, :, ; 뒤에

	-	type casting을 위한 닫는 괄호 뒤에

	-	//의 앞뒤에(여기서는 여러번 띄어쓸 수도 있습니다.)

	-	변수 선언에서 타입 선언 뒤에 : List<String> list

	-	배열을 초기화할 때 {}의 안쪽에 : new int[] {5, 6} 혹은 new int[] { 5, 6 } 모두 허용

	-	type annotation과 [], ...의 사이에

-	가로 정렬(준수하지 않아도 됨)

	-	변수 선언 시 변수명을 보기 좋게 하기 위해 띄어쓰기를 허용합니다.

		-	예시 :

		```
		    -   String     x;
		    -   String[][] arrayX;
		```

### 괄호 묶기 : 추천

가독성을 높이기 위해 괄호로 식을 묶는 것을 추천합니다. 하지만 가독성인 높아지지 않거나 읽는 데 이미 문제가 없는 경우는 묶지 않아도 됩니다.

### 기타 특수 사례

-	Enum

	-	다음 상수를 선언하기 전 ',' 다음의 줄 띄어쓰기는 허용됩니다. 즉 다음은 허용됩니다.

	```
	    public enum Answer{
	        YES{
	              @Override public String toString(){
	                  return 'yes';
	              }
	          },


	        NO,
	        MAYBE
	    }
	```

	-	Enum 클래스 내에서 다른 메서드나 작성 자료가 없는 경우 배열처럼 선언해도 됩니다.

-	변수 선언

	-	한 문장에서 여러 변수를 선언하지 않습니다. int a, b; 와 같은 문장은 허용하지 않습니다.
	-	지역 변수는 사용되기 바로 직전에 선언합니다. 초기화가 있는 경우 선언 바로 직후에 초기화합니다.

-	배열

	-	배열 초기화를 블럭처럼 작성해도 됩니다.
	-	C에서처럼 배열을 선언하지 않습니다. String a[] 대신 String[] a를 사용합니다.

-	switch문

	-	다른 블럭처럼 switch문 안에서도 들여쓰기 2칸을 적용합니다.
	-	case가 return, break, continue, 예외 처리로 끝나지 않는 경우 // fall through 를 표시합니다.
	-	default에 아무 코드도 없더라도 default를 작성합니다. 하지만 Enum으로 모든 case를 관리해야 하는 경우 default를 작성하지 않고 IDE의 warning을 유도합니다.

-	annotation

	-	annotation은 반드시 한 번에 한 줄만 차지합니다. 한 행이 길어져도 line-wrapping하지 않습니다.
	-	변수가 없는 단일 annotation은 선언문 앞에 함께 쓸 수 있습니다. 이것은 변수의 경우도 마찬가지인데 특히 변수에서는 여러 annotation을 쓸 수도 있습니다.
		-	예시 : @Override public String hello()
		-	예시 : @Partial @Mock String str

-	주석

	-	Javadoc의 경우 아래를 참조하면 됩니다.
	-	블럭 형식으로 자유롭게 사용할 수 있습니다.

-	접근 제한

	-	아래 순서를 따릅니다.
		-	public protected private abstract default static final transient volatile synchronized native strictfp

-	Literal

	-	literal의 경우 대문자로 수식합니다. 예를 들어 10000000l 대신 10000000L을 사용합니다.

명명 규칙
---------

### 식별자 공통 규칙

### 식별자 종류별 규칙

### 카멜 케이스 : 구글 스타일 가이드 정

프로그래밍 관행
---------------

### @Override : 필수

### 예외 처리 : 건너뛰지 않음

### 정적 member : 클래스를 통한 사용

### Finalizers : 사용 안 함

Javadoc
-------

### 포맷

### 요약 부

### 작성 예외 대상
