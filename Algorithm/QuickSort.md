# QuickSort

### 구현
```java
public class QuickSorter {

    public static List<Integer> quickSort(List<Integer> list){
        if(list.size() <= 1){ return list; }
        int pivot = list.get(list.size()/2);

        List<Integer> lesserArr = new LinkedList<>();
        List<Integer> equalArr = new LinkedList<>();
        List<Integer> greaterArr = new LinkedList<>();

        for(int num : list) {
            if (num < pivot) {
                lesserArr.add(num);
            } else if (num > pivot) {
                greaterArr.add(num);
            } else {
                equalArr.add(num);
            }
        }

        return Stream.of(quickSort(lesserArr), equalArr, quickSort(greaterArr))
                .flatMap(Collection::stream)
                .collect(Collectors.toList());
    }
}
```

```java
public static void main(String[] args){

        List<Integer> list = new LinkedList<>(Arrays.asList(5, 1, 4, 2, 3));

        QuickSorter quickSorter = new QuickSorter();

        System.out.println(quickSorter.quickSort(list));
    }
```

결과
```java
[1, 2, 3, 4, 5]
```

### 최적화
이 코드는 간결하지만 재귀 호출 될때마다 매번 새로운 List를 생성하여 리턴하기 때문에 메모리 사용 측면에서 비효율 적이다.   
사이즈가 커지는 경우엔 이러한 부분이 치명적일 수 있으므로 추가적인 메모리 사용이 적은 in-place 정렬이 선호된다.   

```java
public static void quickSort(int[] arr) {
    sort(arr, 0, arr.length - 1);

    //for(int i=0; i<arr.length; i++){
    //    System.out.println(arr[i]);
    //}
}

private static void sort(int[] arr, int low, int high) {
    if (low >= high) return;

    int mid = partition(arr, low, high);
    sort(arr, low, mid - 1);
    sort(arr, mid, high);
}

private static int partition(int[] arr, int low, int high) {
    int pivot = arr[(low + high) / 2];
    while (low <= high) {
        while (arr[low] < pivot) low++;
        while (arr[high] > pivot) high--;
        if (low <= high) {
            swap(arr, low, high);
            low++;
            high--;
        }
    }
    return low;
}

private static void swap(int[] arr, int i, int j) {
    int tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}
```
