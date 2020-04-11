---
description: Explanation TriviaGame TDD
---

# Explanation TriviaGame TDD

## 무엇을 하려고 하는가?

* TriviaGame에 대한 TDD 관련된 설명을 합니다.
* 보통 Unity LifeCycle flow를 따라 실행이 됩니다. 
* 하지만 이번 게임에서는 가장 핵심이 되는 Update\(\)함수를 사용하지 않습니다.

{% embed url="https://docs.unity3d.com/kr/2018.4/Manual/ExecutionOrder.html" %}

* Complete Project File에서 구분해놓은 Folder에 따라 작성합니다. 그 순서는 아래와 같습니다.
  * Delivery
  * Domain
  * Presentation
  * Service

## Domain Script

{% code title="TriviaGameView.cs" %}
```csharp
private TriviaGamePresenter _presenter;

        [SerializeField] private TMP_Text _scoreText;
        [SerializeField] private TMP_Text _questionText;
        [SerializeField] private AnswerView[] _answers;
        [SerializeField] private Animator _feedbackAnimations;
```
{% endcode %}

맨 처음 GamePlayScreen Object에 삽입된 TriviaGameView Script부터 살펴봅니다.

이 Script에서는 Game을 맨처음 플레이 할 때 바뀌어야 할 ScoreText, QuestionText, Answer Object가 포함되어 있고 마지막으로 각 상황에 조건을 만족하면 실행될 Animation이 포함되어 있습니다. 

이때 Animation에는 GameObject의 활성화 여부, 상황에 맞는 각 UI의 Scale, Text, Image Component들을 조정합니다.

그 결과물이 위와 같은 변수들입니다.

{% hint style="info" %}
이때  아래와 같은 Class변수는 namespace를 통해 다른 Script에서 사용되었습니다. 

private TriviaGamePresenter \_presenter;

GamePlayScreen Object에서는 이에 관련된 Component들을 확인 할 수 없는데 어떻게 접근이 가능한 것인가에 대한 고찰을 해보면 namespace는 선언과 동시에 

**자동적으로 전역 namespace 라는 단일 공간에 선언이 됩니다. 이로 인해 전역적인 만큼 Script가 존재하기만 한다면 검색하여 해당 namespace 주소를 가져와 컴파일단위로 함께 처리 됩니다.**

여기까지만 알아도 이 Project에 대한 궁금증을 해결되었기에, 나중에 기회가 된다면 좀 더 깊게 알아보겠습니다.
{% endhint %}

{% embed url="https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/language-specification/namespaces" caption="C\# namespace 프로그래밍 가이드" %}

{% code title="TriviaGameView.cs" %}
```csharp
private void Start()
{
    _presenter = TriviaGamePresenterBuilder.BuildTriviaGamePresenter(this);
    foreach (var answerView in _answers)
    {
        answerView.Initialize(OnAnswerSelected);
    }
}
```
{% endcode %}

Start\(\) 함수를 실행과 동시에 다른 namespace에서 가져온 \_presenter Class 변수에 TriviaGamePresenterBuilder Class에 있는 BuildTriviaGamePresenter함수에 TriviaGameView가 추가된 GameObject를 가지게 합니다.

그리고 선언한 Answer\[\] Class 배열에 접근하여 foreach반복문을 통해 AnswerView에 있는 Initialize함수에 OnAnswerSelected\(\) 함수를 실행한 결과값을 넣습니다.

OnAnswerSelected\(\) 함수는 아래와 같습니다.

```csharp
private void OnAnswerSelected(string selectedAnswer)
{
    _presenter.ReceiveAnswer(selectedAnswer);
}
```





