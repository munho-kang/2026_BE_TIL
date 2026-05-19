# 📘 Today I Learned

### 1. 오늘 배운 내용

- 상속, 오버라이딩, 다형성 개념 
- 추상 클래스와 추상 메서드 개념 
- 인터페이스와 default 메서드
- Animal을 부모 타입으로 두고 Rabbit, Seal, Snake 클래스를 다루는 실습
- Playable 인터페이스를 만들어 play() 기능을 추가하는 실습
- 실습2 STEP3에서 코드 에러가 발생하는 이유 정리
- Java 컬렉션 프레임워크 중 List, Map에 대해 추가로 학습

---

### 2. 핵심 정리 (내 언어로)

1. 상속
   상속은 공통되는 속성이나 기능을 부모 클래스에 모아두고, 자식 클래스가 이를 물려받아 사용하는 구조이다.
   예를 들어 Rabbit, Seal, Snake가 모두 동물이라면 Animal이라는 부모 클래스로 묶을 수 있다.

   이렇게 하면 동물들이 공통으로 가져야 하는 feed(), clean() 같은 기능을 기준으로 코드를 작성할 수 있다.

2. 오버라이딩
   오버라이딩은 부모 클래스에 있는 메서드를 자식 클래스에서 상황에 맞게 다시 구현하는 것이다.
   Animal에는 feed()라는 공통 메서드가 있지만,
   Rabbit은 토끼에게 당근을 준다, Seal은 물개에게 생선을 준다처럼
   같은 메서드 이름이어도 실제 실행 내용은 다르게 만들 수 있다.

3. 다형성
   다형성은 하나의 부모 타입으로 여러 자식 객체를 다룰 수 있는 것이다.

```java

Animal[] animals = {
    new Rabbit(),
    new Seal(),
    new Snake()
};

for (Animal animal : animals) {
    animal.feed();
    animal.clean();
}

```

   위 코드에서 배열의 타입은 Animal이지만 실제 들어있는 객체는 Rabbit, Seal, Snake이다.
   반복문에서는 Animal 타입으로 꺼내지만, 실행되는 메서드는 각 클래스에서 오버라이딩한 내용으로 실행된다.

   이 구조를 사용하면 동물이 여러 마리여도 하나씩 따로 호출하지 않고 반복문으로 처리할 수 있어서 코드가 더 깔끔해진다.

4. 추상 클래스
   추상 클래스는 직접 객체를 만들기 위한 클래스라기보다는, 자식 클래스들이 반드시 구현해야 하는 공통 규칙을 정해두는 클래스이다.

```java

abstract class Animal {
    abstract void feed();
    abstract void clean();
}

```
   이렇게 작성하면 Rabbit, Seal, Snake 클래스는 feed(), clean() 메서드를 반드시 오버라이딩해야 한다.
   만약 구현하지 않으면 컴파일 에러가 발생하기 때문에, 필요한 기능을 빼먹지 않도록 도와준다.

5. 인터페이스
   인터페이스는 클래스가 어떤 기능을 할 수 있는지 역할을 정해주는 것이다.
   예를 들어 Playable 인터페이스는 "놀 수 있는 기능"을 의미한다.

```java

interface Playable {
    void play();

    default void rest() {
        System.out.println("잠시 쉰다");
    }
}

```
   인터페이스의 메서드는 기본적으로 public abstract이기 때문에 보통 생략해서 작성한다.
   default 메서드를 사용하면 인터페이스 안에서도 기본 동작을 만들 수 있다.

6. 실습2 STEP3에서 에러가 발생하는 이유
   실습2 STEP3에서는 Animal 배열을 반복하면서 feedAnimal, cleanCage뿐만 아니라 Playable의 기능까지 실행하려고 했다.

```java

for (Animal animal : animals) {
    animal.feed();
    animal.clean();
    animal.play(); // 에러 발생
}
```
   여기서 에러가 발생하는 이유는 반복문에서 꺼낸 animal 변수의 타입이 Animal이기 때문이다.
   실제 객체가 Rabbit, Seal, Snake이고 이 클래스들이 Playable을 구현했더라도, 컴파일러는 animal을 Animal 타입으로 판단한다.

   그런데 Animal 클래스에는 play() 메서드가 없다.
   play()는 Playable 인터페이스에 있는 기능이기 때문에 Animal 타입 변수로 바로 호출할 수 없다.

   즉, 객체의 실제 타입과 변수를 선언한 타입이 다를 수 있고,
   자바는 메서드를 호출할 수 있는지 확인할 때 변수의 타입을 기준으로 판단한다.

   해결하려면 Playable 타입인지 확인한 뒤 형변환해서 사용해야 한다.

