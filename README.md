# Содержание
1. [Общее описание Unicode](#общее-описание-Unicode)
2. [Предпосылки создания](#предпосылки-создания)
3. [Кодовые точки и плоскости](#кодовые-точки-и-плоскости)
4. [Что такое UTF?](#что-такое-UTF?)
5. [UTF-8](#UTF-8)
6. [UTF-16](#UTF-16)
7. [Сравнительная таблица](#сравнительная-таблица)
8. [Вывод](#вывод)

Как известно, любые текстовые данные должны быть представлены в виде последовательности нулей и единиц для возможности восприятия и обработки компьютерами. Ниже речь пойдет о наиболее универсальном варианте — стандарте кодирования Unicode и форматах его представления UTF.

# Общее описание Unicode
**Важно запомнить:** Unicode — стандарт,  UTF-8 или UTF-16 — кодировки.

Unicode — это стандарт, который присваивает каждому символу (а в некоторых случаях отдельным компонентам символов) свой индивидуальный номер.  Этот стандарт является универсальным и включает в себя такие символы как буквы всех основных письменностей, в том числе древних, знаки пунктуации, эмодзи, обозначения валют и многие другие знаки. Порядок символов закреплен и его изменение не предполагается.

# Предпосылки создания
Раньше разработчики на разных концах света  могли добавлять буквы своих языков или другие желаемые символы по своему усмотрению к 7-битной таблице ASCII (American Standart Code for Information Interchange), состоявшей из 128 символов. 
Таким образом для каждого языка получалась своя 8-битная кодировка, а-то и несколько. Некоторые из них получили широкое распространение, например КОИ-8 и Windows-1251, использовавшие последний бит для представления кириллицы.
Но с развитием интернета, получатель документа открывал его в кодировке, распространенной в его стране или используемую на его ПО, и часто видел на экране бессмысленный текст. Появлялись и другие сложности в обращении с традиционными 8-битными кодировками, например:
* Отображение документов не в той кодировке, получение бессмысленного набора символов.
* Проблема преобразования из одной кодировки в другую.
* Невозможность уместить в одной кодировке необходимое количество символов, в частности для языков, использующих при письме иероглифы.
* Невозможность работать с многоязычными текстами , так как каждая кодировка поддерживает только определенный набор символов.

Решением многих из этих проблем оказались многобайтовые кодировки и стандарт Unicode.

# Кодовые точки и плоскости
Символы в кодовом пространстве Unicode упорядочены и находятся в определенной последовательности. Для определения порядкового номера каждого символа в этой последовательности, в Unicode используется понятие **Кодовая точка**. 
Все кодовое пространство разделено на 17 плоскостей с нумерацией от 0 до 16. Нулевая плоскость является Основной многоязычной плоскостью (Basic Multilingual Plane). В ней находятся наиболее часто используемые символы, относящиеся, например, к латинской письменности, кириллице, знакам препинания и т.д.
В каждой плоскости стандарта находится 65536 кодовых точек. Всего же их 1 114 112. 

**Соответствие плоскостей Unicode диапазонам кодовых точек**
Номер плоскости | Название | Диапазон кодовых точек
:------------------:| ------------------|------------------
0                               | Основная многоязычная плоскость | U+0000…U+​FFFF
1                                | Дополнительная многоязычная плоскость | 10000…​1FFFF
2                               | Дополнительная идеографическая плокость | 20000…2FFFF
3                               | Третичная идеографическая плоскость | 30000—3FFFF
4-13                         | Зарезервированы для будущего использования | 40000—DFFFF
14                             | Специализированная дополнительная плоскость | E0000—EFFFF
15                             | Дополнительная область для частного использования — A | F0000—FFFFF
16                             | Дополнительная область для частного использования — B | 100000—10FFFF

Стандарт Unicode состоит из двух частей – набора всех возможных символов (UCS – universal character set) и способов кодирования этих символов или по-другому – форматов представления.

# Что такое UTF?
Формат представления Unicode (UTF – Unicode Transformation Format) – это соответствие каждой кодовой точке (за исключением суррогатных пар) стандарта Unicode уникальной битовой последовательности. То есть исходным материалом кодирования являются не сами символы, а их порядковый номер (кодовая точка). 
Важное условие, накладываемое на любую из кодировок UTF, это обратимость, которая осуществляется за счет использования **уникальной** последовательности кодовых блоков при шифровании. 

Рассмотрим два формата представления Unicode более подробно.

# UTF-8
UTF 8 – многобайтовый кодировщик текста, который представляет каждый символ Unicode последовательностью от 1 до 4х блоков по 8 бит каждый. 
Количество блоков (октетов в Unicode) изменяется в зависимости от порядкового номера символа, что позволяет работать с данными без лишних затрат памяти. Такой вид кодирования совместим с 7-битным кодированием текста ASCII. То есть при работе с латиницей, большинство букв, цифр и знаков пунктуации будут кодироваться лишь одним байтом.
## Зависимость количества требуемых для записи символа байт от его порядкового номера:
Диапазон кодовых точек | Кол-во байтов | Служебные символы, указывающие на количество байтов
:-------------------------------:|:------------------:|:------------------
0x00000000 — 0x0000007F | 1 | 0xxxxxxx
0x00000080 — 0x000007FF | 2 | 110xxxxx 10xxxxxx
0x00000800 — 0x0000FFFF | 3 | 1110xxxx 10xxxxxx 10xxxxxx
0x00010000 — 0x001FFFFF | 4 | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx

# UTF-16
UTF-16 представляет кодовую точку стандарта Unicode одним или двумя блоками по 16 бит каждый. Причем двумя блоками записываются символы, представленные в виде суррогатных пар. 
**Суррогатными** называются кодовые точки из двух специальных диапазонов значений Unicode, зарезервированные под использование парными кодовыми блоками в UTF-16. Верхние суррогаты находятся в диапазоне значений от D80016 до DBFF16, а нижние в промежутке от D80016 до DBFF16. Они используются только совместно для обозначения комбинированных символов типа “Й”.
Большая ошибка пренебрегать использованием двух байтов для кодирования этих суррогатных пар, считая их число незначительным. Такое отношение вызывает ошибки и приводит к появлению нечитабельномого текста.

Для UTF-16 предусмотрено два варианта записи последовательности байтов для каждого символа, которые определяются **меткой порядка байтов**(Byte order mark, BOM), записывающейся в начале текстового файла: U+FEFF для прямого и U+FFFE для обратного порядка считывания.
Также порядок можно указать в описании кодировки вне файла аббревиатурой **BE (big-endian)** для прямой последовательности или **LE (little-endian)** для обратной.

**Примеры представления кодовых точек в разных кодировках**
Кодовая точка | UTF-8 | UTF-16 и UTF-16BE | UTF-16LE
:---------------| :---------------| :---------------| :---------------
U+0041 | 41 | 00 41 | 00 14
U+05D0 | D7 90 | 05 D0 | 50 0D
U+597D | E5 A5 BD | 59 7D | 95 D7
U+233B4 | F0 A3 8E B4 | D8 4C DF B4 | 8D C4 FD 4B

# Сравнительная таблица различных характеристик рассматриваемых кодировок
Параметры | UTF-8 | UTF-16
---------------------| --------------------- | ---------------------
**Более компактна** | В большинстве случаев | В некоторых случаях 
**Визуальная отличимость** | Отличается визуально от других кодировок | Нет
**Однозначность восприятия** | Да, байты читаются последовательно, а служебные биты определяют их количество | Ошибки могут появляться из-за большого кол-ва нулей, неправильной записи суррогатных пар и вариативности порядка байтов.
**Совместимость с ASCII** | Да | Нет
**Где используется** | В Linux и различных Unix-системах | Windows, его библиотеки и приложения; C # String, Java, библиотеки Qt GUI, библиотека ICU Unicode , различные API
**Передача по сети** | Да, рекомендуется к использованию | Не советуется

# Вывод
Нет единого мнения о том, какой кодировки стоит придерживаться. Многие разработчики считают UTF-8 более логичной и более компактной, другие же придерживаются мнения, что UTF-16 совместима с бо́льшим количеством операционных систем и, соответственно, процессы конвертации будут происходить быстрее. При разработке нового проекта нужно учитывать все факторы в совокупности.
