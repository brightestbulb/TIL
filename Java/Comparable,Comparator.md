## Comparable - 오름 차순으로 정렬
- Comparable은 기본적인 정렬을 수행한다. 숫자는 오름차순, 문자는 알파벳 순으로 정렬을 한다.
- Primitive 타입, Integer, String, List 등 정렬을 사용하는 데이터는 Comparable 인터페이스를 implement 한다.
- 즉, Comparable이 제공하는 정렬 기능을 사용한다.


기본적으로 Arrays.sort()를 사용하면 오름차순으로 정렬을 한다.   
그러나 객체를 정렬한다면 오류가 발생한다.   
Car 클래스에서 Comparable 인터페이스의 compareTo()를 재정의하여 정렬을 할 수 있다.
```java
  Car[] arr = new Car[]{
            new Car("avante", 2000),
            new Car("santafe", 3000),
            new Car("genesis", 4000)
    };

    Arrays.sort(arr);

    for(Car c : arr){
        System.out.println(c.getName());
    }

// avante
// genesis
// santafe
```

```java
public class Car implements Comparable<Car>{

    private String name;
    private int price;

    public Car(String name, int price){
        this.name = name;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public int getPrice() {
        return price;
    }

    @Override
    public int compareTo(Car o) {
        // Car 객체의 name을 비교
        return name.compareTo(o.name);
    }
}
```

## Comparator - 다른 기준으로 정렬
Comparator 인터페이스의 compare()를 재정의하여 다른 기준으로 정렬할 수 있다.   
정렬하기 전에 기준을 딱 한번 세우는 일회성 활동이기 때문에 익명 클래스를 사용한다.   
compare()는 리턴값이 양수면 두 객체의 자리를 바꿔주는 역할을 한다.   

```java
// price 내림 차순
Arrays.sort(arr, new Comparator<Car>() {
    @Override
    public int compare(Car o1, Car o2) {
        int p1 = o1.getPrice();
        int p2 = o2.getPrice();
        return p1 > p2 ? -1 : (p1 == p2 ? 0 : 1);
    }
});

for(Car c : arr){
    System.out.println(c.getPrice());
}

// price 오름 차순
Arrays.sort(arr, new Comparator<Car>() {
    @Override
    public int compare(Car o1, Car o2) {
        int p1 = o1.getPrice();
        int p2 = o2.getPrice();
        return p1 > p2 ? 1 : (p1 == p2 ? 0 : -1);
    }
});

for(Car c : arr){
    System.out.println(c.getPrice());
}

```

