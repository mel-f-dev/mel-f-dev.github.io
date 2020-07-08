---
title: "[웹 크롤링] BeautifulSoup"
catalog: true
date: 2019-11-02 00:45:42
subtitle:
header-img: "Demo.png"
tags:
- python
- webcrawling
- TIL
- Today_I_Learned_by_Mel
- 주피터노트북
category:
- Web Crawling
---
> from 2019/10/31
>
> 웹 크롤링 정리 중



# 웹 크롤링(Web Crawling)이란?
---
- 웹 크롤링
  - 검색 엔진과 같은 여러 사이트에 있는 콘텐츠를 수집하는 작업
  - 필요한 내용을 파싱하여 추출
  - 아래 툴들이 필요하다.
    - `﻿BeautifulSoup` : 웹 데이터 크롤링 또는 스크래핑을 할 때 사용하는 python 라이브러리, 가져온 페이지에서 원하는 항목만 but 동적인 처리 못함
    - `Requests` : 웹에 있는 소스 가져오기
    - `Selenium` : 동적 처리 가능하도록
---
# BeautifulSoup
---
BeautifulSoup 라이브러리를 사용하기 위해 lxml을 설치 한다.

- BeautifulSoup
  - `HTML`이나 `XML` 문서 내에서 원하는 정보를 가져오기 위한 파이썬 라이브러리.
  - 아래 둘 중 하나를 아나콘다 프롬프트에 입력한다.
```python
# 1.
conda install beautifulsoup4
```
```python
# 2.
pip install beautifulsoup4
```

- lxml
  - Beautifusoup에서 기본적으로 지원하는 파서보다 더 빠르게 동작하는 모듈.
  - 아래 둘 중 하나를 아나콘다 프롬프트에 입력한다.
```python
# 1.
conda install lxml
```
```python
# 2.
pip install lxml
```
---
# 코딩패턴
---
1. BeautifulSoup 클래스 import
```python
from bs4 import BeautifulSoup
```
2. BeautifulSoup 객체 생성
  - 생성시 조회할 문서 내용 전달
  ```python
  with open('example.html', 'rt', encoding='UTF-8') as f:
    doc = f.read()
    print(doc)

  # BeautifulSoup 객체 생성 - 분석할 HTML과 paser(분석기) 설정
  soup = BeautifulSoup(doc, 'lxml')
  print(soup.prettify())
  ```
3. 문서 내에서 필요한 정보 조회
  - 태그이름과 태그 속성으로 조회
  - css selector를 이용해 조회
  - .표기법을 이용한 탐색  
```python
# class가 animal인 것을 모두 찾기
animal_list = soup.select("div.animal")
animal_list
```
```python
print(type(animal_list[0]))
```
```python
for animal in animal_list:
  print(animal.get_text(), animal.text)
```

---
# BeautifulSoup 객체 생성
---
```python
BeautifulSoup(html str, [,파서])
```

- 매개변수
  - 정보를 조회할 html을 string으로 전달
  - 파서(Parser)
    - html 분석기
    - html.parser: 기본 파서
    - lxml: 기본 파서보다 빠른 파서. html, xml 파싱 가능  
---
# Tag 객체
---
- Tag 객체
  - 문서내에서 원하는 정보 검색
  - 하나의 태그(element)와 관련된 정보를 담은 객체
  - BeautifulSoup 조회 메소드들의 조회결과 반환타입


- 주요 속성/메소드
  - 태그 속성값 조회
    - tag객체.get('속성명')
    - tag객체['속성명']
    - tag.get('href')
    - tag['href']

  - 태그내 text값 조회
    - tag객체.get_text()
    - tag객체.text

  - contents 속성
    - 조회한 태그의 모든 자식 요소들을 리스트로 반환
    - child_list = tag객체.contents;
---
# 조회메소드
---
- 태그 이름과 태그내의 속성으로 조회
  - find_all(name=태그명, attrs={속성명:속성값, ..})
  - find(name=태그명, attrs={속성명:속성값})
    - 일치하는 태그 반환
    - 일치하는 태그가 여러개을 경우 첫번째 것만 반환
```python
from bs4 import BeautifulSoup

with open('example.html', 'rt', encoding='UTF-8') as f:
  doc = f.read()

soup = BeautifulSoup(doc, 'lxml')
ani_list = soup.find_all("div", attrs={"calss":"animal"})

ani_list
```
```python
for ani in ani_list:
# ani 타입 조회
  print(type(ani))
# 속성값 조회
  print(ani.get_text(), ani.text)
# 클래스명 조회
  print(ani.get('class'), ani['class']))
```
```python
ani_1 = soup.find_all('div', attrs={'id':'animal1'})
```
```python
# 맞는지 확인
ani_1 = soup.find_all("div", attrs={'id':'animal1'})
ani_1.text
```
```python
a_list = soup.find_all("a")
a_list
```
```python
links = []
names = []
for a in a_list:
  links.append(a.get('href'))    # a의 href 속성 값 조회
  names.append(a.get_text())
print(links)
print(names)
```
```python
animal_cls = soup.find_all(attrs={'class':'animal'})
animal_cls
```
```python
a_list = soup.find_all('a')
a_list
```
```python
# tag.findXXXX() : tag(부모) 내의 element를 찾는다.
# soup.findXXXX() : 전체 문서 내에서 element를 찾는다.

ani_1 = soup.find('div', attrs={'id':'animal1'})
ani = ani_1.find(attrs={'class':'animal'})
ani
```
```python
soup.find('div', attrs={'id':'animal1'}).find(attrs={'class':'animal'})
```
```python
import re
# find(), find_all(): 태그명, 속성의 값에 정규표현식 pattern 사용할 수 있다.
p = re.compile(r'kr$') #kr$: kr로 끝나는 패턴.^kr : kr로 시작하는 패턴
soup.find_all('a', attrs={'href': p})
```
```python
# 모든 hx 태그
p = re.compiler(r'h\d')    # ₩d: 정수1개
p = re.compile(r'h[1-3]')
soup.find_all(p)
```

- css selector를 이용해 조회
  - select(selector='css셀렉터')
    - css셀렉터와 일치하는 tag들을 반환
  - select_one(selector='css셀렉터')
    - css 셀렉터와 일치하는 tag 반환
    - 일치하는 것이 여러개일 경우 첫번째 것만 반환
```python
# 태그명
soup.select_one("div")
```
```python
soup.select('spna, a')    # A, B A태그와 B태그
```
```python
# ID로 조회
soup.select_one('#animal1')    # 모든 태그 중 id가 animal1
```
```python
soup. select_one('div#animal2')    # div 태그 중 id가 animal2
```
```python
# class로 조회
soup.select('.animal')    # 모든 태그 중 조회
```
```python
soup.select('span.animal')
```
```python
# 하위 태그 조회
# 자식 태그 조회
soup.select("div#animal1 > div.animal")
# div#animal1의 자식인 div.animal
```
```python
soup.select('ul a')    # ul의 자손인 a
```
