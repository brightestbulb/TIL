## String과 new String()의 차이

```java
String str1 = "abc";
String str2 = "abc";
String str3 = new String("abc");
String str4 = new String("abc");

System.out.println(str1 == str3);  // false
```

Java에서 문자열(문자열 리터럴)은 String Pool로 관리된다. "abc"라는 문자열을 다른 변수에 지정했지만,   
JVM Heap 메모리의 String Pool에는 "abc"라는 문자열 하나만 존재한다.   
두 변수 str1, str2는 reference로 가리키게 된다.   

반면에 **new String("abc")** 는 Heap에 객체를 생성하게 된다.   

str1, str2, str3의 HashCode를 비교해보면 String Pool로 관리되는 str1, str2는 값이 같지만   
str3, str4는 다른 HashCode값을 갖게된다.   
