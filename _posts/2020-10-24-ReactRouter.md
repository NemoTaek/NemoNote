---
title: "React Router"
tags:
  - React Router
  - switch
  - route
  - link
---

[React Router 예시](https://reactrouter.com/web/guides/quick-start)  

- React Router  
  Angular같은 것들은 프레임워크이기 때문에 route 기능이 기본적으로 들어가 있지만, React는 라이브러리이기 때문에 route 기능이 들어가있지 않다. 그래서 React를 사용하는 중에 라우팅이 필요할 때를 위해서 react-router가 만들어졌다.  
  react-router의 특징은 SPA(Single Page App) 기반으로 만들어져 페이지의 깜빡임 없이 레이아웃은 고정되어 있고, 변경이 필요한 부분만 변경되는 것이 특징이다.
  
  먼저 react-router을 사용하기 위해서 설치를 해야하기 때문에 다음을 터미널에 입력하여 설치한다.  
  `npm install react-router`
  추가로 react-router는 웹+앱을 모두 사용하고, 웹 것만 가져올 경우에는 react-router 대신 `react-router-dom`, 앱 것만 가져올 경우에는 `react-router-native`를 입력하면 된다.
  
  기존의 react 프로젝트에 맨 위의 예시에서처럼 react-router-dom에서 사용할 컴포넌트를 import 한다.  
  ```
  import {
    BrowserRouter,
    Switch,
    Route,
    Link
  } from "react-router-dom";
  ```
  
  그러면 각각의 컴포넌트에 대하여 알아보자.  
  
  1. BrowserRouter  
    react에서 라우팅을 할때는 BrowserRouter와 HashRouter 2가지 중 하나를 선택하여 라우팅을 할 수 있는데, 보통 BrowserRouter를 사용한다.  
    BrowserRouter는 HTML5의 history API를 활용하여 라우팅하고, HashRouter는 URL의 hash를 사용하여 라우팅한다.  
    
  2. Switch  
    path에 충돌이 일어나지 않게 Route를 관리한다.  
    Route가 여러개 존재할때 제일 처음에 매칭되는 Route만 선별하여 실행하기 때문에 충돌 오류를 방지해주며, Route간에 이동 시 발생할 수 있는 충돌도 방지해준다.  
    
  3. Route  
    요청받은 path에 해당하는 컴포넌트를 렌더링한다.  
    exact 속성을 넣어주면 정확하게 그 path가 되었을 때만 컴포넌트를 렌더링 하게 한다.  
    예를들어 path='/' 와 path='/about'이 있을때 react는 **'/'이 부분이 중복**된다고 생각하기 때문에 두개를 모두 렌더링하게 되는 문제가 발생한다. 그래서 `exact path='/about'`처럼 exact를 넣어주면 중복을 방지할 수 있다.
    
  4. Link  
    말 그대로 링크를 연결해준다.  
    하지만 a태그와는 살짝 다른데, a태그는 페이지 전체를 새로고침 하여 렌더링하지만, Link는 새로고침 하지 않고 필요한 부분만 다시 렌더링하여 더 효율적이라고 생각한다.  <br><br>
    
  react-router가 5.1.0버전이 되면서 몇가지의 hooks API가 포함되었다.  
  ```
  // <= v5.0
  function BlogPost({ match }) {
    const { slug } = match.params
    // ...
  }
  export default withRouter(BlogPost)
  ```
  기존에 match, location, history 객체에 접근하려면 `withRouter` HOC로 감싸줘야 했지만 이제는 `useParams`를 사용해도 된다.  
  
  ```
  import { useParams } from 'react-router'
  // >= v5.1
  export default function BlogPost() {
    const { slug } = useParams()
    // ...
  }
  ```
  1. useHistory  
    기존 인터넷에서 처럼 방문 기록을 가지고 있다고 생각하면 된다.  
    뒤로가기, 앞으로가기처럼 기능을 하는 몇가지 메소드를 알아보자.  
      - history.push(): 해당 path로 이동  
      - history.pop(): 이전 페이지로 이동  
      - history.go(n): n>0이면 n번만큼 앞으로 이동, n<0이동 n번만큼 뒤로 이동  
      - history.goBack(): 뒤로 가기  
      - history.goForward(): 앞으로 가기  

  2. useLocation  
    현재 페이지의 정보를 가지고 있다.
    
  3. useRouteMatch  
    match객체의 값에 접근할 수 있게 해주는 hook이다.  
    또한 인자로 Route 컴포넌트의 프로퍼티들(path, strict, sensitive, exact)을 가진 객체를 받을 수 있으며, 만약 path와 현재 페이지의 url이 일치할 경우 해당 path의 match객체를 반환하고 일치하지 않을 경우 null을 반환한다.  
    만약 아무 인자도 넘겨주지 않고 해당 hook을 호출하면 withRouter HOC로 match객체를 접근했을 때처럼 제일 가까운 부모 Route 컴포넌트의 match값을 리턴한다.  
