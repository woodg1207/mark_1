# 02_workshop

###### 1. 두 개의 정수 n과 m만큼의 직사각형 만들기

```python
n = 5
m = 9
a='*'
for i in range(m):
    print(f'{a*n}')
```

###### 2.과목명과 점수가 담긴 딕셔너리가 있을 때, 평균 점수를 출력하시오.

```python
student = {
    'python' : 80,
    'algoritm' : 99,
    'django' : 89,
    'flask' : 83
}
sum = 0
cnt = 0
for point in student.values():
    cnt += 1
    sum += point
print(sum/cnt)
```

###### 3.다음은 여러 사람의 혈액형() 에 대한 데이터 이다. 반복문을 사용하여 key 는 혈액형의 종류, value는 인원수인 딕셔너리를 만들고 출력하시오.

```python
blood_types = ['A', 'B', 'A', 'O', 'AB', 'AB', 'O', 'A', 'B', 'O', 'B', 'AB']
result ={}
for i in blood_types:
    result[i] = blood_types.count(i)
print(result)
```



