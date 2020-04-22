## 우선순위 큐( PriorityQueue )

자바에서 우선순위 큐는 들어온 순서와 상관없이 우선순위가 높은 엘리먼트가 먼저 나가게 된다.   
다음 예제는 price에 따라 우선순위를 정하는 예제이다.   

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

    public void setName(String name) {
        this.name = name;
    }

    public int getPrice() {
        return price;
    }

    public void setPrice(int price) {
        this.price = price;
    }

    @Override
    public int compareTo(Car o) {  // 오름 차순으로 재정의
        if(this.price > o.price){
            return 1;
        }else if(this.price < o.price){
            return -1;
        }
        return 0;
    }
}
```


```java
public static void main(String[] args) {

    Car c1 = new Car("genesis", 4000);
    Car c2 = new Car("morning", 1500);
    Car c3 = new Car("sonata", 3000);
    Car c4 = new Car("avente", 2000);

    PriorityQueue<Car> priorityQueue = new PriorityQueue<Car>();

    priorityQueue.offer(c1);
    priorityQueue.offer(c2);
    priorityQueue.offer(c3);
    priorityQueue.offer(c4);

    // 내림 차순 시
    PriorityQueue<Car> reversedPriorityQueue = new PriorityQueue<Car>(priorityQueue.size(), Collections.reverseOrder());
    reversedPriorityQueue.addAll(priorityQueue);

    while(!priorityQueue.isEmpty()){
        System.out.println(priorityQueue.poll().getName());
    }

    while(!reversedPriorityQueue.isEmpty()){
        System.out.println(reversedPriorityQueue.poll().getName());
    }
}
```

출력
```java
morning
avente
sonata
genesis

genesis
sonata
avente
morning
```
