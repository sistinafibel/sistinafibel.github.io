---
layout: post
title: '[Node] ESlint를 Typescript에 도입하기'
date: 2021-03-25
author: cloudjun
tags: node eslint typescrit
---

![_2021-03-25__5 21 44](https://user-images.githubusercontent.com/36251104/112447676-efa24f00-8d94-11eb-8ff4-06f133a0b2cb.png)

ESlint는 Javascript / Typescript의 정적 분석 도구로 문법적인 문제나 안티패턴등 코드에서 사용하지 말아야 할 패턴을 먼저 파악하고 수정해주는 역활을 담당한다. 특히나 ESlint의 효과는 두명 이상 개발할때 빛을 발하는데  혼자 또는 토이 프로젝트로 개발하는 경우 내가 날 알고 과거의 나도 날 알기 때문에 코드로 떡을 쩌놔도 과거의 나를 납득 할 수 있겠지만  ~~(과거의 나를 잡아 올 수도 없고 어쩌겠어)~~ 협업의 경우 코드 스타일이나 변수명 생성 규칙등 단순하지만 맞추지 않으면 프로젝트가 산으로 갈 위험이 있기 때문에 ESlint는 그 부분을 원천적으로 막아주는 역활을 담당 한다고 볼 수 있다. 

당연히 개안적으로도 사용하는게 매우 최고다. 코드를 간결하고 안티패턴을 막아준다는 점, 내가 놓친 부분을 깔끔하게 정리해준다는 점에서 이제 더이상 ESlint는 자바스크립트 개발자라면 필수로 적용시켜야하는 플러그인으로 자리 잡았다.

## 1. 설치하기

ESlint와 함께 사용할 플러그인을 모두 설치해준다. eslint가 플러그인을 통해 typescript를 지원하니 더이상 deprecated 된 TSlint를 사용할 필요는 없다.

 

```tsx
// 묘듈내에 typescript 설치
$ npm install typescript
 
// 기본 ESLINT 설치
$ npm install eslint 

//플러그인 임포트를 도와주는 eslint 묘듈 설치
$ npm install eslint-plugin-import

// TypeScript 적용을 위해 필요한 ESlint 플러그인 설치
$ npm install @typescript-eslint/eslint-plugin @typescript-eslint/parser

// airbnb에서 사용하는 코드 규칙인 airbnb-base 설치
$ npm install eslint-config-airbnb-base
```

설치하는게 많지만 한번 설치해두면 제대로된 환경을 만들 수 있으니 깔끔하게 설치해주자.

## 2. 설정하기 (config file 생성)

ESlint 설정을 위해 최상위 폴더에 .eslintrc 라는 파일을 생성한다.

```tsx
{
	//typescript를 위한 parser 설정
  "parser": "@typescript-eslint/parser",
	"plugins": ["@typescript-eslint"],
  "extends": [
		"airbnb-base",
		"plugin:@typescript-eslint/recommended"
	],// 여기까지가 기본 룰 이후는 각자 세팅
  "parserOptions": { // 파서 옵션 
    "ecmaVersion": 2018, //사용할 ECMAScript 버전
    "sourceType": "module" // parser의 export 형태
  },
	"env": { //전역번수 선언 jest를 위해 추가 import 없이 쓰는 명령어들
	    "jest": true
	},
	"rules": { //내가 필요없는 룰 설정
    "no-unused-vars": "off",
    "max-len": "off",
    "import/extensions": "off",
    "class-methods-use-this": "off",
    "max-classes-per-file": "off",
    "import/prefer-default-export": "off",
    "import/no-unresolved": "off",
    "import/no-extraneous-dependencies": ["error", {"devDependencies": ["**/*.test.ts", "**/*.test.js"]}],
    "@typescript-eslint/no-unused-vars": "off",
    "@typescript-eslint/explicit-member-accessibility": 0,
    "@typescript-eslint/explicit-function-return-type": 0,
    "@typescript-eslint/no-parameter-properties": 0,
    "@typescript-eslint/interface-name-prefix": 0,
    "@typescript-eslint/explicit-module-boundary-types": 0,
    "@typescript-eslint/no-explicit-any": "off"
  }
}
```

## 3. package.json에 추가하기

script 부분에 lint , lint:fix를 위한 기능을 넣어주면 기본적인 eslint 설정이 끝이다.

```tsx
...
"scripts": {
    "lint": "eslint src/**/*.ts",
    "lint:fix": "eslint src/**/*.ts --fix"
  },
...
```

이제 콘솔창에 실제 명령어를 넣어 ESlint를 돌려보자

```tsx
## lint 문법 오류 확인
$ npm run lint

## lint 문법 수정 가능한건 알아서 수정  
$ npm run lint:fix 
```

---

## 참조

글 잘 쓰는 멋진사람들

[https://chanyeong.com/blog/post/17](https://chanyeong.com/blog/post/17)

[https://velog.io/@das01063/VSCode에서-ESLint와-Prettier-TypeScript-사용하기](https://velog.io/@das01063/VSCode%EC%97%90%EC%84%9C-ESLint%EC%99%80-Prettier-TypeScript-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)

[https://velog.io/@xortm854/Typescript-React-Eslint-환경설정-2편-ESLint-Prettier-설정](https://velog.io/@xortm854/Typescript-React-Eslint-%ED%99%98%EA%B2%BD%EC%84%A4%EC%A0%95-2%ED%8E%B8-ESLint-Prettier-%EC%84%A4%EC%A0%95)

[https://velog.io/@kyusung/eslint-config-2](