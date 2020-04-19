---
description: tutorial TriviaGame TDD
---

# tutorial TriviaGame TDD - 작성중

##  무엇을 하려고 하는가?

* 게임을 제작할 때 많은 버그와, 이슈, Side Effect가 발생됩니다. 이를 해결할 때 QA\(_Quality Assurance_\) 라는 가이드라인을 통한 수정을 할 수도 있지만, 제작하는 과정에서 미리 테스트를 통해서 발견 할 수 있는 버그들은 제작과정에서 고치는 것이 수월합니다.
* 그 방법의 하나로써 TDD\(Test - Driven - Development\)라는 방법론을 채택하여 Unity 자체에 탑재된 기능의 하나인 Test Runner를 사용하여 Trivia Game을 만들고, Unit 단위로 올바른 값이 들어간지 체크할 수 있습니다.

{% hint style="info" %}
보통 게임을 플레이 하면서 충돌이나, 플레이가 안된다거나, 렌더링이 안된다거나 등에 대한 오류가 대부분을  차지하고, 값에 대한 올바른 값이 들어갔는가에 대한 오류는 아주 극소수입니다.

하지만 그런 오류를 잡는것 뿐만 아니라, Unit 단위의 테스트를 통해, Unity로 Game 뿐만이 아니라 App 개발에 있어서 아주 유용하게 사용할 것 같아서 이 주제로 문서를 작성하게 되었습니다.
{% endhint %}

* 해당 자료는 상업적으로 이용하지 않고 이용되서도 안됩니다.
* 아래의 링크를 타고 들어가면 해당 문서에 대한 보다 풍부한 자료가 있습니다.

{% embed url="https://engineering.etermax.com/how-to-tdd-in-unity-using-the-mvp-pattern-a646ffbe996f" caption="MVP\(Model ,View, Presenter\)패턴을 사용한 TDD 사용법" %}

## 작성법 - UI 배치

* 배치에는 다소 수고스러움이 있으니 왠만하면 Complete Project File을 사용하는걸 추천합니다.
  * 작성법 - TDD 부분으로 넘어가시면 됩니다.
* UI 배치 관하여 연습을 하고 싶다면 Complete Project를 보면서 하나씩 해보는 것을 추천합니다.
* 위의 Hierarchy 그림처럼 GameObject를 배치하는 게 최종 목표입니다.
* **지금 부터 본문에 적는 Object에 대한 설정값은 보기에 편하도록 설정한 것이니, 다르게 설정하셔도 무방합니다.**

