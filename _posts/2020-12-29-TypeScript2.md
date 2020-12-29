---
title: "타입스크립트(TypeScript) 타입"
tags:
  - TypeScript
---

타입 스크립트이니까 타입이 가장 중요하겠죠? 지금부터 타입에 대하여 알아봅시다.  

### 기본 타입

- Boolean: 익히 알고 있듯이 true, false 값이다.  
  `let isDone: boolean = false;`

- Number: 일반적인 숫자는 number로 타입을 지정한다. BigInteger 타입은 bigint로 지정한다.  
  그리고 타입스크립트에서는 ECMAScript 2015에서 지원하는 binary, hex, octal 리터럴도 지원한다.  

  ```
  let decimal: number = 6;
  let hex: number = 0xf00d;
  let binary: number = 0b1010;
  let octal: number = 0o744;
  let big: bigint = 100n;
  ```
  <br>
- String: 문자열이다.  
  `let color: string = "blue"`
  
- Array: 배열은 2가지 방식으로 표현할 수 있다.  
  1. 타입명 뒤에 '[]'를 붙여 표현  
    - `let list: number[] = [1, 2, 3];`  
  2. generic type을 사용  
    - Java에서 사용하는 문법인데, 일반적인 코드를 작성하고 이 코드를 다양한 타입의 객체에 대하여 **재사용**하는 프로그래밍 기법이다.  
    - 그래서 배열의 generic type을 사용한다는 의미로 Array<>를 작성하고, 그 안에 타입을 적어 사용한다.  
    - `let list: Array<number> = [1, 2, 3];`  
    
  <br>
- Tuple: 알고 있는 타입의 고정된 수의 요소를 가진 배열로 표현한다. 그리고 튜플 안의 타입이 모두 같을 필요는 없다.  

  ```
  // Declare a tuple type
  let x: [string, number];
  
  // Initialize it
  x = ["hello", 10]; // OK
  
  // Initialize it incorrectly
  x = [10, "hello"]; // Error
  Type 'number' is not assignable to type 'string'.
  Type 'string' is not assignable to type 'number'.
  ```
  <br>
- Enum: 흔히 열거형이라고 한다. 값들을 한 묶음으로 묶고 이름을 지정하여 표현한다. default 값은 0이며, 값은 차례대로 1씩 증가한다.  

  ```
  enum Color {
    Red, 
    Green, 
    Blue,
  }
  // enum의 이름을 Color라고 지었으므로, 타입도 Color가 되겠다.
  let c: Color = Color.Green;
  ```
  
  그래서 c값을 찍어보면 "Green"이 나오는지, 1이 나오는지 아직은 잘 모르겠다. 한번 console.log로 찍어봐야겠다.  
  TypeSciprt 컴파일러를 사용해서 js로 변환하는 과정을 거쳐야 한다. 이를 **트랜스파일링**이라 하나보다.  
  파일명을 example.ts로 저장하고, js로 트랜스파일링 해보자.  
  `tsc example.ts`  
  
  그랬더니 다음과 같이 js로 변환되어 나왔다.  
  
  ```
  // example.js
  var Color;
  (function (Color) {
    Color[Color["Red"] = 0] = "Red";
    Color[Color["Green"] = 1] = "Green";
    Color[Color["Blue"] = 2] = "Blue";
  })(Color || (Color = {}));
  var c = Color.Green;
  console.log(c);
  ```
  
  실행시켜보니 1이 나온다. 하지만 이렇게 매번 변환해서 실행하기는 귀찮기도 하고 번거로운 일일 것 같다.  
  이런사람들을 위하여 ts-node라는 npm 패키지가 있다. 다음 커맨드로 설치하자.  
  `npm install -g ts-node`
  그리고 `ts-node example.ts` 를 입력하여 실행해보자.  
  오!! 똑같이 1이 나온다. 역시 개발자는 귀찮아야 발전하나보다...
  
  tsc로 변환된 js파일을 보니 enum 값에 따로 값을 지정할 수 있는 모양이다.  
  
  ```
  enum Color {
    Red = 1, 
    Green, 
    Blue,
  }
  
  let c: Color = Color.Green; // 2
  ```
  
  ```
  enum Color {
    Red = 1, 
    Green = 3, 
    Blue,
  }
  
  let c: Color = Color.Blue; // 4
  let colorName: string = Color[4]; // "Blue"
  ```
  
  이렇게 값을 지정하면 다음값은 앞의 값에 따라서 자동으로 증가되어 지정되는 것을 볼 수 있다.  
  그리고 만약 변수(?)를 보고싶다면 enum Color에서 찾고싶은 값을 입력하여 볼 수 있다.  
  
