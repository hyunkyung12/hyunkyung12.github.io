---
layout: post
title: '알고리즘 스터디 2주차'
author: hyunkyung
date: 2018-11-5 16:30
tags: [algorithm]
image: 
---

# 알고리즘 스터디 2주차

 [백준](https://www.acmicpc.net)의 단계별 풀어보기에서 **규칙찾기**를 풀어보았습니다.



#### 1193 분수찾기

```python
def problem_1193(input_num):
    
    # 대각선 기준으로 몇 번째 대각선에 속하는 수 인지 알기
    c = 0
    while input_num > 0:
        c = c+1
        input_num = input_num - c
    
    # c = 몇 번째 대각선에 속하는지, c1 = 해당 대각선의 분수를 만들기 위해
    c1 = c
	# l = 해당 대각선의 분수 list
    l = []
 	   
    # 지그재그 이므로, 홀수번째 대각선과 짝수번째 대각선의 규칙이 다름
    if c1 % 2 == 0:
        for i in range(c1):
            l.append(str(i+1)+'/'+str(c1))
            c1 = c1-1
    
    else:
        for i in range(c1):
            l.append(str(c1)+'/'+str(i+1))
            c1 = c1-1        
            
    print(l[input_num%c-1])
    
my_num = input()
problem_1193(int(my_num))
```



분수로 구성된 대각선 배열에서 N 번째 분수를 찾는 문제입니다.
<br>간단했지만 지그재그라는 말을 놓쳐 계속 틀렸던 문제입니다....



<br>몇 번째 대각선에 속하는지 구하는 부분에서 시간문제가 생길 수 있는데, <br>문제에서 제시한 최대값 10000000 을 넣고 테스트를 했을 때 0.6 초 가량으로 크게 문제가 되지 않았습니다.

참고로 python 에서 코드의 시간 측정은 다음 코드를 사용합니다.

```python
import timeit
start = timeit.default_timer()

####### test 할 코드 #######

stop = timeit.default_timer()
print(stop - start)
```

주의할 점은 input 함수를 사용하는 경우, input 을 입력하는 시간까지 모두 포함합니다.
<br>시간 측정 테스트 할 때에는 임의로 숫자를 넣은 후 테스트를 하는것을 추천합니다.