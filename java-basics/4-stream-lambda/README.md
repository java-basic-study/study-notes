# 람다(Lambda)

**람다식(lambda expression)이** **Java8(JDK 1.8)** 부터 추가되면서 자바는 객체지향 언어인 동시에 함수형 언어가 되었습니다.

## ✅ 람다식이란?

**람다식(lambda expression)** 이란 메서드를 하나의 **식(expression)** 으로 표현한 것입니다.

### 장점

- 코드가 간결하여 의미를 이해하기 쉽다.
- 메서드를 위한 별도의 클래스를 생성하지 않아도 된다.
- 메서드의 매개변수로 전달될 수도 있고, 메서드의 결과로 반환될 수도 있다.
  즉, 메서드를 변수처럼 사용할 수 있게 된다.

### 사용 방법

- 메서드를 람다식으로 변환하면 아래와 같은 코드가 됩니다.

    ```java
    int max(int a, int b) {
    	return a > b ? a : b;
    }
    
    // 람다식
    (int a, int b) -> { return a > b ? a : b; }
    ```


- 반환값이 있는 람다식의 경우, **return**문과 **‘;’** 를 작성하지 않고 **‘식’** 형태로 표현할 수 있습니다.
  괄호{} 안의 **문장이 하나인 경우**, **괄호{}를 생략**할 수 있습니다. 이 때 문장 끝에 **‘;’를 붙이지 않아야합니다.**
  대부분의 경우 **매개변수의 타입**이 추론 가능하기 때문에 **생략 가능**합니다. **반환타입**도 마찬가지의 이유로 생략이 가능합니다.

    ```java
    (int a, int b) -> { return a > b ? a : b; }
    
    (int a, int b) -> a > b ? a : b
    
    (a, b) -> a > b ? a : b
    ```


- **매개변수에 타입이 없고**, 선언된 **매개변수가 하나일 경우** **괄호()를 생략**할 수 있습니다.

    ```java
    (int a ) -> a * a
    
    a -> a * a
    int a -> a * a // error
    ```


## ✅ 함수형 인터페이스

함수형 인터페이스란 람다식을 다루기 위한 인터페이스를 의미합니다.
람다식과 1대1로 매칭되기 위해서 함수형 인터페이스에는 하나의 추상 메서드가 존재해야합니다. 반면에 static메서드와 default메서드에는 개수 제약이 존재하지 않습니다.

`@FunctionalInterface` 애너테이션을 붙이면, 컴파일러가 함수형 인터페이스를 올바르게 정의했는지 검사하게됩니다.

지금까지 살펴본 람다식은 인터페이스의 추상 메서드와 동등하게 동작합니다.
즉, 인터페이스를 구현한 익명 객체를 참조변수로 하여 메서드를 호출하기 때문에, 람다식과 동일한 메서드가 정의되어 있어야합니다.

아래와 같이 인터페이스와 익명 객체를 활용하여 예시를 보여드리겠습니다.

```java
@FunctionalInterface
interface F {
    public abstract int max(int a, int b);
}

// 익명 객체의 메서드 호출
F f = new F() {
    public int max(int a, int b){
        return a > b ? a : b;
    }
};
f.max(1, 2);

// 람다식을 통한 호출
F f = (a, b) -> a > b ? a : b;
f.max(1, 2);
```

### 외부 변수 참조

람다식에서 인스턴스 변수를 사용하게 되면 참조를 통해 값을 읽거나 수정하기 때문에 내부에서 값을 변경해도 문제가 발생하지 않습니다.
하지만 지역 변수를 사용할 경우, 람다식은 해당 지역 변수의 값을 복사해와서 저장하게 됩니다. 이로 인해 원본 지역 변수의 값과 내부 복사본의 값의 불일치가 발생할 수 있습니다. 따라서 이를 방지하기 위해 람다식이 참조하는 지역변수는 final이 붙어 있지 않아도 상수로 간주되어, 값을 변경하려고 하면 컴파일 에러가 발생합니다.
<br>*(추가 학습 필요: 모던 자바 인 액션)*

```java
@FunctionalInterface
interface F {
    public abstract void run();
}

class OuterClass {
    int outerVal = 1; // 인스턴스 변수

    class InnerClass {
        int innerVal = 2; // 인스턴스 변수

        void innerMethod() {
            int methodVal = 3; // 지역 변수

            F f = () -> {
                System.out.println(++outerVal);   // 인스턴스 변수
                System.out.println(++innerVal);   // 인스턴스 변수
                System.out.println(methodVal);  // 지역 변수: 변경하면 error
            };

            f.run();
        }
    }
}
```

## ✅ java.util.function 패키지

대부분의 메서드의 매개변수나 반환값의 개수는 유사합니다. 또한 제네릭 메서드로 정의된다면 타입이 달라도 문제가 발생하지 않기 때문에 자주 쓰이는 형식의 메서드가 java.util.function 패키지에 미리 정의되어 있습니다.
따라서 람다식을 사용하기 위해 매번 함수형 인터페이스를 직접 정의하기 보다는 java.util.function 패키지에 정의된 인터페이스를 활용하는 것이 효율적입니다.

### 제네릭 함수형 인터페이스

| Functional Interface | Method | Description                                                                            |
| --- | --- |----------------------------------------------------------------------------------------|
| **Runnable** | void run() | 매개변수 x, 반환값 x                                                                          |
| **Supplier<T>** | T get() | 매개변수 x, 반환값 o                                                                          |
| **Consumer<T>** | void accept(T t) | 매개변수 o, 반환값 x                                                                          |
| **Function<T>** | R apply(T t) | 매개변수 o, 반환값 o                                                                          |
| **Predicate<T>** | boolean test(T t) | 매개변수 o, 반환값 o<br>반환 타입은 boolean                                                        |
| **BiConsumer<T, U>** | void accept(T t, U u) | 매개변수 2개, 반환값 x                                                                         |
| **BiFunction<T, U>** | R apply(T t, U u) | 매개변수 2개, 반환값 o                                                                         |
| **BiPredicate<T, U>** | boolean test(T t, U u) | 매개변수 2개, 반환값 o<br>반환 타입은 boolean                                    |
| **UnaryOperator<T>** | T apply(T t) | Function의 자손<br>Function과 달리 매개변수와 반환값의 타입이 동일 |
| **BinaryOperator<T>** | T apply(T t1, T t2) | BiFunction의 자손<br>BiFunction과 달리 매개변수와 반환값의 타입이 동일 |

### 기본형 함수형 인터페이스

위에서 살펴본 예시의 경우 참조형을 사용할 수 있는 제네릭 함수형 인터페이스였습니다. 이 경우에도 **오토박싱(Autoboxing)** 을 통해 기본형을 사용할 수 있지만, 변환 과정에서 비용이 소모되기 때문에 비효율적입니다.
따라서 기본형을 사용하는 별도의 함수형 인터페이스가 정의되어 있습니다.

| Functional Interface | Method | Description |
| --- | --- | --- |
| **DoubleToIntFunction** | int applyAsInt(double d) | AToBFunction: 입력이 A타입, 출력이 B타입 |
| **ToIntFunction<T>** | int applyAsInt(T value) | ToBFunction: 입력이 제네릭 타입, 출력이 B타입 |
| **intFunction<R>** | R apply(int value) | AFunction: 입력이 A타입, 출력이 제네릭 타입 |
| **ObjIntConsumer** | void accept(T t, int i) | ObjAFunction: 입력이 T, int 타입, 출력은 x |

## ✅ 메서드 참조

(추후 작성)

# 스트림(Steram)

# 참조

Java의 정석