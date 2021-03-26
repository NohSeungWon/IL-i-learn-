# 유효성 검사 목록

```javascript
export const SpecialCharactersCheck = text => {
  // 특수문자 체크
  const special_pattern = /[~!@#$%^&*()_+|<>?:{}]/;
  if (special_pattern.test(text) === true) {
    return false;
  }
  return true;
};

export const emptyChecker = text => {
  // 공백감지
  const blank_pattern = /[\s]/g;
  if (blank_pattern.test(text) === true) {
    return false;
  }
  return true;
};
```