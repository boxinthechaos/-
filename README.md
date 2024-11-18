# 솔리드와 나머지
## 객체지향 설계과정
1. 요구사항을 찾고 세분화 한다 그리고 그 기능을 알맞은 객채로 할당한다
2. 기능을 구현하는 데에 필요한 데이터를 객체에 추가한다. 
3. 해당 데이터를 이용하는 기능을 구현한다. (기능은 최대한 캡슐화)
4. 객체 간에 어떻게 메소드 호출을 주고받을 지 결정한다.

## 객체지향 설계원칙
흔히 SOLID 라고 부르는 5가지 설계원칙이 존재한다.
### SRP (Single Responsibility) 단일 책임 원칙
- 클래스는 단 한개의 책임을 가져야 함
- 클래스를 변경하는 이유는 단 하나여야 함
- 이를 지키지 않으면, 한 책임의 변경에 의해 다른 책임과 관련된 코드에 영향을 미칠 수 있음 -> 저러면 유지보수가 구려진다

### OCP (Open-Closed) 개방-폐쇄 원칙
- 확장에는 열려있어야 하고, 변경에는 닫혀 있어야 함
- 즉, 기존의 코드를 변경하지 않고 기능을 수정하거나 추가할 수 있도록 설계해야 함
- 이를 지키지 않으면 instanceof 와 같은 연산자를 사용하거나, 다운 캐스팅 발생
개방 폐쇄 원칙을 잘 적용하여 기존 코드를 변경하지 않아도 기능을 새롭게 만들거나 변경할 수 있도록 해야 한다.

### LSP (Liskov Substitution) 리스코프 치환 원칙
- 하위 타입 객체는 상위 타입 객체에서 가능한 행위를 수행할 수 있어야 함
->즉, 상위 타입 객체를 하위 타입 객체로 치환해도 정상적으로 동작해야 함
- 상속관계에서는 꼭 일반화 관계 (IS-A) 가 성립해야 한다는 의미 (일관성 있는 관계인지)
- 상속관계가 아닌 클래스들을 상속관계로 설정하면, 이 원칙이 위배됨 (재사용 목적으로 사용하는 경우)

  ### ISP (Interface Segregation) 인터페이스 분리 원칙
  - 클라이언트는 자신이 사용하는 메소드에만 의존해야 한다는 원칙
  - 한 클래스는 자신이 사용하지 않는 인터페이스는 구현하지 않아야 함
  -> 하나의 통상적인 인터페이스보다는 차라리 여러 개의 세부적인 (구체적인) 인터페이스가 나음
  - 인터페이스는 해당 인터페이스를 사용하는 클라이언트를 기준으로 잘게 분리되어야 함
 
  ### DIP (Dependency Inversion) 의존 역전 원칙
  - 의존 관계를 맺을 때, 변하기 쉬운 것 (구체적인 것) 보다는 변하기 어려운 것 (추상적인 것)에 의존해야 함
  > -> 구체화된 클래스에 의존하기 보다는 추상 클래스나 인터페이스에 의존해야 한다는 뜻
  - 즉, 고수준 모듈은 저수준 모듈의 구현에 의존해서는 안 됨
  - 저수준 모듈이 고수준 모듈에서 정의한 추상 타입에 의존해야 함
  - 저수준 모듈이 변경되어도 고수준 모듈은 변경이 필요없는 형태가 이상적
  
## Spring의 Component
  
### @Component
클래스에 붙는 어노테이션으로 스프링 프레임워크에서 관리할 클래스임을 나타내는 것이다.

### Dependency
한국어로 하면 의존성으로 한클래스가 다른 클래스에 의존하고 있는것
EX)
```
Class A{
private class B;
```
A가 B를 의존하고 있다

### Component Scan
Component가 어디 있는지 위치를 알려주기 위한 것으로 자바 스프링 부트에서는 @ComponentScan("위치") 를 통해 위치 하위에 있는 모든 컴포넌트들을 스캔한다.
생략시 현재 컴포넌트를 기준으로 한다.

### DependenCy Injection
지역하면 의존성 주입으로 컴포넌트로 스캔된 클래스의 의존성이 무엇인지 확인하고 해당 의존성을 해결시켜주는 것이다. 이를 유저가 하는 것에서 스프링이 하는 것으로 바꾼 것을 IOC(Inversion Of Control)이라 한다.

### @Bean이랑 @Component의 차이
둘다 해당 어노테이션이 붙은 대상을 Bean에 등록시켜준다는 공통점이 있다.
|헤더|@Component|@Bean|
|:--------:|:-------:|:-------:|
|어디에 사용되는지?|자바클래스에 사용|빈에 등록시킬 클래스를 생성하는 함수에 사용되며 특히 Spring Configuration에서 클래스 생성 함수에 사용된다|
|사용 난이도|매우 쉬움(붙이기만 하면 된다)|상대적으로 어렵다(직접 하나하나 붙여야 한다, 생성 로직을 작성해야한다.)|
|Autowiring|@Autowired로 필드에 직접, 세터, 생성자로 주입 가능|클래스를 생성할 Bean에 등록시키고자 하는 클래스를 직접 생성하거나 파라미터로 받는다.|
|누가 만드는지Spring framework|생성자 빈을 직접 정의해야한다.|
|언제 쓰는게 좋을까?|직접 작성한 클래스를 빈에 등록시키려 할때|1. 의존성을 주입할때 특정 로직을 실행시켜주고 싶을때 2. 내가 만든게 아닌 다른 사람의 로직을 사용할때|
