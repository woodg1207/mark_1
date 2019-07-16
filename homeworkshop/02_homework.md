# 02 python 2. 데이터 구조

###### 1.아래 보기 중, 변경 가능과 변경 불가능

변경 가능 : List, Set, Dictionary(value)

변경 불가능 :  Tuple, Range, String, Dictionary(key)

###### 2. range와  slicing을 활용하여 1부터 50까지 숫자 중 홀수로 이루어진 리스트를 만드시오.

```python
boxes = []
for i in range(1,51):
    if i %2:
        boxes.append(i)
print(boxes)
#########
a= list(range(1,51))
b=a[0::2]
print(b)
```

###### 3.반 학생들의 정보를 이용하여 key는 이름, value는 나이인 딕셔너리를 만드시오.

```python
student_dict = {
    '이름1' : '나이',
    '이름2' : '나이2'
}
```

