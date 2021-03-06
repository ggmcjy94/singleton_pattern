# singleton
싱글톤 패턴

1. 개요
 - 싱글톤이란 어떤 클래스가 최초 한번만 메모리를 할당하고(static) 그 메모리에 객체를 만들어 사용하는 디자인 패턴을 의미한다.
 - 하나의 객체를 생성하면 생성된 객체를 어디서든 참조할 수 있지만 여러 프로세스가 동시에 참조할 수는 없음
 - 클래스 내에서 인스턴스가 하나뿐임을 보장하며, 불필요한 메모리 낭비를 최소화 할 수 있음
 
2. 사용 이유 
 - 한번씩 객체 생성으로 재 사용이 가능하기 때문에 메모리 낭비를 방지할 수 있다. 
   또한 싱글톤으로 생성된 객체는 무조건 한번 생성으로 전역성을 띄기에 다른 객체와 공유가 용이하다.
 - 객체를 하나만 생성한 뒤 공유해서 사용
 - 웹에서 자주 사용되는 기법 동시접속을 효율적으로 처리할 수 있음
 - 메모리 절약 목적

3. 문제점
 - 전역성을 띄면서 다른 객체와 공통으로 사용하는 경우와 같은 몇 가지 케이스에서만 사용할 때 효율적이며 그 외에는 문제점이 생길 수 있다.
 싱글톤으로 만든 객체의 역할이 간단한 것이 아닌 역할이 복잡한 경우라면 해당 싱글톤 객체를 사용하는 다른 객체간의 결함도가 높아져서 
 객체 지향 설계 원칙에 어긋나게 된다.
 (개방 - 폐쇄 open-closed principle *개방-폐쇄 원칙은 '소프트웨어 개체는 확장에 대해 열려 있어야 하고, 수정에 대해서는 닫혀 있어야 한다'는 프로그래밍 원칙이다)
 해당 싱글톤 객체를 수정할 경우 싱글톤 객체를 사용하는 곳에서 사이드 이팩트 발생 확률이 생기게 되며 멀티 쓰레드환경에서 동기화 처리 문제등이 생기게됌
 
 
4. 싱글톤 구현 6가지
 
 eager-initialization
 - 가장 간단한 형태의 구현 방법이다. 이는 싱글톤 클래스의 인스턴스를 클래스 로딩 간계에서 생성하는 방법이다.
 - 어플리케이션에서 해당 인스턴스를 사용하지 않더라도 인스턴스를 생성하기 때문에 자칫 낭비가 발생할 수 있음
 - 이 방법을 사용할 때는 싱글톤 클래스가 다소 적은 리소스를 다룰 때여야 함
 - File System, Database Connection 등 큰 리소스들을 다루는 싱그톤을 구현할 때는 위와 같은 방식보다는 getinstatnce() 메소드가 호출될 때까지
싱글톤 인스턴스를 생성하지 않는 것이 더 좋다.
 - 그리고 이방식은 exception handleing도 제공하지 않음
 
 static block initialization 
 -  Eager initialization 과 유사하지만 static block을 통해서 Exception Handling 에 대한 옵션을 제공함 
 -  예외에 대한 처리를 할 수 있지만 eager initialization 과 마찬가지로 클래스 로딩 단계에서 인스턴스를 생성하기 때문에 큰 리소스를 다루는 경우에는 적합하지 않게 된다.
 
 Lazy-initialization
 - 앞선 두방식과는 달리 나중에 초기화하는 방법
 - global access 한 getInstance() 메소드를 호출할 때에 인스턴스가 없다면 생성 
 - 이 방식으로 구현할 경우 앞선 두방식에 안고 있던 문제 (사용하지 않았을경우 인스턴스 낭비) 에 대해 어느 정도 해결책이 된다.
 - 허나 Lazy-initialization 방식은 multi-thread 환경에서 동기화문제가 발생한다.
 예를 들어 인스턴스가 생성되지 않은 시점에서 여러 쓰레드가 동시에 getInstance() 를 호출한다면 예상치 못한 결과를 얻을 수 있을뿐더러
 단 하나의 인스턴스를 생성한다는 싱글톤 패턴에 위반하는 문제점이 애기될 수 있다. 그렇게에 이방법으로 구현을 해도 괜찮은 경우는 single-thread 환경이 보장됐을 때이다.
 
Thread_Safe_Singleton
- Lazy-initialization을 해결하기 위한 방법으로 getInstance() 메소드에 synchronized를 걸어두는 방식이다.
- synchronized 키워드는 임계 영역을 형성해 해당 영역에 오직 하나의 쓰레드만 접근 가능하게 해준다.
- getInstance() 메소드 내에 진입하는 쓰레드가 하나로 보장받기 때문에 멀티 쓰레드 환경에서도 정상 동작하게 된다. 
- 그러나 synchronized 키워드 자체에 대한 비용이 크기 때문에 싱글톤 인스턴스 호출이 잦은 어플리케이션에서는 성능이 떨어지게 된다.
 그래서 고안된 방식이 double checked locking 이고 이는 getInstance() 메소드 수준에 lock을 걸지 않고 instance 가 null일 경우에만 synchronized 가 동작되는 방식이다.

Bill_Pugh_singleton_implementaion
- Bill Pugh 가 고안한 방식으러 inner static helper class 를 사용하는 방식이다.
- 앞선 방식이 안고 있는 문제점들을 대부분 해결한 방식으로 현재 가장 널리 쓰이는 싱글톤 구현 방법이다.
- class 내에 private static inner class 를 두어 싱글톤을 갖게 하는 방식이다.

Enum_Singleton
- 앞선 싱글톤 방식들은 완전히 안전할 수 없다 왜냐하면 java의 reflection 을 통해서 싱글톤을 파괴할 수 있기 때문이다,
 그래서 나온 싱글톤 방식이 enum으로 싱글톤을 구현한 방법이다.
- 그러나 이 방법 또한 eager-initialization, static block initialization과 같이 
 사용하지 않았을 경우의 메모리 문제를 해결하지 못한 것과 유연성이 떨어진다는 면에서의 한계를 지니고 있다.



