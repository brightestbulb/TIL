# Tree

트리의 선회 방법에는 3가지가 있다.   
- 중위 순회 Left -> Root -> Right
- 전위 순회 Root -> Left -> Right
- 후위 순회 Left -> Right -> Root

Root를 기준으로 이름이 붙여졌고, 나머지는 Left, Right 순서로 진행된다.

![](https://github.com/brightestbulb/TIL/blob/master/DataStructure/img/tree.png?raw=true)

- 중위 순회 ( 4-> 2-> 5-> 1-> 3 )
- 전위 순회 ( 1-> 2-> 4-> 5-> 3 )
- 후위 순회 ( 4-> 5-> 2-> 3-> 1 )


```java
public class Node {
    int data;
    Node left;
    Node right;

    //getter, setter
}
```

```java
public class Tree {

    public Node root;

    public Node createNode(int data, Node left, Node right){
        Node node = new Node();
        node.setData(data);
        node.setLeft(left);
        node.setRight(right);

        return node;
    }

   //  중위순회  ( 4->2->5->1->3 )
   public void inOrder(Node node) {
       if(node != null) {
           inOrder(node.left);
           System.out.println(node.data);
           inOrder(node.right);
       }
   }

   //  전위순회  ( 1->2->4->5->3 )
   public void preOrder(Node node) {
       if(node != null) {
           System.out.println(node.data);
           inOrder(node.left);
           inOrder(node.right);
       }
   }

   // 후위순회 ( 4->5->2->3->1 )
   public void postOrder(Node node) {
       if(node != null) {
           inOrder(node.left);
           inOrder(node.right);
           System.out.println(node.data);
       }
   }
}
```

```java
    public static void main(String[] args){

        Tree tree = new Tree();
        Node n5 = tree.createNode(5,null,null);
        Node n4 = tree.createNode(4, null, null);
        Node n3 = tree.createNode(3, null, null);
        Node n2 = tree.createNode(2, n4, n5);
        Node n1 = tree.createNode(1, n2, n3);  // root

        tree.inOrder(n1);
        tree.preOrder(n1);
        tree.postOrder(n1);
    }
```