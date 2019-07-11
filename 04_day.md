[TOC]



# 04_day

## 1. flask

### 1.1

source ~/.bash_profile

```python
from flask import Flask
from datetime import datetime
app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello World!'


@app.route('/ssafy')
def ssafy():
    return 'Hello World Wide Web!'


@app.route('/dday')
def dday():
    #오늘날짜
    today_time = datetime.now()
    #수료날짜
    endgame = datetime(2019, 11, 29)
    #수료날짜 - 오늘날짜
    dday = endgame - today_time
    return f'{dday.days} 일 남았습니다.'


@app.route('/html')
def html():
    return '<h1>This is HTML TAG</h1>'


@app.route('/html_line')
def html_line():
    return """
    <h1>여러줄을 보내 봅시다.</h1>
    <ul>
        <li>숫자가 안붙어요</li>
        <li>숫자가 안붙어요</li>
    </ul>
    <ol>
        <li>숫자가 붙어요</li>
        <li>sfjslkfdjlkj</li>
    </ol>
    """
```



### 1.2 variable routing

```python
@app.route('/greeting/<name>') ##기본값이라서 원래는 <string:name> 
#다른변수를 받아올때는?
def greeting(name): #name이라는 변수를 받아오기위해서 
    return f'반갑습니다.!{name}'


@app.route('/cube/<int:number>') #int 형변환
def cube(number):
    num = number**3 #pow(number,3) 제곱은 number**
    return f'{number}의 세제곱은 {num}'


@app.route('/lunch/<int:number>')
def lunch(number):
    #여러메뉴중에서 인원수 만큼의 메뉴를 응답한다. 
    ##order = random.sample(menu,number) 결과는 리스트형식으로 나온다.리턴값으로 올수없어서 
    # str(order) 형식으로 형변환을 해줘야한다. 
    menu = ['한우불고기', '코코넛 머시기', '도시락', '삼계탕', '볶음밥']
    boxes = []
    for i in range(number):
        boxes.insert(i,random.choice(menu))
    return f'여러분의 점심메뉴는{boxes}입니다.'

```

### 1.3 render template

#### 1.3.1

```python
from flask import Flask, render_template
```

```python
from flask import Flask, render_template
from datetime import datetime
import random
app = Flask(__name__)
#render template으로 
@app.route('/')
def hello():
    # return 'Hello World!'
    return render_template('index.html')
    ##flask규칙 app.py위치에 templates라는 폴더를 만들고 안에 문서를 넣어야한다.
    # !+tab을 하면 html 기본서식을 만들어준다.
```

index.html파일

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1>Hello World!!</h1>
</body>
</html>
```

#### 1.3.2 html로 변수 보내기

```python
@app.route('/greeting/<name>') ##기본값이라서 원래는 <string:name> 
#다른변수를 받아올때는?
def greeting(name): #name이라는 변수를 받아오기위해서 
    # return f'반갑습니다.!{name}'
    return render_template('greeting.html',html_name = name) #name 변수값을 옮겨주는것이중요
														#html_name을 name으로 해도 상관없다.
```

greeting.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    {{html_name}}   //중괄호 두번을 통해서 변수를 쓸수있다. jinja2 template이라 한다.
</body>
</html>
```

#### 1.3.3 세제곱 예제 

계산은 서버에서하고 변수만  html로 넘긴다.

```python
@app.route('/cube/<int:number>') #int 형변환
def cube(number):
    #여기서 모든 연산을 끝내고 변수만 html로 넘긴다.
    num = number**3 #pow(number,3) 제곱은 number**
    #return f'{number}의 세제곱은 {num}'
    return render_template('cube.html', html_num = num, html_number = number)
```



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1>{{html_number}}의 세제곱은 {{html_num}} 입니다.</h1>
</body>
</html>
```

### 1.4 jinja2활용

jinja2 ___html내에서 사용할수있다. python에서는 파일만 연결시킨다.

#### 1.4.1 if 문

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <!-- 동균이라는 이름으로 값이오면 인사하고 아니면 누구세요라고 묻는다.  -->
    {% if html_name == '동균' %}   //사람한테 안보여주는것 
        <h2>{{html_name}}, 왔니?</h2>
    {% else %}
        <h2>누구세요?</h2>
    {% endif%}
    <!-- endif라고 if문을끝내줘야한다. -->
</body>
</html>
```

1.4.2 for문

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1> 영화 목록</h1>
    {% for movie in movies %}
        <Ul><Li>{{movie}}</Li></Ul>
    {% endfor %}
