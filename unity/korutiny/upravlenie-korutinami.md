---
description: 'Автор: Дмитрий Прокопьев'
---

# 🪄 Управление корутинами

## Как запустить корутину?

Для этого в классе MonoBehaviour есть метод `StartCoroutine`, он принимает в себя `IEnumerator` - результат вызова метода корутины:

```csharp
Coroutine coroutine = StartCoroutine(DoSomething());
```

## Как сохранить корутину?

Сохранять корутину нужно не всегда, иногда мы можем запустить ее и забыть про нее. Но если мы хотим дальше ее контролировать, то ее будет удобно сохранить в поле:

```csharp
private Coroutine _coroutine;

private void Start()
{
    _coroutine = StartCoroutine(DoSomething());
}
```

## Как остановить корутину?

Для остановки в классе `MonoBehaviour` есть метод `StopCoroutine`:

```csharp
StopCoroutine(_coroutine);
```

В качестве параметра он принимает в себя экземпляр класса `Coroutine` - ту корутину, которую мы хотим остановить. Для этого мы обычно корутины и сохраняем.

**Важно**. До первой инициализации поле с корутиной будет равно `null`. Важно предусмотреть это:

```csharp
if (_coroutine != null)
    StopCoroutine(_coroutine);
```

А так можно остановить все корутины на объекте:

```csharp
StopAllCoroutines();
```

**Важно**. Мы используем метод `StopAllCoroutines()` только когда хотим остановить все без исключения корутины компонента. Такие сценарии крайне редкие, поэтому и метод мы применяем редко.

## Полноценный пример управления корутиной

<pre class="language-csharp" data-title="LightSwitcher.cs"><code class="lang-csharp"><strong>public class LightSwitcher : MonoBehaviour
</strong>{
    [SerializeField] private float _delay;
    [SerializeField] private Light _light;
    
    private Coroutine _coroutine;

    private void Start()
    {
        Restart();
    }
    
    public void Stop()
    {
        if (_coroutine != null)
            StopCoroutine(_coroutine);
    }
    
    public void Restart()
    {
        _coroutine = StartCoroutine(SwitchLighting(_delay));
    }

    private IEnumerator SwitchLighting(float delay)
    {
        var wait = new WaitForSeconds(delay);

        while (enabled)
        {
            _light.enabled = !_light.enabled;
            yield return wait;
        }
    }
}
</code></pre>
