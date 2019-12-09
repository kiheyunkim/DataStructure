# Deque



##### 정의: Queue의 방향성을 한쪽 방향이 아닌 양쪽으로 구현한 선형 자료구조(front, back 모두에서 데이터의 삽입과 삭제가 이루어짐).

##### 등장배경: vector의 메모리 할당 구조에 따른 비효율적인 삭제와 삽입을 해결하기 위하여 만들어진 자료구조



* ### 용어

  * Front: Deque의 앞쪽

  * Back: Deque의 뒤쪽

    

* ### 구현(List로 구현)

  ```c++
  #include<iostream>
  
  struct ListNode
  {
  	int data;			//데이터 저장
  	ListNode* prev;		//바로 앞 노드 가리킴
  	ListNode* next;		//바로 뒤 노드 가리킴
  };
  
  class Deque
  {
  private:
  	ListNode* head;		//리스트의 제일 앞을 가리킴
  	ListNode* tail;		//리스트의 가장 뒤를 가리킴
  
  public:
  	Deque() :
  		head(new ListNode),
  		tail(new ListNode)
  	{
  		head->next = tail;	//처음 추가를 간편하게 하기위해
  		tail->prev = head;	//빈 노드 2개를 추가하여 구현
  	}
  
  	~Deque()
  	{
  		delete head;
  		delete tail;
  	}
  
  	void PushFront(int data)
  	{
  		ListNode* newNode = new ListNode();		
  		newNode->data = data;
  
  		newNode->prev = head;
  		newNode->next = head->next;
  		head->next->prev = newNode;
  		head->next = newNode;
  	}
  
  	void PushBack(int data)
  	{
  		ListNode* newNode = new ListNode();
  		newNode->data = data;
  
  		newNode->prev = tail->prev;
  		newNode->next = tail;
  		tail->prev->next = newNode;
  		tail->prev = newNode;
  	}
  
  	bool isEmpty()
  	{
  		return head->next == tail;
  	}
  
  	void PopFront()
  	{
  		if (isEmpty()) return;
  
  		ListNode* tempNode = head->next;
  		tempNode->prev->next = tempNode->next;
  		tempNode->next->prev = tempNode->prev;
  
  		delete tempNode;
  	}
  
  	void PopBack()
  	{
  		if (isEmpty()) return;
  
  		ListNode* tempNode = tail->prev;
  		tempNode->prev->next = tempNode->next;
  		tempNode->next->prev = tempNode->prev;
  	}
  };
  ```

  

* ### 사용

  ```c++
  int main()
  {
  	Deque d;
  	d.PushFront(1);		//front - 1 - back
  	d.PushFront(2);		//front - 2 - 1 - back
  	d.PushFront(3);		//front - 3 - 2 - 1 - back
  	d.PushFront(4);		//front - 4 - 3 - 2 - 1 - back
  	d.PushBack(8);		//front - 4 - 3 - 2 - 1 - 8 - back
  	d.PushBack(7);		//front - 4 - 3 - 2 - 1 - 8 - 7 - back
  	d.PushBack(6);		//front - 4 - 3 - 2 - 1 - 8 - 7 - 6 back
  	d.PushBack(5);		//front - 4 - 3 - 2 - 1 - 8 - 7 - 6 - 5 - back
  
  	return 0;
  }
  ```

  

* ### 사용하는 곳

  * 연산이 앞 뒤로만 아루어 지는 경우에 사용