---
title: "[웹 크롤링] Requests"
catalog: true
date: 2019-11-02 13:34:04
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

# URL
---
- URL(Uniform Resource Locator)
  - URL의 구성요소
    - 스키마(schema): 통신 방식
    - 호스트(host): 서버 주소
    - 패스(path): 서버에서 문서의 위치
    - 쿼리(query): 문서에 전달하는 추가 정보
- URL 예
```python
https://search.naver.com:80/search.naver?sm=top_hty&fbm=1&ie=utf8&query=scraping
```
  - 스키마(scheme): https://
  - 호스트(host): search.naver.com
  - 포트번호(port): 80. 웹브라우저들은 port 번호 생략시 80으로 전송된다.
  - 자원경로(path): search.naver
  - 쿼리스트링(query): ?sm=top_hty&fbm=1&ie=utf8&query=scraping
# urllib 모듈 - url 분석
---
- urlib
  - url과 관련된 다양한 기능을 제공하는 파이썬 내장 모듈
  - url 경로 합치기
  - url 구문 분석
  - url의 자원(페이지) 요청
- url 분석
```python
from urllib import parse
url = 'https://search.naver.com:80/search.naver?sm=top_hty&fbm=1&ie=utf8&query=scraping'
p = parse.urlparse(url)
```
```python
# 스키마
p.scheme
# 호스트
p.hostname
# 포트번호
# G: url에 없으면 안 나온다.
p.port
# 패스(자원경로)
p.path
# 쿼리
p.query
```
# URL 합치기
---
- parse.urljoin(base, url)
  - host는 동일하고 path 이후가 바뀌는 경우; 같은 사이트의 다른 자원을 조회할 때 유용
  - 중복되는 url을 base url로 지정하고 바뀌는 부분만 붙이면 편리
  - `base`+`/`+`url`형태로 합쳐준다.
```python
from urllib import parse
base_url = 'http://main.com'
path1 = '/news/it.html'
path2 = '/news/sports.html'
path3 = '/shopping/cloth.html'
print(parse.urljoin(base_url, path1))
print(parse.urljoin(base_url, path2))
print(parse.urljoin(base_url, path3))
```
```python
path = [path1, path2, path3]
for p in path:
    url = parse.urljoin(base_url, p)
    print(url, '요청')
```
# Requests 모듈
---
- HTTP 요청을 처리하는 파이썬 패키지
- 쿠키, 헤더정보 등 HTTP의 다양한 요청처리 지원
- get, host 방식 모두 지원
- 내장 라이브러리 X, 별도의 설치 필요
- 아래 둘 중 하나를 아나콘다 프롬프트에 입력
```python
pip install requests2
conda install -c conda-forge requests
```

# 요청 파라미터 전달
---
- 요청파라미터란?
  - 서버가 일하기 위해 클라이언트로 부터 받아야하는 값들
  - name=value 형태이며 여러개일 경우 &로 연결해서 전송됨
- Get 요청시 queryString 으로 전달
  - querystring : URL 뒤에 붙여서 전송한다.
  - ex) https://search.naver.com/search.naver?sm=top_hty&fbm=1&ie=utf8&query=python
  - requests.get() 요청시
    - url 뒤에 querystring으로 붙여서 전송
    - dictionary 에 name=value 형태로 만들어 매개변수 params에 전달
- Post 요청시 요청정보의 body에 넣어 전달
- HTTP 요청 헤더(Request Header)
  - HTTP 요청시 웹브라우저가 client의 여러 부가적인 정보들을 Key-Value 쌍 형식으로 전달한다.
    - accept: 클라이언트가 처리가능한 content 타입 (Mime-type 형식으로 전달)
    - accept-language: 클라이언트가 지원하는 언어(ex: ko, en-US)
    - host: 요청한 host
    - user-agent: 웹브라우저 종류
```python
import requests

url = 'http://www.pythonscraping.com/pages/warandpeace.html'

res = requests.get(url)
print(res.status_code)
if res.status_code == 200:
  print(res.text)
```

