## 최소힙
```java
public class MinHeap {
    private ArrayList<Integer> heap;

    public ArrayList<Integer> getHeap() {
        return heap;
    }

    public MinHeap(){
        heap = new ArrayList<Integer>();
        heap.add(0);  // 1부터 시작하기 위해 인덱스 0 채우기.
    }

    public void insert(int val){
        heap.add(val);
        int p = heap.size()-1;

        // p 값이 1보다 작아질 때까지 진행 -> root로 이동
        while(p>1 && heap.get(p/2) > heap.get(p)){
            //부모보다 자식 노드가 더 작으면 swap(최소힙)
            int tmp = heap.get(p/2);
            heap.set(p/2, heap.get(p));
            heap.set(p, tmp);
            heap.set(p, tmp);

            p = p/2; // p는 부모 인덱스로 이동
        }
    }
    
    // heap에서의 삭제는 루트 노드 삭제
    public void delete(){
        if(heap.size()-1<1){
            return;
        }
        // root에 가장 마지막 노드의 값 넣고 마지막 값 삭제
        heap.set(1, heap.get(heap.size()-1));
        heap.remove(heap.size()-1);

        int pos = 1;
        while(pos*2 < heap.size()){
            int min = heap.get(pos*2);
            int minPos = pos*2;

            // ex) 인덱스 3이 존재하고, 인덱스 2의 값이 인덱스 3보다 큰 경우
            if(pos*2+1<heap.size() && min > heap.get(pos*2+1)){
                min = heap.get(pos*2+1);
                minPos = pos*2+1;
            }
            if(heap.get(pos)<min){
                break;
            }

            // 부모 자식 노드 교환
            int tmp = heap.get(pos);
            heap.set(pos, heap.get(minPos));
            heap.set(minPos, tmp);
            pos = minPos;
        }
    }
}
```

```java
public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int heapSize = Integer.parseInt(br.readLine());

        MinHeap heap = new MinHeap();
        for(int i=0; i<heapSize; i++){
            heap.insert(i+2);
        }

        heap.insert(1);
        System.out.println(heap.getHeap().get(1));
        System.out.println(heap.getHeap().get(2));
        System.out.println(heap.getHeap().get(3));
        System.out.println(heap.getHeap().get(4));

        heap.delete();
        System.out.println(heap.getHeap().get(1));
    }
```

```java
// 값 1 추가 전
//2 3 4
// 추가, 삭제 후
1
2
4
3

2
```


## 최대힙
```java
public class MaxHeap {
    private ArrayList<Integer> heap;

    public MaxHeap(){
        heap = new ArrayList<>();
        heap.add(1000000);  // 인덱스 1부터 시작
    }

    public void insert(int val){
        heap.add(val);
        int p = heap.size()-1;

        //루트까지 이동, 자식노드가 더 크면 swap
        while(p>1 && heap.get(p/2)<heap.get(p)){
            int tmp = heap.get(p/2);
            heap.set(p/2, heap.get(p));
            heap.set(p, tmp);

            p = p/2;
        }
    }

    public void delete(){
        if(heap.size()-1<1){
            return;
        }

        heap.set(1, heap.get(heap.size()-1));
        heap.remove(heap.size()-1);

        int pos = 1;
        while(pos*2< heap.size()){
            int max = heap.get(pos*2);
            int maxPos = pos*2;

            if(pos*2+1 < heap.size() && max < heap.get(pos*2+1)){
                max = heap.get(pos*2+1);
                maxPos = pos*2+1;
            }

            if(heap.get(pos) > max){
                break;
            }

            int tmp = heap.get(pos);
            heap.set(pos, heap.get(maxPos));
            heap.set(maxPos, tmp);
            pos = maxPos;
        }
    }
}
```