# Router
- - - 
계속하면 익숙하지만 안하다가 또 Angular를 만지려면 여지없이 Routing 부분에서 헤메이게 된다.
다시 까먹지 않기 위해 기초 구축법을 적어논다.
- - - 

1. 기초환경
rootModule인 appModule에서 NavComponent와 MainModule을 자식으로가지고 있고,
NavComponent에서 routerLink를 사용해 MainModule의 자식들을 라우팅하는 환경이다.

appComponent구조 -> app-nav와 router-outlet을 갖는다.
  ```javascript
  <div>
    <app-nav>
    <router-outlet></router-outlet>
  </div>
  ```
MainModule구조 -> 자식 컴포넌트를 갖고있다.
  ```javascript
    MainModule
     - oneComponent
     - twoComponent
  ```

2. app.module.ts에 AppRoutingModule import 

  ```javascript
  import { NgModule } from '@angular/core';
  import { AppRoutingModule } from './app-routing.module'; // 추가
  import { AppComponent } from './app.component';

  @NgModule({
    declarations: [AppComponent],
    entryComponents: [],
    imports: [
      BrowserModule, 
      AppRoutingModule // 추가
    ],
    bootstrap: [AppComponent],
  })
  export class AppModule {}
  ```

3. app.routing.module에 경로 설정 
app.module 즉 AppRoutingModule을 import한 곳이라면 MainModule안에 두 Component를 다 사용할 수 있다.
MainModule안에 one,two component가 자식으로 있기 때문이다.

  ```javascript
  import { NgModule } from '@angular/core';
  import { Routes, RouterModule } from '@angular/router';
  import { PageNotFoundComponent } from './page-not-found/page-not-found.component';

  const routes: Routes = [
    {
      path: '',
      loadChildren: () => import ('./component/main/main.module').then(m => m.MainModule) // mainModule 추가
    },
    {path: '**', component: PageNotFoundComponent}
  ];

  @NgModule({
    imports: [RouterModule.forRoot(routes)],
    exports: [RouterModule]
  })
  export class AppRoutingModule { }

  ```

4. Main에 자식컴포넌트 넣기 & RotertModule import 
MainComponent에 자식 컴포넌트 넣기
  ```javascript
    <div>
      <app-one></app-one>
      <app-two></app-two>
    </div>
  ```

MainModule에 RotertModule import 
  ```javascript
  import { NgModule } from '@angular/core';
  import { CommonModule } from '@angular/common';
  import { OneComponent } from './player/player.component';
  import { TwoComponent } from './make-list/make-list.component';
  import { MainComponent } from './main.component';
  import { RouterModule, Routes } from '@angular/router';

  const routes: Routes = [
    {
      path: '',
      redirectTo: 'one'
    },
    {
      path: 'one',
      component: one
    },
    {
      path: 'two',
      component: two
    },
  ]

  @NgModule({
    declarations: [OneComponent, MainComponent, TwoComponent],
    imports: [
      CommonModule,
      RouterModule.forChild(routes)
    ],
    exports: [PlayerComponent, MainComponent],
  })
  export class MainModule { }
  ```

5. Nav 컴포넌트에서 Router-link 사용
  ```javascript
    <div>

      <div>
        <a routerLink="one">one</a>
        <a routerLink="two">two</a>
      </div>

    </div>
  ```