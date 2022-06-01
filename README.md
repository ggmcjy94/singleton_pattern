# singleton
싱글톤 패턴

1. 개요
 - 싱글톤이란 어떤 클래스가 최초 한번만 메모리를 할당하고(static) 그 메모리에 객체를 만들어 사용하는 디자인 패턴을 의미한다.
 
2. 사용 이유 
 - 한번씩 객체 생성으로 재 사용이 가능하기 때문에 메모리 낭비를 방지할 수 있다. 
   또한 싱글톤으로 생성된 객체는 무조건 한번 생성으로 전역성을 띄기에 다른 객체와 공유가 용이하다.

3. 문제점
 - 전역성을 띄면서 다른 객체와 공통으로 사용하는 경우와 같은 몇 가지 케이스에서만 사용할 때 효율적이며 그 외에는 문제점이 생길 수 있다.
 싱글톤으로 만든 객체의 역할이 간단한 것이 아닌 역할이 복잡한 경우라면 해당 싱글톤 객체를 사용하는 다른 객체간의 결함도가 높아져서 
 객체 지향 설계 원칙에 어긋나게 된다.
 (개방 - 폐쇄 open-closed principle *개방-폐쇄 원칙은 '소프트웨어 개체는 확장에 대해 열려 있어야 하고, 수정에 대해서는 닫혀 있어야 한다'는 프로그래밍 원칙이다)
 해당 싱글톤 객체를 수정할 경우 싱글톤 객체를 사용하는 곳에서 사이드 이팩트 발생 확률이 생기게 되며 멀티 쓰레드환경에서 동기화 처리 문제등이 생기게됌
 
 
4. 싱글톤 구현 6가지
 1. eager-initialization
 - 가장 간단한 형태의 구현 방법이다. 이는 싱글톤 클래스의 인스턴스를 클래스 로딩 간계에서 생성하는 방법이다.
 - 어플리케이션에서 해당 인스턴스를 사용하지 않더라도 인스턴스를 생성하기 때문에 자칫 낭비가 발생할 수 있음
 - 이 방법을 사용할 때는 싱글톤 클래스가 다소 적은 리소스를 다룰 때여야 함
 - File System, Database Connection 등 큰 리소스들을 다루는 싱그톤을 구현할 때는 위와 같은 방식보다는 getinstatnce() 메소드가 호출될 때까지
싱글톤 인스턴스를 생성하지 않는 것이 더 좋다.
 - 그리고 이방식은 exception handleing도 제공하지 않음
 
 2. static block initialization 
 - 
 
5. 싱글톤 구현 
 - 위 코드 확인
