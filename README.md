# cpu-algorithm-assignment2

입력데이터 수에따른 정렬 알고리즘의 성능 분석 및 비교를 해보겠습니다
버블정렬 , 삽입정렬, 선택정렬 , 쉘정렬 , 힙정렬 , 퀵정렬 , 우선순위 큐 순서로 시간복잡도를 비교하는데 이때 세가지 경우를 사용해서 비교해 보겠습니다

1. 랜덤 데이터
2. 정렬된 데이터
3. 역 정렬 데이터 순서로 비교해보겠습니다.

## 버블정렬부터 시작하겠습니다

```
int array[16] = {32,64,128,256,512,1024,2048,4096,8192,16384,32768,65536,131072,262144,524288,1048576};
int main(void)
{
	int m = 16;
	
	for(int i=0; i<m; i++) {
		for(int j=0; j<m-1-i; j++) {
			if(array[j] > array[j+1]) {
				swap(array[j],array[j+1]);
			}
		}
	}    
	


	for(int i=0; i<m; i++) {
		cout << array[i] << ' ';
	}
	
	return 0;
}
```

  버블정렬 코드는 위와같습니다.
  
  위의 코드를 따르는 버블정렬은 어떤 상황이 되었든 O(n^2)의 시간복잡도를 가집니다.
  왜냐하면 이미 정렬이 되어 있든 안되어 있든 n개의 input이 있을 때 n개의 대해서 각각 n번, 즉 for문을 2번 돌기 때문에 (교환이 일어나지 않더라도 비교는 계속 진행하기 때문에) 최상, 최악, 평균의 경우 모두 O(n^2)의 시간복잡도를 갖게 됩니다.
  
  ![image](https://user-images.githubusercontent.com/97587573/166616035-7d30d8ea-9cad-47c7-ae10-75a956e22d88.png)
  
  따라서 그래프로 표현하게되면 위의 그림과 같습니다.


## 다음으로는 삽입정렬에 대해 알아보겠습니다.

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

삽입 정렬은 두번째 자료부터 시작하여 그 앞의 자료들과 비교하여 삽입할 위치를 지정한 후 자료를 뒤로 옮기고 지정한 자리에 자료를 삽입하여 정렬하는 알고리즘입니다.

그렇기 때문에 삽입정렬의 경우 이미 정렬이 된 상황에서는 O(N)의 시간복잡도를 가집니다. 그러나 입력자료가 역순이거나 평균적일경우 중첩 for문에 의해 시간복잡도가 O(N^2) 이 됩니다.

이를 그래프로 표현하면

![image](https://user-images.githubusercontent.com/97587573/166873925-fd6d4557-6ac1-441f-98e0-403ac6e23e14.png)

빨간선이 이미 정렬이 된 경우, 파란선이 역순,평균적일 경우입니다.


다음으로는 선택정렬을 살펴보겠습니다.

```
	int temp;
	int key;
	    
    for(int i=0; i < m; i++)
	{
        int minn = array[i];
        int location = i;
        for(int j=i+1; j<8; j++)
        {
            if(minn > array[j]) {
            	minn = array[j];
				location = j;	
			}
            swap(array[i],array[location]);
    	}
    }
```

선택정렬의 과정을 순서대로 살펴보겠습니다

1.주어진 배열 중에서 최솟값을 찾는다.
2.그 값을 맨 앞에 위치한 값과 교체한다
3.맨 처음 위치를 뺀 나머지 리스트를 같은 방법으로 교체한다.
4.하나의 원소만 남을 때까지 위의 1~3과정을 반복한다.

위와같은 순서를 따르기 때문에 선택정렬의 시간복잡도는 두개의 for루프를 항상 실행시킬수 밖에 없습니다.
그러므로 시간복잡도는 어떤 경우던 O(n^2)입니다. 이러한 이유 때문에 선택정렬은 구현이 간단하지만 비효율적인 정렬 알고리즘입니다.

이를 그래프로 표현하면

![image](https://user-images.githubusercontent.com/97587573/166874571-956b68d1-f3ae-4302-8385-b0baa0a61fdc.png)

아래의 그림과 같습니다.


## 다음으로는 쉘정렬에 대해 알아보겠습니다.

```
void ShellSort(int _arrays[], int _arraysLength)
{
	for (int h = _arraysLength / 2; h > 0; h /= 2)	//간격은 배열 길이의 절반 
	{
		for (int i = 0; i < h; i++)
		{
			for (int j = i + h; j < _arraysLength; j += h)
			{
				int num = _arrays[j];
				int index = j;
				while (index > h - 1 && _arrays[index - h] > num)
				{
					_arrays[index] = _arrays[index - h];
					index -= h;
				}
				_arrays[index] = num;
			}
		}
	}
}
```
코드는 위와같습니다.

쉘정렬의 과정을 순서화 하면

1. 먼저 정렬해야 할 리스트를 일정한 기준에 따라 분류
2. 연속적이지 않은 여러 개의 부분 리스트를 생성
3. 각 부분 리스트를 삽입 정렬을 이용하여 정렬
4. 모든 부분 리스트가 정렬되면 다시 전체 리스트를 더 적은 개수의 부분 리스트로 만든 후에 알고리즘을 반복
5. 위의 과정을 부분 리스트의 개수가 1이 될 때까지 반복

```
for (int h = _arraysLength / 2; h > 0; h /= 2)
```

위의 코드를보면 연속적이지 않은 부분 리스트에서 자료의 교환이 일어나면 더 큰 거리를 이동하기때문에 겨ㅛ환요소들이 삽입정렬보다는 최종 위치에 있을 가능성이 높아집니다.

그래서 평균적으로는 O(N^1.5)의 시간복잡도를 가지며 최선의 케이스는 O(N) 최악의 경우 O(N^2)의 시간복잡도를 가집니다.

이를 그래프로 표현하면

![image](https://user-images.githubusercontent.com/97587573/166875933-6d3dcddf-5fbe-4c74-8f85-a62486fbb055.png)

보라색선이 평균, 파란색선은 최악의경우, 초록색선은 최선의 경우입니다.

## 다음은 퀵정렬에 대해서 알아보겠습니다

```
void quick_sort(int *data,int start,int end) 
{
	if(start>=end) 
		return;
		
	int pivot = start;
	int i = pivot + 1;
	int j = end;
	int temp;
	
	while(i <= j) {
		while(i <= end && data[i] <= data[pivot]) 
			i++;
		while(j > start && data[j] >= data[pivot]) 
			j--;
		if(i > j) 
			swap(data[j],data[pivot]);
		else
			swap(data[i],data[j]);
	}
	quick_sort(data,start,j-1);
	quick_sort(data,j+1,end);
	
}
```

코드는 위와같습니다.

퀵정렬이 다른정렬과 확연하게 다른점으로는 pivot을 사용해 재귀함수를 호출한다는 점이 있습니다. 그래서 퀵정렬은 불안정 정렬에 속하며 다른 원소와의 비교만으로 정렬을 수행하는 비교정렬에 속합니다.

퀵정렬의 순서를 살펴보면

1. 리스트 안에 있는 한 요소를 pivot으로 선택한다
2. pivot을 기준으로 피봇보다 작은 요소들은 모두 피봇의 왼쪽으로 옮기고 피봇보다 큰 요소들은 모두 피봇의 오른쪽으로 옮깁니다.
3. 피봇을 제외한 왼쪽 리스트와 오른쪽 리스트를 다시 정렬합니다.
4. 부분 리스트들이 더이상 분할이 불가능할때까지 반복합니다

이러한 순환호출의 사용으로 퀵 정렬은 다른 정렬 알고리즘에 비해 속도가 매우 빠르다는 장점이 있습니다.

그래서 퀵정렬의 경우 최선의 상태일때 O(Nlog2N) 평균일때 O(Nlog2N) 최악의 경우 O(N^2)의 시간복잡도를 가집니다.

이를 그래프로 표현하면 아래 그림과 같습니다.

![image](https://user-images.githubusercontent.com/97587573/166880112-9e07e638-2327-4b4d-baf0-8a41513641d9.png)

nlog2n 그래프는 위와같습니다.

## 마지막으로 힙정렬에 대해서 알아보겠습니다.

힙정렬은 정렬 알고리즘 중 시간복잡도 성능에서 가장 우월한 알고리즘입니다. 어떤 경우에서든 nlog2n의 시간복잡도를 가집니다. 

시간복잡도 성능이 우월한 만큼 구현이 쉽지 않습니다. 힙정렬을 알기 위해서는 힙에 대해서 알아야 하는데 힙이란 완전 이진 트리의 일종으로 우선순위 큐를 위하여 만들어진 자료구조입니다.

코드는 아래와 같습니다.
```
void heap(int *data,int num) {
	for(int i=1; i<num; i++) {
		int child = i;
		while(child > 0) {
			int root = parent(child);
			if(data[root] < data[child]) {
				swap(data[root],data[child]);
			}
			child = root;
		}
	}
}
```

```
for(int i=m-1; i>=0; i--) {
	swap(array[i],array[0]);
	heap(array,i);
}
```
heap 함수를 통해 힙을 생성해준뒤 반복문을 이용해서 정렬을 완료해줍니다.

또한 위 코드를 보면 힙 정렬은 원래 있던 배열을 그대로 사용하기 때문에 메모리 측면에서도 효율적입니다.

시간복잡도는 항상 O(Nlog2N)이므로 그래프로 나타낸다면 아래 보라색 선 그래프와 같습니다.

![image](https://user-images.githubusercontent.com/97587573/166881580-5593a1b1-9f74-471d-8f5b-1b64967163fd.png)


