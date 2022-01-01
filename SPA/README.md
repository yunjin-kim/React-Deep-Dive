# SPA

SPA(Single Page Application)는 한 개의 페이지로 이루어진 애플리케이션이다

사용자가 다른 페이지로 이동할 때마다 새로운 리소스(html)를 요청하여 페이지를 로딩할 때마다
서버에서 리소스를 응답받아 해석한 뒤 화면에 출력한다. 서버 측에서 사용자에게 보이는 화면을
제공하기 위해 html을 미리 만들어 놓거나 데이터에 따라 유동적인 html을 생성하는 템플릿 엔진을
사용하기도 했다


현대 웹은 정보가 방대하여 새로운 화면으로 이동할 때마다 서버 측에서 모든 페이지를 준비한다면
성능 상 문제가 발생할 수 있다. 트래픽이 많이 오거나 서버에 부하가 걸릴 수 있다
속도 및 트래픽 측면에서 캐싱과 압축을 통해 서비스를 제공한다면 어느 정도 최적화 할 수 있지만
사용자와의 인터렉션이 자주 발생하는 모던 웹에서는 적당하지 않을 수 있게 된 것이다
화면 전환이 일어날 때마다 html을 새로 요청하면 사용자의 상태를 유지하는 것도 바뀔 이유가
없는 부분도 새로 불러오는 불필요한 로딩도 비효율적이다


리액트 같은 라이브러리를 사용하여 페이지 렌더링은 브라우저가 담당하고
애플리케이션을 실행시킨 후 사용자와의 인터렉션이 발생하면 필요한 부분만 업데이트 한다
새로운 데이터가 필요하면 서버 API를 호출하여 해당 데이터만 가져와 웹에서 사용할 수 있게 되었다


SPA의 경우 서버에서 사용자에게 제공하는 페이지는 하나지만 자바스크립트와 브라우저 주소
상태에 따라 다양한 화면을 보여줄 수 있다

다른 주소에서 다른 화면을 보여주는 것을 라우팅이라 한다
브라우저 API를 직접 사용하여 이를 관리하거나 라이브러리를 사용하여 이런 작업을 쉽게 구현할 수 있다


## SPA의 단점

페이지 로딩 시 사용자가 실제 방문하지 않을 수 있는 페이지의 스크립트도 불러오므로 너무 방대해진다
그러나 코드 스플리팅을 통해 라우터별로 파일을 나누어 트래픽과 로딩 속도를 계선할 수 있다

브라우저ㅔㅇ서 자바스크립트를 사용하여 라우팅을 관리하는 것은 일반 크롤러가 페이지 정보를
제대로 수집하지 못하는 잠재적인 단점이 존재한다
따라서 검색 엔진이 제대로 조회 되지 않을 수 있다
또한 자바스크립트가 로딩되는 짧은 시간에 흰 페이지가 나타날 수 있다

이러한 문제들은 서버 사이드 렌더링을 통해 해결할 수 있다


## 기본 사용법
```js
import React from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter } from 'react-router-dom';
import App from './App';

ReactDOM.render(
  <BrowserRouter>
    <React.StrictMode>
      <App />
    </React.StrictMode>
  </BrowserRouter>,
  document.getElementById('root')
);
```
- 웹 애플리케이션 HTML 5의 History API를 사용하여 페이지를 새로고침없이 주소를 변경할 수 있게 해준다
- 또한 현재 주소에 관련한 정보를 props로 쉽게 조회하거나 사용할 수 있게 해준다


### Route 

```js
import React from 'react';
import { Route } from 'react-router-dom';
import Home from './components/Home'
import Profile from './components/Profile';

const App = () => {
  return (
    <div>
      <Route path="/" component={Home} exact={true}></Route>
      <Route path="/profile" component={Profile}></Route>
    </div>
  )
}

export default App
```
- exact로 **/**경로 규칙과 **/profile**경로 규칙이 일치하여 두 컴포넌트가 같이 출력되는 문제를 해결할 수 있다


## Link

Link 컴포넌트는 클릭시 다른 주소로 이동시켜 주는 컴포넌트이다

a 태그는 페이지를 전환하는 과정에서 페이지를 새로 불러오므로 애플리케이션이 가진 상태를
모두 초기화시킨다. 그러나 Link 컴포넌트로 페이지를 전환하면 애플리케이션을 유지하면서
History API를 사용해 페이지 주소만 변경한다. Link 태그는 a 태그로 개발되었지만
페이 전환을 방지하는 기능이 내장되어 있다
```js
<Link to="주소"></Link>
```


## URL 파라미터와 쿼리

- 파라미터: **/profile/test**
- 쿼리: **/about?detail=true&foward=false**

