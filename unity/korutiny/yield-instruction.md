---
description: 'Автор: Дмитрий Прокопьев'
---

# ⏰ Yield Instruction

`YieldInstruction` это базовый класс для всех `yield` инструкций. Если класс наследуется от `YieldInstruction`, то его можно передать в `yield return`.&#x20;

Мы можем самостоятельно реализовать такой класс или воспользоваться уже существующими.

## Какие yield инструкции уже реализованы в Unity?

### [WaitForEndOfFrame](https://docs.unity3d.com/ScriptReference/WaitForEndOfFrame.html)

```csharp
yield return new WaitForEndOfFrame();
```

Ожидает момента, когда **текущий кадр** был полностью рассчитан и готов к отрисовке. Вызывается до самой отрисовки - камеры готовы нарисовать изображение, но еще не сделали этого.

### [WaitForFixedUpdate](https://docs.unity3d.com/ScriptReference/WaitForFixedUpdate.html)

```csharp
yield return new WaitForFixedUpdate();
```

Ожидает **следующий** вызов `FixedUpdate.`

### [WaitForSeconds](https://docs.unity3d.com/ScriptReference/WaitForSeconds.html) (float t)

```csharp
yield return new WaitForSeconds(1f);
```

Ожидает пока пройдет `t` секунд c **момента завершения текущего кадра.** Время считается с коэффициентом `Time.timeScale`.

**Важно.** Так как корутины выполняются в основном цикле обновлений кадров (`Update`), между вызовами пройдет не точно `t` секунд. Выполнение корутины продолжится в **первом кадре после прохождения** `t` **секунд** c **момента окончания текущего кадра.**

### [WaitForSecondsRealtime](https://docs.unity3d.com/ScriptReference/WaitForSecondsRealtime.html) (float t)

```csharp
yield return new WaitForSecondsRealtime(1f);
```

Ожидает пока пройдет `t` секунд c **момента завершения текущего кадра.** Время считается без коэффициентов, поэтому совпадает с реальным. В остальном этот класс работает как `WaitForSeconds.`

### [WaitUntil](https://docs.unity3d.com/ScriptReference/WaitUntil.html) (Func\<Bool> func)

{% code overflow="wrap" fullWidth="true" %}
```csharp
// ждем момента, когда объект перестанет двигаться
yield return new WaitUntil(() => _rigidbody.velocity == Vector3.zero);
```
{% endcode %}

Принимает в себя делегат, возвращающий `bool`, и ожидает момента, когда он вернет `true`. До этого момента делегат вызывается каждый кадр.

### [WaitWhile](https://docs.unity3d.com/ScriptReference/WaitWhile.html) (Func\<Bool> func)

```csharp
// ждем пока коллайдер не выключят
yield return new WaitWhile(() => _collider.enabled);
```

Принимает в себя делегат, возвращающий `bool`, и ожидает момента, когда он вернет `false`. До этого момента делегат вызывается каждый кадр.

### null

```csharp
yield return null;
```

Мы можем вернуть null, если хотим продолжить выполнение корутины в следующем кадре без особых условий

### Coroutine

Сами корутины тоже могут выступать в роли yield инструкции:

```csharp
// выполнить новую корутину внутри текущей
yield return StartCoroutine(MoveSmoothly());
// ожидать завершения уже начатой корутины
yield return _coroutine; // поле с запущенной корутиной
```

Таким образом можно дождаться окончания другой корутины и только потом продолжить выполнить текущую

### break

При необходимости выполнение корутины можно завершить вручную:

```csharp
yield break;
```

После такой инструкции корутина завершится.
