# React 그리고 Redux

React와 Redux를 적용하기 전 알아야할 부분을 간략히 정리한 내용입니다.

# 1. React

### 1-1. 설치

`npm install -g create-react-app`

### 1-2. 프로젝트 생성

`create-react-app`

### 1-3. 빌드 및 실행

- 실행 : `npm start` 
- 빌드 : `npm run build`

# 2. Redux

자바스크립트의 상태관리 라이브러리중 하나로 각각의 기능별로 분리시켜 효율적인 관리를 할 수 있습니다. 미들웨어라는 기능을 통해 비동기, 로깅등 확장성 있는 작업을 조금 더 쉽게 할 수 있기도 합니다.

리액트로 프로젝트 진행시 컴포넌트는 local state와 global state가 존재하게 됩니다.

local state는 각 컴포넌트가 가지는 state로 각각의 애플리케이션은 이 state를 기반으로 생성되며 global state는 애플리케이션 전체에서 state에 따라 변화하는 것을 들 수 있다. (Ex: 로그인에 따라 보여지는 화면, 권한별 보여지는 페이지등) 이때 state를 관리하기 위해 리덕스를 사용하면 훨씬 관리하는데 효율적입니다.

![ReduxDataFlowDiagram](https://user-images.githubusercontent.com/48043799/102045565-c0d31f00-3e1c-11eb-97b5-f2655b5b54cd.gif)

> redux가 영감을 받은 flux의 구조 

### 2-1. 폴더구조

**action**

애플리케이션에서 사용하는 명령과 함께 API 요청하는 작업과 같은 액션요소(함수)를 말합니다. 서비스에 따라 명령과 함수를 따로 모아두거나 도메인별로 구분하기도 합니다.

액션 메소드에서는 비지니스 로직을 개발하기때문에 코드 관리에 매우 중요한 역할을 합니다. 또한 리듀서(reducer)로 데이터 생성을 요청합니다.

**reducer**

보통 도메인별로 구분하며 액션 메소드에서 변경한 상태 요청을 받아 update하는 일을 합니다. 보통 index는 도메인별로 구분된 리듀서를 합쳐서 export 하는 역할을 합니다. 위에 적힌 action과 reducer를 기능별로 구분 지을 때 이를 module 이라 부르며 반대로 둘을 함께 사용하는 기법을 duck기법이라 말합니다.

**store**

현재 앱의 상태, 리듀서(reducer)가 들어가 있으며 리덕스(redux)에 한 애플리케이션당 하나의 스토어를 가집니다. 주로 미들웨어 설정하는 일을 하며 비동기 통신등을 사용할 때 redux-thunk, redux-saga등의 라이브러리를 추가하거나 디버깅툴을 설정하는 역할을 합니다.

**component**

리액트(react) 컴포넌트입니다. 각 컴포넌트는 보통 도메인별로 구분됩니다. 또한  컴포넌트는 크게 2가지로 나누어 관리합니다. 

컨테이너 컴포넌트는 여러 개의 프레젠테이션 컴포넌트로 구성되어 있습니다. `프레젠테이션`&`컨테이너`으로 구분되어 `데이터 또는 공통으로 관리하는 경우는 컨테이너`, `프레젠테이션은 각 기능별 UI 컴포넌트`를 뜻합니다.

프레젠테이션의 컴포넌트에선 비지니스 로직이 발생하지 않고 컨테이너 컴포넌트에서 개발해야하는데 이는 프레젠테이션 컴포넌트의 재활용성을 고려한 방법입니다.

`component: 프레젠테이션(화면 UI, 단순 보여주기 위한 화면 컴포넌트)`

`container: 컨테이너(리덕스와 연동하여 비지니스 로직에 관여하는 컴포넌트)`

### 2-2. 각 폴더 구조

**components**

**containers**

**modules**

- actions -> index.js (액션 정의)
- reducers -> 액션에 따른 상태 변경 객체를 반환하는 reducer 작성

**store**

<br>

### 2-3. react - redux

redux는 상태관리하는 전용 장소(store)에서 상태관리하고 컴포넌트는 그것을 보여주는 용도로만 사용

![스크린샷 2020-12-21 오후 2 04 05](https://user-images.githubusercontent.com/48043799/102741338-76f6b580-4395-11eb-9bee-d3ec24e74e4f.png)



Store에 접근하려면 action을 통해 접근 (이벤트와 같은 개념)

1. store에 무엇인가 하고 싶은 경우 action을 발행

2. store의 문지기가 action 발생을 감지시 state 갱신

3. action의 포맷은 아래와 같은 obj

   1. `{type: "action의 식별 문자열", payload: "액션 실행에 필요한 임의 데이터"}`
   2. 상수 - `export const ADD_VALUE = '@@myapp/ADD_VALUE';`
      함수 - `export const addValue = amount => ({type: ADD_VALUE, payload: amount});`

4. reducer는 이전 상태 action을 합쳐 new state를 만드는 조작

   1. **초기상태는 Reducer의 디폴트 인수에서 정의된다**

   2. 상태가 변할 때 전해진 state는 그 자체 값으로 대체되는 것이 아니라 새로운 것에 합성되는 개념으로 쓰인다.

   3. 

      ```
      import { ADD_VALUE } from './actions';
      export default (state = {value: 0}, action) => {
      	switch (action.type) {
      		case ADD_VALUE:
      			return {...state, value: state.value + action.payload};
      	}
      }
      ```



<br>

## References
- https://d2.naver.com/helloworld/1848131
- https://ko.redux.js.org/