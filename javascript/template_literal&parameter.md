## 1-1. template literal

자바스크립트에서 문자열을 입력하는 방식중 하나로 아래의 코드와 같이 str2변수의 문자열의 방식이 아닌 str변수처럼 사용할 수 있는 장점이 있습니다.

```javascript
console.log(`a
bb
ccc`);
// 'a
//  bb
//  ccc'

const a = 10;
const b = 20;
const str = `${a} + ${b} = ${ a + b }` ;
const str2 = a + ' + ' + b + ' = ' + (a+b);
console.log(str) //10 + 20 = 30
```

## 1-2. Detail

### 1) multi-line

```javascript
const func = function () {
  const str = `abc
  def
  ghij`
  console.log(str)
}
func()
```

### 2) ${ } 안에 값 또는 연산식이 가능

```javascript
const counter = {
  cur_num: 0,
  step: 1,
  count: function() { return this.cur_num += this.step },
  reset: function() { return this.cur_num = 0 }
}
console.log(`${counter.count()} ${counter.count()} //1 2
${counter.reset()} $${counter.count()} // 0 $1
${counter.count()}$`) //2$
```

## 2-1. map

호출한 배열의 모든 요소에 대해 실행된 함수 결과로 채워진 새로운 배열을 생성합니다.

```javascript
const arr_1 = [ 1, 2, 3 ]
const arr_2 = arr_1.map(function (v, i, arr) {
    console.log(v, i, arr, this)
    return this[0] + v
}, [ 10 ])
```

`callback`: `function (currentValue[, index[, originalArray]])`

- currentValue: 현재값
- index: 현재 인덱스
- originalArray: 원본 배열

## 2-2. reduce

```javascript
const arr_1 = [ 1, 2, 3 ]
const arr_2 = arr_1.reduce(function (v, i, arr) {
    console.log(v, i, arr); //10 1 0, 11 2 1, 13 3 2
  	return v + i;
}, 10)
```

`reduce(callback[, initialValue])`

- initialValue: 초기값, 생략시 첫번째 인자로 지정됩니다.
- callback: function (accumulator, currenValue, currentIndex[, originalArray]])
  - accumulator: 누적 계산값
  - currentValue: 현재 값
  - currentIndex: 현재 인덱스
  - originalArray: 원본 배열

## 3-1. rest parameter

어떤 함수를 호출했을때의 매개변수에서 지정하지 않은 나머지 파라미터를 말한다.

```javascript
function demo (...param3) {
	console.log(param3) //[1, 30, false, null, undefined, true]
}
demo(1, 30, false, null, undefined, true) 

function demo2 (i, j, ...param) {
  console.log(param) //[null, undefined, true]
}
demo2(-2, 10, null, undefined, true) 
```



<br>

## References

- https://www.inflearn.com/course/ecmascript-6-flow/