# Stack

## 자바 LinkedList 이용해서 Stack 구현
```java
public class LinkedListStack {

    List<Integer> list = new LinkedList<Integer>();

    public void push(int num){
        list.add(num);
    }

    public int pop(){
        int num = list.get(list.size()-1);
        list.remove(list.size()-1);
        return num;
    }

    public int peek(){
        return list.get(list.size()-1);
    }

    public Boolean empty(){
        Boolean b = false;
        if(list.size()==0){
            b = true;
        }
        return b;
    }

    public int search(int num){
        return list.get(num);
    }

}
```


## 연결리스트 개념을 활용하여 Stack 구현
```java
public class Node {

    private int data;
    private Node nextNode;

    public Node(int data){
        this.data = data;
        this.nextNode = nextNode;
    }

    public void linkNode(Node node){
        this.nextNode = node;
    }

    public int getData(){
        return this.data;
    }

    public Node getNextNode(){
        return this.nextNode;
    }
}

public class LinkedListStack {

    Node top;

    public LinkedListStack(){
        this.top = null;
    }

    private void push(int data){
        Node node = new Node(data);
        node.linkNode(top);
        top = node;
    }
}
```