# Expo를 활용한 React-native 기초구성

1. 설치
- npm i -g expo-cli
- react-cli를 사용하는 것과 다른점은 watchman도 자동적으로 깔리는 것 같다.

2. 기초 환경구성
- expo init project-name
- 자동으로 폴더와 환경이 구성되서 나온다. 이때 typeScript를 기반으로 할건지 등등에 대해 묻는다.

3. 기본구조
- 자세하게 설명보다는 자동설치 하게 되면 나오는 코드이다.
- React를 사용할 떄와 다른 점은 없고, style을 적용할 때 StyleSheet를 사용하고 객체 형태로 만들어 view에서는 키를 사용해 적용한다.

``` javascript
import { StatusBar } from 'expo-status-bar';
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});

const App = () => {
  return (
    <View style={styles.container}>
      <Text>기본구조</Text>
      <StatusBar style="auto" />
    </View>
  );
}

export default App;
```