</body>
</html>
```

## 2. flask request & response

###  2.1ping pong

1. /ping >>요청
2. ping.html 응답받음
3. /pong으로 요청
4. pong.html 응답

```python
@app.route('/ping')
def ping():
    return render_template('ping.html')


@app.route('/pong')
def pong():
    name = request.args.get('data') #안녕하세요
    return render_template('pong.html', name = name)
```

ping.html 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <form action = "/pong"> <!-- 만든주소  pong으로 요청 하는 것 -->
        
        <input type="text" name="data">  //딕셔너리 형태로 전달 키=data name을 꼭정해줘야한다
        <input type="submit" value="퐁!"> //퐁버튼 만들어줌
        <!-- 퐁을 누르면 /pong주소로 넘어간다. -->
    </form>
</body>
</html>
```

pong.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1>{{name}}받았음!</h1>
</body>
</html>
```

### 2.2 fake naver, google

검색을 naver, google로 하는 것

```python
#https://search.naver.com/search.naver?where=nexearch&query=
@app.route('/naver')
def naver():
    return render_template('naver.html')


@app.route('/google')
def google():
    return render_template('google.html')
```

naver.html의 바디 부분만

```html
<body>
    <h1>naver search</h1>
    <form action="https://search.naver.com/search.naver?">
        <input type="text" name="query">   	//위 주소의 물음표 이후 query를 이어준다.
        <input type="submit" value="검색!!">
    </form>
</body>
```

google.html의 body

```html
<body>
    <h1>google search</h1>
    <form action="https://www.google.com/search?">
        <input type="text" name="q"> 	//네이버의 query같은 것
        <input type="submit" value="검색!!">
    </form>
</body>
```

### 2.3 bonbon 만들기

이름을 받을 페이지

 결과가 나올 페이지

 함수도 2개 

```python
@app.route('/bonbon')
def input_name():
    return render_template('bonbon.html')


@app.route('/bonbonpong')
def result_char():
    personality = ['돈복', '외모', '운', '인성', '민첩', '힘', '지능']
    ran = range(1,11)
    
    boxes={}
    name_bon = request.args.get('data')
    for i in range(3):
        boxes[random.choice(personality)] = random.choice(ran)

    return render_template('bon.html', name = name_bon, boxes = boxes)
```

bonbon.html

```html
<body>
    <form action="/bonbonpong">
        <input type="text", name = "data">
        <input type="submit" value="성격은?">
    </form>
</body>
```

bon.html

```html
<body>
    <h1>신은 {{name}}님에게  </h1>
    {% for key, value in boxes.items() %}
        <h2>{{key}} 는  {{value}}만큼 줬습니다.</h2>
        
    {% endfor %}
</body>
```

## 3.딕셔너리

딕셔너리가 현재 가장 많이 쓰인다. 리스트보다 많이쓰임 

```python
#딕셔너리 만들기 -1
lunch = {
    '중국집': '02 0000 0000'
}
#딕셔너리 만들기 -2
dinner = dict(중국집 = '02', 일식집 = '031')

#딕셔너리에 내용추가하기
lunch['분식집'] = '031-3323-2232'
print(lunch)
```

```python
#딕셔너리 내용 갖고오기
idol ={
    'bts':{
        '지민': 25,
        'RM': 24,
    }
}
#RM의 나이는??  키를 잘찾아서 가야한다. 
# print(idol['exo']) ## 없는키는 에러가 뜸
#print(idol.get('hot'))## none이 뜬다. 
dict['key']로 존재하지 않는 key를 접근할 경우 key error가 발생 하지만, dict.get('key')로 할경우 none값을 넘겨준다. 
```

 dict['key']로 존재하지 않는 key를 접근할 경우 key error가 발생 하지만, dict.get('key')로 할경우 none값을 넘겨준다. 

```python
# 딕셔너리 반복문 활용하기
lunch = {
    '중국집': '02 0000 0000', 
    '분식집': '02 2332 2323', 
    '일식집': '02 9700 6238'
}
# 기본 활용 
# for key in lunch:
#     print(key)
#     print(lunch[key])#value값 나오게 하는 방법 print(lunch.get(key))
   
for key, value in lunch.items(): ## 한번에 나오게 하는 방법 
    print(key, value)

#value만 갖고오기
for value in lunch.values():
    print(value)
#key만 갖고오기
for key in lunch.keys():
    print(key)
