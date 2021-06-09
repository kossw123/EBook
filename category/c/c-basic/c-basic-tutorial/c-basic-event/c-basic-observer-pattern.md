---
description: 'C# Basic Observer Pattern'
---

# C\# Basic Observer Pattern

## 무엇을 하려고 하는가?

* Design Pattern중 Unity에서 주로 쓰이는 방법중 하나인 Observer Pattern에 대한 개념과 사용방법 및 사례에 관하여 기술합니다.

## Observer Pattern이란?

* Design Pattern중 하나로써 객체의 상태 변화를 관찰하는 Observer들의 목록을 Subject 객체에 등록하여, 상태의 변화가 있을 때 마다 함수를 통해 Subject의 Observer 목록을 가지고 있는 객체를 변화시킵니다.
  * 예시로 RTS 장르의 게임이 이와 유사한 패턴을 가지고 있습니다. 
* Observer와 Subject Class를 통해 Subject에서 객체를 등록하는 기능을 구현하고, Observer에서는 상태에 따른 변화를 등록합니다.
* 설명에 따른 코드를 아래의 예시처럼 작성했습니다.

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
namespace ObserverPattern { 
#region abstract class : 데이터가 없는 기능의 원리만 등록한 Class
    // 기능 클래
    abstract class Subject {
        private List<Observer> _observers = new List<Observer>();
        // 등록
        public void Attach(Observer p_observer) {
            _observers.Add(p_observer);
        }
        // 제거
        public void Detach(Observer p_observer) {
            _observers.Remove(p_observer);
        }
        // 알림
        public void Notify() {
            foreach(Observer item in _observers) {
                item.Update();
            }
        }
    }
    
    abstract class Observer { public abstract void Update(); }
#endregion
    
#region Concrete Class : 추상 클래스를 구현한 Class
    // 상태
    class ConcreteSubject: Subject {
        string _subjectState;
        public string GetState {
            get { return _subjectState; }
            set { _subjectState = value; }
        }
    }
    // 관찰자
    class ConcreteObserver: Observer {
        private string _name;
        private string _observerState;
        private ConcreteSubject _subject;
        public ConcreteObserver(ConcreteSubject subject, string name) {
            this._subject = subject;
            this._name = name;
        }
        // 상태 변경 함
        public override void Update() {
            _observerState = _subject.GetState;
            Console.WriteLine($ "Observer {_name}'s new state is {_observerState}");
        }
        public ConcreteSubject GetSubject {
            get { return _subject; }
            set { _subject = value; }
        }
    }
#endregion
    class Program {
        static void Main(string[] args) {
            ConcreteSubject s = new ConcreteSubject();
            s.Attach(new ConcreteObserver(s, "X"));
            s.Attach(new ConcreteObserver(s, "Y"));
            s.Attach(new ConcreteObserver(s, "Z"));
            s.GetState = "ABC";
            s.Notify();

            Console.ReadKey();
        }
    }
}

/*
    Output : 
    Observer X's new state is ABC
    Observer Y's new state is ABC
    Observer Z's new state is ABC
*/
```

코드의 결과가 Output 처럼 출력이 되면, s.Attach\(new ConcreteObserver\(s, "X"\)\); 의 문장을 쓴다면, 하나의 Observer를 등록하고 등록한 Observer의 name은 X이고, Observer의 상태는 s의 객체\(ConcreteSubject\)가 됩니다.

해당 상태의 그림은 아래와 같습니다.

![&#xC0DD;&#xC131;&#xD55C; ConcreteObserver&#xB294; &#xD558;&#xB098;&#xC758; ConcreteSubject&#xB97C; &#xAC00;&#xB9AC;&#xD0A8;&#xB2E4;.](../../../../../.gitbook/assets/image%20%28216%29.png)

위의 그림과 같은 상태에서 Observer Class에 정보를 입력하면 ConcreteSubject Class의 Observer List에 관한 기능이 작동하며, 동작합니다.





