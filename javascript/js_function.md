

## function arrow

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

### memo

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

## 요약

- `strict mode`가 아닌 경우 브라우저마다 다른 동작이 생기는 이슈가 있다.
  - ex: `strict mode`에서 함수선언문도 블록스코프에 갇히는 경우
- ES6 문법을 사용시 함수 선언문을 사용하지 않고 `arrow function` 사용하기
- 객체: 메소드 축약형
- 즉 function 키워드가 되도록 등장하지 않도록 코드 작성하기

<br>

## References
- https://www.inflearn.com/course/ecmascript-6-flow/lecture/12469?tab=curriculum