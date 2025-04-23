## 1. JDK 선택 및 구조

### 어떤 JDK 배포판을 쓸 것인가?

Java는 오픈소스로 제공되는 OpenJDK라는 표준 구현이 있고, 이를 기반으로 다양한 배포판들이 존재한다. 그중에서 나는 **Temurin (Adoptium)**을 선택했고, JDK 21 버전을 설치했다.

<br>

### 왜 OpenJDK 중 Temurin을 선택했는가?

Temurin은 오픈소스 커뮤니티 중심의 중립적인 배포판으로 Eclipse 재단의 Adoptium 프로젝트에서 관리하며, 특정 기업이나 플랫폼에 종속되지 않는다. Temurin은 OpenJDK 소스를 가져와 자체적으로 빌드한 후, TCK(Technology Compatibility Kit) 테스트를 포함한 철저한 검증을 거친다. 이 테스트는 자바 표준을 제대로 준수하고 있는지를 의미한다. IntelliJ에서 쉽게 SDK로 등록할 수 있으며, 별다른 추가 설정 없이 바로 사용 가능한 점을 토대로 Temurin JDK를 사용하였다.

아래 링크를 통해 쉽게 다운 받아 사용할 수 있다. 

[다운 링크](https://adoptium.net/temurin/releases/)

버전은 21을 사용해서 학습해 나가며 추후에 21에서만 제공하는 기능들까지도 사용해보려고 한다.


<br>

### JDK 구조


![Image](https://github.com/user-attachments/assets/3eefaa0e-6489-4b27-84e9-fae354617126)

<br>

`JDK`<br>
---
자바 소스코드 작성, 컴파일, 실행까지 전 과정을 지원하는 도구 모음

- `javac` : 자바 컴파일러로 자바 소스코드(.java)를 바이트코드(.class) 파일로 변환하는 도구, JVM이 .class 파일을 실행시킬 수 있음
```console
javac HelloWorld
```

<br>

- `java` : JVM 실행명령어로 .class 파일을 JVM에서 실행한다. .class 파일의 main()을 찾아서 실행
```console
java HelloWorld
```

<br>

- `jdb` : 자바 디버거로 GUI 디버깅이 없는 환경에서 사용됨

<br>

- `jar` : 자바 아카이브 툴로 여러 .class, 리소스 파일을 하나의 .jar 파일로 패키징
```console
jar cf myApp.jar com/example/*.class

c : 생성
f : file (파일 이름 지정 ex. myApp.jar)


실행 가능한 JAR를 만들려면 아래와 같이 작성
jar cfe app.jar com.example.Main com/example/*.class

e : entry point (메인 클래스 지정)


실행하는 방법
java -jar app.jar
```

<br>

- `jlink` : 내 애플리케이션이 사용하는 자바 플랫폼 모듈(Java SE Modules)만 포함한 JRE를 생성하는 도구. 즉, JRE를 경량화하는 도구

<br>

- `src.zip` : 자바 API의 소스 코드가 압축된 파일로 IDE에서 내부 구현을 확인할 수 있다. 설계도, 코드가 어떻게 생겼는지 보기 위한 개발자 참고/IDE 디버깅용 파일이다.


<br>

정리해보면 javac를 통해 컴파일된 파일(.class)을 java 명령어를 통해 JVM으로 실행할 수 있고 여러 .class를 묶어서 .jar파일로 만드는 명령어가 바로 jar 명령어다. 그리고 컬렉션 프레임워크같은 기본 소스 코드들이 바로 src.zip에 존재한다.

---

<br>


`JRE`
---


JRE는 자바 프로그램을 실행할 수 있게 해주는 실행 환경이다.
개발이 아닌 "실행"에 초점이 맞춰져 있다.
즉, 누군가 만든 .class, .jar 파일을 실행만 하고 싶을 때 필요한 환경이 바로 JRE이다.



- `표준 자바 클래스 라이브러리`<br>
자바 프로그램이 실행될 때 필수적으로 참조하는 API 집합으로 jdk 9부터 jmods/.jmod 파일로 각 모듈이 분리됨 모듈 시스템 도입으로 불필요한 라이브러리를 제거하고 최소한의 런타임 환경을 구성할 수 있게 됨(jlink)

<br>

- `JVM (Java Virtual Machine)`<br>
자바 바이트코드(.class)를 해석하고 실행하는 엔진으로 JRE의 가장 핵심적인 부분이라고 볼 수 있다. 또한, 플랫폼 독립성(OS)을 실형하게 해주는 요소이다.
클래스 로딩, 바이트코드 검증, JIT 컴파일러 & 인터프리터(실행), 메모리 관리(GC, Heap), 예외처리, 쓰레드관리 같은 기능을 가지고 있다. 플랫폼 독립성을 실형하기 위해서 JVM은 플랫폼마다 다르게 구현돼 있음(Window용, Linux 용 등)


---

<br>

## 2. 빌드 도구 학습

빌드 도구란 소스코드를 자동으로 컴파일, 테스트, 패키징 해주는 도구이다.

빌드 도구를 학습하기 전에, 먼저 jar 파일을 살펴보자.

<br>

### `JAR`란?
---

zip 포맷으로 압축된 클래스 파일 + 리소스 + 메타데이터
즉, .class 파일들을 묶어 놓은 라이브러리 또는 실행 가능 패키지다.

JAR 파일 안에는 다음과 같은 요소가 있다:
```markdown
Main.class
hello/Calculator.class
META-INF/MANIFEST.MF
```

jar 파일은 `실행 가능 jar`와 `라이브러리 jar`로 구분해 볼 수 있는데 실행 가능 jar의 경우 META-INF/MANIFEST.MF 안에 Main-Class 항목이 있다.

- 라이브러리 jar : 다른 앱에서 사용하기 위한 것으로 클래스 파일만 포함
- 실행 가능 jar : 직접 실행 가능하며 Main 클래스 + 의존성 포함 (java -jar로 실행)


<br>

`JAR의 종류`

- Thin jar : 내 클래스만 포함됨. 실행하려면 외부 라이브러리가 classpath에 필요

- Fat jar : 내 클래스 + 모든 의존성 포함. 독립 실행 가능
`

jar 파일을 만드는 방법은 따로 실습 진행<br>

[jar 생성 실습](https://github.com/HeeChanN/java-play-ground/tree/main/practice-build)

jar 실습을 진행하고 느낀점은 무조건 빌드 툴을 사용하자였다. 직접 javac와 java 명령어 그리고 Fat-jar를 만들어보는 과정속에서 빌드 툴의 소중함을 느꼈다.


빌드 툴을 사용하기로 했다면 어떤 도구를 사용할지 결정하는 것도 중요하다고 생각합니다.

<br>

### `Gradle VS Maven`
---

빌드 툴에는 대표적으로 Maven과 Gradle이 존재하는데 다음 이유로 Gradle을 선택하였습니다.

- 빠른 반복 빌드
    - 바뀐 것만 다시 빌드하는 **Incremental Build (증분 빌드)**
    - 이전에 했던 결과를 캐시해서 재사용하는 Build Cache
    - 데몬 프로세스를 이용하여, 다음 빌드가 더 빠르게 시작
    
- 유연성 높은 작업 단위
    - 태스크(작업) 단위로 자유롭게 추가·수정·흐름 제어

- Spring Boot를 사용하며 경험한 groovy 기반의 gradle 빌드 툴

<br>

즉, 빠른 빌드와 자유로운 빌드 라이프 사이클이라는 이점 때문에 Gradle을 선택하였습니다.

물론 그 내부의 DSL 개념(Lazy configuration, Task graph)을 이해하는 과정도 진행되야한다는 학습곡선이 존재했지만 이는 사용하며 배워가면 되는 부분이라고 생각하였습니다.

따라서, 속도, 유연성, 현대적 생태계를 선택해 Gradle 빌드 툴을 선택하였습니다.