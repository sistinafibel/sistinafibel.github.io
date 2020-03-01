---
layout: post
title: '[Mysql] 한글 입력 오류 해결하기 ERROR 1366 (incorrect string value ~~ )'
date: 2020-02-29
author: cloudjun
tags: Mysql
---
새로 mysql을 설치하고 따로 한글 환경을 설정하지 않은 상태에서 발생한 오류였다.<br>아래와 같은 설정을 해주면 추후 만드는 테이블은 문제가 발생하지 않지만, 이미 있는 테이블은 그대로 유지가 되기 때문에 하단 명령어도 같이 입력해줘야한다.

```
Ubuntu 18.04.4 LTS / 5.7.29-0ubuntu0.18.04.1
```

----

### 설정 변경

/etc/mysql/conf.d

```ini
[mysqld]
character-set-server=utf8
collation-server=utf8_general_ci
init_connect = set collation_connection = utf8_general_ci
init_connect = set names utf8
lower_case_table_names = 1

[mysql]
default-character-set=utf8

[mysqld_safe]
default-character-set=utf8

[client]
default-character-set=utf8

[mysqldump]
default-character-set=utf8
```

----

### 이전 테이블 설정

```mysql
ALTER TABLE univerlist CONVERT TO CHARSET utf8
```

----

이후 리스타트 시켜주면 정상적으로 적용된걸 확인 할 수 있다.

```
service mysql restart
```