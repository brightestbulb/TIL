# Bubble Sort

```java
public class BubbleSort {

    public static int[] sort(int[] arr){
        for(int d=1; d<arr.length; d++) {
            for (int i = 0; i < arr.length-1; i++) {
                if (i + 1 < arr.length) {
                    if (arr[i] > arr[i + 1]) {
                        BubbleSort.swap(arr, i, i + 1);
                    }
                }
            }
        }
        return arr;
    }

    public static void swap(int[] arr, int a, int b){
        int tmp = arr[a];
        arr[a] = arr[b];
        arr[b] = tmp;
    }
}
```