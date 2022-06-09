Factory Method 패턴
===================

사용 목적
---------

팩토리 패턴은 한 줄 요약 - 상황에 따라 같은 슈퍼 클래스 하위의 다른 서브 클래스를 생성해야 할 때 - 생성할 객체를 기술하는 책임을 자신의 서브 클래스가 지정했으면 할 때

클래스 목록
-----------

-	상위 객체 클래스
-	서브 클래스들(생성 대상)
-	타입에 따라 서브 클래스를 상위 객체 클래스 이름으로 반환하는 Factory 클래스

구현
----

-	type에 해당하는 값을 넘겨받아 해당하는 서브 클래스를 반환하는 Factory 객체를 사용하는 형태
-	main에서 타입과 필요 사항을 지정하여 Factory 클래스 하위의 메서드 호출
-	이 때 Factory 객체를 Singleton으로 두거나 static method로 객체를 반환하도록 하여 Factory 객체 사용에 메모리 부담을 줄인다.

Abstract Factory 패턴
=====================

사용 목적
---------

팩토리 메서드 패턴도 결국 switch문에서 벗어나지 못하기에 새로운 Interface 구현체의 추가만으로 동작 가능하도록 Abstract Factory 패턴을 사용

클래스 목록
-----------

-	상위 객체 클래스
-	서브 클래스들(생성 대상)
-	서브 클래스 반환 메서드를 강제하는 Factory 기본 클래스
-	Factory 기본 클래스를 구현하는 각 서브클래스별 Factory 클래스
-	Factory 기본 클래스를 전달 받아 서브 클래스를 상위 객체 클래스 이름으로 반환하는 Factories의 Factory 클래스

구현
----

-	Factory 인터페이스를 받아 해당하는 서브 클래스를 반환하는 Factory 객체를 생성(Factories의 Factory)
-	Factory 인터페이스를 구현하여 각각의 SubClass를 반환하도록 하는 Factory Sub클래스
-	new FactorySubClass(생성정보)를 Factories의 Factory에 넘겨주어 서브 클래스 반환
-	의문 : 항상 new Factory를 전달?
