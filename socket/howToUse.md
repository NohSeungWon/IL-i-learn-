# soket 사용법

---

- font => React + hook
- back => nodejs + express

---

1. Install

```javascript
// front
npm i --save socket.io-client

// back
npm i --save socketio
```

2. 기본 통신 열기

- back :

  socket 통신을 위해서는 http 서버를 생성해야합니다.

  <!-- 일반 restAPI server를 열때는 따로 http를 생성하지 않고 app.listen을 그냥 사용했는데
  http를 사용해야하는 이유, 또는 app.listen을 업그레이드 하는 방법을 추후에 찾아봐야 하겠습니다.
  일단, createServer(app)을 열고나면
  import한 soket으로 server를 업그레이드 시켜줍니다. (업그레이드 시킨다는 표현을 사용하는데 왜 그런표현을 쓰는지는 아직 잘 모르겠습니다.) -->

  http 통신을 따로 열지 않아도 listen 함수를 변수에 담아 socket()을 실행시키면 되네요.

```javascript
// server.js
// http를 사용
import socket from 'socket.io';
const app = express();
const server = http.createServer(app);
const io = soket(server);
server.listen(port, () => { console.log(port, "open success!")});

io.on('connection' socket => console.log('user connection!'));

// http를 사용하지 않음
const express = require("express");
const socket = require("socket.io");
const app = express();
const server = app.listen(4000, () => {
  console.log("4000 open!");
});
socket(server);
```

위 코드까지 완료되면 soket통신에 기본적인 커넥션이 열리게 됩니다.
front에서 통신을 시작하면 user connetion이 로그에 찍히게 됩니다.

- front :

  프론트에서는 접속이 더 간단합니다.
  import한 socket으로 서버에 접속합니다.
  전송버튼을 클릭하면 통신을 시작합니다.

```javascript
import io from "socket.io-client";

const Chat = () => {
  let message;
  return (
    <div>
      <button
        onClick={() => {
          const socket = io("http://localhost:5000/"); // 통신할 server url
          socket.on();
        }}
      >
        전송
      </button>
    </div>
  );
};
export default Chat;
```

3. front에서 메세지 보내기

통신 커넥션이 성공하면 emit이라는 메소드를 이용해 메세지를 보낼 수 있습니다.
메소드 앞에 구분자를 front와 back이 동일하게 맞추면 통신이 이루어지는데
restAPI 사용법과 유사합니다.
일단 front 코드에서 emit 메소드를 작성해보겠습니다.

- front :

다른 부분은 크게 없습니다.
실제 채팅처럼 텍스트를 받기위해 input 태크를 만들었고 작성된 텍스트를 사용하도록 만들었습니다.
그리고 emit이라는 메소드를 사용해, 'user'라는 구분자를 사용해서 server에 보내줍니다.

```javascript
const Chat = () => {
  let message;
  return (
    <div>
      <input
        placeholder="채팅입력"
        ref={e => {
          message = e;
        }}
      />
      <button
        onClick={() => {
          const socket = io("http://localhost:5000/");
          socket.emit("user", message.value); // user를 구분자라고 표현했습니다.
        }}
      >
        전송
      </button>
    </div>
  );
};
```

- back :

  back 부분은 분기처리를 위해 router를 사용했습니다.
  실제 서비스를 운영할 때는 soket서버를 따로 만드는 지는 모르겠지만
  현재 저의 테스트 코드에는 기본적인 CRUD가 가능한 restAPI도 작동하고 있기 때문에
  server.js에서 socket을 전부 다루는 것이 맞지 않아 따로 분리하였습니다.

커넥션 이후로 구분자를 이용해 통신을 하려면 소켓을 이어받아야 하기때문에
('connection', (socekt) => { socket.on('구분자', msg => console.log(msg) )})
router에 soket을 파라미터로 연결해줍니다.
구분자가 잘 맞는다면 front input에 넣은 메세지가 log로 출력됩니다.

```javascript
// server.js
import socketRouter from "../api/controllers/chat/router";

const server = http.createServer(app);
const io = socket(server);
server.listen(port, welcome(port));

io.on("connection", socket => {
  console.log("soket user connetion success");
  socketRouter(socket);
});

// soketRouter.js
export default soket => {
  soket.on("user", msg => {
    // front에서 user와 동일해야 이어받을 수 있습니다.
    console.log("user send =>", msg);
  });

  soket.on("wrong", msg => {
    // 구분자가 이렇게 다르다면 메세지를 받을 수 없습니다.
    console.log("user send =>", msg);
  });
};
```

3. 양방향 메세지 주고받기

클라이언트와 다른 클라이언트의 통신을 지원하는 것이 목적에 더 맞겠지만 추후에 작성해보고
이번 글에서는 fornt에서 메세지를 보내면 지정된 메세지를 server가 받을 수 있도록 해보겠습니다.

- front :

  다른 부분은 전역에 socket 커넥션을 열어놓았습니다. 다른 방법을 사용해도 됩니다.
  back 단에서 구분자를 사용해서 emit메소드를 받았듯이
  front에서도 받는 입장이 되었을 땐 on으로 받습니다.
  잘 작동하면 console에서 서버가 보낸 메세지를 받을 수 있습니다.

```javascript
const socket = io("http://localhost:5000/");
socket.on("sever", msg => {
  console.log("server message =>", msg);
});

const Chat = () => {
  let message;
  return (
    <div>
      <input
        placeholder="채팅입력"
        ref={e => {
          message = e;
        }}
      />
      <button
        onClick={() => {
          console.log("onclick!");
          socket.emit("user", message.value);
        }}
      >
        전송
      </button>
    </div>
  );
};
```

- back :

  user라는 구분자로 msg가 들어오면
  server라는 구분자로 hi user라는 메세지를 보내는 코드입니다.

```javascript
export default soket => {
  soket.on("user", msg => {
    console.log("user send =>", msg);
    if (msg) {
      soket.emit("sever", "hi user");
    }
  });
};
```