```java

for (Animal animal : animals) {
    animal.feed();
    animal.clean();

    if (animal instanceof Playable playable) {
        playable.play();
    }
}

   또는 play() 기능만 따로 실행하고 싶다면 처음부터 Playable 배열이나 List로 관리할 수도 있다.

Playable[] playables = {
    new Rabbit(),
    new Seal(),
    new Snake()
};

for (Playable playable : playables) {
    playable.play();
}

```

7. List
   List는 여러 데이터를 순서대로 저장할 때 사용하는 컬렉션이다.
   배열과 비슷하지만 배열처럼 크기가 고정되어 있지 않아서 데이터를 추가하거나 삭제하기 더 편하다.

```java

List<Animal> animals = new ArrayList<>();
animals.add(new Rabbit());
animals.add(new Seal());
animals.add(new Snake());

for (Animal animal : animals) {
    animal.feed();
    animal.clean();
}

```
   실습에서 사용한 Animal[] 배열은 처음부터 크기를 정해줘야 한다.
   하지만 List는 add()를 사용해서 객체를 계속 추가할 수 있기 때문에, 동물 수가 바뀔 수 있는 상황에서는 List가 더 적합하다고 느꼈다.

8. Map
   Map은 key와 value를 한 쌍으로 저장하는 컬렉션이다.
   List가 데이터를 순서대로 저장한다면, Map은 이름표를 붙여서 데이터를 저장하는 느낌이다.

```java

Map<String, Animal> animalMap = new HashMap<>();
animalMap.put("rabbit", new Rabbit());
animalMap.put("seal", new Seal());

Animal rabbit = animalMap.get("rabbit");
rabbit.feed();

```
   예를 들어 전체 동물에게 먹이를 주는 기능은 List로 반복문을 돌리는 것이 편하다.
   반대로 rabbit이라는 이름으로 특정 동물을 바로 찾고 싶을 때는 Map을 사용하는 것이 더 적합하다.

   그래서 List와 Map은 단순히 외우는 개념이 아니라, 데이터를 어떤 방식으로 사용할지에 따라 선택해야 하는 자료구조라고 이해했다.

---

### 3. 실습 / 과제 / 결과물

> 실습2 STEP3 에러 이유 정리

- Animal 타입 변수로 play()를 바로 호출하려고 해서 에러가 발생
- Rabbit, Seal, Snake가 Playable을 구현했더라도 반복문 안의 변수 타입은 Animal임
- Animal에는 play() 메서드가 정의되어 있지 않기 때문에 컴파일러가 호출할 수 없다고 판단
- 해결 방법은 instanceof로 Playable인지 확인한 뒤 play()를 호출하는 것

> 컬렉션 프레임워크 추가 학습

- List
    1. 순서가 있는 데이터 저장 구조
    2. 데이터 중복 가능
    3. add(), get(), remove() 등을 사용해 데이터 관리 가능
    4. 여러 객체를 순서대로 반복 처리할 때 유용함

- Map
    1. key와 value를 한 쌍으로 저장하는 구조
    2. key는 중복될 수 없음
    3. put(), get(), remove() 등을 사용해 데이터 관리 가능
    4. 이름이나 id로 특정 데이터를 빠르게 찾을 때 유용함

---

### 4. 느낀 점 & 다음 계획

처음에는 Rabbit, Seal, Snake가 Playable을 구현했으니까 animal.play()도 당연히 실행될 것이라고 생각했다.
그런데 에러가 나는 이유를 정리하면서, 자바에서는 실제 객체가 무엇인지뿐만 아니라 변수를 어떤 타입으로 선언했는지도 중요하다는 것을 알게 되었다.

다형성을 사용하면 부모 타입 하나로 여러 자식 객체를 깔끔하게 다룰 수 있지만, 부모 타입에 없는 기능을 호출하려면 인터페이스 타입으로 바라보거나 형변환이 필요하다는 점이 중요했다.

List와 Map도 단순히 데이터를 담는 도구라고만 생각했는데, 실제 코드에서 어떻게 데이터를 사용할지에 따라 선택해야 한다는 것을 알게 되었다.
다음에는 Animal 배열로 작성했던 코드를 List<Animal>로 바꿔보고, 동물 이름으로 객체를 찾는 기능은 Map<String, Animal>로 직접 구현해보고 싶다.
