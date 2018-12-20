# Merge Sort



> [Problem Solving with Algorithms and Data Structures using Python](http://interactivepython.org/runestone/static/pythonds/SortSearch/TheBubbleSort.html) 을 공부하고 정리한 내용입니다.



sorting 알고리즘의 성능을 높이기 위해 ``Dived and Conquer`` 전략을 사용할 수 있습니다.

Merge Sort 는 리스트를 재귀적으로 두개로 나누어 정렬 후 정렬된 부분들을 다시 합치는 방식입니다.<br>base case는 merge 를 수행하는 부분이 되겠고, 그 조건은 리스트의 값이 하나 이하일 경우 입니다.

그림으로 예를 들어보겠습니다.

![](http://interactivepython.org/runestone/static/pythonds/_images/mergesortA.png)

위의 그림은 Merge Sort 를 위해 리스트를 분할하는 과정을 나타냅니다.<br>리스트의 원소가 하나 이하가 될 때 까지 계속 분할하는 것을 볼 수 있습니다.

이렇게 분할된 리스트는

![](http://interactivepython.org/runestone/static/pythonds/_images/mergesortB.png)

위와같이 다시 합치는 과정을 거치게 되는데, 이 때 합치는 과정은 정렬된 부분에서 가장 작은 값을 원래 리스트의 맨 앞으로 가져오는 형식으로 진행됩니다.



### 구현하기



- 길이가 1 이하가 될 때 까지 입력리스트를 반으로 나누기 (Python 의 slicing 방법을 사용)
- 반으로 나눈 리스트에 대해 각각 mergerSort 하기
- 입력리스트, 왼쪽리스트, 오른쪽리스트 의 값을 비교하며 가장 작은 값부터 앞쪽에 넣어주기



```python
def mergeSort(alist):
    print('Splitting {}'.format(alist))
    if len(alist) > 1:
        mid = len(alist) // 2
        leftList = alist[:mid]
        rightList = alist[mid:]
        
        mergeSort(leftList)
        mergeSort(rightList)
        
        leftIdx = 0
        rightIdx = 0
        originIdx = 0
        
        while leftIdx < len(leftList) and rightIdx < len(rightList):
            if leftList[leftIdx] < rightList[rightIdx]:
                alist[originIdx] = leftList[leftIdx]
                leftIdx = leftIdx + 1
            else:
                alist[originIdx] = rightList[rightIdx]
                rightIdx = rightIdx + 1
            originIdx = originIdx + 1
            
        while leftIdx < len(leftList):
            alist[originIdx] = leftList[leftIdx]
            leftIdx = leftIdx + 1
            originIdx = originIdx + 1
            
        while rightIdx < len(rightList):
            alist[originIdx] = rightList[rightIdx]
            rightIdx = rightIdx + 1
            originIdx = originIdx + 1
            
        print('Mergeing {}'.format(alist))
```

실행결과를 보면, 재귀적으로 구현되었기 때문에 왼쪽리스트부터 base case 까지 나눈 후 merge 하고<br>그 다음에 오른쪽리스트를 merge 하고 있는것을 확인할 수 있습니다.

```
mergeSort([54,26,93,17,77,31,44,55,20])
```

```
Splitting [54, 26, 93, 17, 77, 31, 44, 55, 20]
Splitting [54, 26, 93, 17]
Splitting [54, 26]
Splitting [54]
Splitting [26]
Mergeing [26, 54]
Splitting [93, 17]
Splitting [93]
Splitting [17]
Mergeing [17, 93]
Mergeing [17, 26, 54, 93]
Splitting [77, 31, 44, 55, 20]
Splitting [77, 31]
Splitting [77]
Splitting [31]
Mergeing [31, 77]
Splitting [44, 55, 20]
Splitting [44]
Splitting [55, 20]
Splitting [55]
Splitting [20]
Mergeing [20, 55]
Mergeing [20, 44, 55]
Mergeing [20, 31, 44, 55, 77]
Mergeing [17, 20, 26, 31, 44, 54, 55, 77, 93]
```



### 분석하기

Merge Sort 는 크게 두 가지의 과정으로 나뉘므로, 각 과정에 대해 생각해 볼 필요가 있습니다.

먼저 Split 의 과정을 생각해보면 Binary Search 와 같은 과정임을 알 수 있습니다.<br>n개의 값을 반으로 나눠가는 과정을 생각해 보면,<br>첫번째 과정을 거쳤을 때에는 $\frac {n}{2}$, 두번째는 $\frac {n}{4}$ , ... , $k$ 번째에는 $\frac {n}{2^k}$ 개의 값이 남는 것을 알 수 있습니다.<br>우리는 $\frac {n}{2^k}$ (맨 마지막으로 남은 원소의 수) 가 1인 경우까지 split 하는 것이기 때문에,  $\frac {n}{2^k} = 1$ 인 k 를 계산하면 됩니다. <br>$k=\log n$ 이므로 반으로 나눠가는 과정의 실행 시간은 $O(\log n)$ 만큼이 걸리는 것을 알 수 있습니다.

다음으로 Merge 의 과정은 각 값당 최대 n 번의 비교를 거쳐야 하기 때문에 $n$ 만큼의 시간이 걸립니다.

그래서 Merge Sort 알고리즘은 $O(n\log n)$ 알고리즘 입니다.



#### 추가중

그런데 slicing 방법의 실행시간은 무시해도 되는걸까 하는 의문이 생겼습니다. <br>검색결과 slicing 은 $k$ 개의 slicing 을 할 때에 $O(k)$  만큼의 실행시간이 필요하다고 합니다.

Merge Sort 의 이론적인 분석시간인 $O(n\log n)$ 을 맞춰주기 위해서는 이 slicing 방법을 제거해야 합니다.



