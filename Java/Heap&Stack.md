# Java의 메모리 관리
Java에서 메모리 관리를 어떻게 하는지 알아보자.

## Stack
- Stack 에는 Heap 영역에 생성된 Object 타입의 데이터의 참조값이 할당된다.
- 원시 타입(byte, short, int, long, float, boolean, char) 타입의 데이터들이 할당된다.
- 원시 타입의 데이터들에 대해서는 참조값을 저장하는 것이 아니라 실제 값을 stack에 직접 저장한다.
- Stack 영역에 저장된 지역변수가 쓰인 함수가 종료되면 Stack에서 Pop되어 사라진다.
- Stack 메모리는 Thread 하나당 하나씩 생성되며 서로 다른 스레드들 간에 Stack 영역에 침범할 수 없다.

예시를 통해 Stack이 어떻게 활용되는지 살펴보자.

```java
public class Main {
    public static void main(String[] args) {
        int param = 10;
        int result = calculator(param);
    }
    
    private static int calculator(int num){
        int multiple = num * 3;
        int divide = multiple / 3;
        return divide;
    }
}
```

### 과정
1. int param = 10; 을 통해 Stack에 **param**이라는 변수명으로 공간이 할당되고, 값인 10이 할당된다.
2. calculator() 함수가 호출되는데, 인자로 변수 param을 넘겨주면 변수의 scope가 calculator() 함수로 이동한다. 기존의 param 값은 기존 scope에서 벗어나게 되면서 사용할 수 없다.
3. 인자로 넘겨받은 값은 파라미터인 num에 복사되어 전달되어 stack에 할당된 공간에 값이 할당된다.
4. 다음으로 multiple, divide 변수에 공간이 할당되어 값이 저장된다.
5. calculator() 함수가 닫히는 **}** 가 실행되어 함수가 종료되고, 호출함수 scope에서 사용되었던 모든 지역 변수들은 stack에서 pop 된다.
6. result 변수가 stack에 할당되어 결과값이 담기게 되고, param 변수는 다시 사용할 수 있는 상태가 된다.
7. main 함수가 종료되는 순간 stack에 있는 모든 데이터들은 pop 되면서 프로그램이 종료된다.

   
   
   
## Heap
- Heap 영역에는 주로 긴 생명주기를 가지는 데이터들이 저장된다. ( 대부분의 오브젝트 )
- 애플리케이션의 모든 메모리 중 Stack에 있는 데이터를 제외한 부분이라고 생각하면 된다.
- 모든 Object 타입( Integer, String, List,..)은 Heap 영역에 생성된다.
- 스레드에 갯수에 상관없이 단 하나의 Heap 영역만 존재한다.
- Heap 영역에 있는 Object를 가리키는 참조변수가 Stack에 올라가게 된다.

예시를 통해 Heap이 어떻게 활용되는지 살펴보자.

```java
public class Main {
    public static void main(String[] args) {
        String name = "Brightestbulb";
    }
}
```

### 과정
1. String 변수에 저장된 데이터의 참조값은 Heap 영역에 할당된다.
2. Stack에는 name이라는 변수가 할당되어 heap에 있는 **"Brightestbulb"** 값을 참조하게 된다. 

   
   
   
## Stack & Heap 복합 이해

```java
public class Main {
    public static void main(String[] args) {
        HashMap<String, Object> map = new HashMap<String, Object>();
        map.put("name", "Brightestbulb");
     
        getNameInMap(map);
    }
    
    public static void getNameInMap(HashMap<String, Object> param){
        String name = param.get("name");
        System.out.println(name);
    }
}
```

### 과정
1. new 키워드를 통해서 Heap에 새로 만든 Object를 저장할수 있는 공간이 충분한지 먼저 찾는다.
2. Heap에 비어있는 HashMap을 할당하고, Stack에 map 로컬 변수를 할당하고 Heap에 있는 HashMap을 참조한다.
3. **map.put("name", new String("Brightestbulb"));** 과 같은 역할을 한다. 즉 new 키워드에 의해  Heap 영역에 충분한 공간이 있는지 확인 후 **Brightestbulb** 문자열을 할당한다.
4. 이때 **Brightestbulb** 문자열은 Stack에 할당되는게 아니라 Heap에 있는 HashMap내부에 추가된 참조값을 갖게 된다.
5. **getNameInMap(map)** 함수가 호출된다. map이라는 참조 변수를 파라미터로 넘겨주게 되고, 함수 호출 시 원시타입의 경우와 같이 넘겨주는 인자가 갖고 있는 값이 그대로 파라미터에 복사된다. 
6. 따라서 Stack에 map 변수는 Scope에서 벗어나게 되면서 사용할 수 없게 돠고, param 변수가 Stack에 추가되면서, param이 기존에 map이 Heap에 참조하고있던 참조 변수를 참조하게 된다.
7. param에 있는 "name" 키값에 접근하여 해당 값을 name 변수에 저장한다. 이때 getNameInMap() Scope에서 Stack에 "name"이 추가되고, "name"은 param을 통해 name 키 값에 접근하여 그 값을 참조한다.
8. "name"의 값을 출력하고 **}** 에 도달하여 getNameInMap() 함수의 지역 변수는 모두 Stack에서 pop되어 사라진다.
9. main() 함수가 종료되면서 모든 변수가 Stack에서 pop되어 사라진다.
