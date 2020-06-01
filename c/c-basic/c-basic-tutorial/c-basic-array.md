---
description: 'C# Basic Array'
---

# C\# Basic Array

## 무엇을 하려고 하는가?

* Reference type중 하나인 배열\(Array\)에 대해서 정리합니다.
* 자주 쓰는 데이터 타입중 하나로써, 동일한 데이터 요소들로 구성된 데이터 집합으로써 사용법이 굉장히 많습니다.



## 배열\(Array\)이란?

* 동일한 데이터 타입의 요소들로 구성된 집합입니다.
* .NET Framework의 System.Array에서 파생되었습니다.
* 보통 변수선언 앞에 Square Bracket\( \[ \] \)을 넣어서 배열임을 표시합니다.
  * ex\) int\[\] arr
* Square Bracket\( \[ \] \)안에 인덱스를 넣어서 이를 통해 데이터 요소에 접근합니다.
  * 인덱스가 0번 부터 시작합니다.
  * ex\) 배열 A의 첫번째 요소 = A\[0\]
* 최고 32차 배열까지 가질 수 있습니다.
  * 2차 이상의 배열은 아래와 같이 분류합니다.
    * Rectangular 배열 : 각 차원별 요소 크기가 고정
    * Jagged 배열 : 각 차원별 요소 크기가 다름

## 배열의 메모리 할당

* int\[3\] arr이라는 배열\(Array\) 변수가 존재할 때 배열\(Array\)은 아래와 같은 그림의 메모리 할당이 존재합니다.

![](../../../.gitbook/assets/image%20%28202%29.png)

* Stack에서 동적으로 생성된 같은 데이터 타입에 접근할 때, 인덱스로 접근하고 있습니다.
* 이는 배열 변수가 Reference type임을 증명하는 동시에 배열 변수를 복사할 때, 데이터를 복사하지 않고 배열 전체를 가리키는 Pointer임을 알 수 있습니다.



* 다차원 배열\(\)의 메모리 할당은 수학의 행렬을 통해 쉽게 이해 할 수 있습니다.

![&#xCD9C;&#xCC98;: https://blog.hexabrain.net/136 \[&#xB05D;&#xB098;&#xC9C0; &#xC54A;&#xB294; &#xD504;&#xB85C;&#xADF8;&#xB798;&#xBC0D; &#xC77C;&#xAE30;\]](../../../.gitbook/assets/image%20%28204%29.png)

* 위와 같이 이산 수학에서 배우는 행렬을 가지고 이를 이해할 수 있습니다.
* 하지만 실제로 저장되는 메모리 할당은 아래와 같은 그림을 가지고 있습니다.

![](../../../.gitbook/assets/image%20%28203%29.png)

## 배열의 활용

* 일단 기본적으로 동일한 데이터 타입을 가진 집합이라는 것은 활용도가 굉장히 높습니다.
* System.Array Class에서 파생 되었기 때문에, System.Array와 관련된 속성과 함수를 모두 사용할 수 있습니다.

![&#xCD9C;&#xCC98;: https://blog.hexabrain.net/136 \[&#xB05D;&#xB098;&#xC9C0; &#xC54A;&#xB294; &#xD504;&#xB85C;&#xADF8;&#xB798;&#xBC0D; &#xC77C;&#xAE30;\]](../../../.gitbook/assets/image%20%28198%29.png)

* **Reference type이기 때문에, 배열 변수를 다른 객체나, 함수로 전달할 때, Pointer만을 넘겨서 사용합니다.**

