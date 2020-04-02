# 그래프 순회
: 그래프의 모든 노드를 방문하는 일

**대표적인 두가지 방법**
- BFS(Breadth-First Search, 너비우선순회)
- DFS(Depth-First Search, 깊이우선순회)

## BFS(너비우선순회)
: 그래프에서 노드들을 동심원의 형태로 탐색(순회)하는 방법이다.
![](https://github.com/brightestbulb/TIL/blob/master/Algorithm/img/BFS1.png?raw=true)   

## BFS 구현 방법
### 1. 큐를 이용한 BFS
![](https://github.com/brightestbulb/TIL/blob/master/Algorithm/img/BFS2.png?raw=true)   
![](https://github.com/brightestbulb/TIL/blob/master/Algorithm/img/BFS3.png?raw=true)   
반복문(while)을 통해 다음 작업을 반복한다.   
1에 인접한 노드들 중 아직 체크를 안한 2, 3을 Queue에 넣는다. (순서 중요 X)   

![](https://github.com/brightestbulb/TIL/blob/master/Algorithm/img/BFS4.png?raw=true)   
2에 인접한 노드는 1, 4, 5인데 1은 이미 체크 됬으므로 체크가 안된 4, 5를 Queue에 넣는다.   

이러한 과정들을 통해 Queue에 담긴 모든 노드에 인접한 노드들을 체크하고 Queue에서 제거 하면 다음과 같다.
![](https://github.com/brightestbulb/TIL/blob/master/Algorithm/img/BFS5.png?raw=true)   

#### BFS와 최단 경로
BFS는 모든 노드를 방문하는것 이상의 중요한 일을 할 수 있는데 그것이 **최단 경로** 이다.   
![](https://github.com/brightestbulb/TIL/blob/master/Algorithm/img/BFS6.png?raw=true)    

입력 : 방향 혹은 무방향 그래프 G=(V, E), 그리고 출발 노드 s∈V
출력 : 모든 노드 V에 대해서
- d[V] = s로 부터 V까지의 최단 경로의 길이(에지의 개수)
- ⫪[V] = s로 부터 V까지의 최단 경로상에서 V에 도착 직전 노드
이 두가지 값을 계산한다.



출처 : https://www.youtube.com/watch?v=O7pDLEMsiBs&list=PLB9fFuvSr1RcP6NzrfE1Ot2OrWY8ocdVg&index=30