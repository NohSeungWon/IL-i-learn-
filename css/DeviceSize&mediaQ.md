# DeviceSize와 @media적용법
- - -
1. web, mobile, tablet 별 사이즈 기록
2. device별로 css 코드적용법 기록
- - -

- mediaQ 적용

```jacascript
    /* Laptop large */

    <link rel="stylesheet" href="./laptop.css" media="(min-width: 1025px)">
    @media all and (min-width:1025px) {}

    /* Laptop small */

    <link rel="stylesheet" href="./lap.css" media="(max-width: 1024px) and (min-width: 768px)">
    @media all and (min-width:768px) and (max-width:1024px) {}

    /* tablet Device */

    <link rel="stylesheet" href="./tablet.css" media="(min-width: 424px) and (max-width: 767px)" >
    @media all and (min-width:424px) and (max-width: 767px) {}

    /* Mobile Device */

    <link rel="stylesheet" href="./mobile.css" media="(max-width: 425px)">
    @media all and (min-width: 425px) {}
```
- web
 1. laptop L - 1440 / 900
 2. laptop - 1024 / 720
 
 - mobile

  1. L - 425 / 540 
  2. M - 375 / 540 -> 대부분 iphone, glaxy ..
  3. S - 320 / 540

3. tablet
  1. 768 / 540