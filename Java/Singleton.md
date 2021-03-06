# Singleton

단 하나만 생성할 수 있는 객체를 **싱글턴** 이라고 한다.   
싱글턴을 만들려면 클래스 외부에서 new 연산자로 생성자를 호출할 수 없도록 막아야 한다.   
생성자를 호출한 만큼 객체가 생성되기 때문이다.   
생성자를 외부에서 호출할 수 없도록 하려면 생성자 앞에 private 접근 제한자를 붙여줘야 한다.   
외부에서 객체를 생성하는 유일한 방법은 getInstance()를 호출하는 방법이다.   

```java
public class Single {
    private static Single singleton = new Single();
    
    private Single() { }

    static Single getInstance(){ 
        return singleton;
    }
}

public class SingletonExample {
    public static void main(String[] agrs){
        Single obj1 = new Single();   // 컴파일 에러
        Single obj2 = new Single();   // 컴파일 에러
   
        Single obj1 = Single.getInstance();
        Single obj2 = Single.getInstance();
        
        System.out.println(obj1==obj2); // true
    }
}
```