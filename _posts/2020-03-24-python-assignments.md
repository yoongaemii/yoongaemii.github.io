---
layout: post
title: "헷갈리는 파이썬 변수 할당과 변경"
tags:
  - python
excerpt: "파이썬에서 에러가 나지는 않지만 프로그램이 의도대로 작동하지 않는 경우, 문제를 되짚어가다 보면 맞닥뜨리는 것은 두 가지 중 하나이다. 첫번째는 '이 변수 왜 바뀌어있지?'이고 두번째는 '저 변수는 왜 그대로지?'다. 그럴 때 파이썬의 기본 작동 방식을 되짚어 볼 필요가 있다."
---
파이썬으로 알고리즘 문제를 풀면서 바꾼 적 없는 변수의 값이 바뀌어 있고, 변경했다고 생각한 값이 그대로인 경우를 자주 경험했다. 그럴 때마다 대수롭지 않게 수정하고 넘어갔는데 얼마 전에 게슈탈트 붕괴가 왔다. 단순한 할당 코드 한 줄을 쓰면서도 이 한 줄 정확히 무슨 일을 하고 있는지 혼란스러웠다. 이런 저런 글들을 참고하여 헷갈렸던 부분에 대해 정리해봤는데, 특히 [Facts and myths about Python names and values](https://nedbatchelder.com/text/names.html)라는 글이 크게 도움이 됐다.

## 변수의 할당에 대한 오해들
내가 부딪힌 문제를 단순화하면 다음과 같다.
```python
spam = 42
cheese = spam
spam = 100
spam
>>> 100
cheese
>>> 42
```
두번째 줄에서 spam이 cheese와 포인터를 공유하면서 spam의 값이 바뀌면 cheese의 값도 바뀔 것이라고 생각했다. 이는 몇 가지 점에서 파이썬에서의 변수 할당을 오해한 것이다.
1. 변수들은 값을 참조한다.
2. 변수들은 독립적으로 참조한다.
3. 할당 선언은 변수의 값을 바꾸지 않는다.  

세 가지 사실을 종합해보면, `cheese = spam`으로 마법처럼 두 변수가 연동되는 것이 아니라 그 순간에 spam이 가리키고 있는 42이라는 값을 cheese도 똑같이 가리키게 되는 것이다. 그 이후 `spam = 100`에서는 42라는 값을 변경하는 것이 아니라 100이라는 새로운 변수를 `spam`에 할당한다.  

이것을 스택오버더플로우의 누군가는 '라벨'으로 설명했다. 즉, 파이썬에서 메모리 주소를 데이터를 담는 버킷으로, 변수의 이름을 버킷에 붙이는 이름표라고 생각하는 것이다. 위 코드는 42를 담고 있는 버킷에 spam라는 라벨을 붙이고 cheese라는 라벨을 붙였다가, spam 라벨을 떼어 100을 담고 있는 버킷에 붙인 것과 같다.

## 파이썬에서 변수를 할당하면 일어나는 일
비유도 좋지만, 이제 정확히 어떤 일이 일어나는지 살펴보자. 가장 기본적인 할당문의 형식은 다음과 같다.
```python
target = expression
```
파이썬은 오른쪽에 있는 expression의 값을 평가(evaluate)하여 왼쪽에 있는 target과 묶는다. 자세히 들여다보면 이는 값을 담고있는 메모리의 위치를 일컫는 '이름'을 만들어준 것이다. 파이썬이 레퍼런스를 통해서 데이터에 접근한다는 것은 이런 내부 사정을 뜻한다.  

하나의 레퍼런스는 여러 개의 이름을 가질 수 있다. 변수들이 독립적으로 같은 값을 참조하는 상황이다. 위의 예시에서 cheese와 spam은 42라는 값을 참조하는 서로 다른 이름이었다.  

위 선언문에서 왼쪽에 있는 target은 그냥 '상징'이다. 내가 처음 오해했던 것처럼 target이 참조하고 있는 데이터 값을 변경하는 것은 불가능하다. 이것은 할당이 아니며, mutable object에 한해 다른 형태의 operation이 지원된다.

## 정수와 리스트는 할당 방식이 다르다?
지금까지 '데이터를 변경했다고 생각했는데 변경되지 않은 경우'를 살펴보았다. 그 문제의 핵심은 변경이라고 생각했던 할당문이 사실은 새로운 오브젝트를 만들어 별개의 참조를 생성한다는 점에 있었다. 우리는 반대의 경우 - 엥 저것만 바꾸려고 했는데 이것도 바뀌어있네 -도 종종 겪는다. 리스트와 같은 mutable object에서 겪는 일이다.
```python
x = [1,2,3]
y = x
x.append(4)
```
이 경우 y를 출력해보면 결과는 [1,2,3,4]이다. 이 예시를 보고 숫자와 같은 immutable object와 리스트나 딕셔너리 같은 mutable object가 할당 방식이 다르다고 오해할 수 있겠지만 사실이 아니다. 오브젝트의 종류와 상관없이 파이썬에서 할당은 동일하게 작동한다. y는 x와 함께 `[1,2,3]`이라는 리스트를 참조하게 된다. x.append(4)에서 둘이 함께 가리키고 있는 [1,2,3]을 수정했다. 여러 개의 이름이 하나의 데이터 레퍼런스를 공유하고 있을 때, 값의 변경 사항은 모든 이름에서 관찰된다. cheese와 spam의 예시에서 우리는 변수를 재할당한 것이며, 이 예시에서는 데이터 값 자체를 수정했다고 볼 수 있다.

```python
x = [1,2,3]
y = x
x[0] = 10
```
위 예시 역시 y에서도 [10,2,3]을 관찰할 수 있다. 그러면 이런 질문을 할 수 있겠다. "할당은 값을 변경하는게 아니라 rebinding이라면서요? 데이터 값 자체가 바뀌어야 y가 바뀌는 것 아닌가요?" 이는 리스트 내부의 사정을 보면 이해할 수 있다. 리스트는 값을 그대로 저장하는 것이 아니라, 데이터 값을 참조하는 레퍼런스를 원소로 가지고 있다. `x[0] = 10`에서 우선 `x[0]`이라는 변수에 새로운 레퍼런스가 할당된다(1을 변경하는 것이 아니라). 하지만 x와 y 입장에서는 가리키고 있는 데이터의 값이 변경된 것이므로 y에서도 변경사항을 관찰할 수 있다.
