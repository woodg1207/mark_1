[TOC]



# 02_day_>>코딩도장에서 공부하기

## 1. 스크랩핑

### 1.1 실시간 검색어 리스트

#code name.py -> gitbash를 이용한 파이썬파일생성

리스트형식

```python
.select_one('경로')
.select('경로') # 리스트로 나오는 경우  
# 리스트로 오기때문에 .text가 안나온다. 
```

ex1) 실시간 검색어 뽑기

```python
import requests
from bs4 import BeautifulSoup
url = 'https://www.naver.com/' #더 좋은 표현

html = requests.get(url).text 
#url을 받고 text형식으로 뽑는다. 

soup = BeautifulSoup(html, 'html.parser')

sites = soup.select('#PM_ID_ct > div.header > div.section_navbar > div.area_hotkeyword.PM_CL_realtimeKeyword_base > div.ah_list.PM_CL_realtimeKeyword_list_base > ul > li > a.ah_a > span.ah_k')
#ul:nth-child(6) > li:nth-child(1) 한개 지정(등수)으로 되어있다.
##ul> li로 바꿔줌 >> 리스트로 뽑기위해서 
for site in sites:
    print(site.text) # 
```

## 2.git

### 2.1 (분산)버전 관리시스템

  과거버전으로 복원 변경 비교 분석이 가능 

버전 1과 2의 차이를 알수있다. 수정이유를 남길수 있다. 

### 2.2github

**git** **setting**

git config --global user.name "woodg1207"

git config --global user.email woodg1207@gmail.com

**check**

git config --global --list

git status     #git 상태확인

**add**

git add 00_startcamp  #index에 추가 

**commit**

git commit -m "firts commit"

// commit check

​		git log

**push**

​	git remote add origin https://github.com/woodg1207/TIL.git

 		//check

​		git remote -v

git add .

집에서 할때

 git clone 주소

 버전 동기화할때는 pull을 써줘야햐 한다. 

마스터의 권한 부여 

**git ignore**

 github내에 안올릴 파일을 선택하는것  or git의 관리를 끝내는 의미 

  code .gitignore 를 통해서 파일 만들고 안올릴 것들을 넣을수있음

​		gitignore.io싸이트를 이용해서 필요 코드 복붙가능



## 3. 문자열(string)삽입

### 3.1 print(f'{a},{b}') 

```python
#과거
'%s %s' % ('one', 'two')
#pyformat(~3.5)
'{}{}'.format('one', 'two')
#f-string(new in 3.6)
a = 'one'
b = 'two'
print(f'{a},{b}') #문법이 변화함


name = '우동균'
ex = f'sjdkfjsklfdjkj{name}'
print(ex)
print(f'안녕하세요, {name}입니다.')

#점심메뉴 추천
import random

menu = ['돈까스', '라면', '빵']
lunch = random.choice(menu)
# print(f'오늘의 점심 메뉴는 {lunch}입니다.')

#lotto

numbers = range(1, 46)

lotto = random.sample(numbers, 6)
print(f'오늘의 당첨 번호는 {sorted(lotto)}입니다.')#중괄호 안에서 함수를 사용할 수 있다. 
#필요하면 이렇게 할 수 있다. 
name = '홍길동'
print('안녕하세요.'+ name+'입니다.')
```

### 3.2 파일명 바꾸기 import os

```python

import os
#1. 해당 파일들이 있는 위치로 이동
os.chdir(r'C:\Users\student\Desktop\TIL\00_startcamp\02_day\change_filenames')#   \때문에  r을 주소 앞에 써준다. 
#2. 현재 폴더 안에 모든 파일 이름을 수집
filenames = os.listdir('.')  # os.listdir('디렉토리 주소')  .은 현재위치를 의미한다.  #  파일 리스트를 구했음
#3.각각의 파일명을 돌면서 수정한다. 
#os.rename(현재이름, 바꿀이름)이름 재설정
# for filename in filenames:
#      os.rename(filename,f'SAMSUNG_{filename}')

# samsung 을  ssafy로 변환 
for filename in filenames:
     os.rename(filename, filename.replace('SAMSUNG_', 'SSAFY_'))       # 'happy.replace('h','b') h를 b로바꿈




```

