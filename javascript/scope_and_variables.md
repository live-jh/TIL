# javascript scope & variables



## 1. hoisting

변수의 선언 및 사용, 함수 실행등에 이어 호출 코드보다 후에 선언을 했어도 정상적으로 실행하도록 해주는 기능을 말합니다.

자바스크립트 ES6부터 let, const가 추가되며 변수 생명주기의 제어와 유연성을 위해 블록 레벨 스코프가 추가되었습니다.

## 2. scope

변수가 접근할 수 있는 범위

함수 레벨 스코프와 블록 레벨 스코프로 나뉘며 함수레벨 스코프는 함수 내부에서만 유효하며 내부에 선언한 변수는 지역변수, 외부에 선언한 변수는 모두 전역변수로 인식합니다.

블록 레벨 스코프는 블록 레벨 내에 범위에서 유효하며 코드 블록 외부엔 참조할 수 없습니다. 블록 내부에 선언된 변수는 지역변수로 인식합니다.

```javascript
{
  let a = 10 //지역변수
  {
    let a = 20
    console.log(a) //20
  }
  console.log(a) //10
}
console.log(a) // ReferenceError -> 전역 스코프에 a 선언시 값이 할당된다.

//block scope
// if, for, while, try/catch, switch
```

## 3. variables

### let, const

let은 재할당이 필요할 때 const는 상수일 때 사용하는 것이 좋습니다. (보통 const는 객체를 사용할 때 가장 유용합니다.)

```javascript
let funcs = []
for (let i = 0; i < 10; i++) {
  funcs.push(function () {
	  console.log(i)
  })
}
funcs.forEach((func) => {
  func()
})

//재할당 x
const PI = 3.141593
PI = 3.14 //SyntaxError

//참조타입 o
const OBJ = {
  prop1 : 1,
  prop2 : 2
}
OBJ.prop1 = 3
console.log(OBJ.prop1) // 3
```

## 4. TDZ

**Temporal Dead Zone**이란 뜻으로 변수선언은 되어있지만 (호이스팅으로 인해 코드 상단으로 끌어올려질 때) 할당이 되어있지 않을때 임시적으로 사용할 수 없는 구간이란 의미로 쓰입니다.

let과 const가 TDZ로 제약을 받는 경우는 var와 달리 undefined를 반환하지 않고 referenceError가 발생할 때입니다. 이는 let과 const가 hoisting이 되지 않는 것처럼 보이지만 실제로는 스코프 진입시 변수가 선언되어 호이스팅은 이뤄지고  실행시 변수의 위치에 도달할 수 없는 범위를 가지고 있기 때문에 에러가 발생한다는 점을 참고해야합니다.

<br>

## References

- https://poiemaweb.com/es6-block-scope
- https://www.inflearn.com/course/ecmascript-6-flow/