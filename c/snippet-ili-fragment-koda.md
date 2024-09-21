---
description: 'Автор: Александр Михновец'
---

# 🔖 Snippet или фрагмент кода

## Часть 1. Что такое Snippet или фрагмент кода?

Набирая код, очень часто приходится повторять однотипные действия – например набирать по буквам `Console.WriteLine();`

Со временем начинаем замечать подсказки для быстрого введения кода, которое предлагает IntelliSense

Параметры IntelliSense включены по умолчанию. Чтобы отключить их, перейдите в раздел _Сервис > Параметры > Текстовый редактор > Все языки_ и снимите флажок Сведения о параметрах или Автоматический список участников, если вы не хотите использовать функцию Списка участников.

<figure><img src="../.gitbook/assets/image (43).png" alt=""><figcaption><p>IntelliSense предлагает дополнить текст до "Console"</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (44).png" alt=""><figcaption><p>IntelliSense предлагает дополнить текст до "Console.WriteLine"</p></figcaption></figure>

Потом узнаем, что есть комбинация клавиш для быстрого написания `Console.WriteLine();` _- **cw**_ и 2 раза клавиша **Tab,** и дело идет гораздо быстрее.

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption><p>Этот значок указывает на snippet (в английской редакции) или фрагмент кода (в русской редакции)</p></figcaption></figure>

У **Microsoft Visual Studio** есть целый комплект сниппетов – часто встречающихся комбинаций клавиш для быстрого ввода. Найти их перечень можно в _Средства_ – _Диспетчер фрагментов кода_ или набрав сочетание **Ctrl+K, Ctrl+B**

<figure><img src="../.gitbook/assets/image (46).png" alt=""><figcaption><p>Диспетчер фрагментов кода в меню</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (47).png" alt=""><figcaption><p>Выбираем опцию CSharp</p></figcaption></figure>

![](<../.gitbook/assets/image (49).png>)![](<../.gitbook/assets/image (50).png>)![](<../.gitbook/assets/image (51).png>)

Здесь представлены все стандартные сниппеты, которые есть для языка `C#`.

Т.е. набрав сочетание клавиш, указанное выше, например, `for` и нажав 2 раза клавишу **Tab**

Основной скелет цикла for появится в коде и его останется только заполнить:

![](<../.gitbook/assets/image (52).png>)

Обратите внимание, что часть кода, которую надо заменить – сразу подсвечивается. Использование snippet’ов сокращает в разы время набора часто повторяющихся фрагментов кода.

## Часть 2. Создаем свой Snippet или фрагмент кода

Освоившись с применением snippet начинаем понимать, что стандартных маловато будет. И нам срочно нужен фрагмент кода для `Console.ReadLine();` которого нет в перечне.

Открываем диспетчер фрагментов кода:

![](<../.gitbook/assets/image (53).png>)

Выбираем папку `Visual C#`:

<figure><img src="../.gitbook/assets/image (54).png" alt=""><figcaption><p>Папка Visual C#</p></figcaption></figure>

Копируем путь к папке:

<figure><img src="../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

Переходим в указанное место через Проводник / Total Commander / или что там у Вас для этого:

<figure><img src="../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

Копируем `cw.snippet` в новую папку `cr`, где будем изменять файл под себя. Переименовываем файл в `cr.snippet` и открываем в текстовом редакторе:

```xml
<?xml version="1.0" encoding="utf-8"?>
<CodeSnippets xmlns="http://schemas.microsoft.com/VisualStudio/2005/CodeSnippet">
    <CodeSnippet Format="1.0.0">
        <Header>
            <Title>cw</Title>
            <Shortcut>cw</Shortcut>
            <Description>Фрагмент кода для Console.WriteLine</Description>
            <Author>Microsoft Corporation</Author>
            <SnippetTypes>
                <SnippetType>Expansion</SnippetType>
            </SnippetTypes>
        </Header>
        <Snippet>
            <Declarations>
                <Literal Editable="false">
                    <ID>SystemConsole</ID>
                    <Function>SimpleTypeName(global::System.Console)</Function>
                </Literal>
            </Declarations>
            <Code Language="csharp"><![CDATA[$SystemConsole$.WriteLine($end$);]]>
            </Code>
        </Snippet>
    </CodeSnippet>
</CodeSnippets>
```

