# NodeJs Mysql Db 접속시 async/await 사용법
- - -
정말 이상하게 이해가 잘 안되었는데.
가장 확실한 방법은 async/await을 로그로 찍어보고
promise가 반환되지 않는다면
Promise를 새로 만드는 것이 방법이다.
- - -

* Pull Code
```javascript
// client - side 
import testModels form './,,,' // models 가져오기

export default {
  getContorl: async (req, res) => {
    const data = await testModels.getModel();
    res.send(data);
  }
}

// server - side 
export default {
  getModel: async () => {
    const queryString = 'select * from ...';
    const setData = () => {
      return new Promise(resolve => {
        db.query(queryString, (err, data) => {
          resolve(data);
        })
      })
    }
    const resultData = await setData();
    return resultData;
  }
}
```