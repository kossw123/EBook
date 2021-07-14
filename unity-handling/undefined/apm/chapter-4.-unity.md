# Chapter 4. Unity와의 연동

## 1. 주의점

* Unity에서는 Pro가 아닌 Personal에서 SQL와의 연동을 지원하지 않는다.
* 앞에서 Nuget Package를 통해 MySql.Data DLL을 사용하여 라이브러리를 가져왔지만, Unity에서는 지원하지 않는다.
* 그래도 사용하려면 Pro를 구입하거나, Script로 PHP를 통해 우회해서 사용하거나,  DLL을 프로젝트 내부에 위치시켜서 불러오거나, Nuget Package를 프로젝트 내부에 위치 시키거나 해야한다.
* 아래의 Tutorial을 통해 구한한 것은 PHP를 이용하는 방식이고, phpMyAdmin이 설치 되어 연동이  되야하는 상태여야 가능한 Tutorial이다.

{% embed url="https://www.youtube.com/watch?v=b4zh4xl4vLc" %}



## 2. 작성법

이전에는 MySqlConnection이라는 MySql.Data에 존재하는 Class를 사용했지만,  
이번에는 WWWForm으로 localhost에 접속해서 직접 데이터를 가져온다.

DLL이나, 다른 Package를 설치하지 않고 PHP를 이용해서 설치해야 하기 때문에   
먼저 작성해야 할 것이 있다.

* CRUD를 위한 PHP Script
* Apache에서 작성한 PHP Script를 실행하기 위한 다른 Folder 생성 및 설정

대충 아래와 같은 그림이 완성된다.

![](../../../.gitbook/assets/image%20%28286%29.png)