![&#xC704;&#xC758; &#xADF8;&#xB9BC;&#xCC98;&#xB7FC; &#xBC30;&#xCE58;&#xD569;&#xB2C8;&#xB2E4;.](../../.gitbook/assets/image%20%2875%29.png)

* Canvas
  * Canvas Scaler - UI Scale Mode를 Scale With Screen Size로 설정해 1920 \* 1080로 조절합니다.
* Child Object로 GamePlayScreen을 배치하고 그림과 같이 Child Object를 설정합니다.
* BackGround, Bonzo, Tina
  * BackGround
    * Image Ccomponent 추가 후 Color를 임의의 색으로 조정합니다.
  * Bonzo
    * Image Component 추가 후 Bonzo Sprite 설정합니다.
    * 화면의 양 옆에 배치합니다.
  * Tina
    * Image Component 추가 후 Tina Sprite 설정합니다.
    * 화면의 양 옆에 배치합니다.

 여기까지 했으면 아래와 같은 그림의 Object 배치가 완료 됩니다.

![](../../.gitbook/assets/image%20%28107%29.png)

* Score
  * Score Object를 추가하고 Image Component를 추가하여, ScoreContainer Sprite로 설
  * Anchors를 활용하여 Top, Left로 설정합니다.
    * 이렇게 하면 설정한 위치를 기준으로 Position, Scale이 적용 됩니다.
  * 설정한 Anchors를 기반으로 Width, Height를 설정하여 크기를 조정합니다.
    * 작성자는 300 / 200정도로 설정했습니다.
  * Score Label Object를 Child Object로 추가합니다.
* ScoreLabel
  * Score Object의 Child Object로써, TextMeshPro - Text\(UI\) Component를 추가합니다.
  * 굵게\(Font Style : B\), 크기는 50\(Font Size : 50\), Alignment\(Center, Middle\)을 설정합니다.
    * 임의대로 바꿔도 무방합니다.
* ScoreText
  * Score Object Child Object로써, TextMeshPro - Text\(UI\) Component를 추가합니다.
  * ScoreLabel과 같이 Font에 대한 설정을 임의 대로 합니다.
    * 작성자는 \(B, 50, Center / Middle\)로 설정하고 Vertex Color를 Black으로 설정했습니다.

![&#xC784;&#xC758;&#xB85C; &#xC791;&#xC131;&#xD55C; Score Object&#xC5D0; &#xB300;&#xD55C; &#xACB0;&#xACFC;](../../.gitbook/assets/image%20%28100%29.png)

* Question
  * Image Component를 추가하여 QuestionContainer Sprite로 설정합니다.
  * Width, Height\(1050, 920\)를 설정합니다.
* QuestionContainer
  * Question의 Child Object로써 Image Component를 추가하여 TextContainer Sprite로 설정합니다.
  * Anchors를 아래와 같이 설정합니다.
    * min X : 0.5 / min Y : 1
    * max X : 0.5 / max Y : 1
  * Pivot을 설정합니다.
    * X : 0.5 / Y : 1
* QuestionText
  * TextMesh Pro - Text\(UI\) Component를 추가합니다.
  * Anchors를 아래와 같이 설정합니다.
    * min X : 0 / min Y : 0
    * max X : 1 / max Y : 1
  * Pivot을 설정합니다.
    * X : 0.5 / Y : 0.5
  * Font를 아래와 같이 설정합니다.
    * Font Style : B
    * Auto Size Check
      * Min : 18 / Max : 60
    * Vertex Color : Black
    * Alignment : Center / Middle
* ContainerAllAnswer
  * Question Object의 Child Object로써, Answer 기능에 관련한 Object를 배치하기 위한 BackGround Object입니다.
  * Image Component를 추가하여 AnswerContainer Sprite를 설정하고 Color.Alpha값을 줄여줍니다.
  * 아래와 같이 Rect Transform을 설정합니다.
    * Pos X : 0 / Pos Y : -157 / Pos Z : 0
    * Width : 1000 / Height : 538
* Answer
  * Question Object의 Child Object로써, 해당 Object에 Child Object를 추가하여 Answer Object를 관리합니다.
  * Vertical Layout Group을 추가하여 Control Child Size의 Width를 Check합니다.
    * 이렇게 하면 간편하게 수직적으로 Object를 추가할 때 Transform이나 Position, Anchors의 변화 없이 추가할 수 있습니다.
  * Rect Transform을 아래와 같이 설정합니다.
    * Anchors
      * middle Stretch로 설정
      * Left : 50 / Pos Y : -167 / Pos Z : 0
      * Right : 50 / Height : 512
* Answer N Object
  * Answer Object의 Child Object로써, 해당 Object는 Button Component를 통해 동작합니다.
  * Button Component 추가 후 On Click\(\) Event를 추가해줍니다.

작성법 - Script 단락에서 Answer Object에 대한 Script와 동시에 배치도 기재하겠습니다.

여기까지 하셨다면 아래의 그림과 같은 결과가 나옵니다.

![](../../.gitbook/assets/image%20%285%29.png)

* Feedback
  * Animator Component를 추가하여 상황에 따른 동작을 추가합니다.
    * Feedback Animator를 설정
* Correct
  * Feedback의 Child Object로써, 올바른 정답을 맞혔을 때, 화면에 출력되는 이미지 입니다.
  * BG
    * Correct Object의 Child Object로써 Image Component를 추가하여 Tiny Sprite로 설정합니다.
    * Rect Transform을 아래와 같이 설정합니다.
      * Width : 4500 / Height : 2500
  * Container
    * Correct Object의 Child Object로써 Image Component를 추가하여 WinContainer Sprite로 설정합니다.
    * Rect Transform을 아래와 같이 설정합니다.
      * Width : 600 / Height : 700
  * Bonzo
    * Correct Object의 Child Object로써 Image Component를 추가하여 BonzoCorrect Sprite로 설정합니다.
    * Rect Transform을 아래와 같이 설정합니다.
      * Width : 268 / Height : 442\(적당한 크기로 설정합니다.\)
  * Text
    * Correct Object의 Child Object로써 TextMeshPro - Text\(UI\)를 추가하여 아래와 같이 설정합니다.
      * Font Style : B
      * Font Size : 50
      * Alignmnet : Center / Middle
    * Rect Transform을 아래와 같이 설정합니다.
      * Pos X : 0 / Pos Y : 307 / Pos Z : 0
      * Width : 200 / Height : 50

이 이상으로 설정값을 적으면 길어지기 때문에 Correct Object를 복제하여 나머지 Feedback의 Child Object인 Lose, Win를 Sprite만 변경해서 구성하시면 됩니다.



## 작성법 - Script

해당 프로젝트의 완전환 파일은 아래의 링크에서 다운 받으실 수 있습니다.

{% embed url="https://github.com/ValeriaColombo/unity\_tdd\_example" caption="Complete Project File " %}



* Script들을 생성하고 아래의 Project View 그림처럼 폴더가 나눠집니다.
* 각 폴더는 이름과 관련된 Script들이 모아져 있습니다.

![Scirpts&#xC758; &#xD558;&#xC704; &#xD3F4;&#xB354;](../../.gitbook/assets/image%20%2884%29.png)

{% tabs %}
{% tab title="AnswerView.cs" %}
{% code title="AnswerView.cs" %}
```csharp
using System;
using TMPro;
using UnityEngine;
namespace TriviaGame.Delivery {
    public class AnswerView: MonoBehaviour {
        [SerializeField] private TMP_Text _answerText;
        private Action<string> _onAnswerSelected;
        public void Initialize(Action < string > onAnswerSelected) {
            _onAnswerSelected = onAnswerSelected;
        }
        public void FillData(string answerText) {
            _answerText.text = answerText;
        }
        public void OnClick() {
            _onAnswerSelected ?. Invoke(_answerText.text);
        }
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="TriviaGameView.cs" %}
{% code title="TriviaGameView.cs" %}
```csharp
using System.Linq;
using TMPro;
using TriviaGame.Domain;
using TriviaGame.Presentation;
using UnityEngine;

namespace TriviaGame.Delivery
{
    public class TriviaGameView : MonoBehaviour
    {
        private TriviaGamePresenter _presenter;

        [SerializeField] private TMP_Text _scoreText;
        [SerializeField] private TMP_Text _questionText;
        [SerializeField] private AnswerView[] _answers;
        [SerializeField] private Animator _feedbackAnimations;

        private void Start()
        {
            _presenter = TriviaGamePresenterBuilder.BuildTriviaGamePresenter(this);
            foreach (var answerView in _answers)
            {
                answerView.Initialize(OnAnswerSelected);
            }
        }

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

        public virtual void ShowPositiveFeedback()
        {
            _feedbackAnimations.Play("RightAnswer");
        }

        public virtual void ShowWinFeedback()
        {
            _feedbackAnimations.Play("Win");
        }

        public virtual void ShowLoseFeedback()
        {
            _feedbackAnimations.Play("Lose");
        }

        public virtual void UpdateScore(int currentScore)
        {
            _scoreText.text = currentScore.ToString("000");
        }

        private void OnAnswerSelected(string selectedAnswer)
        {
            _presenter.ReceiveAnswer(selectedAnswer);
        }

    }
}
```
{% endcode %}
{% endtab %}

{% tab title="Question.cs" %}
{% code title="Question.cs" %}
```csharp
namespace TriviaGame.Domain {
    public class Question {
        public string QuestionText {
            get;
            private set;
        }
        public string RightAnswer {
            get;
            private set;
        }
        public string[] WrongAnswers {
            get;
            private set;
        }
        public Question() {}
        public Question(string questionText, string rightAnswer, string[] wrongAnswers) {
            QuestionText = questionText;
            RightAnswer = rightAnswer;
            WrongAnswers = wrongAnswers;
        }
        public virtual bool IsRightAnswer(string anAnswer) {
            return string.Equals(RightAnswer, anAnswer);
        }
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="TriviaGamePresenter.cs" %}
{% code title="TriviaGamePresenter.cs" %}
```csharp
using TriviaGame.Delivery;
using TriviaGame.Domain;
namespace TriviaGame.Presentation {
    public class TriviaGamePresenter {
        private TriviaGameView _view;
        private Question[] _questions;
        private int _currentQuestion;
        public int Score {
            get;
            private set;
        }
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
            } else {
                OnWrongAnswerReceived();
            }
        }
        private void OnWrongAnswerReceived() {
            _view.ShowLoseFeedback();
        }
        private void OnRightAnswerReceived() {
            Score ++;
            _currentQuestion ++;
            _view.UpdateScore(Score);
            if (Score == 3) {
                _view.ShowWinFeedback();
            } else {
                _view.ShowPositiveFeedback();
                _view.ShowNextQuestion(_questions[_currentQuestion]);
            }
        }
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="TriviaGamePresenterBuilder.cs" %}
{% code title="TriviaGamePresenterBuilder.cs" %}
```csharp
using TriviaGame.Delivery;
using TriviaGame.Service;
namespace TriviaGame.Presentation {
    public class TriviaGamePresenterBuilder {
        public static TriviaGamePresenter BuildTriviaGamePresenter(TriviaGameView view) {
            return new TriviaGamePresenter(view, ServicesProvider.QuestionsService().GetQuestions(3));
        }
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="QuestionsService.cs" %}
{% code title="QuestionsService.cs" %}
```csharp
using System.Collections.Generic;
using TriviaGame.Domain;
using Random = UnityEngine.Random;
namespace TriviaGame.Service {
    public class QuestionsService {
        private List<Question> _allQuestions;
        public QuestionsService() {
            _allQuestions = new List<Question>() {
                new Question("How many months are there in a year?", "12", new[]{"10", "15", "30"}),
                new Question("How many days are there in a week?", "7", new[]{"12", "6", "5"}),
                new Question("Witch of these is not an insect", "Cow", new[]{"Worm", "Fly", "Spider"}),
                new Question("Witch of these is not a country", "Paris", new[]{"England", "Argentina", "Spain"}),
                new Question("Witch of these is not a city", "Uruguay", new[]{"Tokyo", "Buenos Aires", "Madrid"})
            };
        }
        public Question[] GetQuestions(int amount) {
            _allQuestions.Sort(
                (a, b) => Random.Range(0, 2) == 0
                    ? 1
                    : -1
            );
            return _allQuestions.GetRange(0, amount).ToArray();
        }
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="ServicesProvider.cs" %}
{% code title="ServicesProvider.cs" %}
```csharp
namespace TriviaGame.Service {
    public class ServicesProvider {
        private static QuestionsService _questionsService = new QuestionsService();
        public static QuestionsService QuestionsService() {
            return _questionsService;
        }
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

* 위의 Script들을 생성 했다면 아래의 가이드라인을 따라 Object에 Script들을 추가합니다.
  * Answer 0 Object에 AnswerView.cs 추가 합니다.
    * OnClick\(\) Event에 Answer 0 Object를 넣고 Function을 AnswerView.Click으로 설정합니다.
    * AnswerView.cs의 AnswerText에는 Child Object인 AnswerText를 설정합니다.
  * 이런 방식으로 Answer 0~4 Object를 생성합니다.



* GamePlayScreen -- TriviaGameView.cs 추가 합니다.
  * Score Text, Question Text Object에는 Hierarchy에서 같은 이름의 Object로 설정합니다.
  * Answer Size 4로 조정합니다.
  * 생성한 Answer 0~4를 Answers에 넣습니다.
  * Feedback Animations 항목에 Feedback Animator를 넣습니다.

## 작성법 - TDD

* 이번 TDD는 Edit Mode를 사용한 Script 단위에서의 Testing 방식입니다.
* Play Mode로 하는 방식은 나중에 서술하겠습니다.
* Window -&gt; General -&gt; Test Runner창을 실행시키면 아래와 같은 그림이 나옵니다.

![Window -&amp;gt; General -&amp;gt; Test Runner -&amp;gt; Edit mode&#xC758; &#xAE30;&#xBCF8;&#xD654;&#xBA74;](../../.gitbook/assets/image%20%2867%29.png)

* Create EditMode Test Assembly Folder를 누르면 Test Runner에 필요한 Assembly Definition File을 생성합니다.
* 해당 파일은 아래와 같습니다.

![ProjectView&#xC758; Assembly File / Assembly File&#xC758; Inspector / Test Runner &#xCC3D;](../../.gitbook/assets/image%20%284%29.png)

* **위의 사진은 예시를 들기위한 사진이기 때문에 본문의 Complete Project File이 아닙니다.**
* 그 후 미리 생성한 TriviaGamePresenterTests.cs파일을 Test Folder에 넣습니다.
* TriviaGamePresenterTests.cs와 같은 TestCase Script는 아래와 같습니다.

{% tabs %}
{% tab title="TriviaGamePresenterTests.cs" %}
{% code title="TriviaGamePresenterTests.cs" %}
```csharp
using NSubstitute;
using NUnit.Framework;
using TriviaGame.Delivery;
using TriviaGame.Domain;
using TriviaGame.Presentation;

namespace Test.TriviaGame
{
    [TestFixture]
    public class TriviaGamePresenterTests {
        private TriviaGameView _view;
        private TriviaGamePresenter _presenter;

        private Question _firstQuestion = Substitute.For<Question>();
        private Question _secondQuestion = Substitute.For<Question>();
        private Question _thirdQuestion = Substitute.For<Question>();

        #region SetUp
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
        #region Given
        #endregion
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
        #region Then
        private void ThenScoreIsZero() {
            Assert.AreEqual(0, _presenter.Score);
        }
        private void ThenShowsPositiveFeedback() {
            _view.Received(1).ShowPositiveFeedback();
        }
        private void ThenScoreIncreasedByOne(int initialScore) {
            Assert.AreEqual(initialScore + 1, _presenter.Score);
        }
        private void ThenScoreDoesntChange(int initialScore) {
            Assert.AreEqual(initialScore, _presenter.Score);
        }
        private void ThenViewIsShowingTheFirstQuestion() {
            _view.Received(1).ShowNextQuestion(_firstQuestion);
        }
        private void ThenViewIsShowingTheSecondQuestion() {
            _view.Received(1).ShowNextQuestion(_secondQuestion);
        }
        private void ThenShowsWinningFeedback() {
            _view.Received(1).ShowWinFeedback();
        }
        private void ThenShowsLosingFeedback() {
            _view.Received(1).ShowLoseFeedback();
        }
        private void ThenShowsCurrentScore(int currentScore) {
            _view.Received().UpdateScore(currentScore);
        }
        #endregion

        [Test] public void ANewTriviaGameStartsWithZeroScore() {
            ThenScoreIsZero();
        }
        [Test] public void ANewGameShowsTheFirstQuestion() {
            ThenViewIsShowingTheFirstQuestion();
        }


        
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="QuestionsServiceTests.cs" %}
{% code title="QuestionsServiceTests.cs" %}
```csharp
using NUnit.Framework;
using TriviaGame.Service;

namespace Test.TriviaGame
{
    [TestFixture]
    public class QuestionsServiceTests
    {
        private QuestionsService _service;
        #region SetUp
        [SetUp]
        public void SetUp()
        {
            _service = new QuestionsService();
        }
        #endregion
        #region Test Case
        [Test]
        public void ReturnsTheRequiredAmountOfQuestions()
        {
            var amount = 3;            
            var questions = _service.GetQuestions(amount);
            Assert.AreEqual(amount, questions.Length);            
        }
        #endregion
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="QuestionTests.cs" %}
{% code title="QuestionTests.cs" %}
```csharp
using NUnit.Framework;
using TriviaGame.Domain;

namespace Test.TriviaGame
{
    [TestFixture]
    public class QuestionTests
    {
        private Question _question;

        #region SetUp
        [SetUp]
        public void SetUp()
        {
            _question = new Question("question text", "correct", new []{"incorrect 1", "incorrect 2", "incorrect 3"});
        }
        #endregion
        #region Test Case
        [Test]
        public void CheckCorrectAnswerReturnsCorrect()
        {
            Assert.IsTrue(_question.IsRightAnswer("correct"));
        }
        
        [Test]
        public void CheckIncorrectAnswerReturnsIncorrect()
        {
            Assert.IsFalse(_question.IsRightAnswer("incorrect 1"));            
        }
        #endregion
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

* 위의 방식대로 Script를 생성한다면 Test Runner에 다음과 같은 사진의 결과가 됩니다.

![Test Scirpt &#xCD94;&#xAC00; &#xD6C4; TestRunner&#xB97C; &#xB3CC;&#xB9B0; &#xACB0;&#xACFC;](../../.gitbook/assets/image%20%2865%29.png)

## 마치며

* TDD에 대한 Explanation 문서가 굉장히 길것 같습니다. 문서를 보실 때 최대한 가독성을 높이는 구성으로 작성하겠지만 불편한 점이 있다면 아래의 메일로 보완점이나, 바라는점을 보내주시면 반영하겠습니다.
* 8iioww@gmail.com

{% page-ref page="../../how-to-guide/unity/how-to-guide-triviagame-tdd.md" %}

