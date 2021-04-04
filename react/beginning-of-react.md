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

### npm or yarn command

- default install : ./node_modules 디렉토리에 패키지 설치
  - `$ npm install [package_name]`
- --save : ./node_modules 디렉토리 패키지 설치 + /package.json dependencies에 추가
  - `$ npm install [package_name] -- save
- --save-dev : ./node_modules 디렉토리 패키지 설치 + /package.json Dev/dependencies에 추
  - `$ npm install [package_name] -- save-dev



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



## Babel과 Webpack

### Babel

바벨은 자바스크립트 컴파일러로 쉽게 말해 최신 사양의 자바스크립트코드를 각 브라우저의 버전별로 동작할 수 있도록 변환해주는 기능을 합니다. 리액트를 사용시 튜토리얼을 따라하다보면 babel을 사용하게 되지만 결국 자바스크립트를 기본으로 사용할 시엔 babel도 함께 사용하는 것이 바람직합니다.

### Webpack

웹팩은 자바스크립트 어플리케이션의 정적 모듈 번들을 말합니다. 복잡한 애플리케이션 서비스가 많아짐에 따라 자바스크립트 파일들의 규모 또한 커지게 되어 관리와 효율적 측면이 필요하게 되었습니다. 이로 인해 탄생한 것이 웹팩이고 간단히 다수의 자바스크립트 파일을 하나의 자바스크립트로 모듈(번들)화 하는 것을 말합니다. (모듈성, 네트워크 성능 향상)

### 특징

- 코드가 필요시 로딩
- Minifying: 불필요한 코드, 공백/줄바꿈, 긴 이름, 파일크기 줄이는 기능
- HMR: 개발모드에서 원본 소스 변경을 감지하여 변경 모듈만 즉시 생신

### 필요 유틸리티 라이브러리

- `$ npm install --global yarn`
- `$ yarn global add webpack webpack-cli`
  - 개발시 필요한 라이브러리 `$ yarn add --dev @bable/core bable-loader @babel/preset-env @babel/preset-react`
  - 프로덕션에 필요한 라이브러리 `$ yarn add react react-dom`

## create-react-app

react 프로젝트를 생성해주는 명령어로 webpack, babel, eslilnt 등 기본 설정으로 이루어진 리액트 프로젝트를 생성하는 기능을 수행합니다.

`$ yarn global add create-react-app`

`$ npm install -g create-react-app`

### 프로젝트 생성

`$ create-react-app <프로젝트 디렉토리>`

### 절대경로 지정 방법

- pycharm: src -> 마우스 우클릭 -> Mark Directory as -> Resource Root 선택
- jsconfig.json 파일 생성 후 `{"compilerOptions": {"baseUrl": "src"},"include": ["src"]}` 입력



## React Element

UI 데이터를 관리하는 방법 제공하고 화면을 담당합니다. React앱의 가장 작은 단위로 UI 데이터가 변경될때 해당 컴포넌트의 `render() ` 함수가 호출되어 화면을 자동 갱신하는 기능을 합니다.

- 부모 컴포넌트로부터 내려받는 속성 -> props
- 컴포넌트 내부에 생성되거나 관리하는 상태값 -> state

함수형, 클래스형 2가지로 나뉘며 **클래스형 컴포넌트**는 `render() 함수 호출`, **함수형 컴포넌트**에선 `해당 함수`가 매번 호출됩니다. (함수형의 경우 Hook으로 관리)

### React 개발의 핵심 (선언적 UI)

- UI에 노출되는 값들을 효율적으로 관리하고, 변경됨에 따라 필요한 UI만 변경되도록 하는 방법으로 개발하며 DOM에 직접 접근하여 추가, 변경, 삭제하는 것을 지양합니다.
- 직접 상태값을 변경하는 것은 성능 하락 이슈**가 있으며 때에 따라 강제 업데이트를 하는 경우외에는 잘 사용하지 않습니다.
- 상태값에 따라 화면이 불필요하게 업데이트 되지 않게 해야합니다.

```react
import React, {useState} from "react";
import 'App.css';

function Counter(props) {
    const [num, setNum] = useState({value: props.initValue});
    const {value} = num;
    return (
        <div>
            <button onClick={() => {
                setNum({...num, value: value + 1})
            }}>
                1씩 더하기
            </button>
            <button onClick={() => {
                if (value === 0) alert("0 이하로는 못내려요!")
                else setNum({...num, value: value - 1})
            }}>
                1씩 빼기
            </button>
            <div style={{marginTop: "10px"}}>
                <span>현재 숫자 : {value}</span>
            </div>
        </div>
    );
}

export default Counter;

```

