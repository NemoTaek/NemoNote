---
title: "Async & await"
tags:
  - async
  - await
---

- async & await이란  
  javascript에서 비동기 처리 패턴 중 가장 최근에 나온 문법으로, 기존의 callback 함수와 promise의 단점을 보완하고 개발자가 읽기 좋은 코드를 작성 할 수 있게 해주는 패턴이다.  
  <br>

- async & await 기본 문법  
  함수 앞에 `async`라는 예약어를 붙여 사용한다. 그 후에 비동기 처리 코드 앞에 `await`을 붙이면 된다.  
  await은 promise가 fulfilled 상태나 rejected 상태가 될 때 까지 async 함수를 일시정지한다. promise가 await에 넘겨지면, await은 promise가 fulfilled 상태가 되고 해당 값을 리턴한다. fulfilled 상태가 되면 async 함수를 일시정지한 부분부터 다시 실행한다.  
  
  내가 적고도 무슨말인지 모르겠다. 이럴때는 역시 예시를 들면 좋을 것 같다.  
  
  ```
  function fetchItems() {
    return new Promise(function(resolve, reject) {
      var items = [1,2,3];
      resolve(items);
    });
  }
  
  async function logItems() {
    var resultItems = await fetchItems();
    console.log(resultItems); // [1,2,3]
  }
  ```
  
  먼저 fetchItems() 함수는 promise 객체를 반환하는 함수이다. fetchItems() 함수를 실행하면 promise가 fulfilled 상태가 되며 결과 값은 items 배열이 된다.  
  그리고 logItems() 함수를 실행하면 fetchItems() 함수의 결과 값인 items 배열이 resultItems 변수에 담기게 된다.  

  await를 사용하지 않았다면 데이터를 받아온 시점에 콘솔을 출력할 수 있게 콜백 함수나 .then()등을 사용해야 했을 것이다. 하지만 async await 문법 덕택에 비동기에 대한 사고를 하지 않아도 된다. 그냥 한마디로 await을 만나면 그 함수를 실행하고, 그 결과값이 나오면 그때 그 값을 변수에 담는다고 생각하면 될 것 같다.  
  <br>
- try & catch  
  async await에서도 오류에 대한 핸들링을 해야하는데, promise에서 catch() 메소드를 사용했던 것 처럼 여기서는 **try-catch** 문으로 해결한다.  
  
  ```
  async function logItems() {
    try {
      var resultItems = await fetchItems();
      console.log(resultItems);
    } catch(error) {
      console.log(error);
    }
  }
  ```
  
  async 함수 안에 실행할 내용을 try 안에 담고, 예기치 못한 오류가 났을 때를 대비하여 catch문을 사용하여 오류를 핸들링 해준다. 웬만한 오류들은 다 `error` 객체에 담기기 때문에 에러 유형에 따라 에러 코드를 처리해 주면 된다.  
