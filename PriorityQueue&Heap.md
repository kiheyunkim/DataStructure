# Priority Queue(우선순위 큐)

우선순위가 높은 순서대로 나가는 Queue 기존의 Queue가 먼저 들어온 것이 먼져 나간다면 Priority Queue에서는 우선순위가 높은 것이 먼져 나간다. 

### 

* ## 구현

  * 배열로 구현

    ```C++
    #define ARRAY_SIZE 100 + 1
    
    void Swap(int& target1, int& target2)
    {
    	int temp = target1;
    	target1 = target2;
    	target2 = temp;
    }
    
    //1. 배열을 이용
    int arrayPriorityQueue[ARRAY_SIZE];
    int currentSize = 0;
    
    void ArrayPriorityQueueAdd(int data)
    {
    	arrayPriorityQueue[currentSize++] = data;
    
    	for (int i = currentSize - 1; i > 0; --i)
    	{
    		if (arrayPriorityQueue[i] < arrayPriorityQueue[i - 1])
    			Swap(arrayPriorityQueue[i], arrayPriorityQueue[i - 1]);
    		else
    			break;
    	}
    }
    
    int ArrayPriorityQueuePop()
    {
    	int retval = arrayPriorityQueue[0];
    	--currentSize;
    
    	for (int i = 0; i < currentSize; ++i)
    		Swap(arrayPriorityQueue[i], arrayPriorityQueue[i + 1]);
    
    	return retval;
    }
    ```

    * 정렬을 계속 시도하도록 짰으므로 삽입과 삭제 모두 O(N)을 갖는다. 만약 삽입때는 배열의 마지막에 넣고 삭제에서 탐색을 통해서 찾게 되면 삽입과 삭제는 각 O(1) 과 O(N)의 속도를 갖는다.
      

  * 리스트를 이용한 구현

    ```c++
    #include<list>
    
    std::list<int> listPriorityQueue;
    void ListPriorityQueueAdd(int data)
    {
    	std::list<int>::iterator findIter = listPriorityQueue.begin();
    	while (findIter != listPriorityQueue.end())
    	{
    		if (*findIter > data)		//더 큰 순간.
    			break;
    		++findIter;
    	}
    	listPriorityQueue.insert(findIter, data);
    }
    
    int ListPriorityQueuePop()
    {
    	int retval = listPriorityQueue.front();
    	listPriorityQueue.pop_front();
    	return retval;
    }
    ```

    * STL 사용은 구현 편의를 위함.

    * 추가할때 삽입하려는 데이터보다 큰 값을 만나면 그 값 앞에 데이터를 삽입한다. O(N)

    * 삭제할떄는 가장 앞에 는 데이터가 가장 작으므로 이를 반환한다 O(1)

      

  * Heap을 이용한 구현

    ```c++
    #define ARRAY_SIZE 100 + 1
    
    int heapPriorityQueue[ARRAY_SIZE];
    int heapIndex = 1;
    void HeapPriorityQueueAdd(int data)
    {
    	heapPriorityQueue[heapIndex] = data;
    	int cur = heapIndex++;
    
    	while (cur != 1)
    	{
    		if (heapPriorityQueue[cur] < heapPriorityQueue[cur / 2])
    			Swap(heapPriorityQueue[cur], heapPriorityQueue[cur / 2]);
    		else
    			break;
    
    		cur /= 2;
    	}
    }
    
    int HeapPriorityQueuePop()
    {
    	int retval = heapPriorityQueue[1];
    	heapPriorityQueue[1] = heapPriorityQueue[heapIndex-1];
    	--heapIndex;
    
    	int cur = 1;
    
    	while (true)
    	{
    		if (heapPriorityQueue[cur * 2] > heapPriorityQueue[cur * 2 + 1])	//작은걸 우선해야함
    		{
    			if (heapPriorityQueue[cur] > heapPriorityQueue[(cur * 2) + 1] &&
                    (cur * 2) + 1 < heapIndex)
    			{
    				Swap(heapPriorityQueue[cur], heapPriorityQueue[(cur * 2) + 1]);
    				cur = (cur * 2) + 1;
    			}
    			else
    				break;
    		}
    		else
    		{
    			if (heapPriorityQueue[cur] > heapPriorityQueue[cur * 2] &&
                    cur *2 < heapIndex)
    			{
    				Swap(heapPriorityQueue[cur], heapPriorityQueue[cur * 2]);
    				cur = cur * 2;
    			}
    			else
    				break; 
    		}
    	}
    
    	return retval;
    }
    ```

    * Heap의 정의: 부모의 Key값이 그 자식의 Key값 보다 항상 작도록 만들어진 이진 트리를 의미.

    * Heap의 삽입과정

      1. 힙은 새로운 값을 가장 마지막 위치에 배치한다.

      2. 이 값이 루트이거나 부모가 더 작아질 때 까지 자신의 위치를 현재의 부모와 비교하고 교체한다. 이과정에서 모든 데이터와의 비교가 아니라 자신의 루트와의 비교만을 계속 하므로 속도는 O(logN/log2)가 된다. (최소 우선순위 큐 기준)

         

    * Heap의 삭제 과정

      1. Root를 삭제하고 가장 마지막에 있는 요소를 Root로 옮긴다.

      2. 이 값이 끝에 도달하거나 자신의 자식들이 모두 자신보다 클때 까지 비교하고 교체한다. 만약 자식 둘이라면 그중에서 더 작은 요소와 교체한다.(최소 우선순위 큐 기준)

         

* ### 큐의 이용법

  * 정렬( O(logN/log2)  )
  * 허프만 코드