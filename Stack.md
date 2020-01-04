# Stack

#### 사전적 정의:  무더기, 더미 

##### 정의:  한쪽 끝에서만 자료를 넣거나 뺄 수 있는 선형자료구조(데이터의 가장 위(TOP)에서 데이터를 빼고 넣는다.)



* ### 용어

  * LIFO: Last In First Out(나중에 들어온 위에 쌓인 데이터가 가장 먼져 나감을 말함, 후입선출)

  * TOP: 데이터가 빠지고 나가는 위치, 가장 위에 있는 데이터를 의미

  * POP: 가장 위에 있는 데이터를 하나 빼냄

  * PUSH: 데이터를 밀어넣음

    

* ### 구현

  ```c++
  #include<iostream>
  
  class Stack
  {
  private:
  	int topIndex;
  	int size;
  	int* stackArray;
  
  public:
  	Stack(int size) :
  		stackArray(new int[size]),
  		topIndex(0),
  		size(size)
  	{
  	}
  
  	~Stack()
  	{
  		delete[] stackArray;
  	}
  
  	void Push(int data)
  	{
  		if (topIndex == size) return;
  
  		stackArray[topIndex++] = data;
  	}
  
  	int Pop()
  	{
  		if (topIndex == 0) return -1;
  		return stackArray[--topIndex];
  	}
  
  };
  ```

  

* ### 사용

  ```c++
  int main(int argc,char* argv[])
  {
  	Stack s(10);                  //10개 사이즈의 스택 선언
  	s.Push(5)                     //5삽입			top - 5
  	s.Push(1);                    //1삽입			top - 1 - 5
  	s.Push(3);                    //3삽입			top - 3 - 1 - 5
  	s.Push(2);                    //2삽입			top - 2 - 3 - 1 - 5
  	s.Push(4);                    //4삽입			top - 4 - 2 - 3 - 1 - 5
  	std::cout << s.Pop() << "\n"; //Top 반환 4		top - 2 - 3 - 1 - 5
  	s.Push(8)                     //8삽입			top - 8 - 2 - 3 - 1 - 5
  
  	return 0;
  }
  ```





* ### 사용하는 곳

  * 괄호검사(짝이 맞는지 확인)
  * 수식 계산(후위 수식 표기 계산)
  * 길찾기(DFS)
