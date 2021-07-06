# Chapter 2. 설치

* APM을 수동으로 설치함에 따라 설치 과정에서 얻을 수 있는 설정 값이나, 파일들을 보고 전체적인 그림을 파악하는데 의미를 둔다.
* Window 10 환경에서 적용시킨다.
* VSCode를 사용했기에 이를 사용한 느낌을 똑같이 문서에 담고자 아래와 같은  Code block Convention을 작성한다.

```php
줄번호     Property       PropertyValue
 12     Define SRVROOT   "C:\Apache24"     
```

* 혹여나 설정값을 제대로 수정했는데 오류 및 원하는 결과가 안나온 다면, 설치 과정을 점검해야 한다.
* 그러나 명심해야 할 것은 APM 설치 링크에서 다운받은 파일들을 "전혀 문제가 없다." 
* 설치 과정을 점검하는데 중점은  "같은 설정값을 쓰는 또다른 파일이 존재" 한다는 가능성이 굉장히 크다.
* php까지 연동했다면, phpInfo를 통해 꼭 경로를 확인하도록 한다.

## 1. Apache

{% tabs %}
{% tab title="1. 다운로드" %}
{% embed url="https://www.apachelounge.com/download/" %}

위 링크에 들어가서 자신의 컴퓨터의 시스템 종류에 맞는 것을 확인 한 후   
알맞는 Bit를 선택하여 다운 받는다.
{% endtab %}

{% tab title="2. 설정값 수정" %}
다운 받은 Apache 파일을 C:\ 혹은 D:\ 같은 root Directory에 다운받는다.

그리고 root Directory\(이하 root\):\Apache24\conf\httpd.conf 파일을 찾아서 다음과 같은 Code로   
수정한다.

```php
/// 작성자는 rootDirectory(C:\ or D:\)에 다운 받았기 때문에
/// 아래와 같이 작성했지만, 다른 경로에 저장해도 상관없다.
/// 중요한 것은 Apache24 Folder의 경로를 설정한다는 것이다.
37 Define SRVROOT ""
39 ServerRoot "${SRVROOT}"  -> 위에서 선언한 Path값의 이름은 SRVROOT
-> 
37 Define SRVROOT "rootDirectory\Apache24"
39 ServerRoot "${SRVROOT}"


/// Apache는 기본적으로 모든 주소에 대해 요청을 기다린다.
/// 하지만 Listen 지시어를 사용하여 특정 주소, 특정 포트 조합에 대해서만 요청을
/// 기다리도록 할 수 있다.
/// 그리고 로컬 개발환경이기 때문에, 다른 주소는 필요 없다. 
60 #Listen 80
->
60 Listen 80 -> 주석 해제하고 기본 포트번호 80을 사용한다.

이때 먼저 확인해야 할것은 주소창에 localhost:80을 입력해서 포트가 사용중인지를 
확인해야한다.

작성자는 이미 Internet infomation Service(IIS)가 이미 포트르 쓰고 있기 때문에 
작업관리자 -> 서비스 -> W3SVC를 중지 시켜 포트를  확보했다.

아니면 다른 포트를 사용해도 된다.

219 ServerAdmin 이메일 주소 입력 -> 서버에 문제가 생기면 에러문서를 생성해 보낸다.


/// SRVROOT를 기반으로 htdocs folder를 찾는 코드
/// 만약 설정값을 수정할 필요가 생기면 알아두는 것도 나쁘지 않
252 DocumentRoot "${SRVROOT}/htdocs"
253 <Directory "${SRVROOT}/htdocs">



/// 파일명을 지정하지 않고 Apache에 통신을 요청하면, DirectoryIndex의 파일을
/// 반환한다.
/// 맨 처음 주소창에 "localhost:포트번호"를 입력하면 index.html을 반환한다.
285 <IfModule dir_module>
286    DirectoryIndex index.html
287 </IfModule>
->
285 <IfModule dir_module>
286    DirectoryIndex index.html index.php 
        -> 하지만 PHP를 사용하여 웹 페이지를 확인해야 하기 때문에, 
           index.php의 파일도 확인할 수 있도록 한다.
           그리고 index.php는 phpinfo()를 통해 PHP정보를 확인한다.
287 </IfModule>CMD를 관리자 권한을 통해 실행한다.
그리고 다음과 같은 명령어를 입력하여 설치한다.
```
{% endtab %}

{% tab title="3. 시스템 환경 변수\(Path\)에 Apache 등록" %}
시스템 환경 변수\(Path\)는 OS가 명령 프롬프트 또는 터미널 창에서   
필요한 실행 파일을 찾는데 사용하는 시스템 변수다.

즉, Path에 Apache\bin 파일을 등록하면 명령 프롬프트에서 따로 경로를 찾을 필요없이  
바로 사용할 수있다.

Window Key -&gt; 시스템 환경 변수 편집 -&gt; 환경 변수 -&gt;   
시스템 변수 박스에서 Path 찾기-&gt; 더블 클릭하고 Apache\bin 경로를 새로만들기 -&gt;   
확인
{% endtab %}

{% tab title="4. 명령 프롬프트를 통해 Apache 설치" %}
CMD\(명령 프롬프트\)를 관리자 권한으로 실행시키고 다음 명령어 입력

