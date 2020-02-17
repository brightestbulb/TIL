# Queue

## 연결리스트 개념을 활용하여 Queue 구현

```java
public class Node {
    private int data;
    private Node beforeNode;

    public Node(int data){
        this.data = data;
    }

    public void linkBeforeNode(Node node){
        this.beforeNode = node;
    }

    public int getData(){
        return this.data;
    }

    public Node getbeforeNode(){
        return this.beforeNode;
    }
}
```

```java
public class LinkedListQueue {

    Node last;
    Node first;
    Node second;

    public void add(int val){
        Node newNode = new Node(val);
        newNode.linkBeforeNode(last);
        last = newNode;
    }

    public int peek(){  // 맨 앞 요소 반환

        Node peekNode = last;

        if(first == null) {
            while(true){
                if (peekNode.getBeforeNode() != null) {
                    peekNode = peekNode.getBeforeNode();
                } else {
                    first = peekNode;
                    break;
                }
            }
        }
        return first.getData();
    }

    public int poll(){  // 맨 앞 요소 반환 후 삭제
        int val = 0;
        Node peekNode = last;

        while(true){
            if(peekNode.getBeforeNode() != null){
                second = peekNode;
                peekNode = peekNode.getBeforeNode();
            }else{
                val = peekNode.getData();
                first = second;
                break;
            }
        }
        return val;
    }
    
}
```