# 요청함수
---
- http 프로토콜은 왜 요청하느냐에 따라 요청 방법을 여러가지 만들어둠
    - 요청방식 두 가지: get(목적: sql의 select), post(목적: sql의 insert)
- `requests.get(URL)`
  - 주요 매개변수
    - params: 요청파라미터를 dictionary나 tuple로 전달
    - headers: HTTP 요청 header를 dictionary로 전달
        - 'User-Agent', 'Referer' 헤더 설정
    - cookies: 쿠키정보를 전달
  - 응답 결과를 Response 객체에 담아 반환
    - Response의 속성을 이용해 응답결과를 조회
    - status_code: HTTP 응답 상태코드
    - text: 응답내용(HTML)
    - content: 응답내용(응답결과가 binary 일 경우 - image, 동영상등)
```python
import requests
from urllib import parse

base_url = 'http://httpbin.org'
url = parse.urljoin(base_url, 'get')
print(url)

#요청 header 값 추가 및 변경. dictionary key: 헤더명, value: 헤더값
headers = {
    'User-Agent':'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.70 Safari/537.36',
    'my-header':'test'
}

# 요청파라미터 : GET- 1. url뒤에 queryString 붙여서 전송.
#                 2. params 매개변수에 dictionary로 넣어 설정.
# url = url+"?id=test-id&password=11111"
# print(url)

#2.
params = dict(id = 'test-id2', password='2345')
params = [('name', '홍길동'), ('name', '이순신'), ('name', '유관순')]

res = requests.get(url, headers=headers, params=params)
if res.status_code == 200:
    res_text = res.text
    print(res_text)
else:
    print("요청페이지를 못 가져옴.", res.status_code)
```


- `requests.post(URL)`
  - 크롤링시 많이 사용하지는 않는다.
  - **POST 방식 요청**
  - 주요 매개변수
    - datas : 요청파라미터를 dictionary나 tuple로 전달
    - headers: Get방식과 동일
    - cookies: Get방식과 동일
```python
import requests
from urllib import parse

base_url = 'http://httpbin.org'
url = parse.urljoin(base_url, 'post')
print(url)

headers = {
    'User-Agent':'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.70 Safari/537.36',
    'my-header':'test'
}

# POST 요청파라미터 : datas 매개변수에 dictionary나 리스트로 전달. (querystring은 사용안함.)
# 매개변수 param X, data O

datas = {"id":"my-id", "pwd":'1111', "age":[20, 30, 40]}
res = requests.post(url, headers=headers, data=datas)

if res.status_code == 200:
    res_text = res.text
    print(res_text)

# user agent : 웹브라우저 상태를 보고 뭔가 처리가 안 될 경우 응답 결과의 user agent를 확인
```
# HTTP 요청정보 구조
---
|구분자|내용|
|:---:|:---:|
|**요청라인**|url|
|**header**|클라이언트 웹브라우저 {name:value}|
|**body**|post일 때는 요청 파라미터, get 방식일 대는 아무것도 없다. cf. get 방식일 때는 url 뒤에 요청파라미터 (ex. url?get방식 요청파라미터)|
# HTTP 응답정보
---
|구분자|내용|
|:---:|:---:|
|**응답라인**|응답상태 코드|
|**header**|응답하는 서버, 데이터 양식, 상태 등등 정보|
|**body**|응답 내용, html, binary image, video 등...|
# JSON(JavaScript Object Notation)
---


- key-value 형태 또는 배열 형태의 text
- 다른 기종간 데이터 교환에 많이 이용
- object와 array를 생성하는 문법을 이용해 만듦
- JSON 모듈 : JSON 형식 문자열을 다루는 모듈
  - loads(json문자열): JSON 형식 문자열을 dictionary/list로 변환
  - dumps(dictionary): dictionary를 JSON 형식 문자열로 변환

# Dumps : dictionary를 json 형식으로
```python
import json
d = {'n1':'value', 'n2':20, 'n3':None, 'n4':True}
# dictionary -> json 문자열
j_s = json.dumps(d)
print(type(j_s), j_S)
 # 자바스크립트 기법으로 바뀐다. None -> null, True -> ture
# 키값이 문자열인 것을 확인할 수 있다. - > j_s['n1'] 조회 불가, 딕셔너리 형태지만 딕셔너리가 아니니까!
```

