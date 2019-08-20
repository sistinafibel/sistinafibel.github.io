---
layout: post
title: '[ASP] newobjects.utilctls.VarDictionary 묘듈 호출 실패'
date: 2019-06-14
author: cloudjun
tags: asp iis
---
서버 OS에 문제가 발생하여 긴급하게 IIS 서버를 이전하는 작업을 진행하던 도중 ASP 묘듈오류가 발생하며 newobjects.utilctls.VarDictionary 묘듈을 불러올 수 없다는 내용을 확인했다.

### 해결 방법
이 경우 Newobjects에서 제작한 ActiveXPack1을 설치하면 해결되는데
[다운로드 링크](http://www.newobjects.com/product.asp?Category=63) 에서 
Download(SFX 1.8MB PC/PPC) 를 설치해주자.

### 여담
생소한 묘듈이라 구글링을 해보니 레퍼런스 페이지만 뜨고
따로 묘듈 관련 문제에 대한 게시글은 없었다. 혹시 나와 비슷한 삽질을 한 사람에게 단비같은 게시글이 되길 바란다.

