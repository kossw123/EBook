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

* Delivery
  * `TriviaGameView` : GamePlayerScreen Object에 추가된 Component로써, 계속적인 GamePlay의 Controller 역할을 합니다.
  * `AnswerView` :
* Domain
  * `Question` :
* Presentation
  * `TriviaGamePresenterBuilder` : TriviaGamePresenter Class를 받아서 static을 선언하여 전역 데이터 영역에 올리고, 해당 함수인 BuildTriviaGamePresenter 함수를 통해 Controller\(TriviaGameView\), 질문을 가져와\(QuestionsService\(\).GetQuestion\(\)\) 동적으로 생성합니다.
  * `TriviaGamePresenter` : 
* Service
  * 

## Domain Script

{% tabs %}
{% tab title="TriviaGameView.cs" %}
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

* 이 Script에서는 Game을 맨처음 플레이 할 때 바뀌어야 할 ScoreText, QuestionText, Answer Object가 포함되어 있고 마지막으로 각 상황에 조건을 만족하면 실행될 Animation이 포함되어 있습니다. 
* 이때 Animation에는 GameObject의 활성화 여부, 상황에 맞는 각 UI의 Scale, Text, Image Component들을 조정합니다.

{% hint style="info" %}
이때  아래와 같은 Class변수는 namespace를 통해 다른 Script에서 사용되었습니다. 

private TriviaGamePresenter \_presenter;

namespace에 대한 보다 자세한 설명은 기술문서에 기재했습니다.
{% endhint %}

{% page-ref page="../../technical-reference/unity/tr-triviagame-tdd.md" %}

{% embed url="https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/language-specification/namespaces" caption="C\# namespace 프로그래밍 가이드" %}

다음으로 TriviaGameView에 사용된 함수를 살펴 보겠습니다.

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

* Start\(\) 함수를 실행과 동시에 다른 namespace에서 가져온 `_presenter` Class 변수에 `TriviaGamePresenterBuilder` Class에 있는 `BuildTriviaGamePresenter`함수에 `TriviaGameView`가 추가된 GameObject를 가지게 합니다.
* 그리고 선언한 `Answer[]` Class 배열에 접근하여 foreach반복문을 통해 `AnswerView`에 있는 Initialize함수에 `OnAnswerSelected()` 함수를 실행한 결과값을 넣습니다.
* `OnAnswerSelected()` 함수는 아래와 같습니다.

```csharp
private void OnAnswerSelected(string selectedAnswer)
{
    _presenter.ReceiveAnswer(selectedAnswer);
}
```

* 위의 함수는 `TriviaGamePresenter` Class의 string parameter를 가진 `ReceiveAnswer()`함수를 받아와서 비교 함수를 통해 맞는 정답이거나\(`OnRightAnswerReceived()`\) 틀린 답\(`OnWrongAnswerReceived()`\)을 가려냅니다.

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

* `ShowNextQuestion()` 함수는 `_questionText.text`에 `Question` Class에서 `QuestionText`를 받아서 할당하고, 실질적으로 QuestionText Object의 Text에 할당합니다.
* var type의 `allAnswers`를 가지고 `Question` Class의 `string[] WrongAnswer` 배열에 접근하여 `Concat`함수를 이용해 배열을 합칩니다.
* 이때 new string 배열로 선언 후 `Question` Class의 `RightAnswer`를 할당합니다.

{% hint style="info" %}
var allAnswer = question.WrongAnswers.Concat\(new string\[\] {question.RightAnswer}\).ToList\(\);

Qustion Class의 WrongAnswer\[\] 배열 변수에 접근합니다. 이때 Concat을 통해 병합을 실시하며, 병합의 대상은 string\[\] 배열을 동적으로 생성한 Question Class의 RightAnswer입니다.

최종적으로 ToList\(\) 함수를 통해 Array -&gt; List로 변환하여 List.Sort\(\)를 통해 정렬합니다.
{% endhint %}

* 그 후 `ToList()` 함수를 통해 Array를 List로 변환합니다. 이는 Sort를 통해 정렬하기 위함입니다.
* `Sort()` 함수를 통해 정렬을 하는데 이때 Lambda expression이 쓰이는데 \(a, b\)와 같이 parameter만 전달 하여 배치합니다.

{% hint style="info" %}
allAnswer.Sort\(\(a, b\) =&gt; Random.Range\(0 ,2\) &gt; 0 ? 1 : -1\);

