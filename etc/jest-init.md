# Jest mock 기초

- - -
mocking이란 쉽게 말해 가짜를 구축하는 것이다. 예를들어,
데이터베이스를 왔다갔다 하는 테스트 로직을 짜야할 때 데이터베이스 자체를 만들기는 부담스러우니 mock을 사용해 구현한다.
- - -

1. jest.fn() -> 가짜함수
변수에 대입하는 형태로 만들 수 있다. 기본적으로 함수 값은 없고
mockReturnValue로 설정해줘야 한다.
```javascript
  const mock = jest.fn();
  console.log(mock()) // => undefined

  mock.mockReturnValue('hello');
  console.log(mock()) // => hello
```

2. mockResolvedValue -> 비동기함수

```javascript
  describe('', () => {
  const mock = jest.fn();
  it('', async () => {
    mock.mockResolvedValue('hello');
    const hello = await mock();
    expect(hello).toBe('hello');
  })
});
```

3. 함수 감시
jest.fn()은 함수가 실행된 시점 또는 파라미터 등을 감시하고 체킹할 수 있다.
```javascript
  describe('', () => {
    it('', async () => {
      // toBeCalledTimes -> 함수가 불린 회수 체크 
      expect(getHello).toBeCalledTimes(1); // -> fail 함수가 실행이 안됨
      getHello();
      expect(getHello).toBeCalledTimes(1); // -> success 함수 1번 실행 됨

      //toBeCalledWith -> 함수 실행시 파라미터로 뭐가 왔는지 체크
      expect(getHello).toBeCalledWith(); // 함수 실행시 파라미터로 뭐가 왔는지 체크
      // console.log(getHello());
    })  
  });
```

jest.spyOn(object, methodName) 을 통해서는 가짜함수를 작성하지 않더라도 감시할 수 있다.
```javascript
  const test = { tests: (a, b) => a + b }
  const spy = jest.spyOn(test, 'tests');

  test.tests(1,2)
  expect(spy).toBeCalledTimes(1);
  expect(spy).toBeCalledWith(1, 2);
```


4. jest.mock() -> module의 함수를 자동으로 mock 함수로 변경해줌
```javascript
  
  import { getHello } from '../service/getHello'; // 함수 import
  jest.mock('../service/getHello'); // mock 함수로 사용할 module 가져오기
  // 위 과정을 거치면

  getHello.mock... -> // 이런식으로 mock 함수사용이 가능해진다.  
});
```