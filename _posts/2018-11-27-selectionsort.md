---
layout: post
title: '[Sorting #2] Selection Sort'
author: hyunkyung
date: 2018-11-27 13:40
tags: [algorithm, sorting]
image: 
---

# Selection Sort



> [Problem Solving with Algorithms and Data Structures using Python](http://interactivepython.org/runestone/static/pythonds/index.html) 을 공부하고 정리한 내용입니다.



selection sort 는 리스트 중 가장 큰 값을 골라 뒤에서 부터 채우는 형식입니다.<br>bubble sort 와는 달리 selection sort 는 **리스트를 처음부터 끝까지 볼 때 한 번 의 swap 연산**만 합니다.

bubble sort 는 한 값을 정렬하기 위해 $n-1$ 번의 검사와 최대 $n-1$ 번의 swap 연산을 했다면,<br>selection sort 는 $n-1$ 번의 검사와 한번의 swap 연산을 합니다. 

그림으로 예시를 보겠습니다.

![](http://interactivepython.org/runestone/static/pythonds/_images/selectionsortnew.png)



정렬되지 않은 리스트 중 가장 큰 값을 찾아 뒤에서 부터 차례로 채우는 방식임을 알 수 있습니다.



### 구현하기

- unsorted list 까지의 최댓값 찾기
- 최댓값과 unsorted list 의  마지막값을 swap 하기

```python
def selectionSort(alist):
    tempIdx = len(alist)-1
    
    while tempIdx > 0:
        maxIdx = 0
        maxValue = alist[0]
        
        for i in range(tempIdx+1):
            if alist[i] > maxValue:
                maxIdx = i
                maxValue = alist[i]
        print('max value : {}, temp value : {}'.format(alist[maxIdx],alist[tempIdx]))
        print('before swap : {}'.format(alist))
        temp = alist[tempIdx] 
        alist[tempIdx] = alist[maxIdx]
        alist[maxIdx] = temp
        
        tempIdx = tempIdx - 1
        print('after swap : {}\n'.format(alist))
    
    return alist
```

최댓값과 그 위치만 구할 수 있다면 간단하게 해결할 수 있는 구현입니다.



```python
selectionSort([54,26,93,17,77,31,44,55,20])
```

코드의 실행 결과는 아래와 같습니다.

```
max value : 93, temp value : 20
before swap : [54, 26, 93, 17, 77, 31, 44, 55, 20]
after swap : [54, 26, 20, 17, 77, 31, 44, 55, 93]

max value : 77, temp value : 55
before swap : [54, 26, 20, 17, 77, 31, 44, 55, 93]
after swap : [54, 26, 20, 17, 55, 31, 44, 77, 93]

max value : 55, temp value : 44
before swap : [54, 26, 20, 17, 55, 31, 44, 77, 93]
after swap : [54, 26, 20, 17, 44, 31, 55, 77, 93]

max value : 54, temp value : 31
before swap : [54, 26, 20, 17, 44, 31, 55, 77, 93]
after swap : [31, 26, 20, 17, 44, 54, 55, 77, 93]

max value : 44, temp value : 44
before swap : [31, 26, 20, 17, 44, 54, 55, 77, 93]
after swap : [31, 26, 20, 17, 44, 54, 55, 77, 93]

max value : 31, temp value : 17
before swap : [31, 26, 20, 17, 44, 54, 55, 77, 93]
after swap : [17, 26, 20, 31, 44, 54, 55, 77, 93]

max value : 26, temp value : 20
before swap : [17, 26, 20, 31, 44, 54, 55, 77, 93]
after swap : [17, 20, 26, 31, 44, 54, 55, 77, 93]

max value : 20, temp value : 20
before swap : [17, 20, 26, 31, 44, 54, 55, 77, 93]
after swap : [17, 20, 26, 31, 44, 54, 55, 77, 93]
```



### 분석하기

최댓값을 알기 위해 unsorted list 를 모두 검사해야 하므로 최대 $n-1$ 번의 검사를 합니다.<br>worst 케이스의 경우 bubble sort 와 마찬가지로 최대 $\frac{1}{2}n^{2} + \frac{1}{2}n - n$  번의 연산을 합니다.

따라서 $O(n^{2})$ 만큼 시간이 걸립니다.

하지만 swap 연산을 bubble sort 보다 훨씬 적게 하기 때문에, 성능면에서는 bubble sort 보다 나은 알고리즘 이라고 볼 수 있습니다.