# cpu-algorithm-assignment2

입력데이터 수에따른 정렬 알고리즘의 성능 분석 및 비교를 해보겠습니다
버블정렬 , 삽입정렬, 선택정렬 , 쉘정렬 , 힙정렬 , 퀵정렬 , 우선순위 큐 순서로 시간복잡도를 비교하는데 이때 세가지 경우를 사용해서 비교해 보겠습니다

1. 랜덤 데이터
2. 정렬된 데이터
3. 역 정렬 데이터 순서로 비교해보겠습니다.

버블정렬부터 시작하겠습니다

```
#include <iostream>
#include <stdlib.h>
#include <algorithm>

using namespace std;

int main(void)
{
	cout << "거듭 제곱할 n을 입력하세요: ";
	int n;
	cin >> n;
	int m = 2 << n-1; 
	
	int array[m];
	
	for(int i=0; i<m; i++) {
		array[i] = rand() % 100;
	}

	for(int i=0; i<m; i++) {
		cout << array[i] << ' ';
	}
	
	return 0;
}
```

기본 베이스는 다음과 같이 설정하겠습니다.

```
	for(int i=0; i<m; i++) {
		for(int j=0; j<m-1-i; j++) {
			if(array[j] > array[j+1]) {
				swap(array[j],array[j+1]);
			}
		}
	} 
  ```
  버블정렬 코드는 위와같습니다.
  
  위의 코드를 따르는 버블정렬은 어떤 상황이 되었든 O(n^2)의 시간복잡도를 가집니다.
  왜냐하면 이미 정렬이 되어 있든 안되어 있든 n개의 input이 있을 때 n개의 대해서 각각 n번, 즉 for문을 2번 돌기 때문에 (교환이 일어나지 않더라도 비교는 계속 진행하기 때문에) 최상, 최악, 평균의 경우 모두 O(n^2)의 시간복잡도를 갖게 됩니다.
  
  ![image](https://user-images.githubusercontent.com/97587573/166616035-7d30d8ea-9cad-47c7-ae10-75a956e22d88.png)
  
  따라서 그래프로 표현하게되면 위의 그림과 같습니다.


다음으로는 삽입정렬에 대해 알아보겠습니다.
템플릿은 기존과 같고 코드는 아래와 같습니다.

```
    int key;
   
    for(int i=1; i < m; i++)
	{
        int j;
        key = array[i];
        
        for(j=i-1; j>=0; j--)
        {
            if(array[j] > key)
                array[j+1] = array[j];
            
			else
            	break;
        }
        
        array[j+1] = key;
    }
```

