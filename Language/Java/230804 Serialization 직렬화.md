## Serialization 직렬화
* 데이터를 전송 및 저장하기 위해서 직렬화/역직렬화를 사용
* PC의 OS마다 서로 다른 가상 메모리 주소 공간을 가지므로, Reference Type의 데이터들은 인스턴스를 전달 할 수 없음
* 따라서 주소값이 아닌 Byte 형태로 직렬화된 객체 데이터를 전달해야함
* 직렬화된 데이터들은 모두 Primitive Type이 되어 파일 저징 및 네트워크 전송시 파싱이 가능한 유의미한 데이터가 됨

### Java에서의 직렬화
* java.io.Serializable 인터페이스를 통해 직렬화/역직렬화
* Primitive type 같이 값 자체를 저장하는 데이터 직렬화 가능
* Reference type 같이 주소값을 가지는 객체는 직렬화를 위해서 Serializable 인터페이스 구현 필요

### 직렬화 사용 예시
* JVM에 상주하는 객체 데이터를 영속화할 때
* 데이터베이스 저장
* 캐시
* 세션 관리
* 분산 시스템, 클러스터링

```java
// 직렬화 가능한 클래스를 만듭니다.
class Person implements Serializable {
    private static final long serialVersionUID = 1L;
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String toString() {
        return "Person{name='" + name + "', age=" + age + "}";
    }
}

public class SerializationExample {
    public static void main(String[] args) {
        // 객체 생성
        Person person = new Person("Alice", 30);

        // 객체를 파일로 직렬화
        try {
            FileOutputStream fileOut = new FileOutputStream("person.ser");
            ObjectOutputStream out = new ObjectOutputStream(fileOut);
            out.writeObject(person);
            out.close();
            fileOut.close();
            System.out.println("객체가 직렬화되어 person.ser 파일로 저장되었습니다.");
        } catch (IOException e) {
            e.printStackTrace();
        }

        // 파일로부터 객체 역직렬화
        Person deserializedPerson = null;
        try {
            FileInputStream fileIn = new FileInputStream("person.ser");
            ObjectInputStream in = new ObjectInputStream(fileIn);
            deserializedPerson = (Person) in.readObject();
            in.close();
            fileIn.close();
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }

        // 역직렬화된 객체 출력
        if (deserializedPerson != null) {
            System.out.println("역직렬화된 객체: " + deserializedPerson);
        }
    }
}
```

### 참고사항
* 직렬화/역직렬화 호환성을 위한 클래스 버전 관리 주의
  * serialVersionUID 관리
* 클래스 변경을 개발자가 예측할 수 없을 때는 직렬화 지양
* 개발자가 직접 컨트롤 할 수 없는 클래스는 직렬화 지양
* 자주 변경되는 클래스는 직렬화 지양
* 역직렬화 실패에 대한 예외처리 필수
* 직렬화된 데이터의 크기 및 성능에 대해 고려
* 직렬화 데이터는 사이즈가 크므로 JSON을 사용하는 것이 효율적
  * java 표준라이브러리 사용 혹은 특정 상황을 제외하면
  * 대부분의 경우 JSON을 사용하는 것이 효율적