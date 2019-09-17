# Nodejs Sequelize 사용법 
- - -
nodeJS에서 Sequelize를 사용하는 방법
with mysql
- - -

1. Install
```javascript
npm i --save-dev sequelize
npm i --save-dev mysql2
```

2. Init
```javascript
// index.js

import Sequelize from 'sequelize';
import config from '../dbConfig';
import test_table from './test_table'; //define 모델 정의한 곳 

let db = {};

db.Sequelize = Sequelize;
db.sequelize = new Sequelize(
  config.DATABASE,
  config.USERNAME,
  config.PWD, {
    host: 'test.st-2.rds.amazonaws.com',
    dialect: 'mysql'
});

db.test_table = test_table(db.sequelize, Sequelize); // 모델 정의

export default db;
```
* 모델 정의한 곳에 import된 Sequlize를 연결시켜주는 것이 포인트

2. Model define
```javascript
// test_table.js

export default (sequelize, dataTypes) => {
  return sequelize.define('test_table', // table name
  {
    idx: {
      type: dataTypes.INTEGER,
      allowNull: false,
      primaryKey: true,
      autoIncrement: true
    },
    text: {
      type: dataTypes.STRING,
      allowNull: false
    }
  } ,
  {
      timestamps: true
  });
}

```
* new Sequelize로 생성된 것은 sequelize로 념겨받아 define과 같은 메소드로 활용한다.
* 파라미터인 dataTypes는 넘겨받은 Sequlize이고 type으로 쉽게 생각하기위해 이름을 dataTypes로 정의

3. Model usage

```javascript
import db from '../db/index'

db.sequelize.sync();
let resultData = [] ;

export default {
  find: () => {
    //일반 쿼리 
    // 일반 row 쿼리를 날려야 할 때는 define을 사용하지 않고
    // 연결된 sequelize를 바로 써야 query가 수행된다.
    // define으로 query를 날리면 not a function이 뜬다.
    // ex: db.test_table.query() => is not a func
    // {type: db.sequelize.QueryTypes.SELECT} 는 다른거 다 떼고 데이터만 던져주게 한다.
    const querys = 'SELECT * FROM test_tables;';
    return db.sequelize.query(querys, {type: db.sequelize.QueryTypes.SELECT} )
      .then((data) => {
        return data;
      })
      .catch(err => console.log('err',err));

    //sequelize 사용
    return db.test_table.findAll()
      .then((data) => {
        data.forEach(table => {
          resultData.push(table.dataValues);
        });
        return resultData;
      })
      .catch(err => console.log('err', err));
  },

  create: (userInput) => {
    //일반 쿼리 
    const querys = "INSERT INTO `test_tables` (`text`) VALUES(:text)";
    const values = {
      text: userInput
    };
    return db.sequelize.query(querys, {replacements: values})
      .then((data) => {
        return data;
      })
      .catch(err => console.log('err',err));
    
    //sequelize 사용 
    return db.test_table.create({
          text: userInput
        })
          .then((log) => {
            return log
          })
          .catch(err => console.log('err',err));
  },

  delete: (userInput) => {
    return db.test_table.destroy(
      { where: { text: userInput.input } }) 
      .then((log) => {
        return log
      })
      .catch(err => console.log('err',err));
  },

  update: (userInput) => {
    return db.test_table.update(
      { text : userInput.fix },
      { where: { text: userInput.input } }) 
      .then((log) => {
        return log
      })
      .catch(err => console.log('err',err));
  },
}
```
* 일반 즉 mysql raw query를 사용하려면 sequelize.query를 메소드를 사용한다.
* 정의된 model 즉 test_table.query()를 쓰면 query is not a func 라는 error 를 던져버린다.