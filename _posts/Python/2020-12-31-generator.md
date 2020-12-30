---
layout: post
title: "파이썬에서 Generator란? - yield 함수"
category: python
---

파이썬을 그냥 아무 생각하지않고 지내다가 다른 사람들의 코드를 보면 같은 언어가 맞나 싶을 때가 있다. 최근에는 그때가 바로 이 `yield` 를 만났을 때였는데, Generator라는 생소하면서도 신기한 개념이었기때문에 정리를 하고자 이렇게 글을 쓴다!

# Generator란?

Generator에 대해서 이해를 하기 전에 iterator 부터 알고 가자!

## Iterator란?

반복 가능한 객체. 즉, 반복문을 활용하여 데이터를 순회하면서 처리하는 것을 의미한다. 아래 코드 예시를 보자.

```python
a_tuple = (1, 2, 3)
b_iterator = iter(a_tuple) # tuple -> iterable -> iterator 반환

print(next(gen))
print(next(gen))
print(next(gen))
#print(b_iterator.__next__()) -> 예외 발생! 
```

- 첫 번째 라인에서 튜플을 선언어하고, 두번재 라인에서 튜플을 iter 객체로 변환했다.
- 아래 print 문에서 next() 메소드를 호출하며 내부 데이터를 순회한다.


## 다시 돌아와서, Generator란?

iterator를 생성해주는 함수를 의미한다. 함수 안에 `yield` 키워드를 사용한다.

여기서 포인트는 *모든 값을 포함하여 반환하는 대신* **호출 할 때 마다 한개의 값을 리턴** 한다는 점이다. 

그런 의미에서 아주 작은 메모리로도 효율적으로 대용량의 반복가능한 구졸르 순회할 수 있다는 점이 가장 큰 장점이다. 

### Generator의 특징

- iterable한 순서가 지정됨
- 느슨하게 평가된다
- 함수의 내부 로컬 변수를 통해 내부 상태가 유지된다.
- 무한한 순서가 있는 객체를 모델링할 수 있다. (명확한 끝이 없는 데이터 스트림)
- 자연스러운 스트림 처리를 위 파이프라인으로 구성할 수 있다.


# Generator 사용해보기!

그럼 한번 예제 코드를 확인해보자!

```python
>>> def generator():
...     yield 1
...     yield 2
...     yield 3
... 
>>> gen = generator()
>>> type(gen)
<type 'generator'>
>>> next(gen)
1
>>> next(gen)
2
>>> next(gen)
3
>>> next(gen)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

- `yield` 키워드를 사용해 generator를 만들 수 있다.
- `yield`가 호출되면 임시적으로 return이 호출되며, 한번 더 실행되면 실행되었던 `yield`의 다음 코드가 실행된다. 


- generator를 동시에 두개 생성하면, 서로 다른 객체로 이해하기 때문에 각기 따로 동작을 한다. 

```python
>>> h = generator()
>>> i = generator()
>>> h == i
False
>>> h is i
False
>>> next(h)
1
>>> next(i)
1
>>> next(h)
2
>>> next(i)
2
>>> next(i)
3
>>> next(h)
3
```

대용량의 데이터를 처리할 때 사용하는 generator!! 잘 알고 넘어가자 😆