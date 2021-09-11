> 자바스크립트 기본기를 익히기 위한 학습 내용 핵심 정리입니다.

### 실행 컨텍스트(execution-context)

scope, this, hoisting, function closure등 동작 원리를 담고 있는 자바스크립트의 핵심 원리입니다. 실행 가능한 코드를 형상화하고 구분하는 추상적 개념이란 뜻으로 실행 가능한 코드의 필요한 환경의 의미를 담고 있습니다. 실행 가능한 코드는 **함수 코드**, **전역 코드**, **eval 코드**등이 있습니다.

실행 컨텍스트가 생성되면 JS엔진은 실행에 필요한 여러 정보를 담을 객체를 생성합니다. 이를 VO (Variable Object)라 부릅니다. 실행 컨텍스트의 실행은 아래와 같습니다.

- 실행 컨텍스트 스택에 추가
- 변수 객체(VO) 결정
- 스코프 체인 생성(전역 스코프 영역인 전역객체와 활성 객체를 순차적 바인딩)
- this에 바인딩할 객체 결정

### 클로저

정의한 함수가 선언되었을 때 자신을 포함하고 있는 외부함수보다 내부함수가 더 오래 유지되는 경우 (즉, 내부에 함수가 종료되었어도 값을 접근할 수 있는 스코프영역) 지역변수처럼 접근할 수 있는데 이런 함수를 클로저라고 부릅니다.



### function arrow

```javascript
let cur_date = function () {
	return new Date()
}

let cur_date = () => new Date();
// function 을 ()로
// return {}를 생략 가능, return이 아닌 것은 {} 생략 불가

let evt = function (x) {
  return {
    y: y
  }
}

let evt = x => ({ x })
//객체는 ()를 통해 현재 => 함수 블록이 아닌 객체라는 표기를 해준다.
```

- (매개변수) => { 내용 }
- 매개변수가 없을 시 ( ) 필수
- 본문에 return { 내용 } 만 있을 때 { } return 생략 가능
- return의 값이 객체일때 { } 를 ( ) 로 감싸기
- 매개변수가 하나일 경우 ( ) 생략 가능
- 실행컨텍스트 생성시 this 바인딩을 하지 않음

```javascript
const obj = {
  a: function () {
    console.log(this) // obj를 가리킴

    const b = () => {
      console.log(this)
    }
    const c = function () {
      console.log(this)
    }
    b() // obj를 가리킴
    c() // Window 객체
  }
}
obj.a()
```

### 요약

- `strict mode`가 아닌 경우 브라우저마다 다른 동작이 생기는 이슈가 있다.
  - ex: `strict mode`에서 함수선언문도 블록스코프에 갇히는 경우
- ES6 문법을 사용시 함수 선언문을 사용하지 않고 `arrow function` 사용하기
- 객체: 메소드 축약형
- 즉 function 키워드가 되도록 등장하지 않도록 코드 작성하기



### 평가

- 코드가 계산(evaluation) 되어 값을 생성하는 것
  - `ex) const num = 1+1;`

### 일급(first-class)

함수를 다른 변수와 동일하게 다루는 언어에서 사용하는 용어입니다. 생성, 대입, 연산, 인자, 리턴값으로 전달등의 프로그래밍 언어의 기본적 기능에 제한없이 사용할 수 있는 대상을 일컫습니다. 

일급 함수를 가진 언어에서는 함수를 다른 함수의 매개변수, 또는 변수로 활용 가능합니다. 

- 값을 다루는 것
- 변수에 값을 할당하는 것
- 함수의 인자 또는 결과로 사용하는 것
- 리턴값으로 사용하는 것

```javascript
    const a = 10;
    const add_50 = a => a + 50; // same (a) => { return a + 50 } 
    const init = add_50(a);
    log(init)

    const func_1 = () => { return () => { return 10 } };
    // const f1 = () => () => 10; // same = () => { return () => { return 10 } }
    log(func_1()()) // 10
    const tmp_func_1 = func_1()
    log(tmp_func_1)
    log(tmp_func_1()) // 원하는 시점에 평가(함수())해서 결과를 나타냄
```

