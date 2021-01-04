# React DOM

### dangerouslySetInnerHTML 속성

react의 DOM 어트리뷰트엔 checked, className, dangerouslySetInnerHTML, htmlFor등 다양한 속성이 존재합니다.

브라우저 DOM에서 `innerHTML`을 사용하기 위한 React의 대체 방법으로 코드에서 HTML을 설정하는 것은 [사이트 간 스크립팅](https://ko.wikipedia.org/wiki/사이트_간_스크립팅) 공격에 쉽게 노출될 수 있기 때문에 위험합니다. 따라서 React에서 직접 HTML을 설정할 수는 있지만, 위험하다는 것을 상기시키기 위해 `dangerouslySetInnerHTML`을 작성하고 `__html` 키로 객체를 전달해야 합니다.

```react
function createMarkup() {
  return {__html: 'First &middot; Second'};
}

function MyComponent() {
  return <div dangerouslySetInnerHTML={createMarkup()} />;
}

```



## Reference

- https://ko.reactjs.org/docs/dom-elements.html