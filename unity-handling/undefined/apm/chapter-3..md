# Chapter 3. C\#과 연동

## 1. 목적

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

## 2. DB 설정, 권한 부여 

주소창에서 localhost와 포트번호를 붙이고 phpmyadmin 폴더로 들어가서 root로 로그인한다.

phpMyadmin에서 csharp이라는 이름의 DB와 csharp\_user라는 user를 생성한다.

{% tabs %}
{% tab title="csharp DB 생성" %}
## phpMyAdmin에서는

왼쪽 목차에서 '새로운' 혹은 New를 클릭하고 생성할 DB 이름으로 csharp을 입력후   
생성한 DB로 들어가서 다음과 같은 Query를 입력한다.

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

Table에 element가 입력 됐는지 확인 

## CMD에서는 

다음과 같은 Query를 입력한다.

```sql
// DB 생성
CREATE DATABASE csharp;
// DB 목록 확인
show databases;
// 사용할 DB 선택
use csharp
// 테이블 생성
CREATE TABLE IF NOT EXISTS items (
  uid int(11) NOT NULL AUTO_INCREMENT,
  ItemName varchar(100) NOT NULL,
  Price double NOT NULL,
  Quantity int(11) NOT NULL,
  d_regis datetime NOT NULL,
  PRIMARY KEY (uid)
) ENGINE=MyISAM DEFAULT CHARSET=utf8 AUTO_INCREMENT=1;
// items 테이블 element 목록 호
SELECT * FROM items;
```
{% endtab %}

{% tab title="csharp\_user 생성, 권한 부여" %}
## phpMyAdmin에서는 

phpMyAdmin에서는 기본적으로 설치되어 있는 mysql을 통해 다음과 같이 설정한다.

mysql -&gt; user -&gt; SQL 탭 선택 -&gt; 다음과 같은 sql Query를 순차적으 입력

```sql
// User 생성
CREATE user csharp_user@localhost IDENTIFIED BY '123123';

// 생성한 User에 권한부여
GRANT SELECT, UPDATE, DELETE ON csharp.items TO csharp_user@localhost;
```

## CMD로 하는 방법

mysql -u root - p를 통해 mysql에 접속 후 다음과 같은 Query 순차적으로 입력

```sql
// 사용할 DB 선택
use mysql
// User 생성
CREATE user csharp_user@localhost IDENTIFIED BY '123123';
// User 목록 확인
Select * from user;
// 생성된 user에서 권한 부여
GRANT SELECT, UPDATE, DELETE ON csharp.items TO csharp_user@localhost;
// 부여된 권한 확인
SHOW GRANTS FOR TestID@localhost;
```
{% endtab %}
{% endtabs %}

## 3. C\#과 연동

IDE로 Visual studio를 사용하는데, MySQL과 연동 시킬때 필요한 라이브러리가 존재한다.

```csharp
using MySql.Data.MySqlClient;
```

해당 라이브러리는 Nuget Package에서 MySql.Data를 검색하여 설치하면   
위의 라이브러리를 사용할 수 있다.



이젠 코드를 작성하여 MySQL과 연동을 해야하는데, 이때 연결 방법은 3가지가 존재한다.

* C\#에서 직접적으로 DB에 접근
  * 연결형 DB 사용
    * MySqlDataReader Class 사용
  * 비연결형 DB 사용
    * MySqlDataAdapter Class 사용
  * 여기서 연결, 비연결의 차이는 Client에서 지속적으로 DB에 접속하는가 안하는가 차이다.
* C\#에서 간접적으로 DB에 접근
  * php를 사용하여 DB에 접근



기본적으로 연동하기 위해서는 3가지 Class가 필요하다.

* MySqlConnection Class
  * DB에 연결하기 위한 Class
  * Constructor에 URL parameter만 넣었는데 접속이 가능하다.
* MySqlCommand
  * 입력할 Query를 접속한 DB의 Query문에 입력해주는 Class
* MySqlDataReader / MySqlDataAdapter
  * Client와 DB사이에 어떤 연결형태로 데이터를 서버에서 가져올 것인가?를 선택하는 Class

이 3가지 Class를 가지고 작성된 간단한 예시는 다음과 같다.

```csharp
using System;
using MySql.Data.MySqlClient;

static void Main(string[] args)
{
    string connectionString = 
    "SERVER =localhost; DATABASE = csharp; UID = csharp_user; PASSWORD = csharp1234";
    using (MySqlConnection mySql = new MySqlConnection(connectionString)) 
    {
        string sql = "select * from items order by uid desc";
        MySqlCommand cmd = new MySqlCommand(sql, mySql);

        mySql.Open();
        MySqlDataReader r = new MySqlCommand(sql, mySql).ExecuteReader();

        while (r.Read()) 
        {            
            Console.WriteLine(
            $"[uid] : [{r[0].ToString()}], [Name] : [{r[1].ToString()}]"
            );
        }
    }
}
```

using을 사용하여 Dispose하기 때문에, 코드상에서는 보이진 않지만,   
각 Class마다 Close\(\)로 객체를 차단하는 Callback이 있다.

C:/ProgramData/MySQL/Server 8.0/Data/csharp에 들어가보면   
items Table이 파일형식으로 작성되어 있는 것을 확인할 수 있다.

이 table 파일을 while문을 사용하여 한줄씩 읽어와서, Console로 출력하는 것 까지가   
해당 예시 코드의 마지막이다.

![&#xACB0;&#xACFC; &#xD654;&#xBA74;](../../../.gitbook/assets/image%20%28281%29.png)



## 4. 정리

* C\#에서 DB에 대한 연동은 파일입출력과 동일하게 Open\(\), Close\(\)의 형태로 접근하고
* MarshalByRefObject Class가  MySqlTransaction Class에 사용된 것을 확인하여  Stream Class의 역할을 비슷하게나마 한다는 것을 알았다.



* APM을 설치하는 게 쉽지만은 않았고, 중복된 설정파일 때문에 꽤 오래 걸렸다.



