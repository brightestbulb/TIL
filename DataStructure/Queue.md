# Queue

## 연결리스트 개념을 활용하여 Queue 구현

```java
public class Node {
    private int data;
    private Node NextNode;

    public Node(int data){
        this.data = data;
    }

    public void linkNextNode(Node node){
        this.NextNode = node;
    }

    public int getData(){
        return this.data;
    }

    public Node getNextNode(){
        return this.NextNode;
    }
}
```

```java
public class LinkedListQueue {

    Node first;
    Node last;

    public void offer(int val){  // 값 추가
        Node newNode = new Node(val);
        if(first == null){
            first = newNode;
            last= newNode;
        }else{
            last.linkNextNode(newNode);
            last = newNode;
        }
    }

    public int peek(){  // 맨 앞 요소 반환
        return first.getData();
    }

    public int poll(){  // 맨 앞 요소 반환 후 삭제
        int data = first.getData();
        first = first.getNextNode();
        return data;
    }
}
```