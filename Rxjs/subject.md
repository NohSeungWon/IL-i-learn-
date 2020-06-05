# Subject multicast
rxjs의 구독개념에서 
observable은 하나만 구독할 수 있다.
하지만 subject를 사용하면 여러 곳에서 동일한 구독을 수행할 수 있게된다.

환경은 Angular8 
service에서 Observer패턴을 관리하는 것으로 가정
- - - 

## 1. Service.ts
```javascript
// service 
import { Subject } from 'rxjs';

class ObserverService {
  multiSubject : new Subject<any>;
  constructor() {}

  getSubject = () => {
    return this.multiSubject
  }

  inintSubject = () => {
    this.multiSubject =  new Subject<any>();
    // 로직 수행 
    this.multiSubject.next(true);
    if (err) this.multiSubject.next(false);
  }
}
```

## 1. ObserverComponent.ts
```javascript
// service 
import ObserverService from'ObserverService';
import { Subscribe } from 'rxjs';

class ObserverService {
  observer : Subscribe;
  constructor(private observerService: ObserverService ) {}

  ngOninit() : void {
    this.observer = this.observerService.getSubject(); // 구독 변수담기
  }

  textColorRedChange = ():void => {
    this.observer.subscribe((data: any) => {
      // data는 ObserverService에 next 함수로 오는 true
      // true면 red로 색상변경
      if (data) // 로직수행
      else // 예외처리
    })
  }

  textColorBlueChange = ():void => {
    this.observer.subscribe((data: any) => {
      // data는 ObserverService에 next 함수로 오는 false다.
      // false면 blue로 색상변경
      if (!data) // 로직수행
      else // 예외처리
    })
  }
}
```

* 위 처럼 하나의 observer를 두개의 함수가 공유하고 있다.