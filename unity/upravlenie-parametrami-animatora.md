---
description: 'Автор: Дмитрий Прокопьев'
---

# 🚶‍♂️ Управление Параметрами Аниматора

Чтобы менять параметры аниматора нужно для начала получить ссылку на компонент `Animator`, с которым мы хотим работать.

{% code fullWidth="false" %}
```csharp
private Animator _animator;

private void Start()
{
    _animator = GetComponent<Animator>();
}
```
{% endcode %}

Для каждого типа параметра есть свои методы. Эти методы принимают имя или id параметра (его числовое представление в аниматоре):

```csharp
private const string Speed = nameof(Speed);
private const string IsGrounded = nameof(IsGrounded);
private const string StepsAmount = nameof(StepsAmount);
private const string Attack = nameof(Attack);

private void UseParameters()
{
    _animator.GetFloat(Speed);
    _animator.GetBool(IsGrounded);
    _animator.GetInteger(StepsAmount);
    // Значение триггера нельзя получить, так как его нет
}

private void Setup(float speed, bool isGrounded, int stepsAmount, bool shouldAttack)
{
    _animator.SetFloat(Speed, speed);
    _animator.SetBool(IsGrounded, isGrounded);
    _animator.SetInteger(StepsAmount, stepsAmount);
    
    if (shouldAttack)
        _animator.SetTrigger(Attack);
}
```

Несмотря на то, что мы можем передать имя в качестве параметра, внутренняя логика аниматора использует id параметра вместо его имени для работы. По этой причине при каждом вызове этих методов аниматору приходится их пересчитывать, что плохо влияет на производительность. Решить эту проблему можно заранее посчитав _id_ каждого параметра, в этом поможет статический метод класса `Animator` - `StringToHash`:

```csharp
public readonly int Speed = Animator.StringToHash(nameof(Speed));
```

Этот метод возвращает `int` - _id_ данного параметра. Если таких параметров много, то их желательно организовать отдельно:

```csharp
public static class PlayerAnimatorData
{
    public static class Params
    {
        public static readonly int Speed = Animator.StringToHash(nameof(Speed));
        public static readonly int IsGrounded = Animator.StringToHash(nameof(IsGrounded));
        public static readonly int StepsAmount = Animator.StringToHash(nameof(StepsAmount));
        public static readonly int Attack = Animator.StringToHash(nameof(Attack));
    }
}
```

Финальный вариант:

```csharp
private void LogParameters()
{
    float speed = _animator.GetFloat(PlayerAnimatorData.Params.Speed);
    bool isGrounded = _animator.GetBool(PlayerAnimatorData.Params.IsGrounded);
    int stepsAmount = _animator.GetInteger(PlayerAnimatorData.Params.StepsAmount);

    Debug.Log($"Текущая скорость - {speed}");
    Debug.Log(isGrounded ? "Мы стоим на земле" : "Мы падаем");
    Debug.Log($"Мы делаем {stepsAmount} шагов чтобы разогнаться");
}

private void Setup(float speed, bool isGrounded, int stepsAmount, bool shouldAttack)
{
    _animator.SetFloat(PlayerAnimatorData.Params.Speed, speed);
    _animator.SetBool(PlayerAnimatorData.Params.IsGrounded, isGrounded);
    _animator.SetInteger(PlayerAnimatorData.Params.StepsAmount, stepsAmount);
    
    if (shouldAttack)
        _animator.SetTrigger(PlayerAnimatorData.Params.Attack);
}
```

