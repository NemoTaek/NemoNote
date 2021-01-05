---
title: "타입스크립트(TypeScript) 프로젝트 세팅"
tags:
  - TypeScript
---

지난 포스팅까지는 타입스크립트에 대한 개요 및 문법 정리였다.  
이번 시간에는 실전으로 들어가 리액트에 타입스크립트를 입힌 프로젝트를 만들어보도록 하겠다..

----------------------------------------------------------------

- 프로젝트 생성  
  다들 리액트 프로젝트 생성법 알죠? 모르신다구요? 그런분들을 위해서 한번 더 알려드릴게요  
  `npx create-react-app [프로젝트명]`
  하지만 지금은 타입스크립트를 입혀야 하니까 무언가를 더 추가해야겠죠? 해봅시다.  
  `npx create-react-app [프로젝트명] --template typescript`
  맨 뒤에 --typescript만 추가로 작성하면 된다. 그러면 나도 한번 해볼가?  
  
  어?? 나는 웨 않되??  
  1시간을 검색한 결과 이미 create-react-app이 글로벌로 깔려있으면 무슨 짓을 해도 타입스크립트가 적용이 안된다는 것을 알아내었다. 그러면 `npm uninstall -g create-react-app` ㄱㄱ  
  
  삭제했으니 다시 하면 되겠지? 제발...  
  오! 드디어 되었습니다! 짝짝짝  
  js인 파일들이 다 ts나 tsx로 생성이 된 것을 볼 수 있다.  
  
- App.tsx  
  가장 메인이 되는 App.tsx를 보도록 하자.  
  검색을 하면 app 함수를 표현하는 방법이 2가지가 있다.  
  `function App() {...}`
  또는
  `const App: React.FC = () => {...}`
  
  저는 생성되었을 때 function App으로 설정이 되었기 때문에 이로 진행해보도록 하겠습니다.  

먼저 인삿말을 건네는 코드를 작성해보자.  
```
// greet.tsx
type GreetingsProps = {
  name: string;
  mark: string;
};

function Greetings({ name, mark }: GreetingsProps) {
  return (
    <div>
      Hello, {name} {mark}
    </div>
  );
}

Greetings.defaultProps = {
  mark: '!'
};

export default Greetings;
```

그리고 App.tsx  
```
import Greetings from './component/greet';

function App() {
  return <Greetings name="React" />;
}
```

실행을 하면 `Hello, React !` 가 나오는 것을 볼 수 있을 것이다.  

그러면 간단한 버튼 클릭 이벤트를 추가해보자.  
먼저 greet.tsx을 다음과 같이 바꿔보자.  
```
type GreetingsProps = {
  name: string;
  mark: string;
  click: (name: string) => void;  // 추가
};

// props에 click 추가
function Greetings({ name, mark, click }: GreetingsProps) {
  return (
    <div>
      Hello, {name} {mark}
      <br></br>
      // 버튼 추가
      <button onClick={() => click(name)}>Click Me</button>
    </div>
  );
}
```
props로 click을 추가해줬습니다. 그리고 버튼을 만들어 onClick 이벤트를 추가하였습니다.  
App.tsx도 살짝 바꿔볼까요?  
```
function App() {
  const onClick = (name: string) => {
    console.log(`${name} says hello`);
  };
  return <Greetings name="React" click={onClick} />;
}
```
클릭 이벤트로 console.log를 찍도록 하였습니다. 이제 실행하고 버튼을 클릭하면 React says hello가 콘솔에 찍히는 것을 볼 수 있습니다.  

- 부록  
  `tsconfig.json`의 jsx에 계속 빨간줄이 그여서 거슬려서 검색을 해보았다.  
  `"jsx": "react"`로 바꾸면 된단다.  
  바꿨다.  
  npm start만 하면 다시 원래대로 react-jsx로 바뀌면서 빨간줄이 다시 생긴다.  
  화가 난다.  
  다시 검색했다.  
  include에 `"./src/**/*.ts"` 를 추가하란다.  
  무슨 외계어인지는 모르겠지만 일단 해보겠다.  
  오! 된다! 그런데 왜인지는 모르겠다.  
