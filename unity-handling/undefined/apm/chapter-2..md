# Chapter 2. 설치

* APM을 수동으로 설치함에 따라 설치 과정에서 얻을 수 있는 설정 값이나, 파일들을 보고 전체적인 그림을 파악하는데 의미를 둔다.
* Window 10 환경에서 적용시킨다.
* VSCode를 사용했기에 이를 사용한 느낌을 똑같이 문서에 담고자 아래와 같은  Code block Convention을 작성한다.

```php
줄번호     Property       PropertyValue
 12     Define SRVROOT   "C:\Apache24"     
```

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
60 Listen 80 -> 주석 해제하고 기본 포트번호 80을 특정 한다 .


```
{% endtab %}
{% endtabs %}

## 2. PHP



## 3.MySQL



## 4. phpMyAdmin

