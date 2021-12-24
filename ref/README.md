# ref

특정 DOM 요소에 작업이 필요할 때 id, data- 어트리뷰트를 통해 자바스크립트에서 해당 요스를 직접 찾아 작업할 수 있다

ref는 DOM을 반드시 직접 제어해야할 때 사용한ㄷ
- 특정 input에 포커스
- 스크롤 박스 조작
- canvas 요소에 그림 그리기

## ref 사용

#### 콜백 함수를 통한 ref 설정

ref를 달고자 하는 요소에 ref라는 콜백 함수를 props로 전달한다

이 콜백 함수는 ref 값을 파라미터로 전달 받으며 함수 내부에서 파라미터로 받은 ref를 컴포넌트의 멤버 변수로 설정해준다
```js
<input ref={(ref) => {this.input = ref}} />
```
this.input 은 input 요소의 DOM을 가리킨다
ref의 이름은 자유롭게 지정할 수 있다


#### createRef를 통한 ref 설정

createRef를 사용하려면 컴포넌트 내부에서 멤버 변수로 React.createRef()를 달아주어야 한다
그 후 해당 멤버 변수를 ref로 지정할 요소에 props로 넘겨주면 ref 설정이 끝난다

```js
import React from 'reat';

class MyComponent extends Component {
  constructor(props) {
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />
  }
}
```