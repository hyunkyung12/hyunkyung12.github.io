# Quick Sort



> [Problem Solving with Algorithms and Data Structures using Python](http://interactivepython.org/runestone/static/pythonds/SortSearch/TheBubbleSort.html) 을 공부하고 정리한 내용입니다.



quick sort 는 추가적인 메모리 사용 없이 ``Divide and Conquer`` 를 사용할 수 있습니다.<br>**pivot** 을 이용해 divide 될 지점을 찾아주는 것이 핵심입니다.

그림으로 예를 들어 설명해보겠습니다.

![](http://interactivepython.org/runestone/static/pythonds/_images/firstsplit.png)

이 리스트에서 첫번째 pivot 을 54 라고 합시다.<br>( pivot 을 정하는 방법은 여러가지가 있지만, 예제에서는 첫번째 원소를 pivot 으로 하는 방법을 선택하고 있습니다.)



이제 이 pivot 을 기준으로 하고, 나머지 리스트 에 대해 **leftmartk** 와 **rightmark** 를 정합니다.<br>leftmark 와 rightmark 는 서로의 방향으로 한 칸씩 이동을 하는데<br>leftmark > pivot 이고 rightmark < pivot 인 시점에서 두 값을 swap 합니다.

이런 과정을 거치면서 leftmark 와 rightmark 를 이동하다 보면 둘이 교차하는 지점을 찾을 수 있는데,<br>그 지점에서 rightmark 의 값과 pivot 의 값을 swap 해 줍니다.<br>( 아래 그림에서는 31과 54 를 swap 해 주어야 합니다. )

![](http://interactivepython.org/runestone/static/pythonds/_images/partitionA.png)



이렇게 한 pivot 에 대해 완료하고 나면 리스트를 분할할 수 있습니다.

![](http://interactivepython.org/runestone/static/pythonds/_images/partitionB.png)

이미 정렬이 된 pivot 인 54 를 기준으로 앞과 뒤의 원소들로 리스트를 분할합니다.<br>각 리스트에 대해 다시 quicksort 를 실행해 주면 정렬이 완료됩니다.



### 구현하기

- pivot , leftmark, rightmark 를 정해 한번의 quick sort 과정을 거치기
- 분할된 리스트에 대해 quicksort 하기

~~quicksort 를 재귀로 계속 하면 좋을것 같은데, base case 가 무엇일지 잘 생각이 안나 일단은 구현한 대로 포스팅 합니다..ㅠ~~



```python
def quickSort(alist):
    quickSortHelper(alist,0,len(alist)-1)

def quickSortHelper(alist,first,last):
    if first < last:

        splitpoint = partition(alist,first,last)

        quickSortHelper(alist,first,splitpoint-1)
        quickSortHelper(alist,splitpoint+1,last)


def partition(alist,first,last):
    pivotvalue = alist[first]

    leftmark = first+1
    rightmark = last

    done = False
    while not done:

        while leftmark <= rightmark and alist[leftmark] <= pivotvalue:
            leftmark = leftmark + 1

        while alist[rightmark] >= pivotvalue and rightmark >= leftmark:
            rightmark = rightmark -1

        if rightmark < leftmark:
            done = True
        
        else:
            temp = alist[leftmark]
            alist[leftmark] = alist[rightmark]
            alist[rightmark] = temp

    temp = alist[first]
    alist[first] = alist[rightmark]
    alist[rightmark] = temp

    return rightmark
```



### 분석하기

길이가 $n$ 인 리스트에 대해, 파티션이 항상 리스트의 중간에 있다면 binary search 와 마찬가지로 $\log n$ 만큼의 시간이 소요됩니다.<br>또한 분할지점을 찾기 위해 pivot 과 값을 비교하는 과정은 최대 $n$ 만큼 소요되므로 Quick Sort 는 $O(n\log n)$  인 알고리즘 입니다.   $n-1$ 개의 값을 피벗과 비교해야 합니다.<br>

Merge Sort 와 실행시간은 비슷하지만, 추가적인 메모리를 필요로 하지 않는다는 장점이 있습니다.

하지만 파티션이 왼쪽이나 오른쪽으로 치우쳐 있는 경우에는 문제가 될 수 있습니다. <br>예를 들어 파티션이 계속해서 맨 앞쪽에 나온다고 하면 의미없는 분할을 계속 해야 하기 때문입니다.<br>이 경우 길이가 $n$ 인 리스트를 처음 분할 했을 때  $0$ , $n-1$  인 리스트로 분할이 되고 그 다음에는 $0$, $n-2$  인 리스트 로 분할이 됩니다.<br>즉 worst case 의 실행시간은 $O(n^{2})$ 이 되어버립니다.

위와 같은 worst case 는 항상 pivot 을 맨 앞의 원소로 할 때에 발생합니다.<br>이를 해결하기 위해 pivot 을 결정하는 많은 방법들이 존재합니다.<br>그 중 ``median of three`` 라는 방법이 있는데, 리스트의 처음,중간,끝 값 중 중위수를 pivot 으로 사용하는 방법입니다.<br>리스트가 어느 정도 정렬되어 있을 때 ``median of three`` 를 사용하면 리스트가 거의 반으로 분할 되기 때문에 유용합니다.