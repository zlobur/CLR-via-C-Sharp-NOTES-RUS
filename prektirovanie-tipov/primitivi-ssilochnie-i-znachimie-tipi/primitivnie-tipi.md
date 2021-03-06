# Примитивные типы в языках программирования

Некоторые типы данных применяются так часто, что для работы с ними во многих компиляторах предусмотрен упрощенный синтаксис. Например, целую переменную можно создать следующим образом:

` System.Int32 a = new System.Int32(); `

Конечно, подобный синтаксис для объявления и инициализации целой переменной кажется громоздким. К счастью, многие компиляторы \(включая C\#\) позволяют использовать вместо этого более простые выражения, например:

` int a = 0;`

В двух случаях, в IL-коде эти два лбъявления будут выглядеть абсолютно одинаково.

* Примитивные типы - это типы, которые поддерживаются компилятором _напрямую_

![](/assets/primitives.png)_ _![](/assets/prim2.png)

Несмотря на наличие примитивных типов, Рихтер советует использовать FCL вариант \(вместо int - Int32, string - String, bool - Boolean\). При работе с несколькими ЯП, используя FCL-тип мы сможем добиться их корректного взаимодействия; У многих FCL-типов есть методы, в имена которых включены имена типов. Например, у типа BinaryReader есть методы ReadBoolean, ReadInt32, ReadSingle и т. д., а у типа System.Convert — методы ToBoolean, ToInt32, ToSingle и т. д.;

! В C\# long соответствует тип System.Int64, но в другом языке это может быть Int16 или Int32. Как известно, в С++/CLI тип long трактуется как Int32. Если кто-то возьмется читать код, написанный на новом для себя языке, то назначение кода может быть неверно им истолковано.

!Используйте типы со знаком \(Int32 и Int64\) вместо числовых типов без знака \(UInt32 и UInt64\) везде, где это возможно. Это позволит компилятору выявлять ошибку переполнения. Кроме того, некоторые компоненты библиотеки классов \(например, свойства Length классов Array и String\) жестко запрограммированы на возвращение значений со знаком, и передача этих значений в коде потребует меньшего количества преобразований типа \(а следовательно, упростит структуру кода и его сопровождение\). Кроме того,  числовые типы без знака несовместимы с CLS.

