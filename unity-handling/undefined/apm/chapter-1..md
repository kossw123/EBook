# Chapter 1. 정의

## 왜 해야 하는가?

환경이 너무 많이 변한다.

7월 1일 기준으로 Window11가 나온다는 소식이 있다.\
그런데, 중요하다고 생각되는 부분 중 하나는, Window가 세상에서 가장 많은 OS를 차지하는데,\
이제는 PC에서 Android를 지원한다는 소식이 들렸다.

또한 WSL(Windows Subsystem for Linux) 서비스를 통해 Linux도 지원하여, 다양한 개발 환경이 되도록 \
지원하고 있다.

근데 생각해보면

* 이제 경제적인 면에서는 디지털 전환으로 인한 자본이 유입되고 있다.
* 프로그래머적인 면에서는 이런 투입되는 자본으로 인해 몸값이 상승하고, 그로 인해 기존의 환경보다\
  더 많은 환경에 대해 접근하여 호환을 해야한다.

결론적으로 환경이 달라진다고 해서 환경을 구성하는 요소가 달라진다고 생각하지 않기에 APM이라는 기본적인 개발 환경을 구축하도록 한다.

그리고 기본적인 개발 환경과 동시에, CICD 개념을 도입한 Jenkins등 여러가지 환경이 있다.

## 그래서?

가장 기초적인 Local Development Enviroment는 APM이기 때문에, 한번 구축해 보자.

Bitnami, yum, XAMPP 등등 개발 환경을 통합적으로 설치하고 설정해주는 웹 서버 설치 프로그램이 \
존재하며, 이를 사용하면 수동으로 하는 것보다 훨씬 더 빠르게 설치가 가능하다.

하지만 여기서는 APM만 설치하고 기본설정까지 한다.

그리고 각자 정의된 기능에 따라서 다음과 같은 전체적인 그림이 그려진다.

![Client 1 ㅡ> Client 2까지 HTML문서 혹은 Object가 전달되는 과정](<../../../.gitbook/assets/image (277).png>)



그리고 최종적으로 phpMyAdmin이라는 프로그램을 통해 웹 페이지에서 DB로 접근하도록 설정한다.

## 1. [Apache](../../unity-handling/h-i-j-k-l-m-n.md#local-development-enviroment)

W3(www.)라는 공간에서 HTTP라는 절차(혹은 순서가 있는 약속, 규약)을 통해 HTML문서, Object를 전송해주는 프로그램이다.&#x20;

* 웹 서버이다.
* 웹 서버는 HTTP를 통해 HTML문서, Object들을 전송해주는 서비스 프로그램이다.
* HTTP는 HyperText Transfer Protocol의 준말로, W3상에서 정보를 주고받을 수 있는 프로토콜이다.

{% embed url="https://web.archive.org/web/20170302030636/https://www.koresight.com/hangeul-inteones-tong-gye" %}
위 사이트를 통해 많은 웹 서버가 Apache로 구성되있음을 알 수 있다.&#x20;
{% endembed %}

## 2. PHP

프로그래밍 언어의 일종이다.

* 원래는 동적 웹 페이지를 만들기 위해 설계 되어 PHP로 작성된 코드를 HTML 소스 문서에 넣으면\
  PHP 처리 기능이 있는 웹 서버에서 작성자가 원하는 페이지를 생성한다.
* 그러나 요즘에는 PHP와 HTML 코드를 따로 분리하여 작성하는 경우가 일반적
* PHP 또한, 웹 서버가 아닌 php-fpm을 통해 실행되는 경우가 많아졌다.

## 3. MySQL

세계에서 가장 많이 쓰이는 오픈 소스의 관계형 데이터 베이스이다.

* DB 자체의 목적이 여러 사람이 공유하여 사용할 목적으로 체계화 시키고 통합, 관리하는 데이터의 \
  집합이다.
* 그러나 GUI를 따로 제공하고 있진 않다.
* 하지만 PHP와 결합되어 아주 많이 사용된다.

## 4. phpMyAdmin

MySQL을 웹 페이지로 관리하기 위해 PHP로 작성된 Tool이다.\
UI, DB관리, CRUD등과 같은 여러 기능이 존재하기 때문에 웹 호스팅 서비스를 위한 가장 대중적인 MySQL 관리 도구로 대표되고 있다.

![phpMyAdmin try demo version](<../../../.gitbook/assets/image (278).png>)

\
