# 리액트를 시작할 때 알아두면 좋을 개념 - 1

## 

## NodeJS 설치

`$ node --version` 및 [Node 버전 관리](https://futurecreator.github.io/2018/05/28/nodejs-npm-update-latest-or-stable-version/)

- 공식 사이트 설치
- 패키지 관리자 chocolatey를 통한 설치
  - `$ choco install nodejs-lts`
- Docker를 활용한 개발환경

### Package 관리자 yarn

yarn은 페이스북 주도로 개발된 패키지 관리자입니다. 설치 명령어 -> `$ npm install --global yarn` 

- `$ yarn (global) add 패키지명`
- Node는 파이썬과 다르게 default로 로컬에 설치되며 전역 설치에 대한 옵션은 `--global`
  - 파이썬은 default로 전역에 설치되며 로컬 설치를 위해 가상환경을 사용합니다.



## 상수/변수

var 대신 **const** 혹은 **let** 사용 -> block scope

- const: 재할당 불가, 내부 obj 속성 필드 값은 수정 가능
- let: 스코프는 함수 호출할 때가 아니라 **선언시 생성**되는데 함수를 선언하였을때 결정되는 스코프를 lexical variable scoping이라 하고 let은 이를 지원하는 변수 선언 문법입니다.

## 객체 복사

JavaScript는 Obj, Array에 대해 대입시 얕은 복사를 통해 복사됩니다.

```javascript
const obj_1 = {val: 10};
const obj_2 = obj_1; // 얕은 복사 
const obj_3 = JSON.parse(JSON.stringify(obj_1)) // 새로운 객체 복사
obj_2.val += 1;

console.log("obj_1", obj_1) // obj_1 { val: 11 }
console.log("obj_2", obj_2) // obj_2 { val: 11 }
console.log("obj_3", obj_3) // obj_3 { val: 10 }
```

## Template Literals

```javascript
const str = `텍스트 문자열입니다.
띄어쓰기도 가능합니다. ${1 + 1}`
console.log(str) 

// 텍스트 문자열입니다.
// 줄바꿈도 가능합니다. 2

```

## 배열 비구조화 (Array Destructuring)

```javascript
let [name] = ["tom", 10, "london"] // 갯수가 같지 않아도 에러 x
console.log(name)

let [, demo, ] = ['cc', 323, '10'] // 갯수가 같지 않아도 에러 x
console.log(demo)

let [nick, height , age, region] = ['tim', 187, 20]
console.log(region) // default -> undefined

let [nick, height , age, region = "default 변수 및 함수 선언 가능"] = ['tim', 187, 20]
console.log(region) // undefined
```

## 비구조화 할당 (Destructuring Assignment)

**구조 분해 할당** 구문은 배열이나 객체의 속성을 해체해 개별 변수에 담는 것을 말합니다.

```javascript
const live = {
		name:"없음",
		age: 30,
		region: 'earth'
}

const {age, region} = live; // 객채에 필요한 값만 뽑아서 사용할 수 있고 없는 값을 변수로 선언시 undefined


const demo_func_1 = (obj) => {
    console.log(obj.title)
}

const demo_func_2 = ({title}) => {
    console.log(title)
}


for (const person of people) {
    console.log(person.name);
    console.log(person.age);
}

for (const {name, age} of people) {
    console.log(name);
    console.log(age);
}
```



## 전개 연산자(Spread Operator)

배열이나 문자열과 같이 **반복 가능한 문자**를 0개 이상의 변수로 확장하여 배열의 값을 하나하나 넘기는 용도로 사용합니다.

```javascript
// 기존 es5
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const newArr = arr1.concat(arr2);
console.log(newArr);

// es6 spread operator
const arr3 = [1, 2, 3];
const arr4 = [4, 5, 6];
const newArr2 = [...arr3, ...arr4];
console.log(newArr2);


let person1 = {
    name: "steve",
    age: 1,
    region: 'busan'
}

let person2 = {
    ...person1,
    name: "bill"
}
console.log(person1) // { name: 'steve', age: 1, region: 'busan' }
console.log(person2) // { name: 'bill', age: 1, region: 'busan' } 필드명 중복시 마지막 값
```

리액트에서는 많은 값들을 **불변 객체**로 처리하며 이때 전개연산자를 사용하게 되는데 복잡한 구조일 경우는 `immer 라이브러리`를 사용합니다.



## Named Parameters (객체 비구조화 할당 활용)

```javascript
function print_func({name, age}) {
		console.log(name, age)
}

print_func({name: "james", age: 12}) // javascript
```

```python
def print_func(name, age):
		print(name, age)
		
print_func(name="james", age=12) # python
```

## 

## Arrow Function

return을 선언하지 않아도 계산된 함수의 값을 반환, 인자가 1개일 경우 소괄호 생략이 가능합니다.

```javascript
let paul = (name, age) => `${name}은 ${age}살`;
console.log(paul("paul", 10))

const plus_func = (x, y) => { // const plus_arrow_func = (x, y) => x+y; 같은 표현
  return x+y;
}
// this, args를 바인딩하지 않습니다. 
```

### 함수의 다양한 형태

```javascript
const sum1 = (x, y) => x + y;
const sum2 = (x, y) => {x, y};
const sum3 = (x, y) => ({x: x, y: y}); 
const sum4 = (x, y) => {x: x, y: y};
const sum5 = function(x, y) {
  return {x: x, y: y};
};
function sum6(x, y) {
  return {x: x, y: y};
}
```



## ES6 module

react를 사용할때 쓰는 모듈 시스템이며 IE를 포함한 구형 브라우저는 지원하지 않습니다.

문법은 `import " " from ... ` 으로 사용합니다.



## 고차함수(HOF)

함수를 인자로 전달받거나 리턴이 가능하고 다른 함수 자체를 조작하는 함수이며 함수 또는 클래스는 모두 객체이어야 합니다.

```javascript
// arrow function 및 고차함수 표기
const base_10 = fn => (x, y) => fn(x, y) + 10;
const mysum = (a, b) => a + b;

const return_fn = base_10(mysum); // 인자의 mysum은 mysum 함수가 아니라 base_10 내부의 wrap함수 1, 2는 wrap의 인자
console.log(return_fn(1, 2))
```



## 리액트의 함수형 프로그래밍

- 순수 함수로써의 기능을 유지한 채 코드 작성
  - **상태값**과 **속성값**이 같으면 항상 같은 값을 반환해야합니다.
  - 또 다른 side effects를 발생시키지 않아야 합니다.(저장, 쿠키 변경, http 요청등)
- 컴포넌트의 상태값은 불변 객체(Immutable)로 관리해야합니다.
  - 기존값 변경 x -> 같은 이름의 새로운 객체 반환

## 순수 함수(Pure Function)

```javascript
let person1 = {
    name: "steve",
    age: 1,
    is_checed: false
}

const pure_func = (person) => {
    return {
        ...person,
        is_checed: true
    }
}

console.log(pure_func(person1))

// 순수함수를 활용한 데이터 변환
// reduce, filter, map, join등
const numbers = [3,5,123]
const number = numbers.reduce((acc, n) => acc + n, 0); // accumulate 의 초기값 0
const even_numbers = numbers.filter(i => i % 2 == 0); // 짝수 찾기 배열 리턴
 
```

## 커링(Currying)

일부의 인자가 고정된 새로운 함수를 반환하는 기능의 함수를 만드는 기법입니다. 리액트에서는 HOF, Hook등 전반적으로 많이 사용됩니다.

```javascript
const userPost = username => message => {
		console.log(`${username} - ${message}`);
}
const post = userPost("livejh")
post("Hello")
```

