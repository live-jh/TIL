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



<br>

## References

- https://www.inflearn.com/course/ecmascript-6-flow/