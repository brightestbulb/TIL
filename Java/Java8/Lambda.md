# 람다식
- 객체 지향 프로그래밍과 함수적 프로그래밍을 혼합하여 효율적으로 프로그래밍 할 수 있도록 개발 언어가 지원하고 있다.
- 함수적 프로그래밍은 **병렬 처리**와 **이벤트 지향 프로그래밍**에 적합하다. 자바는 함수적 프로그래밍을 지원하기 위해 람다식을 지원한다.
- 람다식은 **익명 함수**를 생성하기 위한 식이다.
- 자바에서 람다식을 사용하는 이유는 코드가 매우 간결해지고, 컬렉션 요소를 필터링 하거나 매핑해서 원하는 결과를 쉽게 처리할 수 있기 때문이다.
- 람다식의 형태는 매개 변수를 가진 블록이지만 런타임 시에는 익명 구현 객체를 생성한다.

예시로 Runnable 인터페이스의 익명 구현 객체를 생성하는 코드는 다음과 같다.
```java
Runnable runnable = new Runnable(){
    public void run(){ . . . }
}
```

익명 구현 객체를 람다식으로 표현하면 다음과 같다.
```java
Runnable runnable = () -> { . . . };   // Runnable의 익명 구현 객체를 생성
```

## 람다식 기본 문법
1. 매개 변수 타입은 자동으로 인식된다. 
```java
(a) -> { System.out.println(a); }
```

2. **매개변수가 하나일 경우 괄호를 생략**할 수 있고, **하나의 실행문 일 경우에 중괄호**도 생략 가능하다.
```java
a -> System.out.println(a);
```

3. 값의 return
```java
(a, b) -> { return a * b; };
```

4. 중괄호에 return만 있을 경우엔 다음과 같이 사용하는 것이 정석이다.
```java
(a, b) -> a * b
```

## 람다식 타겟 타입과 함수적 인터페이스
람다식은 단순히 메소드의 선언뿐만 아니라 이 메소드를 가지고 있는 객체를 생성한다.

```java
인터페이스 변수 = 람다식;
```
- 람다식은 인터페이스 변수에 대입된다. 즉 람다식은 인터페이스의 익명 구현 객체를 생성하고 객체화한다.
- 인터페이스는 직접 객체화할 수 없기 때문에 구현 클래스가 필요하므로 람다식을 사용할 수 있다.
- 람다식은 대입될 인터페이스 종류에 따라 작성 방법이 달라지기 때문에 람다식이 대입될 인터페이스를 람다식의 **타겟 타입** 이라고 한다.


### 함수적 인터페이스( @FunctionalInterface )
- 두개 이상의 추상 메소드가 선언된 인터페이스는 람다식으로 구현 객체를 생성할 수 없다. ( 1개일 경우만 가능 )
- 하나의 추상 메소드가 선언된 **함수적 인터페이스**만이 람다식의 타켓이 될 수 있다.
- 함수적 인터페이스를 작성하기 위해 두개 이상의 추상 메소드가 선언되지 않도록 **@FunctionalInterface**을 통해 컴파일 체킹을 할 수 있다.


### 매개 변수와 리턴값이 없는 람다식
```java
@FunctionalInterface
public interface MyFuncInterface{
    public void method();
}
```

다음과 같은 형태로 람다식을 작성한다.
```java
MyFuncInterface fi = () -> { . . . }
```

람다식이 대입된 인터페이스의 참조 변수를 통해서 인터페이스의 추상 메소드를 호출할 수 있다.   
method()는 람다식의 중괄호를 실행시킨다.
```java
fi.method();
```


```java
public static void main(String[] args){
    MyFuncInterface fi;
    
    fi = () -> { System.out.println("method call1"); }
    fi.method();
    
    fi = () ->  System.out.println("method call2");
    fi.method();
}
```

```java
method call1
method call2
```


### 매개 변수가 있는 람다식
```java
@FunctionalInterface
public interface MyFuncInterface{
    public void method(int x);
}
```

```java
MyFuncInterface fi = (x) -> { . . . }  ||  x -> { . . . } 
```

```java
fi.method(1);
```


```java
public static void main(String[] args){
    MyFuncInterface fi;
    
    fi = (x) -> { System.out.println(x+1); }
    fi.method(1);    // 2
    
    
    fi = x -> System.out.println(x*2);
    fi.method(2);    // 4
}
```


출처 : 이것이 자바다 -신용권