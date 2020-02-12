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
    private Node beforeNode;
    
    public Node(int data){
      this.data = data;
      this.beforeNode = beforeNode;
    }
    
    public void linkNode(Node node){
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

    Node top;

    public LinkedListStack(){
        this.top = null;
    }

    public void push(int data){
        Node node = new Node(data);
        node.linkNode(this.top);
        this.top = node;
    }

    public int pop(){

        if(empty()){
            throw new NullPointerException();
        }
        int data = this.top.getData();
        top = top.getbeforeNode();

        return data;
    }

    public int peek(){
        return this.top.getData();
    }

    public boolean empty(){
        return this.top == null;
    }

    public int search(int num){

        return num;
    }
}
```