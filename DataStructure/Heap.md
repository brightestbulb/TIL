# Heap

트리의 선회 방법에는 3가지가 있다.   
- 중위 순회 Left -> Root -> Right
- 전위 순회 Root -> Left -> Right
- 후위 순회 Left -> Right -> Root

Root를 기준으로 이름이 붙여졌고, 나머지는 Left, Right 순서로 진행된다.

![](https://github.com/brightestbulb/TIL/blob/master/DataStructure/img/Heap.png?raw=true)

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

반복문을 사용한 예시와, 재귀를 사용한 예시 두가지로 중위 순회를 직접 구현했다.
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

    //  [반복문] 중위 순회  ( 4->2->5->1->3 )
    public List<Integer> inOrder(Node root){

        List<Node> box = new ArrayList<Node>();
        List<Integer> result = new ArrayList<Integer>();

        box.add(root);
        Node node = root.getLeft();
        while(node != null){
            box.add(node);
            Node leftNode = node.getLeft();
            if(leftNode == null){  // left 제일 마지막이면
                node = null;
            }else{
                box.add(leftNode);
                node = leftNode.getLeft();
            }
        }

        for(int i=box.size()-1; i>=0; i--){
            Node n = box.get(i);
            result.add(n.getData());

            Node right = n.getRight();
            if(right != null){
                result.add(right.getData());
            }
        }

        return result;
    }

    // [Recursive]
    public void inOrderRecursive(Node node) {
        if(node != null) {
            inOrderRecursive(node.left);
            System.out.println(node.data);
            inOrderRecursive(node.right);
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


        // 반복문으로 구현한 중위 선회 출력 
        List<Integer> result = tree.inOrder(n1);

        for(int i=0; i<result.size(); i++){
            System.out.println(result.get(i));        
        }

        // 재귀로 구현한 중위 선회 출력
        tree.inOrderRecursive(n1);

    }
```