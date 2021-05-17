# Part 03 DI와 서비스 로케이터

## 001 어플리케이션 영역과 메인 영역

- 메인 영역은 어플리케이션 영역의 객체를 **생성, 설정, 실행**함
- 어플리케이션 영역에서 사용한 하위 수준의 모듈을 변경하고 싶다면 메인 영역을 수정하면 됨
- **메인 영역→어플리케이션 영역**으로 의존이 존재함

## 002 DI(Dependency Injection)

- 사용할 객체를 직접 생성하는 경우 DIP를 위반함
- DI : 필요한 객체를 외부에서 삽입해주는 방식

## 003 DI 적용 방식 : 생성자

- 생성자를 통해서 의존성을 주입하는 것
- 장점 : 객체 생성 시점에 필요한 의존성을 모두 준비할 수 있음 → 한번 객체가 생성되는 객체가 정상적으로 동작함

```java
public class AClass{
	private BClass b;
	public AClass(BClass b){
		this.b = b;
	}
}
```

## 004 DI 적용 방식 : 설정 메소드

- 설정 메소드를 이용해 의존성을 주입하는 것
- 장점 : 의존 객체가 나중에 생성되는 경우 사용 가능, 의존 객체가 많은 경우 가독성을 높여줄수있음
- 단점 : NullPointerException 발생 가능성 존재

```java
public class AClass{
	private BClass b;
	public vois setBClass(BClass b){
		this.b = b;
	}
}
```

## 004 DI와 테스트

- DI 패턴을 따른다면 테스트가 용이함
- 의존 객체가 다 구현이 되지 않았더라도 임시로 구현해서 외부에서 주입해주면 됨

## 005 서비스 로케이터(Service Locator)

- 서비스 로케이터(Service Locator) : 사용할 객체를 제공하는 것
- 로케이터를 통해 필요로 하는 객체를 직접 찾는 방식

→ 외부에서 사용할 객체를 주입해주는 DI를 일반적으로 사용함

## 006 서비스 로케이터 적용 방식 : 객체 등록

1. 서비스 로케이터 생성 시 사용할 객체를 전달한다.
2. 서비스 로케이터 인스턴스를 지정하고 참조하기 위한 static 메소드를 제공한다.

```java
public class ServiceLocator{
	private AClass a;
	private BClass b;

	public ServiceLocator(AClass a, BClass b){..}
	public AClass getAClass() {..}
	public BClass getBClass() {..}
	
	private static ServiceLocator instance;
	public static void load(ServiceLocator locator){
		ServiceLocator.instance = locator;
	}
	public static ServiceLocator getInstance(){
		return instance;
	}
}
```

- 메인 영역 : `ServiceLocator.load()`해서 사용할 객체 전달
- 어플리케이션 영역 : `ServiceLocator.getInstance()`로 객체 사용

## 007 서비스 로케이터 적용 방식 : 상속

1. 객체를 구하는 추상 메소드를 제공하는 상위 타입 구현
2. 상위 타입을 상속받은 하위 타입에서 사용할 객체 설정

```java
public abstract class ServiceLocator{
	public abstract AClass getAClass();
	public abstract BClass getBClass();

	public ServiceLocator(){
		ServiceLocator.instance = this;
	}
	private static ServiceLocator instance;
	public static ServiceLocator getInstance(){
		return instance;
	}
}
public class MyServiceLocator extends ServiceLocator{
	private AClass a;
	private BClass b;

	public ServiceLocator(){
		super();
		this.a = new AClass();
		this.b = new BClass();
	}
	@Override
	public AClass getAClass() { return a; }
	@Override
	public BClass getBClass() { return b; }
}
```

## 008 서비스 로케이터 적용 방식 : 제네릭/템플릿

- 서비스 로케이터는 **인터페이스 분리 원칙**을 위반함
- 제네릭 프로그래밍을 하면 인터페이스를 분리한 것 같은 효과를 낼 수 있음

## 009 서비스 로케이터 단점

- 동일 타입의 객체가 다수 필요할 경우, 각 객체 별로 제공 메소드를 만들어야함
- 인터페이스 분리 원칙 위반함