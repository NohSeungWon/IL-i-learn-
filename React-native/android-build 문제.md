# Andorid 빌드시에 문제와 해결 방법 정리

1. Unable to load script. Make sure youre dither running a Metro server... 
  처음 실행시에 빌드관련 main 참조 영역? 이 없는 것 같다.
 - [패키지명]/android/app/src/main/assets 폴더가 있는지 확인하고 없으면 생성
 - [패키지명]/android 폴더에서 ./gradlew clean 실행
 - [패키지명] 폴더에서 아래 명령어 실행
  - react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res
 - run android 실행 

