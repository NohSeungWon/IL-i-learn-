thunk로 비동기를 구현할 때 중요한 지점은 아래와 같다. 

- redux에게 middleWare 사용을 알리고 시작한다.
- 서버에서 데이터를 받아올 때 준비,성공, 실패에 대한 액션 3가지를 준비한다.
- saga와 다른점은 ?
    - generater 기반에 rootSaga를 따로 작성할 필요없이 Action에서 비동기를 수행할 수 있다.

Redux MiddlerWare에 thunck 등록

```jsx
import React from 'react';
import { StyleSheet } from 'react-native';;
import { applyMiddleware, createStore } from 'redux';
import { Provider } from 'react-redux' ;
import rootReducer from './src/reduxPattern/reduce/index';
import ReduxThunck from 'redux-thunk'; // 청크 import 
import Thunck from './src/component/Thunck';
import Counter from './src/component/Counter';

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center'
  }
});

export const store = createStore(
	rootReducer, 
	applyMiddleware(ReduxThunck) // 미들웨어 등록
);

const App = () => {

  return (
    <Provider store={store}>
      {/* <Counter /> */}
      <Thunck />
      {/* <Saga /> */}
    </Provider>
  );

}

export default App;
```

액션 3가지 준비

READY는 실행

COMPLETE는 실행 후 데이터 받기

ERR는 그냥 ERR

actioType.tsx

```jsx
export const READY = 'READY';
export const COMPLETE = 'COMPLETE';
export const ERR = 'ERR';
```

thunckAction.tsx

```jsx
import { READY, ERR, COMPLETE } from '../action/actionType';

export const err = (err) => ({ type: ERR , texts: err});
export const complete = (text) => ({ type: COMPLETE, texts: text});
export const ready = () => {
  return (dispatch, getState) => {
    return fetch('http://localhost:3002')
    .then(res => res.json())
    .then(data => {
      dispatch(complete(data.texts))
    })
    .catch(error => dispatch(err('err')))
  }
};
```

asyncReducer.tsx

```jsx
import { READY, ERR, COMPLETE } from '../action/actionType';

// 초기 상태 정의
const initialState = {
  texts: 'not yet',
};

export default (state = initialState, action) => {
  console.log(action);
  
  switch(action.type) {
    case READY :
      return {
        ...state,
        texts: 'ready'
      }
    case ERR : 
      return {
        ...state,
        texts: action.texts
      }
    case COMPLETE : 
      return {
        ...state,
        texts: action.texts
      }
    default : 
     return state
  }
}
```

Thunck.tsx (component)

```jsx
import React, { useEffect } from 'react';
import { View, Text, Button } from 'react-native';
import { ready, complete } from '../reduxPattern/action/thunckAction';
import { connect, useDispatch, useSelector } from 'react-redux';


const Thunck = () => {
  
  const dispatch = useDispatch(); 
  const stateText = useSelector(state => state.asyncReducer.texts);
  
  useEffect(() => { 
    dispatch(ready());
  },[]);

  return (
    <View>
      <Text style={{textAlign: 'center'}}>{stateText}</Text>
      <Button
        title="textChange"
        onPress={() => dispatch(complete())}
      ></Button>
    </View>   
  )
}

export default Thunck;
```