### 문법 종류 

- JS 문법
  - `$ const reactElem = React.createElement('h1', null, "CCO CEO CTO")`
  - 번거롭고 선언해야할 것, 코드의 길이가 많아짐
- JSX 문법
  - JS문법의 확장, HTML과 비슷한 문법 사용 (babel을 통한 transpile이 필요)
  - class 선언시 -> className
  - 태그의 속성엔 `{}`  안에 식을 지정
  - `$ const reactElem = <h1>CCO CEO CTO</h1>;`



## React State

**Default State**

- 컴포넌트에서 사용되는 데이터들을 컴포넌트 안에서 생성 및 갱신하여 관리

**Redux, ContextAPI, Mobx State**

- 여러 컴포넌트에서 사용되는 데이터들을 별도 store 공간에서 생성 및 갱신하여 관리



## State Managed

**내부**

- **this.setState** or **useState** 사용 또는 ContextAPI 사용

**외부**

- 외부에 별도 상태값 저장소 관리
- 여러 컴포넌트의 공유될 정보(로그인, 유저정보)등을 dispatch 함수를 통해 관리 



## setState

- 클래스형 컴포넌트에선 setState는 유일한 상태값 **setter**
- 객체 또는 함수, 변경 및 기능처리가 끝났을때 호출되는 콜백함수
- 비동기로 동작
- **함수를 지정 가능**하고 매개변수로 **호출되기 직전의 상태값**을 받을 수 있다. (immer 라이브러리)
- 클래스형 컴포넌트 생성자에는 setState 호출은 무시
  - setState 자체가 컴포넌트가 마운드된 이후만 **유효**
  - 데이터를 가져오기 위해 API를 호출하고 응답을 바로 state에 적용할 때
    - 클래스형 **componentDidMount()** 사용
    - 함수형 **useEffect()** 사용

```react
onClick = () => {
  			this.setState({value: this.state.value + 1}) 
			  this.setState({value: this.state.value + 1}) // 값이 +1+1 총 2가 증가하는 게 아니라 1만 증가 -> 비동기처리
  
  
        this.setState(prevState => ({value: prevState.value + 1}))
        this.setState(prevState => ({value: prevState.value + 1})) // 이전 상태를 가져와서 +1을 해서 총 2 증가
        this.setState(prevState => (console.log(prevState.value)))
}
```



## State Reducer pattern

- 상태값의 직접적 변화를 주는 setter 함수를 제공하지 않음
- 하나의 함수에 임의 객체를 넘겨받아 해당 상태값에 반영
  - action 객체를 활용
- 해당 애플리케이션에서 공유할 상태값들을 한 곳에서 관리 (store)
- useReducer / ContextAPI 참고

```react
dispatch(action, state) { //prevState와 같이 직전 state가 default로 함께 넘어온다. 
    const {type, payload} = action; 
    if (type === "INCREMENT") {
        const { value } = payload;
        return {
            ...state,
            value: state.value + value,
        }
    } else {
        return state;
    }
}

const action = {
    type : "INCREMENT",
    payload: {value: 1},
};
    
dispatch(action)
```



## Props

컴포넌트 생성시 넘겨지는 값의 목록으로  **읽기 전용**으로 취급하고 **변경하지 않습니다**. 컴포넌트는 HOC(고차컴포넌트) 기법을 통해 Redux의 값이나 함수를 넘겨받을 수도 있습니다.

컴포넌트가 생성되는 초기 state에 건내받고이후 state로 관리

```react
// exam-1
getPostListLength() { //함수로 대응
	return this.props.post_list.length;
}

//exam-2
get postListLength() { //
	return this.props.post_list.length;
}

//exam-3
state = {
	postLength: 0
}
static getDerivedStateFromProps(props, state) { // render 호출 직전 상태값을 갱신하여 반영
	return {
		postLength: this.props.post_list.length
	};
}
```



## React Life Cycle

- Mounting 단계
  - constructor -> componentWillMount -> render -> componentDidMount
