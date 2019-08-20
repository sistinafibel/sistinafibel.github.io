---
layout: post
title: '[Mysql] 우분투에서 MYSQL 처음부터 설치하기'
date: 2019-08-20
author: cloudjun
tags: mysql
---
### MYSQL 처음부터 설치하기

mysql를 설치할때마다 간혹 빼먹는 일이 있어서 문서로 한번에 정리합니다.<br>
간혹 설정 없이 하다가 한글 깨지는거 보면 현타 빡 오는데 한번에 이쁘게 설정해두면 두고두고 과거의 나에게 감사함을 느낄수 있다.

### 설치하기

> 이 문서를 작성하면서 사용한 시스템 환경
>
> ubuntu16.04.1 LTS , Mysql 5.7

모든 패키지 업데이트를 진행해준다.

```shell
> apt-get update
```

패키지 업데이트가 완료되면 mysql을 설치한다

```shell
> apt-get install mysql-server-5.7
```
중간에 디스크 공간사용이 나오는데, 이때 y를 눌러서 설치를 진행한다.
```shell
다음 패키지를 업그레이드할 것입니다:
libperl5.22 perl perl-base perl-modules-5.22
4개 업그레이드, 24개 새로 설치, 0개 제거 및 133개 업그레이드 안 함.
27.5 M바이트 아카이브를 받아야 합니다.
이 작업 후 165 M바이트의 디스크 공간을 더 사용하게 됩니다.
계속 하시겠습니까? [Y/n] y

```

설치중 두번 root 비밀번호를 설정하라고 나오는데 비밀번호를 설정한다.

설치가 완료되면 정상적으로 접속되는지 mysql 에 들어가보자.

```shell
> mysql -u root -p 
> 설치한 비밀번호입력
```

정상적으로 접속되면 exit 명령어를 사용하여 나오고 이후 utf8 설정을 진행한다.

------

### Mysql 기본 설정하기 (UTF8 설정)

처음 mysql을 설치하고 status를 해보면 서버의 캐릭터 셋이 utf8을 사용하지 않고 있는것을 확인 할 수 있다.
한글을 사용하기 위해서는 utf8 설정을 해줘야하는데 설치한 기본 경로로 들어가보자.

```shell
> cd /etc/mysql/conf.d
//혹시 못찾겠으면 find / -name 'mysql.cnf'로 검색한 경로로 들어가자
```

이후 이 경로에서 mysql.d를 수정해야하는데, nano를 사용하거나 vi를 사용하여 열어준다.

```shell
> nano mysql.cnf
```
다음과 같이 추가하여 수정해준다.
```shell
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

이기서 lower_case_table_names = 1은 테이블 대소문자를 구분하지 않게 하는 구문으로 혹시나 대소문자 구문이 필요하다면 제거해준다.

이후 저장하고 재시작 해준다.

```
> service mysql restart
```

----

### Mysql 기본 설정하기 (네트워크 설정)

처음 mysql를 설정하면 방화벽이나 포트를 추가해줬는데도 외부에서 접속이 안되는 경우가 발생한다.
mysql 처음 설정이 로컬로만 설정이 되어 있어서 그런건데, 다음과 같은 방법으로 풀어 줄 수있다.

```
> cd /etc/mysql/mysql.conf.d 
```

로 이동하여 mysqld.cnf 를 열어준다.

```shell
[mysqld]
#
# * Basic Settings
#
user            = mysql
pid-file        = /var/run/mysqld/mysqld.pid
socket          = /var/run/mysqld/mysqld.sock
port            = 3306
basedir         = /usr
datadir         = /var/lib/mysql
tmpdir          = /tmp
lc-messages-dir = /usr/share/mysql
#skip-external-locking # 이 부분 주석처리 할것

```

그러면 이런 화면을 볼 수 있는데, 여기서 skip-external-locking 을 주석처리한다. 
이 설정이 되어 있다면, 서버는 로컬에서 유닉스 소켓 접속만 허용하게 된다.

이후 바로 아래에 있는 bind-address가 127.0.0.1또는 비활성화 되어 있는지 확인한다.

```shell
# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
bind-address            = 0.0.0.0 # 127.0.0.1로 되어있다면 0.0.0.0로 수정
```

listen과 관련된 TCP/IP 소켓 바인딩을 어느 아이피로 설정할지 결정하는 부분인데, 127.0.0.1는 외부 접속을 허용하지 않는다. 0.0.0.0으로 설정하여 모두가 접속 가능하도록 설정해두자.

이후 적용을 위해 서버 재시작하기

```
> service mysql restart
```

이렇게 하면 기본적인 mysql 설정은 모두 끝난다. 
사용자 권한 관련 문서는 [SQL 사용자 권한 추가, 삭제 부여](https://sistinafibel.github.io/2019/06/05/Mysql-사용자-추가,-삭제,-권한부여.html)를 참조하자



### 참조

http://blog.naver.com/PostView.nhn?blogId=ez_&logNo=140119374985



