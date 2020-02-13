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

    public int search(int num){

        HashMap<Integer, Object> map = new HashMap<Integer, Object>();

        int i = 1;

        map.put(i, this.topNode.getData());

        this.topNode = recursiveGetNode(this.topNode);
        while(this.topNode != null){
            map.put(++i, this.topNode.getData());
            this.topNode= recursiveGetNode(this.topNode);
        }

        int size = map.size()+1;
        return (Integer)map.get(size-num);
    }

    private Node recursiveGetNode(Node node){
        node = this.topNode.getbeforeNode();
        return node;
    }
}
```