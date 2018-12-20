# Shell Sort



> [Problem Solving with Algorithms and Data Structures using Python](http://interactivepython.org/runestone/static/pythonds/SortSearch/TheBubbleSort.html) 을 공부하고 정리한 내용입니다.



shell sort 는 조금 생소할 수도 있는 소팅 방법입니다.<br>insertion sort 방식을 조금 개선한 방법인데, insertion sort 가 두 연속된 sublist 로 나누어 정렬하는 방식이었다면<br>shell sort 는 **일정 gap 을 가진 sublist 로 나누어 정렬**하는 방식입니다.



그림을 보면서 설명하겠습니다.

![](http://interactivepython.org/runestone/static/pythonds/_images/shellsortA.png)

이 그림은 gap 이 2 인 sublist 로 나누어 정렬한 예시입니다.<br>그림처럼 일정한 gap 만큼 떨어진 값들을 하나의 sublist 로 묶어서 생각해 정렬하면 됩니다.

각 sublist 를 정렬한 후 의 그림은 다음과 같습니다.

![](http://interactivepython.org/runestone/static/pythonds/_images/shellsortB.png)

각 sublist 를 정렬했지만 전체 리스트에서 보았을 때 정렬이 완료되지 않은것을 알 수 있습니다.<br>이때는 2를 반으로 나눈 1을 gap 으로 가지는 sublist 를 만들어 다시 정렬합니다. 



### 구현하기

