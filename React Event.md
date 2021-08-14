# React Event

> - React의 이벤트는 소문자 대신 **캐멀 케이스**를 사용한다.
> - **JSX**를 사용하여 문자열이 아닌 **함수로 이벤트 핸들러를 전달**한다.



### 주의할 점

**DOM 요소에만 이벤트 설정이 가능**하다. `div`, `button`, `input`, `form`, `span` 등의 **DOM 요소에는 이벤트 설정이 가능하지만, 리액트의 컴포넌트에는 불가능**하다. 

예를 들면 위와 같이 `button`이라는 DOM 요소에 이벤트를 설정했는데, 아래처럼 `<Component>`라는 리액트 컴포넌트에는 onClick을 달아서 우리가 의도한대로 이벤트가 실행되지 않는다. 그냥 color, name과 같은 **props를 전달해주는 것에 불과하다.**

````js
function App() {
  const sayHi = function () {
    alert('hello');
  }
  return (
    <>
      <Component onClick={sayHi} name="홍길동" nickname="홍홍" />
    </>
  );
}
````



### 이벤트 핸들러 네이밍

- **Props**의 경우 : 보통 `onClick`과 같이 `on`접두사를 지정한다.
- **Function Names**의 경우: 보통 `handleClick`과 같이 `handle`접두사를 지정한다.
  이 두 경우를 함께 사용한 결과 코드는 이런 패턴을 가질 것이다.
  `<Component onClick={handleClick}/>`

> 정리하면! `on`접두사가 붙었을때는 이 prop에 실제 이벤트가 연결되어 있으며, 
>
> `handle`접두사가 붙은건 이벤트가 발생했을 때 호출되는 실제 function을 의미한다고 생각하면 된다.



---



### HTML vs React

#### 1. 함수 호출 방법

HTML(기존 DOM element 에서 사용하는 방법)

````html
<button onclick="activateLasers()">
  Activate Lasers
</button>
````



React

1. `onClick` 와 같이 `camelCase`로 이벤트 이름을 설정 
2. event handler의 연결은 `JSX` 표기인 `{ }`을 사용

````html
<button onClick={activateLasers}>
  Activate Lasers
</button>
````



#### 2. 기본 동작 방지

> React에서는 `false`를 반환해도 기본 동작을 방지할 수 없다.
>
> 반드시 `preventDefault`를 명시적으로 호출해야 한다.

1. Html

````html
<form onsubmit="console.log('You clicked submit.'); return false">
  <button type="submit">Submit</button>
</form>
````



2. react

````js
function Form() {
  function handleSubmit(e) {
    e.preventDefault();
    console.log('You clicked submit.');
  }

  return (
    <form onSubmit={handleSubmit}>
      <button type="submit">Submit</button>
    </form>
  );
}
````



---



#### EX) `Toggle` 컴포넌트는 사용자가 “ON”과 “OFF” 상태를 토글 할 수 있는 버튼을 렌더링하는 예제.

````js
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // 콜백에서 `this`가 작동하려면 아래와 같이 바인딩 해주어야 합니다.
    // 여기서 this 는 Toggle 컴포넌트를 뜻한다.
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
````

> JavaScript에서 클래스 메서드는 기본적으로 [바인딩](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)되어 있지 않다. `this.handleClick`을 바인딩하지 않고 `onClick`에 전달하였다면, 함수가 실제 호출될 때 `this`는 `undefined`가 된다.
>
> 이는 React만의 특수한 동작이 아니며, [JavaScript에서 함수가 작동하는 방식](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/)의 일부이다. 일반적으로 **`onClick={this.handleClick}`과 같이 뒤에 `()`를 사용하지 않고 메서드를 참조할 경우, 해당 메서드를 바인딩 해야 한다.** [꼭 참고!! binding 관련 페이지](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=dilrong&logNo=221542329638)



#### 위의 예제 자세한 설명

여기에서 `constructor`에 `binding` 부분을 주목해보자

```js
constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
}
```

살펴보면, `this.handleClick` 함수에 `this`을 `bind` 하고 있다.

여기서 this 는 Toggle 컴포넌트를 뜻한다. 왜 this을 bind 해야 할까?

사실 내용은 간단하다. **JSX을 사용해서, this.handleClick을 연결했을때, JSX는 this을 bind 하지 않는다**.

따라서, 해들러 함수 내부에서, `this` 객체(컴포넌트 자기자신)를 사용하려면, 그전에 미리 `this`를 `bind` 해주어야 한다.



