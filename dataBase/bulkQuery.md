# bulkQuery 사용법

- - -
Node.js 기준
어디 공식적인 사용법이나 이름을 찾을 수가 없어서 맞는지는 모르겠지만
query parmas에 ? 를 사용하여 client에서 받은 정보를 매핑할때
또는 여러개의 쿼리를 실행하고자 할 때 for문을 돌리지 않고 넣을 때 사용한다.
중요한건 3중배열
- - -

```javascript

  const parmas = [['값'], ['값']]
  const sql = `INSERT INTO test (name) VALUSE ?`;

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