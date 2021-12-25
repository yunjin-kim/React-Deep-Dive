# HOOK

## useState

함수 컴포넌트는 this를 가질 수 없기 때문에 useState Hook을 직접 컴포넌트에 호출한다
```js
import React , { useState } from 'react';

function App() {
  const [count, setCount] = useState(0);
}
```
- useState를 호출하는 것은 무엇을 하는 것?
**state 변수**를 선언할 수 있다
useState는 클래스 컴포넌트의 this.state가 제공하는 기능과 똑같다
일반적으로 일반 변수는 함수가 끝날 때 사라지지만 state 변수는 React에 의해 사라지지 않는다

- useState의 인자로는 무엇을 넘겨야 하는가?
useState Hook의 인자로 넘겨주느 값은 state의 초기 값이다
함수 컴포넌트의 state는 클래스와 달리 객체일 필요는 없고 숫자 타입과 문자 타입을 가질 수 있다

- useState는 무엇을 반환하는가?
state 변수, 해당 변수를 갱신할 수 있는 함수, 이 두 쌍을 반환한다


## useEffect

리액트 컴포넌트가 렌더링 될 때마다 특정 작업을 수행하도록 설정할 수 있는 Hook이다

- useEffect가 하는 일은?
useEffect Hook을 이용해서 React에게 컴포넌트가 렌더링 이후 어떤 일을 수행해야하는 지를 말한다
React는 우리가 넘긴 함수를 기억했다가(이 함수를 effect라고 부른다) DOM 업데이트를 수행한 이후에 불러낸다

- useEffect를 컴포넌트 안에서 불러내는 이유?
useEffect를 캄포넌트 내부에 둠으로써 effect를 통해 state 변수 또는 prop 에 접근할 수 있게 된다
Hook은 자바스크립트 클로저를 이용해 문제를 해결한다

- useEffect는 렌더링 이후에 매번 수행되는가?
기본적으로 첫번째 렌더링과 이후 모든 업데이트에서 수행된다
effect를 렌더링 이후에 발생한다고 생각하면된다
React는 effect가 수행되는 시점에 이미 DOM이 업데이트되었음을 보장한다

#### 마운트 때만 실행하고 싶을 때
useEffect에서 함수를 컴포넌트가 화면에 맨 처음 렌더링될때만 실행되고 업데이트 될 떄는 실행되고 싶지 않으면 함수의 두번째 파라미터로 빈 배열을 넣어준다
```js
useEffect(() => {
  
}, [])
```

#### 특정 값이 업데이트 될 때만 실행하고 싶을 때
uesEffect의 두번째 파라미터로 전달되는 배열 안에 검사하고 싶은 배열을 넣어주면 특정 값이 변경될 때만 호출된다
```js
useEffect(() => {

}, [특정값])
```

#### clean-up 을 이용하는 Effects
clean-up이 필요없는 side effect도 있지만 clean-up이 필요한 effect도 있다
메모리 누수가 발생하지 않도록 clean-up하는 것이 매우 중요하다
```js
import React , { useState, useEffect } from 'react';

function FriendState(props) {

  useEffect(() => {

    return() => {
      // effect 이후 어떻게 clean-up 할 건지 표시
    }

  });

}

```
- effect에서 함수를 반환하는 이유?
이는 effect를 위한 clean-up 메커니즘이다
모든 effect는 clean-up을 위한 함수를 반환할 수 있다

- React가 effect를 clean-up하는 시점은 정확히 언제인가?
React는 캄포넌트가 마운트 해제될 때 clean-up을 실행한다
effect가 한번이 아니라 렌더링이 실행될 때마다 실행된다면 다음 차례의 effect를 실행하기 전에 이전 렌더링에서 파생된 effect를 clean-up하는 이유이다
이깃은 버그를 방지하고 성능 저하를 막는다