```

```python
"""
Python dictionary 연습 문제
"""

# 1. 평균을 구하시오.
scores = {
    '수학': 80,
    '국어': 90,
    '음악': 100
}

# 아래에 코드를 작성해 주세요.
print('==== Q1 ====')
sum_1 = 0
for subject_score in scores.values():
    sum_1 = sum_1 + subject_score  # sum += subject_score
mean = float(sum_1/len(scores))##딕셔너리의 길이를 알수있는 함수 len(dict)
print(mean)




# 2. 반 평균을 구하시오. -> 전체 평균
scores = {
    'a': {
        '수학': 80,
        '국어': 90,
        '음악': 100
    },
    'b': {
        '수학': 80,
        '국어': 90,
        '음악': 100
    }
}

# 아래에 코드를 작성해 주세요.
print('==== Q2 ====')
sum_a = 0
sum_b = 0
for subject_score in scores.get('a').values():
    sum_a += subject_score
for subject_score in scores.get('b').values():
    sum_b += subject_score
mean = (sum_a + sum_b)/(len(scores.get('b'))+len(scores.get('a')))
print(mean)
'''
i = 0
for person_score in scores.values():
    for indivi in personscore.values():
        total_score += indivi
        i+=1
mean = total_score / i
'''
# 3. 도시별 최근 3일의 온도입니다.
city = {
    '서울': [-6, -10, 5],
    '대전': [-3, -5, 2],
    '광주': [0, -2, 10],
    '부산': [2, -2, 9],
}

# 3-1. 도시별 최근 3일의 온도 평균은?

# 아래에 코드를 작성해 주세요.
print('==== Q3-1 ====')
"""
출력 예시)
서울 : 값
대전 : 값
광주 : 값
부산 : 값
"""
sum_seoul = 0
seoul = city['서울']##city.get('서울')랑 비슷한표현
d = city['대전']
g = city['광주']
b = city['부산']
for i in seoul:
    sum_seoul += i
mean = sum_seoul / len(seoul)
print(f'{round(mean, 2)}')
sum_seoul = 0
for i in d:
    sum_seoul += i
mean = sum_seoul / len(d)
print(f'{round(mean, 2)}')
sum_seoul = 0
for i in g:
    sum_seoul += i
mean = sum_seoul / len(g)
print(f'{round(mean, 2)}')
sum_seoul = 0
for i in b:
    sum_seoul += i
mean = sum_seoul / len(b)
print(f'{round(mean, 2)}')

for name, temp in city.items():
    avg_temp = sum(temp)/ len(temp)
    print(f'{name}:{avg_temp:.2f}')





# 3-2. 도시 중에 최근 3일 중에 가장 추웠던 곳, 가장 더웠던 곳은?

# 아래에 코드를 작성해 주세요.
print('==== Q3-2 ====')
count = 0
for name, temp in city.items():
    #첫번째 시행때 
    #name = '서울'
    #temp = [-6, -10, 5]
    #단한번만 실행되는 조건이 필요 
        if count == 0:
            hot_temp = max(temp)
            cold_temp = min(temp)
            hot_city = name
            cold_city = name
        else:
            pass
            #최저온도가 cold_temp보다 낮으면, cold_temp에 넣고
        if min(temp) < cold_temp:
            cold_temp = min(temp)
            cold_city = name
        #최고온도가 hot_temp 보다 높으면, hot_temp에 넣는다. 
        if max(temp) > hot_temp:
            hot_temp = max(temp)
            hot_city = name
        count +=1 
print(f'추운곳은{cold_city}, 더운곳은{hot_city}')


# 3-3. 위에서 서울은 영상 2도였던 적이 있나요?

# 아래에 코드를 작성해 주세요.
print('==== Q3-3 ====')


if 2 in city['서울']:
    print(f'있어요')
else:
    print('없어요')
```

```python
import random
ssafy = {
    'location': ['서울', '대전', '구미', '광주'],
    'language': {
        'python': {
            'python standard library': ['os', 'random', 'webbrowser'],
            'frameworks': {
                'flask': 'micro',
                'django': 'full-functioning'
            },
            'data_science': ['numpy', 'pandas', 'scipy', 'sklearn'],
            'scraping': ['requests', 'bs4'],
        },
        'web' : ['HTML', 'CSS']
    },
    'classes': {
        'dj': {
            'lecturer': 'harry',
            'manager': '노구하',
            'class president': '박나율',
            'groups': {
                'A': ['이길현', '우동균', '이승현', '이가경', '이병재'],
                'B': ['차진권', '박성진', '심규현', '남승현'],
                'C': ['신승호', '조현호', '이병주', '박홍은'],
                'D': ['조규홍', '조수지', '임소희', '이해인'],
                'E': ['박상원', '고병권', '김준호', '신정우', '박나율']
            }
        },
        'gj': {
            'lecturer': 'change',
            'manager': 'pro-gj'
        }
    }
}


