# [Как писать скрипты на BATCH?](https://www.youtube.com/watch?v=J4pkFey3DlY) 🤔
##### Все на самом деле проще, чем кажется. 😉

## Вывод текста на экран
```bat
@echo off
echo It's my the first script on BATCH!
```
Вывод:
> It's my the first script on BATCH!

С первого взгляда стало понятно, чтобы вывести текст на экран нужно воспользоваться командой `echo`. \
Но что значит первая строчка `@echo off`? 🤔 Первая строчка отвечает за отключения "эхо", то есть, другими словами она отключает вывод *командной строки*. Представим, что вы ее не записали, и вот что у вас получится:

> echo It's my the first script on BATCH! \
It's my the first script on BATCH!

У вас отобразятся команды, которые вы записали в ваш скрипт, а потом их результат.

## Комментарии, кодировка и перенос строки
### Комментарии
Комментарии в коде чаще всего создаются, для описания какой-либо строчки или какого-либо блока кода. \
Имеется несколько способов оставить комментарий:
```bat
@rem Смотри какой красивый комментарий
rem А как тебе этот комментарий?
:: И даже так можно!
```
Разницы не будет как вы это сделаете, комментарий не читается при исполнении скрипта - он только для вас.

### Кодировка
Допустим вы захотели вывести текст в консоль, который написан кириллицей, но при выводе возникает проблема? Что это за символы?! \
Для этого следует указать *интернациональную кодировку UTF-8*:
```bat
@echo off
@rem chcp *сюда код кодировки* (65001 это UTF-8)
chcp 65001

echo Привет, кириллица!
```

### Перенос строк
Если у вас в коде имеется длинная строка, которую вы хотите перенести на другую строчку, чтобы визуально это смотрелось красиво и удобно, можно воспользоваться символом `^`:
```bat
@echo off
chcp 65001

echo У меня есть очень большой текст, который по сути бессмысленный и я придумываю этот текст на ходу, ^
в прямом эфире и совершенно не важно, что здесь написано. С помощью ёлочки вверх, вы сможете перенести текст ^
на следующую строку и это очень просто работает, но если вывести этот текст в консоль, он будет идти в строчку.
```

А что, если вы хотите вывести текст выше также, как это было и в коде? Эти `^` ёлочки не способны такого сделать, и тут можно сделать такую штуку:
```bat
@echo off
chcp 65001

set "n=&echo."
echo Это можно сказать простая табуляция.%n%Точнее обычная переменная,^
которая может спокойно перенести строчку на следующую.%n%^
Переменные мы разберем в следующей главе.
```

## Переменные
В *BATCH* переменные бывают **глобальные 🔓** и **локальные 🔒**, изначально они все глобальные, а также их несколько видов. \
**Глобальные переменные** можно использовать повсюду, в скрипте, другом `bat-файле`, но только в *текущей сессии*. Что такое текущая сессия? 🤔 Простыми словами это открытая консоль в данный момент. Давайте разбираться, но для начала посмотрим, как можно создать самую простую переменную.
```bat
set variable_name=Simple variable
```
Любая переменная создается с ключевого слова `set`, дальше можно указать тип переменной (необязательно), после обозвать её. Значения переменной указываются после символа `=` без пробелов. Обычная переменная может содержать только строку (string).
```bat
@echo off
@rem Моя первая переменная, и сейчас я ее выведу на экран.

set text=Hello, world!
echo %text%
```
Вывод:
> Hello, world!

Как вы наверное догадались, чтобы получить значение переменной, нужно обернуть *название переменной* в символы `%`. \
Другой вид переменной, числовой.
```bat
set /a number=2022
```
С такой переменной можно проводить математические операции:
```bat
@echo off
@rem Как мне сложить два числа?

set /a number=2021 + 1
echo New %number% year
```
Вывод:
> New 2022 year

Также можно создать две разных переменных и сложить их воедино.
```bat
@echo off

set /a number1=100
set /a number2=899
set /a result=number1 + number2

echo %number1% + %number2% = %result%
```
Вывод:
> 100 + 899 = 999

Давайте расмотрим еще один вид переменной.
```bat
@echo off

set /p input=Enter some text: 
echo %input%
```
Вход:
> **script** *запускаем файл* \
> Enter some text: Don't worry, smile!

Вывод:
> Don't worry, smile!

