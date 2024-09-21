---
description: 'Автор: Дмитрий Прокопьев'
---

# 🔥 Удаление объектов и компонентов

Динамически удалять объекты и компоненты на сцене может только компонент, который сам находится на этой сцене.

## Как удалить объект или компонент?

В классе MonoBehaviour есть метод, предназначенный для удаления - это метод `Destroy`.

У него есть несколько перегрузок:

1. Передать `GameObject`. Переданный объект будет удален целиком:

```csharp
Transform spawnPoint = _coin.transform.parent;

Destroy(spawnPoint.gameObject); // удалили весь объект
```

2. Передать `MonoBehaviour`. Переданный компонент будет удален со своего объекта. Сам объект удален не будет:

```csharp
Rigidbody rigidbody = _box.GetComponent<Rigidbody>();

Destroy(rigidbody); // убрали компонент Rigidbody с объекта
```

Метод `Destroy` также может принимать второй аргумент - время до отложенного удаления в секундах:

```csharp
Transform spawnPoint = _coin.transform.parent;

Destroy(spawnPoint.gameObject, 2f); // удалить объект через 2 секунды
```