"""
난이도* 1. 지역(location)은 몇 개 있나요?
출력예시)
4
"""
for key, value in ssafy.items():
    if key == 'location':
        print(len(value))



"""
난이도** 2. python standard library에 'requests'가 있나요?
출력예시)
False
"""
i = 0
for value in ssafy.get('language').get('python').get('python standard library'):
    if value != 'requests':
        i+=1
    else:
        pass
if i != 0:
    print('아니요. 없습니다.')
else:
    pass


"""
난이도** 3. dj 반의 반장의 이름을 출력하세요.
출력예시)
박나율
"""
print(ssafy.get('classes').get('dj').get('class president'))
   

"""
난이도*** 4. ssafy에서 배우는 언어들을 출력하세요.
출력 예시)
python
web
"""
for i in ssafy.get('language').keys():
    print(i)


"""
난이도*** 5 ssafy gj반의 강사와 매니저의 이름을 출력하세요.
출력 예시)
change
pro-gj
"""
for key, value in ssafy.get('classes').get('gj').items():
    print(f'{key}이름은 {value}')

"""
난이도***** 6. framework 들의 이름과 설명을 다음과 같이 출력하세요.
출력 예시)
flask는 micro이다.
django는 full-functioning이다.
"""
for key, value in ssafy.get('language').get('python').get('frameworks').items():
    print(f'{key}는 {value}이다.')


"""
난이도***** 7. 오늘 당번을 뽑기 위해 groups의 E 그룹에서 한명을 랜덤으로 뽑아주세요.
출력예시)
오늘의 당번은 김준호
"""
lucky = random.choice(ssafy.get('classes').get('dj').get('groups').get('E'))
print(lucky)

boxes =[]
count = 0
for value in ssafy.get('classes').get('dj').get('groups').values():
    boxes.insert(count, random.choice(value))
    count += 1
print(f'{random.choice(boxes)}')
```

## 4.로또 flask

```python
import requests
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route('/lotto_check')
def lotto_check():
    return render_template('lotto_check.html')


@app.route('/lotto_result')
def lotto_result():
    #회차번호를 받아와야한다. 
    num = request.args.get('num')
    #동행복권에 요청을 보내 응답을 받는다. 
    res = requests.get(f'https://www.dhlottery.co.kr/common.do?method=getLottoNumber&drwNo={num}')
    #json형태로 바꿔준다. (우리가 크롬에서 보고있는 결과와 동일한 모습)
    lotto = res.json()
    #당첨번호 6개 갖고오기
    winner = []
    for i in range(1,7):
        winner.append(lotto[f'drwtNo{i}'])#append()를 이용하면 하나씩 넣는다.
    
    # 내번호 리스트 만들기 
    numbers = []
    for num in request.args.get('numbers').split():
        numbers.append(int(num))

    #등수 가리기(몇개 맞았는지 교집합이 필요하다.)
    matched = 0
    # 내 번호 요소를 뽑아서 당첨번호 리스트에 있는지 확인.
    for num in numbers:
        if num in winner: #winner 안에 있는지 판단
            matched += 1
    if matched == 6:
        result ='1등입니다.'
    elif matched == 5 :
        if lotto['bnusNo'] in numbers:
            result = '2등입니다.'
        else:
            result = '3등입니다.'
    elif matched == 4:
        result = '4등'
    elif matched ==3:
        result = '3등'
    else:
        result = '꼬ㅓㅏㅇ'
    return render_template('lotto_result.html',
                            winner = winner,
                            numbers = numbers,
                             result = result)
    



```

lotto_check.html

```html
<body>
    <h1>lotto</h1>
    <form action="/lotto_result">
        로또회차 : <input type="text" name = "num">
        로또번호 : <input type="text" name = "numbers">
        <input type="submit" value="로또가즈아">
    </form>
</body>
```

lotto_result.html

```html
<body>
    당첨번호 {{winner}}
    내번호 {{numbers}}
    결과 {{result}}
</body>
```

