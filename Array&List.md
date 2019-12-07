# Linked-List(연결 리스트)

#### 배열(Array)

##### 	정의: 프로그래밍에서 기본적으로 제공되는 연속된 데이터 타입

* 선언

  ```c++
  int dataArray[10];
  ```

* 사용법

  ```c++
  #include <iostream>
  
  int main()
  {
  	int dataArray[10]{ 0,1,2,3,4,5,6,7,8,9 };		//선언
  
  	std::cout << dataArray[1] << "\n";				//Index를 즉시 접근  //1
  
  	int* cur = dataArray;							//이름을 통해 첫 배열 주소 얻음
  	std::cout << *cur << "\n";						//0
  	++cur;											//다음 배열칸으로 이동
  	std::cout << *cur << "\n";						//1
  	cur += 3;										//배열 3칸 이동
  	std::cout << *cur << "\n";						//4
  
  	return 0;
  }
  ```

  

* #### 배열의 장단점
  * ##### 장점

    * 연속된 데이터의 형식이어서 다음 데이터를 찾는것이 매우 빠르다. 
    * Index를 통해서 O(1)의 속도로 임의의 데이터에 접근할 수 있다.

    

  * 단점

    * 배열이 크기가 충분하지 않으면 크기 이상의 데이터를 삽입할 수 없다.
    * 크기를 변경할 수 없다. 너무 크다면 메모리 낭비, 너무 작다면 바꿀 수 없어 저장이 불가능 하다.
    * 메모리의 재사용이 불가능 하다.(이미 할당된 이상 데이터가 차있지 않아도 메모리는 차지하고 있다.)



### 리스트(List)

##### 	정의: 배열의 고정된 크기의 단점을 해결하기 위해서 만들어진 순차적으로 데이터를 저장하는 자료구조



* #### 노드(Node)

  * 데이터를 저장하고 각 노드를 가르키는 자료형

  * 데이터와 포인터 2개로 구성된다.

    ```c++
    struct ListNode
    {
    	int i;				//데이터 저장
    	ListNode* prev;		//바로 앞 노드 가리킴
    	ListNode* next;		//바로 뒤 노드 가리킴
    };
    ```

    

* #### 구현

  * 구현에는 노드를 사용하여

    ```c++
    #include<iostream>
    
    struct ListNode
    {
    	int data;			//데이터 저장
    	ListNode* prev;		//바로 앞 노드 가리킴
    	ListNode* next;		//바로 뒤 노드 가리킴
    };
    
    class List				//이중 연결 리스트
    {
    private:
    	ListNode* head;		//리스트의 제일 앞을 가리킴
    	ListNode* tail;		//리스트의 가장 뒤를 가리킴
    
    public:
    	List():
    		head(new ListNode),
    		tail(new ListNode)
    	{
    		head->next = tail;	//처음 추가를 간편하게 하기위해
    		tail->prev = head;	//빈 노드 2개를 추가하여 구현
    	}
    
    	~List()
    	{
    		delete head;
    		delete tail;
    	}
    
    	void Add(int data)
    	{
    		ListNode* newNode = new ListNode();
    		newNode->data = data;
    
    		tail->prev->next = newNode;
    		newNode->prev = tail->prev;
    		newNode->next = tail;
    		tail->prev = newNode;
    	}
    
    	bool isEmpty()
    	{
    		return head->next == tail;
    	}
    
    	ListNode* find(int data)
    	{
    		ListNode* cur = head->next;
    		while (cur != tail)
    		{
    			if (cur->data == data)
    				return cur;
    
    			cur = cur->next;
    		}
    
    		return nullptr;
    	}
    
    	void Delete(ListNode* node)
    	{
    		node->next->prev = node->prev;
    		node->prev->next = node->next;
    		delete node;
    	}
    };
    ```

  

* 사용

  ```c++
  int main(int argc,char* argv[])
  {
  	List list;								//head - tail
  
  	std::cout << list.isEmpty() << "\n";	//TRUE
  
  	list.Add(1);							//head - 1 - tail
  	list.Add(2);							//head - 1 - 2 - tail
  	list.Add(3);							//head - 1 - 2 - 3 - tail
  	list.Add(4);							//head - 1 - 2 - 3 - 4 - tail
  	list.Add(5);							//head - 1 - 2 - 3 - 4 - 5 - tail
  	
  	std::cout << list.isEmpty() << "\n";	//FALSE
  
  	ListNode* pos = list.find(2);
  	list.Delete(pos);						//head - 1 - 3 - 4 - 5 - tail
  
  	return 0;
  }
  ```

  



* #### 리스트의 장단점

  * 장점
    * 구현하기 쉽다.
    * 크기를 무한정 늘릴 수 있다.(메모리가 허락하는 만큼)
    * 중간에 값을 넣기 쉽다.
  * 단점
    * 데이터를 앞에서 부터 순차적으로 찾아야한다.(헤더 포인터부터 순차탐색)
    * 리스트의 구현에 따라 앞으로 진행할 수 없을 수 있다.(단일 연결 리스트의 경우)