![example](https://github.com/Fsoky/batch-script/blob/main/images/Screenshot_1.png)

Такая переменная может принимать в себя данные, которые вы передадите. \
Имеется еще один интересный вид переменной, она называется *переменной аргумента*. \
Обозначается она таким образом: `%1 %2 %3...`
```bat
@echo off
echo %1
```
Вход:
> **script** Something

Вывод:
> Something

![example](https://github.com/Fsoky/batch-script/blob/main/images/Screenshot_2.png)

Когда мы запускаем наш `bat-файл` из консоли, мы можем передать любой аргумент после его названия: `script Something`. \
*script* - название нашего файла, *Something* - наш желаемый аргумент. Если мы попытаемся передать несколько слов (аргументов) через пробел, то у нас засчитает только первое слово. То есть каждое *новое слово* по сути является *новым аргументом*.
```bat
@echo off
echo %1
```
Вход:
> **script** Everything will be fine...

Вывод:
> Everything

Этого можно избежать следующими способами:
1. Указать больше переменных.
2. Обернуть текст в кавычки `" "`
3. Указать символ `*`

###### Способ первый:
Я хочу передать вот этот текст: `Everything will be fine...`. Здесь 4 слова (необязательно слова, после каждого нового пробела получается новый аргумент)
```bat
@echo off
echo %1 %2 %3 %4
```
Вход:
> **script** Everything will be fine...

Вывод:
> Everything will be fine...

###### Второй способ:
В этом способе я оберну текст в кавычки `" "`
```bat
@echo off
echo %1
```
Вход:
> **script** "Everything will be fine..."

Вывод:
> "Everything will be fine..."

###### Третий способ:
```bat
@echo off
echo %*
```
Вход:
> **script** Everything will be fine...

Вывод:
> Everything will be fine...

\
Также на основе таких переменных можно сделать небольшой калькулятор, для этого эту переменную нужно как-бы преобразовать в числовой тип:
```bat
@echo off

set /a value=%1
echo %value%
```
Вход:
> **script** 12+12

Вывод:
> 24

![example](https://github.com/Fsoky/batch-script/blob/main/images/Screenshot_3.png)

### Локальные переменные
Локальные переменные задаются в блоке от `setLocal` до `endLocal`. Такими переменными нельзя воспользоваться за пределами блока, и также они недоступны в сессии, как глобальные.
```bat
@echo off

set global_variable=I'm Global Elite

setLocal
set local_variable=I'm... I.. nobody?...
endLocal
```
Попробуйте запустить этот скрипт, а после прописать в консоли `echo %global_variable%`, получилось? \
Теперь попробуйте `echo %local_variable%`. Мм.. нет? \
Также глобальными переменными можно пользоваться в других `bat-файлах`. Попробуйте создать другой файлик и к примеру вывести глобальную переменную на экран.

В принципе все просто. \
**Практика:**
1. Попробуйте написать скрипт, который будет принимать ваше имя и возраст, после выводить его на экран.
```
Ваше имя: Даниил
Ваш возраст: 17

Привет, Даниил, а знаю, что тебе 17 лет!
```
2. Попробуйте сделать простой калькулятор, который будет только складывать числа.
```
Введите первое число: 12
Введите второй число: 12

Ответ: 24
```

## Циклы
### Цикл for (по умолчанию)

Данный цикл используется для повторения файлов, пример:
```bat
@echo off

for %%i in (C:\folder\fantasy.txt C:\folder\myths.txt) do (
  copy %%i C:\Users\user\Desktop
)
```
![example](https://github.com/Fsoky/batch-script/blob/main/images/Screenshot_4.png)
Разберем начало цикла, цикл создается с *ключевого слова* `for`, следующим можно указать *вид* цикла `/r`, `/d`, `/f`, `/l` (необязательно). Вид цикла используется в разных ситуациях, в которых вы хотите его применить. Дальше в этом разберемся. \
Переменная в цикле начинается с двух символов `%%`, а после записывается само название (**названием переменной должен служить 1 единственный символ**). \

В `( )` записываем пути к файлам, с которыми в будущем будем осуществлять работу. \
Работа осуществляется в теле цикла, чтобы туда попасть, нужно записать *ключевое слово* `do`, открыть скобки `()` и приступить к написанию скрипта. \
В этом случае, мы просто копируем файлы (*fantasy.txt*, *myths.txt*) в новую директорию.

### Цикл for /R
Данный цикл используется для перебора файлов в директории:
```bat
@echo off

for /r C:\folder %%f in (*.txt) do (
  echo %%f
)
```
![example](https://github.com/Fsoky/batch-script/blob/main/images/Screenshot_5.png)
Здесь мы уже указываем команду `/r`, следующим можно передать папку, которая будет считаться корневой, если не передавать директорию (*C:\folder*) - текущая директория будет считаться корневой. Также поиск файлов будет осуществляться и в подпапках. \
`%%f` - является переменной. В скобках `( )`, можно передать файлы, по которым будет осуществляться поиск. Их может быть несколько `(*.txt *.py *.bat)` или можно записать `.`, она будет искать все файлы в целом (в подпапках тоже). 

### Цикл for /D
Используется для загрузки списка папок, которые являются подпапками текущей директории:
```bat
@echo off

cd C:\folder

for /d %%f in (f* n*) do (
  echo %%f
)
```
![example](https://github.com/Fsoky/batch-script/blob/main/images/Screenshot_6.png)
В данном примере мы получим список папок в директории *C:\folder*, которые начинаются с букв `f` и `n`. Если передать `*`, мы получим список всех папок находящихся в директории.

## Цикл for /l
Этот цикл служит для загрузки на ряде цифр (*range of numbers*):
```bat
@echo off

for /l %%i in (1, 1, 10) do (
  echo %%i
)
```
![example](https://github.com/Fsoky/batch-script/blob/main/images/Screenshot_7.png)
В этом примере мы выведим цифры от *1* до *10*. \
```bat
for /l %%i in (start, step, end) do (
  echo %%i
)
```
+ start: Первое значение переменной
+ step: После каждого повтора (iteration) значение переменной будет прибавлять 'step'.
+ end: Последнее значение.

*...дописывается*
