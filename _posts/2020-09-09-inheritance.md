---
title: "Inheritance(상속)"
tags:
  - prototype chain
  - 객체
  - 생성자
  - prototype inheritance
  - class
---

- javascript에서 객체를 생성하는 방법(Prototype Chain)  
  1. Object Literal  
    일반적으로 객체를 만들듯이 중괄호({})로 만드는 방법이다. 객체가 정의되는 순간 객체는 프로토타입으로 Object.prototype을 가진다. 따라서 Object.prototype에 정의된 메소드들을 사용할 수 있다. 그래서 MDN에 검색하는 메소드를 보면 Object.prototype.메소드명 이라고 검색되는 것이다.  
    예를들어 `let object = {};` 라는 객체를 선언한다는 것은 `let object = new Object();` 라는 생성자 명령으로 이루어진다고 볼 수 있다.  
<br>
  2. Constructor Function(생성자 함수)

		  function Person(){
            this.name = 'NemoTaek';
            this.age = 26;
		  }
		
		  Person.prototype.nextYear = function(){
            this.age++;
		  }
		
		  let info = new Person();
		
      위와 같이 생성자 함수를 만들고 **new 키워드**로 객체를 생성한다.  
      info 객체는 this 객체를 output으로 가지기 때문에 Person에서 this에 담긴 속성들이 객체로 담기기 된다.  
      그리고 Person 객체는 nextYear이라는 프로토타입 메소드를 가지고 있기 때문에 new로 생성된 info 객체는 Person의 프로토타입에 접근할 수 있다.  
<br>
  3. Object.create
  
          let personPrototype = {
              nextYear: function(){
                  this.age++;
              }
          }

          let info = Object.create(personPrototype);
          info.name = 'NemoTaek';
          info.age = 26;
        
      `Object.create` 메소드는 인자로 들어온 객체를 프로토타입으로 하는 새로운 객체를 생성한다.  
      예시에서 personPrototype이라는 객체를 Object.create의 인자로 넣었으므로 info의 프로토타입 객체는 personPrototype이 된다.  
<br>

- Prototype Inheritance  

        function Person(name, age){
          this.name = name;
          this.age = age;
        }

        Person.prototype.nextYear = function(){
          this.age++;
        }

  그리고 인턴의 생성자 함수를 만들어 보겠다.  

        function Intern(name, age){
          this.isIntern = true;
        }

  사원과 인턴을 구분하기 위한 isIntern 속성을 추가하였다. 다른 속성 및 메소드는 Person의 것을 가져올 것이다.  
  이제 프로토타입 상속을 해보자.  

        function Intern(name, age){
          Person.call(this, name, age);
          this.isIntern = true;
        }

        Intern.prototype = Object.create(Person.prototype);
        Intern.prototype.constructor = Intern;

  Object.create 메소드를 사용하여 부모 클래스의 프로토타입을 프로토타입으로 하는 객체를 생성하고, 자식 프로토타입에 할당한다.  
  이 과정을 통해 생성된 객체도 프로토타입 체인을 통해 부모 클래스의 메소드에 접근할 수 있게 된다.  
  그리고 Object.create 메소드를 사용하여 할단된 객체의 프로토타입에는 생성자가 사라지게 된다. 따라서 프로토타입에 있어야 하는 생성자 속성을 할당한다.  

  마지막으로 call 메소드를 통하여 자식 함수가 실행될 때 부모 함수가 실행되고 부모의 this를 자식의 this 객체로 할당한다.  

<br>
- 그런데 ES2015에서는 이러한 프로토타입 상속의 과정을 더 간단하게 코딩하기 위한 방법으로 **Class**가 도입되었다. 위의 예제와 동일한 Class 구문은 다음과 같다.  

        Class Person{
          constructor(name, age){
            this.name = name;
            this.age = age;
          }

          nextYear(){
            this.age++;
          }
        }

        class Intern extends Person{
          constructor(name, age){
            super(name, age);
            this.isIntern = true;
          }
        }

  객체의 생성자를 생성하는 방법과 상속하는 방법이 매우 간단해진 것을 볼 수 있다.  
  생성자임을 강조하기 위해 **constructor객체** 아래에 함수식으로 작성하면 되고, 자식객체가 부모객체를 상속받는다는 것을 정의할 때는 **extends와 super 키워드**를 사용하면 된다.  
