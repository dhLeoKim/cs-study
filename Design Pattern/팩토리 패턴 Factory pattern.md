## 디자인 패턴

### 2. 팩토리 패턴
* 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴
* 상속 관계의 두 클래스에서 상위 클래스가 뼈대를 잡고, 하위 클래스가 구체적인 내용을 결정
* 따라서 느슨한 결합에 의해 유연성이 높고 유지보수성이 좋아짐

```java
abstract class Coffee {
    public abstract int getPrice();

    @Override
    public String toString() {
        return "Hi this coffee is " + this.getPrice();
    }
}
class CoffeeFactory {
    public static Coffee getCoffee(String type, int price) {
        if ("Latte".equalsIgnoreCase(type)) return new Latte(price);
        else if ("Americano".equalsIgnoreCase(type)) return new Americano(price);
        else {
            return new DefaultCoffee();
        }
    }
}
class DefaultCoffee extends Coffee {
    private int price;

    public DefaultCoffee() {
        this.price = -1;
    }

    @Override
    public int getPrice() {
        return this.price;
    }
}
class Americano extends Coffee {
    private int price;

    public Americano (int price) {
        this.price = price;
    }
    @Override
    public int getPrice() {
        return this.price;
    }
}
public class HelloWorld {
    public static void main (Stirng[] args) {
        Coffee latte = CoffeeFactory.getCoffee("Latte", 4000);
        Coffee ame = CoffeeFactory.getCoffee("Americano", 3000);
        System.out.println("Factory latte : " + latte);
        System.out.println("Factory ame : " + ame);
    }
}
```