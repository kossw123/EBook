---
description: How-to-guide TriviaGame TDD
---

# How-to-guide TriviaGame TDD - 작성중

## 무엇을 하려고 하는가?

* Script에 쓰인 모든 Script에 대한 보완점이나 간단한 코드리뷰를 하려고 합니다.
* 이 문서를 상업적으로 이용하지 않습니다.

## Code Review

{% tabs %}
{% tab title="AnswerView.cs" %}
{% code title="AnswerView.cs" %}
```csharp
using System;
using TMPro;                                    // TextMesh Pro를 사용하기 위한 명령문
using UnityEngine;
namespace TriviaGame.Delivery {                // namespace를 통해 다른기능 같은이름의 모호성을 방지하기 위해 사용
    public class AnswerView: MonoBehaviour {
        [SerializeField] private TMP_Text _answerText;
        private Action<string> _onAnswerSelected;    // string 형 Action을 통해 함수 포인터를 받는다.
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

* 
