---
layout: post
title: '[Mysql] Mysql 5.7 Error this is incompatible with sql_mode=only_full_group_by 오류 해결 방법'
date: 2019-06-08
author: cloudjun
tags: mysql DB
---
실 서버에서는 잘 작동하던 쿼리가 테스트 서버에서는 동작하지 않은체 다음과 같은 오류를 표출하는것을 확인했다.

```mysql
Error this is incompatible with sql_mode=only_full_group_by
```

문제는 Mysql 5.7버전 이상에서 Group By 쿼리를 사용하면 발생하는 오류로
다음과 같이 설정하면 해결 할 수 있다.

--------
### 해결방법 
mysql 설정파일인 my.cnf에서 다음과 같이 수정한다.

```mysql
[mysqld]
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
```

이후 MySql 서버 재시작 진행하면 반영완료.

### 임시 해결방법

```mysql
> set session;
>sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
```

완료.
