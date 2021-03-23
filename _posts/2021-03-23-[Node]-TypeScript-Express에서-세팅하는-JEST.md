---
layout: post
title: '[Node] TypeScript Express에서 세팅하는 JEST'
date: 2021-03-23
author: cloudjun
tags: node jest express
---
언제까지 포스트맨으로 하나하나 눌러보며 테스트를 하는건 불안함의 연속이다. 실제 라이브가 되는 서비스일수록 기능은 매우 많았고 로직의 변화도 어디에 문제가 생길지 모르기 때문에 보수적으로 유지보수를 할 수 밖에 없다. 새로운 패턴으로 고도화를 적용 시키려 해도 이미 잘 돌아가는 서비스를 건든다는건 기술부채의 폭탄을 해체하는 것과 비슷하다보니 결국 해체해야하는 폭탄을 더욱 쉽게 해체하고자 TDD를 도입하자 마음먹었다.

![Untitled](https://user-images.githubusercontent.com/36251104/112114905-f184da80-8bfb-11eb-804e-88c140d65c4e.png)

> 폭탄을 해체하는 도구를 얻는다 해도 그건 도움을 주는 도구일 뿐 터지지 않는다는 소리가 아니다. TDD에서 예상하지 못하는 예외상황은 분명히 있다.

그리고 이 글을 쓰게 된 핵심적인 이유는 TypeScript + Express + JEST를  모두 설정하는 글이 별로 많지 않았다. 

구 버전 글도 많았고.. 이상하게 설치하는데 삽질을 하다보니 설정하는 과정을 정리한다.

## 👀 왜 JEST 인가?

~~둘다 똑같은 여우인데~~

![Untitled 1](https://user-images.githubusercontent.com/36251104/112114883-ec279000-8bfb-11eb-895d-00f6e42959d1.png)

이 짤이 참 마음에 드는 이유인데, 결국 어차피 테스트 도구는 성능에 이슈를 미치는게 아니니 자신에게 편한거 쓰면 된다. 

국내 레퍼런스는 MOCHA 자료보다는 JEST 자료가 많았고 테스트 기능에 충실한 JEST를 선택하게 되었다.

어차피 테스트를 하는 툴이니 편한거 쓰면 된다.

## ⏳ JEST 설치하기

jest는 테스트를 담당하는 에셋으로 우리는 typescript를 포함하여 사용하니 ts-jest도 같이 받는다.

supertest는 request 통신을  만들어주는 역활을 하는데, express 테스트를 위해 필요하다.

```tsx
npm install jest ts-jest @types/jest supertest
```

jest.config.js 파일을 프로젝트 최상단에 만들어주고 다음과 같은 내용을 작성하여 jest가 ts-jest를 사용하도록 알려주자

```tsx
module.exports = {
  preset: "ts-jest", // 이 부분에서 ts-jest를 사용한다고 알려준다
  testEnvironment: "node", //테스트 환경 'node' 환경을 사용한다 알려줌
  testMatch: ["**/tests/*.test.(ts|tsx)"] //js 파일은 dist에서도 감지가 될 수 있으니 폴더를 조정해서 test이 있는 위치로 잡아준다. 
};
```

추가 설정은 [https://jestjs.io/docs/configuration](https://jestjs.io/docs/configuration) 여기에서 확인 할 수 있다.

이후 package.json을 수정해서 jest를 사용 할 수 있게 설정한다.

```tsx
"scripts": {
  "test": "jest --setupFiles dotenv/config --forceExit --detectOpenHandles"
}
```

> 🔥 혹시 env 파일을 사용중이라면 jest —setupFiles dotenv/config를 꼭 넣어서 사용해야한다. 안하면 환경설정을 가져올 수가 없다

env 문제로 시간을 많이 허비했는데 다른곳에서는 안 알려주는 꿀팁이고 이거 관련된 오류도 안난다.. 다른 오류로 표시되서 사람은 모르니깐 꼭 env를 사용하면 dotenv/config 도 불러오도록 설정해주자. 

EXPRESS 또는 비동기 NODE APP은 JEST 테스트 실행이 모두 완료되어도 비동기가 남아있는 경우 그대로 jest가 종료되지 않는데 detectOpenHandles 옵션은 열려있는 핸들을 모두 수집하고 forceExit 옵션은 테스트가 끝나면 강제 종료하라는 의미의 옵션이다.

이걸로 기본적인 테스트 환경설정은 모두 끝났다.

## 🧑‍💻 실제 테스트 코드 작성해보기

test를 작성하는 파일은 파일명.test.ts 형식으로 작성하면 된다.

ex) 유저 테스트를 하는 경우 user.test.ts 로 파일명 짓기 

describe : 여러개의 테스트 케이스를 묶어서 가독성을 높인다.

test / it : 테스트 단위

Methods LIST : 전역에 쓰면 모든 테스트가 적용되고 묶은 describe 속에 넣으면 그 속에 있는 test에만 methods가 적용된다.

- afterAll(fn, timeout) : 모든 테스트가 끝나고 실행된다
- afterEach(fn, timeout) : 하나의 테스트가 끝날 때마다 실행된다
- beforeAll(fn, timeout): 모든 테스트가 시작하기 전에 한번 실행된다.
- beforeEach(fn, timeout): 하나의 테스트가 시작하기 전에 매번 실행한다

```tsx
describe('테스트 단위', () => {
    beforeAll(() => {
      ...
    });

    afterAll(() => {
     ...
    });

    test('[GET] /user', (done) => {
      ...
    });
});
```

src/tests/user.test.ts

```tsx
import request from 'supertest';
import App from '../app';
import AuthRoute from '../routes/auth.route';
import BoardRoute from '../routes/board.route';
import { CreateUserDto } from '../dtos/users.dto';

afterAll(async () => {
  await new Promise<void>((resolve) => setTimeout(() => resolve(), 500));
});

//스코프를 공유해서 가독성 높이기 (테스트 케이스 묶기)
describe('Testing Auth', () => {
  describe('서버 상태 확인', () => {
    test('기록 체크', async (done) => {
      const reoute = new BoardRoute(); //라우트 정보 가져오기
      const app = new App([reoute]); //
      const response: any = await request(app.getServer()).get('/board/free'); //리퀘스트 요청

      // console.log(response);
      expect(response.statusCode).toBe(200);
      done();
    });
  });

  describe('[POST] /login', () => {
    it('testsetestestestsetest', async () => {
      const userData: CreateUserDto = {
        id: 'test',
        password: 'tset',
      };

      const reoute = new AuthRoute();
      const app = new App([reoute]);

      return request(app.getServer())
        .post(`${reoute.path}/login`)
        .send(userData)
        .set('Accept', 'application/json');
    });
  });
});
```