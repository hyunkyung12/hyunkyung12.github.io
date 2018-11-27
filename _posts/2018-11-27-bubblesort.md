---
layout: post
title: '[Sorting #1] Bubble Sort'
author: hyunkyung
date: 2018-11-27 11:30
tags: [algorithm]
image: 
---


# Bubble Sort


> [Problem Solving with Algorithms and Data Structures using Python](http://interactivepython.org/runestone/static/pythonds/SortSearch/TheBubbleSort.html) 을 공부하고 정리한 내용입니다.



bubble sort 는 **인접한 값을 비교하고 순서를 정렬**하는 방법입니다.<br>sorting 방법 중 가장 많이 알고있는 방법인데, 성능면에서는 우수하지 않습니다.<br>compare 와 swapping 을 불필요하게 여러 번 하기 때문입니다.



그림으로 예를 들어보겠습니다.

![](http://interactivepython.org/runestone/static/pythonds/_images/bubblepass.png)

[54, 26, 93, 17, 77, 31, 44, 55, 20] 이라는 bubble sort 방식으로 정렬할 때,  처음 비교해야 하는 쌍은 총 8 ( 9-1 ) 개 입니다. <br>8쌍을 비교하고 나면, 리스트 내에서 가장 큰 수는 올바른 자리에 정렬이 되게 됩니다.<br>그렇다면 두번째로 비교해야 하는 쌍은 가장 큰 수를 제외한 7 ( 8 -1 ) 쌍이 되는 것을 알 수 있습니다.<br>이런식으로 9개의 값에 대해 쌍을 비교하면 총 36 번의 비교를 하게 됩니다.

조금 더 일반적으로 표현하면 n 개의 값에 대해 $\frac{1}{2}n^{2} + \frac{1}{2}n - n$ 번의 비교를 한다고 할 수 있습니다.<br>문제는 단순히 비교를 하는것 뿐 아니라 swapping 을 하는 경우도 있다는 것입니다. <br>swapping 은 그 값의 최종 위치를 알기 전 까지 계속 되어야 하기 때문에 매우 비효율적 입니다.



### 구현하기

- 앞에서 부터 pair 를 만들어가며 비교
- swapping 

```python
def bubbleSort(alist):
    for pairNum in range(len(alist)-1,0,-1):
        print(pairNum)
        for i in range(pairNum):
            if alist[i] > alist[i+1]:
                print('swapping {} and {}'.format(alist[i-1], alist[i]))
                temp = alist[i]
                alist[i] = alist[i+1]
                alist[i+1] = temp
        print(alist)
    return alist
```

참고로 python 에서 for loop 를 역순으로 돌고 싶을 때에는 ``range(start, stop, step)`` 의 형태로 사용하면 됩니다.



```python
bubbleSort([54, 26, 93, 17, 77, 31, 44, 55, 20])
```

위의 코드를 실행한 결과는 다음과 같습니다.

```
8
swapping 20 and 54
swapping 54 and 93
swapping 17 and 93
swapping 77 and 93
swapping 31 and 93
swapping 44 and 93
swapping 55 and 93
[26, 54, 17, 77, 31, 44, 55, 20, 93]
7
swapping 26 and 54
swapping 54 and 77
swapping 31 and 77
swapping 44 and 77
swapping 55 and 77
[26, 17, 54, 31, 44, 55, 20, 77, 93]
6
swapping 93 and 26
swapping 26 and 54
swapping 31 and 54
swapping 54 and 55
[17, 26, 31, 44, 54, 20, 55, 77, 93]
5
swapping 44 and 54
[17, 26, 31, 44, 20, 54, 55, 77, 93]
4
swapping 31 and 44
[17, 26, 31, 20, 44, 54, 55, 77, 93]
3
swapping 26 and 31
[17, 26, 20, 31, 44, 54, 55, 77, 93]
2
swapping 17 and 26
[17, 20, 26, 31, 44, 54, 55, 77, 93]
1
[17, 20, 26, 31, 44, 54, 55, 77, 93]
```

이론 내용과 동일하게 잘 작동한 것을 확인할 수 있습니다.



### 분석하기

앞에서 말했던 것 처럼 n 개의 원소에 대해서 (n-1) + (n-2) + ... + 1 만큼의 비교가 필요합니다.<br>n 까지의 sum 이 $\frac{1}{2}n^{2} + \frac{1}{2}n$ 인 것을 고려하면 n-1 까지의 sum 은 $\frac{1}{2}n^{2} + \frac{1}{2}n - n$ 이 됩니다.

이는 결국 $O(n^{2})$ 만큼의 시간이 걸린다는 것이므로 bubble sort 는 성능이 좋지 않은 알고리즘인것을 알 수 있습니다.



#### 참고

bubble sort 의 성능을 높이기 위해서 정렬이 필요하지 않은 경우 일찍 멈추는 방법을 사용할 수 있습니다.

```python
def betterBubbleSort(alist):
    exchange = True
    pairNum = len(alist) - 1
    while pairNum > 0 and exchange:
        print(pairNum)
        exchange = False
        for i in range(pairNum):
            if alist[i] > alist[i+1]:
                print('swapping {} and {}'.format(alist[i-1], alist[i]))
                temp = alist[i]
                alist[i] = alist[i+1]
                alist[i+1] = temp
                exchange = True
        pairNum = pairNum-1
        print(alist)
     return alist
```

exchange 라는 flag 변수를 사용해 리스트의 정렬 여부를 나타냈습니다. <br>pairNum 을 모두 돌았을 때 exchange == False 라면 더이상 검사하지 않고 함수를 종료합니다.

위의 예제에서 본 ``[54, 26, 93, 17, 77, 31, 44, 55, 20]``  같은 리스트에서는 별 차이가 없지만, <br>

``[20,30,40,90,50,60,70,80,100,110]`` 와 같이 일부만 정렬이 필요한 경우에는 차이를 보입니다.



```python
bubbleSort([20,30,40,90,50,60,70,80,100,110])
```

```
9
swapping 40 and 90
swapping 50 and 90
swapping 60 and 90
swapping 70 and 90
[20, 30, 40, 50, 60, 70, 80, 90, 100, 110]
8
[20, 30, 40, 50, 60, 70, 80, 90, 100, 110]
7
[20, 30, 40, 50, 60, 70, 80, 90, 100, 110]
6
[20, 30, 40, 50, 60, 70, 80, 90, 100, 110]
5
[20, 30, 40, 50, 60, 70, 80, 90, 100, 110]
4
[20, 30, 40, 50, 60, 70, 80, 90, 100, 110]
3
[20, 30, 40, 50, 60, 70, 80, 90, 100, 110]
2
[20, 30, 40, 50, 60, 70, 80, 90, 100, 110]
1
[20, 30, 40, 50, 60, 70, 80, 90, 100, 110]
```



```python
betterBubbleSort([20,30,40,90,50,60,70,80,100,110])
```

```
9
swapping 40 and 90
swapping 50 and 90
swapping 60 and 90
swapping 70 and 90
[20, 30, 40, 50, 60, 70, 80, 90, 100, 110]
8
[20, 30, 40, 50, 60, 70, 80, 90, 100, 110]
```

