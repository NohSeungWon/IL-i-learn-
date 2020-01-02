# Form에 대한 이해
- - -
여러모로 angular에서 Form 포멧을 사용하는 것은 편리하다.
간단한 Form의 경우에는 내장되어있는 Form 모듈을 impot하는 것만으로 사용자의
동적인 값을 그대로 받을 수 있다.
하지만, checkBox 또는 selectBox같이 여러가지의 값들을 처리해야하는 경우는 단순히
모듈을 사용하는 것을 넘어 코드를 작성해야한다. 
- - -

  
1. Html init
------
 - 기본 세팅
 - js 코드를 입혔을 때의 변화를 잘 체크하면 좋다.

  ```javascript
    <div class='container'>
      <form [formGroup]="sendForm">
        <div class="row justify-content-center">
          <p style="margin-right: 10px;">취미를 선택하세요</p>
          <label formArrayName='etcCheck'>
            <input type='checkbox' [formControlName]="i">
            축구
          </label>
          <label formArrayName='etcCheck'>
            <input type='checkbox' [formControlName]="i">
            농구
          </label>
          <label formArrayName='etcCheck'>
            <input type='checkbox' [formControlName]="i">
            야구
          </label>
          <label formArrayName='etcCheck'>
            <input type='checkbox' [formControlName]="i">
            배구
          </label>
        </div>
        <div class="row justify-content-center">
          <button class="col" type="submit" class="btn btn-success">제출</button>
        </div>
      </form>
    </div>
  ```


2. ts Code
------
 - FormArray, FormBuilder, FormControl, FormGroup
 - Form(Boolean)과 사용자에게 보여줄 값(strtig)들을 1:1 매칭시켜
 - 사용자가 제출을 눌렀을 때 체크한 것들만 추려서 받을 수 있도록 만들것이다.

  ```javascript
  import { Component, OnInit } from '@angular/core';
  import { FormArray, FormBuilder, FormControl, FormGroup } from '@angular/forms';

  export class AppComponent implements OnInit {
  
  sendForm: FormGroup;
  // 사용자에게 보여줄 값
  etcCheckVal = [
    '축구','농구','야구','배구'
  ];

  constructor(private fb: FormBuilder) {}

    ngOnInit() {
      this.initSendForm();
      this.addEtcCheckbox();
    }

    initSendForm() { // 폼을 세팅
      this.sendForm = this.fb.group({
        etcCheck: new FormArray([])
      });
    }

    addEtcCheckbox() { // 세팅 된 폼에 boolean값 init
      this.etcCheckVal.forEach(etcs => {
        const control = new FormControl(false);
        (this.sendForm.controls.etcCheck as FormArray).push(control);
      });
    }


    isEtcCheck() { // true 즉 사용자가 체크한 값들만 추려주는 함수
      const valueArr = this.sendForm.get('etcCheck').value; // 불리언 값들이 저장되어 있다.
      const result = [];
      valueArr.forEach((etcs, index) => {
        if (etcs) {
          result.push(this.etcCheckVal[index].value);
        }
      });
      return result;
    }

    onSubmit() { // 제출을 눌렀을 때 함수
      console.log("어느 것이 선택되었나", this.sendForm.value);
      console.log("선택된 값은 무엇인가", this.isEtcCheck());
    }
  }
  ```

3. Html + Angular
------
 - form 태그를 angular 코드의 sendForm과 바인딩 시킴
 - 각각의 취미들은 formArrayName인 etcCheck로 바인딩
 - formControlName을 index 값으로 순서를 지정
 - etcCheckVal 변수를 ngFor함수를 통해 html 코드상에서 실행하며 동적 생성

  ```javascript
    <div class='container'>
      <form [formGroup]="sendForm" (ngSubmit)='onSubmit()'>
        <div class="row justify-content-center">
          <p style="margin-right: 10px;">취미를 선택하세요</p>
          <label formArrayName='etcCheck' *ngFor="let item of etcCheckVal; let i = index">
            <input type='checkbox' [formControlName]="i">
            {{item}}
          </label>
        </div>
        <div class="row justify-content-center">
          <button class="col" type="submit" class="btn btn-success">제출</button>
        </div>
      </form>
    </div>
  ```