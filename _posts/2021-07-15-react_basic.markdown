---
title:  "React Basic"
excerpt: "리액트 클래스 컴포넌트 기본"

categories:
  - React
tags:
  - [React]

date: 2021-07-15
last_modified_at: 2021-07-15
---

## Component

컴포넌트는 하나의 기능 구현을 위한 여러 종류의 코드 묶음  
  
패키지  
덩어리  
  
Root = 최상위 컴포넌트  

## render()

**render** 함수는 화면에서 보고자 하는 내용을 반환  
**React**는 설명을 전달받고 결과를 표시

```js
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}
```
함수 컴포넌트의 경우 별도의 렌더 없이 리턴으로 렌더링한다


ShoppingList => React 컴포넌트 클래스 또는 React 컴포넌트 타입  
개별 컴포넌트는 props 라는 매개변수를 받아오고  
render 함수를 통해 표시할 뷰 계층 구조를 반환  

### JSX 없이 구현하면
```jsx
return React.createElement('div', {className: 'shopping-list'},
  React.createElement('h1', /* ... h1 children ... */),
  React.createElement('ul', /* ... ul children ... */)
);
```

### prop 값 전달
```jsx
class Board extends React.Component {
  renderSquare(i) {
    return <Square value={i} />;
  }
}

class Square extends React.Component {
  render() {
    return (
      <button className="square" 
        onClick={() => console.log('click')}>
        {this.props.value}{/* this 는 button 엘리먼트를 가리킴 */}
      </button>
    );
  }
}
```
  
함수에 파라미터를 주려면  
  
```jsx
// ...
<button className="i_want_parameter" 
  onClick={(param) => plsGiveParamFunc(param)}>
</button>
```
  
### state
React 에서 컴포넌트는 무언가를 “기억하기”위해 **state**를 사용

React 컴포넌트는 생성자에 **this.state**를 설정하는 것으로 state를 가질 수 있다.  
**this.state**는 정의된 React 컴포넌트에 대해 비공개로 간주해야 한다.  

```jsx
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button className="square" onClick={() => alert('click')}>
        {this.props.value}
      </button>
    );
  }
}
```

js class 에서 하위 클래서의 생성자를 정의할때는 **super** 를 호출해야 한다.
React 컴포넌트 클래스는 생성자를 가질 때 **super(props)** 호출부터 작성해야한다.

React는 state 가 바뀌면 다시 렌더링
```jsx
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button
        className="square"
        onClick={() => this.setState({value: 'X'})}
      >
        {this.state.value}
      </button>
    );
  }
}
```
컴포넌트에서 setState를 호출하면 React는 자동으로 컴포넌트 내부의 자식 컴포넌트 역시 업데이트한다.

### props
props 는 부모에게 전달받은 무언가 -> 객체형태  
이름 바꿔서 줄 수 있음  
```jsx
<Test props={object} />
```
state 는 자기 상태

## SPA

**Single Page Application**  
전통적인 웹사이트는 페이지 단위로 바뀌는데  
중복되는 Header 나 Navigation 등과 같이 중복되는 요소들을  
계속 불러와야 해서 서버와 불필요한 트래픽이 발생한다  

SPA 는 페이지 전체를 로딩하지 않고 부분만 로딩한다  
필요한 데이터만 서버에서 전달받아 해당하는 부분만 업데이트 하는 방식으로  
불필요한 트래픽을 줄여준다  

#### 장점
- 전체 페이지가 아니라 필요한 부분의 데이터만 받아서 화면을 업데이트 하면 되기 때문에  
  사용자와의 Interaction 에 빠르게 반응
- 서버에서는 요청받은 데이터만 넘겨주기에 과부하 문제 감소
- 전체 페이지를 렌더링 하지 않기에 더 나은 유저경험 제공

#### 단점
- JS 파일의 크기가 커져서 첫 화면 로딩시간이 길다
- 검색 엔진 최적화가 좋지 않다.  
  HTML 파일이 비어서 자료 수집이 충분하지 않기때문

## Wireframe

Wireframe은 디자인에 들어가기 전 단계로 선(wire)를 이용해 윤곽선(frame)을 잡는 것