# ☕ Java 학습 정리


---

## 📚 학습 항목

<br>

### 1. JDK 구조 & 빌드 도구 학습
- **JDK, JRE, JVM**
- **빌드 도구의 필요성**
  - **Gradle vs Maven**
  - **의존성 관리와 리포지토리**
  - **단순 Java + JUnit 환경 구축 실습**

---

<br>

### 2. 🧩 Generics, Enum, 클래스 확장 문법

- **Generics**: 타입 안정성과 재사용성을 위한 제네릭 문법 (`<T>`)
- **Enum**: 열거형 타입 정의 및 활용, `switch`와의 조합
- **클래스 확장 문법**  
  - `Inner Class`: 인스턴스 내부 클래스  
  - `Static Nested Class`: 정적 내부 클래스  
  - `Anonymous Class`: 익명 클래스  
  - `Local Class`: 메서드 내부에 정의된 클래스
- **`var` (Java 10+)**: 지역 변수 타입 추론
- **`record` (Java 14+)**: 불변 데이터 객체를 간결하게 표현

---

<br>

### 3. 📦 컬렉션 프레임워크

- **List / Set / Map / Queue** 계열의 인터페이스 구조
- 주요 구현체: `ArrayList`, `LinkedList`, `HashSet`, `TreeSet`, `HashMap`, `TreeMap`, `PriorityQueue`
- **정렬**: `Comparable`, `Comparator`
- **동기화 컬렉션**: `Collections.synchronizedList`, `ConcurrentHashMap`
- **equals & hashCode**의 중요성과 동작 방식

---

<br>

### 4. 🧪 Stream & 람다 (Java 8+)

- **람다 문법**: 함수형 표현식의 기본 구조
- **Stream API**  
  - 중간 연산: `map`, `filter`, `sorted`, `distinct`
  - 종단 연산: `collect`, `reduce`, `forEach`, `count`
  - `Optional` 사용과 null-safe 설계
- **함수형 인터페이스**  
  - `Function`, `Consumer`, `Supplier`, `Predicate` 등

---

<br>

### 5. 📂 입출력 (I/O & NIO)

- **기본 I/O 스트림**: `InputStream`, `OutputStream`, `Reader`, `Writer`
- **버퍼 I/O**: `BufferedReader`, `BufferedWriter`
- **파일 입출력**: `File`, `Files`, `FileReader`, `FileWriter`
- **직렬화 / 역직렬화**: `Serializable`, `ObjectInputStream`
- **NIO (New I/O)**  
  - `Path`, `Files`, `Channels`, `ByteBuffer`

---

<br>

### 6. ⏰ 날짜와 시간 API

- 구버전: `Date`, `Calendar`
- 신버전 (Java 8+): `LocalDate`, `LocalTime`, `LocalDateTime`, `ZonedDateTime`
- 날짜 계산, 비교, 포맷팅: `Duration`, `Period`, `DateTimeFormatter`
- 타임존 처리

---

<br>

### 7. 🧰 유틸리티 클래스들

- `Arrays`: 배열 정렬, 검색, 복사
- `Collections`: 리스트/셋 정렬, 불변 컬렉션 만들기
- `Math`: 수학 함수 (절댓값, 제곱, 랜덤 등)
- `StringBuilder` vs `StringBuffer`: 성능 차이, 쓰레드 안전성 비교
- `Objects`, `System`, `UUID`, `Random` 등도 함께 정리 예정

---

<br>

### 8. 🧵 멀티스레딩 & 동시성

- **기본 개념**: `Thread`, `Runnable`, `synchronized`, `volatile`
- **동기화 기법**: `wait/notify`, `ReentrantLock`, `AtomicInteger`
- **Executor Framework**: `ExecutorService`, `ThreadPoolExecutor`
- **Callable & Future**
- **Concurrent 패키지**: `BlockingQueue`, `CountDownLatch`, `Semaphore`

---

<br>

## 🧭 정리 방식

- 각 주제는 `디렉터리 + README.md` 구조로 정리됩니다.
- 실습 예제는 `code/` 하위에 포함됩니다.
- 목차는 학습 흐름에 맞춰 유동적으로 수정할 예정입니다.
- 기타 필요시 모르는 내용이 나올 때 참고하는 방식으로 학습

---

