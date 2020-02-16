# Stack

## 연결리스트 개념을 활용하여 Stack 구현

```java
public class Node {

    private int data;
    private Node beforeNode;

    public Node(int data){
        this.data = data;
        this.beforeNode = beforeNode;
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
public class LinkedListStack {

    Node topNode;

    public LinkedListStack(){
        this.topNode = null;
    }

    public void push(int data){
        Node node = new Node(data);
        node.linkBeforeNode(this.topNode);
        this.topNode = node;
    }

    public int pop(){

        if(empty()){
            throw new NullPointerException();
        }
        int data = this.topNode.getData();
        topNode = topNode.getbeforeNode();

        return data;
    }

    public int peek(){
        return this.topNode.getData();
    }

    public boolean empty(){
        return this.topNode == null;
    }

    public int search(int data){

        Node searchNode = this.topNode;
        int index = 1;

        while(true){
            if(searchNode.getData() == data){
                break;
            }else{
                searchNode = searchNode.getbeforeNode();
                ++index;
                if(searchNode == null){
                    index = -1;
                    break;
                }
            }
        }
        return index;
    }
}
```