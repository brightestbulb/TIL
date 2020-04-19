## 문제
https://programmers.co.kr/learn/courses/30/lessons/42748

## 나의 풀이
```java
import java.util.Arrays;
class Solution {
    public int[] solution(int[] array, int[][] commands) {
        
        int[] answer = new int[commands.length];
        
        for(int k=0; k<commands.length; k++){
            int [] result = new int[commands[k][1]-commands[k][0]+1];
            int d = 0;
            for(int i= commands[k][0]; i<=commands[k][1]; i++){
                result[d++] = array[i-1];
            }
            Arrays.sort(result);
            answer[k] = result[commands[k][2]-1];
        }
        
        return answer;
    }
}
```


## 다른 사람 풀이
```java
import java.util.Arrays;
class Solution {
    public int[] solution(int[] array, int[][] commands) {
        int[] answer = new int[commands.length];

        for(int i=0; i<commands.length; i++){
            int[] temp = Arrays.copyOfRange(array, commands[i][0]-1, commands[i][1]);
            Arrays.sort(temp);
            answer[i] = temp[commands[i][2]-1];
        }

        return answer;
    }
}
```


출처 : https://programmers.co.kr/learn/courses/30/lessons/42748