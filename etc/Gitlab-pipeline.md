# GitLab-Runner PupeLine 적용법 정리
- - -
각 케이스별 파이프라인 적용 상태 정리 

## 1. 공통사항
 * yml 파일 예시
 ```javascript
 deploy:
  stage: deploy
  script:
        - echo 'please run please!'
        - npm i
        - ./node_modules/@angular/cli/bin/ng build --prod --aot --output-hashing=all
        - rm -rf /test/tests
        - mv dist /test/tests
  only:
    - feature/cicd
  tags:  // <-- git tag로 생성한 tag가 아니다.
    - test
```
 * script에 적힌 명령어대로 파이프라인이 실행되면 ssh 같이 순서대로 실행됨
 * gitlab runner 설정에 tags의 의미는 git tag의 의미가 아닌 yml에 적힌 tag다.

## 2. Runner 설정에 tag를 적용했을경우
 * Runner가 실행하고 파이프라인에 적용되는 기준 1순위는 only: 에 적힌 브랜치다.
 * case 1 : 만약 only에는 develop을 적고 git push를 feature에 하면 파이프라인이 실행되지 않는다.
 * case 2 : yml파일에 tags 이름을 다르게하면 파이프라인에서 fail 처리를 한다.
 * case 3 : yml파일에 tags를 올바르게 적었지만 only에 브런치가 다를 경우역시 파이프라인이 실행되지 않는다.

## 3. Runner 설정에 tag를 적용하지 않는경우
* only에 적혀있는 branch에 push를 할 경우 파이프라인이 실행된다.