```js
handleClick() {
    // 여기서 this(Toggle)을 사용하려면, 반드시 this가 bind 되어 있어야 한다.!!!!!
    this.setState(prevState => ({
        isToggleOn: !prevState.isToggleOn
    }));
}
```



#### 만약 `bind`를 호출하는 것이 불편하다면, 다음의 2가지 방법이 있다.

1. 클래스 필드 사용

````js
class LoggingButton extends React.Component {
  // 이 문법은 `this`가 handleClick 내에서 바인딩되도록 합니다.
  // 주의: 이 문법은 *실험적인* 문법입니다.
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
````



2. 콜백에 [화살표 함수](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)를 사용

````js
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // 이 문법은 `this`가 handleClick 내에서 바인딩되도록 합니다.
    return (
      <button onClick={() => this.handleClick()}>
        Click me
      </button>
    );
  }
}
````

> => 이 문법의 문제점은 `LoggingButton`이 렌더링될 때마다 다른 콜백이 생성된다는 것이다. 대부분의 경우 문제가 되지 않으나, 콜백이 하위 컴포넌트에 props로서 전달된다면 그 컴포넌트들은 추가로 다시 렌더링을 수행할 수도 있다. 
>
> 이러한 종류의 성능 문제를 피하고자, **생성자 안에서 바인딩**하거나 **클래스 필드 문법**을 사용하는 것을 권장합니다.



---

##### 참고

**bind(this)로 하지 않아도 되는 경우**

- this를 이용해서해당 클래스를 참조할 필요가 없을 때
- ES6+ 클래스 대신 예전 방식인 React.createClass()를 사용할 때, 이때는 createClass()가 자동으로 바인딩한다.
- 화살표 함수(()=>{})를 사용할 때
- render()에서 같은 메서드를 한 번 이상 사용한다면 새성자에서 바인딩하여 중복을 줄 일 수 있다.

---

##### 미리 참고

### 함수형 컴포넌트

일단 함수형 컴포넌트의 상태값은 `useState` 훅으로 관리되기 때문에 컴포넌트의 `this`로부터 자유롭다. 

또한 함수형 컴포넌트 자체와 함수형 컴포넌트 안에서 선언한 함수들 모두 전역 객체를 `this`로 가지기 때문에 애초에 `this`가 다 같다. 

그래서 이벤트 핸들러에 콜백 함수를 넘기는 상황에도 딱히 신경 쓸 필요가 없다.

- `const` 키워드 + 함수 형태로 선언해야한다
- 요소에 적용하기 위해 `this`가 따로 필요하지 않다

```js
import React, {useState} from "react";

function NumberTest() {
  const [num, setNumber] = useState(0);

  const increase = () => {
    setNumber(num+1);
  }

  return (
    <div>
      <h1>Number Test</h1>
      <h2>{num}</h2>
      <button onClick={increase}>증가</button>
    </div>
  );
}

export default NumberTest;
```



---





---



### 이벤트 핸들러에 인자 전달하기

루프 내부에서는 이벤트 핸들러에 추가적인 매개변수를 전달하는 것이 일반적이다. 

예를 들어, `id`가 행의 ID일 경우 다음 코드가 모두 작동한다.

```html
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

위 두 줄은 동등하며 각각 [화살표 함수](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)와 [`Function.prototype.bind`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind)를 사용한다.

두 경우 모두 React 이벤트를 나타내는 `e` 인자가 ID 뒤에 두 번째 인자로 전달된다. 

화살표 함수를 사용하면 명시적으로 인자를 전달해야 하지만 `bind`를 사용할 경우 추가 인자가 자동으로 전달된다.











참고

1. https://ko.reactjs.org/docs/handling-events.html
2. https://gseok.gitbooks.io/react/content/bd80-bd84-bd80-bd84-c9c0-c2dd-b4e4/react-c5d0-c11c-event-handling-d558-b294-bc95-c815-b9ac.html
3. https://medium.com/@psm88732/%EB%A6%AC%EC%95%A1%ED%8A%B8-%EA%B5%90%EA%B3%BC%EC%84%9C-6%EC%9E%A5-react-%EC%97%90%EC%84%9C-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%8B%A4%EB%A3%A8%EA%B8%B0-8a8459f900d8
4. https://velog.io/@yoonvelog/React-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EC%B2%98%EB%A6%AC