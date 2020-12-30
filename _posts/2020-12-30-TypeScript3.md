---
title: "타입스크립트(TypeScript) 문법"
tags:
  - TypeScript
---

타입스크립트 개요 포스트에서 특징 중 하나가 객체지향 언어라는 특징이 있었다.  
그래서 이번 포스팅에는 이 객체지향에 포인트를 두고 작성하도록 하겠다.  

----------------------------------------------------------------

- 인터페이스  
  인터페이스는 타입스크립트의 여러 객체를 정의하는 일종의 규칙이다.  
  일단 예시로 알아보자.  
  
  ```
  interface userInfo {
    name: string,
    age: number
  }
  ```
  
  이런 식으로 인터페이스를 정의할 수 있다. 유저 정보로 이름과 나이를 정의할 수 있도록 구조를 만들어 놓은 것이다.  
  정의한 후에 다음과 같이 사용할 수 있다.  
  
  ```
  let user1: userInfo = {
    name: "Kim",
    age: 24
  };
  
  // 인터페이스에 정의된 age 값이 없기 때문에 오류 발생
  let user2: userInfo = {
    name: "Choi"
  };
  ```
  
  하지만 인터페이스에서 변수 뒤에 `?`를 붙이면 필수가 아닌 선택 속성으로 바뀌며, 작성하지 않아도 오류가 발생하지 않게 된다.  
  
  ```
  interface userInfo {
    name: string,
    age?: number // optional property
  }
  
  // 오류가 발생하지 않음
  let user2: userInfo = {
    name: "Choi"
  };
  ```
  
  변수 앞에 `readonly`를 붙이면 수정할 수 없는 읽기 전용 속성으로 바뀝니다.  
  
  ```
  interface userInfo {
    readonly name: string,
    age: number
  }
  
  let user1: userInfo = {
    name: "Kim"
    age: 24
  };
  
  user1.name = "Lee" // 오류
  user1.age: 25 // OK
  ```
  
- class  
  객체지향 프로그래밍의 대표적인 특징인 클래스이다.  
  인터페이스를 클래스로 정의하는 경우에는 `implements` 키워드를 사용한다.  
  
  ```
  interface userInfo {
    name: string,
    getName(): string
  }

  class User implements userInfo {
    constructor(public name: string) {}
    getName() {
      return this.name;
    }
  }

  const makeUser = new User('Kim');
  makeUser.getName(); // 'Kim'
  ```
  
  여기서 constructor 생성자 함수를 이용하여 객체를 생성할 수도 있습니다.  
  
  ```
  interface userInfo {
    name: string,
    getName(): string
  }

  class User implements userInfo {
    constructor(public name: string) {
      this.name = name;
    }
    getName() {
      return "Hello, " + this.name;
    }
  }

  const makeUser = new User('Kim');
  makeUser.getName(); // 'Hello, Kim'
  ```
  
- Inheritance(상속)  
  클래스 기반 프로그래밍의 꽃 상속!  
  `extends` 키워드로 부모 클래스의 것들을 물려받아 자식 클래스에서도 사용할 수 있게 한다.  
  
  ```
  class Animal {
    name: string;
    constructor(theName: string) {
      this.name = theName;
    }
    
    move(distanceInMeters: number = 0) {
      console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
  }

  class Dog extends Animal {
    constructor(name: string) {
      super(name);
    }
    
    move(distanceInMeters: number) {
      super.move(distanceInMeters);
    }
    
    bark() {
      console.log('으르렁!');
    }
  }

  const dog = new Dog("멍멍이");
  dog.move(10); // '멍멍이 moved 10m.
  dog.bark(); // '으르렁!'
  ```
  
  constructor 생성자 함수에서 `super` 키워드로 부모의 인스턴스(name)를 사용하여 이름을 지을 수 있고, 부모의 메소드(move)를 사용하여 움직인 거리도 지정할 수 있게 하였다.  
  
- Public, Protected, Private(접근 지정자)  
  + public  
    프로그램 내에서 선언된 멤버들에 어디서나 자유롭게 접근할 수 있는 지정자이다.  
    defalut로 public으로 지정되어 있다.  
  
  + protected  
    나와 파생된 자식 클래스에서 접근 가능할 수 있는 지정자이다.  
    
    ```
    class Animal {
      protected name: string;
      constructor(theName: string) {
        this.name = theName;
      }
    }

    class Cat extends Animal {
      getName(): string {
        return `Cat name is ${this.name}.`;
      }
    }
    
    let cat = new Cat('Lucy');
    console.log(cat.getName()); // Cat name is Lucy.
    console.log(cat.name); // 오류: protected로 선언했기 때문에 접근할 수 없음
    cat.name = 'Tiger'; // 오류: protected로 선언했기 때문에 접근할 수 없음
    ```
    
  + private  
    내 클래스에서만 접근 가능할 수 있는 지정자이다.  
    
    ```
    class Animal {
      private name: string;
      constructor(theName: string) {
        this.name = theName;
      }
    }

    class Cat extends Animal {
      getName(): string {
        return `Cat name is ${this.name}.`; // 오류: private으로 선언했기 때문에 접근할 수 없음
      }
    }
    
    let cat = new Cat('Lucy');
    console.log(cat.getName()); // Cat name is Lucy.
    console.log(cat.name); // 오류: private으로 선언했기 때문에 접근할 수 없음
    cat.name = 'Tiger'; // 오류: private으로 선언했기 때문에 접근할 수 없음
    ```
    
- abstract class(추상 클래스)  
  추상 클래스는 인터페이스와 같은 개념이라고 생각하면 된다.  
  인터페이스처럼 구조만 세우는 개념이라 추상 클래스 내에서 생성과 구현을 할 수 없다.  
  그래서 자식 클래스에서 인스턴스를 생성하고, 메소드를 구현해야 한다.  
  
- export와 import  
  타입스크립트에서는 변수, 함수, 클래스 뿐만 아니라 인터페이스나 타입 별칭도 모듈로 export, import할 수 있다.  
  
-----------------------------------------------------------------------

학부생 시절때 Java에서 봐왔던 객체지향 프로그래밍을 이번 타입스크립트를 공부하면서 다시 만나게되니 감회가 새로웠다.  
이 글을 정리하면서 타입스크립트가 Java와 매우 유사하다는 것을 느꼈다.  
그래서 타입스크립트를 공부하면 저절로 Java가 같이 공부가 될 것 같다는 생각이 들었다.  
