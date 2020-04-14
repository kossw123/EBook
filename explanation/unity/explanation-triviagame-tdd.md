---
description: Explanation TriviaGame TDD
---

# Explanation TriviaGame TDD - 작성중

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

## 각 Script의 역할 및 Class Diagram

해당 Project의 Diagram은 아래와 같습니다.

![Visual Studio&#xC758; Class Designer&#xB97C; &#xC774;&#xC6A9;&#xD55C; Diagram](../../.gitbook/assets/image%20%2827%29.png)

![&#xC8FC;&#xAD00;&#xC801;&#xC778; TriviaGame Script &#xC791;&#xB3D9; &#xC21C;&#xC11C;](../../.gitbook/assets/image%20%2816%29.png)

* TriviaGameView : GamePlayerScreen Object에 추가된 Component로써, 계속적인 GamePlay의 Controller 역할을 합니다.
* TriviaGamePresenterBuilder : TriviaGamePresenter Class를 받아서 static을 선언하여 전역 데이터 영역에 올리고, 해당 함수인 BuildTriviaGamePresenter 함수를 통해 Controller\(TriviaGameView\), 질문을 가져와\(QuestionsService\(\).GetQuestion\(\)\) 동적으로 생성합니다.
* TriviaGamePresenter : 



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

namespace에 대한 보다 자세한 설명은 기술문서에 기재했습니다.
{% endhint %}

{% page-ref page="../../technical-reference/unity/tr-triviagame-tdd.md" %}

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

* Start\(\) 함수를 실행과 동시에 다른 namespace에서 가져온 \_presenter Class 변수에 TriviaGamePresenterBuilder Class에 있는 BuildTriviaGamePresenter함수에 TriviaGameView가 추가된 GameObject를 가지게 합니다.
* 그리고 선언한 Answer\[\] Class 배열에 접근하여 foreach반복문을 통해 AnswerView에 있는 Initialize함수에 OnAnswerSelected\(\) 함수를 실행한 결과값을 넣습니다.
* OnAnswerSelected\(\) 함수는 아래와 같습니다.

```csharp
private void OnAnswerSelected(string selectedAnswer)
{
    _presenter.ReceiveAnswer(selectedAnswer);
}
```

* 위의 함수는 TriviaGamePresenter Class의 string parameter를 가진 ReceiveAnswer함수를 받아와서 비교 함수를 통해 맞는 정답이거나\(OnRightAnswerReceived\(\)\) 틀린 답\(OnWrongAnswerReceived\(\)\)을 가려냅니다.

{% hint style="info" %}
하지만 어떻게 해서 answerView.Initialize\(\) 함수에 아무런 parameter를 넘기지 않고도 동작하는가에 대한 궁금증이 생겼습니다.

이 부분은 answerView.Initialize\(Action&lt;string&gt; onAnswerSelected\)라는 parameter가 Action이기 때문입니다.

반환값도, 인자값도 가지지 않는 Action기능이라면, parameter를 적지 않고도 해당 함수에 대한 주소값을 전달하여 해당 주소의 함수를 실행 시킵니다.
{% endhint %}

{% code title="TriviaGameView.cs" %}
```csharp
    public virtual void ShowNextQuestion(Question question)
    {
        _questionText.text = question.QuestionText;

        var allAnswers = question.WrongAnswers.Concat(new string[] { question.RightAnswer }).ToList();
        allAnswers.Sort((a, b) => Random.Range(0, 2) > 0 ? 1 : -1);

        for (var i = 0; i < _answers.Length; i++)
        {
            _answers[i].FillData(allAnswers[i]);
        }
    }
```
{% endcode %}

ShowNextQuestion\(\) 함수는 \_questionText.text에 Question Class에서 QuestionText를 받아서 할당하고, 실질적으로 QuestionText Object의 Text에 할당합니다.

var type의 allAnswers를 가지고 Question Class의 string\[\] WrongAnswer 배열에 접근하여 Concat함수를 이용해 배열을 합칩니다.

