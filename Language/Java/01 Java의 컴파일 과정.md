## Java 특징
* Java는 플랫폼에 독립적인 언어
* 어떤 종류의 CPU, OS 와 상관없이 컴파일된 코드(바이트 코드)가 JVM을 통해 실행되기 때문
* 어떠한 플랫폼에서든 작성한 소스 코드의 변경없이 실행 가능
* 이를 위해서 JVM(Java Virtual Machine)이 필요

## JVM Java Virtual Machine

![](./img/2024-03-15-20-35-00.png)

* 바이트 코드를 OS에 특화된 코드로 변환하여 실행시켜주는 가상머신
* JVM은 H/W와 OS에서 실행되기에, JVM 자체는 플랫폼에 종속적
* JVM을 거쳐 변환된 코드가 인터프리트(interpret)되기 때문에 속도가 느림
* 하지만 JIT 컴파일러와 기술의 발전으로 속도의 격차를 많이 줄임
* Garbage Collection을 통해 불필요한 메모리 정리

## Java 컴파일 과정

![](./img/2023-07-24-10-10-00.png)

![](./img/2023-07-24-10-21-52.png)

![](./img/2024-03-15-20-06-17.png)

1. 자바 소스 코드(.java)를 작성
2. 자바 컴파일러(Java Compiler)가 자바 소스 코드(.java)를 자바 바이트 코드(Java Byte Code)로 컴파일
   * 자바 바이트 코드(.class)는 자바 가상 머신 JVM이 이해할 수 있는 코드
   * 바이트 코드의 각 명령어는 1바이트 크기의 Opcode와 추가 피연산자로 이루어져 있음
3. 컴파일된 바이트 코드를 JVM의 클래스로더(Class Loader)에게 전달
4. 클래스 로더는 동적로딩(Dynamic Loading)을 통해 필요한 클래스들을 로딩 및 링크하여 런타임 데이터 영역(Runtime Data area), 즉 JVM의 메모리에 올림
5. 실행엔진(Execution Engine)은 JVM 메모리에 올라온 바이트 코드들을 명령어 단위로 하나씩 가져와서 실행. 

### JIT (Just in Time) 컴파일러
![](./img/2024-03-15-21-09-59.png)

* 인터프리터의 단점을 보완하기위해 도입
* 인터프리터 방식으로 실행하다 적절한 시점에 바이트코드 전체를 네이티브 코드로 바꿔 네이티브 코드로 컴파일된 코드를 사용
* 네이티브 코드는 캐시에 보관하기 때문에 한 번 컴파일된 코드는 계속 빠르게 수행 가능
* JIT컴파일러를 사용하는 JVM들은 내부에서 메소드의 수행빈도를 확인한 후 일정 정도를 넘을 때, 컴파일 수행

### JVM 구성 요소

![](./img/2024-03-15-21-13-10.png)

### 클래스 로더 세부 동작
* 클래스 로더가 컴파일된 자바 바이트 코드를 메모리 영역에 로드하고,
* 실행 엔진이 자바 바이트코드를 실행!

![](./img/2023-07-24-10-32-00.png)

#### 1. Loading
* 클래스 파일을 가져와서 JVM의 메모리에 로드
* 로딩이 끝나면 해당 클래스 타입의 클래스 객체를 생성하여 Heap 영역에 저장

#### 2. Linking
1. verifying 검증 : 자바 언어 명세(Java Language Specification) 및 JVM 명세에 명시된 대로 구성되어 있는지 검사
2. preparing 준비 : 클래스가 필요로 하는 메모리(필드, 메서드, 인터페이스 등)를 할당
3. resolving 분석 : 클래스의 상수 풀 내 모든 심볼릭 레퍼런스를 다이렉트 레퍼런스로 변경

#### 3. initializing
* 초기화 : 클래스 변수들을 적절한 값으로 초기화 (static 변수의 값을 할당)
   

### 실행 엔진 Execution Engine
* 실행 엔진은 클래스 로더를 통해 런타임 데이터 영역에 배치된 바이트 코드를 명령어 단위로 읽고 실행
* 두 가지 방식 존재
* Interpreter
  * 바이트 코드 명령어를 하나씩 읽어서 해석하고 실행
  * 하나하나의 실행은 빠르나, 전체적인 실행 속도가 느림
* JIT Compiler (Just-In-Time Compiler)
  * Interpreter의 단점을 보완
  * 바이트 코드 전체를 컴파일하여 바이너리 코드로 변경하고 이후에는 해당 메서드를 더이상 인터프리팅 하지 않고, 바이너리 코드로 직접 실행하는 방식
  * 하나씩 인터프리팅하여 실행하는 것이 아니라 바이트 코드 전체가 컴파일된 바이너리 코드를 실행하는 것이기 때문에 전체적인 실행속도는 인터프리팅 방식보다 빠름
  * 한 번 컴파일된 코드를 캐시에 보관했다가 꺼내어 실행하기 때문에 빠르게 수행
  * 하지만 JIT 컴파일러가 컴파일 하는 과정이 오래 걸리기 때문에, JVM은 내부적으로 해당 메서드가 얼마나 자주 호출되고 실행되는지 체크하고, 일정 기준을 넘었을 때에 JIT 컴파일러를 통해 컴파일

### 런타임 데이터 영역 Runtime Data Area

![](./img/2023-07-24-10-47-05.png)

* JVM이 OS위에서 실행 되면서 할당받는 영역
* PC Register, JVM stack Native Method Stack은 스레드마다 하나씩 생성
* Heap, Method Area는 모든 스레드가 공유해서 사용
* PC Register
  * Program Counter Register는 현재 수행 중인 명령의 주소를 가지며 스레드가 시작될 때 생성
* JVM stack
  * Stack Frame이라는 구조체를 저장하는 스택
  * 예외 발생 시 printStackTrace() 메서드를 보여주는 Stack Trace의 각 라인 하나가 스택 프레임을 표현
* Native Method Stack
  * Java 외의 언어로 작성된 네이티브 코드를 위한 스택
  * 언어에 맞게 스택이 생성
* Heap
  * 인스턴스 또는 객체를 저장하는 공간
  * Garbage Collection 대상
* Method Area
  * JVM이 읽어 들인 각각의 클래스와 인터페이스에 대해 런타임 상수 풀, 필드와 메서드에 대한 정보, Static 변수, 메서드의 바이트 코드 등을 보관

### JDK와 JRE
#### JRE Java Runtime Environment
* JVM + 라이브러리
* 자바 애플리케이션을 실행할 수 있도록 구성된 배포판
* JVM과 핵심 라이브러리 및 자바 런타임 환경에서 사용하는 프로퍼티 세팅이나 리소스 파일을 가지고 있음
* 개발 관련 도구는 포함x

#### JDK Java Development Kit
* JRE + 개발툴
* 오라클은 Java11부터는 JDK만 제공, JRE는 x



참고

[1] https://steady-snail.tistory.com/67

[2] https://d2.naver.com/helloworld/1230

[3] https://catsbi.oopy.io/df0df290-9188-45c1-b056-b8fe032d88ca#0af471f2-69b3-45a0-8d32-5dff9a21310c