- Updating 단계 
  - props 변경시 : componentWillReceiveProps -> shouldComponentUpdate -> componentWillUpdate -> render -> componentDidUpdate
  - state 변경시 : shouldComponentUpdate -> componentWillUpdate -> render -> componentDidUpdate
  - 새로운 props 전달 받을 때
  - setState() 호출시
  - forceUpdate 호출시
- Unmounting 단계
  - componentWillUnmount (컴포넌트 제거시)

```react
componentDidUpdate(prevProps) {
    const {postId} = this.props;
    if (postId !== prevProps.postId) { // id 변경 감지
        //do something
    }
}
```

## 

## 속성값의 타입 명시 & 필수 설정

TypeScript 사용 또는 prop-types 패키지 통해 지정할 수 있습니다.

- `$ npm i prop-types`
- `$ yarn add prop-types`

```react
//클래스형
class Person extends Component {
    static propTypes = {
        name: PropTypes.string.isRequired, //문자, required
        age: PropTypes.number.isRequired, //int, required
    }

    static defaultProps = {
        name: "bill"
    };

    render() {
        const {name, age} = this.props;
        return (
            <div>
                <h1>Name: {name}</h1>
                <p>age: {age}</p>
            </div>
        )
    }
}

//함수형
function Person({name, age}) {
    return (
        <div>
            <h1>Name: {name}</h1>
            <p>age: {age}</p>
        </div>
    )
}

Person.defaultProps = {
    name: "bill"
}

Person.propTypes = {
    name: PropTypes.string.isRequired, //문자, required
    age: PropTypes.number.isRequired, //int, required
}
```



## recompose library (HOC)

- `$ yarn add recompose`

```react
import PropTypes from "prop-types";

function ThemedButton({theme, label, ...restProps}) {
    return (
        <button className={`btn btn-${theme}`} {...restProps}>
            {label}
        </button>
    )
}

ThemedButton.defaultProps = {
    theme: "default",
}

ThemedButton.propTypes = {
    theme: PropTypes.string,
    label: PropTypes.string.isRequired
}
```



## Event

컴포넌트상엔 여러가지 이벤트가 발생하며 해당 이벤트핸들러 속성명은 **camelCase** 로 작성하며 필히 함수로 지정합니다.

커스텀 리액트 컴포넌트에선 HTML 이벤트를 지원하지 않지만 내부 Element에서는 DOM 요소를 담아 핸들러 지정이 가능합니다.



## CSS

전통적으론 CSS를 별도 파일에 저장해 link 태그를 사용해 웹페이지에 노출하도록 하였지만 이는 include 처리시 전역 설정이 되어 컴포넌트간 충돌이나 중복 적용이 발생할 수 있는 점을 고려 **컴포넌트 관점에서 컴포넌트 내부에서 관리 및 적용**하는 방법을 사용합니다.

다른 컴포넌트에서 같은 className을 사용하다 보면 **한 페이지에 두 컴포넌트가 존재시 css가 겹쳐 나중에 import한 컴포넌트만 적용** 됩니다. 이를 방지하기 위해 사용하는 것이 **css-module**입니다.

### css-module

```react
import PostStyle from ".Post.module.css"
import CommentStyle from ".Comment.module.css"

const Post = () => <div className={PostStyle.wrapper}>Post</div>;
const Comment = () => <div className={CommentStyle.wrapper}>Post</div>;
```

css-module은 각 클래스명에 고유한 해시값이 적용되어 클래스명의 중복을 방지해주는 기능을 합니다.

### Sass

`$ yarn add --dev node-sass`

scss/sass에서 변수, 중첩, 임포트, 함수, 연산등을 지원하는 것을 활용하여 중복을 방지합니다.

```scss
$text-color: orange;
$background-color: blue; //변수 선언

.wrapper {
  color: lighten($text-color, 10%); //변수 활용
  background-color: lighten($background-color, 10%);
}
```



## 불변성(Immutable)

리액트는 불변성을 유지하며 상태값을 업데이트해야합니다. 다음과 같은 예제처럼 작성합니다.

```react
const new_state = {
	...todo,
	is_checked: true
}

// 레몬, 수박을 제거하고 메론을 추가
const fruits = ['사과', '레몬', '수박', '딸기']

//bad
fruits.splice(1,2,'메론')

//good
const new_fruits = {
		...fruits.slice(0, 1),
		'메론',
		...fruits.slice(3)
}

//immer
import {produce} from "immer";

const newFruits = produce(fruits, draft => {
  draft.splice(1, 2, '메론');
})

```

