## 문제
![](https://github.com/brightestbulb/TIL/blob/master/Algorithm/img/tower1.png?raw=true)
![](https://github.com/brightestbulb/TIL/blob/master/Algorithm/img/tower2.png?raw=true)

## 나의 풀이
```java
class Solution {
    public int[] solution(int[] heights) {
        // heights = [6, 9 ,5, 7, 4]
        int[] answer = new int[heights.length];
        
        if(heights.length<2 || heights.length>100){
            return heights;
        }
        int flg = 0;
        for(int i=heights.length-1; i>=0; i--){
            flg = 0;
            for(int k=i-1; k>=0; k--){
                if(heights[i]<heights[k]){
                    answer[i] = k+1;
                    flg = 1;
                    break;
                }
            }
            if(flg==0){
                answer[i] = 0;
            }
        }
        return answer;
    }
}
```

## 다른 사람의 풀이
```java
import java.util.*;

class Solution {
    class Tower {
        int idx;
        int height;

        public Tower(int idx, int height) {
            this.idx = idx;
            this.height = height;
        }
    }

    public int[] solution(int[] heights) {
        Stack<Tower> st = new Stack<>();
        int[] answer = new int[heights.length];

        for (int i = 0; i < heights.length; i++) {
            Tower tower = new Tower(i + 1, heights[i]);
            int receiveIdx = 0;

            while (!st.isEmpty()) {
                Tower top = st.peek();

                if (top.height > tower.height) {
                    receiveIdx = top.idx;
                    break;
                }
                st.pop();
            }
            st.push(tower);
            answer[i] = receiveIdx;
        }
        return answer;
    }
}
```

출처 : https://programmers.co.kr/learn/courses/30/lessons/42588