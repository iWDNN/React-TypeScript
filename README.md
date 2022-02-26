# React + Typecript 
##  `styled-component`
### 1. 설치 
```bash
npm i styled-components
```
```js
  const Container = styled.div`
    display:flex;
    justify-content:center;
    align-item:center;
  `;
```
- extension: vscode-styled-components

### 2. extend, props styled-components
- props
```js
  const Box = styled.div`
    backgroundColor:${props=>props.bgColor}
    width:100px;
    height:100px;
  `
```

- extend
```js
  const Circle = styled(Box)`
    border-radius:50px;
  `
```
다른 컴포넌트의 스타일을 그대로 사용하면서 확장시키는 기능

### 3. 'As' and Attrs
- As
```js
  const Btn = styled.button`
    background-color:tomato;
  `
```
```html
  <Btn/> /* button tag */
  <Btn as="a"/> /* a tag */
```
  Btn의 스타일을 그대로 사용 하되 html의 다른 태그로 사용하고 싶은 경우 `as`사용

- Attrs
```js
  const Input = styled.input.attrs({required:true, type:"number"})`
    background-color:teal;
  `
```
```html
  <Input required type="number" />
```
html태그의 attrs 설정하게 만들어 주는 기능


### 4. Animation and PseudoSelectors
- animation
```js
import { keyframes } from 'styled-components';

const rotateAnimation = keyframes`
  0%{
    transfrom:rotate(0deg);
    border-radius:0px;
  }
  50%{
    border-radius:100px;
  }
  100%{
    transfrom:rotate(360deg);
    border-radius:0px;
  }
`
const Box = styled.div`
width:200px;
  height:200px;
  display:flex;
  justify-content:center;
  align-items:center;
  animation:${rotateAnimation} 1s linear;
`
```
keyframes를 통해 애니메이션 생성 가능

- PseudoSelectors
```html
  <Box>
    <span>😀</span>
  </Box>
```
```js
const Box = styled.div`
  display:flex;
  justify-content:center;
  align-items:center;
  animation:${rotateAnimation} 1s linear;
  span{
    font-size:36px;
    &:hover{
      font-size:40px;
    }
  }
`
```
컴포넌트 안에서 하위 요소의 스타일도 지정 가능

### 5. PseudoSelectors II

```js
  const Emoji = styled.span`
    font-size:36px;
  `;
  const Box = styled.div`
  display:flex;
  justify-content:center;
  align-items:center;
  animation:${rotateAnimation} 1s linear;
  ${Emoji}{
    &:hover{
      font-size:40px;
    }
  }
`
```

styled-component 이름을 선택하여 하위 컴포넌트 스타일 관리 가능

### 6. theme
- index.js
```js
import {ThemeProvider} from "styled-components";

const darkTheme = {
  textColor: "whitesmoke",
  backgroundColor: "#111"
}
const lightTheme = {
  textColor: "#111",
  backgroundColor: "whitesmoke"
}

ReactDOM.render(
  <React.StrictMode>
    <ThemeProvider theme={lightTheme}>
      <App />
    </ThemeProvider>
  </React.StrictMode>,
  document.getElementById('root')
);
```

- App.js
```js
const Wrapper = styled.div`
  background-color:${props=>props.theme.backgroundColor}
`

const Title = styled.h1`
  color:${props=>props.theme.textColor}
`
```
ThemeProvider를 통해 전역 색상을 관리하기 용이하다.
다크모드/라이트 모드, 테마를 만들때 편하다.

##  `TypeScript`

- javascript를 기반으로 한 언어, javascript의 모든 기능은 제공하면서 추가적인 기능 제공.
- 언어가 작동하기 전에 타입을 확인해줌(strongly typed)
- 장점 : javascript는 명시적인 설명 유형, 데이터에 대 한 설명을 제공하지 못하지만, typescript는 가능.

```js
const plus = (a,b) => a+b;
plus(1,"a");
// result : 1a
```

```ts
const plus = (a:number, b:number) => a+b;
plus(1,"a");
// TypeError!
```

## `React + Typescript`

### 설치
```bash
npx create-react-app my-app --template typescript

or

npm install --save typescript @types/node @types/react @types/react-dom @types/jest
```
### `Typing the props`
- 컴포넌트의 타입을 정해주기
```tsx
// App.tsx
function App() {
  return (
    <div>
      <Circle bgColor="teal" />
      <Circle bgColor="tomato" />
    </div>
  );
}

// Circle.tsx
interface ContainerProps{
  bgColor: string;
}
const Container = styled.div<ContainerProps>``;

interface CircleProps{
  bgColor: string;
}
function Circle({bgColor}: CircleProps){
  return <Container bgColor={bgColor}/>;
}
```
> interface: 객체모양을 타입스크립트에게 설명해주는 타입스크립트 개념

- 컴포넌트 자기 자신과 props를 interface를 사용하여 보호

### `Optional Props and Default Props`
- props를 옵션으로 필수나 선택할수 있게 만들어줌
```tsx
interface CircleProps{
  bgColor: string;
  borderColor?: string; // ===  borderColor: string | undefined;
}
```
- props 기본 값 설정해주기
```html
<Container borderColor={borderColor ?? "black"}>
// borderColor 값이 정해지지 않았다면 기본값은 black으로 설정 
```
```tsx
function Circle({bgColor,borderColor,text="default text"}) 
// 이런식으로도 기본값 설정 가능
```
- 높이가 일정한 모형을 만들어 내는데 props를 설정하지 않으면 기본값으로 정해놓은 높이의 모형을 만드는 것과 같은 경우

