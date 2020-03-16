# 코드 스타일 가이드
- eslint와 prettier를 함께 적용하는 방법

1. vscode Extension 설치
  - eslint install
  - prettier install

2. npm 설치
  - npm install eslint --save-dev
  - npm install prettier --save-dev --save-exact
  - --save-exact 는 버전 변경시 변화 이슈를 막아준다고 한다.
  - npm install eslint-plugin-prettier --save-dev : Prettier와 충돌할 설정들을 비활성화
  - npm install eslint-config-prettier --save-dev : 코드 코맷할 때 Prettier를 사용하게 만드는 규칙을 추가

3. .eslinrc.json (root에) 생성
 - 중요한건 extends에 prettrer 플러그인, rules에 추가
 - 또는 lint init 으로 자동생성 
```json
  {
  "plugins": ["prettier"],
  "extends": ["eslint:recommended", "plugin:prettier/recommended"],
  "rules": {
    "prettier/prettier": "error"
  }
}
```

4. vscode 설정 
- 설정을 들어가서 json 형태로 변환 
```json
{
    // Set the default
    "editor.formatOnSave": false,
    // Enable per-language
    "[javascript]": {
        "editor.formatOnSave": true
    },
    "editor.codeActionsOnSave": {
        // For ESLint
        "source.fixAll.eslint": true
    }
}
``` 

5. .prettier (root에) 생성
```json
{
    "trailingComma": "es5",
    "tabWidth": 2,
    "semi": true,
    "singleQuote": true
}
```
