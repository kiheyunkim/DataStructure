# Tree

정의: 선형의 데이터 구조와는 다르게 **계층적인 구조**를 가지 있는 자료구조



## 용어

* 노드(Node): 트리의 구성 요소 하나하나를 노드라고 한다.
* 루트(Root): 계층적 구조에서 가장 높은 곳에 있는 노드를 Root 라고 한다.
* 서브 트리(SubTree): Root를 기준으로 밑에 있는 노드들을 묶어 서브 트리라고 한다.
* 간선(Edge): 노드와 노드를 잇는 연결선
* 부모 노드(Parent Node): 한 노드의 바로 위에 있는 노드
* 자식 노드(Children Node): 한 노드의 왼쪽이나 오른쪽 아래에 있는 노드
* 형제관계(sibling): 같은 레벨에 있는 노드들의 관계를 의미
* 조상노드(Acenstor Node): Root로 부터 임의의 노드까지의 모든 노드
* 자손노드(Descendent Node):  노드의 하위에 연결된 모든 노드
* 단말노드(Terminal Node, Leaf Node): 자식이 없는 노드
* 차수(Degree): 어떤 노드가 가지고 있는 자식 노드의 개수
* 레벨(Level): 루트를 Level 1로 잡고 각 층마다 1씩 증가하여 번호를 매김
* 높이(Height): 트리가 가지고 있는 최대 레벨.



## 이진트리

가장 많이 쓰이는 트리 ,즉 자식을 2개씩만 가지고 있는 트리

* 이진트리의 특징
  * 이진 트리의 모든 노드는 차수가 2 이하이다.
  * 노드가 하나도 없을 수도 있다.
* 이진트리의 성질
  * N개의 노드를 가진 이진 트리는 N-1개의 Edge를 가진다.
  * 높이가 h인 이진 트리의 경우 최소 h개, 최대 2^(h-1)개의 노드를 가진다.
  * n개의 노드를 가지는 이진 트리의 높이는 최대 n, 최소 log(n+1)/log2의 올림
  * 이진트리의 분류
    * 포화 이진 트리 : 높이 k의 트리에서 2^k-1개의 노드를 가진 트리.
    * 완전 이진 트리: 높이가 k인 트리에서 레벨 1~k-1에는 노드들은 모두 채워져있고 마지막 레벨에서만 왼쪽 부터 순서대로 채워져 있는 이진트리.



## 이진 트리의 구현 방법

* 동적 할당을 통한 구현

  * 각 노드들을 각각 구현 후,포인터를 직접 연결하여 구현

    ```c++
    struct TreeNode
    {
        int data;
        TreeNode* left,right;
    }
    ```

    

* 배열을 이용한 구현

  * 배열을 선언한 뒤 규칙에 따라서 부모, 자식 관계를 구성한다.
  * 루트가 1인 경우
    * 현재 노드 위치가 N일때 왼쪽 자식은 Nx2, 오른쪽 자식은 Nx2 +1의 인덱스를 가진다.
    * 현재 노드의 위치가 N일때 부모는 N/2의 인덱스를 갖는다.

* 구현과 사용

  * 배열

    ```C++
    struct TreeNode
    {
        int data;
    }
    
    TreeNode tree[100];
    
    int main()
    {
        for(int i=0;i<100;++i)
        	tree[i].data = -1	//비어있는 곳을 체크하기 위해 -1로 초기화
        
        tree[1].data = 2;       //Root 트리
        tree[1*2].data = 3;     //Root 왼쪽 자식
        tree[1*2+1].data = 4    //Root 오른쪽 자식
    
        return 0;
    }
    ```

  * 동적할당

    ```c++
    struct TreeNode
    {
        int data;
        TreeNode* left;
        TreeNode* right;
        TreeNode():
        left(nullptr),
        right(nullptr),
        data(0){}
    }
    
    int main()
    {
        TreeNode* nodes = new TreeNode[3];
    
        nodes[0].data = 2;          //Root
        node[0].left = node[1];     //왼쪽 자식과의 Edge 설정
        node[0].right = node[2];    //오른쪽 자식과의 Edge 설정
    
        node[1].data = 3;           //Root의 왼쪽 자식
        node[2].data = 4;           //Root의 오른쪽 자식
    
        return 0;
    }
    ```



## 트리의 순회

이진 트리를 탐색하기위한 방법 3가지

![Img](image/Tree.png)

* 전위 순회(Preorder Traversal) 

  * ```c++
    void PreOrder(TreeNode* pos)
    {
        print(pos->data);
        if(pos->left != nullptr)
    	    PreOrder(pos->left);
        if(pos->right != nullptr)
    	    PreOrder(pos->right);
    }
    ```

  * 순회 결과 : 1 - 2 - 4 - 9 - 5 - 10 - 11 - 3 - 6 - 13 - 7- 14

    

* 중위 순화(Inoder traversal)

  * ```c++
    void InOrder(TreeNode* pos)
    {
        if(pos->left != nullptr)
    	    PreOrder(pos->left);
     	print(pos->data);
        if(pos->right != nullptr)
    	    PreOrder(pos->right);
    }
    ```

  * 순회 결과 : 8 - 4 - 9 - 2 - 10 - 5 - 11 - 1 - 6 - 13 - 3 - 14 - 7

* 후위 순회(Postorder traversal)

  * ```c++
    void PostOrder(TreeNode* pos)
    {
        if(pos->left != nullptr)
    	    PreOrder(pos->left);
        if(pos->right != nullptr)
    	    PreOrder(pos->right);
        print(pos->data);
    }
    ```

  * 순회 결과 :  8 - 9 - 4 - 10 - 11 - 5 - 2 - 13 - 6 - 14 - 7 - 3 - 1



## 이진 트리 사용처

* 수식 계산
* 디렉터리 탐색



## 이진 트리 확장

* 이진탐색 트리
* 우선순위 큐
* 힙



# Reference

Img1: [Link](https://www.geeksforgeeks.org/difference-between-binary-tree-and-binary-search-tree/)