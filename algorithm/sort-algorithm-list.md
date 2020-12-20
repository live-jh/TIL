## 삽입정렬

각 숫자를 적절한 위치에 맞게 배치하는 정렬

```python
def insert_sort(arr):
	for i in range(1, len(arr)):
		j = i - 1
		key = arr[i]
		while arr[j] > key and j>=0:
			arr[j+1] = arr[j]
			j = j - 1
		arr[j+1] = key
	return arr

new_arr = insert_sort([1,10,3,6,4,8,9,5,2])
for i in new_arr:
  print(i)

```



## 선택정렬

가장 작은 수를 찾아 제일 앞으로 보내는 정렬

```python
arr = [1, 10, 5, 8, 7, 6, 4, 3, 2, 9]

def selected_sort(arr):
  n = len(arr)
  for i in range(n-1):
    min_index = i
    for j in range(i+1, n):
      if(arr[j] < arr[min_index]):
        min_index = j
    arr[i], arr[min_index] = arr[min_index], arr[i]
  return arr

selected_sort(arr)
print(arr)

```



## 버블정렬

인접한 두 수를 비교해서 작은걸 왼쪽으로 보내는 정렬

```python
def bubble_sort(arr):
	length = len(arr)-1
	for i in range(length):
		for j in range(length-i):
			if arr[j] > arr[j+1]:
				arr[j], arr[j+1] = arr[j+1], arr[j]
	return arr

new_arr = bubble_sort([1,10,3,6,4,8,9,5,2])

for i in new_arr:
  print(i)

```



삽입, 버블, 선택 정렬 모두 시간복잡도 `n2` 이며 주어진 데이터가 정렬이 거의 되어 있는 상태라면 삽입정렬이 가장 빠릅니다. 하지만 반대로 거의 정렬이 되어 있지 않으면 제일 느린 성능을 보입니다.
