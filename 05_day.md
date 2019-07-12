# 05_day

## 1.복습

### 1.1 lotto

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
    lotto = res.json()  #딕셔너리형태
    #당첨번호 6개 갖고오기
    winner = []
    for i in range(1,7):
        winner.append(lotto[f'drwtNo{i}'])
        #append()를 이용하면 하나씩 넣는다.
    
    # 내번호 리스트 만들기 
    numbers = []
    for num in request.args.get('numbers').split():
        #공백을 잘라 리스트로 만드는 과정   .split()
        numbers.append(int(num))# 아직 문자였기때문에 int()로 형변환

    #등수 가리기(몇개 맞았는지 교집합이 필요하다.)
    matched = 0
    # 내 번호 요소를 뽑아서 당첨번호 리스트에 있는지 확인.
    for num in numbers:
        #내번호리스트를 돌면서 winner내의 값과 비교를 한다. 
        if num in winner: #winner 안에 있는지 판단
            matched += 1
    if len(numbers) == 6:
        if matched == 6:
            result ='1등입니다.'
        elif matched == 5 :
            #보너스 번호가 내 번호 리스트에 존재하면 if문을 수행
            if lotto['bnusNo'] in numbers:
                result = '2등입니다.'
            else:
                result = '3등입니다.'
        elif matched == 4:
            result = '4등'
        elif matched ==3:
            result = '5등'
        else:
            result = '꼬ㅓㅏㅇ'
    else:
        result = '번호의 수가 6개가 아닙니다.'
    return render_template('lotto_result.html',
                            winner = winner,
                            numbers = numbers,
                             result = result)

```

### 1.2 set()

set() : 집합연산자 

합집합/ 교집합 / 차집합

```python
for num in numbers:
	if num in winner:
		matched+=1
	else:
이런걸
matched = len(set(winner)&set(numbers))
같은 결과가 나온다. 
```

## 2.telegram bot 만들기

### 2.1

bot name : dong-gyun//dong_gyun_bot

token :: 가장중요, 노출X  //   

8491FPzA-aYlotq08I-jR4qzt6D0jN94E

 get 방식  외부로 노출되도 되는 정보(네이버 검색시 주소창에 검색어 뜨는경우)

post 방식 노출이 되면 안되는것 (비밀번호는 주소에 안뜬다.)

https://api.telegram.org/bot<token>/METHOD_NAME 

이 구조를 지켜야 한다.



```json
{
  "ok": true,
  "result": {
    "id": ,
    "is_bot": true,
    "first_name": "dong-gyun",
    "username": "dong_gyun_bot"
  }
}
```

chat_id : 847284941

/getName

/getUpdates  텔레그램으로 내가 들어가면 정보가 나온다.

```json
{
  "ok": true,
  "result": [
    {
      "update_id": 806251745,
      "message": {
        "message_id": 1,
        "from": {
          "id": 841,    ////내 아이디
          "is_bot": false,
          "first_name": "dong gyun",
          "last_name": "woo",
          "language_code": "ko"
        },
        "chat": {
          "id": 8,
          "first_name": "dong gyun",
          "last_name": "woo",
          "type": "private"
        },
        "date": 1562892423,
        "text": "/start",
        "entities": [
          {
            "offset": 0,
            "length": 6,
            "type": "bot_command"
          }
        ]
      }
    }
  ]
}
```



sendMessage



---> 봇에게 답장이 온다.  '안녕하세요'

##pip install python-decouple  파이썬 파일을 안보이게 만들어준다. 

.env 파일에

TELEGRAM_BOT_TOKEN='849

github에 올라가는 걸 방지하기위해서



ngrok 으로컬서버를 밖으로 개방시켜준다.  cmd로 실행~

ngrok http 5000 /// 5000번 포트로 연결해준다.

```
ngrok by @inconshreveable                                                                               (Ctrl+C to quit)

Session Status                online
Session Expires               7 hours, 59 minutes
Version                       2.3.30
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://802438e1.ngrok.io -> http://localhost:5000
Forwarding                    https://802438e1.ngrok.io -> http://localhost:5000

Connections                   ttl     opn     rt1     rt5     p50     p90
                              0       0       0.00    0.00    0.00    0.00
```

web hook 등록주소

https://api.telegram.org/bot<token>/setWebhook?url=<ngrok-forwarding-http-url>

https://api.telegram.org/bot849145138:AAFPzA-aYlotq08I-ITBljR4qzt6D0jN94E/setWebhook?url=woodonggyun12184388.pythonanywhere.com/849145138:AAFPzA-aYlotq08I-ITBljR4qzt6D0jN94E

### woodonggyun12184388.pythonanywhere.com

```

```

파파고 

클라이언트 id

HfuP3OPoRKdutG1yqC0A

클라이언트 시크릿

3QpkD1OG3r

```python
from flask import Flask, render_template, request
import requests
from decouple import config

app = Flask(__name__)

api_url = 'https://api.telegram.org'
token = config('TELEGRAM_BOT_TOKEN')
chat_id = config('CHAT_ID')
naver_config_id = config('NAVER_CLIENT_ID')
naver_config_secret = config('NAVER_CLIENT_SECRET')

@app.route('/')
def hello():
    return 'Hi there'


@app.route('/write')
def write():
    return render_template('write.html')


@app.route('/send')
def send():
    text = request.args.get('message')#write에서 갖고온다.
    requests.get(f'{api_url}/bot{token}/sendMessage?chat_id={chat_id}&text={text}')
    ##get방식
    return render_template('send.html')


@app.route(f'/{token}', methods = ['POST'])
def telegram():
    #1 데이터 구조 프린트 해보기
    from_telegram = request.get_json()
    if from_telegram.get('message') is not None:
       
        chat_id = from_telegram.get('message').get('from').get('id')
        text = from_telegram.get('message').get('text')
        #한글 키워드 받기 

        #/번역 으로 입력이 시작되면 파파고로 번역이 동작한다. 
        if text[0:4] == '/번역 ':
            headers = {
                'X-Naver-Client-Id': naver_config_id,
                'X-Naver-Client-Secret': naver_config_secret
            }
            data = {
                'source': 'ko',
                'target': 'en',
                'text': text[4:]
            }            
            papago_res = requests.post('https://openapi.naver.com/v1/papago/n2mt', headers = headers, data = data)#포스트방식
            text = papago_res.json().get('message').get('result').get('translatedText') #여기에 한영 번역텍스트가 있다.
            print(text)


        requests.get(f'{api_url}/bot{token}/sendMessage?chat_id={chat_id}&text={text}')


    return '', 200

```

배포 (dploy) - pythonanywhere