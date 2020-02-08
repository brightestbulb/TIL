# QuickSort


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