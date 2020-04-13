## 문제
https://programmers.co.kr/learn/courses/30/lessons/12916?language=java

## 나의 풀이
```java
class Solution {
    boolean solution(String s) {
        boolean answer = true;

        int p = 0;
        int y = 0;
        String [] arr = s.split("");
        for(int i=0; i<arr.length; i++){
            if(arr[i].equals("p")||arr[i].equals("P")){
                p++;
            }
            if(arr[i].equals("y")||arr[i].equals("Y")){
                y++;
            }
        }
        return p==y;
    }
}
```

## 다른 사람 풀이
```java
class Solution {
    boolean solution(String s) {
        s = s.toUpperCase();

        return s.chars().filter( e -> 'P'== e).count() == s.chars().filter( e -> 'Y'== e).count();
    }
}
```