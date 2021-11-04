# Python 문법

파이썬에서 자주 사용하는 문법을 간략히 정리한 내용입니다.



## 1-1. 정수형

정수를 다루는 자료형 (0, 음의 정수, 양의 정수)

```python
a = 100
print(a)

b = -30
print(b)
```

## 1-2. 실수형

소수점 아래의 데이터를 포함하는 숫자 자료형 (소수부가 0일때 0 생략 가능)

실수를 비교시 round함수를 이용하여 반올림해서 비교하기

```python
a = 133.23
print(a) #133.23

b = -393.12
print(b) #-393.12

c = 5.
print(c) #5.0

d = 0.3 + 0.6
print(d) #0.899999

if round(d,1) == 0.9:
  	print("yes")
else:
		print("no")
```

## 1-3. 지수 표현

e나 E 다음에 오는 수는 10의 지수부를 말합니다. (ex: 1e9 -> 10의 9제곱)

```python
a = int(1e9) #정수로 사용시 int() 감싸주기
print(a) #1000000000.0

b = 75.25e1
print(b) #752.5
```

## 1-4. 연산

- 목을 얻기 위한 몫 연산자 (//)
- 거듭 제곱 연산자 (**)
- 나누기 연산자 (/) -> 실수형 return
- 나머지 연산자 (%)

## 2-1. 리스트

여러 데이터를 연속적으로 담아 처리하기 위한 자료형 (자바의 배열 기능과 유사 기능), 배열 또는 테이블이라고도 부릅니다. `[]` 안에 데이터를 넣어 초기화하며 `,`로 원소 구분합니다.

**데이터 변경이 가능**하며 빈 리스트를 선언시 `list() or []` 로 표기할 수 있으며 접근시엔 인덱스(index) 값을 사용합니다.

```python
a = [1,2,3,4,5]
print(a) #출력

print(a[3]) #4

length = 5
b = [0] * length
print(b) #[0,0,0,0,0]
```

### 2-1-1. 슬라이싱

연속적 위치를 가지는 원소를 가져올때 슬라이싱을 사용한다. `[:]` 표기법을 사용하며 : 앞은 시작인덱스 뒤엔 끝 인덱스를 설정할 수 있습니다. (항상 끝 인덱스는 +1)

```python
a = [1,2,3,4,5]

print(a[0:2])  # 	[1,2]
```

### 2-1-2. 컴프리헨션

리스트를 초기화 하는 방법중 하나로 대괄호 안에 조건문, 반복문을 사용해 리스트를 초기화 하는 방법을 말합니다. 2차원 리스트 초기화할 때 효과적으로 사용할 수 있으며 n * m 크기의 리스트를 한번에 초기화 할 때 장점을 가지고 있습니다.

```python
arr = [i for i in range(5)] # 0부터 4까지 반복하며 i를 배열에 담아 return
print(arr) # [0,1,2,3,4]

arr2 = [i for i in range(10) if i % 2 == 1] # 0부터 9까지 반복하며 i가 2로 나누었을때 1이 나머지로 떨어지는 i만 return (홀수만)
print(arr2)

arr3 = [i * i for i in range(1,10)] #값의 제곱을 리턴하기
print(arr3)

n, m = 5, 3 # 5번 반복 1개의 배열의 3개 데이터
arr4 = [[0] * m for _ in range(n)] #반복을 수행할때 반복 변수값을 무시하고자 할때 _를 사용 
print(arr4) #[[0,0,0], [0,0,0]...]

```

### 2-1-3. 관련 메소드

| 함수명    | 표기                          | 설명                                           | 시간복잡도 |
| --------- | ----------------------------- | ---------------------------------------------- | ---------- |
| append()  | 변수명.append()               | 원소 하나 삽입시 사용                          | O(1)       |
| sort()    | 변수명.sort()                 | 기본 정렬 (오름차순, reverse=True 시 내림차순) | O(_NlogN_) |
| reverse() | 변수명.reverse()              | 원소 순서 뒤집기                               | O(_N_)     |
| insert()  | 변수명.insert(삽입인덱스, 값) | 특정 위치에 원소 삽입시 사용                   | O(_N_)     |
| count()   | 변수명.count(값)              | 특정 값을 가지는 데이터 갯수                   | O(_N_)     |
| remove()  | 변수명.remove(값)             | 특정 값을 갖는 원소 제거시 (1개 제거)          | O(_N_)     |

**특정 값 원소 제거**

```python
a = [1,2,3,4,5,6,6,7,10]
remove_set = [5,6,10]

# remove_set에 있는 것 지우기
# 순차적으로 작성하기 -> i return, a를 반복적으로 돌면서 a의 데이터가 if remove_set not in일 때 표기
result = [i for i in a if i not in remove_set]
print(result) #[1, 2, 3, 4, 7]

```

## 3. 튜플

리스트와 비슷한 자료형으로 한번 **선언된 값을 변경할 수 없습니다.** 표기는 `()` 를 사용하며 리스트에 비해 적은 메모리를 사용하여 조금 더 효율적입니다. 튜플을 사용하기 좋은 경우는 다른 성질의 데이터를 관리할때이며 최단 경우 알고리즘에서 자주 사용합니다. 또한 데이터의 묶음을 해싱의 키값을 사용하거나 메모리를 효율적으로 사용해야하는 경우도 쓰기 좋습니다.

### 3-1. map 내장함수

파이썬 내장함수는 반복적으로 여러 데이터를 한번에 다른 형태로 변환할 때 사용합니다. 여러 데이터가 담긴 리스트와 튜플에 자주 사용되며 문법과 의미는 아래 코드와 같습니다.

```python
# 리스트와 튜플 응용하기
list(map(변환 함수, 리스트))
tuple(map(변환 함수, 튜플))

# ex -> 실수리스트를 정수리스트로 변환
a = [1.2, 3.4, 5.7, 1.8]
for i in range(len(a)):
  a[i] = int(a[i])
print(a) #[1, 3, 5, 1]

#map 내장함수로 실수리스트를 정수리스트로 변환
a = list(map(int, a))
print(a) #[1, 3, 5, 1]

b = list(map(str, range(5)))
print(b) ['0','1','2','3','4']

#여러 숫자 데이터 입력 받을때 int 형변환시 map으로 변환 후 list로 감싸기
data = list(map(int, input.split())) # 30 10 8
print(data) #[30, 10, 8]
```

## 4. 딕셔너리

키 : 값 `({key : value})` 의 쌍을 하나의 데이터로 가지는 자료형을 말합니다. 딕셔너리라고도 부르며 해시테이블을 사용해서 조회 및 수정 작업에서는 `O(1)` 시간복잡도를 가집니다.

key값을 통해 접근하여 value값을 변경할 수 있습니다.

```python
team = dict()
team['eng'] = 'chelsea'
team['fra'] = 'paris'

print(team)

list(team.keys()) #키 데이터만 담은 리스트
list(team.values()) #값 데이터만 담은 리스트

for data in team:
  print(team[data]) #chelsea paris
```

## 5. 집합 자료형 (set)

집합자료형은 **중복을 허용하지 않으며**, 순서(인덱싱)이 존재하지 않습니다. 리스트나 문자열을  `set()` 함수를 사용하여 초기화할 수 있으며 사전자료형과 마찬가지로 조회 및 수정작업에서 `O(1)` 시간으로 처리할 수 있습니다. 

```python
set_1 = set([1,2,3])
l1 = list(set_1)
print(l1) #[1,2,3]
```

만약 set 자료형에 인덱싱을 사용해 접근하려면 위에 코드와 같이 리스트나 튜플함수를 이용해 변환하여 사용해야 합니다.

### 5-1. 교집합 & 합집합 & 차집합

```python
#교집합
set_1 = set([1,2,3])
set_2 = set([3,4,5])
result = set_1&set_2 # or  set_1.intersection(set_2)
print(result) #{3}

#합집합
result2 = set_1 | set_2 # or set_1.union(set_2)
print(result2) #{1, 2, 3, 4, 5}

#차집합
result3 = set_1 - set_2 # or set_1.difference(set_2)
print(result3) #{1, 2}

```

### 5-2. 집합 자료형 관련 함수

**add(값 1개 추가)**

```python
set_1 = set([1,2,3])
set_1.add(5)
print(set_1) #{1,2,3,5}
```

**update(값 여러개 추가)** 

```python
set_1 = set([1,2,3])
set_1.update([31,32,34])
print(set_1) #{1,2,3,31,32,34}
```

**remove(특정값 제거)**

```python
set_1 = set([1,2,3])
set_1.remove(3)
print(set_1) #{1,2}
```

## 6. 문자열

- 문자열 변수에 `+` 를 사용시 문자열이 더해져서 표시
- `\` 백슬래시 사용으로 큰따옴표, 작은따옴표 사용 가능
- 리스트와 마찬가지로 인덱싱과 슬라이싱을 사용가능하지만 변경할 수 없다. (Immutable)

```python
str = "Hello world! \"python\" is good"
print(str) #Hello word! "python" is good

#인덱싱 사용 가능
print(str[0:4])

#변경불가
str[0] = 'T' # TypeError: 'str' object does not support item assignment

```

## 

## 7. 빠르게 입력받기

파이썬의 경우 빠르게 입력받아야 할 때 sys라이브러리에 `sys.stdin.readline()` 을 사용하용할 수 있습니다. 엔터를 누를시 줄바꿈 기호로 입력되므로 `rstrip()` 함수를 함께 사용하는걸 권장합니다.

```python
data = sys.stdin.readline().rstrip()
print(data)
```

## 8. f-string

파이썬 버전 3.6부터 사용 가능하며 자바스크립트의 template literals과 비슷한 기능으로 문자열과 변수를 함께 사용하는 문법을 말합니다.

```python
number = 5
print(f"평일은 {5}일입니다.")
```

##  9. 람다 표현식

어떤 기능을 하는 함수를 한줄에 작성할 수 있는 장점을 가지고 있습니다.

```python
def add(a, b): return a + b

print(add(2,5)) #7

#람다 표현식
print((lambda a, b: a + b)(2,5)) #7

#2번째 데이터 숫자로 정렬하기
array = [('soccer', 11), ('basketball', 5), ('boxing', 1)]
print(sorted(array, key=lambda x:x[1])) #[('boxing', 1), ('basketball', 5), ('soccer', 11)]

#여러 리스트 
list_1 = [1,3,5,7,9]
list_2 = [2,4,5,6,8]
print(list(map(lambda a, b: a + b, list_1, list_2)) #[3, 7, 10, 13, 17]

```

## 10. 표준 라이브러리

### 10-1. 내장함수

`sum(), min(), eval(), print(), input(), sorted()` 등의 기본 내장 라이브러리 함수를 말합니다.

### 10-2. itertools

반복형태의 데이터를 처리할 수 있도록 도와주는 라이브러리로 순열&조합 기능을 제공합니다.

### 10-3. heapq

힙 기능과 우선순위 큐 기능을 제공하는 라이브러리입니다.

### 10-4. bisect

이진 탐색 기능을 제공하는 라이브러리입니다.

### 10-5. collections

카운터, 덱등 자주 사용되는 자료구조를 포함하고 있는 라이브러리입니다.

## 11. 순열과 조합

순열 : 서로 다른 n개에서 서로 다른 r개를 선택해 나열하는 것을 말합니다. 쉽게 말해 주어진 데이터에서 선택한 데이터의 중복되지 않는 각 조합을 구하는 것을 말합니다.

```python
from itertools import permutations

data = ['E',"I","S","N"]

result = list(permutations(data,2))
print(result)
#'N'), ('S', 'E'), ('S', 'I'), ('S', 'N'), ('N', 'E'), ('N', 'I'), ('N', 'S')]
```

조합: 서로 다른 n개에서 순서 관계 없이 다른 데이터 r개를 선택하는 것을 말합니다.

```python
from itertools import combinations

data = ['E',"I"]

result = list(combinations(data,2))
print(result) #[('E', 'I')] permutations는 E,I / I,E
```

<br>

## References

- https://www.youtube.com/watch?v=m-9pAwq1o3w