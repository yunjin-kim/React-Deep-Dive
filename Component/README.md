# Component

리액트는 컴포넌트로 이루어져 있다

이 컴포넌트를 선언하는 방식은 두가지가 있다

### 함수형 컴포넌트
```js
function App() {
  return (
    <>
    </>
  )
}

const App = () => {
  return (
    <>
    </>
  )
}
```

### 클래스형 컴포넌트
```js
class App extends Component {
  render() {
    const name = "Lee";
    return <div>{name}</div>
  }
}
```

### props

properties의 약어로 컴포넌트 속성을 설정할 때 사용하는 요소다
- props 값은 해당 컴포넌트를 불러와 사용하는 부모 컴포넌트에서 사용할 수 있다
```js
// App.js
import Person from './Person';

function App() {
  return (
    <Person name="Hong" />
  )
}
```
```js
// Person.js
function Person(props) {
  return <div>{props.name}</div>
}

export default Person;
```
props는 객체 형태로 전달된다


#### defaultProps로 기본값 설정
props의 기본값을 설정하고 싶다면 컴포넌트에 defaultProps라는 값을 설정한다
```js
// App.js
import Person from './Person';

function App() {
  return (
    <Person name="Hong" age="22" />
  )
}
```
```js
// Person.js
function Person({ name, age }) {
  return (
    <>
      <div>{name}</div>
      <div>{age}</div>
    </>
  )
}

Person.defaultProps = {
  age: 33
}

export default Person;
```

#### 비구조화 할당 문법으로 props 내부 값 추출하기
```js
function Person() {
  const { name } = props;
  return (
    <>
      <p>{name}</p>
    </>
  )
}
```
```js
function Person({ name }) {
  return (
    <>
      <p>{name}</p>
    </>
  )
}
```


#### propTypes 로 props 검증하기
```js
import PropTypes from 'prop-types';

function Person({ name }) {
  return (
    <>
      <p>{name}</p>
    </>
  )
}

Person.propTypes = {
  name: PropTypes.string
}
```
name의 값은 반드시 문자열이어야 한다


## state

리액트에서의 state는 컴포넌트 내부에서 바뀔 수 있는 값이다

props는 컴포넌트가 사용되는 과정에서 **부모 컴포넌트가 설정하는 불변 값**이며, 컴포넌트 자신은 props를 읽기 전용으로만 사용한다
즉 부모 컴포넌트만이 props를 변경할 수 있는 것이다
부모 - 자식 컴포넌트 사이에서 데이터를 변경하고 전달하기 위해 사용하는 것이 가변값 state이다

state는 클래스형 컴포넌트가 지니고 있고, 함수형 컴포넌트는 useState를 지니고 있다


#### 클래스형 컴포넌트의 state

컴포넌트의 state는 constructor 메서드를 통해 설정하며 반드시 super(props)를 호출하고 객체여야만 하며, constructor가 존재하지 않으면 class의 멤버 변수로서 선언할 수 있다

this.setState에는 객체 대신 함수 인자를 전달할 수 있다
```js
this.setState((prevState, props) => ({
  number: prevState + 1
}))
```
화살표 함수에서 값을 반환하고 싶으면 코드 블럭을 생략하고
객체를 반환할 경우 소괄호로 감싸주면 **(prevState => ({number: prevState + 1}))**

this.setState가 끝난 뒤 특정 작업을 진행하고 싶으면 콜백으로 함수 인자를 넘겨준다
```js
this.setState({number: number + 1}, () => console.log('가잣'));
```

#### 함수형 컴포넌트의 state

Hook 중 useState 함수를 사용하여 state를 사용한다

useState는 배열 비구주화 할당 문법으로 표현한다

useState 함수의 인자에는 상태의 초기값을 넣어준다
useState를 호출하면 현재 상태를 바뀌주는 함수를 배열로 반환한다
이를 배열 비구조화 할당을 통해 정할 수 있다

- state 사용시 주의 사항
state 값이 바꿔야 할 때는 setState나 useState를 통해 전달 받은 setter 함수를 사용해야 한다

객체에 대한 복사본을 생성하여 그 값을 업데이트하고 복제본의 상태를 setState 혹은 세터 함수를 통해 변경한다
```js
// 객체
const object = { a: 1, b: 2 };
const copyObject = { ...object, b: 3 } // b값만 덮어쓰기

// 배열
const array = ['kim', 'lee'];
let copyArray = array.concat('hong');
```

props는 **부모 컴포넌트가 설정**한다
state는 **컴포넌트가 자체적으로 지난 값**으로 컴포넌트 내부에서 값을 업데이트 한다

props를 유동적으로 사용하기 위해서는
부모 컴포넌트의 state를 자식 컴포넌트의 props로 전달하여 자식 컴포넌트의 이벤트가 발생하면
부모 컴포넌트의 메서드를 호출하면 된다