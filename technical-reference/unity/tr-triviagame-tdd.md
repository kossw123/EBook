---
description: TR TriviaGame TDD
---

# TR TriviaGame TDD - 작성중

## 무엇을 하려고 하는가?

* TriviaGame을 이루는 Script 구조에 대해서 좀 더 모호성을 줄이고 가독성을 높이는 방법이 사용되었기 때문에 그에대한 명령문과, 방법에 대해 자세히 다룹니다.

## namespace를 통한 다른 기능, 같은 이름에 대한 구별화

* TriviaGameView.cs에서 아래와 같이 namespace를 통한 클래스 이름에 대한 충돌을 방지하고 있습니다.
* 사용방법 보다는 전체적인 Script들의 Class에 대해 구분지어 놓은 구조에 대해서 살펴보시면 분명 얻어가는 것이 있다고 생각합니다.

```text
using System.Linq;
using TMPro;
using TriviaGame.Domain;
using TriviaGame.Presentation;
using UnityEngine;

namespace TriviaGame.Delivery
{
    // 
    public class TriviaGameView...
}
```

{% hint style="info" %}
namespace TriviaGame.Delivery

위의 구문을 통해 namespace를 선언하고 있습니다. 여기서는 TriviaGame.Delivery 자체가 하나의 namespace 입니다.

접근시 using명령문을 써서 상단에 미리 선언해야 합니다.

ex\) using TriviaGame.Domain;
{% endhint %}

이제 다른 Script들을 보면 

