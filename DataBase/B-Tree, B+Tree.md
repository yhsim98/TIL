# B-Tree, B+Tree 이란?
이것은 인덱스나 Mysql를 이루고 있는 자료구조의 일종이다.  
인덱스는 내부적으로 B-Tree로 구성되어 있고 Mysql은 DB engine인 InnoDB로 이뤄져있는데  InnoDB는 B+로 구성되어 있다.

# 들어가기 전 디스크 구조
컴퓨터는 프로세스, 메모리, 입출력장치로 이루어져 있다. 이 중 데이터는 메모리 그 중에서도 비휘발성을 가진 보조기억장치 즉 디스크에 저장되게 된다. 

[디스크에 대한 정리](https://github.com/yhsim98/TIL/blob/master/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/%ED%8C%8C%EC%9D%BC%EC%8B%9C%EC%8A%A4%ED%85%9C.md)

프로세스에서 실행중인 프로그램이 데이터에 접근할 때 성능을 위해 메인 메모리에서 디스크에 접근해 데이터를 가져오고 프로세스에서는 메인 메모리에서 데이터를 가져오게 된다.

이때 메모리에서는 디스크에 있는 데이터의 실제 주소값을 가지고 디스크에 접근하게 된다. 

InnoDB에서는 이러한 데이터에 접속의 효율성을 위해 B+Tree를 사용하여 데이터의 주소값을 관리하고 있다.  

**% 주의 % 제가 나름대로 찾아보고 정리한 내용이라 틀릴 수 있습니다.** 

# B-tree
트리구조로서 데이터 검색의 효율성을 보장해 준다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FqycZ2%2FbtqBQnr4QYG%2F7J8KpnmNaJiTjgS0K9TEIK%2Fimg.png)

위 사진처럼 Root, Branch, Leaf 이렇게 3가지 종류의 노드로 구성되어 있다.

[직접 B-Tree 만들어 보기](https://www.cs.usfca.edu/~galles/visualization/BTree.html)

각 노드는 여러 key, value쌍을 가지고 있다. 여기서 key값을 이용하여 트리구조로 원하는 value 즉 데이터의 주소에 접근하게 된다. 


# B-tree 특징
B-tree 구조적 특징
1. root node가 leaf node인 경우를 제외하고는 항상 최소 2개의 children을 가진다
2. root node와 leaf node를 제외한 모든 node 들은 최소 M/2, 최대 M개의 서브트리를 가진다
3. 모든 leaf node들은 같은 level에 있어야 한다
4. 새로운 key 값은 leaf node에 삽입된다
5. node 내의 key 값들은 오름차순이다
6. leaf node는 최소 M/2 - 1 개의 key를 가지고 있어야 한다

이 외 삽입 삭제 등등시 원리를 따로 찾아보기 바란다.

B-tree 특징
* B-Tree는 균형트리로서 어떤 값에 대해서도 같은 시간에 검색결과를 얻을 수 있다. 
* B-Tree는 Root Branch Leaf 3가지 종류의 노드들이 전부 value 즉 데이터의 주소를 가지고 있다.

# B+tree
B+tree는 B tree와 비슷하지만 value를 leaf노드만 가지고 있고, 리프 노드들끼리 linked list로 연결되어 있다.


# B+tree 장점
1. 리프 노드를 제외하고 데이터를 담아주지 않기 때문에 메모리를 더 확보함으로써 더 많은 key들을 수용할 수 있다. 하나의 노드에 더 많은 key 들을 담을 수 있기에 트리의 높이는 더 낮아진다.(cache hit를 높일 수 있음)
2. 풀 스캔시, B+tree는 리프 노드에 데이터가 모두 있기 때문에 한 번의 선형탐색만 하면 되기 때문에 B-tree에 비해 빠르다. B-tree의 경우에는 모든 노드를 확인해야 한다.

* InnoDB B+tree
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCbs9b%2FbtqBVf7DVW2%2F8JOOKlHiwkoTsqbvbTt7R1%2Fimg.png)

InnoDB의 경우는 Linked List가 아닌 Double Linked List를 사용했다. 






### 출저
* https://zorba91.tistory.com/293
* https://potatoggg.tistory.com/174
* 