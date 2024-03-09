## 디자인 패턴

### 1. 싱글톤 패턴 singleton pattern
* 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴
  * 애플리케이션이 시작될 때, 어떤 클래스가 최초 한 번만 메모리를 할당하고, 해당 메모리에 인스턴스를 만들어 사용하는 패턴
  * 인스턴스가 필요할 때, 새로 만들지 않고 기존의 인스턴스를 활용
  * 생성자가 여러번 호출되어도, 실제로 생성되는 객체는 하나
* 보통 데이터베이스 연결 모듈에 많이 사용
* 인스턴스가 절대적으로 한 개만 존재하는 것을 보증하고 싶을 때 사용
* 하나의 인스턴스를 공유하여 사용하기에,
  * 장점 : 인스턴스를 생성할 때 드는 비용이 줄어듬
  * 단점 : 의존성이 높아져서 유지보수가 힘들고 테스트 진행에도 문제 발생
  * 단점 : 멀티스레드 환경에서 인스턴스가 2개 생성되는 문제 발생
  
```java
public class Singleton {
    // 정적 멤버 변수로 유일한 인스턴스를 저장합니다.
    private static Singleton instance;
    
    // 생성자를 private으로 선언하여 외부에서 직접 인스턴스를 생성할 수 없도록 합니다.
    private Singleton() {}
    
    // 인스턴스를 반환하는 정적 메서드를 구현합니다.
    public static Singleton getInstance() {
        // 인스턴스가 생성되어 있지 않은 경우에만 생성하고 반환합니다.
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

```java
class Singleton {
    private static class singleInstanceHolder {
        private static final Singleton INSTANCE = new Singleton();
    }
    public static Singleton getInstance() {
        return singleInstanceHolder.INSTANCE;
    }
}

public class HelloWorld {
    public static void main(String[] args) {
        Singleton a = Singleton.getInstance();
        Singleton b = Singleton.getInstance();
        System.out.println(a.hashCode());
        System.out.println(b.hashCode());
    }
}
```

### 의존성 주입 DI Dependency Injection
* 싱글톤패턴의 단점으로 단위 테스트에 걸림돌이 된다.
* 하나의 인스턴스를 기반으로 구현하는 패턴이라 모듈간의 결합도가 높아져, 각 테스트마다 '독립적인'인스턴스를 만들기가 어렵다.
* 이때 의존성 주입(DI)을 통해 모듈간의 결합을 느슨하게 만들어 해결 가능하다.
* '의존성 주입자'를 통하여 메인 모듈이 간접적으로 하위 모듈에게 의존성을 주입한다.

```java
// 서비스 인터페이스
public interface Service {
    void execute();
}

// 구현 클래스
public class ServiceImpl implements Service {
    @Override
    public void execute() {
        System.out.println("ServiceImpl 실행");
    }
}

// 클라이언트 클래스
public class Client {
    private Service service;

    // 생성자를 통한 의존성 주입
    public Client(Service service) {
        this.service = service;
    }

    public void doSomething() {
        // 서비스 실행
        service.execute();
    }

    public static void main(String[] args) {
        // 클라이언트 인스턴스 생성 시 서비스 주입
        Service service = new ServiceImpl();
        Client client = new Client(service);
        client.doSomething();
    }
}
```
* 위 예시에서 Client 클래스는 Service 인터페이스에 의존
* 클라이언트 클래스 내에서 직접 생성하지 않고, 생성자를 토해 외부에서 주입받고 있음