# HashTable

## HashTable에 대한 이해

HashTable은 Key/Value를 저장하는 데이터 구조이다.   
예를들어 차 이름을 Key, 가격을 Value로 하고 전체 데이터 양을 10개라고 했을 때,    
1. **index = hashFunc("avante") % 10;** 를 통해서 index 값을 정한다.
2. array[index] = 2000;
이렇게 저장한다.   
 
이러한 방식으로 Key를 찾을 때 **hashFunc()** 을 한번만 수행하면 Array에 저장된 index를 찾을 수 있기 때문에 데이터의 저장과 삭제가 매우 빠르다.   


## Key 충돌
HashTable에는 문제가 있는데 hashFunc(key) % Array.size(); 값이 중복 될 수 있다.   
예를들어 hashFunc(key)값이 0,10,20,30 있다면, 이 값들을 10으로 나눈 나머지가 0으로 중복됨을 알 수 있다.
이로써 키 값이 0으로 중복되는 현상이 발생하는데, 이를 **Key 충돌**이라고 한다.


## Key 충돌 해결 방식 ( Separate Chaining 방식 )
LinkedList를 이용하는 방식으로써 JDK 내부에서 사용하고 있다.
만약 같은 index로 인해 충돌이 발생하면 그 index가 가리키고 있는 배열 저장 공간에 LinkedList 추가를 통해 데이터를 저장하고 중복된 index가 가리키는 값들을 포인터를 통해 관리한다.
이렇게 충돌을 해결한다. 데이터를 가져올 때는 Key에 대한 index를 구한 후, index가 가리키고 있는 LinkedList를 선형 검색하여 해당 key에 대한 데이터를 리턴한다. 


## 해쉬 함수
좋은 해쉬 함수란 데이터를 고르게 분포하여 충돌을 최소화하는 함수이다. 다음은 간단하게 사용할 수 있는 메소드이다.
```java
public int getHashCode(String key){
    char[] ch = key.toCharArray();
    int index = 0;
    for(int i=0; i<key.length(); i++){
        index += index * 31 + ch[i];
    }
    return index;
}

System.out.println(getHashCode("key"));  // 112921
```

## 구현
```java
public class HashTable {

    LinkedList<Node>[] data;

    public HashTable(int size){
        this.data = new LinkedList[size];
    }

    public int getHashCode(String key){
        int hashCode = 0;
        for(char c : key.toCharArray()){
            hashCode += c;
         }
        return hashCode;
    }

    public int convertHashToIndex(int hashCode){
        return hashCode % data.length;
    }

    public void put(String key, String value){

        int hashCode = getHashCode(key);
        int index = convertHashToIndex(hashCode);

        LinkedList<Node> list = data[index];
        Node node = new Node(key, value);

        if(list != null){  //같은 Hash 키 값이 있을 때
            list.add(node);
        }else{
            LinkedList<Node> nodeList = new LinkedList<Node>();
            nodeList.add(node);
            data[index] = nodeList;
        }
    }

    public String get(String key){

        String value = "";

        int hashCode = getHashCode(key);
        int index = convertHashToIndex(hashCode);

        LinkedList<Node> list = data[index];

        for(Node n : list){
            if(n.getKey().equals(key)){
                value = n.getValue();
            }
        }
        return value;
    }
}
```

```java
    public static void main(String[] args){

        HashTable ht = new HashTable(3);
        ht.getHashCode("1");

        ht.put("1", "11");
        ht.put("2","22");
        ht.put("d","dd");
        ht.put("ff","fff");

        System.out.println(ht.get("1"));
        System.out.println(ht.get("2"));
        System.out.println(ht.get("d"));
        System.out.println(ht.get("ff"));

    }
```