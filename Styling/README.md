# 컴포넌트 스타일링

## 일반 css
```js
function App() {
  return (
    <>
      <div className="app-container">
        <p>App</p>
      </div>
    </>
  )
}
```


## Sass

Sass는 스타일 코드의 재활용성과 가독성을 향상시킨다

### utils 함수 분리
여러 파일에서 사용될 수 있는 Sass 변수 및 믹스인(Mixins)은 다르 파일로 분리하여 작성할 수 있다

- utils.scss
```js
// 변수
$red: #fa5252;
$orange: #fd7e14;
$yellow: #fcc419;
$green: #40c057;
$blue: #339af0;

// 믹스인(재활용되는 스타일 블록을 함수처럼 사용)
@mixin sqare($size) {
  $calculate: 32px * $size;
  width: $calculate;
  height: $calculate;
}
```
- 
```js
@import './styles/utils.scss';

.SassComponent {
  // ...
}
```

### sass-loader 설정 커스터마이징
프로젝트 구조가 깊어지면 해당 파일은 상위 폴더로 거슬러 올라간다
**@import '../../../utils.scss'**
이런 문제점은 Webpack에서 Sass를 처리하는 sass-loader의 설정을 커스터마이징하여 해결할 수 있다

create-react-app으로 설정한 프로젝트는 프로젝트 구조의 복잡성을 낮추기 위해 세부 설정이 모두 숨겨져 있는데, 이를 커스터마이징하기 위해 **eject** 명령어를 사용하여 세부 설정을 외부로 노출시켜야 한다. eject 명령어는 커밋되지 않은 변경사항이 존재하면
진행되지 않으므로 커밋을 완료한 후 실행해야 한다
```js
npm run eject
y
// or
yarn eject
react-scripts eject
y
```
config 라는 디렉토리가 생성되었다면 webpack.config.js를 수정한다
```js
{
  test: sassRegex,
  exclude: sassModuleRegex,
  use: getStyleLoaders(
    {
      importLoaders: 3,
      sourceMap: isEnvProduction
        ? shouldUseSourceMap
        : isEnvDevelopment,
    },
    'sass-loader'
  ),
  // Don't consider CSS imports dead code even if the
  // containing package claims to have no side effects.
  // Remove this when webpack adds a warning or an error for this.
  // See https://github.com/webpack/webpack/issues/6571
  sideEffects: true,
},
```
use: 의 'sass-loader'를 지우고 concat을 통해 커스터마이징된 sass-loader 넣어준다
```js
{
  test: sassRegex,
  exclude: sassModuleRegex,
  use: getStyleLoaders(
    {
      importLoaders: 3,
      sourceMap: isEnvProduction
        ? shouldUseSourceMap
        : isEnvDevelopment,
    },
    'sass-loader'
  ),
  // Don't consider CSS imports dead code even if the
  // containing package claims to have no side effects.
  // Remove this when webpack adds a warning or an error for this.
  // See https://github.com/webpack/webpack/issues/6571
  sideEffects: true,
},
```
이제 utils.scss 파일을 불러올 때 해당 scss 파일이 어디에 위치해도 상대 경로를 입력할 필요 없이 **styles 디렉토리 기준** 절대 경로를 사용해 불러올 수 있다
```js
@import 'utils.scss';
```

### node_modules에서 라이브러리 불러오기
물결 문자를 사용하면 자동으로 node_modules의 라이브러리 디렉토리를 탐지해 스타일을 불러올 수 있다
```js
@import '~include-media/dist/include-media' // 라이브러리 불러오기
```
node_modules 내부 라이브러리 경로 안의 scss파일을 불러와야 하므로 직접 경로를 확인해야 한다


## CSS Module

css를 import하여 사용할 때 클래스 이름을 고유한 값인 **[파일이름]_[클래스이름]_[해시값]** 형태로 자동 변환하여 컴포넌트 스타일 클래시 이름이 중복되는 현상을 방지하는 기술이다

- CSSModule.module.css
```js
/* 고유한 값으로 자동 변환되므로 아무 단어나 클래스 이름으로 설정해도 됩니다. */

.wrapper {
  background: black;
  padding:1rem;
  color:white;
  font-size: 2rem;
}

/* 글로벌 CSS 작성 시 */
:global .something {
  padding: 0;
  margin: 0;
}
```
```js
import React from 'react';
import styles from './CSSModule.module.css';

const CSSModule = () => {
  return (
    <div className={styles.wrapper}>
      <span className="someting">Hola</span>
    </div>
  );
};

export default CSSModule;
```
- CSS Module이 적용된 스타일 파일을 불러오면 객체를 전달받고, CSS Modeule에서 사용한 클래스 이름과 해당 이름을 고유화한 값이 Key-Value쌍으로 들어 있는데
styles 를 console로 출력하면 다음과 같다
```js
{ "wrapper": "CSSModule_wrapper__26PNO" }
```
- 고유화된 값을 사용하고 싶다면 **className={styles.클래스 이름}**형태로 전달한다
- **:global**을 적용한 전역 클래스는 기존 클래스를 사용하듯 **className="something"**으로 넣어주면 된다

여러 스타일을 적용하고 싶을 때 두가지 방법이 있다
- 템플릿 문자열
```js
<div className={`${styles.wrapper} ${styles.inverted}`}>
```
- Array 내장 메서드
```js
<div className={[styles.wrapper, styles.inverted].join(' ')}>
```


## postCss