---
description: Explanation Reusable UI System
---

# Explanation Reusable UI System - 작성중

## 무엇을 하려고 하는가?

* Reusable UI System Project의 해설문서를 통해 Code의 흐름을 기재합니다.

## IP\_UI\_System

1. 변수선언
   1. 이 프로젝트에서 Script를 통해 Control 할 목록을 씁니다.
      1. 각 Screen에 넣을 Screen Script를 가져옵니다.

         **\*\*\* 이때 Component Class 변수로 가져오는데 이부분이 중요합니다.**

      2. 가져온 Screen이 현재화면, 이전화면으로 나눕니다.

         \*\*\* Property를 통해 필요한 변수들만 외부에서 선언 가능하도록 은닉화 시킵니다.
   2. UnityEvent\(\) 함수 변수를 통해 Screen이 전환될 때 마다 필요한 Event를 등록하는 변수
2. 선언한 변수를 초기화 하기
   1. 보통 Unity Event Life Cycle에서 Start\(\), Awake\(\)에서 초기화를 하기 때문에 Start\(\), Awake\(\) 함수 둘중에 하나에서 초기화 하기
      1. 하지만 Awake\(\) 함수에서 선언한다면, Script의 비활성화와 관계없이 무조건 Complier가 돌아갈 때 실행되기 때문에 비효율적이므로, Start\(\) 함수에서 초기화
      2. 초기화를 해야하는데 IP\_UI\_Screen Script를 통해 각 Screen을 통제하기 때문에, IP\_UI\_Screen 타입을 선언했었는데, 이를 통해 

