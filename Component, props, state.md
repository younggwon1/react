# Component, props, state

### Component

컴포넌트는 JavaScript 함수와 유사하다.

- 컴포넌트를 통해 UI를 재사용 가능한 개별적인 여러 조각으로 나눈다.
- 컴포넌트는 개념적으로 JavaScript 함수와 유사하다. **'props'라는 입력을 받은 후, 화면에 어떻게 표시되는지 기술하는 React 엘리먼트를 반환**한다.
- **엘리먼트**는 일반 객체(plain object)로 React 앱의 가장 작은 단위다. 엘리먼트는 **컴포넌트의 '구성 요소'**다.
- 컴포넌트를 선언하는 방식에는 **함수형 컴포넌트**와 **클래스형 컴포넌트**가 있다.



#### 컴포넌트 종류

#### 1. 함수 컴포넌트

````js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
````

- 함수형 컴포넌트는, props로 받을 객체 인자를 정의하고, JSX를 반환하는 것으로 기본 형태를 갖춘다.
- **함수형 컴포넌트에서는 return문에서 JSX를 반환**할 수 있다.
- 상태를 사용하지 않고 Life Cycle을 사용할 수도 없었다. 이를 보완하기 위해 Hooks를 추가시켰는데, 간단한 개념은 함수 컴포넌트에서도 React Life Cycle, State를 사용할 수 있게 한다는 것이다.



#### 2. 클래스 컴포넌트

````js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
````

- 클래스형 컴포넌트는 **<u>React.Component</u> 클래스를 상속**(이미 구현되어 있는 클래스에 기능을 추가함)받는 것으로 기본 형태를 갖춘다.
- 컴포넌트는 jsx를 반환해야하는데, 클래스는 return 문을 사용할 수 없으므로, **클래스형 컴포넌트에서는 JSX를 반환하기 위해 `render()` 함수를 사용**한다. 리액트는 클래스형 컴포넌트의 render() 함수를 자동으로 실행시킨다.
- React Life Cycle과 State를 사용할 수 있다.



---



<img width="795" alt="스크린샷 2021-07-30 오후 7 12 26" src="https://user-images.githubusercontent.com/73063032/127638396-8bf23a72-7b81-49f3-bf85-76ca4897abd7.png">



### Props

- **props는 속성을 나타내는 데이터**다.
- **props는 컴포넌트에서 컴포넌트로 전달하는 데이터**다. 컴포넌트의 속성으로, 해당 컴포넌트를 불러와 사용하는 <u>부모 컴포넌트에서만 설정 가능</u>하다.



#### 1. props 지정하기

````JS
<ComponentName prop1={propValue1} prop2={propValue2} ... />

# EX)
<Dog name="Ari" age={10} />
<Dog name="Bori" age={7} />
````

- 같은 타입의 컴포넌트에 다른 props 값을 주어, 패턴이 반복되는 형태로 컴포넌트의 효율적인 재사용이 가능
- props에는 불리언 값(true, false), 숫자, 배열과 같은 다양한 형태의 데이터를 담을 수 있다. 공백 구분으로 여러 개를 담는 것도 가능하다.
  - props에 있는 데이터는 문자열인 경우를 제외하면 모두 중괄호({})로 값을 감싸야 한다.



#### 2. props 받아 사용하기

- 💡 props는 **읽기 전용**이므로 **컴포넌트의 내부에서 props를 수정해서는 안 된다.** 입력값을 수정하지 않는 함수를 순수 함수라고 호칭하며, 모든 React 컴포넌트는 자신의 props를 다룰 때, 순수 함수처럼 동작해야한다.
  - 만약 수정이 필요하다 하면 자식 컴포넌트의 **<form />** 템플릿을 통해 값을 변경한다.

- **props를 받는 함수형 컴포넌트에 인자를 정의하면, props를 속성으로 가지는 객체 Object가 해당 인자로 전달**된다. 컴포넌트 내부에서 사용 시, 객체에 존재하는 값을 가져오듯 점 연산자(.)를 사용하여 원하는 props를 꺼내 쓸 수 있고, 이를 중괄호로 감싸 `{ [인자 이름].[props 이름] }` 형태로 사용한다.

````js
// 객체 인자를 통해 props 받아오기
function Dog(props) {
	return {
		<div>{props.name}</div>
		<div>{props.age}</div>
	}
}
````



