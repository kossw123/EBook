---
description: Explanation TriviaGame TDD
---

# Explanation TriviaGame TDD - 작성중

## 무엇을 하려고 하는가?

* TriviaGame에 대한 TDD 관련된 설명을 합니다.
* 보통 Unity LifeCycle flow를 따라 실행이 됩니다. 
* 하지만 이번 게임에서는 여태까지 가장 중요한 Event\(\) 함수중 하나라고 생각했던 가장 핵심이 되는 Update\(\)함수를 사용하지 않습니다.

{% embed url="https://docs.unity3d.com/kr/2018.4/Manual/ExecutionOrder.html" %}

* Complete Project File에서 구분해놓은 Folder에 따라 작성합니다. 그 순서는 아래와 같습니다.
  * Delivery
  * Domain
  * Presentation
  * Service

해설문서를 보기전에 TriviaGame은 MVP Pattern을 가지고 있습니다.

{% embed url="https://beomy.tistory.com/43" %}

![MVP Pattern&#xC758; &#xAD6C;&#xC870;](../../.gitbook/assets/image%20%2821%29.png)

* MVP Pattern이란?
  * Model + View + Presenter를 합친 단입니다. MVC Pattern에서 파생되었으며, 각 Component에 대한 역할을 아래와 같습니다.
    * Model : 사용되는 데이터와 데이터를 처리하는 부분입니다.
    * View : 사용자에게 보여지는 UI 부분입니다.
    * Presenter : View에서 사용자가 어떤 정보를 요청을 하면 해당 정보를 가지고 Model로 가공하여 다시 View에 전달합니다.
* MVP Pattern를 기초로 하여 Script들을 생성합니다.  그 결과 아래와 같은 그림으로 Script를 나눌 수 있습니다.

![](../../.gitbook/assets/image%20%2823%29.png)

## 각 Script의 역할 및 Class Diagram

해당 Project의 Diagram은 아래와 같습니다.

![Visual Studio&#xC758; Class Designer&#xB97C; &#xC774;&#xC6A9;&#xD55C; Diagram](../../.gitbook/assets/image%20%2829%29.png)

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

* `_presenter`  : 이 변수를 통해 `TriviaGamePresenterBuilder` Class에서 해당 `TriviaGamePresenter` Class를 동적 생성하고 할당합니다.
* `_scoreText` : 점수에 대한 Score 변수입니다. `UpdateScore()`를 통해 조건에 따라 증가 시킵니다.
* `_questionText` : Question Class의 질문을 Text에 할당하기 위한 변수입니다.
* `_answers` : AnswerView Class의 배열변수를 생성하여 질문들을 할당합니다.
* `_feedbackAniamations` : Animator Component를 받아와서 조작하기 위한 변수입니다.

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
private void Start() {
    _presenter = TriviaGamePresenterBuilder.BuildTriviaGamePresenter(this);
    foreach (var answerView in _answers) {
        answerView.Initialize(OnAnswerSelected);
    }
}
```
{% endcode %}

* `Start()` : \_presenter 변수를 통해 TriviaGamePresenterBuilder Class에서 TriviaGameView의 instance를 할당합니다.
  * Foreach문을 통해 내부 Class의 `OnAnswerSelected()`로 AnswerView에 대한 `Initialize()` 를 실행시킵니다.
  * 아래 Code Block은 `OnAnswerSelected()` 에 대한 함수 입니다.

```csharp
private void OnAnswerSelected(string selectedAnswer) {
    _presenter.ReceiveAnswer(selectedAnswer);
}
```

* 위의 함수는 `TriviaGamePresenter` Class의 string parameter를 가진 `ReceiveAnswer()`함수를 받아와서 비교 함수를 통해 맞는 정답이거나\(`OnRightAnswerReceived()`\) 틀린 답\(`OnWrongAnswerReceived()`\)을 가려냅니다.

{% hint style="info" %}
하지만 어떻게 해서 `answerView.Initialize()` 함수에 아무런 parameter를 넘기지 않고도 동작하는가에 대한 궁금증이 생겼습니다.

이 부분은 `answerView.Initialize(Action<string> onAnswerSelected)`라는 parameter가 Action이기 때문입니다.

반환값도, 인자값도 가지지 않는 Action기능이라면, parameter를 적지 않고도 해당 함수에 대한 주소값을 전달하여 해당 주소의 함수를 실행 시킵니다.
{% endhint %}



{% code title="TriviaGameView.cs" %}
```csharp
public virtual void ShowNextQuestion(Question question) {
    _questionText.text = question.QuestionText;
    var allAnswers = question.WrongAnswers.Concat(new string[] { question.RightAnswer }).ToList();
    allAnswers.Sort((a, b) => Random.Range(0, 2) > 0 ? 1 : -1);

    for (var i = 0; i < _answers.Length; i++) {
        _answers[i].FillData(allAnswers[i]);
    }
}
```
{% endcode %}

* `ShowNextQuestion()` : 미리 선언한 questionText에 Question Class의 Text 변수를 할당합니다.

{% hint style="info" %}
var allAnswer = question.WrongAnswers.Concat\(new string\[\] {question.RightAnswer}\).ToList\(\);

Qustion Class의 WrongAnswer\[\] 배열 변수에 접근합니다. 이때 Concat을 통해 병합을 실시하며, 병합의 대상은 string\[\] 배열을 동적으로 생성한 Question Class의 RightAnswer입니다.

최종적으로 ToList\(\) 함수를 통해 Array -&gt; List로 변환하여 List.Sort\(\)를 통해 정렬합니다.
{% endhint %}

{% hint style="info" %}
`Sort()` 함수를 통해 정렬을 하는데 이때 Lambda expression이 쓰이는데 \(a, b\)와 같이 parameter만 전달 하여 배치합니다.

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

* `_answerText` : TextMeshPro - Text의 Text Component를 의미합니다.
* `_onAnswerSelected` : string type의 Action을 의미합니다. Action에 대한 자세한 설명은 아래의 페이지를 참조하세요.

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

* `Initialize()` : string type의 Action을 \_onAnswerSelected 변수에 할당합니다.
* `FillData()` : answerText string을 \_answerText.text에 할당합니다.
* `OnClick()` : Button Component의 Click Event에 사용합니다. 이때 onAnswerSelected? statement가 의미하는 것은 __\_onAnswerSelected를 null로 초기화 한다는 표현입니다.

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

* `_view`: `TriviaGameView` Class 변수입니다. 이 변수를 통해 GamePlayScreen Object에 접근하고, Animator를 조작합니다.
* `_questions`: Question Class 배열변수를 선언하고 QuestionContainer Object의 Child Object인 QuestionText에 접근합니다.
* `_currentQuestion`: int type의 Count 변수입니다. 배열의 Index 역할을 합니다.
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

* `TriviaGamePresenter()` : 해당 Class의 parameter가 있는 생성자 입니다. 여기서는 parameter 변수들을 미리 선언한 변수에 초기화를 시킵니다. 그리고 `_view.ShowNextQuestion()` 함수를 실행시켜 다음 Question문을 호출합니다.
* `ReceiveAnswer()` : `_questions` 배열 변수에 접근하여 `IsRightAnswer()` 함수로 정답인지 아닌지를 판단하여 분기문의 내용을 실행합니다.
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
QuestionsService.cs는 질문에 대한 list와, 질문을 정렬하여 복사 후 다시 배열로 전환하는 Script입니다.

{% code title="QuestionsService.cs" %}
```csharp
private List<Question> _allQuestions;
        public QuestionsService() {
            _allQuestions = new List<Question>() {
                new Question("How many months are there in a year?", "12", new[] {"10", "15", "30"}),
                new Question("How many days are there in a week?", "7", new[] {"12", "6", "5"}),
                new Question("Witch of these is not an insect", "Cow", new[] {"Worm", "Fly", "Spider"}),
                new Question("Witch of these is not a country", "Paris", new[] {"England", "Argentina", "Spain"}),
                new Question("Witch of these is not a city", "Uruguay", new[] {"Tokyo", "Buenos Aires", "Madrid"})
            };
        }
        public Question[] GetQuestions(int amount) {
            _allQuestions.Sort((a, b) => Random.Range(0, 2) == 0 ? 1 : -1);
            return _allQuestions.GetRange(0, amount).ToArray();
        }
```
{% endcode %}

* `_allQuestions` : 질문에 대한 List 변수를 동적으로 생성합니다.
* `GetQuestion()` : 생성한 `Question`배열 타입의 함수로써 정렬 하고, parameter로 받은 값을 통해 List의 복사본을 생성 후 배열로 치환합니다.
{% endtab %}

{% tab title="ServicesProvider.cs" %}
QuestionsService Class의 Instance를 생성 하고, return 받습니다.

{% code title="ServicesProvider.cs" %}
```csharp
private static QuestionsService _questionsService = new QuestionsService();

public static QuestionsService QuestionsService() {
    return _questionsService;
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Test

Test Script를 작성하기 이전에 TDD에 대한 설명을 먼저 기재하겠습니다.

* TDD란?
  * Test - Driven - Development의 준말으로써 **방법론중 하나입니다**. Test Case를 작성하고 통과 하면 아래의 그림과 같은 Cycle로 계속적으로 수정 및 테스트를 합니다.
  * TDD를 통해 코드에 대한 자동 테스트를 작성하고 소프트웨어를 변경할 수 있습니다.
  * 새로운 개발자가 팀에 합류하면 테스트를 통해 모든 기능의 작동 및 요구 사항을 이해할 수 있습니다.
  * 버그를 일으킬 염려없이 코드에서 Refactor\(크고 작은 것 모두\)로 작업 할 수 있습니다.
  * 코드에 너무 많은 Coupling이 있는지 감지하는 데 도움이 됩니다.

![TDD Flow Chart](../../.gitbook/assets/image%20%2853%29.png)

이러한 TDD의 개념을 가지고 또 하나의 방법론을 통해 Test Case들을 선언합니다. 그 방법은 아래와 같습니다.

* Given - When - Then
  * BDD\(Behavior - Driven - Development\) 방법론 중 하나의 스타일로써 Test를 3가지 부분으로 나눕니다.
    * Given :  Test 수행하기 이전의 상태를 설명하며, Test 위한 사전 조건으로 생각할 수 있습니다.
    * When : 사용자가 지정하는 동작을 의미합니다.
    * Then : 예상되는 변화에 대해 설명합니다.

{% embed url="https://velog.io/@pop8682/%EB%B2%88%EC%97%AD-Given-When-Then-martin-fowler" caption="BDD\(Given - When - Then\)" %}

BDD 방법론에 따라 Test Case들을 나눕니다.

* Given - When - Then\(Script\)
  * Given : Test를 하기 전에 사전 조건으로 필요한 변수들이나, 설정해야 하는 것들을 미리 `[SetUp]` 을 통해 설정합니다.
  * When : `[SetUp]`을 기반으로 Test Case들을 작성합니다.
  * Then : When의 결과값에 따라 실행되어야 할 Method들을 작성합니다.



{% tabs %}
{% tab title="TriviaGamePresenterTests.cs" %}
TriviaGamePresenterTests.cs는 Test Runner를 통해 생성된 Test Script입니다. 해당 Script를 통해 Test Case를 작성하고 조건을 넣어서 참, 거짓을 판별합니다.

{% hint style="info" %}
해당 Script를 작성하기 위해 NSubstitute라는 라이브러리가 필요하며 이를 사용하려면 Project에 미리 Import해서 Script에 using문을 써서 사용해야 합니다.
{% endhint %}

{% embed url="https://nsubstitute.github.io/help/getting-started/" %}

{% page-ref page="../../technical-reference/unity/tr-triviagame-tdd.md" %}

{% code title="TriviaGamePresenterTests.cs" %}
```csharp
namespace Test.TriviaGame
{
        [TestFixture]
        public class TriviaGamePresenterTests {
        }
}
```
{% endcode %}

* `namespace Test.TriviaGame` : Test.TriviaGame이라는 namespace 생성하여 TriviaGamePresenterTests Class를 구분합니다.
* `[TestFixture]` : `[Test]`, `[SetUp]`, `[Teardown]` 를 표시하는 Class입니다.

{% code title="TriviaGamePresenterTests.cs" %}
```csharp
        private TriviaGameView _view;
        private TriviaGamePresenter _presenter;

        private Question _firstQuestion = Substitute.For<Question>();
        private Question _secondQuestion = Substitute.For<Question>();
        private Question _thirdQuestion = Substitute.For<Question>();
```
{% endcode %}

* `_view` : UI부분을 담당하는 TriviaGameView Class를 정의 합니다.  모의 객체를 생성한 뒤 올바른 결과값이 UI에 제대로 들어갔는지 확인하기 위해 정의합니다.
* `_presenter` : MVP pattern의 Model부분과 View부분의 데이터 가공을 위해 정의합니다.
* `_firstQuestion` : Question Class의 객체를 모의객체로 생성합니다.
* `_secondQuestion` : 위와 동일합니다.
* `_thirdQuestion` : 위와 동일합니다.

```csharp
#region Given
        [SetUp] public void SetUp() {
            _view = Substitute.For<TriviaGameView>();
            _presenter = new TriviaGamePresenter(_view, new Question[]{_firstQuestion, _secondQuestion, _thirdQuestion});

            _firstQuestion.IsRightAnswer("ok").Returns(true);
            _firstQuestion.IsRightAnswer("nope").Returns(false);
            
            _secondQuestion.IsRightAnswer("ok").Returns(true);
            _secondQuestion.IsRightAnswer("nope").Returns(false);
            
            _thirdQuestion.IsRightAnswer("ok").Returns(true);
            _thirdQuestion.IsRightAnswer("nope").Returns(false);
        }
        #endregion
```

\*\* \#region은 Code Block을 지정하여 Code를 접거나 펼수 있도록 하기 위해 임의로 정의했습니다. \#endregion을 통해 Code Block의 끝 부분을 설정합니다.

* `SetUp()` : `[Test]` 호출 전에 수행되는 공통 기능 집합입니다.
* `_view` : `Substitute.For()`를 사용하여 TriviaGameView의 모의 객체를 생성합니다.
* `_presenter` : 미리 생성한 Question 모의 객체를 가지고 Presenter Class의 Question parameter로 넣습니다.
* `_IsRightAnswer()` :  해당 함수가 써진 코드 부분은 Data를 넣으면 참,거짓을 판단하는 부분입니다.

{% code title="TriviaGamePresenterTests.cs" %}
```csharp
    #region When Test Case
    [Test] public void WhenRightAnswerScoreIncreases()
    {
        var initialScore = _presenter.Score;
        WhenRightAnswer();
        ThenScoreIncreasedByOne(initialScore);
    }
    [Test] public void WhenRightAnswerShowsPositiveFeedback()
    {
        WhenRightAnswer();
        ThenShowsPositiveFeedback();
    }
    [Test] public void WhenRightAnswerShowsUpdatedScore()
    {
        WhenRightAnswer();
        ThenShowsCurrentScore(_presenter.Score);
    }
    [Test] public void WhenRightAnswerShowsNextQuestiion()
    {
        WhenRightAnswer();
        ThenViewIsShowingTheSecondQuestion();
    }
    [Test] public void When3RightAnswersWin()
    {
        When3RightAnswers();
        ThenShowsWinningFeedback();
    }
    [Test] public void WhenWrongAnswerScoreDoesntChange()
    {
        var initialScore = _presenter.Score;
        WhenWrongAnswer();
        ThenScoreDoesntChange(initialScore);
    }
    [Test] public void WhenWrongAnswerGameOver()
    {
        WhenWrongAnswer();
        ThenShowsLosingFeedback();
    }
    [Test] public void ANewTriviaGameStartsWithZeroScore() {
        ThenScoreIsZero();
    }
    [Test] public void ANewGameShowsTheFirstQuestion() {
        ThenViewIsShowingTheFirstQuestion();
    }

    #region When method
    private void WhenWrongAnswer()
    {
        _presenter.ReceiveAnswer("nope");
    }
    private void WhenRightAnswer()
    {
        _presenter.ReceiveAnswer("ok");
    } 
    private void When3RightAnswers()
    {
        WhenRightAnswer();
        WhenRightAnswer();
        WhenRightAnswer();
    }
    #endregion
    #endregion
```
{% endcode %}

TestCase에서 공통적으로 쓰이는 When Method를 먼저 설명하겠습니다.

* `WhenWrongAnswer()` : 오답을 골랐을 시 반환되는 함수이며 앞서 설정한 `[SetUp]` 에서 설정한 `IsRightAnswer`, `IsWrongAnswer`의 값에 따라 참, 거짓을 반환합니다.
* `WhenRightAnswer()` : 위와 동일합니다.
* `When3RightAnswers()` : WhenRightAnswer를 3번 반환합니다.

다음으로 Test Case에서 쓰이는 `[Test]`들을 살펴보겠습니다. 이 때 List로 각 Test method들에 대한 설명을 하고 하위 List로 연관 method에 관하여 설명하겠습니다.

* `WhenRightAnswerScoreIncreases()` : 정답을 골랐을 때, Score가 증가 되는 경우입니다.
  * `ThenScoreIncreasedByOne()` : NUnit의 Assert Class의 AreEqual\(\)를 사용해, 기대값과 결과값이 일치하는지 확인합니다.
* `WhenRightAnswerShowsPositiveFeedback()` : 정답을 골랐을 때 보여지는 UI를 호출할 경우입니다.
  * `ThenShowsPositiveFeedback()` : Substitute Class의 `Received()` 함수를 통해 해당 모의 객체가 필요한 횟수만큼 Receive 받았는지 확인합니다.
* `WhenRightAnswerShowsUpdatedScore()` : 정답을 골랐을 때 Score를 Update 되는 경우입니다.
  * `ThenShowsCurrentScore()` : Score를 Update합니다.
* `WhenRightAnswerShowsNextQuestiion()` : 정답을 골랐을 때, 다음 질문을 보여줄 때의 경우입니다.
  * `ThenViewIsShowingTheSecondQuestion()` : Received\(\) 함수로 횟수만큼 Receive 받았는지 확인 후 다음 질문을 보여줍니다.
* `When3RightAnswersWin()` : 3개의 정답을 모두 맞췄을 경우입니다.
  * `ThenShowsWinningFeedback()` : Received\(\) 함수로 횟수만큼 확인 후 `ShowWinFeedback()`을 재생합니다.
* `WhenWrongAnswerScoreDoesntChange()` : 
  * `ThenScoreDoesntChange()` : 
* `WhenWrongAnswerGameOver()` : 
  * `ThenShowsLosingFeedback()` : 
* `ANewTriviaGameStartsWithZeroScore()` : 
  * `ThenScoreIsZero()` : 
* `ANewGameShowsTheFirstQuestion()` : 
  * `ThenViewIsShowingTheFirstQuestion()` : 
{% endtab %}
{% endtabs %}

