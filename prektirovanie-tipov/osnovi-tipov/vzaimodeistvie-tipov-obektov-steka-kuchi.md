# Как разные компоненты взаимодействуют во время выполнения

Представим, что у нас есть процесс Microsoft Windows с загруженной в него исполняющей средой CLR. У процесса может быть много потоков. После создания потоку выделяется стек размером в 1 Мбайт. Выделенная память используется для передачи параметров в методы и хранения определенных в пределах методов локальных переменных. 

Допустим, есть следующие два определения классов: 

`internal class Employee {`

    `public Int32 GetYearsEmployed () { ... }`

    `public virtual String GetProgressReport () { ... }`

    `public static Employee Lookup(String name) { ... }`

` } `

`internal sealed class Manager : Employee {`

`   public override String GenProgressReport() { ... }`

` }`

Процесс Windows запустился, в него загружена среда CLR, инициализирована управляемая куча, и создан поток \(с его 1 Мбайт памяти в стеке\). Поток уже выполняет какой-то код, из которого вызывается метод M3 . Метод M3 содержит код, продемонстрирующий, как работает CLR; вряд ли вы будете включать такой код в свои приложения, потому что он, в сущности, не делает ничего полезного.

```C\#
void M3(){
    Employee e;
    Int32 year;
    e = new Manager();
    e = Employee.LookUp("Joe");
    year = e.GetYearsEmployed();
    e.GetProgressReport();
}
```

В процессе преобразования IL-кода метода М3 в машинные команды JIT-компилятор выявляет все типы, на которые есть ссылки в M3, — это типы Employee, Int32, Manager и String \(из-за наличия строки "Joe"\). На данном этапе CLR обеспечивает загрузку в домен приложений всех сборок, в которых определены все эти типы. Затем, используя метаданные сборки, CLR получает информацию о типах и создает структуры данных, собственно и представляющие эти типы. Структуры данных для объектов-типов Employee и Manager показаны на рис. 4.7. Поскольку до вызова M3 поток уже выполнил какой-то код, для простоты допустим, что объекты-типы Int32 и String уже созданы \(что вполне возможно, так как это часто используемые типы\), поэтому они не показаны на рисунке. ![](/assets/M3.png)После того как среда CLR создаст все необходимые для метода объекты-типы и откомпилирует код метода M3, она приступает к выполнению машинного кода M3. При выполнении входного кода M3 в стеке потока выделяется память для локальных переменных \(рис. 4.8\). В частности, CLR автоматически инициализирует все локальные переменные значением null или 0 \(нулем\) — это делается в рамках выполнения входного кода метода. Однако при попытке обращения к локальной переменной, неявно инициализированной в вашем коде, компилятор С\# выдаст сообщение об ошибке Use of unassigned local variable \(использование неинициализированной локальной переменной\).

> Все объекты в куче содержат два дополнительных члена: **указатель на объект-тип** и **индекс блока синхронизации. **
>
> Объект-тип - это указатель на тип объекта. Он недоступен программисту напрямую без всяческих колдунств, поскольку предназначен для самой CLR
>
> Индекс блока синхронизации предназначен для целого ряда целей. Но основная его задача - это обеспечение работы объекта в условиях многопоточности. Вероятно, вам знакомо такое ключевое слово, как **lock**. Оно используется для синхронизации доступа к объекту из нескольких потоков \(то есть позволяет выполнять некий блок кода только одним потоком, заставляя остальные потоки ждать своей очереди\). Кроме того индекс блока синхронизации может использоваться в качестве хэш-кода этого объекта в случае, если механизм генерации хэш-кода не был переопределен

Далее будет выполнен остальной код метода путём создания объектов, вызова методов и прочего, о чём можно подробнее прочитать в самой книге \(стр. 135\)