### `useState`
- Typescript가 초기값을 가지고 타입을 자동으로 추론
```ts
const [counter,setCounter] = useState(1);
setCounter("2"); //Error!
setCounter(true); //Error!
```
- 두가지 자료형 타입을 사용해야 할 경우
```ts
const [counter,setCounter] = useState<number|string>(1);
setCounter(2); 
setCounter("hello");
```

### `Form`
[참고링크](https://reactjs.org/docs/events.html)
- 폼이벤트에서는 처음에 알수없는 타입으로 제공한다. (구글링필수)
```ts
const onChange =  (event:React.FormEvent<HTMLInputElement>)=>{
  setValue(e.currentTarget.value);
}
```
- ES6 문법

```ts
// 둘은 동일한 의미
 const {currentTarget: {value}} = event; 

 const value = event.currentTarget.value;
// 여러 개를 가져 올때 유용함
const {currentTarget: {value,tagName,width,id}} = event; 

const value = event.currentTarget.value;
const tagName = event.currentTarget.value;
const width = event.currentTarget.value;
const id = event.currentTarget.value;
```

### `theme`
- styled.d.ts
  - 기본적으로 DefaultTheme의 인터페이스는 비어 있으므로 확장해야 한다.
```ts
import 'styled-components';

declare module 'styled-components' {
  export interface DefaultTheme {
    textColor: string;
    bgColor: string;
    btnColor: string;
  }
}
```
> declare : 변수, 상수, 함수 또는 클래스가 어딘가에 이미 선언되어 있음을 알린다.

> d.ts: 구현부가 아닌 선언부만을 작성하는 용도의 파일을 의미

[참고링크](https://it-eldorado.tistory.com/127)



## `crypto tracker app`
- setup
  - react-query
  - react-router-dom@5.3.0
  - typescript
  - styled-components

- api
  - 코인전체 : https://api.coinpaprika.com/v1/coins
  - 코인정보 : https://api.coinpaprika.com/v1/coins/{코인이름}
  - 코인가격 : https://api.coinpaprika.com/v1/tickers/{코인이름}

- Router.tsx
```tsx
function Router() {
  return <BrowserRouter>
    <Switch>
      <Route path="/:coinId">
        <Coin />
      </Route>
      <Route path="/">
        <Coins />
      </Route>
    </Switch>
  </BrowserRouter>
}
export default Router;
```
### `useParams`
- useParams훅을 사용하면 현재 파라미터 값을 객체로 내보낸다.
- 타입지정 : useParams<타입>

### `Reset.css`
- createGlobalStyle을 사용하면 렌더링 될 때, 이 컴포넌트는 전역 스코프에 스타일을 돌려줌.
```ts
const GlobalStyle = createGlobalStyle`// reset.css`;
```
### `()()`
- 즉시 실행되는 함수.  ex) (()=>console.log('hi'))();

### `Behind the scene`
- 화면 이동할 때 데이터를 보낸다는 것은 parameter를 통해 url에게 데이터를 요청하여 정보를 얻는다. 이러한 방식이 있는 반면, Link 컴포넌트를 통해 state를 사용하는 것이다. 항상 api 요청으로만 미리 보여주는 것이 아닌 홈 화면에서 받아왔던 작은 정보들을 미리 데이터로 출력한다.
```tsx
<Link to={{
  pathname: `/${coin.id}`,
  state: { name: coin.name },
}}>
```
- 이러한 방식의 경우 정보를 얻는 화면을 거쳐가지않고 바로 이동하는 경우 에러가 발생한다.
```tsx
<Title>{state?.name || "Loading..."}</Title>
// 이러한 경우 ||로 에러대신 출력할 말을 위와 같이 적어주도록 하자.
```

### `api request`
```ts
const response = await fetch('apiUrl')
const json = await response.json();
// 위와 동일
const response = await (
  fetch('apiUrl')
).json();
```

### `Data Types`
```js
Object.keys(data).join() //객체 키값
Object.values(data).map(v => typeof v).join() // 객체 자료형
```
- VScode shortcut 
> Shift+Alt(Option)+i : 선택한 문자열 가장 우측 끝으로 포커싱

> Ctrl(Command)+Shift+오른쪽 화살표: 현재 선택한 문자열을 기준으로 우측 끝까지 선택

[JSON 데이터 -> 타입스크립트 타입 사이트](https://app.quicktype.io/?l=ts)

### `nested route(중첩된 라우트)`

- 웹안에서 탭을 많이 사용할 경우, 스크린 안에 많은 섹션이 나뉘어진 곳에서도 유용한 라우트

[CSS Grid](https://studiomeal.com/archives/533)

### `useRouteMath`
- 현재 url과 일치하는지 알려주는 훅
```js
const priceMatch = useRouteMatch("/:coinId/price");
```
- 아래는 priceMatch의 값
```bash
isExact: true
params: {coinId: 'eth-ethereum'}
path: "/:coinId/price"
url: "/eth-ethereum/price"
```

### `React Query`
```bash
npm i react-query
```
- react 어플리케이션에서 서버 state를 fetching,caching,synchronizing,updating 할 수 있도록 도와주는 라이브러리
- global state를 건드리지 않고 react 어플리케이션에서 데이터를 가져오고 캐시하고, 업데이트 한다.