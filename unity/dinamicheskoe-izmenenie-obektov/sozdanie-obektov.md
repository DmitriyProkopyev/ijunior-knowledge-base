---
description: 'Автор: Дмитрий Прокопьев'
---

# 🛠️ Создание объектов

## Как создавать объекты динамически?

В ситуациях когда мы хотим добавить объект на сцену динамически нам потребуется компонент, который уже находится на сцене - некоторый `Spawner`, действующий по нашим правилам.

Такой компонент может создавать другие объекты на текущей сцене, так как он сам находится на ней. Для этого используется метод `Instantiate`.

**Важно**. Метод `Instantiate` принадлежит классу `MonoBehaviour`, поэтому вызывать его можно только из компонента-наследника `MonoBehaviour`.

## Метод `Instantiate`

Может принимать в себя либо `GameObject`, либо `MonoBehavior`:

1. Если передать `GameObject`, то метод создаст его копию на текущей сцене и вернёт ссылку на нее с типом `GameObject`:

{% code overflow="wrap" %}
```csharp
[SerializeField] private GameObject _prefab; 
// префаб монеты

private void Spawn()
{
    GameObject copy = Instantiate(_prefab); 
    // копия монеты
}
```
{% endcode %}

2. Если передать `MonoBehaviour`, то метод создаст копию объекта, которому принадлежит переданный компонент. Затем метод вернёт ссылку на тот же компонент в копии с тем же типом, который был передан.

{% code overflow="wrap" %}
```csharp
[SerializeField] private Rigidbody _prefab;
// компонент Rigidbody префаба монеты

private void Spawn()
{
    Rigidbody copy = Instantiate(_prefab); 
    // компонент Rigidbody копии монеты
}
```
{% endcode %}

Второй вариант предпочтителен, так как он предлагает четкую типизацию. Допускается создание пустых компонентов для разметки объектов:

```csharp
public class Coin : MonoBehaviour { }
```

Позже эти компоненты зачастую перестают быть пустыми, так как выясняется, что требуется дополнительная функциональность. Таким образом, лучше всего хранить ссылку на тот компонент префаба, с которым нам нужно будет работать в копиях.

## Перегрузки метода `Instantiate`

Рассмотрим часто применяемые перегрузки:

1. Префаб + transform объекта-родителя

```csharp
Instantiate(_prefab, transform);
```

2. Префаб + позиция + поворот

```csharp
Instantiate(_prefab, transform.position + Vector3.right, Quaternion.identity);
```

Все перегрузки метода Instantiate можно посмотреть [здесь](https://docs.unity3d.com/ScriptReference/Object.Instantiate.html).
