---
layout: post
title: '[Mysql] root 비밀번호 초기화, 계정 비밀번호 변경하기'
date: 2019-06-19
author: cloudjun
tags: mysql
---
처음 mysql을 설치하거나 또는 비밀번호를 매년 바꿔야 하는 경우 다음과 같은 명령어를 사용하면
비밀번호 수정이 가능하다.

```mysql
> use mysql; --mysql 테이블 선택
> Update user SET password = password('바꿀 패스워드') where user='계정아이디';
> FLUSH PRIVILEGES; --변경사항 즉시 적용
```

### 예제 )  mysql root 계정 비밀번호 변경 하기

```mysql
> Update user SET password = password('1234') where user='root';
```

