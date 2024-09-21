---
description: 'Автор: Дмитрий Прокопьев'
---

# 📡 Action и UnityAction

## Что такое Action и UnityAction?

`Action` и `UnityAction` это типы делегатов `C#`, которые можно использовать в качестве типа события. Посмотрим на пример объявления события:

```csharp
public event Action AmountChanged;
```

1. Объявление начинается с модификатора доступа. Так как события используются как средство передачи информации, это всегда `public`
2. Ключевое слово `event` инкапсулирует событие, ограничивает количество доступных операций. Подробнее при это [здесь](broken-reference)
3. Тип делегата для этого события определяет поведение и передаваемые параметры события
4. Название события указывает, какую информацию оно передает. События всегда именуются в прошедшем времени, так как указывают на уже произошедшее.

Именование происходит в формате **Существительное** _(какой объект изменился?)_ + **Глагол** (как объект изменился?). Например, `MoneyAdded`, `EnemyKilled`, `DoorOpened`, `PaymentReceived` и.т.д.

## В чем разница между Action и UnityAction?

Эти типы отличаются несколькими параметрами:

1. Название
2. `Action` находится в `namespace System`, а `UnityAction` в `namespace UnityEngine`
3. `Action` может принимать от 0 до 16 параметров, а `UnityAction` от 0 до 4.
4. `UnityAction` может в некоторых обстоятельствах работать некорректно, в отличие от `Action`

По указанным выше причинам рекомендуем использовать только `Action`.

## Как вызвать объявленное событие?

Нам известно, что событие передает информацию своим слушателям. Поэтому возникает вопрос, как запустить процесс передачи информации?

Продолжим рассматривать пример с кошельком из [введения в тему событий](./):

```csharp
public class Wallet : MonoBehaviour
{
    public int Money { get; private set; }
    
    public event Action AmountChanged;
    
    public void AddMoney(int amount)
    {
        if (amount <= 0)
            return;
        
        Money += amount;
    }
    
    public bool TrySpendMoney(int amount)
    {
        if (amount <= 0)
            return;
        
        bool isEnough = Money >= amount;
        
        if (isEnough)
            Money -= amount;
        
        return enough;
    }
}
```

Мы добавили кошельку событие `AmountChanged`, через которое он сообщает, что количество денег изменилось. Для начала подумаем, когда об этом нужно сообщать. Количество денег изменяется только в методах `AddMoney` и `TrySpendMoney`, поэтому вызывать событие будет логично именно в них. Сделать это можно c помощью метода `Invoke`:

```csharp
AmountChanged.Invoke();
```

Однако есть одна проблема. Так как такое событие это делегат, то до первого подписанного метода оно не будет проинициализировано, а значит будет равно `null`. Из за этого в момент вызова метода `Invoke` мы получим `NullReferenceException`. Решение простое, для этого нужно проверить событие на `null` перед вызовом:

```csharp
if (AmountChanged != null)
    AmountChanged.Invoke();
```

Правда каждый раз расписывать это заново неудобно, поэтому для этого есть краткая форма записи:

```csharp
AmountChanged?.Invoke();
```

## Как получать информацию из события?

Для этого другим компонентам потребуется подписаться на событие. Но как указать, что именно им делать при вызове события? Чтобы разобраться, попробуем привязать к кошельку компонент его отображения - `WalletView`. Так он реализован сейчас:

```csharp
public class WalletView : MonoBehaviour
{
    [SerializeField] private Text _amountView;
    [SerializeField] private Wallet _wallet;
    
    private void DisplayAmount()
    {
        float amount = _wallet.Money;
        _amountView.text = amount.ToString();
    }
}
```

Мы хотим, чтобы при срабатывании _события_ `AmountChanged` вызывался метод `DisplayAmount`. Тогда каждый раз, когда количество денег в кошельке изменится, оно будет выведено на экран, чего мы и хотели.

Для этого мы можем _подписаться_ на это _событие_ методом `DisplayAmount`. Сделать это нам поможет оператор `+=`, вот так:

```csharp
_wallet.AmountChanged += DisplayAmount;
```

По техническим причинам необходимо всегда отписываться от событий, когда мы больше не хотим принимать информацию из них. Для этого есть оператор `-=`:

```csharp
_wallet.AmountChanged -= DisplayAmount;
```

Но когда это делать? Подписываться нужно с того момента, когда мы хотим начать слушать событие, а отписываться нужно, когда мы больше не хотим это делать. Но в этом случае мы всегда хотим получать информацию с кошелька!

Тогда будет удобно сделать это в методах `OnEnable` и `OnDisable`, так как они выполняются при включении и выключении объекта, включая начало и конец рантайма:

{% code title="WalletView.cs" %}
```csharp
public class WalletView : MonoBehaviour
{
    [SerializeField] private Text _amountView;
    [SerializeField] private Wallet _wallet;
    
    private void OnEnable()
    {
        _wallet.AmountChanged += DisplayAmount;
    }

    private void OnDisable()
    {
        _wallet.AmountChanged -= DisplayAmount;
    }
    
    private void DisplayAmount()
    {
        float amount = _wallet.Money;
        _amountView.text = amount.ToString();
    }
}
```
{% endcode %}

А так теперь выглядит сам кошелек:

{% code title="Wallet.cs" %}
```csharp
public class Wallet : MonoBehaviour
{
    public int Money { get; private set; }
    
    public event Action AmountChanged;
    
    public void AddMoney(int amount)
    {
        if (amount <= 0)
            return;
        
        Money += amount;
        AmountChanged?.Invoke();
    }
    
    public bool TrySpendMoney(int amount)
    {
        if (amount <= 0)
            return;
        
        bool enough = Money >= amount;
        
        if (enough) 
        {
            Money -= amount;
            AmountChanged?.Invoke();
        }
        
        return enough;
    }
}
```
{% endcode %}

И вот, это наконец-то работает! Теперь `WalletView` реагирует на каждое изменение в `Wallet` и отображает это на экране!
