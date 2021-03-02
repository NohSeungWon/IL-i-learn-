#CodePush를 활용한 react-native 앱 배포

- - -

CodePush는 번거로운 앱 배포과정을 쉽게 만들어준다.

CodePush를 사용하지 않으면 빌드 -> 배포 -> 심사기간 을 거치지만

사용하면 코드수정 -> push 만 하면 실시간으로 앱 업데이트가 이루어진다고 한다.

- - -
- 개념

app.js 즉 로드가 시작될때 codepush 코드로 sync를 검사하고 (코드자체를 비교하는지 version차이를 캐치하는지 확인해야함) 새로운게 있으면 코드를 덮어씌우는 형태인 것 같다.

- 기본설정

```javascript

/* 전역설치 */

sudo npm i -g code-push-cli

/* 앱등록을 위한 절차

- 실행시키면 app center 웹화면에서 인증토큰이 발생한다.
- /

code-push register

/*

- app center 사이트에서 add new app 으로 프로젝트를 생성하는 것 처럼
- terminal에서는 명령어로 수행한다.
- 명령어를 수행하고나면 key를 부여받는다.
- /

code-push app add MyApp-Android android react-native

code-push app add MyApp-iOS ios react-native

// 해당 폴더에서 react-native-code-push 설치

npm i --save react-native-code-push

```

- ios 설정

```javascript

// pod install 실행

pod install

// xcode 실행 -> ios/projectname/AppDelegate.m 파일열고 상단에 아래코드 입력

#import <CodePush/CodePush.h>

// 같은 파일안에 아래 코드 찾고

return [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"];

// 이 코드로 변경

{

#if DEBUG

return [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index" fallbackResource:nil];

#else

return [CodePush bundleURL];

#endif

}

// ios/projectName 폴더아래 AppCenter-Config.plist 생성

1. new file로 생성해

2. property list로 클릭하고 next

// CodePushDeploymentKey 추가

code-push deployment ls 서비스 이름 -k 으로 키 확인

1. info.plist 파일 열고 CodePushDeploymentKey에 staging 키추가 (production은 배포용인듯) -> CodePushDeploymentKey가 list에 없으면 생성한다.

// 조금 헷갈리는 부분인데 배포자체는 appcenter cli로 한다.

// codepush를 이용해서 appcenter에 올리는건가 codepush가 appcenter로 통합된건가 이해가 잘 안간다.

npm install -g appcenter-cli

appcenter login

appcenter codepush release-react -a [owner email]/[project name] -d [Production|Staging]

```