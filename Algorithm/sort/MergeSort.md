# 병합 정렬

대표적인 O(logN) 알고리즘인 병합정렬(Merge Sort)에 대해서 알아보자.
병합 정렬은 분할 정복기법과 재귀 알고리즘을 이용하는 알고리즘이다.
주워진 배열의 값이 하나밖에 남지 않을 때까지 계속 둘로 쪼갠 후에 다시 크기 순으로 재배열 하면서
원래의 크기의 배열로 합친다.

### 특징

분할 단계와 병합 단계로 나눌 수 있으며 단순히 중간 인덱스를 찾아야 하는 분할 비용보다 모든 값들을 비교해야 하는 병합 비용이 크다.
만약 **[8,2,3,5,7,1,4,6]** 배열이 있다면 8 -> 4 -> 2 -> 1 식으로 전반적인 반복의 수는 점점 절반으로 줄어들기 때문에
**O(logN)** 시간이 필요하며, 각 패스에서 병합할 때 모든 값들을 비교하므로 **O(N)** 시간이 든다.
따라서 총 시간 복잡도는 **O(NlogN)** 이다.
두개의 배열을 병합할 때 병합 결과를 담아 놓을 배열이 추가로 필요하므로 공간 복잡도는 **O(N)** 이다.


```java
import java.util.Arrays;

public class MergeSort {

    public static int[] sort(int[] arr){
        if(arr.length<2){ return arr; }

        int mid = arr.length/2;
        int[] low_arr = sort(Arrays.copyOfRange(arr,0, mid));
        int[] high_arr = sort(Arrays.copyOfRange(arr,mid, arr.length));

        int[] mergedArr = new int[arr.length];
        int md = 0, low = 0, high =0;

        while(low < low_arr.length && high < high_arr.length){
            if(low_arr[low] < high_arr[high]){
                mergedArr[md++] = low_arr[low++];
            }else{
                mergedArr[md++] = high_arr[high++];
            }
        }

        while(low < low_arr.length){
            mergedArr[md++] = low_arr[low++];
        }
        while(high < high_arr.length){
            mergedArr[md++] = high_arr[high++];
        }

        return mergedArr;
    }
}
```

```java
public static void main(String[] args){
    int[] arr = {4,2,3,1};

    MergeSort mergeSort = new MergeSort();
    int [] result = mergeSort.sort(arr);

    System.out.println(Arrays.toString(result));
}
```

결과
```java
[1, 2, 3, 4]
```