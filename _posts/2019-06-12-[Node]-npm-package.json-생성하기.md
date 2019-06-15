---
layout: post
title: '[Node] npm package.json 생성하기'
date: 2019-06-12
author: cloudjun
tags: nodejs
layout: default
comments: true
# other options
---
node.js로 프로젝트를 진행하면 npm 이라는 의존성 관리를 사용하게 된다.
각각의 프로젝트마다 다운로드 받는 묘듈의 버전이 다르기 때문에 문제가 발생 할 수 있는데
이 경우 의존 묘듈에서 버전을 관리해주기 때문에 문제가 발생하지 않는다.

(자바의 경우 Maven을 사용하는데, 의존 정보를 관리한다는 점에서 비슷함!)

자바에서는 의존 정보을 관리할때 XML파일에 묘듈 정보를 관리하나
Node.js는 package.json에 의존 정보를 관리하게 된다.

###  package.json 설정하기
터미널이나 CMD 창에서 다음과 같은 명령어를 입력해주자.
```javascript
> npm init
```

그러면 프로젝트에 대한 내용을 물어보기 시작하는데
관련 내용을 프로젝트에 맞게 작성해준다.

```javascript
#프로젝트 이름 || 입력이 없을시 폴더명을 자동으로 입력해준다.
> package name :

#프로젝트 버전 정보 || default 1.0.0
> version : 

#프로젝트 설명
> description : 

#가장 처음 시작할 프로젝트 파일 || default (가로속 내용)
> entry point : (index.js)

#깃허브 repository(URL)가 있으면 작성해준다.
> git repository : 

#프로젝트 관련 핵심 단어 (키워드)
> keywords : 

#프로젝트 제작자
> author :

#저작권 관련 정보를 기입한다. || default ISC
> license : (ISC)
```

관련 내용을 모두 작성하면 작성한 폴더속에 packge.json이 생성되는것을 확인 할 수 있다.

packge.json는 프로젝트 관리를 위한 모듈의 명세 집합으로
npm init를 사용해도 되지만, 자신이 작성을 하거나 동일한 기능을 포함하고 있는 다른 프로젝트에서 가져와서 간단하게 수정하여 사용해도 무방하다.

