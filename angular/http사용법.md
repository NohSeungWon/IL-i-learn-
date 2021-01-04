# Angular Http module 사용법

- - -
Angular에서는 http 통신을 하기위해 내장 module을 제공한다. 
사용법이 그렇게 어렵진 않았었는데, Angular 11버전에서 사용법이 좀 쉽게 이해가 가지 않아 남긴다.

* Angular vervsion 11

- - -

1. module 등록 
사용할 module.ts에 HttpClientModule을 import 해준다.
```javascript
  import { NgModule } from '@angular/core';
  import { CommonModule } from '@angular/common';
  import { HttpClientModule } from '@angular/common/http';

  @NgModule({
    declarations: [component....],
    imports: [
      CommonModule,
      HttpClientModule
    ],
    exports: [component....],
  })
  export class MainModule { }
```

2. httpService.ts 생성
이부분이 이해가 제일 안가는데 catchError와 throwError를 import 해주는 방식을해야
통신응답을 잘 받을 수 있다. 
여기서 .pipe를 안쓰고 subscribe를 써버리면 service를 사용하게 되는 부분에서 subscribe를 못쓰게 되기 때문에
위 같은 로직을 써야하는 것 같다.

```javascript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { catchError } from 'rxjs/operators';
import { throwError } from 'rxjs';
@Injectable({
  providedIn: 'root'
})

export class PlayerService {
  
  constructor(private http: HttpClient) { }

  getSometing () {
    return this.http.get(END_POINT).pipe(
      catchError(err => {
        this.errorHandlerService.acceptErr(err);
        return throwError(''); // * 파라미터 '' 가 서비스를 구독하는 곳에서 err로 받아짐
      })
    )
  }
  
}

```

3.component에서 service 등록

```javascript
import { PlayerService } from './../../../service/player.service';
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-player',
  templateUrl: './player.component.html',
  styleUrls: ['./player.component.scss']
})
export class PlayerComponent implements OnInit {

  lists = [{}];
  addMinusBtnisHide = false;

  constructor(private playerService: PlayerService) { }

  ngOnInit(): void {
    this.playerService.getList().subscribe(
      res => console.log('HTTP response', res),
      err => console.log('HTTP Error', err), // * throwError()가 실행되면 이곳에 받아짐
      () => console.log('HTTP request completed.')
    );
  }

}

```