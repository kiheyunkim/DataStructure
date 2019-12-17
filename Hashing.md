# 해싱(Hashing)

키 값에 직접 산술적인 연산(Hash Function)을 적용하여 항목이 저장되어있는 테이블의 주소를 찾고 항목에 접근하는 탐색법



* ### 용어

  * Hash Table(해시 테이블): 해싱을 이용하기 위한 테이블

  * Hasing(해싱) 해시 테이블을 이용한 탐색

  * Has Function(해시 함수): 탐색 키를 입력 받아 해시 주소를 생성하고 이 주소는 해시 테이블의 인덱스가 된다.

  * Collision(충돌): 해시 테이블에서 해시함수를 통해서 만들어진 인덱스로 접근했을 때, 이미 기존의 값이 들어가있을 때 충돌이 일어났다고 함.

  * Overflow(오버플로): 해시 테이블의 모든 공간에 데이터가 들어가 있을 때를 의미한다. 

    
    

* ### 해싱의 구조

  * 해싱에서는 배열을 사용한다. 배열을 사용하면 인덱스를 통해서 빠르게 접근할 수 있다는 장점이 있기 때문이다. 해싱의 구현 방식에 따라서 조금 다르게 구현되는데, 배열을 사용해서 만들기도 하지만 연결 리스트 배열을 이용해서 만들기도 한다. 



* ### 이상적인 해싱?

  * 이상적인 해싱
    * 모든 각 데이터들이 자신만의 고유한 인덱스를 갖게 되고 서로간의 충돌이 없는 해싱이고 , 데이터의 개수보다 한없이 많은 배열의 크기를 가지기 때문에 역시 충돌이 방지된다.
  * 현실적인 해싱
    * 해시 테이블의 크기는 제한되어 있으므로 탐색키 전체(모든 경우의수)의 대응하는 크기 설정은 불가능하다. ex(주민번호로 국민 전체를 한다면 10^13 크기의 배열을 가져야 함)
    * 이를 해결하기 위해서 탐색키를 테이블의 크기로 Mod연산을 해서 그 나머지를 주소로 가져간다.



* ### 해시 함수

  * 해싱에서는 해시 테이블의 주소로 변환하는 해시 함수가 잘 설계되었을 때 효율을 증대 시킬 수 있다. 다음은 좋은 해시함수의 조건이다.

    * 충돌이 적어야 한다.

    * 해시 함수 값이 해시 테이블의 주소 영역 내에서 고르게 분포되어야 한다.

    * 계산이 빨라야 한다.

      

  * 사용하는 해시함수의 종류

    * 제산 함수: 나머지 연산자를 사용해서 탐색 키를 해시 테이블의 크기로 나눈 나머지를 해시로 사용하는 방법, 이 경우 테이블의 크기는 어느 수로도 나누어 지지 않는 소수(Prime Number)을 선택한다. h(k) = k mod M (M은 테이블 크기, k는 키값, h는 해시 함수)

      

    * 폴딩 함수: 탐색 키를 여러 부분으로 나누어 모두 더한 값을 해시 주소로 사용하는 방법이다. 대표적으로 이동 폴딩(Shift Folding), 경계 폴딩(Boundary Folding)이 대표적이다. 

      * 이동 폴딩: 여러 부분으로 나눈 값을 모두 더함

      * 경계 폴딩: 여러부분으로 나는 값을 거꾸로 더하여 해시 주소를 얻는다.

        

    * 중간 제곱 함수: 탐색 키를 제곱한 다음, 중간의 몇 비트를 취해서 해시 주소를 생성하는 함수.

    * 비트 추출 방법: 테이블의 크기가 2^k일 떄 탐색 키를 이진수로 간주하여 임의의 위치의 k개의 비트를 해시 주소로 사용하는 것, 하지만 충돌의 가능성이 높다.

    * 숫자 분석 방법: 숫자로 구성된 키에서 각각의 위치에 있는 수의 특징을 미리 알고 있을 때 편중되지 않은 수를 해시 테이블의 인텍스에 알맞게 조합나다.

      

  * 탐색 키가 문자열일 경우 주의할 점

    * 탐색키들은 정수일 때는 주소 변환하기가 좋다 하지만 대부분은 문자열이다.

    * 대체는 문자열각각에 정수를 할당한다.(1~26) 하지만 이 방법을 사용하면 크게 차이 나지 않는 문자들에 대해서 충돌이 일어날 확률이 매우 높다.

    * 따라서 모든 문자를 골고루 사용하되 각 자리자리 마다 위치에 따른 값을 곱해준다.

      ```
      //Example 총 n자리
      h[0] * x^n-1 + h[1] * x^n-2 + ...... + h[n-1] * x^0
      ```



* ### 충돌의 해결

  * 위의 방법들을 통해서 충돌을 최대한 막을 수는 있지만 완벽하게 고유의 값을 뽑아내는 함수를 만들어 내지 못하면 충돌이 일어나도록 되어있다. 충돌이 일어나는 경우에 데이터의 저장은 2가지 방법을 통해서 해결할 수 있다.

    

  * #### 선형 조사법(linear Probing), 개방 주소법(Open Addressing)

    현재 인덱스 n에 데이터가 있다면 n+1을 조사한다. n+1에도 데이터가 있다면 n+2를 조사한다. 이와같이 선형적으로 조사를 하게된다면 언젠가는 빈 칸을 찾게 될 것이다. 그러나 선형으로 찾게 되면 **군집화** 현상까지 발생할 수 있다.

    
    * 선형 조사법의 문제를 해결하기 위한 방법들

      * 이차 조사법: 조사할 위치를 조사할 때 현재 인덱스를 제곱해 가며 삽입 위치를 찾는다.

      * 이중 해싱법(Double Hasing): 원래의 해시 함수와는 다른 별개의 해시 함수를 이용하여 균일하게 분포시킨다.

        

  * #### 체이닝(Chaining)

    해시 주소가 같은 탄색 키들을 하나의 리스트에 묶어 두는 방법. 하지만 키가 같은 것들이 몇개인지는 모르기 때문에 리스트로 구현하는 것이 바람직한다.