### 고차함수

함수를 인자로 전달받거나 함수를 결과 그 자체로 반환하는 함수를 말합니다. 인자로 받은 함수를 필요한 시점에 호출 또는 클로저를 생성하여 반환합니다. 이는 자바스크립트의 함수는 일급 객체이기 때문에 값 그 자체 그대로 인자로 전달할 수 있는 특징에서 비롯됩니다.

고차함수는 크게 2가지 용도로 사용되어집니다.

- 함수를 인자로 받아서 실행
- 함수를 받아서 리턴

```javascript
// 클로저란 b => a + b 함수가 a를 계속 기억하고 있는 것을 의미
// const addMaker = a => b => a + b; // a를 받고 b를 받아 함수를 리턴함
const addMaker = (a) => {
  return (b) => { return a + b } //내부에서 함수를 생성해 리턴
};
const add10 = addMaker(10); //인자를 숫자값으로 전달
log(add10(5));
log(add10(10));
```



### Array

```javascript
log('Arr -----------');
const arr = [1, 2, 3];
let iter1 = arr[Symbol.iterator]();
log(iter1.next()) //{value: 1, done: false}
log(iter1.next()) //{value: 2, done: false}
log(iter1.next()) //{value: 3, done: false}
log(iter1.next()) //{value: undefined, done: true}
for (const a of iter1) log(a);
```

### Set

```javascript
const set = new Set([1, 2, 3]);
for (const a of set) log(a);
```

### Map(순회)

```javascript
const map = new Map([['a', 1], ['b', 2], ['c', 3]]);
let iter_map = map[Symbol.iterator]();
console.log(iter_map.next())
console.log(iter_map.next())
console.log(iter_map.next()) //value: (2) ["c", 3]

let it = map.values();
let iter_map_values = it[Symbol.iterator](); // value만 추출
console.log(iter_map_values.next()) // {value: 1, done: false}
console.log(iter_map_values.next())
console.log(iter_map_values.next())

for (const a of map) log(a); // same entries
for (const a of map.keys()) log(a); // key만 추출
for (const a of map.entries()) log(a);

// 기존 map
const demo_map = products.map(elem => elem.name)

// custom map
const map = (f, iter) => { //f -> 함수를 받아 함수내에서 조건 처리
  let res = []
  for (const it of iter) { // iterator이며 곧 => iter[Symbol.iterator]()
    res.push(f(it)); // 함수에게 값을 위임
  }
  return res;
}

```

### Map의 다형성

```javascript
const map = (f, iter) => { //f -> 함수를 받아 함수내에서 조건 처리
  let res = []
  for (const it of iter) { // iterator이며 곧 => iter[Symbol.iterator]()
    res.push(f(it)); // 함수에게 값을 위임
  }
  return res;
}

// 다형성 실제 querySelectorAll('*')이 map을 가지고 있지 않지만 custom을 통해 구현 가능
log(map(el => el.nodeName, document.querySelectorAll('*'))) //querySelectorAll('*')가 iterable 프로토콜을 따르는 것을 의미


const it = document.querySelectorAll('*')[Symbol.iterator]();
log(it.next());
log(it.next());
log(it.next());

//generator 함수
function* gen() {
  yield 1;
  if (false) yield 2;
  yield 3;
}
// iterable 프로토콜을 따르는 함수를 사용하는 것은 다른 많은 헬퍼 함수들과의 조합이 다양해지는 것을 의미한다.
log(map(e => e * e, gen()))
```



### Iterator

반복기 또는 반복자라는 의미로 시퀀스를 정희하고 종료시 반환값을 잠재적으로 정의하는 객체를 말합니다. **value, done을 반환하는 next() 메소드를 사용**해 객체의 iterator protocol을 구현할 수 있습니다. 시퀀스의 마지막 값이 이미 출력되었으면 done의 값은 true가 되며 value의 값이 done과 함께 존재한다면 반복의 반환값을 의미합니다.

