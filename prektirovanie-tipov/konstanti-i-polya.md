# Константы и поля

### Константы

Константа \(constant\) — это идентификатор, значение которого никогда не меняется. Значение, связанное с именем константы, должно определяться во время компиляции. Затем компилятор сохраняет значение константы в метаданных модуля. Это значит, что константы можно определять только для таких типов, которые компилятор считает примитивными. В C\# следующие типы считаются примитивными и могут использоваться для определения констант: Boolean, Char, Byte, SByte, Int16, UInt16, Int32, UInt32, Int64, UInt64, Single, Double, Decimal и String. Тем не менее C\# позволяет определить константную переменную, не относящуюся к элементарному типу, если присвоить ей значение null:

```
using System;
public sealed class SomeType {
   // Некоторые типы не являются элементарными, но С# допускает существование
    // константных переменных этих типов после присваивания значения null
   public const SomeType Empty = null;
}
```

Работая с константами, нужно помнить:

* В период выполнения память для констант не выделяется
* Значение контанты изменять нельзя
* Нельзя получать адрес окнстанты и передавать его по ссылке
* Константы считаются статическими, ане экземплярными членами \(они являются частью типа\)

### Поля

Поле \(field\) - это член данных, котоырй хранит экземпляр значимого типа или ссылку на ссылочный тип.

Модификаторы, применяемые по отношению к полям:

| Термин CLR | Термин C\# | Описание |
| :--- | :--- | :--- |
| Static | static | Поле является частью состояния типа, а не объекта |
| Instance | \(по умолчанию\) | Поле, связанное с экземпляром типа, а не с самим типом |
| InitOnly | readonly | Запись в поле разрешается только с использованием конструктора |
| Volatile | volatile | Код, обращающийся к полю не должен оптимизироваться компилятором, CLR или оборудованием с целью обеспечения безопасности потоков |

Поскольку поля хранятся в динамической памяти, их значения можно получить лишь в период выполнения. Поля также решают проблему управления версиями, возникающую при использовании констант. Кроме того, полю можно назначить любой тип данных, поэтому при определении полей можно не ограничиваться встроенными элементарными типами компилятора \(что приходится делать при определении констант\). 

