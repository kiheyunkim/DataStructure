# Graph(그래프)



**정의: 연결되어있는 객체간의 관계를 표현할 수 있는 자료 구조.**



그래프로 표현하는 것:  지도, 도로, 선수과목등



### 용어

* Vertex(정점): 객체 각각을 의미
* Edge(간선): 정점들을 연결하는 선
* Adjacent Vertex(인접 정점): 간선에 의해 직접 연결된 정점
* Degree(차수): 하나의 정점에서 인접한 정점의 수
* In-Dgree(진입 차수): 외부에서 들어오는 간선의 수(방향 그래프)
* Out-Dgree(진출 차수): 외부로 향하는 간선의 수(방향 그래프)
* Path Length(경로 길이): 경로를 구성하는데 사용된 간선의 수
* Simple Path(단순 경로): 경로중에서 반복되는 간선이 없는 경우의 경로
* Cycle(사이클): 단순 경로의 시장 정점과 종료 정점이 동일함
* Connected Graph(연결 그래프): 무방향 그래프에 있는 모든 정점쌍에 대하여 항상 경로가 존재한 그래프
* Complete Grapg): 그래프에 속해있는 모든 정점이 서로 연결되어있는 그래프





### 그래프의 종류

* 무방향 그래프: 양 방향으로 갈 수 있는 그래프.
* 방향 그래프: 일방 통행과 같이 한 방향으로만 갈 수 있는 그래프.
* 가중치 그래프: 정점 간의 연결의 중요도가 정해져있는 그래프.



### 그래프의 표현방법

* 인접 행렬

  ```c++
  #define GRAPH_DEGREE 10
  bool graph[GRAPH_DEGREE][GRAPH_DEGREE];
  ```

   간선을 표현할 떄는 정점 번호와 정점 번호를 x,y라고 했을때 

  ```
  graph[x][y]=1 없으면 0
  ```

  로 표현한다. x,y 순서에 따라 방향성 표현도 가능하다.

  * 장점

    * 구현이 간단하다

  * 단점

    * 간선 조사에 n^2번 조사를 해야한다.

    * 없는 간선에 대해서도 표현을 해야하므로 메모리 낭비가 심하다.

      

* 가변 배열을 통한 표현

  ```c++
  #include<vector>
  #define GRAPH_DEGREE 10
  
  std::vector<std::vector<int>> graph(GRAPH_DEGREE);
  ```

  간선을 표현할 때 정점 x와 y를 연결할때

  ```c++
  graph[x].push_back(y);
  ```

  와 같이 추가한다.

  * 장점
    * 구현이 간단하다.
    * 메모리 절약된다.
    * 간선 조사에 시간 낭비가 없다
  * 단점
    * 직접 구현에 어렵다.