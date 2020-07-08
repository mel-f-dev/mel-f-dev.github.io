---
title: "[python] 반복문 - for문"
catalog: true
date: 2019-11-03 14:40:13
subtitle:
header-img: "Demo.png"
tags:
- TIL
- by_Mel
- python
- python_for_in
- 파이썬반복문
- 주피터노트북
- 점프투파이썬
category:
- Python
---
> 반복문 활용을 잘 못해서 복습용으로 써보는 포스팅
>
> 출처 : [점프투파이썬-for문](https://wikidocs.net/22)
> 참고 : [Sing Happy Birthday-인문학도개발일지](https://blog.naver.com/iklues_/221671467901)

# for문 기본구조
---
```python
for 변수 in 리스트(또는 튜플, 문자열):
  수행할 문장1
  수행할 문장2
  ...
```
# 리스트와 for문
---
```python
num_list = ['one', 'two', 'three']
for i in num_list:
  print(i)
```
```python
>one
>two
>three
```
위 코드를 실행하면 num_list의 요소가 순서대로 i에 대입되어 print(i) 문장을 수행하고 리스트 마지막 요소 까지 반복한다.

# 튜플과 for문
---
```python
my_tuple = [(1,2), (3,4), (5,6)]
for (first, last) in a:
  print(first+last)
```
```python
>3
>7
>11
```
튜플의 각각의 요소가 자동으로 (first, last)변수에 대입된다.

# for문 응용
---
***Q. 총 5명의 학생이 시험을 보았는데 시험 점수가 60점이 넘으면 합격이고 그렇지 않으면 불합격이다. 합격인지 불합격인지 결과를 보여 주시오.***
```python
marks = [90, 25, 67, 45, 80]

number = 0
for i in marks:
  number += 1
  if i >= 60:
    print("{}번 학생은 합격입니다.".format(number))
  else:
    print("{}번 학생은 불합격입니다.".format(number))
```
```python
>1번 학생은 합격입니다.
>2번 학생은 불합격입니다.
>3번 학생은 합격입니다.
>4번 학생은 불합격입니다.
>5번 학생은 합격입니다.
```
# for문과 continue
---
for문 안의 문장을 수행하는 도중 continue문을 만나면 for문의 처음으로 돌아간다.
***Q. 위 문제에서 60점을 넘은 합격자에게 축하 메시지를 보내고 나머지에게는 아무 메시지도 전하지 않는 프로그램을 작성해보자.***
```python
marks = [90, 25, 67, 45, 80]

number = 0
for i in marks:
  number += 1
  if i < 60:
    continue
  print("{}번 학생 축하합니다. 합격입니다.".format(number))
```
```python
>1번 학생 축하합니다. 합격입니다.
>3번 학생 축하합니다. 합격입니다.
>5번 학생 축하합니다. 합격입니다.
```
# for in 연관 내장함수
---
## range()
- range([시작값], 멈춤값, [증감값]) : []는 생략 가능
- 시작값은 포함, 멈춤값은 포함X
- 시작값 생략시 0이 기본값
- 증감값 생략시 1이 기본값
- 멈춤값은 생략 불가
![range함수](https://blogfiles.pstatic.net/MjAxOTEwMDdfODYg/MDAxNTcwNDA5NDUyNDA3.k9MBnzkzcnnDIWlDvKjluZRAfMvBzOl5lrXlfKVzbZ8g.fts1Rjtb63ZTSpwDeNnINkdERHJPcLUlV1FZTz5q_Gwg.PNG.iklues_/image.png "파이썬 range함수 용례")
***Q. 위의 문제를 range함수를 활용해서 해결해보자.***
```python
marks = [90, 25, 67, 45, 80]
for i in range(len(marks)):
  if marks[i] < 60:
    continue
  print("{}번 학생 축하합니다. 합격입니다.".format(i+1))
```
## enumerate()
- 반복 조회시 현재 원소의 인덱스와 원소를 튜플로 묶어서 반환
```python
s = ['A', 'B', 'C', 'D']
for i, ss in enumerate(s):
  if i%2 == 0:
    print(i, ss)
```
```python
>0 A
>2 C
```
- 전달인자 start : 시작할 때 인덱스 값을 지정, 기본값은 0
```python
l = [1, 2, 3, 4]
for i, num in enumerate(l, start = 100):
  print(i, num)
```
```python
>100 1
>101 2
>102 3
>103 4
```
## zip()
- 여러개의 자료구조 객체를 받아 같은 인덱스 값끼리 튜플로 묶어서 반환
- 반복할 때 마다 같은 인덱스의 값들을 묶어서 반환
- 묶는 자료구조 객체의 개수는 상관없다.
- 각 자료구조 객체의 크기가 다를 경우 작은 것의 개수에 맞춘다.
```python
ids = ['id-1', 'id-2', 'id-3', 'id-4']
ages = [20, 25, 30, 28]
talls = [180, 178, 184, 182]
```
```python
for info in zip(ids, ages, talls):
  print(info)
```
```python
>('id-1', 20, 180)
>('id-2', 25, 178)
>('id-3', 30, 184)
>('id-4', 28, 182)
```
```python
for id, age, tall in zip(ids, ages, talls):
  print(f'아이디:{id}, 나이:{age}, 키:{tall}')
```
```python
>아이디:id-1, 나이:20, 키:180
>아이디:id-2, 나이:25, 키:178
>아이디:id-3, 나이:30, 키:184
>아이디:id-4, 나이:28, 키:182
```
# 리스트 내포(컴프리헨션)
---
- 기존 자료구조가 가진 원소들을 이용해 새로운 자료구조를 만드는 구문
- 주로 기존 자료구조의 원소들을 처리한 결과를 새로운 자료구조에 넣을 때 사용
- **튜플 컴프리헨션은 없다.**
- 딕셔너리/집합 컴프리헨션은 파이썬 3.0부터 지원
## 리스트 컴프리헨션
```python
# for문을 하나만 사용할 때
[표현식 for 항목 in 반복가능객체 if 조건문]
# for문을 두 개 이상 사용할 때
[표현식 for 항목1 in 반복가능객체1 if 조건문1
       for 항목2 in 반복가능객체2 if 조건문2
       ...
       for 항목n in 반복가능객체n if 조건문n]      
```
```python
results = [num * 10 for num in range(10)]
results
```
```python
>[0, 10, 20, 30, 40, 50, 60, 70, 80, 90]
```
## 딕셔너리 컴프리헨션
```python
l = [0, 10, 20, 30, 40, 50, 60, 70, 80, 90]
{'key'+str(i+1):num+10 for i, num in enumerate(l)}
```
```python
>
{'key1': 10,
 'key2': 20,
 'key3': 30,
 'key4': 40,
 'key5': 50,
 'key6': 60,
 'key7': 70,
 'key8': 80,
 'key9': 90,
 'key10': 100}
```
