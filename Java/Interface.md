# 인터페이스
자바에서 인터페이스는 객체의 사용 방법을 정의한 타입이다.   
인터페이스는 객체의 교환성을 높여주기 때문에 **다형성**을 구현하는데 중요한 역할을 한다.   
인터페이스는 개발 코드와 객체가 서로 통신하는 접점 역할을 한다.   

## 인터페이스가 왜 필요한가?
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

인터페이스를 파라미터로 받음으로써 인터페이스를 구현한 클래스를 파라미터로 받을 수 있다.   
즉, getCar() **직접 수정하지 않고 구현 클래스만 교체함으로써 코드를 변경**할 수 있다.   
인터페이스는 여러 구현 클래스의 객체들로 사용이 가능하므로 어떤 객체를 사용하느냐에 따라 실행 내용과 리턴값이 다르다.   
**따라서 코드 변경 없이 실행 내용과 리턴값을 다양화** 할 수 있다.   



## 인터페이스 구성 요소

```java
public interface Interface {
    
    // 상수 
    String str = "abc";
    int num = 0;
    
    // 추상 메소드
    public abstract void method1();  
    public abstract void method2();
    
    // 디폴트 메소드 ( 실행 내용까지 작성 )
    public default void defaultMethod(){
        System.out.println("디폴트 메소드 입니다.");
    }
    
    // 정적 메소드
    public static void staticMethod(){
        System.out.println("정적 메소드 입니다.");
    }
    
}
```

인터페이스에 선언된 추상 메소드에 대응하는 실체 메소드를 구현하지 않으면 구현 클래스는 자동으로 추상클래스가 된다.   
그러므로 인터페이스의 추상 메소드를 한개라도 구현하지 않으면 구현 클래스 선언부에 **abstract** 키워드를 추가해야 한다.   

```java
public abstract class CarPriceCheck implements Car{
    public void method1(){ . . . }
    // method2() 구현 메소드가 없다.
}
```



## 다중 인터페이스 구현 클래스

클래스에서 여러 인터페이스를 다중으로 구현할 수 있다.
```java
public class CarPriceCheck implements Car, Check{
    carMethod();
    checkMethod();
}
```
구현한 인터페이스들에서 선언된 모든 추상 메소드를 구현해야한다.   


## 인터페이스의 사용

### 추상 메소드 사용
```java
//인터페이스 변수 = 구현 객체;
Car car = new CarPriceCheck();
car.CarTypeCheck();
```
구현 객체가 인터페이스 타입에 대입되면 인터페이스에 선언된 추상 메소드를 개발 코드에서 호출할 수 있다.   
즉, **car.CarTypeCheck(); 이렇게 하면 CarPriceCheck 클래스의 CarTypeCheck()가 실행**된다.   


### 디폴트 메소드 사용
디폴트 메소드가 인터페이스에 선언되고 구현되었지만 그렇다고 인터페이스에서 바로 사용할 수 없다. 
마찬가지로 구현 객체를 통해 사용해야 한다.
```java
//인터페이스 변수 = 구현 객체;
Car car = new CarPriceCheck();
car.defaultMethod();
```
디폴트 메소드는 인터페이스의 모든 구현 객체가 갖고있는 기본 메소드다.   
디폴트 메소드를 구현 클래스에서 재정의하여 사용할 수도 있다.   


### 정적 메소드 사용
인터페이스로 바로 호출할 수 있다.
```java
Car.staticMethod();
```








