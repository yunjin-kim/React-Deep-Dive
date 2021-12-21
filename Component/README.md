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