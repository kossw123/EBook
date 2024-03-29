# 티켓 판매 애플리케이션 구현하기

### Object Handling을 예시를 티켓 애플리케이션 구현을 통해 구체화하도록 한다.

#### 티켓 애플리케이션은 다음과 같은 특성을 가지고 있어야한다.

1. 티켓을 구매, 판매하는 시스템
2. 그러나 소소한 이벤트가 다음과 같은 방식으로 추가됨
   1. 추첨을 통해 선정된 대상에게 무료로 공연을 관람할 수 있는 티켓을 발송

여기서 한가지 염두해야 할 점은 다음과 같다.

1. 이벤트 당첨된 관람객과 그렇지 못한 관람색을 다른 방식으로 입장해야 한다.
2. 이벤트에 당첨된 관람객은 초대장을 티켓으로 교환한 후에 입장

결과 적으로 관람객을 입장시키기 전에 이벤트 당첨 여부를 확인하고 그에 따라 다른 방식으로 입장 시켜야 한다.





우선 Invitation(초대) 객체를 구현해 보자&#x20;

```csharp
using System;

public class Invitation
{
    private DateTime when;
}
```





다음으로 Ticket(티켓) 객체를 구현해 보자&#x20;

```csharp
public class Ticket
{
    private Long fee;
    public long GetFee()            { return fee; }
}
```





### 이벤트 당첨자는 티켓으로 교환할 초대장을 가지고 있을 것이고, 그렇지 않는 사람은 현금을 소지하고 있을 것이다.

따라서 관람객이 가질 수 있는 소지품은 초대장, 현금, 티켓 3가지 밖에 없다.

이때 관람객은 가방을 가지고 있다고 가정하자. 그렇다면 다음과 같은 코드를 가지고 구현할 수 있다.

```csharp
public class Bag
{
        private long amount;
        private Invitation invitation;
        private Ticket ticket;

        public Bag(long amount) : this(null, amount)        { }
        public Bag(Invitation invitation, long amount)
        {
            this.invitation = invitation;
            this.amount = amount;
        }

        public bool HasInvitation()                 { return invitation != null; }
        public bool HasTicket()                     { return ticket != null; }

        public void SetTicket(Ticket ticket)        => this.ticket = ticket;
        public void MinusAmount(long amount)        => this.amount -= amount;
        public void PlusAmount(long amount)         => this.amount += amount;
}
```





Bag Class의 상태는 초대장 소유 여부에 따라 상태가 달라지기 때문에, 관람객의 기본정보를 받아올 때, 이를 확인하여 Runtime에서 Binding하도록한다.

그러면 아래와 같은 경우로 나눠질 것이고, 이에 기초하여 코드를 구현한다.

1. 이벤트가 당첨된 관람객의 가방에는 현금과 초대장이 들어있지만,
2. 그렇지 않는 관람객의 경우 초대장이 들어있지 않을 것이다.

Bag은 관람객의 가방 정보를 가리키기 때문에, 생성자를 통해 instance를 생성하는 시점에서 강제한다.&#x20;

```csharp
public class Bag
{
    public Bag(Long amount)
    {
        this(null, amount);
    }
    public Bag(Invitation invitation, Long amount)
    {
        this.invitation = invitation;
        this.amount = amount;
    }
}
```





그 다음 관람객의 객체를 구현해보자. 관람객은 소지품을 보관하기 위한 Bag Class를 가지고 있다.

```csharp
public class Audience
{
        private Bag bag;
        public Audience(Bag bag)        { this.bag = bag; }
        public Bag GetBag()             { return bag; }
}
```





이때 관람객이 입장하기 위해서는 매표소에서 티켓으로 교환하거나 구매를 해야한다.&#x20;

따라서 매표소에는 관람객에게 판매할 티켓의 판매 금액이 보관돼 있어야 한다. 이를 구현하기 위해 TicketOffice Class를 추가하도록 하자.

TicketOffice에서는 판매하거나 교환해 줄 Ticket의 목록과 판매 금액을 가지고 있어야 한다. 부차적으로 판매 금액을 더하거나 차감하는 함수도 구현하도록 하자.

