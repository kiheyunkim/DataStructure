# Queue

##### 정의: 데이터가 뒤로만 들어오고 앞으로만 나가는 선형자료구조(데이터의 앞(front)에서 데이터가 빠지고 뒤(back)에서 데이터를 넣는다.)



* ### 용어

  * FIFO: First In First Out(먼져 들어온 데이터는 가장 먼저 나감,선입 선출)

  * Front: 제일 앞에 있는 데이터가 있는 위치.

  * Back: 제일 뒤에 있는 마지막 데이터가 있는 위치.

  * Enqueue: Queue의 가장 마지막에 데이터를 추가함.

  * Dequeue: 큐의 가장 앞에 데이터를 제거함.

    

* ### 구현

  ```c++
  #include<iostream>
  
  class Queue
  {
  private:
  	int* dataArray;
  	int size;
  	int front;
  	int back;
  
  public:
  	Queue(int size = 10) :
  		size(size),
  		dataArray(new int[size]),
  		front(0),
  		back(0)
  	{}
  
  	~Queue()
  	{
  		delete[] dataArray;
  	}
  
  	int GetFront()
  	{
  		return dataArray[front];
  	}
  
  	int GetFack()
  	{
  		return dataArray[back];
  	}
  
  	void Enqueue(int data)
  	{
  		if (back == size)	//뒷꽁무니가 배열 크기에 도달
  		{
  			if (front != 0)	//앞쪽에 빈칸이 있으면 당겨줌
  			{
  				for (int i = front; i < back; ++i)//당겨줌
  					dataArray[i - front] = dataArray[i];
  
  				//위치 재조정
  				back -= front;
  				front = 0;
  
  				//삽입
  				dataArray[back] = data;
  			}
  			else // 완전히 꽉찬 경우 배열을 새로 할당해줌
  			{
  				int newSIze = size * 2;
  				int* newArray = new int[newSIze];
  
  				for (int i = front; i < back; ++i)
  					newArray[i] = dataArray[i];
  
  				delete[] dataArray;
  				dataArray = newArray;
  				size = newSIze;
  
  				dataArray[back] = data;
  			}
  		}
  		else
  			dataArray[back] = data;		//해당없으면 삽입
  
  		++back;
  	}
  
  	void Dequeue()
  	{
  		if (front == back) return;
  
  		++front;
  	}	
  };
  ```

  

* ### 사용

  ```c++
  int main()
  {							//ㅁ는 빈칸
  	Queue q(2);				//front ㅁ - ㅁ - back
  	q.Enqueue(1);			//front  1 - ㅁ - back
  	q.Enqueue(2);			//front  1 -  2 - back
  	q.Enqueue(3);			//front  1 -  2 - 3 - ㅁ - back
  	q.Enqueue(4);			//front  1 -  2 - 3 -  4 - back
  	q.Dequeue();			//front ㅁ -  2 - 3 -  4 - back
  	q.Enqueue(10);			//front  2 -  3 - 4 - ㅁ - back   
      						//	--> //front  2 -  3 - 4 - 10 - back
  	q.Enqueue(11);			//front  2 -  3 - 4 - 10 - 11 - ㅁ - ㅁ - ㅁ back
  }
  ```

  

* ### 사용하는 곳

  * 순차적으로 처리하는 일을 저장할 때