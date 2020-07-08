---
title: "[python] 반복문 - while문"
catalog: true
date: 2019-11-03 16:58:39
subtitle:
header-img: "Demo.png"
tags:
- TIL
- by_Mel
- python
- python_while
- 파이썬반복문
- 주피터노트북
- 점프투파이썬
category:
- Python
---
> 반복문 활용을 잘 못해서 복습용으로 써보는 포스팅
>
> 출처 : [점프투파이썬-while문](https://wikidocs.net/21)
> 참고 : [Sing Happy Birthday-인문학도개발일지](https://blog.naver.com/iklues_/221669244591)

# while 반복문 개요
---
- 프로그램은 위에서 아래로 실행된다.
- 조건문과 반복문으 논리형 값으로 작업한다.
- 따라서, while문 조건이 True인 동안 계속 작업을 수행한다.

> **False인 경우**
> - 숫자: 0, 0.0
> - 문자: 빈 문자열
> - length가 0(값이 없는)인 자료구조: 튜플, 리스트, 딕셔너리
> - None

# while문 기본 구조
---
특정 조건이 True인 동안 명령문을 반복해서 실행
```python
while 조건:
  반복할 구문1
  반복할 구문2
  ...
```
```python
treeHit = 0
while treeHit < 10:
  treeHit += 1
  print("나무를 %d번 찍었습니다." % treeHit)
  if treeHit == 10:
    print("나무 넘어갑니다.")
```
```
나무를 1번 찍었습니다.
나무를 2번 찍었습니다.
나무를 3번 찍었습니다.
나무를 4번 찍었습니다.
나무를 5번 찍었습니다.
나무를 6번 찍었습니다.
나무를 7번 찍었습니다.
나무를 8번 찍었습니다.
나무를 9번 찍었습니다.
나무를 10번 찍었습니다.
나무 넘어갑니다.
```
# while문 만들기
---
***Q. 여러가지 선택지 중 '4'를 입력 받았을 때 종료하는 프로그램을 작성해보자.***
```python
prompt = """
1. Add
2. Del
3. List
4. Quit

Enter number:"""

number = 0
while number != 4:
  print(prompt)
  number = int(input())
```
```
1. Add
2. Del
3. List
4. Quit

Enter number:
1

1. Add
2. Del
3. List
4. Quit

Enter number:
2

1. Add
2. Del
3. List
4. Quit

Enter number:
3

1. Add
2. Del
3. List
4. Quit

Enter number:
4
```
# break - while문 강제로 빠져나가기
---
***Q. 커피 자판기 예제***
```python
coffee = 10
money = 300    # money 값은 300으로 고정, 계속 True
while money:    # money 값이 True인 동안
  print("돈을 받았으니 커피를 줍니다.")
  coffee -= coffee
  print("남은 커피의 양은 %d개입니다." % coffee)
  if coffee == 0:
    print("커피가 다 떨어졌습니다. 판매를 중지합니다.")
    break
```
money 값이 300으로 고정되어 있으므로 `while money:`에서 money는 항상 참이다. 따라서 `coffee -= 1`에 의해 `coffee`값이 음수가 되더라도 계속 실행되는 무한 루프를 돌게 된다.
if문 속에서 **break** 문에 의해 while문을 빠져나가게 된다.
<br>
***Q. 위의 예제를 활용하여 실제 자판기 작동 과정과 비슷하게 만들어보자.***
```python
coffee = 5
while True:
  money = int(input("돈을 넣어주세요:"))
  if money == 300:
    print("커피를 줍니다.")
    coffee -= 1
  elif money > 300:
    print("거스름돈 %d를 주고 커피를 줍니다." % (money - 300))
    coffee -= 1
  else:
    print("돈을 다시 돌려주고 커피를 주지 않습니다.")
    print("남은 커피의 양은 %d개 입니다." % coffee)
  if coffee == 0:
    print("커피가 다 떨어졌습니다. 판매를 중지합니다.")
    break
```
```
돈을 넣어주세요:4000
거스름돈 3700를 주고 커피를 줍니다.
돈을 넣어주세요:300
커피를 줍니다.
돈을 넣어주세요:200
돈을 다시 돌려주고 커피를 주지 않습니다.
남은 커피의 양은 3개 입니다.
돈을 넣어주세요:300
커피를 줍니다.
돈을 넣어주세요:300
커피를 줍니다.
돈을 넣어주세요:300
커피를 줍니다.
커피가 다 떨어졌습니다. 판매를 중지합니다.
```
# while문과 continue
---
while문 안의 문장을 수행할 때 입력 조건을 검사해서 조건에 맞지 않으면 while문을 빠져나간다.
그런데 while문을 빠져나가지 않고 while문의 맨 처음 조건문으로 다시 돌아가게 만들고 싶은 경우 continue문을 사용한다.
```python
a = 0
while a < 10:
  a += 1
  if a % 2 == 0:
    continue
  print(a)
```
```
1
3
5
7
9
```
a가 10보다 작은 동안 a는 1만큼씩 계속 증가한다. `if a%2 == 0`이 참이 되는 경우는 a가 짝수일 때이므로 continue문장을 수행한다. 따라서 a가 짝수이면 `print(a)`는 수행되지 않는다.
