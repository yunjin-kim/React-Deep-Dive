# immer를 사용한 불변성 제어

immer를 사용하면 불변성을 유지할 수 있다
```js
import produce from 'immer';

const nextState = produce(originalState, draft => {
  // 바꾸고 싶은 값 바꾸기
  draft.somewhere.deep.inside = 5;
})
```

produce 함수는 두 개의 파라미터를 받는데 첫번째는 **수정하고 싶은 상태**,
두번째는 **상태를 어떻게 업데이트할 지 정의하는 함수**이다
두번째 파라미터로 전달되는 함수 내부에서 원하는 값을 변경하려면 produce 함수가 
불변성 유지를 대신해 주면서 새로운 상태를 생성한다

해당 라이브러리 핵심은 **불변성에 신경 쓰지 않는 것처럼 작성하지만 불변성이 관리되는 것**이다. 

```js
import produce from 'immer';

const originalState = {
  {
    id: 1,
    name: 'Hong',
    age: 33,
  },
  {
    id: 2,
    name: 'Park',
    age: 21,
  }
};

const nextState = produce(originalState, draft => {
  const person = draft.find(v => v.id === 2);

  draft.push({
    id: 3,
    name: 'Lee',
    age: 17,
  });

  draft.splice(draft.findIndex(v => v.id === 1), 1);
})
```
immer를 사용하여 컴포넌트 상태를 작성할 때는 객체 안에 있는 값을 직접 수정하거나
배열에 직접 변화를 일으키는 메서드를 사용해도 무방하다

## useState 함수형 업데이트와 immer 함꼐 쓰기
```js
const [number, setNumber] = useState(0);
const onIncrease = useCallback(() => setNumber(prevNumber => prevNumber + 1), [])
```
immer에서 제공하는 produce 함수를 호출할 때 첫번째 파라이멑가 함수 형태라면 업데이트 함수를 반환한다
```js
const update = produce(draft => {
  draft.value = 2;
});

const originalState = {
  value: 1,
  foo: 'bar',
}

const nextState = update(originalState);
```