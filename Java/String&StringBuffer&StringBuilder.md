## String 대신 StringBuffer, StringBuilder를 써야할 경우
세 클래스는 모두 문자열을 처리하기 위한 클래스이다.   
문자열을 더하는 연산을 할 때 성능의 차이가 발생하는데 String 클래스가 StringBuffer, StringBuilder 보다 느리고 메모리 관리 면에서도 큰 차이를 보인다.
String 과 StringBuffer/StringBuilder 클래스의 가장 큰 차이점은 String은 불변(immutable)의 속성을 갖는다는 점이다.

```java
String str = "abc"; // String str = new String("abc");
str = str + "def";  // abcdef
```
**abc**값을 갖고 있던 String 클래스의 참조 변수 str이 가리키는 곳에 저장된 **abc**에 **def** 문자열을 더해 **abcdef**로 변경된걸로 착각할 수 있다.   
하지만 기존에 **abc** 값이 들어가있던 String 클래스의 참조변수 str이 **abcdef** 라는 값을 갖는 새로운 메모리 영역을 가리키게 되고,
처음 선언했던 **abc** 로 값이 할당되어 있던 메모리 영역은 Garbage로 남아있다가 GC에 의해 사라지게 된다.   
**String 클래스는 불변하기 때문에 문자열을 수정하는 것이 아닌 새로운 String 인스턴스를 생성한다.**    

String은 변하지 않는 문자열을 자주 읽어들일 경우엔 사용하면 좋은 성능을 기대할 수 있다.   
그러나 문자열 추가, 수정, 삭제 등의 연산이 자주 발생하는 곳에서 사용하면 힙(Heap) 메모리에 많은 임시 Garbage가 생성되어 힙메모리 부족으로 어플리케이션 성능에 치명적인 영향을 끼친다.   

이를 해결하기 위해 가변성(mutable)을 갖는 **StringBuffer/StringBuilder** 클래스를 도입했다.   
.append(), .delete() 등의 API를 사용하여 동일 객체내에서 문자열을 변경하는것이 가능하다.   
따라서 문자열의 추가, 수정, 삭제가 빈번하게 일어난다면 **StringBuffer/StringBuilder**를 사용해야 한다.

```java
StringBuffer sb = new StringBuffer("abc");
sb.append("def");
```


## StringBuffer, StringBuilder 차이
큰 차이점은 동기화 유무이다.   
StringBuffer는 동기화 키워드를 지원하여 멀티 스레드 환경에서 안전(thread-safe)하다는 점이다.   
(String도 불변성을 가지기 때문에 thread-safe)   
StringBuilder는 동기화를 고려하지 않는 만큼 단일 스레드에서는 성능이 좋다.   
