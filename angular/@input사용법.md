# @input 사용법
- - -
프론트작업을 하다보면 재사용되는 컴포넌트들을 만나게 됩니다.
디자인이 같아서 단순히 컴포넌트를 불러오기만 하면 될수도 있으나,
페이지마다 재사용되는 컴포넌트의 색상이나 사이즈가 다를 경우가 많죠.
그떄 Angular의 input을 사용하면 동적으로 편하게 바꿀 수 있습니다.
- - -

작업순서
------
작업순서는 크게 2가지로 나누겠습니다.

 - 하위컴포넌트를 사용하기 위한 작업 
  1. 상위컴포넌트 declarations에 하위컴포넌트 선언
  2. 상위컴포넌트 Html파일에 하위 컴포넌트 Tag 선언

 - Input으로 하위컴포넌트에 데이터 전달하기 위한 작업 
  3. 상위컴포넌트에 하위컴포넌트로 넘겨줄 데이터 선언
  4. 상위컴포넌트에서 넘겨줄 데이터를 하위컴포넌트 Tag에 데이터 얹기
  5. 하위컴포넌트에 Input import / @Input으로 데이터 바인딩
  6. 바인딩 된 데이터로 원하는 동적 구성

폴더구조
------
- 상위컴포넌트: App / 하위컴포넌트: Parent


작업
------
 - 최소한의 코드들로만 적어서 생략된 코드들이 존재합니다.
 - 하위컴포넌트의 텍스트 색상을 부모로부터 넘겨받은 색상으로 변경하는 예제입니다.

  1. 상위컴포넌트 declarations에 하위컴포넌트 선언

    ```javascript
    // 컴포넌트 import
    import { ParentComponent } from './parent/parent.component';

    @NgModule({
      declarations: [
        AppComponent,
        ParentComponent // 쓰겠다고 선언
      ]
    })
    export class AppModule {}
    ```

  2. 상위컴포넌트 Html파일에 하위 컴포넌트 Tag 선언

   * 하위컴포넌트의 selector를 Tag형태로 만들어 얹습니다.

    ```javascript
    // 하위 컴포넌트.ts

    @Component({
      selector: 'app-parent', // 이부분을 가져다 씁니다.
      templateUrl: './parent.component.html',
      styleUrls: ['./parent.component.css']
    })
    export class ParentComponent implements OnInit {}

    // 상위컴포넌트 Html
    <h1>나는 부모 컴포넌트 h1 태그</h1>
    // 아래처럼 사용하고자 하는 곳에 선언해서 사용합니다.
    <app-parent></app-parent>
    ```

  3. 상위컴포넌트에 하위컴포넌트로 넘겨줄 데이터 선언

   * 가장 상위에 선언해 줍니다. 

    ```javascript
    export class AppComponent {

    fromParentColor = 'red';  
    constructor() {}
    
    }
    ```

  4. 상위컴포넌트에서 넘겨줄 데이터를 하위컴포넌트 Tag에 데이터 얹기

   * [fromParentColor]는 5.에 나오는 하위 컴포넌트의 @Input() fromParentColor 이름과 동일하게 맞워야합니다.
   * 'fromParentColor'는  3.에서 선언된 데이터를 의미합니다.

    ```html
    <h1>나는 부모 컴포넌트 h1 태그</h1>
    <app-parent [fromParentColor]='fromParentColor'></app-parent>
    ```

  5. 하위컴포넌트에 Input import / @Input으로 데이터 바인딩

   * '@angular/core'에 Input을 import해야 데이터 바인딩을 사용할 수 있습니다.

    ```javascript
    import { Component, OnInit, Input } from '@angular/core'; // Input을 import

    export class ParentComponent implements OnInit {

      @Input() fromParentColor: string; // 상위컴포넌트의 값과 매칭

      constructor() {}

      ngOnInit() {
        console.log(this.fromParentColor); // 로그를 찍어보면 나오는 것을 볼 수 있습니다.
      }
    }

    ```

  6. 바인딩 된 데이터로 원하는 동적 구성

   * 동적구성하는 방법은 여러가지가 있으니 원하는데로 구현해보시면 좋을 것 같습니다. 
   * 

  ```javascript
  export class ParentComponent implements OnInit {

    @Input() fromParentColor: string;

    constructor() {}

    ngOnInit() {
      this.changeColor();
    }

    changeColor() {
      const pTagDiv = document.getElementById('pTag');
      pTagDiv.style.color = this.fromParentColor;
    }
  }
  ```

Pull Code 
------

```javascript
// app.module.ts 

import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { ParentComponent } from './parent/parent.component';

@NgModule({
  declarations: [
    AppComponent,
    ParentComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

// app.component.ts

import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})

export class AppComponent {

  fromParentColor = 'red'; 
   
  constructor() {}

}

// parent.component.ts
import { Component, OnInit, Input } from '@angular/core';

@Component({
  selector: 'app-parent',
  templateUrl: './parent.component.html',
  styleUrls: ['./parent.component.css']
})

export class ParentComponent implements OnInit {

  @Input() fromParentColor: string;

  constructor() {}

  ngOnInit() {
    this.changeColor();
  }

  changeColor() {
    const pTagDiv = document.getElementById('pTag');
    pTagDiv.style.color = this.fromParentColor;
  }
}


// app.component.html
<h1>나는 부모 컴포넌트 h1 태그</h1>
<app-parent[fromParentColor]='fromParentColor'></app-parent>

// parent.component.html
<p id='pTag'>난 하위 컴포넌트의 p Tag</p>

```
