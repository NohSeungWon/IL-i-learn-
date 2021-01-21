# Redux test
- - -
React-native 에서 Redux + Typescript로 테스트를 구축하는 법.
React 16.10.13 version
- - -

- init 
 1. npx create-react-app my-app --template typescript
 2. npm install --save typescript @types/node @types/react @types/react-dom @types/jest
 3. npm i -D typescript @types/jest @types/react @types/react-native @types/react-test-renderer
 4. npm i react-native react-native-web
 5. npm i redux react-redux
 6. npm i -D @types/redux-mock-store
 7. npm i -D @types/react-redux
 8. npm i -D @testing-library/react-native
 9. tsconfig.json에 추가 -> 필수아님
  ```javascript 
    "exclude": [
      "node_modules",
      "babel.config.js",
      "metro.config.js",
      "jest.config.js"
    ]
  ```
  10. jest.config.js 추가 -> 필수아님
  ```javascript 
    module.exports = {
      preset: 'react-native',
      moduleFileExtensions: ['ts', 'tsx', 'js', 'jsx', 'json', 'node']
    };
  ```
  
  * 리덕스 테스트는 action, reducer, 그리고 component에서 state 변경을 받는지를 테스트 하는 것이 좋겠다.
  * action, actionType, reducer는 한곳에 작성한다.
  * 순서는 action, reducer, component 순이다.
  
  - action 테스트
  action 테스트는 실제로 많이 하지는 않을 것 같다. action이 생성되는 것만 확인하는 것에 그치기 때문이다.
  
  reducer.tsx 
  ```javascript 
    export const GET_TEXT = 'GET_TEXT';
    export const CHANGE_TEXT = 'CHANGE_TEXT';

    export const getText = () => ({ type: GET_TEXT });
    export const changeText = (data: string) => ({ type: CHANGE_TEXT, payload: data });

    const reducer = (state: any = { msg: '' }, action?: {type: string, payload?: string} ) => {
      if (action?.type === GET_TEXT) {
        return state
      }
      if (state && action?.type === CHANGE_TEXT) {
        state.msg = action?.payload
        return state
      }
      return state
    }

    export default reducer;
  ```
  
  action.test.tsx
  ```javascript
    import { getText, changeText } from '../reduxs/reducer';

    describe('action test', () => {
      it('create action work', () => {
        const wantedActionTypes = [
          {type: 'GET_TEXT'},
          {type: 'CHANGE_TEXT', payload: 'test'}
        ];

        const actions = [ getText(), changeText('test') ];

        expect(actions).toEqual(wantedActionTypes);
      })
    })
  ```

  - reducer 테스트
  reducer.test.tsx
  ```javascript
    import reducer, {changeText} from '../reduxs/reducer';

    describe('reducer test', () => {
      let state = reducer();
      it('default reducer result is empty object', () => {
        expect(state).toEqual({ msg: ''});
      });

      it('changeText action result is hello', () => {
        state = reducer(state ,changeText('hello'))
        expect(state).toEqual({msg: 'hello'});
      });

    });
  ```

   - component 테스트
  First.tsx
  ```javascript
    import { View,Text, Button } from 'react-native';
    import { useDispatch, useSelector } from 'react-redux';
    import { changeText } from '../reduxs/reducer';

    const First = () => {
      const dispatch = useDispatch();
      const state = useSelector((state: any) => {
        return state.msg
      });
      return (
        <View>
          <Text>{state? state : 'not yet'}</Text>
          <Button title="change" onPress={() => {dispatch(changeText('world'))}}></Button>
        </View>
      );
    }

    export default First;
  ```

  First.test.tsx
  ```javascript
    import { render, fireEvent } from '@testing-library/react-native';
    import First from '../component/First';
    import { Provider } from 'react-redux';
    import store from '../reduxs/store';

    describe('First Component test', () => {
      it('after first render text is not yet', () => {
        const { getByText } = render(
          <Provider store={store}>
            <First />
          </Provider>
        );

        expect(getByText('not yet')).toBeTruthy();
      })

      it('after changeBtn click text is world', () => {
        const { getByText } = render(
          <Provider store={store}>
            <First />
          </Provider>
        );

        fireEvent.press(getByText('change'))
        expect(getByText('world')).toBeTruthy();
      })
    })
  ```



  

