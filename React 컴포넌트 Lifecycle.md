# React 컴포넌트 Lifecycle

UI를 구성하기 위해서는 화면에 컴포넌트를 생성하고(**Mounting)**, 갱신하고(**Updating)**, 지워야(**Unmounting)** 한다.

컴포넌트는 각 프로세스가 진행될 때에 따라 Lifecycle 함수로 불리는 특별한 함수가 실행된다. 개발자는 이를 재정의하여 컴포넌트를 제어하게 된다.



<img width="687" alt="스크린샷 2021-07-30 오전 10 53 18" src="https://user-images.githubusercontent.com/73063032/127587973-e51d97f5-525f-4678-979c-7bbb2ad3e6f1.png">



<img width="660" alt="스크린샷 2021-07-30 오전 11 00 48" src="https://user-images.githubusercontent.com/73063032/127588572-3a4aa985-81c9-4df5-b7a9-5dd9951a0769.png">



<img width="764" alt="스크린샷 2021-07-30 오전 11 21 25" src="https://user-images.githubusercontent.com/73063032/127590196-b6488426-aac0-4b7c-9f00-895a4f42f766.png">



- **Mouting**: Creating 중인 `componentWillMount()`에서 Ajax 요청을 날리면 응답 시간만큼 컴포넌트를 그리는 것이 늦어짐을 알 수 있다. 따라서 일반적으로 `componentDidMount()`에서 Ajax 요청을 하는 게 낫다는 것을 알 수 있다.
- **Updating**: Receiving State 중에 `setState()` API를 호출하면 프로세스가 끝난 후 또다시 Receiving State 할 것을 알 수 있다. 따라서 `setState()` API를 해당 Lifecycle 함수에서 호출하면 개념적으로 무한 루프에 빠질 수밖에 없다는 것을 알 수 있다.

- **Will*** : 화면이 렌더링되기 전에 처리해야할 로직( ex) 데이터 호출(렌더링 되기 전 필요한 데이터) 해당 컴포넌트가 있어야하는지 showYN.. 등등  )

- **Did*** : 화면이 렌더링된 후 처리해야할 로직 (상태 변경 , 데이터 호출.. 등등)



**=> 직접 코드에서 선언하지 않아도 내부적으로 처리가 되도록 되어있기 때문에 사용을 안해도 무방. 하지만 필요한 경우라면 선언하여 사용!!**



### 메소드들의 역할

### 생성(Mount)

> DOM이 생성되고 웹 브라우저상에 나타나는 것

#### 1. constructor

````js
constructor(props){
    super(props);
    console.log("constructor");
}
````

> - 생성자 메소드로서 컴포넌트가 처음 만들어 질 때 실행된다. 이 메소드에서 기본 state 를 정할 수 있다.
> - super함수를 호출해야 React.Component클래스의 constructor메서드가 호출된다. super를 호출하지 않으면 컴포넌트가 제대로 동작하지 않는다.
> - **props는 상위 컴포넌트에서 전달해주는 속성값이다.**
>   **이 props를 이용해 <u>초기 상태값</u>을 만들 때 constructor가 유용하다.**



````js
class MyComponent() extends React.Component{
  constructor(props){
    super(props);
    this.state = {
      currentMovie : props.age < 10? '뽀로로':'어벤져스
    }
  }
}
````

> - 초기 속성값(props)로부터 age가 10미만이면 '뽀로로' 아닐경우 '어벤져스를 초기 상태값으로 설정한다.
> - **this.state** 처럼 직접 상태값을 설정하는 것은 **constructor에서만 허용**. **다른 곳에서는 setState를 통해 설정해야 컴포넌트가 자동으로 업데이트**됩니다.



#### 2. componentWillMount

````js
componentWillMount(){
    console.log("componentWillMount");
}
````

> - 컴포넌트가 DOM 위에 만들어지기 전에 실행된다.



#### 3. render

> - 컴포넌트 렌더링을 담당
> - render 메서드는 컴포넌트를 정의할 때 반드시 작성
> - 주의 사항
>   - render함수 내부에서 setState를 호출하면 안된다. (무한루프)
>   - render함수 반환값은 속성값과 상태값만으로 결정되어야 한다.
>   - 부수 효과를 발생시키면 안된다.