* ### 해시의 구현

  * ##### 개방 주소법

    ```c++
    #define STR_COUNT 10000
    #define HASH_TABLE_SIZE 20033
  #include <vector>
    #include <string>
  #include <iostream>
    
  std::vector<std::string> strVec;
    unsigned long ToHash(const char* str)
    {
    	unsigned long hash = 5381;
  	int c;
    
  	while (c = *str++)
    		hash = (((hash << 5) + hash) + c) % STR_COUNT;
  
    	return hash % STR_COUNT;
    }
    
  struct HashNode
    {
  	int key;
    	std::string str;
  };
    
    HashNode hashTable[HASH_TABLE_SIZE];
    
  void InitHash()
    {
    	for (int i = 0; i < HASH_TABLE_SIZE; ++i)
    		hashTable[i].key = HASH_TABLE_SIZE;
    }
    
    void AddHash(std::string inputStr)
    {
    	int hashValue = ToHash(inputStr.c_str());
    	int cur = hashValue;
    
    	while (true)
    	{
     		if (hashTable[cur].key == HASH_TABLE_SIZE)
    		{
    			hashTable[cur].str = inputStr;
    			hashTable[cur].key = strVec.size();
    			strVec.push_back(inputStr);
    			break;
    		}
    
    		cur = (cur + 1) % HASH_TABLE_SIZE;
    	}
    }
    
    int FindIndex(std::string inputStr)
    {
    	int count = 0;
    	int hashValue = ToHash(inputStr.c_str());
    	while (count < HASH_TABLE_SIZE)
    	{
    		if (hashTable[hashValue].str == inputStr)
    		{
    			return hashTable[hashValue].key;
    		}
    
    		hashValue = (hashValue + 1) % HASH_TABLE_SIZE;
    		++count;
    	}
    
    	return -1;
    }
    ```
  
    
  
  * ##### 개방 주소법 검증 및 사용
  
    ```
    
    int main(int argc, char* argv[])
    {
    	InitHash();
    
    	for (register int i = 0; i < STR_COUNT; ++i)
    	{
    		int randomLength = 10 + rand() % 20;
    		std::string str;
    		for (int j = 0; j < randomLength; ++j)
    			str.push_back('a' + rand() % 26);
    
    		AddHash(str.c_str());
    	}
    
    
    	int checkCount = rand() % STR_COUNT;
    	int answerCount = 0;
    	for (int i = 0; i < checkCount; ++i)
    	{
    		int pick = rand() % STR_COUNT;
    		if (pick == FindIndex(strVec[pick]))
    			++answerCount;
    	}
    
    	std::cout << (answerCount == checkCount ? "OK" : "NO") << "\n";
    
    	return 0;
    }
    ```
  
    
  
  * ##### 체이닝
  
    ```c++
    #define STR_COUNT 10000
    
    #include <iostream>
    #include <vector>
    #include <cstring>
    
    std::vector<std::vector<std::pair<int, std::string>>> hashVector(STR_COUNT * 2 + 33);
    std::vector<std::string> strVec;
    
    unsigned long ToHash(const char* str)
    {
    	unsigned long hash = 5381;
    	int c;
    
    	while (c = *str++)
    		hash = (((hash << 5) + hash) + c) % STR_COUNT;
    
    	return hash % STR_COUNT;
    }
    
    void AddHash(std::string inputStr)
    {
    	int hashValue = ToHash(inputStr.c_str());
    	hashVector[hashValue].push_back(std::make_pair(strVec.size(), inputStr));
    	strVec.push_back(inputStr);
    }
    
    int FindIndex(std::string inputStr)
    {
    	int hashValue = ToHash(inputStr.c_str());
    	int findLength = hashVector[hashValue].size();
    
    	for (int i = 0; i < findLength; ++i)
    	{
    		if (hashVector[hashValue][i].second == inputStr)
    			return hashVector[hashValue][i].first;
    	}
    
    	return -1;
    }
    ```
  
    
  
  * ##### 체이닝 주소법 검증 및 사용
  
    ```c++
    int main(int argc, char* argv[])
    {
  	for (register int i = 0; i < STR_COUNT; ++i)
    	{
    		int randomLength = 10 + rand() % 20;
    		std::string str;
    		for (int j = 0; j < randomLength; ++j)
    			str.push_back('a' + rand() % 26);
    
    		AddHash(str.c_str());
    	}
    
    
    	int checkCount = rand() % STR_COUNT;
    	int answerCount = 0;
    	for (int i = 0; i < checkCount; ++i)
    	{
    		int pick = rand() % STR_COUNT;
    		if (pick == FindIndex(strVec[pick]))
    			++answerCount;
    	}
    	
    	std::cout << (answerCount == checkCount ? "OK" : "NO") << "\n";
    
    	return 0;
    }
    ```
    
    