파라미터는 특정 아이디 또는 이름을 조회할 때 사용
쿼리는 특정 키워드를 검색하거나 페이지에 필요한 옵션을 전달할 때 사용


### URL 파라미터

라우터로 사용하는 컴포넌트에서 전달 받는 **match**라는 객체 안에 params 값을 참조한다
match 객체 안에는 현재 컴포넌트가 어떤 경로 규칙에 의해 보이는지가 들어있다
App.js
```js
function App() {
  return(
    <>  
      <Route path="/:id" component={PostPage} />
    </>
  )
}

export default App;
```
PostPage.js
```js
export default function PostPage({ match }) {
  const { id } = match.params;
  console.log(match);
  // {path: '/:id', url: '/3', isExact: true, params: {…}}
  // isExact: true
  // params: {id: '3'}
  // path: "/:id"
  // url: "/3"

  return (
    <>
      <PostContainer postId={parseInt(id)} />
    </>
  )
}
```


### URL 쿼리

쿼리는 location 객체의 search 값에서 조회할 수 있다
location 객체는 라우트로 사용된 컴포넌트에게 props로 전달하며
웹 애플리케이션의 현재 주소에 대한 정보를 가지고 있다
**http://localhost:3000/home?detail=true&foward=false**주소 값
```js
{
  "pathname": "/home",
  "search": "?detail=true&foward=false",
  "hash": "",
}
```

URL 쿼리를 읽을 때는 위 객체의 search 값을 확인하는데 이 값은 문자열 형태이다
search 값에서 특정 값을 읽어오기 위해서 이 문자열을 객체 형태로 반환한다
보통 쿼리 문자열을 객체로 반환할 때는 **qs** 라이브러리를 사용한다
**npm i qs**
```js
import qs from 'qs';

const Home = ({ location }) => {
  const query = qs.parse(location.search, {
    ignoreQueryPrefix: true // 이 설정이 있어야 문자열에서 ? 를 생략한다
  });
  const queryDetail = query.detail === 'true';
  return (
    <>
      <h1>Hola</h1>
      { queryDetail && <p>detail=true</p> }
    </h1>
  )
}
```
쿼리의 결과 값은 항상 문자열이다


### 서브 라우트

라우트로 사용되는 컴포넌트 내부에 Route 컴포넌트를 또 사용하는 것이다
```js
const Home = () => {

  return (
    <>
      <h1>Home</h1>
      <Route path="/profile" exact render={() => <div>사용자 정보</div>} />
    </>
  )
}
```

component 대신 render라는 props를 넣어 주었는데 컴포넌트 자체가 아니라 보여주고 싶은 JSX를 넣어줄 수 있다
컴포넌트를 따로 분리하기 애매하거나 prop를 별도로 넣어주고 싶을 때도 사용한다


### 라우터 부가 기능


#### history

history 객체는 라우트로 사용된 컴포넌트에게 match, location 과 함께 전달되는 props 중 하나이다.
이 객체를 통하여, 우리가 컴포넌트 내에 구현하는 메소드에서 라우터에 직접 접근을 할 수 있다 - 뒤로가기, 특정 경로로 이동, 이탈 방지 등..

#### withRouter

withRouter HoC 는 라우트 컴포넌트가 아닌곳에서 match / location / history 를 사용해야 할 때 쓰면 된다
```js
import React from 'react';
import { withRouter } from 'react-router-dom';
const WithRouterSample = ({ location, match, history }) => {
  return (
    <>
      <h4>location</h4>
      <textarea value={JSON.stringify(location, null, 2)} readOnly />
      <h4>match</h4>
      <textarea value={JSON.stringify(match, null, 2)} readOnly />
      <button onClick={() => history.push('/')}>홈으로</button>
    </>
  );
};

export default withRouter(WithRouterSample);
```


#### Switch

Switch는 여러 Route 들을 감싸서 그 중 규칙이 일치하는 라우트 단 하나만을 렌더링 시켜준다
Switch를 사용하면 아무것도 일치하지 않았을 때 보여줄 **Not Found페이지** 를 구현할 수 있다
```js
      <Switch>
        <Route path="/" exact={true} component={Home} />
        <Route path="/about" component={About} />
        <Route
          // path 를 따로 정의하지 않으면 모든 상황에 렌더링됨
          render={({ location }) => (
            <div>
              <h2>이 페이지는 존재하지 않습니다:</h2>
              <p>{location.pathname}</p>
            </div>
          )}
        />
      </Switch>
```


#### NavLink

NavLink는 Link와 비슷한데 만약 현재 경로와 Link에서 사용하는 경로가 일치할 경우 특정 스타일 혹은
클래스를 적용할 수 있는 컴포넌트이다