또한 iterable & iterator protocol은 for..of, 전개 연산자등을 대상으로 사용할 수 있도록 정한 규약입니다.

### Iterable 객체

반복 가능한 객체, 즉 배열 객체를 말합니다. for...of 반복문을 적용할 수 있으며 배열은 Symbol.iterator() 메소드를 소유하고 있기에 **배열은 대표적 iterable에 속합니다.** 문자열 역시 iterable이라 말할 수 있으며 목록, 집합등 또한 for..of 문법을 적용할 수 있을때 iterable 객체라 표현합니다. 

잘 구현된 iterable은 iterator를 생성시 iterator를 진행하다 순회하기도 하고 객체 그대로 for문을 활용해 사용할 수 있어야 합니다.

```javascript
const iterable = {
  // iterable의 Symbol.iterator 
  [Symbol.iterator]() {
    let i = 3;
    return {
      // next method
      next() {
        // i가 마지막이면 done true
        return i === 0 ? { done: true } : { value: i--, done: false }
      },
      // 존재하지 않을 시 iterator is not iterable
      [Symbol.iterator]() { return this; } //자기 자신 return 즉 iterator 역시 Symbol.iterator를 가지고 있어야함
    }
  }
}
let iterator = iterable[Symbol.iterator]();
// log(iterator.next());
for (const a of iterator) log(a)

const arr = [1,2,3];
let iter = arr[Symbol.iterator]();
console.log(iter[Symbol.iterator]() === iter); // true iterator 실행값이 자기자신
```



### Filter (조건)

```javascript
// 중복 제거 또는 조건 걸기
const filter = (f, iter) => {
  let res = []
  for (const it of iter) {
    if (f(it)) res.push(it);
  }
	return res;
}

log(filter(p => p.price < 20000, products))

let under20000 = [];
for (const p of products) {
	if (p.price < 20000) under20000.push(p);
}
log([...under20000]); // filter(p => p.price < 20000, products)과 동일

let over20000 = [];
for (const p of products) {
	if (p.price > 15000) over20000.push(p);
}
log([...over20000]);
log(filter(p => p.price > 15000, products))

log(filter(n => n % 2, function* () {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
}())) // generator 즉시 실행 함수
//1, 3
```



### Reduce (모든 값을 구할 때 array가 아닌 하나의 값)

```javascript
const nums = [1, 2, 3, 4, 5];

// let total = 0;
// for (const n of nums) {
//     total = total + n;
// }
// log(total);

const add = (a, b) => a + b;
const reduce = (f, acc, iter) => {
  if (!iter) {
    iter = acc[Symbol.iterator](); //인자로 보낸 acc에 iterator로 할당
    acc = iter.next().value;       //iterator의 첫번째 요소를 acc로 할당
  }
  for (const it of iter) {
    acc = f(acc, it); // func 안에 2개 인자 보내기 (add)
  }
  return acc; //acc 누적값
}

log(reduce(add, nums)) // 도전
log(reduce(add, 0, nums)) // add(add(add(0, 1), 2), 3) 의미 
```



### 함수 중첩시 효율적인 go와 pipe 함수

```javascript
const go = (...args) => reduce((a, f) => f(a), args); // (f(a)[연산], args[iter])
go(
    add(0, 1),
    a => a + 1,
    a => a + 10,
    log
)

const pipe = (f, ...funcs) => (...as) => {
        log(...as) // func의 파라미터
        log(f) // pipe의 첫번째 인자
        log(funcs) // pipe의 2,3번째 인자
        return go(f(...as), ...funcs)
    };

const func = pipe(
        (a, b) => a + b, // go add(0, 1)과 같은 의미 (인자 2개 이상)
        a => a + 1,
        a => a + 10,
    )
log(func(0, 1))
```







<br>

## References
- https://www.inflearn.com/course/ecmascript-6-flow/lecture/12469?tab=curriculum