1. В строчке `<Code Language="csharp"><![CDATA[$SystemConsole$.WriteLine($end$);]]>` заменяем `Write` на `Read` и переносим `$end$` за точку с запятой. Получаем такую запись: `<Code Language="csharp"><![CDATA[$SystemConsole$.ReadLine();$end$]]>`
2. В `<Title>cw</Title>` меняем на `<Title>cr</Title>`
3. В `<Shortcut>cw</Shortcut>` меняем на `<Shortcut>cr</Shortcut>`
4.  В `<Description>Фрагмент кода для Console.WriteLine</Description>`

    меняем на `<Description>Фрагмент кода для Console.ReadLine</Description>`
5. Авторство `<Author>Microsoft Corporation</Author>` меняем на себя любимого `<Author>IJunior</Author>`

Осталось импортировать новый snippets в **Microsoft visual studio**.

Средства – Диспетчер фрагментов кода или сочетание **Ctrl+K**, **Ctrl+B**:

<figure><img src="../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

Язык `CSharp`, расположение **MyCodeSnippets**, кнопка **Импорт**.

Указываем путь к нашему созданному файлу и нажимаем кнопку **Открыть**.

Переходим в **Microsoft Visual Studio**, запускаем проект и вводим `cr` и 2 раза клавишу **Tab**

<figure><img src="../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

## Часть 3. Модифицируем стандартный Snippet или Фрагмент кода

Меня не устраивает стандартный snippet cw для `Console.WriteLine()`, т.к. все время использую интерполяцию. Делать новый фрагмент кода ради добавления в него `Console.WriteLine($””)` не имеет смысла, поэтому будем модифицировать стандартный snippet.

Для начала сделаем резервное копирование стандартного сниппета `cw` на случай, если что-то пойдет не так.

Переходим к месту хранения сниппетов и копируем его оттуда в свою отдельную папку резервных копий и папку для препарирования.

<figure><img src="../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

Так выглядел код до модификации:

```xml
<?xml version="1.0" encoding="utf-8"?>
<CodeSnippets xmlns="http://schemas.microsoft.com/VisualStudio/2005/CodeSnippet">
    <CodeSnippet Format="1.0.0">
        <Header>
            <Title>cw</Title>
            <Shortcut>cw</Shortcut>
            <Description>Фрагмент кода для Console.WriteLine</Description>
            <Author>Microsoft Corporation</Author>
            <SnippetTypes>
                <SnippetType>Expansion</SnippetType>
            </SnippetTypes>
        </Header>
        <Snippet>
            <Declarations>
                <Literal Editable="false">
                    <ID>SystemConsole</ID>
                    <Function>SimpleTypeName(global::System.Console)</Function>
                </Literal>
            </Declarations>
            <Code Language="csharp"><![CDATA[$SystemConsole$.WriteLine($end$);]]>
            </Code>
        </Snippet>
    </CodeSnippet>
</CodeSnippets>
```

Так после модификации:

```xml
<?xml version="1.0" encoding="utf-8"?>
<CodeSnippets xmlns="http://schemas.microsoft.com/VisualStudio/2005/CodeSnippet">
    <CodeSnippet Format="1.0.0">
        <Header>
            <Title>cw</Title>
            <Shortcut>cw</Shortcut>
            <Description>Фрагмент кода для Console.WriteLine</Description>
            <Author>OldBeliever</Author>
            <SnippetTypes>
                <SnippetType>Expansion</SnippetType>
            </SnippetTypes>
        </Header>
        <Snippet>
            <Declarations>
                <Literal Editable="false">
                    <ID>SystemConsole</ID>
                    <Function>SimpleTypeName(global::System.Console)</Function>
                </Literal>
                <Literal Editable="false">
                    <ID>Interpolation</ID>
                    <Function>SimpleTypeName($)</Function>
                </Literal>
            </Declarations>
            <Code Language="csharp"><![CDATA[$SystemConsole$.WriteLine($Interpolation$"$end$");]]>
            </Code>
        </Snippet>
    </CodeSnippet>
</CodeSnippets>
```

Единственной загвоздкой было выведение **знака $**, который воспринимался, как внутренняя команда. Пришлось сделать для него отдельный литерал.

Добавил строчки:

```xml
<Literal Editable="false">
    <ID>Interpolation</ID>
    <Function>SimpleTypeName($)</Function>
</Literal>
```

Изменил:

`<Code Language="csharp"><![CDATA[$SystemConsole$.WriteLine($end$);]]>`

На

`<Code Language="csharp"><![CDATA[$SystemConsole$.WriteLine($Interpolation$"$end$");]]>`

## Часть 4. Примеры горячих клавиш для фрагментов кода

* сс - `Console.Clear();`
* cr - `Console.ReadLine();`
* crk - `Console.ReadKey();`
* _cw -_ `Console.WriteLine($"");`
* cww - `Console.WriteLine($"{}");`

