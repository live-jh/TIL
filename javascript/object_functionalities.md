## shothand property

프로퍼티의 key value 할당할 변수명이 동일할 시 value는 생략이 가능하다.

### ex-1

```javascript
let a = 30;
let b = -12;

let abc = {
  a,
  b
} // console.log(abc) -> {a:30, b:-12}
```

### ex-2 (파일명 구분하기)

```javascript
const convertExtension = function (fullFileName) {
  const fullFileNameArr = fullFileName.split('.')
  const filename = fullFileNameArr[0]
  const ext = fullFileNameArr[1] && fullFileNameArr[1] === 'png' ? 'jpg' : 'gif'
  return {
    filename,
    ext
  }
}
convertExtension('abc.png') //{filename: "abc", ext: "jpg"}
```

## destructuring assignment

```javascript
const {
	name,
	age
} = {
	name: "첼시",
	age: 115
}

console.log(name) //첼시
console.log(age) //115
```



<br>

## References

- https://www.inflearn.com/course/ecmascript-6-flow/lecture/12469?tab=curriculum