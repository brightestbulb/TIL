# Lambda


람다를 알아보기 전에 동작 파라미터화 패턴에 대해서 살펴보자.

## 동작 파라미터화

```java
class Car {
    private String name;
    private int price;
    private String type;

    public Car(String name, int price, String type) {
        this.name = name;
        this.price = price;
        this.type = type;
    }

    public String getName() {
        return name;
    }

    public int getPrice() {
        return price;
    }

    public String getType() {
        return type;
    }
}
```

```java
public class Application {

    public static void main(String[] args){

        List<Car> list = Arrays.asList(
            new Car("a", 100, "sedan"),
            new Car("b", 150, "sedan"),
            new Car("c", 200, "suv"),
            new Car("d", 250, "suv"),
            new Car("e", 300, "sport")
        );

    }

    public static List<Car> getCar(List<Car> list, int price, String type){
        List<Car> result = new LinkedList<>();

        for(Car car : list){
            if(car.getPrice() >= price && car.getType().equals(type)){
                result.add(car);
            }
        }
        return result;
    }
}
```

위 코드에 getCar() 메소드를 보면 차의 가격과 타입을 비교하는 조건문이 있다.   
요구사항이 늘어날수록 조건문이 더 복잡하게 추가될 것이다.   
이 문제를 **동작 파라미터화**를 통해서 해결해보자.   


## Lambda 적용 예시

**동작 파라미터화**는 메소드 로직 자체를 파라미터로 전달하는 것이다. 이러한 패턴을 **전략 디자인 패턴** 이라고 한다.    
차를 구매하는데 가격과 타입에 따라 안내를 받고 싶다면 가격과 타입을 조건에 맞게 가져오는 것을 알고리즘화 할 수 있다.   
그래서 가격, 타입을 처리하는 각각의 알고리즘 클래스를 개발하면 된다. 타입과 가격이 적합한지에 대한 조건이 있기 때문에 Boolean 값을 리턴한다.    
이 공통 부분을 인터페이스로 만든다.


```java
public interface CarCheck {
    boolean check(Car car);
}
```

```java
public class CarPriceCheck implements CarCheck {

    private int price;
    public CarPriceCheck(int price){
        this.price = price;
    }

    @Override
    public boolean check(Car car) {
        return car.getPrice() >= price;
    }
}
```

```java
public class CarTypeCheck implements CarCheck {

    private String type;
    public CarTypeCheck(String type){
        this.type = type;
    }

    @Override
    public boolean check(Car car) {
        return car.getType().equals(type);
    }
}
```


```java
public static void main(String[] args){

        List<Car> carList = Arrays.asList(
            new Car("a", 100, "sedan"),
            new Car("b", 150, "sedan"),
            new Car("c", 200, "suv"),
            new Car("d", 250, "suv"),
            new Car("e", 300, "sport")
        );

        CarPriceCheck carPriceCheck = new CarPriceCheck(200);
        List<Car> car1 = getCar(carList, carPriceCheck);

        for(Car car : car1){
            System.out.println("name : " + car.getName()+", price : "+car.getPrice());
        }

        CarTypeCheck carTypeCheck = new CarTypeCheck("suv");
        List<Car> car2 = getCar(carList, carTypeCheck);

        for(Car car : car2){
            System.out.println("name : " + car.getName()+", price : "+car.getType());
        }
    }

    public static List<Car> getCar(List<Car> list, CarCheck c){
        List<Car> result = new LinkedList<>();

        for(Car car : list){
            if(c.check(car)){
                result.add(car);
            }
        }
        return result;
    }
```

위에 getCar() 메소드에서는 CarCheck 인터페이스를 파라미터로 받고있다. 실제 비교 로직은 인터페이스를 구현한 각 클래스에 있으므로   
앞으로는 getCar() 메소드를 직접 수정할 필요가 없다. 이런 패턴을 **전략 디자인 패턴** 라고 한다.   




















