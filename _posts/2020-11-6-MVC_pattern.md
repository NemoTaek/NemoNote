---
title: "MVC 패턴"
tags:
  - MVC
  - Model
  - View
  - Controller
---

개발하면서 코드가 점점 길어지면 관리가 어려워진다는 것은 알고 있을것이다. 지금까지 위대하신 개발자님들 께서는 긴 코드를 관리하고자 여러가지 디자인 패턴을 만들어두셨다. 그중에서 오늘은 흔히 볼 수 있는 MVC 패턴에 대해서 알아보도록 하겠다.  

MVC 패턴은 Model-View-Controller로 구성되어있는 소프트웨어 디자인 패턴이다. 잘 사용하면 UI부분과 비즈니스 로직부분을 분리하여 어느 한쪽이 수정되어도 서로 영향을 받지 않는 어플리케이션을 만들 수 있다.  
그러면 이제 모델, 뷰, 컨트롤러가 어떠한 역할을 하는지 알아보자.  

일단 대략적으로 알기 위해 MVC패턴 다이어그램을 먼저 보고 가도록 하자.  
![MVC패턴 다이어그램](https://upload.wikimedia.org/wikipedia/commons/thumb/5/53/Router-MVC-DB.svg/300px-Router-MVC-DB.svg.png)  

먼저 흐름을 살펴보도록 하자.  
1. user가 UI를 맡고 있는 View에서 화면에 표시된 내용을 변경하게 되면(ex Submit 버튼을 누른다.), Controller가 사용자의 행동을 받아서 사용자의 요청 명령에 응답한다.  

2. Controller는 변경이 발생한 View에 해당하는 Model에게 어디가 바뀌었으니 데이터를 변경하라고 통지한다.  

3. Model의 데이터가 변경됐다면, Model은 데이터가 변경되었음을 Controller에게 알린다.  

4. Model의 변경통지를 받은 Controller는 최종 UI로 출력될 View를 결정하고, View한테 화면을 다시 그리라고 알려준다.  

5. Controller에게 통지받은 View는 화면을 다시 그리고 user는 새롭게 그려진 View를 보게 된다.  

그러면 이제 Model, View, Contriller 하나하나 알아보도록 하자.  

- Model  
  데이터를 가지고 있는 객체이며 모든 데이터와 상태에 대한 정보와 데이터 처리 관련 로직을 가지고 있다.(보통 DB를 주로 다루게 된다.)  
  그리고 Controller에서 Model의 상태를 조작하거나 가져오기 위한 인터페이스를 제공한다.  
  
- View  
  Model에 포함된 데이터의 시각화를 담당하는 UI이다.  
  데이터를 화면에 그리기만 할 뿐, 모델이 가지고있는 정보를 저장하지 않고, 행동에 대한 결정은 Controller에게 맡긴다.  
  
- Controller  
  어플리케이션의 비즈니스 로직이 구현되어 있다.  
  User가 View에서 어떤 행동을 했는지 해석하고, 그에따라 Model에게 어떤 데이터를 변경하라고 알려준다.  
  Model을 통해서 필요한 데이터를 가져와 응답을 구성한다.  
  보통 Model과 Controller의 다리 역할을 한다.  
  
MVC 패턴의 장점으로는 UI, 데이터 처리, 이 둘을 제어하는 컨트롤러가 서로 분리되어 각자 맡은 역할에만 집중하여 최대한 서로에게 영향을 주지 않게 한다.  
그래서 유지보수성, 확장성, 유연성이 증가하게 된다.  
하지만 단점으로는 어플리케이션의 크기가 커질수록 Controller에게 무게가 치우쳐지게 될 수 있기 때문에 스파게티 코드가 되지 않으려면 꾸준한 관리가 필요하다.  