내부적으로 살펴보면 list로 변환된 allAnswer는 아래의 함수를 사용하여 Sort합니다.

public void Sort\(Comparison&lt;T&gt; comparison\);

Sort는 내부적으로 정렬을 합니다. 

### 하지만 Comparision&lt;T&gt;는 어떤 의미인가? 하는 의문점이 있습니다.

Comparision은 delegate가 선언된 함수 원형입니다. 그 내용은 아래와 같습니다.

public delegate int Comparison\(T x, T y\);

int type의 함수원형을 선언 하고 Delegate를 사용해서 함수 포인터를 사용하는 곳으로 넘깁니다. 이 함수는 Sort\(\)함수로 넘어가서 Sort\(\)에 사용된 Algorithm을 통해 x, y를 비교하게 됩니다.

Lambda식을 사용해서 간결하게 표시하고 최종적으로, 

a와 b를 Random.Range\(0, 2\) &gt; 0 ? 1 : -1의 조건에 따라 배치합니다.

\(a, b\) =&gt; Random.Range\(0, 2\) &gt; 0 ? 1 : -1 

### Sort\(\) 알고리즘은

간단하게 1, -1을 비교하여 두가지의 비교 대상의 위치를 변환합니다.
{% endhint %}

그리고 마지막으로 반복문을 사용해 `AnswerView[]` 배열 변수인 `_answers`를 `FillData()`함수를 통해 Text에 넣습니다.
{% endtab %}

{% tab title="AnswerView.cs" %}
AnswerView.cs에서는 단순하게 Text를 설정하고, Action&lt;string&gt; 기능을 통해 함수를 전달하여 다른 namespace의 Class에서 실행 시킵니다.

이제 AnswerView.cs를 살펴 보겠습니다. 

{% code title="AnswerView.cs" %}
```csharp
[SerializeField] private TMP_Text _answerText;
private Action<string> _onAnswerSelected;
```
{% endcode %}

* \_answerText : TextMeshPro - Text의 Text Component를 의미합니다.
* \_onAnswerSelected : string type의 Action을 의미합니다. Action에 대한 자세한 설명은 아래의 페이지를 참조하세요.

{% page-ref page="../../technical-reference/unity/tr-triviagame-tdd.md" %}

다음으로 사용된 함수를 살펴 보겠습니다.

{% code title="AnswerView.cs" %}
```csharp
public void Initialize(Action<string> onAnswerSelected) {
            _onAnswerSelected = onAnswerSelected;
        }

public void FillData(string answerText) {
            _answerText.text = answerText;
        }

public void OnClick() {
            _onAnswerSelected?.Invoke(_answerText.text);
        }
```
{% endcode %}

* Initialize : string type의 Action을 \_onAnswerSelected 변수에 할당합니다.
* FillData : answerText string을 \_answerText.text에 할당합니다.
* OnClick : Button Component의 Click Event에 사용합니다. 이때 onAnswerSelected? statement가 의미하는 것은 __\_onAnswerSelected를 null로 초기화 한다는 표현입니다.

{% hint style="info" %}
이때 Action.Invoke\(\)는 함수를 호출 하는데 사용합니다. 

\_onAnswerSelected는 Action이고, Action은 delegate이기 때문에 Invoke를 통해 전달한 함수를 실행시키는 기능을 하고 있습니다.
{% endhint %}
{% endtab %}
{% endtabs %}



## Domain

{% tabs %}
{% tab title="Question.cs" %}
Domain Folder의 Question.cs를 살펴보겠습니다.

{% code title="Question.cs" %}
```csharp
public string QuestionText { get; private set; }
public string RightAnswer { get; private set; }
public string[] WrongAnswers { get; private set; }
```
{% endcode %}

* `QuestionText` : TriviaGame의 QuestionContainer Object의 하위 Object인 QuestionText를 의미합니다. 이를 Property로 생성하여 읽기,쓰기 전용으로 설정하여 다른 지역에서 값을 바꾸지 못하게 합니다.
* `RightAnswer` : 실제 정답과 비교하기 위한 property 변수입니다.
* `WrongAnswers` : 나머지 오답을 담는 property 배열 변수입니다.

