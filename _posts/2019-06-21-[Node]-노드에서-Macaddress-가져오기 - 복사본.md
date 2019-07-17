---
layout: post
title: '[Node] 노드에서 Macaddress 가져오기'
date: 2019-06-21
author: cloudjun
tags: node
---
Node에서 맥 어드레스를 확인 해야하는 일로 macaddress 묘듈을 찾았는데, 
너무 만족스러워서 블로그에도 같이 포스팅 합니다.

Linux , Mac , Windows 및 대부분의 UNIX 시스템에서 사용 가능!

### 설치법

node.js에서 macaddress를 사용하기 위해 "node-macaddress"를 설치합니다.

```javascript
> npm install node-macaddress
```

사용 준비 완료!

### Node-Macaddress 사용법

묘듈을 사용하기전 최상단에 node-macaddress를 선언합니다.

```javascript
//최상단에 node-macaddress를 불러옵니다.
var macaddress = require('node-macaddress');
```

#### .one([iface] , callback)
비동기 방식으로 컴퓨터의 맥 주소를 단일로  가져옵니다.

```javascript
macaddress.one(function (err , mac){
   console.log(`MacAddress 주소 : ${mac}`);
});
```
#### .all(callback)
비동기 방식으로 컴퓨터 내부에 있는 모든 맥주소와 아이피(Ipv6 , Ipv4)를 가져옵니다.
```javascript
macaddress.one(function (err , all){
   console.log(JSON.stringify(all, null, 2));
});
```
#### .networkInterfaces()

동기 방식으로 컴퓨터 내부에 있는 모든 맥주소와 아이피(Ipv6 , Ipv4)를 가져옵니다.
```javascript
console.log(JSON.stringify(macaddress.networkInterfaces(), null, 5));
```

