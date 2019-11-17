---
layout: post
title: geeks for geeks로 알고리즘 공부하기
tags:
  - Basic
  - study-note
excerpt: 
---

geeks for geeks에서 [10 most commonly used Data Structure이라는 아티클](https://www.geeksforgeeks.org/introduction-to-data-structures-10-most-commonly-used-data-structures/)을 보고 우선적으로 학습해야할 자료구조를 상위 여덟가지만 정리해봤다. 알고보니 그냥 geeks for geeks DS 탭에 있는 자료구조 순서대로 쓴 거였음..
  
자료구조의 이름을 처음 들어본다면 introductory 수준에 해당하는 영상강좌를 보고 임하는 것을 추천(그냥 개인적으로 아주 처음 배울 땐 영상을 선호해서, 도움 될만한 영상 링크는 보이는 대로 추가 예정). 각 자료 구조당 geeks for geeks에서 관련 아티클을 선정했다. 아티클은 자료구조의 특성과 관련된 토픽(예: array에서 순환 array)이 있고 그 토픽 안에 구체적인 문제 상황(순환 어레이를 구현하는 문제, 순환 어레이에서 특정 element를 찾는 문제) 등이 제시된다.  
  
array와 linked list를 제외했다. 조금 보니 문제들이 너무 쉽거나 다른 자료구조를 써서 풀어야 하는 문제여서. 나머지는 자료 구조 하나 당 토픽 2개를 선정한다. 그냥 주관적으로 익숙한 이슈들을 골랐다. 토픽 당 하나의 아티클을 선정하되, __순서 상 나중에 나오는 자료구조를 배우지 않았다는 가정 하에 풀 수 있는 문제__를 골랐다(생각보다 쉽지 않음).  


## 1. Array
(https://www.geeksforgeeks.org/array-data-structure/)
  
- 연속하는 메모리 공간에 아이템을 저장
- 같은 타입의 아이템을 저장하는데 사용하므로 각 아이템의 위치를 파악하기 용이하다. 그냥 데이터 크기만큼 더하고 빼고 하면 되니까
- random access: 어떤 아이템에든 (인덱스를 활용해) 접근이 가능하다
- better cache locality: __앞으로 공부해 볼 것__
- python에서는 generic container인 `list`말고도 `array`라는 모듈을 제공한다: [array 공식문서](https://docs.python.org/3/library/array.html#module-array)

> `list` vs `array`(https://www.pythoncentral.io/the-difference-between-a-list-and-an-array/)  
> 둘 다 어떤 형태의 데이터든 저장할 수 있으며, index를 통해 element에 접근하고 iterate할 수 있다는 것은 동일. 그러나 array는 함수를 적용할 때 elementwise로 적용된다. R에서의 기본 어레이 형태와 비슷하게 이해하면 될듯

나중에
https://www.geeksforgeeks.org/search-an-element-in-a-sorted-and-pivoted-array/

## 2. Linked List
https://www.geeksforgeeks.org/data-structures/linked-list/
- 어레이와 다르게 아무데나 저장하고 포인터를 활용해 알려주는 방식
- *1) 사이즈가 고정되어 있고(미리 저장공간을 할당해야함) 2) 새로운 아이템을 insert하는 비용이 높다*는 array의 한계를 보완
- 반대로 1) random access 불가 2) 포인터를 위해 extra memory 필요 3) not cache friendly 하다는 단점이 있다


## 3. [Stack](https://www.geeksforgeeks.org/stack-data-structure/)

## 4. [Queue](https://www.geeksforgeeks.org/queue-data-structure/)

## 5. [Binary Tree](https://www.geeksforgeeks.org/binary-tree-data-structure/)

## 6. [Binary Search Tree](https://www.geeksforgeeks.org/binary-search-tree-data-structure/)

## 7. [Heap](https://www.geeksforgeeks.org/heap-data-structure/)

## 8. [Hashing Data Structure](https://www.geeksforgeeks.org/hashing-data-structure/)

## 보조 영상
[인프런 알고리즘 무료 강좌](https://www.inflearn.com/course/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EA%B0%95%EC%A2%8C/#)

