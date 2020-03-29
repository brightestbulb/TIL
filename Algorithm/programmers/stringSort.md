## 문제
https://programmers.co.kr/learn/courses/30/lessons/12915

## 나의 풀이
```java
import java.util.*;
class Solution {
  public String[] solution(String[] strings, int n) {
      String [] answer = new String[strings.length];

      TreeSet<String> set = new TreeSet<String>();
      HashMap<String, LinkedList> map = new HashMap<String, LinkedList>();
      for(int i=0; i<strings.length; i++){
          String str = Character.toString(strings[i].charAt(n));
          set.add(str);
          if(map.get(str) != null){
              LinkedList<String> list = map.get(str);
              list.add(strings[i]);
          }else{
              LinkedList<String> list = new LinkedList<String>();
              list.add(strings[i]);
              map.put(str, list);
          }
      }

      int d =0;
      Iterator<String> it = set.iterator();
      while (it.hasNext()) {
          LinkedList<String> list = map.get(it.next());
          Collections.sort(list);
          for(String str : list){
              answer[d++] = str;
          }
      }
      return answer;
  }
}
```

## 다른 사람 풀이
```java
import java.util.*;
class Solution {
    public String[] solution(String[] strings, int n) {
        String[] answer = {};
        ArrayList<String> arr = new ArrayList<>();
        for (int i = 0; i < strings.length; i++) {
            arr.add(strings[i].charAt(n) + strings[i]);
        }
        Collections.sort(arr);
        answer = new String[arr.size()];
        for (int i = 0; i < arr.size(); i++) {
            answer[i] = arr.get(i).substring(1, arr.get(i).length());
        }
        return answer;
    }
}
```