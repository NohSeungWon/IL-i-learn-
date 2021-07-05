Context Api 사용법

- 기본 코드 쿠조

```javascript
  <RootComponent>
    <ContextProvider>
      <UseComponent>
      </UseComponent>
    </ContextProvider>
  </RootComponent>
```

- Context 설정 -> indexContext.js
```javascript
import React, { useReducer, createContext, useContext } from 'react';
import produce from 'immer';
const initial = {
  data: null,
};

export const ActionTypes = {
  SET: 'SET',
  GET: 'GET',
};

const reducer = (state, action) => {
  switch (action.type) {
    case ActionTypes.SET:
      return produce(state, (draft) => {
        draft.data = action.paylod;
      });
    case ActionTypes.GET:
      return state;
    default:
      throw new Error(`Unhandled action type: ${action.type}`);
  }
};

const stateContext = createContext();
const dispatchContext = createContext();

export const Provider = ({ children }) => {
  const [state, dispatch] = useReducer(socialReducer, initial);
  return (
    <stateContext.Provider value={state}>
      <dispatchContext.Provider value={dispatch}>{children}</dispatchContext.Provider>
    </stateContext.Provider>
  );
};

export const state = () => {
  return useContext(stateContext);
};

export const dispatch = () => {
  return useContext(dispatchContext);
};
```

- 적용법

```javascript
import { Provider, state, dispatch, actionTypes } from './indexContext';

const Children = () => {
	const contextData = state(); // 사용
	
	useEffect(() => {
		..원하는 로직 init
	}, [])

	return (
		<Button>
			카카오 로그인
		</Button>
	)
}

const Parent = () => {
	const useDispatch = dispatch(); // set 준비

	useEffect(() => {
    useDispatch({ type: actionTypes.SET, paylod: {...} }); // set
  }, []);

	return (
		<div>
			<Children>
			</Children>
		</div>
	)
}

const root = () => {
	return (
      <>
        <Head>
          <meta
            name="viewport"
            content="user-scalable=no,initial-scale=1,maximum-scale=1,minimum-scale=1,width=device-width,height=device-height"
          />
          <meta charSet="utf-8" />
          <meta httpEquiv="X-UA-Compatible" content="IE=edge" />
          <link rel="shortcut icon" href="/images/triangle.png" />
          <title>One - React Next.js Ant Design Dashboard</title>
          <link rel="preconnect" href="https://fonts.gstatic.com" />
          <link rel="stylesheet" href="/styles.css" />
          
          <script src="https://developers.kakao.com/sdk/js/kakao.js"></script>
          {pageProps.ieBrowser && (
            <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-polyfill/7.2.5/polyfill.min.js" />
          )}
        </Head>
        <Provider> // contextMapPing
          <Parent>
          </Parent>
        </Provider>
      </>
    );
}
```