{% code title="Question.cs" %}
```csharp
public Question() { }
public Question(string questionText, string rightAnswer, string[] wrongAnswers) {
    QuestionText = questionText;
    RightAnswer = rightAnswer;
    WrongAnswers = wrongAnswers;
}
public virtual bool IsRightAnswer(string anAnswer) {
    return string.Equals(RightAnswer, anAnswer);
}
```
{% endcode %}

* `Question()` : parameter가 있는 생성자를 생성하기 위한, Default 생성자를 선언합니다.
* `Question(parameter)` : Class에 property를 통해 타 Class, Class 내부에서 값을 변경하지 못하게 하기 위한 생성자입니다.
* `IsRightAnswer` : parameter와 `RightAnswer`를 비교하여 참, 거짓을 반환합니다.
{% endtab %}
{% endtabs %}

## Presentation

{% tabs %}
{% tab title="TriviaGamePresenter.cs" %}
`TriviaGamePresenter`  Class는 

TriviaGamePresenter Class를 살펴 보겠습니다.

{% code title="TriviaGamePresenter.cs" %}
```csharp
private TriviaGameView _view;
private Question[] _questions;
private int _currentQuestion;
public int Score { get; private set; }
```
{% endcode %}

* `_view` : TriviaGameView Class 변수입니다. 이 변수를 통해 GamePlayScreen Object에 접근하고, Animator를 조작합니다.
* `_questions` : Question Class 배열변수를 선언하고 QuestionContainer Object의 Child Object인 QuestionText에 접근합니다.
* `_currentQuestion` : int type의 Count 변수입니다. 배열의 Index 역할을 합니다.
* `Score` : int type의 property변수입니다.

{% code title="Question.cs" %}
```csharp
public TriviaGamePresenter(TriviaGameView view, Question[] questions) {
    _view = view;
    _questions = questions;
    Score = 0;
    _currentQuestion = 0;
    _view.ShowNextQuestion(_questions[_currentQuestion]);
}

public void ReceiveAnswer(string anAnswer) {
    if (_questions[_currentQuestion].IsRightAnswer(anAnswer)) {
        OnRightAnswerReceived();
    }
    else {
        OnWrongAnswerReceived();
    }
}

private void OnWrongAnswerReceived() {
    _view.ShowLoseFeedback();
}

private void OnRightAnswerReceived() {
    Score++;
    _currentQuestion++;
    _view.UpdateScore(Score);
    if (Score == 3) {
        _view.ShowWinFeedback();
    }
    else {
        _view.ShowPositiveFeedback();
        _view.ShowNextQuestion(_questions[_currentQuestion]);
    }
}
```
{% endcode %}

* `TriviaGamePresenter()` : 해당 Class의 parameter가 있는 생성자 입니다. 여기서는 parameter 변수들을 미리 선언한 변수에 초기화를 시킵니다. 그리고 \_view.ShowNextQuestion\(\) 함수를 실행시켜 다음 Question문을 호출합니다.
* `ReceiveAnswer()` : \_questions 배열 변수에 접근하여 IsRightAnswer\(\) 함수로 정답인지 아닌지를 판단하여 분기문의 내용을 실행합니다.
* `OnWrongAnswerRecevied()` : 틀렸을 때 반응할 Animator 함수를 실행합니다.
* `OnRightAnswerReceived()` : 정답을 맞췄을 때 해야할 일들을 실행시킵니다.
{% endtab %}

{% tab title="TriviaGamePresenterBuilder.cs" %}
`TriviaGamePresenterBuilder`  Class를 살펴 보겠습니다.

{% code title="TriviaGamePresenterBuilder.cs" %}
```csharp
public class TriviaGamePresenterBuilder {
    public static TriviaGamePresenter BuildTriviaGamePresenter
    (TriviaGameView view) {
        return new TriviaGamePresenter(view, 
        ServicesProvider.QuestionsService().GetQuestions(3));
    }
}
```
{% endcode %}

* `BuildTriviaGamePresenter()` : `TriviaGamePresenter` type의 함수로써 반환 받는 값은 `TriviaGamePresenter` Class의 생성자를 호출합니다. parameter는 `TriviaGameView`와 `ServiceProvider` Class의 `QuestionsService().GetQuestion()` 함수를 호출합니다.
{% endtab %}
{% endtabs %}

## Service

{% tabs %}
{% tab title="QuestionsService.cs" %}

{% endtab %}

{% tab title="ServicesProvider.cs" %}

{% endtab %}
{% endtabs %}



