#  Parse Tree



> [Problem Solving with Algorithms and Data Structures using Python](http://interactivepython.org/runestone/static/pythonds/SortSearch/TheBubbleSort.html) 을 공부하고 정리한 내용입니다.



Parse Tree 에 대해 설명을 하기 전에 Binary Tree 에 대해 보겠습니다.



### Binary Tree 구현하기

노드가 key 값과 leftchild, rightchild 로 구성된 트리입니다.<br>insert 할 때 그 위치에 다른 값이 있으면 새 노드를 만들고 새 노드의 자식으로 달아주는 방식을 사용했습니다.<br>( traverse 에 관한 내용은 뒤의 포스팅에 이어서 다루도록 하겠습니다. )

```python
class BinaryTree:
    def __init__(self,rootObj):
        self.key = rootObj
        self.leftChild = None
        self.rightChild = None
        
    def insertRight(self,newNode):
        if self.rightChild == None:
            self.rightChild = BinaryTree(newNode)
        else:
            t = BinaryTree(newNode)
            t.rightChild = self.rightChild
            self.rightChild = t
            
    def insertLeft(self,newNode):
        if self.leftChild == None:
            self.leftChild = BinaryTree(newNode)
        else:
            t = BinaryTree(newNode)
            t.leftChild = self.leftChild
            self.leftChild = t
            
    def getRightChild(self):
        return self.rightChild

    def getLeftChild(self):
        return self.leftChild

    def setRootVal(self,obj):
        self.key = obj

    def getRootVal(self):
        return self.key
```



### Binary Tree 사용하기

Stack, Queue 와 마찬가지로 BinaryTree 역시 파이썬에서 제공하는 라이브러리 ``pythonds``로 사용할 수 있습니다.

> 참고로, ``pythonds`` 는 standard library 는 아니기 때문에 코딩테스트 나 대회에서는 사용할 수 없습니다.<br>python standard library 리스트는 [여기](https://docs.python.org/3.6/library/) 에서 확인하실 수 있습니다.





### Parse Tree

Parse Tree 는 계층적인 순서를 나타낼 때 많이 사용하는데, 대표적으로 문장구조 분석이나 사칙연산이 있습니다.

![](http://interactivepython.org/runestone/static/pythonds/_images/meParse.png)

위와 같이 가장 먼저 계산되어야 하는 * 연산이 맨 위 계층을 차지하고, 리프노드에는 계산해야 할 숫자들이 위치하는 것을 알 수 있습니다.

사칙연산 식으로 파스트리를 어떻게 만드는지 예제를 통해 하나씩 알아보도록 하겠습니다.



#### 사칙연산 식에서 Parse Tree 만들기

```
(3+(4*5))
```

간단한 예제를 구현해 보겠습니다.

고려해야 할 사항은

1. 왼쪽 괄호 : 새로운 operation 시작,  오른쪽 괄호 : operation 종료
2. 모든 operation 은 leftchild 와 rightchild 를 가짐

입니다. 

즉, 

1. 왼쪽 괄호가 나오면 새로운 tree 생성
2. 오른쪽 괄호가 나오면 해당 tree 의 부모노드로 이동
3. 연산자는 leaf 모드가 아닌 곳에 있어야 함

의 조건을 가집니다.



