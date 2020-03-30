# Graph
## 그래프의 종류
### 1.무방향 그래프
그래프는 노드(정점)와 에지(링크)로 구성되어 있다.   
**G = (V, E)**  
- V : 노드 또는 정점
- E : 노드를 연결하는 edge(에지) 또는 링크
- n : 노드의 갯수
- m : 에지의 갯수

무방향 그래프
![](https://github.com/brightestbulb/TIL/blob/master/Algorithm/img/graph1.png?raw=true)

### 2.방향 그래프
에지 (u, v)는 u로부터 v로의 방향을 가진다.

![](https://github.com/brightestbulb/TIL/blob/master/Algorithm/img/graph2.png?raw=true)

### 3.가중치 그래프
에지마다 가중치(weight)가 지정


## 그래프의 표현
### 1. 인접 행렬 (무방향)
: 정점의 갯수가 n개인 그래프를 nxn 행렬로 표현하는 방법이다.
예를들어 노드 1, 2 사이에 에지가 있으면 **1**, 없으면 **0** 으로 표현한다. 
![](https://github.com/brightestbulb/TIL/blob/master/Algorithm/img/graph3.png?raw=true)

인접 행렬에서 중요한 연산 두가지는 다음과 같다.   
첫째, 그래프에서 어떠한 노드에 인접한 모든 노드를 찾기 위해서는 O(n) 시간이 필요하다.   
왜냐하면, 인접한 노드를 찾기위해 위 이미지의 행렬에서 해당 노드에 해당하는 행을 전부 다 읽어들이기 때문이다.   
그래서 실제 인접한 노드의 갯수와 무관하게 O(n)이 필요하다.   
   
둘째, 두 정점 u, v에 대해서 연결되어 있는지, 즉 에지가 있는지 검사하는 연산이 기본적인 연산이다.
이 경우에는 위 행렬에서 u행과 v열에 값이 0인지 1인지만 확인하면 되므로 O(1) 시간이 필요하다.

### 2. 인접 리스트 (무방향)
![](https://github.com/brightestbulb/TIL/blob/master/Algorithm/img/graph4.png?raw=true)
다음과 같이 노드가 5개인 그래프가 있을 때, 각 노드의 숫자는 배열의 각 index를 나타낸다.   
그리고 각 칸에 연결리스트를 넣는데, 이 노드에 인접한 노드들의 번호를 연결리스에 넣어서 저장하는 방식이다. (순서는 중요X)   

연결리스트의 노드의 총 갯수는 2m개 이다. ( m : 에지의 갯수 )    
따라서 인접 리스트로 그래프를 표현하게 되면 필요한 저장 공간은 O(n+m)이다. (n : 노드 갯수, m : 에지의 갯수)    

어떤 노드에 v에 인접한 모든 노드를 찾으려면, 해당 노드에 있는 연결리스트의 길이에 비례하는 시간인데,
연결리스트의 길이가 실제로 인접한 노드의 갯수이므로 보통 이것을 degree라고 부른다.   
그래서 어떤 노드 v에 인접한 모든 노드를 찾으면 O(degree(v)) 시간이 필요하고, 이 연산은 인접 행렬보다 유리하다.   
반면에 어떤 에지(u, v)가 존재하는지 검사하려면 O(degree(u)) 시간이 필요하고, 에지(3, 5)에 에지가 있는지 확인하려면
배열 index 2에 있는 연결 리스트에 5 노드가 있는지 확인해야 하므로 인접 행렬보다 안좋다.   
 
 
### 3. 방향 그래프
![](https://github.com/brightestbulb/TIL/blob/master/Algorithm/img/graph5.png?raw=true)


### 4. 가중치 그래프의 인접행렬 표현
에지의 존재를 나타내는 값으로 1대신 에지의 가중치를 저장하는 방법을 사용한다.   
에지가 없을 때 혹은 대각선 인 경우에는 특별히 정해진 규칙은 없으며, 그래프와 가중치가 의미하는 바에 따라서   
- 가중치가 거리 혹은 비용을 의미하는 경우 : 행렬에 에지가 없으면 무한대로 표기, 대각선은 0
- 가중치가 용량을 의미한다면 행렬에 에지가 없을때 0, 대각선은 무한대로 표기


## 그래프에서 사용하는 기본 용어
![](https://github.com/brightestbulb/TIL/blob/master/Algorithm/img/graph6.png?raw=true)