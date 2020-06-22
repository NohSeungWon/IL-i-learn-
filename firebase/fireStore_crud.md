# fireStore CRUD 사용법
- - -
* 구글의 docs는 정말 잘 되어있는 것인지 의문이다.
* 항상 무언가를 찾으려하면 굉장히 오래 그리고 여러번 다른 곳을 들어가야한다.
* fireStore도 결국엔 데이터베이스인데.. 내가 다르게 생각하는지 모르겠지만
* 디비이면 CRUD가 기본인데 그걸 찾기가 꽤나 어렵다. 적어놓고 여기서 봐야겠다.
- - -

0. init

```javascript
  import firebase from 'firebase/app';

  const firebaseConfig = {
    apiKey: ''
    authDomain: '',
    databaseURL: '',
    projectId: '',
    storageBucket: '',
    messagingSenderId: '',
    appId: '',
    measurementId: ''
  };

  firebase.initializeApp(firebaseConfig);

  export defautl firebase;
```

1. create
```javascript
  const NOTICE = 'url/'; 
  const firestore = await firebase.firestore(); 
  /*
    * fireStore은 collection / docs / field 3 구성요소로 되어있다.
    * collection 카테고리 하위에 docs가 있고 docs 하위에 field (data) 가 있다.
  */
  await firestore.collection(NOTICE) // 콜렉션 가져오고
    // 여기서 헷갈렸는데 set이 예제에 많아 헷갈렸는데 set은 그냥 계속 초기화를 시킨다.
    // add는 문서를 자동으로 생성하고 그 아래 데이터를 넣어준다.
    .add('data').catch((err) => {});
```


2. get
```javascript
  const NOTICE = 'url/'; 
  const result = [];
  const firestore = await firebase.firestore(); 
  await firestore.collection(NOTICE)
  .get() // get 메소드사용해서 
  .then(snapshot => { // 스냅샷 들고오고 
        if (snapshot.empty) {} // 비어있으면 이렇게 사용한다.
        snapshot.forEach(datas => { // 순회로 값을 주는데
          result.push({ 
            id: data.id, // id 는 문서의 제목이다. 이것으로 수정 삭제가 가능하기에 저장하는게 좋다.
            data: datas.data() // data는 함수로 가져온다.
      })
    })
  }).catch(err => {});
  res.json({ data: result })
```

3. update 
```javascript
  const firestore = await firebase.firestore();
  const Ref = await firestore.collection('url/').doc('문서제목');
  await Ref.update('내용').catch(err => {});
```


4. delete
```javascript
  const firestore = await firebase.firestore();
  const Ref = await firestore.collection('url/').doc('문서제목');
  await Ref.delete().catch(err => {});
```


