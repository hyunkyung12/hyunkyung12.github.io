---
layout: post
title: '[Sorting #3] Insertion Sort'
author: hyunkyung
date: 2018-11-27 15:00
tags: [algorithm, sorting]
image: 
---

# Insertion Sort



> [Problem Solving with Algorithms and Data Structures using Python](http://interactivepython.org/runestone/static/pythonds/index.html) 을 공부하고 정리한 내용입니다.



insertion sort 는 한 리스트 내에서 **두 부분으로 리스트**를 나누고, 뒤의 리스트의 값들을 하나씩 앞의 리스트에 정렬해서 넣는 방식입니다.<br>

결론부터 말하자면 ``bubble sort`` 와 마찬가지로 성능이 좋지 않은 방법입니다.



그림으로 예를 들어보겠습니다.

![](http://interactivepython.org/runestone/static/pythonds/_images/insertionsort.png)

편의상 앞의 list 를 ``sorted sublist`` , 뒤의 list 를 ``unsorted sublist`` 라고 이름붙여 보겠습니다.

``unsorted sublist`` 에 있는 값들을 차례로 ``sorted sublist`` 로 가져오는데, 이 때 가져오는 값과 sorted sublist 의 값을 하나씩 비교하며 삽입합니다.



위의 그림에서 31 을 sorted sublist 에 삽입하는 과정을 자세히 보겠습니다.

![](http://interactivepython.org/runestone/static/pythonds/_images/insertionpass.png)

31 과 sorted sublist 의 값을 비교하며 31 보다 작은 값이 나올 때 까지 계속 shift 하는 것을 볼 수 있습니다.<br>early stop rule 이 없다고 가정했을 때 한 원소를 삽입하기 위해 검사해야 하는 횟수는 최대 $n-1$ 번 입니다.



### 구현하기

- 현재 값, 현재 인덱스 저장
- ``sorted sublist`` 와 비교 후 shift

```python
def insertionSort(alist):
    for temp in range(len(alist)):
        tempValue = alist[temp]
        tempIdx = temp
        print('\n tempValue : {}'.format(tempValue))
        while tempIdx > 0 and alist[tempIdx-1] > tempValue:
            print('shift {} -> {}'.format(alist[tempIdx-1], alist[tempIdx]))
            alist[tempIdx] = alist[tempIdx-1]
            tempIdx = tempIdx-1
            
        alist[tempIdx] = tempValue
    return alist

```

shift 하면서 찾은 인덱스에 현재 값을 넣어주면 됩니다.



위의 코드를 실행한 결과는 다음과 같습니다.

```python
insertionSort([54, 26, 93, 17, 77, 31, 44, 55, 20])
```

```

tempValue : 54

tempValue : 26
shift 54 -> 26

tempValue : 93

tempValue : 17
shift 93 -> 17
shift 54 -> 93
shift 26 -> 54

tempValue : 77
shift 93 -> 77

tempValue : 31
shift 93 -> 31
shift 77 -> 93
shift 54 -> 77

tempValue : 44
shift 93 -> 44
shift 77 -> 93
shift 54 -> 77

tempValue : 55
shift 93 -> 55
shift 77 -> 93

tempValue : 20
shift 93 -> 20
shift 77 -> 93
shift 55 -> 77
shift 54 -> 55
shift 44 -> 54
shift 31 -> 44
shift 26 -> 31
```



### 분석하기

worst case 의 경우는 완전 역순으로 정렬된 리스트가 들어오는 경우입니다.<br>이 경우 최대 $n-1$ 번의 비교를 하게 되고, bubble sort 와 마찬가지로 $\frac{1}{2}n^{2} + \frac{1}{2}n - n$ 의 연산을 하게 됩니다.<br>

따라서 $O(n^{2})$ 만큼 시간이 걸린다고 볼 수 있습니다.

bubble sort 와 차이점은 swap 이 아닌 shift 연산을 한다는 것입니다. <br>shift 연산은 swap 보다 가벼운 연산이기 때문에 bubble sort 보다는 좀 더 나은 알고리즘으로 볼 수 있습니다.

