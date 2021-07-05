~~아직 Redux를 많이 사용해보지 않아서 action에서 비동기처리를 하는 로직을 추가하는 것이 왜 사람들이 꺼려하는지 잘 체감하기는 아직 어렵다.~~ (코드를 작성하면서 보니 Redux는 비동기처리를 하려면 따로 미들웨어를 사용해야한다. 공홈에서도 middleWare 사용을 이야기한다.) 물론 action에서 퓨어한 함수 처리만 하는 것이 좋다는 것을 머리로는 받아들이고 있다.

saga로 비동기를 구현할 때 중요한 지점은 아래와 같다.

- redux에게 middleWare 사용을 알리고 시작한다.
- 서버에서 데이터를 받아올 때 준비,성공, 실패에 대한 액션 3가지를 준비한다.
    - 위 처럼 하지 않으면 useEffect에서 무한 루프가 줄줄 돈다.
    - useEffect(function(),[]) → 빈 배열이나 변하는 변수를 넣어도 무한 루프다.

Redux에게 MiddlerWare를 등록하기

추가된 사항들에 주석

```jsx
import React from 'react';
import { StyleSheet } from 'react-native';;
import { applyMiddleware, createStore } from 'redux';
import { Provider } from 'react-redux' ;
import reduxSaga from 'redux-saga'; // saga 사용
import rootReducer from './src/reduxPattern/reduce/index';
import Counter from './src/component/Counter';
import Saga from './src/component/Saga';
import rootSaga from './src/reduxPattern/saga/rootSaga'; // combineReducer 와 비슷하게 root 입력

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center'
  }
});
const sagaMiddleWare = reduxSaga(); // 미들웨어 만들기
const store = createStore(rootReducer, applyMiddleware(sagaMiddleWare)); // 미들웨어 등록
sagaMiddleWare.run(rootSaga); // 미들웨어 시작

const App = () => {

  return (
    <Provider store={store}>
      <Counter />
      <Saga />
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

asyncAction.tsx

```jsx
import { ERR, READY, COMPLETE } from './actionType';

export const ready = () => ({ type: READY });
export const complete = (text) => ({ type: COMPLETE, texts: text });
export const err = (err) => ({ type: ERR , texts: err});
```

rootsaga.tsx

```jsx
/*
	* takeEvery는 무엇인지 잘 모르겠다.
	* call이 비동기를 기다릴 수 있게 하는 메소드
	* put은 state? props? 변경을 알리는 메소드
*/
import { takeEvery, put, call } from 'redux-saga/effects';
import { READY, ERR, COMPLETE } from '../action/actionType';
import { ready, complete, err } from '../action/asyncAction';

function* dataFetch() {
  try {
    const data = yield call(() =>
      fetch("http://localhost:3002")
        .then(response => response.json())
        .then(myJson => myJson)
    );
    yield put(complete(data.texts))
  } catch (error) {
    yield put(err(error));
  }
}

function* rootSaga () {
  yield takeEvery(READY, dataFetch) 
	// READY로 dataFetch를 실행하고 
	// 성공하면 complete를 실행해서 props를 넘겨준다.
}

export default rootSaga;
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

Saga.tsx (component)

```jsx
import React, { useEffect } from 'react';
import { View, Text } from 'react-native';
import { ready } from '../reduxPattern/action/asyncAction';
import { connect, useDispatch } from 'react-redux';

const Saga = (props) => {
  const dispatch = useDispatch(); // component 안에서 dispatch를 사용가능하게 해준다.
  
  useEffect(() => { 
    dispatch(props.ready()); 
  },[]);

  return (
    <View>
      <Text style={{textAlign: 'center'}}>{props.texts}</Text>
    </View>   
  )
}

const stateToProps = state => ({ texts: state.asyncReducer.texts });
const dispatchToProps = { ready };

export default connect(stateToProps, dispatchToProps)(Saga);
```