{% hint style="info" %}
httpd -k install
{% endhint %}

설치 완료라고 표시 된다면 성공.  
성공 메시지 밑에 자잘한 메시지는 무시해도 된다.

그 다음 명령어 입력

{% hint style="info" %}
net start apache2.4
{% endhint %}

그 후 작업 관리자 -&gt; 서비스에서 Apache2.4가 실행중인지 확인한다.

그리고 주소창에 "localhost:포트번호"를 입력하고 "It's work"라는 문구가 뜨면 설치 완료
{% endtab %}
{% endtabs %}

## 2. PHP

{% tabs %}
{% tab title="1. 다운로드" %}
{% embed url="https://windows.php.net/download/" %}

자기 OS 환경\(bit, Windows Version 등\)에 맞는 Thread safe Zip 다운 받는다.
{% endtab %}

{% tab title="2. 설정값 수정\(php.ini\)" %}
다운받은 파일의 압축을 풀면, 해당 파일 내부에 2개의 파일이 존재한다.

* php.ini-development
* php.ini-production

이 중 php.ini-development를 php.ini로 수정한다.  
그리고 다음과 같이 수정한다.

```php
198 short_open_tag = false
->
198 short_open_tag = On

768 ;extension_dir = "./"
->
768 extension_dir = "php파일 내부의 ext 파일 경로"

933 ;extension=mysqli
937 ;extension=openssl
->
933 extension=mysqli
937 extension=openssl
```
{% endtab %}

{% tab title="3. 설정값 수정\(httpd.conf\)" %}
php와 apache를 연동할 모듈을 Apache파일 httpd.conf의 맨 아래에 추가하도록 한다.

```php
PHPIniDir "php 파일이 있는 경로"
LoadModule php_module "php 파일 내부의 apache.dll"
AddType application/x-httpd-php .htm .html .php
```
{% endtab %}

{% tab title="4. phpInfo\(\) 출력" %}
Apache에서 localhost를 치면 "it's work"라는 index.html이 실행 됐다.

해당 파일은 Apache의 htdocs파일 내부의 index.html에 존재하며,  
여기에 새로운 php파일을 생성한다.

\*\*\* php파일은 Notepad같은 편집기를 통해 생성이 가능하고, 여기서 주의해야 할 점은  
      보통 같은 경우는 파일명에 확장자명을 같이 적으면 해당 확장자명의 파일이 생성되지만,  
      php파일은 그렇지 않다. 꼭 파일 형식을 모든 파일로 변경하고 파일 이름 끝에 php를 붙여 생성  
      하도록 한다.

해당 php 파일 내부는 다음과 같이 적는다.

```php
<?php
  phpinfo();
?>
```

저장한 후에 주소창에 PHP 설정값에 관한 웹 페이지가 나오면 성공
{% endtab %}
{% endtabs %}

## 3.MySQL

{% tabs %}
{% tab title="1. 다운로드" %}
{% embed url="https://dev.mysql.com/downloads/mysql/" %}

현재 사용하고 있는 PC를 Server로 사용할 것이기 때문에, 이를 포함해서 여러가지 귀찮은 설치 작업을 Workbench 하나로 통합하여 해결한다.
{% endtab %}

{% tab title="2. 시스템 환경 변수 추가" %}
설치 하는 과정을 모두 Default로 설정했다면,   
C 드라이브의 Program Files\MySQL\ 에 있다.

우리가 사용하는 PC를 Server로 사용할 것이기 때문에, MySQL 폴더 안에는 MySQL Server 폴더가 있을 것이고, 이 폴더 안에 bin폴더를 시스템 환경 변수 Path에 추가한다.
{% endtab %}

{% tab title="3. 명령 프롬프트를 통한 MySQL 설치 확인" %}
Workbench를 설치 할 때, 기본적인 사용자를 생성할 수 있었는데, 그 사용자 ID와 password를 기억한 후 다음과 같이 입력한다.

mysql -u 기본사용자ID -p

password를 입력 후 mysql&gt; 이 뜬다면 연동 성
{% endtab %}
{% endtabs %}

## 4. phpMyAdmin

{% tabs %}
{% tab title="1. 다운로드" %}
{% embed url="https://www.phpmyadmin.net/" %}

위 링크를 들어가서 phpMyAdmin을 다운 받는다.
{% endtab %}

{% tab title="2. 다운받은 파일 경로 설정" %}
php를 이용해서   
Apache라는 웹 호스팅 서비스를 할 수 있는 웹 서버를 통해 phpMyAdmin이라는 웹페이지를 통해 MySQL이라는 데이터베이스를 UI를 통해 볼 수 있다.

라는 개념인데, Apache를 통해 웹에서 볼 수 있게 되기 때문에, htdocs 폴더 내부에 phpmyadmin이라는 파일을 생성 후 압축을 푼다.
{% endtab %}

{% tab title="3. phpMyAdmin 설정값 수정" %}
config.sample.inc.php 파일을 config.inc.php로 수정 후 다음과 같이 설정값을 수정한다.

```php

```
{% endtab %}
{% endtabs %}