- Unknwon: 타입을 지정하고 싶은데, 아직 어떤 타입인지 모를 때 사용한다.  
  일단 unknown으로 타입을 정의하고, 나중에 어떤 값으로 초기화를 해주면 그 타입에 맞게 알아서 변경된다.  
  
  ```
  let notSure: unknown;
  notSure = false;
  typeof(notSure) // boolean
  ```
  
  하지만 이미 어떤 타입으로 지정된 변수에 unknown 타입의 변수를 할당하면 오류를 뱉어낸다.  
  ```
  let notSure: unknown;
  let aNumber: number = notSure;
  // Type 'unknown' is not assignable to type 'number'
  ```
  <br>
- any: unknown처럼 타입을 모를 시 사용한다.  
  unknown과의 차이점은 위처럼 타입이 지정된 변수에 any 타입의 변수를 할당해도 오류를 뱉지 않는다.(메타몽같은걸..?)  
  
  ```
  let notSure: any;
  let aNumber: number = notSure; // OK
  ```
  <br>
- void: any 타입의 반대 개념이다. 보통은 값을 리턴하지 않는 함수의 타입으로 사용된다.  

  ```
  function warnUser(): void {
    console.log("This is my warning message");
  }
  ```
<br>
- null, undefined: 이것도 익히 알고 있는 null과 undefined이다.  

- never: void와 약간 비슷한 개념인데, 절대로 일어날 수 없는 값에 대한 타입이다.  
  그래서 함수를 실행하는데 throw처럼 오류가 발생하는 상황에 사용된다.  
  
  ```
  function error(message: string): never {
    throw new Error(message);
  }
  ```
  <br>
- object: number, string, boolean, bigint, symbol, null, undefined 와는 다르게 원시타입이 아닌 객체 타입이다.  

- symbol: ESE6에서 새롭게 추가된 7번째 타입으로 변경 불가능한 원시타입이다.  
  symbol 값은 Symbol 생성자를 사용해서 만든다.  

  ```
  // symbol 선언 방법
  let mySymbol = Symbol();
  console.log(mySymbol); // Symbol()
  console.log(typeof(mySymbol)); // symbol
  ```
  
  symbol은 서로 값이 유니크하다.  
  
  ```
  let sym1 = Symbol("key");
  let sym2 = Symbol("key");
  sym1 === sym2; // false
  ```
  <br>
- type assertions: 타입 단언이라고도 하며, 컴파일러에게 "나만 믿어, 내가 알아서 할게" 라고 하는것 처럼 알아서 타입을 지정해주는 것이다. 다른 언어의 타입 변환과 비슷하지만, 특별한 검사나 데이터 재설정같은 것을 하지 않는다. 런타임에 영향을 미치지 않고 컴파일러에서만 사용된다.  

  사용 방법에는 2가지가 있다.  
  
  1. as-syntax  
  
      ```
      let someValue: unknown = "this is a string";
      let strLength: number = (someValue as string).length;
      ```
    
  2. angle-bracket ( <> )  
  
      ```
      let someValue: unknown = "this is a string";
      let strLength: number = (<string>someValue).length;
      ```
<br>
    someValue가 unknown 타입이지만, string으로 해석하고 길이를 반환한다.
    그리고 만약 JSX에서 이 type assertions를 사용한다면 as-syntax만 허용된다.
  
----------------------------------------------------------------

익숙한 타입도 있고, 처음보는 타입도 있었다.  
지금은 단지 정리한 것 뿐이고 나중에 이를 적용해서 제대로 알아봐야 할 것 같다.