- **propTypes**를 선언하여 컴포넌트가 받은 **props의 타입을 확인**하거나, **defaultProps** 프로퍼티를 할당하여 **props의 초기값을 정의**할 수 있다.

````js
// 객체 인자를 구조 분해 할당하여 props 받아오기
function Dog({ name, age }) {
	return {
		<div>{name}</div>
		<div>{age}</div>
	}
}

// 컴포넌트 props 초기값 지정
Dog.defaultProps = {,
	  name: '이름',
    age: 0,
}
// 컴포넌트 props 타입 확인
Dog.propsTypes = {,
	  name: PropTypes.string.isRequired,
    age: PropTypes.number,
}
````



- **클래스형 컴포넌트**에서 props를 사용할 때는 **`this.props`**로 불러와 사용한다. 클래스형 컴포넌트에서 propTypes나 defaultProps를 사용할 때는, 클래스 내부에서도 지정할 수 있다.

````js
// 클래스형 컴포넌트에서 props 사용하기
class Dog extends React.Component {
	static defaultProps = { ... };	// 컴포넌트 props 초기값 지정
	static propTypes = { ... };	// 컴포넌트 props 타입 확인
	render() {
		// 구조 분해 할당으로 props 사용
		const { name, age } = this.props;	
		return <div>{name}</div>;
    }
}
````



### State

- **state**는 **컴포넌트 내부의 동적 데이터**를 의미한다. props는 부모 컴포넌트가 설정하는 값으로 컴포넌트 자신은 props를 읽기 전용으로만 사용할 수 있다.
- state를 사용하는 방식에는 컴포넌트의 종류에 따라 2가지가 있다.
  - **클래스형 컴포넌트에서는 컴포넌트 자체가 state를 지니는 방식**으로 사용한다. 
  - **함수형 컴포넌트에서는 useState라는 함수, Hook을 통해 사용**한다.
- 여러 개의 자식으로부터 데이터를 모으거나 두 개의 자식 컴포넌트들이 서로 통신하게 하기 위해서 조상 컴포넌트에 state를 정의한다. 조상 컴포넌트가 props를 사용하여 자식 컴포넌트에 state를 다시 전달함으로 서로 동기화한다.
  두 컴포넌트 간 state 값을 동기화하는 일, state를 공유하는 일은, 그 state 값을 필요로 하는 컴포넌트 간의 가장 가까운 공통 조상 컴포넌트로 state를 끌어올림으로 가능하다. **👉 여러 개의 컴포넌트 간의 state를 동기화시키기보다, 공통 조상으로 끌어올려 하향식 데이터 흐름을 이용하자.**



#### 1. Class Component에서 state 사용하기

- 클래스형 컴포넌트에서 state를 사용할 때는 객체 형식의 `this.state`를 통해 state 객체의 초기값을 설정하고, 조회할 수 있다.
  - state 초기값은 보통 클래스의 생성자 함수인 Constructor 안에 정의한다.
  - **이때 props 객체를 인자로 하는 부모 클래스의 생성자를 먼저 호출한 다음 현재 컴포넌트(Component)의 state 초기값을 설정한다**.
- state 값을 변경할 경우, `this.setState`를 사용하여 새로운 값을 줄 수 있다.

````js
class ClassExample extends React.Component {
  constructor(props) {
    super(props);
    // state 초기값 설정
    this.state = {
      count: 0,
    };
  }

  render() {
    const { count } = this.state;		// state 조회
      return (
        <div>
          <p>You clicked {count} times</p>
          <button onClick={() => {
            this.setState({		// this.setState를 통해 state 업데이트
              count: count + 1
            });
          }}>
            Click me
          </button>
        </div>
      );
    }
  }
}
````



#### 2. Function Component에서 state 사용하기

- Hook이 React 버전 16.8에 새로 추가되면서, Function Component에서도 상태 값과 여러 React의 기능을 사용할 수 있게 되었다.



### **Props 와 State 정리**

#### Props

부모가 자식한테 넘겨주는 값으로 읽기전용

#### State

컴포넌트 자신이 가지고 있는 값으로 변경할 수 있음(setState() 함수를 사용하여)







참고

1. https://ko.reactjs.org/docs/components-and-props.html
2. https://velog.io/@soyi47/React-Component-props-state
3. https://lktprogrammer.tistory.com/118?category=744795
4. https://studyingych.tistory.com/52