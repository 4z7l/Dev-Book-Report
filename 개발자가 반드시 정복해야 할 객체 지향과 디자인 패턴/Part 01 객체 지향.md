# Part 01 객체 지향

## 001 절차 지향과 객체 지향

### 절차지향(Procedural Oriented)

- 프로시저들이 데이터를 조작하는 방식
- 단점 : 수정 시 함께 수정해야하는 프로시저 증가, 같은 데이터를 프로시저들이 서로 다르게 사용하는 경우 발생 → 수정 시 비용이 많이 듦

### 객체지향(Object Oriented)

- 데이터 및 프로시저를 **객체**단위로 묶음
- 단점 : 규모가 작을 때에는 절차지향보다 복잡한 구조를 가짐
- 장점
  - 데이터를 변경하더라도 다른 객체에는 영향을 주지 않음 → **캡슐화**
  - 수정이 용이하므로 변화된 요구사항을 빠르게 반영할 수 있음

## 002 객체

- 한 객체가 갖는 책임을 결정하는 것에서 객체 지향 설계가 시작됨
  - → SRP Single Responsibility 단일 책임 원칙

## 003 의존

- 의존

   : 한 객체의 코드에서 다른 객체를 

  생성

  하거나 다른 객체의 

  메소드를 호출

  하는 것, 혹은 파라미터에 필요한 경우

  - 의존하는 타입에 변경이 발생하면 나도 함께 변경될 가능성이 높음

- 만약 의존성에 순환 구조가 존재한다면 내 변경이 나한테 다시 영향을 미칠 수 있음

  - → DIP Dependency Inversion 의존 역전 원칙

## 004 캡슐화

- **캡슐화** : 한 곳의 변화가 다른 곳에 미치는 영향을 최소화할 수 있음
- 데이터를 직접적으로 사용하면 데이터의 변화에 직접적인 영향을 받음
- 캡슐화를 잘하면 기능 변경 시 **유연함**을 얻을 수 있음

> Tell, Don't Ask

- 데이터를 물어보지 말고 기능을 실행해달라고 말하라

```java
if(member.getExpiryDate() != null && member.getExpiryDate() < System.currentTimeMillis()) { ... }
```

보다는

```java
if(memver.isExpired())
```

를 써라!

> 데미테르의 법칙

1. 메소드에서 생성한 객체의 메소드만 호출하라
2. 파라미터로 받은 객체의 메소드만 호출하라
3. 필드로 참조하는 객체의 메소드만 호출하라

**EX)**

```java
if(member.getDate().getTIme() < ...) // 데미테르 법칙 위반
```

**데미테르 법칙을 지키지않는 전형적인 증상 두 가지**

1. 연속된 get 호출
2. 임시 변수의 get 호출이 많음

## 006 객체 지향 설계 과정

1. 제공할 기능을 찾고, 세분화하고, 객체에 할당한다.
2. 객체 간에 어떻게 메시지를 주고 받을 지 결정한다.
3. 1과 2를 반복한다.

## 007 상속

- **상속** : 한 타입을 그대로 사용하면서 구현을 추가할 수 있도록 해주는 방법
- Overriding : 하위 클래스가 상위 클래스의 메소드를 재정의함
- 인터페이스 상속 : 타입 정의만 상속 (인터페이스/추상 클래스 상속)
- 구현 상속 : 클래스 상속

## 008 다형성

- **다형성**(Polymorphism) : 한 객체가 여러 가지 모습을 가짐

## 009 추상화

- **추상화** : 데이터나 프로세스를 의미가 비슷한 개념이나 표현으로 정의하는 것
- 왜 사용하는가?
  - 변경의 유연함을 얻기 위해
  - 객체의 책임과도 관련있음. 객체가 여러 책임을 갖는 것을 방지할 수 있음

> Program to Interface(인터페이스에 대고 프로그래밍 하기)

- 콘크리트 클래스를 사용해서 프로그래밍하지 말고 **인터페이스**를 사용해서 프로그래밍하라
- 하지만, 인터페이스는 최초 설계에서 바로 도출되기 보다는, **요구 사항의 변화와 함께 점진적으로 도출됨**
- 그렇다고 모든 곳에서 인터페이스를 사용하는 것을 불필요한 복잡도만 증가시킴 → 변화 가능성이 높은 경우에만 사용

> 인터페이스는 사용자 입장에서

- interface를 사용하는 곳의 입장에서 작성해야함

## 010 인터페이스와 테스트

- 추상화를 잘 시킨다면 테스트도 용이함
- EX) 둘 이상의 개발자가 협업하여 각각의 클래스를 만든다면, 한 쪽의 개발이 아직 안끝났어도 잘 설계된 추상화로 Mock 객체를 통해 테스트 가능

## 011 상속을 통한 재사용의 단점

### 상위 클래스 변경의 어려움

- 상위 클래스가 변경되면 상위 클래스에 의존하고있는 하위 클래스에 영향이 갈 수 있음
- 최악의 경우 상위 클래스가 계층을 따라 모든 하위 클래스에 영향을 줄 수 있음

### 클래스의 불필요한 증가

- 상속을 통해 기능을 재사용하면 클래스는 계속 늘어나게 됨
- EX) `Storage`, `CompressedStorage`, `EncryptedStorage`, `CacheableStorage`, `CompressedEncryptedStorage`, `EncryptedCompressedStorage` 등등..

### 상속의 오용

- 상속 자체를 잘못 아용하는 경우
- IS-A 관계가 성립할 때에만 사용해야 함

## 012 조립을 이용한 재사용

- **객체 조립** : 여러 객체를 묶어서 더 복잡한 기능을 제공하는 객체를 만들어내는 것

**객체 조립의 장점**

1. 클래스의 불필요한 증가 방지 : 객체를 조립하는 방법을 이용한다면 새로운 기능이 요구되었을 때 새로운 기능만 조립해서 사용하면 되므로 불필요한 클래스 증가를 방지할 수 있음
2. 상속의 오용 방지 : 런타임 시의 변경을 포함해 상위 클래스의 변경을 용이하게 함

## 013 위임(delegation)

- **위임** : 내가 할 일을 다른 객체에게 넘김
- 보통은 객체 조립을 통해 위임할 객체를 필드로 가짐
- 위임은 내가 할 수 있는 일을 다른 객체에게 한번 더 요청하게 되므로 추가적인 메소드 호출이 필요함. 따라서 실행 시간을 다소 증가할 수 있다. 하지만 이 때 발생하는 미세한 성능 저하보다 위임을 통해 얻을 수 있는 **유연함과 재사용의 장점이 더 크다.**

## 014 그렇다면 상속은 언제 사용할까?

- 기능의 확장이라는 관점에서 사용해야 한다.
- IS-A 관계가 성립해야 한다.
- **EX)** View - TextView - Button - RadioButton
- "버튼은 UI 위젯이다"