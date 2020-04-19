## 문제
https://programmers.co.kr/learn/courses/30/lessons/42576

## 나의 풀이
```java
import java.util.HashMap;
class Solution {
    public String solution(String[] participant, String[] completion) {
        String answer = "";

        HashMap<String, Integer> m1 = new HashMap<String,Integer>();
        HashMap<String, Integer> m2 = new HashMap<String,Integer>();

        // participant를 m1, completion를 m2에 넣기
        for(int i=0; i<participant.length; i++){
            if(m1.get(participant[i])==null){
                m1.put(participant[i], 1);
            }else{
                int num1 = m1.get(participant[i]);
                m1.put(participant[i], num1+1);
            }
            if(i<participant.length-1){
                if(m2.get(completion[i])==null){
                    m2.put(completion[i], 1);
                }else{
                    int num2 = m2.get(completion[i]);
                    m2.put(completion[i], num2+1);
                }
            }
        }

        for(int d=0; d<participant.length; d++){
            if(m2.get(participant[d])== null){
                answer = participant[d];
            }
        }
        if(answer == ""){  // 동명이인이 있을 때
            for(int k=0; k<completion.length; k++){
                if(m1.get(completion[k]) > m2.get(completion[k])){
                    answer = completion[k];
                }
            }
        }
        return answer;
    }
}
```

## 다른 사람의 풀이
```java
import java.util.HashMap;

class Solution {
    public String solution(String[] participant, String[] completion) {
        String answer = "";
        HashMap<String, Integer> hm = new HashMap<>();
        for (String player : participant){ hm.put(player, hm.getOrDefault(player, 0) + 1); }
        for (String player : completion){ hm.put(player, hm.get(player) - 1); }

        for (String key : hm.keySet()) {
            if (hm.get(key) != 0){
                answer = key;
            }
        }
        return answer;
    }
}
```

```java
import java.util.Arrays;
import java.util.Iterator;
import java.util.Map;
import java.util.function.Function;
import java.util.stream.Collectors;

class Solution {
    public String solution(String[] participant, String[] completion) {

        Map<String, Long> participantMap = Arrays.asList(participant).stream()
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

        for(String name : completion) {

            Long value = participantMap.get(name) - 1L;

            if(value == 0L) {
                participantMap.remove(name);
            } else {
                participantMap.put(name, value);
            }
        }

        return participantMap.keySet().iterator().next();
    }
}
```

```java
import java.util.*;
class Solution {
    public String solution(String[] participant, String[] completion) {
      String answer = "";
        HashMap<String, Integer> hm = new HashMap<>();
        for(int i = 0; i < completion.length; i++){
            if(hm.containsKey(completion[i])){
                hm.put(completion[i], hm.get(completion[i])+1);
            }else{
                hm.put(completion[i], 1);
            }
        }
        for(int i = 0; i < participant.length; i++){
            if(hm.containsKey(participant[i])){
                if(hm.get(participant[i]) > 0){
                    hm.put(participant[i], hm.get(participant[i]) - 1);
                }else{
                    answer = participant[i];
                    break;
                }
            }else{
                answer = participant[i];
                break;
            }
        }
        return answer;
    }
}
```

출처 : https://programmers.co.kr/learn/courses/30/lessons/42576