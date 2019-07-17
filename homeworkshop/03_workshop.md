# 03_workshop

###### Palindrome은 앞에서 읽었을 때와 뒤에서 부터 읽었을때 같은 단어를 뜻한다. 

###### 단어를 입력 받아 palindrome을 검증하고 true or false를 리턴하는 함수 

###### palindrome(word)를 만들어라

```python
def palindrome(word): # naan
    list_word = list(word) #[n a a n]
    #리스트 요소의 양쪽 끝끼리 계속 비교하면서 진행
    for i in range(len(list_word) // 2): #2가나온다.
        if list_word[i] !=list_word[-i-1]:
            return False
    return True

a = input('')
print(palindrome(a))

if word == word[::-1]:
```



