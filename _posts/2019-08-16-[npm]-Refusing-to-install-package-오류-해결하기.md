---
layout: post
title: '[npm] Refusing to install package 오류 해결하기'
date: 2019-08-16
author: cloudjun
tags: npm 오류해결
---
최근 예제 프로젝트를 만들어보며 경험했던 문제인데, 
정상적으로 npm에 등록되어 있는 프로젝트가 다음과 같은 오류를 내며 설치가 되질 않았다.

```javascript
npm ERR! code ENOSELF
npm ERR! Refusing to install package with name "graphql" under a package
npm ERR! also called "graphql". Did you name your project the same
npm ERR! as the dependency you're installing?
npm ERR!
npm ERR! For more information, see:
npm ERR!     <https://docs.npmjs.com/cli/install#limitations-of-npms-install-algorithm>

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\null\AppData\Roaming\npm-cache\_logs\2019-08-16T01_16_25_945Z-debug.log

```

### 문제점 해결

관련 내용을 구글링해보니 https://github.com/npm/npm/issues/18143 여기서 힌트를 얻을 수 있었는데

> Is the `"name"` property in your `package.json` set to `"graphql"` by any chance? If so, could you try changing it to something more unique?

살펴보니 이 오류는 npm 프로젝트가 등록되지 않았을경우도 발생하지만, install 하는 폴더에 있는 package.json에서 name을 설치할 패키지명과 똑같이 작성했을때 발생한다. package.json에서 name만 다른 이름으로 변경해주면 해결.

### 후기

댕청했다..

