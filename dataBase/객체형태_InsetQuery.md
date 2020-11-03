# insertQuery 사용법

- - -
Node.js 기준
insert 구문시 객체 형태로 유용하게 적용할 수 있는 쿼리문이다.
객체안에 키를 필드이름으로 세팅하면 자동으로 적용된다.
여러개 insert는 지원히자 않는다.
- - -

```javascript

  const parmas = { name: '값' }
  const sql = `INSERT INTO SET ?`;

  new Promise((resolve, reject) => {
        DB.connection.query(sql, [parmas], (err, result) => {
          if (err) {
            err.sql = sql;
            return reject(err);
          }
          resolve(result);
        });
      });
```