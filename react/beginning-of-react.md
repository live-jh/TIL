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
const obj_2 = obj_1; //얕은 복사 
const obj_3 = JSON.parse(JSON.stringify(obj_1)) //새로운 객체 복사
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

//텍스트 문자열입니다.
//줄바꿈도 가능합니다. 2

```

## 배열 비구조화 (Array Destructuring)

```javascript
let [name] = ["tom", 10, "london"] // 갯수가 같지 않아도 에러 x
console.log(name)

let [, demo, ] = ['cc', 323, '10'] // 갯수가 같지 않아도 에러 x
console.log(demo)

let [nick, height , age, region] = ['tim', 187, 20]
console.log(region) //default -> undefined

let [nick, height , age, region = "default 변수 및 함수 선언 가능"] = ['tim', 187, 20]
console.log(region) //undefined
```

### 비구조화 할당

**구조 분해 할당** 구문은 배열이나 객체의 속성을 해체해 개별 변수에 담는 것을 말합니다.

```javascript
const live = {
		name:"없음",
		age: 30,
		region: 'earth'
}

const {age, region} = live; //객채에 필요한 값만 뽑아서 사용할 수 있고 없는 값을 변수로 선언시 undefined


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



