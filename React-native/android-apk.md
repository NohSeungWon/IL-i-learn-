# develop 모드의 apk 뽑는 방법

1. android/app/src/main/assets 폴더 생성 
2. root 폴더에 아래코드 작성
  - react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res/
3. 안드로이드 스튜디오 build -> build apk 클릭
4. /android/app/build/outputs/apk/debug/  app-debug.apk 파일 생성됨