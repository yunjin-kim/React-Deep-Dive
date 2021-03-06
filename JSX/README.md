# JSX

JSX 형식으로 작성한 코드는 브라우저가 실행되기 전에 코드가 번들링되는
과정 전에 Babel을 사용하여 자바스크립트로 변환한다

- 변환 전
```js
function App() {
  return (
    <div>
      Hello world
    </div>
  )
}
```

- 변환 후
```js
function App() {
  return React.createElement('div', null 'Hello world');
}
```

### JSX의 장점

자바스크립트로 요소를 계속 생성할 필요가 없어진다
```js
// 위 JSX문법 내용을 자바스크립트로 표현
const divFragment = documnet.createElement('div');
divFragment.innerHTML = 'Hello world';
```

컴포넌트의 태그를 HTML 태그처럼 사용할 수 있다
```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

### JSX 문법

컴포넌트에 여러개의 요소가 존재한다면 반드시 부모 요소 하나로 감싸야 한다
```js
import React from 'react';

functiom App() {
  return (
    <>
      <header>
        <h1>Hola</h1>
      </header>
      <body>
        <p>World</p>
      </body>
    </>
  )
}
```
이렇게 감싸는 이유는 Virtual DOM에서 컴포넌트 변화를 감지할 때 효율적으로 비교하도록
컴포넌트 내부는 하나의 DOM 트리 구조로 이루어져 한다는 규칙이 있기 때문이다


#### 조건부 연산자

JSX내부의 자바스크립트 표현식에서 if문 대신 삼항 연산자를 사용한다
```js
function App() {
  const name = 'Lee';

  return(
    <>
      <div>
        {
          name === 'Lee'
          ? (<p>Hola Lee</p>)
          : (<p>Who are you?</p>)
        } 
      </div>
    </>
  )
}
```

#### AND 연산자
```js
function App() {
  const name = 'Lee';

  return(
    <>
      <div>
        {name === 'Lee' && <p>Hola Lee</p>}
      </div>
    </>
  )
}
```
예외적으로 && 연산자로 조건부 렌더링할 때 falsy한 값 0은 화면에 나타난다

#### undefined 렌더링하지 않기
리액트 컴포넌트의 함수는 undefined만 반환하여 렌더링하는 상황을 만들면 안 된다
```js
function App() {
  const name = undefined;

  // X
  return name;

  // O
  return (
    <>
      {name || 'error'}
    </>
  )
}
```

#### 인라인 스타일링
```js
function App() {
  return (
    <> 
      <div style={{
                    backgroundColor: 'black',
                    color: 'white',
                    fontSize: '48px',
                    padding: 16 // 단위 생략시 px 자동 지정
                  }}>
      </div>
    </>
  )
}
```

#### class X classsName O
```js
function App() {
  return (
    <>
      <div className='addBtn'>
      </div>
    </>
  )
}
```