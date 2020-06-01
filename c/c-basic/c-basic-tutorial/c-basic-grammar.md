---
description: 'C# Basic Grammar'
---

# C\# Basic Grammar

## 무엇을 하려고 하는가?

* PL\(Programming Language\)을 사용하기 위해서는 해당 언어에 대한 문\(Grammar\)에 대해서 서술합니다.
* 명령문을 사용하기 위한 연산자\(Operator\)와 구문\(Grammar\)에 대해서 알아봅니다.

## Syntax vs Semantics

* 언어에 대한 구문, 의미, 문장에 대해서 구별하는 것이 필요할 것 같습니다.
* 보통 PL에서는 아래와 같이 분류합니다.



* 문법\(Grammar\)
  * 이미 정의되고 형식화된 의미입니다.
    * 보통 우리가 쓰는 반복문\(for, while, do-while\)이나 조건문\(if, switch\)등과 같은 이미 형식화된 단어들의 집합을 문법이라고 니다.
* 구문\(Syntax\)
  * 프로그램의 모습, 형태, 구조가 어떻게 보이는 것인가에 대한 정의입니다.
    * 문법\(Grammar\)에 따라 유효한지 타당한지 확인하는 것과 관련이 있습니다.
      * 예시로, 반복문을 의미하기 위해 작성해야 하는 문법이 있습니다.
      * 반복문을 사용하기 위해서는\(ex : for Loop\)
        * for라는 키워드
        * for를 사용하기 위한 괄호\(\)
        * 괄호\(\) 안에 들어가야할 문법들\(ex : 초기 변수, 반복 범위 변수, 증가 변수\)
    * 즉, 문법\(Grammar\) 규칙을 다루는 분야입니다.
* 의미\(Semantic\)
  * 문장\(Syntax\)이 타당한 의미를 지니고 있는지 아닌지를 판별합니다.
    * 보통 Compiler - Error가 일어나는 이유는 Compiler 자체의 의미 분석기\(Semantic Analytics\)를 통해 Error를 표시합니다.
      * 반복문을 사용하기 위한 Syntax 중에 하나라도 타당하지 않은 값을 넣는다면, 의미\(Semantic\)가 맞지 않기 때문에 발생하는 일입니다.



* 하지만 C\#에 대한 Basic을 위해 문법\(Grammar\)을 통해 어떤 프로그램을 작성하기 위한 키워드와 그에 맞는 구문\(Syntax\)에 대해 설명합니다.





## C\# Basic Grammar

### 조건문

* 
