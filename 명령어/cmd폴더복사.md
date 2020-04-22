## cmd폴더복사 명렁어

- copy 명령어를 사용하면 폴더안에 있는 파일들을 다 가져올 수 없다.
- mkdir 명령어로 폴더를 copy와 함께 싱크를 맞춰줘야되는데 상당히 고통스럽다
- 예를들면 

```javascript
  mkdir test
  mkdir test\\one
  mkdir test\\two
  copy origin test
  copy origin\\one test\\one
  copy origin\\two test\\two
```
- 이런식으로 일일이 맞춰줘야한다.

- xcopy를 이용하면 한번에 다 이동이 가능하다 
- 단 /s 옵션을 붙여야 한다.
- 옵션은 꽤 여러가지가 있는데 사용법이 단순하지만 잘 안된다.
- xcopy를 이용하면 한줄이 된다.

```javascript
  xcopy origin test /s
```