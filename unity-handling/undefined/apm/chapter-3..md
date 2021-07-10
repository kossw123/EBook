# Chapter 3. C\#과 연동

## 1. 목

![](../../../.gitbook/assets/image%20%28280%29.png)

APM 설치로 위와 같은 그림의 청사진이 완성 되었지만,   
Client와 연동 하기 위해서는 또 다른 설정이 필요하다.

1. phpMyAdmin을 통해 Database와 User를 생성
2. 생성한 User에게 생성한 Database에 접근할 권한 부여
3. IDE에서 phpMyAdmin과 연동

위 세가지를 설정하여 최종적으로 하려는 것은

{% hint style="info" %}
IDE에서 phpMyAdmin과 연동하여 결과같은 Console로 확인한다.
{% endhint %}

## 2. phpMyAdmin

주소창에서 localhost와 포트번호를 붙이고 phpmyadmin 폴더로 들어가서 root로 로그인한다.

phpMyadmin에서 csharp이라는 이름의 DB와 csharp\_user라는 user를 생성한다.

{% tabs %}
{% tab title="csharp DB 생성" %}
## phpMyAdmin에서는

왼쪽 목차에서 '새로운' 혹은 New를 클릭하고 생성할 DB 이름 입력후 생성한 DB로 들어가서  
다음과 같은 Query를 입력한다.

```sql
CREATE TABLE IF NOT EXISTS items (
  uid int(11) NOT NULL AUTO_INCREMENT,
  ItemName varchar(100) NOT NULL,
  Price double NOT NULL,
  Quantity int(11) NOT NULL,
  d_regis datetime NOT NULL,
  PRIMARY KEY (uid)
) ENGINE=MyISAM DEFAULT CHARSET=utf8 AUTO_INCREMENT=1;
```

위의 Query를 복사 후 SQL문에 입력하면 Table이 생성된다.  
그리고 아래의 element를 넣어준다.  
element를 입력하는 방법은 똑같이 SQL문에서 입력하면 된다.

```sql
INSERT INTO items (ItemName, Price, Quantity, d_regis) VALUES
('복숭아', 300, 10, '2015-08-27 17:52:17'),
('감자', 44400000, 30, '2015-09-03 22:39:56'),
('배', 333, 20, '2015-09-03 02:23:06'),
('수박', 100, 15, '2015-08-27 17:52:17'),
('자두', 200, 20, '2015-09-03 01:53:55'),
('사과', 20, 10, '2015-09-03 02:07:13'),
('111', 1, 500, '2015-09-04 00:32:29'),
('망고', 555, 40, '2015-09-03 02:23:06'),
('888', 8, 90, '2015-09-03 23:26:15'),
('777', 7, 50, '2015-09-03 02:40:08'),
('222', 2, 2, '2015-09-12 23:30:06'),
('333', 3, 3, '2015-09-12 23:30:06'),
('444', 4, 4, '2015-09-12 23:30:06'),
('555', 5, 5, '2015-09-12 23:34:03'),
('식빵', 3000, 5, '2015-09-12 23:44:39');
```

Table에 element가 입력 됐는지 확

## CMD에서는 

다음과 같은 Query를 입력한다.

```sql
CREATE DATABASE csharp;
GRANT ALL PRIVILEGES ON csharp.items TO csharp_user@localhost;
```
{% endtab %}

{% tab title="csharp\_user 생성" %}
## phpMyAdmin에서는 

phpMyAdmin에서는 기본적으로 설치되어 있는 mysql을 통해 다음과 같이 설정한다.

mysql -&gt; user -&gt; SQL 탭 선택 -&gt; 다음과 같은 sql Query 입력

```sql
CREATE user csharp_user@localhost IDENTIFIED BY '123123';
```

mysql의 user 테이블에 들어가서 csharp\_user가 생성됐는지 확인 후 아래와 같은 Query로 권한부여

```sql
GRANT SELECT, UPDATE, DELETE ON csharp.items TO csharp_user@localhost;
```

이제 phpMyAdmin을 로그아웃하고 csharp\_user로 로그인 후 csharp DB에 접근할 수 있는지 확

## CMD로 하는 방법

mysql -u root - p를 통해 mysql에 접속 후 다음과 같은 Query를 입력

```sql
use mysql
CREATE user csharp_user@localhost IDENTIFIED BY '123123';
Select * from user;
```

위의 Query를 순차적으로 입력 후 맨 마지막 Query 입력후 나오는 Table에서 csharp\_user 확

그리고 해당 User에 권한 부여

```sql
GRANT SELECT, UPDATE, DELETE ON csharp.items TO csharp_user@localhost;
```
{% endtab %}
{% endtabs %}