### immer

`$ yarn add immer`



### immer 를 사용한 TodoList

```react
import React, {Component} from "react";
import {Button, Col, Input, List} from "antd";
import Immer_test from "./Immer_test";
import {produce} from "immer";

class TodoList extends Component {
    state = {
        todoList: [],
        cur_value: '',
        holder_text: '할일을 입력해주세요.',
    }

    onChange = e => {
        this.setState({
            cur_value: e.target.value,
        })
    }
    onKeyDown = e => {
        if (e.keyCode === 13) {
            this.setState(prevState =>
                produce(prevState, draft => {
                    const current = draft.cur_value;
                    if (current.trim().length > 0) {
                        draft.cur_value = "";
                        draft.todoList.push(current);
                    } else {
                        draft.holder_text = "최소한 한글자 이상 입력해주세요 :)"
                    }
                })
            )
            console.log(this.state.todoList)
        }
    }

    deleteTodo = (index) => {
        console.log(typeof index)
        const {todoList} = this.state;
        const newTodoList = todoList.filter((item, i) => i !== index);
        this.setState({
            todoList: newTodoList
        })
    }

    render() {
        const {todoList, cur_value, holder_text} = this.state;
        return (
            <div style={{width: "500px", margin: "30px auto"}}>
                <Col>
                    <h1>TodoList</h1>
                </Col>
                <Col>
                    <List
                        dataSource={todoList}
                        bordered={true}
                        renderItem={(item, index) => (
                            <List.Item
                                actions={[<Button
                                    type="danger"
                                    size="small"
                                    onClick={() => this.deleteTodo(index)}
                                >
                                    delete
                                </Button>]}
                            >
                                <List.Item.Meta
                                    title={item}
                                />
                            </List.Item>
                        )}
                    />
                    <Col style={{marginTop: "10px"}}>
                        <Input type="text" onChange={this.onChange} onKeyDown={this.onKeyDown} value={cur_value}
                               placeholder={holder_text}/>
                    </Col>
                </Col>
                <Immer_test/>
            </div>

        )
    }
}

export default TodoList;
```



## 클래스 컴포넌트

- Render 단계
  - **순수 함수**로 구성
  - No Side Effects (API 호출, 쿠키, 로컬스토리지 접근 x)
  - DOM에 반영될 변경사항 확인
- Pre-Commit 단계
  - DOM 읽기 가능
- Commit 단계
  - DOM 접근 가능 
  - Side Effect 허용
  - 이벤트 리스너, 스케줄링 등록 가능



### Constructor

초기 속성값으로 상태값을 생성시 구현합니다. 초기 props에 대한 대응 용도일뿐 변경되는 props에 대한 부분은 반영하지 않습니다. 

setState는 mount 이후 유효하기 때문에 무시되며 외부 API 호출이 필요할 때는 `componentDidMount()` 에서 구현합니다.

### 속성값의 변화에 따라 외부 API 호출이 필요시

- 클래스 컴포넌트 `componentDidUpdate(prevProps)` 사용
- 함수형 컴포넌트 `useEffect Hook ` 사용

### 속성값을 계산하여 상태값 업데이트시

- 함수형 컴포넌트 `momoize 패키지 사용`
  - `import memoize from 'lodash/memoize';`

### Render()

화면에 보여질 내용 반환 (함수형 컴포넌트는 함수 자체 return)

- 반환 타입 : 리액트 컴포넌트, Array(Key 속성 필요), null/bool, 문자열, 숫자등 속성과 상태값으로 반환값 결정
- 조건부 렌더링 가능 : null or false 반환

###componentDidMount() 

- 이벤트 리스너 등록시 (함수형 컴포넌트에서는 didUpdate와 함께 useEffect 훅을 사용)
- 실행 이후 꼭 componentWillUnmount에서 이벤트 해제하기
- Render() 이후 한번 호출

### componentDidUpdate()

```react
componentDidUpdate(prevProps) {
		const { post } = this.props;
		if ( prevProps.post !== post ) {
				this.setCommentList(post);
		} 
}
```

### 클래스형 컴포넌트 작성 순서

1. propTypes 정의
2. state 초기
3. render를 제외한 생명주기 메소드 (componentDid ...)
4. 생명주기를 제외한 메소드
5. render 메소드
6. 컴포넌트 외부에서 사용하는 함수 및 변수