# Loads : json 형식 문자열을 dictionary/list로
```python
d2 = json.loads(j_s)
print(d2)
d2['n1'], d2['n2]
```
```python
import requests
from urllib import parse
base_url = 'http://httpbin.org'   
url = parse.urljoin(base_url, 'post')    # 반복문 활용시 post, get 병용할 때 활용
print(url)

headers = {
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.70 Safari/537.36',
    'my-header':'test'
}

# Post 요청 파라미터 : data 매개변수에 dictionary나 리스트로 전달.
#                    (queryStirng은 사용 안 함.)
# 매개변수 param X, data O

datas = {"id":"my-id", "pwd":'1111', "ages":[20, 30, 50]}
res = requests.post(url, headers=headers, data=datas)    
# 요청파라미터를 datas로 받아온다는 점이 get과 다르다.
if res.status_code == 200:
    res_text = res.text
    print(res_text)


import json
if res.status_code == 200:
    res_text = res_text
    res_dict = res.json() 
    # 제이슨으로 변환해서 처리해주므로 굳이 load를 하지 않아도 됨
    print(type(res_text), type(res_dict))
    print(res_dict['headers'])
    print("----------------")
    print(res_dict['headers']['Accept'])
```

# 응답결과(Response) 조회
- HTTP 응답 상태코드
  - 2XX: 성공
    - 200: OK
  - 3XX: 다른 주소로 이동 (이사)
    - 300번대이면 자동으로 이동해 준다. 크롤링시는 볼일이 별로 없다.
  - 4XX: 클라이언트 오류 (사용자가 잘못한 것)
    - 404: 존재하지 않는 주소
  - 5XX: 서버 오류 (서버에서 문제생긴 것)
    - 500: 서버가 처리방법을 모르는 오류
    - 503: 서버가 다운 등의 문제로 서비스 불가 상태

# 예제
```python
# 'url'에 접속하는 코드
# 초록색으로 된 단어들만 추출
# ToDo: 사이트 접근, 사이트 분석(크롤링할 대상, 가져올 데이터, 가져오려면 어떻게?=> 개발자 도구 활용)
import requests
from bs4 import BeautifulSoup

url = 'http://www.pythonscraping.com/pages/warandpeace.html' 
# 주소 넣어서 Enter 치면 나온다 -> 요청 파라미터 없다는 것을 의미
# 요청파라미터 : 서버가 일하기 위해 클라이언트로 부터 받아야하는 값들

# 요청 
req = requests.get(url)

# 응답코드
print(req.status_code)    # 200이 나오면 정상

# 응답이 정상이면 응답 HTML을 읽어서 변수로 저장.
if req.status_code == 200:
    req.text = req.text
# 위 응답이 정상이라면 응답 html 중 초록 단어들만 가져온다.
# <span class = 'green'>
## find를 쓸 경우
soup = BeautifulSoup(req.text, 'lxml')
green_tag = soup.find_all('span', attrs={'class':'green'})
green_content_list = []
for tag in green_tags:
    green_contents_list.append(tag.get_text().replace('\n', ' ').strip())
# tag.get_text 값을 contet_list에 append
# replace('\n', ' ') : 엔터값을 공백으로 대체
# strip() : 좌우공백 없애기

## select를 쓸 경우
# 빨간색 문장들을 조회 - <span class = 'red>
red_tags = soup.select('span.red')
red_content_list = []
for tag in red_tags:
    red_content_list.append(tag.get_text().strip())

print(len(green_content_list), len(red_content_list))
```
# Pickle로 파이썬 객체를 그대로 파일에 저장
```python
import pickle
with open('green_list.pkl', 'wb') as f:
    pickle.dump(green_content_list, f)
with open('red_list.pkl', 'wb') as f:
    pickle.dump(red_cotent_list, f)
# 읽어올 때
with open('green_list.pkl', 'rb') as f:
    green_list = pickle.load(f)
green_list[:3]   # 세 개만 출력
```

