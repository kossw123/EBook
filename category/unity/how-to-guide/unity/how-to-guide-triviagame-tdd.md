---
description: How-to-guide TriviaGame TDD
---

# How-to-guide TriviaGame TDD

## 무엇을 하려고 하는가?

* Script에 쓰인 모든 Script에 대한 보완점이나 간단한 코드리뷰를 하려고 합니다.
* 이 문서를 상업적으로 이용하지 않습니다.

## Code Review - TriviaGame

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
        public string QuestionText { get; private set; }
        public string RightAnswer { get; private set; }
        public string[] WrongAnswers { get; private set; }
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
        public int Score { get; private set; }
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

## Code Review - TDD

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

namespace Test.TriviaGame {
    [TestFixture] public class QuestionsServiceTests {
        private QuestionsService _service;
        #region SetUp
        [SetUp]
        public void SetUp()
        {
            _service = new QuestionsService();
        }
        #endregion
        #region Test Case
        [Test] public void ReturnsTheRequiredAmountOfQuestions() {
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

namespace Test.TriviaGame {
    [TestFixture] public class QuestionTests {
        private Question _question;
        #region SetUp
        [SetUp] public void SetUp() {
            _question = new Question("question text", "correct", new []{"incorrect 1", "incorrect 2", "incorrect 3"});
        }
        #endregion
        #region Test Case
        [Test] public void CheckCorrectAnswerReturnsCorrect() {
            Assert.IsTrue(_question.IsRightAnswer("correct"));
        }
        [Test] public void CheckIncorrectAnswerReturnsIncorrect() {
            Assert.IsFalse(_question.IsRightAnswer("incorrect 1"));            
        }
        #endregion
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Code Review가 없는 이유

* 다양한 패턴 및 문법을 사용하다 보니, 이 내용들이 혼합해서 정리되었고, 이를 분리할만한 성격이 아니라고 판단이 되어 How-to-guide는 작성했지만, 해설문서에서 한번에 다루도록 하겠습니다.

{% page-ref page="../../explanation/unity/explanation-triviagame-tdd.md" %}