#### 4. componentDidMount

````js
componentDidMount(){
    console.log("componentDidMount");
}
````

> - 컴포넌트가 만들어지고 첫 렌더링을 다 마친 후 실행되는 메소드.
> - 이 안에서 다른 JavaScript 프레임워크를 연동하거나, setTimeout, setInterval 및 AJAX 처리 등을 넣는다.
> - componentDidMount()에서 돔 요소에 접근할 수 있다.
> - setState()도 마운트 후에 호출할 수 있기 때문에 componentDidMount()에서 비동기 통신을 수행하기 적합하다.



### 변경(Update)

#### 1. componentWillReceiveProps

````js
componentWillReceiveProps(nextProps){
    console.log("componentWillReceiveProps: " + JSON.stringify(nextProps));
}
````

> - 컴포넌트가 prop 을 새로 받았을 때 실행.
> - prop 에 따라 state 를 업데이트 해야 할 때 사용하면 유용하다. 이 안에서 `this.setState()` 를 해도 추가적으로 렌더링하지 않습니다.



#### 2. shouldComponentUpdate

````js
shouldComponentUpdate(nextProps, nextState){
    console.log("shouldComponentUpdate: " + JSON.stringify(nextProps) + " " + JSON.stringify(nextState));
    return true;
}
````

> - prop 혹은 state 가 변경 되었을 때, 리렌더링을 할지 말지 정하는 메소드.
>
> - 매개변수 nextProps,nextState로 각각 속성값과 상태값으로 컴포넌트가 업데이트를 할지 결정한다. true를 return하면 render메서드가 호출. 반대로 false를 반환하면 업데이트 단계는 중단.
>
> - 위 예제에선 무조건 true 를 반환 하도록 하였지만, 실제로 사용 할 때는 필요한 비교를 하고 값을 반환하도록 하시길 바람.
>
>   예: `return nextProps.id !== this.props.id;`
>
>   `JSON.stringify()` 를 쓰면 여러 field 를 편하게 비교 할 수 있다.



#### 3. componentWillUpdate

````js
componentWillUpdate(nextProps, nextState){
    console.log("componentWillUpdate: " + JSON.stringify(nextProps) + " " + JSON.stringify(nextState));
}
````

> - 컴포넌트가 업데이트 되기 전에 실행된다.
> - 이 메소드 안에서는 `this.setState()` 를 **사용하지 않기** – 무한루프에 빠진다.



#### 4. render

> - 컴포넌트 렌더링을 담당
> - render 메서드는 컴포넌트를 정의할 때 반드시 작성
> - 주의 사항
>   - render함수 내부에서 setState를 호출하면 안된다. (무한루프)
>   - render함수 반환값은 속성값과 상태값만으로 결정되어야 한다.
>   - 부수 효과를 발생시키면 안된다.



#### 5. componentDidUpdate

````js
componentDidUpdate(prevProps, prevState){
    console.log("componentDidUpdate: " + JSON.stringify(prevProps) + " " + JSON.stringify(prevState));
}
````

> - 컴포넌트가 리렌더링을 마친 후 실행



### 삭제(Unmount)

#### 1. componentWillUnmount

````js
componentWillUnmount(){
    console.log("componentWillUnmount");
}
````

> - 컴포넌트가 DOM 에서 사라진 후 실행되는 메소드













참고

1. https://ko.reactjs.org/docs/react-component.html
2. https://medium.com/little-big-programming/react%EC%9D%98-%EA%B8%B0%EB%B3%B8-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90-92c923011818
3. https://velopert.com/1130
4. https://velog.io/@sik2/React-%EB%9D%BC%EC%9D%B4%ED%94%84-%EC%82%AC%EC%9D%B4%ED%81%B4-API
5. https://velog.io/@kwonh/React-%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B01-%EC%B4%88%EA%B8%B0%ED%99%94%EB%8B%A8%EA%B3%84constructor-getDerivedStateFromProps-render-ComponentDidMount-Memoization-%EC%99%84%EC%A0%84%EC%A0%9C%EC%96%B4-%EC%99%84%EC%A0%84%EB%B9%84%EC%A0%9C%EC%96%B4-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8