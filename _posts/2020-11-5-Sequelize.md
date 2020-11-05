---
title: "Sequelize"
tags:
  - sequelize
  - ORM
---
  
[Sequelize 공식문서](https://sequelize.org/master/manual/migrations.html)  
Sequelize를 cli 명령어로 실행하는 방법을 담은 공식 문서이다.  

- Sequelize  
  Sequelize는 Node.js에서 가장 많이 사용되고 있는 ORM이며, MySQL, PostgreSQL, MariaDB, SQLite, MSSQL을 지원한다.  
  가장 큰 특징은 Promise를 기본으로 지원해준다는 점이다.  
  Promise를 사용하면 복잡한 비동기 코드를 깔끔하고 쉽게 만들 수 있도록 하고, chaining으로 값을 전달할 수 있고, 에러 핸들링 처리를 깔끔하게 할 수 있도록 해준다.  

는 이전 글에서 작성했던 내용이고 지금부터 Sequelize 설치 및 사용방법을 알아보도록 하자.  

- 설치  
  다음을 입력한다.  
  `npm install --save sequelize`
  
  그리고 cli로 실행하기 위한 npm도 설치한다.  
  `npm install --save-dev sequelize-cli`  
  <br>
- 프로젝트 시작  
  다음을 입력해서 프로젝트 시작할 준비를 한다.  
  `npx sequelize-cli init`  
  
  그러면 4개의 폴더가 생길 것이다.  
  ```
  config/config.json
  models/
  migrations/
  seeders/
  ```
  
  `config/config.json` 파일은 데이터베이스에 접속할 옵션을 정해놓은 파일이다. 구조는 다음과 같다.  
  ```
  {
    "development": {
      "username": "root",
      "password": null,
      "database": "database_development",
      "host": "127.0.0.1",
      "dialect": "mysql"
    },
    "test": {
      "username": "root",
      "password": null,
      "database": "database_test",
      "host": "127.0.0.1",
      "dialect": "mysql"
    },
    "production": {
      "username": "root",
      "password": null,
      "database": "database_production",
      "host": "127.0.0.1",
      "dialect": "mysql"
    }
  }
  ```
  보통 development는 개발할때, test는 테스트할때, production은 배포할때 사용한다.  
  model/index.js에서 `process.env.NODE_ENV`로 어떤 옵션으로 데이터베이스에 접속할지 정의한다.  
  <br>
- 모델 생성  
  다음을 입력해서 모델을 생성하자.  
  `npx sequelize-cli model:generate --name 모델명 --attributes 칼럼명:데이터타입,칼럼명:데이터타입`
  이를 입력하면 models/모델명.js 파일과 migration/XXXXXXXXXXXXXX-create-모델명.js 파일이 자동으로 생성된다.  
  attribute에 id, createdAt, updatedAt 이 3개는 자동으로 생성된다.  
  
  sequelize는 models폴더에 테이블을 표현하는 js파일을 사용한다. 그리고 migration파일은 model폴더에서 표현된 테이블을 보고 `queryInterface.createTable()`로 데이터베이스에 ORM을 사용해서 저장한다.  
  <br>
- 마이그레이션 실행  
  다음을 입력하여 마이그레이션을 실행하여 데이터베이스에 저장한다.  
  `npx sequelize-cli db:migrate`
  
  그런데 만약 스키마를 잘못 작성했다? 되돌려야한다? 그러면 다음을 입력하여 마이그레이션 한 것을 되돌릴 수 있다.  
  `npx sequelize-cli db:migrate:undo`
  
  그리고 특별하게 제약조건이나 어떤 것을 추가로 설정하고 싶다면 migration 뼈대만 따로 생성할 수 있다.  
  `npx sequelize-cli migration:generate --name migration-skeleton`
  <br>
- Sequelize의 CRUD  
  1. create  
    Model.create()로 칼럼을 생성한다.  
    `await User.create({ firstName: "Jane", lastName: "Doe" });`
    이는 User테이블에 firstName은 Jane, lastName은 Doe인 칼럼을 추가하는 코드이다.  
    sql문으로 하면 `INSERT INTO user (firstName, lastName) VALUES ("Jane", "Doe");`와 같다.  
    <br>
  2. select  
    sql에서 select는 sequelize에서는 find로 대체된다. 특정 칼럼을 조회하고 싶다면 Model.findOne(), 모든 칼럼을 조회하고 싶다면 Model.findAll()을 사용한다.  
    조건을 달고싶으면 괄호 안에 object를 삽입하고, key는 where로 한다.  
      ```
      Post.findAll({
        where: {
          authorId: 2
        }
      });
      ```
      이는 post테이블에서 authorId가 2인 칼럼을 모두 조회하는 코드이다.  
      sql문으로 하면 `SELECT * FROM post WHERE authorId = 2;`와 같다.  
    
      또한 기본키로 조회하는 `findByPk()`, 조건에 부합되는게 있으면 조회하고 없으면 생성하는 `findOrCreate()`도 자주 사용된다.  
    <br>
  3. update  
    Model.update()로 수정한다.  
      ```
      await User.update({ lastName: "Doe" }, {
        where: {
          lastName: null
        }
      });
      ```
      이는 user테이블에서 lastName이 Doe인 칼럼을 null로 변경하는 코드이다.  
      sql문으로 하면 `UPDATE user SET lastName = "Doe" WHERE lastName = nuill;`와 같다.  
    <br>
  4. delete  
    Model.destroy()로 삭제한다.  
      ```
      await User.destroy({
        where: {
          firstName: "Jane"
        }
      });
      ```
      이는 user테이블에서 firstName이 Jane인 칼럼을 삭제하는 코드이다.  
      sql문으로 하면 `DELETE FROM user WHERE firstName = "Jane";`와 같다.  
