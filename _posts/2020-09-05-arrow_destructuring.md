---
title: "화살표함수 && 구조분해"
categories:
  - 화살표
  - 함수
  - 구조분해
tags:
  - 화살표
  - 함수
  - 구조분해
---

1. 화살표 함수  

    기존의 함수 표현식은 function문 안에 넣는걸로 표현했다.  
    <br>
    ```
    const add = function (x, y) {
      return x + y
    }
    ```

    화살표 함수를 사용하면 function 키워드를 생략하고 화살표(=>)를 붙여 표현한다.  
    <br>
    ```
    const add = (x, y) => {
      return x + y
    }
    ```

    화살표 함수에서는 return문을 생략 가능하여 축약한 형태로도 사용이 가능하다.  
    <br>
    ```
    const add = (x, y) => x + y
    ```

    클로져 함수에 활용하면 몇 줄 안되게 표현이 가능하다.  
    <br>
    ```
    const adder = (x) => {
      return (y) => {
        return x + y
      }
    }
    ```
    <br>

        const adder = x => y => x + y


    이렇게 5줄짜리 코드가 1줄 코드로 변신이 되었다.  

    다만 화살표 함수는 call, apply, bind를 사용할 수 없고, this를 가지지 않는다.  

    <br>
    <br>

2. 구조분해(Destructuring Assignment)  

    배열이나 객체에 있는 값 추출을 쉽게 할 수 있다.  
    예시를 드는게 더욱 이해가 빠를 것 같다.  
    <br>
    ```
    const arr = [1, 2, 3]
    const [one, two, three] = arr // 기본 destructuring

    console.log(one)   // 1
    console.log(two)   // 2
    console.log(three) // 3

    const arr2 = ['a', 'b', 'c', 'd', 'e', 'f']
    const [a, b, c, ...rest] = arr2 // ...을 이용해서 나머지를 모두 배열 몰아넣습니다.

    console.log(a)     // 'a'
    console.log(b)     // 'b'
    console.log(c)     // 'c'
    console.log(rest)  // ['d', 'e', 'f']
    ```

    <br>
    
        const obj1 = {a: 1, b: 2, c: 3}
        const {c, b, a} = obj1 // key의 값을 가져오므로 순서는 상관없습니다.

        console.log(c) // 3
        console.log(b) // 2
        console.log(a) // 1

        const obj2 = {aa: 1, bb: 2, cc: 3, dd: 4, ee: 5}
        const {aa, bb, ...rest} = obj2 // ...을 이용해서 나머지를 모두 객체에 몰아넣습니다.

        console.log(aa)     // 1
        console.log(bb)     // 2
        console.log(rest)   // {cc: 3, dd: 4, ee: 5}
    

    배열은 순서가 없기에 argument 순서에 따라서 할당되는 값이 달라질 수 있지만, 객체는 key, value가 있으므로 순서를 다르게 적어도 그 key에 해당하는 값이 자동으로 할당되게 된다.  



