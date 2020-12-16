중요 키워드

1. 액션
2. 액션 생성함수
3. 리듀서
4. 스토어
5. 디스패치
6. 구독

React-Native 에서 Redux counter 예제를 구현하면서 Redux의 사용방법을 알아보자.

기초환경준비

현재 폴더구조 

- App.tsx
- Src
    - component
        - Counter.tsx
    - reduxPattern

1. App.tsx

```jsx
import React, { useState } from 'react';
import { View, StyleSheet, Text, Button} from 'react-native';
import Counter from './src/component/Counter';

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center'
  }
});

const App = () => {
  
  return (
  
      <View style={styles.container}>
        <Counter />
      </View>
  );

}

export default App;

// store.subscribe(App);
```

2. Counter.tsx

```jsx
import React from 'react';
import { View, Text, Button } from 'react-native';

const Counter = () => {
  
  return (
    <View>
      <Text style={{textAlign: 'center'}}>증가할 번호</Text>
      <Button 
        title='+'
        onPress={()=>{}}
      />
      <Button 
        title='-'
        onPress={()=>{}}
      />
    </View>
  )
}

export default Counter;
```

Action 정의

폴더구조 

- App.tsx
- Src
    - component

        Counter.tsx

    - reduxPattern
        - action
            - actionType.tsx
            - action.tsx
        - reduce
            - index.tsx
            - reducer.tsx

1. 상수선언 (actionType.tsx)

```jsx
// 액션 타입 정의
export const INCREMENT = 'INCREMENT' ;
export const DECREMENT = 'DECREMENT' ;
```

2. action 생성 (action.tsx)

```jsx
import { INCREMENT, DECREMENT } from './actionType';

export const increment = () => ({ type: INCREMENT }) ;
export const decrement = () => ({ type: DECREMENT }) ;
```

reducer 생성

폴더구조

- App.tsx
- Src
1. reducer (reducer.tsx)

```jsx
import { INCREMENT, DECREMENT } from '../action/actionType';

// 초기 상태 정의
const initialState = {
  number: 0
} ;

export default (state=initialState, action) => {
  switch(action.type) {
      case INCREMENT:
          return {
              ...state,
              number: state.number + 1 ,
          } ;
      case DECREMENT:
          return {
              ...state,
              number: state.number - 1,
          } ;
      default:
          return state ;
  }
}
```

2. reducer 합치기 (index.tsx)

```jsx
import { combineReducers } from 'redux' ;
import reducer from './reducer';

export default combineReducers({
  reducer,
    //다른 리듀서를 만들게 되면 여기에 import
}) ;
```

component / redux 연동

폴더구조

- App.tsx
- Src

Redux 연동 (App.tsx, Counter.tsx)

```jsx
// App.tsx
import React, { useState } from 'react';
import { View, StyleSheet, Text, Button} from 'react-native';
import Container from './src/component/Container';
import { createStore } from 'redux';
import rootReducer from './src/reduxPattern/reduce/index';
import { Provider } from 'react-redux' ;

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center'
  }
});

const store = createStore(rootReducer);

const App = () => {
  
  return (
    <Provider store={store}>
      <Container
      />
    </Provider>
  );

}

export default App;=
```

중요한건 redux store를 사용할 counter.tsx에서의 작업이다.

아직 접한지 얼마안되서 이해가 잘 안되는데

connect란 이름처럼 state와 dispatch를 component와 연동시켜야 Redux를 사용할 수 있다.

```jsx
// Counter.tsx
import React from 'react';
import { View, Text, Button } from 'react-native';
import { increment, decrement } from '../reduxPattern/action/action';
import { connect } from 'react-redux';

const Counter = (props) => {
  console.log(props);
  const { number, increment, decrement } = props;
  
  return (
    <View>
      <Text style={{textAlign: 'center'}}>{number}</Text>
      <Button 
        title='+'
        onPress={increment}
      />
      <Button 
        title='-'
        onPress={decrement}
      />
    </View>
  )
}
// 축약
// const mapDispatchToProps = ({number: state.reducer.number,})
const mapStateToProps = state => {
  return {
    number: state.reducer.number,
  }
}

// 축약
// const mapDispatchToProps = {increment, decrement};
const mapDispatchToProps = dispatch => {
  return {
    increment: () => dispatch(increment()),
    decrement: () => dispatch(decrement()),
  }
}

export default connect ( // 스토어와 연결
  mapStateToProps,
  mapDispatchToProps
)(Counter) ;
```