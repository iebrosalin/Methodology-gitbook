# Argparse

{% embed url="https://jenyay.net/Programming/Argparse" %}

## Разбор параметров командной строки в Python

#### Содержание

* [Введение](https://jenyay.net/Programming/Argparse#intro)
* [Примеры без использования argparse](https://jenyay.net/Programming/Argparse#noargparse)
* [Использование библиотеки argparse](https://jenyay.net/Programming/Argparse#using)
  * [Первый пример](https://jenyay.net/Programming/Argparse#first)
  * [Добавляем именованные параметры](https://jenyay.net/Programming/Argparse#optional)
  * [Параметры как списки](https://jenyay.net/Programming/Argparse#list)
  * [Выбор из вариантов](https://jenyay.net/Programming/Argparse#choices)
  * [Указание типов параметров](https://jenyay.net/Programming/Argparse#type)
  * [Имена файлов как параметры](https://jenyay.net/Programming/Argparse#files)
  * [Обязательные именованные параметры](https://jenyay.net/Programming/Argparse#required)
  * [Параметры как флаги](https://jenyay.net/Programming/Argparse#flags)
  * [Использование нескольких параметров](https://jenyay.net/Programming/Argparse#several)
  * [Использование подпарсеров](https://jenyay.net/Programming/Argparse#subparsers)
  * [Оформление справки](https://jenyay.net/Programming/Argparse#help)
* [Заключение](https://jenyay.net/Programming/Argparse#outro)
* [Комментарии](https://jenyay.net/Programming/Argparse#comments)

#### Введение <a href="#intro" id="intro"></a>

Как вы, наверное, знаете, все программы можно условно разделить на консольные и использующие графический интерфейс (сервисы под Windows и демоны под Linux не будем брать в расчет). Параметры запуска, задаваемые через командную строку, чаще всего используют консольные программы, хотя программы с графическим интерфейсом тоже не брезгуют этой возможностью. Наверняка в жизни каждого программиста была ситуация, когда приходилось разбирать параметры командной строки, как правило, это не самая интересная часть программы, но без нее не обойтись. Эта статья посвящена тому, как Python облегчает жизнь программистам при решении этой задачи благодаря своей стандартной библиотеке argparse.

В дальнейшем все примеры я буду приводить с расчетом на Python 3.3, но практически все то же самое можно использовать и в Python 2.7, хотя для запуска на Python 2.7 примеры придется исправить, в основном добавить символ "u" перед началом строк.

#### Примеры без использования argparse <a href="#noargparse" id="noargparse"></a>

Путь для начала у нас есть простейший скрипт на Python. Для определенности назовем скрипт _coolprogram.py_, это будет классический Hello World, над которым мы будем работать в дальнейшем.

\#!/usr/bin/python\
\# -\*- coding: UTF-8 -\*-\
\
if \_\_name\_\_ == "\_\_main\_\_":\
&#x20;   print ("Привет, мир!")[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=1)

Мы завершили эту сложнейшую программу и отдали ее заказчику, он доволен, но просит добавить в нее возможность указывать имя того, кого приветствуем, причем этот параметр может быть не обязательным. Т.е. программа может использоваться двумя путями:

$ python coolprogram.py\
\
или\
\
$ python coolprogram.py Вася[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=2)

Первое, что приходит на ум при решении такой задачи, просто воспользоваться переменной _argv_ из модуля _sys_. Напомню, что _sys.argv_ содержит список параметров, переданных программе через командную строку, причем нулевой элемент списка - это имя нашего скрипта. Т.е. если у нас есть следующий скрипт с именем _params.py_:

\#!/usr/bin/python\
\# -\*- coding: UTF-8 -\*-\
\
import sys\
\
if \_\_name\_\_ == "\_\_main\_\_":\
&#x20;   for param in sys.argv:\
&#x20;       print (param)[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=3)

и мы запускаем его с помощью команды

python params.py[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=4)

то в консоль будет выведена единственная строка:

params.py[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=5)

Если же мы добавим несколько параметров,

python params.py param1 param2 param3[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=6)

то эти параметры мы увидим в списке _sys.argv_, начиная с первого элемента:

params.py\
param1\
param2\
param3[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=7)

Здесь можно обратить внимание на то, что ссылка на интерпретатор Python в список этих параметров не входит, хотя он также присутствует в строке вызова нашего скрипта.

Вернемся к нашей задаче. Погрузившись в код на неделю, мы могли бы выдать заказчику следующий скрипт:

\#!/usr/bin/python\
\# -\*- coding: UTF-8 -\*-\
\
import sys\
\
if \_\_name\_\_ == "\_\_main\_\_":\
&#x20;   if len (sys.argv) > 1:\
&#x20;       print ("Привет, {}!".format (sys.argv\[1] ) )\
&#x20;   else:\
&#x20;       print ("Привет, мир!")[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=8)

Теперь, если программа вызывается с помощью команды

python coolprogram.py[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=9)

то результат будет прежний

Привет, мир\![Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=10)

а если мы добавим параметр:

python coolprogram.py Вася[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=11)

то программа поприветствует некоего Васю:

Привет, Вася\![Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=12)

Пока все легко и никаких проблем не возникает. Теперь предположим, что требования заказчика вновь изменились, и на этот раз он хочет, чтобы имя приветствуемого человека передавалось после именованного параметра _--name_ или _-n_, причем нужно следить, что в командной строке передано только одно имя. С этого момента у нас начнется вермишель из конструкций _if_.

\#!/usr/bin/python\
\# -\*- coding: UTF-8 -\*-\
\
import sys\
\
if \_\_name\_\_ == "\_\_main\_\_":\
&#x20;   if len (sys.argv) == 1:\
&#x20;       print ("Привет, мир!")\
&#x20;   else:\
&#x20;       if len (sys.argv) < 3:\
&#x20;           print ("Ошибка. Слишком мало параметров.")\
&#x20;           sys.exit (1)\
\
&#x20;       if len (sys.argv) > 3:\
&#x20;           print ("Ошибка. Слишком много параметров.")\
&#x20;           sys.exit (1)\
\
&#x20;       param\_name = sys.argv\[1]\
&#x20;       param\_value = sys.argv\[2]\
\
&#x20;       if (param\_name == "--name" or\
&#x20;               param\_name == "-n"):\
&#x20;           print ("Привет, {}!".format (param\_value) )\
&#x20;       else:\
&#x20;           print ("Ошибка. Неизвестный параметр '{}'".format (param\_name) )\
&#x20;           sys.exit (1)[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=13)

Здесь мы проверяем ситуацию, что мы вообще не передали ни одного параметра, потом проверяем, что дополнительных параметров у нас ровно два, что они называются именно _--name_ или _-n_, и, если нас все устраивает, выводим приветствие.

Как видите, код превратился в тихий ужас. Изменить логику работы в нем в дальнейшем будет очень сложно, а при увеличении количества параметров нужно будет срочно применять объектно-ориентированные меры по отделению логики работы программы от разбора командной строки. Разбор командной строки мы могли бы выделить в отдельный класс (или классы), но мы этого здесь делать не будем, поскольку все уже сделано в стандартной библиотеке Python, которая называется **argparse**.

#### Использование библиотеки argparse <a href="#using" id="using"></a>

**Первый пример**

Как как было сказано выше, стандартная библиотека _argparse_ предназначена для облегчения разбора командной строки. На нее можно возложить проверку переданных параметров: их количество и обозначения, а уже после того, как эта проверка будет выполнена автоматически, использовать полученные параметры в логике своей программы.

Основой работы с командной строкой в библиотеке _argparse_ является класс _ArgumentParser_. У его конструктора и методов довольно много параметров, все их рассматривать не будем, поэтому в дальнейшем рассмотрим работу этого класса на примерах, попутно обсуждая различные параметры.

Простейший принцип работы с _argparse_ следующий:

1. Создаем экземпляр класса _ArgumentParser_.
2. Добавляем в него информацию об ожидаемых параметрах с помощью метода _add\_argument_ (по одному вызову на каждый параметр).
3. Разбираем командную строку с помощью метода _parse\_args_, передавая ему полученные параметры командной строки (кроме нулевого элемента списка _sys.argv_).
4. Начинаем использовать полученные параметры.

Для начала перепишем программу _coolprogram.py_ с единственным параметром так, чтобы она использовала библиотеку _argparse_. Напомню, что в данном случае мы ожидаем следующий синтаксис параметров:

python coolprogram.py \[Имя][Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=14)

Здесь \[Имя] является необязательным параметром.

Наша программа с использованием _argparse_ может выглядеть следующим образом:

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. &#x20;
8. def createParser ():
9. &#x20;   parser = argparse.ArgumentParser()
10. &#x20;   parser.add\_argument ('name', nargs='?')
11. &#x20;
12. &#x20;   return parser
13. &#x20;
14. &#x20;
15. if \_\_name\_\_ == '\_\_main\_\_':
16. &#x20;   parser = createParser()
17. &#x20;   namespace = parser.parse\_args()
18. &#x20;
19. &#x20;   \# print (namespace)
20. &#x20;
21. &#x20;   if namespace.name:
22. &#x20;       print ("Привет, {}!".format (namespace.name) )
23. &#x20;   else:
24. &#x20;       print ("Привет, мир!")

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=15)

На первый взгляд эта программа работает точно так же, как и раньше, хотя есть отличия, но мы их рассмотрим чуть позже. Пока разберемся с тем, что мы понаписали в программе.

Создание парсера вынесено в отдельную функцию, поскольку эта часть программы в будущем будет сильно изменяться и разрастаться. На строке 9 мы создали экземпляр класса _ArgumentParser_ с параметрами по умолчанию. Что это за параметры, опять же, поговорим чуть позже.

На строке 10 мы добавили ожидаемый параметр в командной строке с помощью метода _add\_argument_. При этом такой параметр будет считаться позиционным, т.е. он должен стоять именно на этом месте и у него не будет никаких предварительных обозначений (мы их добавим позже в виде '-n' или '--name'). Если бы мы не добавили именованный параметр _nargs='?_', то этот параметр был бы обязательным. _nargs_ может принимать различные значения. Если бы мы ему присвоили целочисленное значение больше 0, то это бы означало, что мы ожидаем ровно такое же количество передаваемых параметров (точнее, считалось бы, что первый параметр ожидал бы список из N элементов, разделенных пробелами, этот случай мы рассмотрим позже). Также этот параметр может принимать значение '?', '+', '\*' и _argparse.REMAINDER_. Мы их не будем рассматривать, поскольку они важны в сочетании с необязательными именованными параметрами, которые могут располагаться как до, так и после нашего позиционного параметра. Тогда этот параметр будет показывать как интерпретировать список параметров, где будет заканчиваться один список параметров и начинаться другой.

Итак, мы создали парсер, после чего можно вызвать его метод _parse\_args_ для разбора командной строки. Если мы не укажем никакого параметра, это будет равносильно тому, что мы передадим в него все параметры из _sys.argv_ кроме нулевого, который содержит имя нашей программы. т.е.

parser.parse\_args (sys.argv\[1:])[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=16)

В качестве результата мы получим экземпляр класса _Namespace_, который будет содержать в качестве члена имя нашего параметра. Теперь можно раскомментировать строку 19 в приведенном выше примере, чтобы посмотреть, чему же равны наши параметры.

Если мы это сделаем и запустим программу с переданным параметром

python coolprogram.py Вася[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=17)

, то увидим его в пространстве имен.

Namespace(name='Вася')[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=18)

Если же теперь мы запустим программу без дополнительных параметров, то это значение будет равно None:

Namespace(name=None)[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=19)

Мы можем изменить значение по умолчанию, что позволит нам несколько сократить программу. Пусть по умолчанию используется слово 'мир', ведь мы его приветствуем, если параметры не переданы. Для этого воспользуемся дополнительным именованным параметром _default_ в методе _add\_argument_.

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. &#x20;
8. def createParser ():
9. &#x20;   parser = argparse.ArgumentParser()
10. &#x20;   parser.add\_argument ('name', nargs='?', default='мир')
11. &#x20;
12. &#x20;   return parser
13. &#x20;
14. &#x20;
15. if \_\_name\_\_ == '\_\_main\_\_':
16. &#x20;   parser = createParser()
17. &#x20;   namespace = parser.parse\_args (sys.argv\[1:])
18. &#x20;
19. &#x20;   \# print (namespace)
20. &#x20;
21. &#x20;   print ("Привет, {}!".format (namespace.name) )

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=20)

Программа продолжает работать точно также, как и раньше. Вы, наверное, заметили, что в предыдущем примере в метод _parse\_args_ на строке 17 передаются параметры командной строки из _sys.argv_. Это сделано для того, чтобы показать, что список параметров мы можем передавать явно, при необходимости мы его можем предварительно обработать, хотя это вряд ли понадобится, ведь почти всю обработку можно возложить на плечи библиотеки _argparse_.

**Добавляем именованные параметры**

Теперь снова переделаем нашу программу таким образом, чтобы использовать именованные параметры. Напомню, что согласно последнему желанию (в смысле, для данной программы) заказчика имя приветствуемого человека должно передаваться после параметра _--name_ или _-n_. С помощью _pyparse_ сделать это проще простого - достаточно в качестве первых двух параметров метода _add\_argument_ передать эти имена параметров.

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. &#x20;
8. def createParser ():
9. &#x20;   parser = argparse.ArgumentParser()
10. &#x20;   parser.add\_argument ('-n', '--name', default='мир')
11. &#x20;
12. &#x20;   return parser
13. &#x20;
14. &#x20;
15. if \_\_name\_\_ == '\_\_main\_\_':
16. &#x20;   parser = createParser()
17. &#x20;   namespace = parser.parse\_args(sys.argv\[1:])
18. &#x20;
19. &#x20;   \# print (namespace)
20. &#x20;
21. &#x20;   print ("Привет, {}!".format (namespace.name) )

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=21)

Теперь, если мы запустим программу без параметров, то увидим знакомое "Привет, мир!", а если мы запустим программу с помощью команды

python coolprogram.py -n Вася[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=22)

или

python coolprogram.py --name Вася[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=23)

То приветствовать программа будет Васю. Обратите внимание, что теперь в методе _add\_argument_ мы убрали параметр _nargs='?'_ , поскольку **все именованные параметры считаются необязательными**. А если они не обязательные, то возникает вопрос, как поведет себя argparse, если этот параметр не передан? Для этого уберем параметр _default_ в _add\_argument_.

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. def createParser ():
8. &#x20;   parser = argparse.ArgumentParser()
9. &#x20;   parser.add\_argument ('-n', '--name')
10. &#x20;
11. &#x20;   return parser
12. &#x20;
13. &#x20;
14. if \_\_name\_\_ == '\_\_main\_\_':
15. &#x20;   parser = createParser()
16. &#x20;   namespace = parser.parse\_args(sys.argv\[1:])
17. &#x20;
18. &#x20;   print ("Привет, {}!".format (namespace.name) )

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=24)

Если теперь запустить программу без параметров, то увидим приветствие великого _None_:

Привет, None\![Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=25)

Таким образом, **если значение по умолчанию не указано, то оно считается равным **_**None**_**.**

Раньше мы на это не обращали внимание, но нужно все-таки сказать, что _argparse_ по умолчанию заточен для создания интерфейса командной строки именно в стиле UNIX, где именованные параметры начинаются с символов "-" или "--". Если такой параметр попытаться называть в стиле Windows (например, "/name"), то будет брошено исключение:

ValueError: invalid option string '/name': must start with a character '-'[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=26)

На самом деле буквально с помощью одного параметра мы можем заставить работать _argparse_ в стиле Windows, но это выходит за рамки статьи, поэтому мы везде будем использовать стиль UNIX.

До этого мы задавали два имени для одного и того же параметра: длинное имя, начинающееся с "--" (_--name_) и короткое сокращение, начинающееся с "-" (_-n_). При этом получение значение параметра из пространства имен осуществляется по длинному имени:

print ("Привет, {}!".format (namespace.name) )[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=27)

Если мы не зададим длинное имя, то придется обращаться к параметру через его короткое имя (_n_):

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. def createParser ():
8. &#x20;   parser = argparse.ArgumentParser()
9. &#x20;   parser.add\_argument ('-n')
10. &#x20;
11. &#x20;   return parser
12. &#x20;
13. &#x20;
14. if \_\_name\_\_ == '\_\_main\_\_':
15. &#x20;   parser = createParser()
16. &#x20;   namespace = parser.parse\_args(sys.argv\[1:])
17. &#x20;
18. &#x20;   print (namespace)
19. &#x20;
20. &#x20;   print ("Привет, {}!".format (namespace.n) )

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=28)

При этом пространство имен будет выглядеть как:

Namespace(n='Вася')[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=29)

Хорошо, с уменьшением количества имен параметров разобрались, но мы можем еще и увеличить количество имен, например, мы можем добавить для того же параметра еще новое имя _--username_, для этого достаточно его добавить следующим параметром метода _add\_argument_:

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. def createParser ():
8. &#x20;   parser = argparse.ArgumentParser()
9. &#x20;   parser.add\_argument ('-n', '--name', '--username')
10. &#x20;
11. &#x20;   return parser
12. &#x20;
13. &#x20;
14. if \_\_name\_\_ == '\_\_main\_\_':
15. &#x20;   parser = createParser()
16. &#x20;   namespace = parser.parse\_args(sys.argv\[1:])
17. &#x20;
18. &#x20;   print (namespace)
19. &#x20;
20. &#x20;   print ("Привет, {}!".format (namespace.name) )

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=30)

Теперь мы можем использовать три варианта передачи параметров:

python coolprogram.py -n Вася\
python coolprogram.py --name Вася\
python coolprogram.py --username Вася[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=31)

Все три варианта равнозначны, при этом надо обратить внимание, что при получении значения этого параметра используется первое длинное имя, т.е. _name_. Пространство имен при использовании всех трех вариантов вызова программы будет выглядеть одинаково:

Namespace(name='Вася')[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=32)

**Параметры как списки**

До сих пор мы ожидали, что в качестве значения параметра выступает строка, но бывают ситуации, когда необходимо, чтобы в качестве значения параметра принимался список строк. Например, пусть нам теперь нужно изменить программу так, чтобы мы могли приветствовать нескольких человек. Первое, что приходит на ум - это вручную разобрать строку после имени параметра, т.е.

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. def createParser ():
8. &#x20;   parser = argparse.ArgumentParser()
9. &#x20;   parser.add\_argument ('-n', '--name', default='мир')
10. &#x20;
11. &#x20;   return parser
12. &#x20;
13. &#x20;
14. if \_\_name\_\_ == '\_\_main\_\_':
15. &#x20;   parser = createParser()
16. &#x20;   namespace = parser.parse\_args(sys.argv\[1:])
17. &#x20;
18. &#x20;   print (namespace)
19. &#x20;
20. &#x20;   for name in namespace.name.split():
21. &#x20;       print ("Привет, {}!".format (name) )

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=33)

Но это не самое лучшее решение, поскольку если пользователь захочет передать несколько параметров после _--name_, то ему придется оборачивать список имен в кавычки:

python coolprogram.py --name "Вася Оля Петя"[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=34)

Поскольку, если написать просто

python coolprogram.py --name Вася Оля Петя[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=35)

, то _argparse_ решит, что "Оля Петя" - это отдельные параметры, имя для которых не задано, и напишет ошибку:

error: unrecognized arguments: Оля Петя[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=36)

Чтобы указать библиотеке _argparse_, что значений параметров у нас может быть несколько, вспомним уже используемый нами параметр _nargs_. Но если в прошлый раз в качестве его значения мы использовали значение "?", обозначающее, что у нас может быть 0 или 1 значение, то теперь мы будем использовать значение _nargs='+_', обозначающее, что мы ожидаем одно или более значение (по аналогии с регулярными выражениями).

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. def createParser ():
8. &#x20;   parser = argparse.ArgumentParser()
9. &#x20;   parser.add\_argument ('-n', '--name', nargs='+', default=\['мир'])
10. &#x20;
11. &#x20;   return parser
12. &#x20;
13. &#x20;
14. if \_\_name\_\_ == '\_\_main\_\_':
15. &#x20;   parser = createParser()
16. &#x20;   namespace = parser.parse\_args(sys.argv\[1:])
17. &#x20;
18. &#x20;   print (namespace)
19. &#x20;
20. &#x20;   for name in namespace.name:
21. &#x20;       print ("Привет, {}!".format (name) )

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=37)

Обратите внимание, что здесь в качестве значения по умолчанию используется **список** из одного слова.

Теперь, если мы выполним эту программу с помощью команды

python coolprogram.py --name Вася Оля Петя[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=38)

То программа нам выведет:

Namespace(name=\['Вася', 'Оля', 'Петя'])\
Привет, Вася!\
Привет, Оля!\
Привет, Петя\![Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=39)

Таким образом, мы возложили задачу разделения параметров на плечи _argparse_. Напомню, что _nargs_ может принимать и другие значения, в том числе целое число _N_, обозначающее, что мы ожидаем ровно _N_ значений.

**Выбор из вариантов**

Представим себе еще один случай. Пусть у нас есть консольная программа, наподобие [ImageMagick](http://www.imagemagick.org/script/index.php), то есть работающая с графикой. Наверняка эта программа будет поддерживать ограниченное количество форматов графических файлов. В этом случае может понадобиться ограничить значения некоторого параметра заранее заданным списком значений (например, gif, png, jpg). Разумеется, такую несложную проверку мы могли бы сделать и сами, но лучше пусть этим займется _argparse_. Для этого достаточно в метод _add\_argument_ передать еще один параметр - _choices_, который принимает список возможных значений параметра.

Чтобы не удаляться от наших предыдущих примеров, изменим последнюю программу таким образом, чтобы она могла приветствовать только лишь заранее очерченный круг лиц (неких Васю, Олю или Петю). Сделать это абсолютно несложно:

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. def createParser ():
8. &#x20;   parser = argparse.ArgumentParser()
9. &#x20;   parser.add\_argument ('-n', '--name', choices=\['Вася', 'Оля', 'Петя'], default='Оля')
10. &#x20;
11. &#x20;   return parser
12. &#x20;
13. &#x20;
14. if \_\_name\_\_ == '\_\_main\_\_':
15. &#x20;   parser = createParser()
16. &#x20;   namespace = parser.parse\_args(sys.argv\[1:])
17. &#x20;
18. &#x20;   print (namespace)
19. &#x20;
20. &#x20;   print ("Привет, {}!".format (namespace.name) )

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=40)

Теперь, если мы передадим в качестве параметра одно из перечисленных имен, то программа будет работать, как и прежде, однако незнакомых людей она откажется приветствовать:

$ python coolprogram.py --name Вася\
\
Namespace(name='Вася')\
Привет, Вася!\
\
$ python coolprogram.py --name Иван\
\
usage: coolprogram.py \[-h] \[-n {Вася,Оля,Петя}]\
coolprogram.py: error: argument -n/--name: invalid choice: 'Иван' (choose from 'Вася', 'Оля', 'Петя')[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=41)

Иногда может быть полезным в качестве значения параметра _choices_ использовать список целых чисел, полученных с помощью стандартной функции _range_.

**Указание типов параметров**

До сих пор мы в качестве входных параметров использовали строки, что логично, поскольку на самом нижнем уровне все входные параметры считаются строками. Однако часто на практике в качестве параметров хотелось бы использовать другие типы: целые числа или числа с плавающей точкой. Конечно, не проблема вручную преобразовать строку в нужный тип или написать ошибку в случае, если этого сделать невозможно. Однако, даже такую мелочь можно переложить на _argparse_. Для этого достаточно добавить еще один именованный параметр в метод _add\_argument_, а именно параметр _type_, который будет указывать ожидаемый тип значения параметра. Изменим пример таким образом, чтобы в качестве входного параметра с именем _-c_ или _--count_ принималось целое число, которое обозначает, сколько раз нужно вывести строку "Привет, мир!" (временно забудем про параметры _-n_ и _--name_).

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. def createParser ():
8. &#x20;   parser = argparse.ArgumentParser()
9. &#x20;   parser.add\_argument ('-c', '--count', default=1, type=int)
10. &#x20;
11. &#x20;   return parser
12. &#x20;
13. &#x20;
14. if \_\_name\_\_ == '\_\_main\_\_':
15. &#x20;   parser = createParser()
16. &#x20;   namespace = parser.parse\_args(sys.argv\[1:])
17. &#x20;
18. &#x20;   print (namespace)
19. &#x20;
20. &#x20;   for \_ in range (namespace.count):
21. &#x20;       print ("Привет, мир!")

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=42)

Нас здесь больше всего интересует 9-я строка, где мы указали ожидаемый тип параметра. Обратите внимание, что в качестве значения параметра _type_ мы передали не строку, а стандартную функцию преобразования в целое число. В остальном там нет никаких особенностей.

В следующем блоке показан пример работы этой программы:

$ python coolprogram.py --count 5\
\
Namespace(count=5)\
Привет, мир!\
Привет, мир!\
Привет, мир!\
Привет, мир!\
Привет, мир\![Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=43)

Здесь мы видим, что в пространстве имен параметр _count_ сразу записан как целое число (без кавычек). Если же мы попытаемся передать в качестве параметра какую-то строку, которую преобразовать в целое число невозможно, то мы получим ошибку:

$ python coolprogram.py --count Абырвалг\
\
usage: coolprogram.py \[-h] \[-c COUNT]\
coolprogram.py: error: argument -c/--count: invalid int value: 'Абырвалг'[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=44)

**Имена файлов как параметры**

Но это не все возможности параметра _type_. Во многих программах в качестве входных параметров используются имена файлов, которые нужно прочитать. При этом имена файлов передают обычно не просто так, эти файлы или читают, или в них что-то пишут, а, как известно, открытие файла - это лотерея, может быть он откроется, а может и нет, здесь всегда надо ловить исключения. А раз это такая частая операция, то _argparse_ позволяет и это автоматизировать.

Для примера напишем программу, которая просто выводит содержимое текстового файла, имя которого задается после именованного параметра _-n_ или _--name_ Для работы программы понадобится дополнительный файл "text.txt" для простоты в кодировке UTF-8.

Для того, чтобы указать _argparse_, что в качестве входного параметра мы ожидаем файл, который должен быть открыт для чтения, в метод _add\_argument_ нужно передать параметр _type_, равный _open_ (опять же, это стандартная функция для открытия файла, а не строка). Использование этого параметра показано ниже:

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. def createParser ():
8. &#x20;   parser = argparse.ArgumentParser()
9. &#x20;   parser.add\_argument ('-n', '--name', type=open)
10. &#x20;
11. &#x20;   return parser
12. &#x20;
13. &#x20;
14. if \_\_name\_\_ == '\_\_main\_\_':
15. &#x20;   parser = createParser()
16. &#x20;   namespace = parser.parse\_args(sys.argv\[1:])
17. &#x20;
18. &#x20;   print (namespace)
19. &#x20;
20. &#x20;   text = namespace.name.read()
21. &#x20;
22. &#x20;   print (text)

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=45)

Если вы запустите этот пример, то кроме содержимого файла, переданного в качестве параметра _--name_, увидите, что пространство имен _namespace_ будет содержать уже открытый файл:

$ python coolprogram.py --name text.txt\
\
Namespace(name=<\_io.TextIOWrapper name='text.txt' mode='r' encoding='UTF-8'>)\
Содержимое текстового файла[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=46)

Если же мы укажем имя несуществующего файла, то получим исключение:

1. $ python coolprogram.py --name invalid.txt
2. &#x20;
3. Traceback (most recent call last):
4. &#x20; File "coolprogram.py", line 16, in \<module>
5. &#x20;   namespace = parser.parse\_args(sys.argv\[1:])
6. &#x20; File "/usr/lib/python3.3/argparse.py", line 1714, in parse\_args
7. &#x20;   args, argv = self.parse\_known\_args(args, namespace)
8. &#x20; File "/usr/lib/python3.3/argparse.py", line 1746, in parse\_known\_args
9. &#x20;   namespace, args = self.\_parse\_known\_args(args, namespace)
10. &#x20; File "/usr/lib/python3.3/argparse.py", line 1952, in \_parse\_known\_args
11. &#x20;   start\_index = consume\_optional(start\_index)
12. &#x20; File "/usr/lib/python3.3/argparse.py", line 1892, in consume\_optional
13. &#x20;   take\_action(action, args, option\_string)
14. &#x20; File "/usr/lib/python3.3/argparse.py", line 1804, in take\_action
15. &#x20;   argument\_values = self.\_get\_values(action, argument\_strings)
16. &#x20; File "/usr/lib/python3.3/argparse.py", line 2247, in \_get\_values
17. &#x20;   value = self.\_get\_value(action, arg\_string)
18. &#x20; File "/usr/lib/python3.3/argparse.py", line 2276, in \_get\_value
19. &#x20;   result = type\_func(arg\_string)
20. FileNotFoundError: \[Errno 2] No such file or directory: 'invalid.txt'

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=47)

Как видно из строк 4-5, исключение будет брошено при попытке разобрать параметры с помощью метода _parse\_args_. Сначала кажется странным, что argparse не обрабатывает такую простую ситуацию, бросая такое некрасивое исключение. Казалось бы, почему бы библиотеке самой его не обработать и не вывести ошибку, как это было сделано при использовании параметра _type=int_?

Несмотря на то, что использование параметра _type=open_ описано в документации, на мой взгляд его лучше не использовать в таком виде, поскольку _argparse_ предлагает более красивое решение этой проблемы. Достаточно в качестве параметра _type_ передать не функцию _open_, а экземпляр класса, полученного с помощью функции _argparse.FileType_, предназначенной для безопасной попытки открытия файла.

Функция _argparse.FileType_ выглядит следующим образом и напоминает упрощенную версию функции _open_:

1. argparse.FileType(mode='r', bufsize=-1, encoding=None, errors=None)

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=48)

Здесь первый параметр задает режим открытия файла ('r' - чтение, 'w' - запись и т.д. по аналогии с функцией _open_), второй задает размер буфера, если нужно использовать буферизированое чтение, третий параметр задает кодировку открываемого файла, а последний - действие в случае ошибки декодирования файла. Эта функция создает экземпляр класса, предназначенного для работы с файлами.

Таким образом, мы можем исправить предыдущий пример:

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. def createParser ():
8. &#x20;   parser = argparse.ArgumentParser()
9. &#x20;   parser.add\_argument ('-n', '--name', type=argparse.FileType())
10. &#x20;
11. &#x20;   return parser
12. &#x20;
13. &#x20;
14. if \_\_name\_\_ == '\_\_main\_\_':
15. &#x20;   parser = createParser()
16. &#x20;   namespace = parser.parse\_args(sys.argv\[1:])
17. &#x20;
18. &#x20;   print (namespace)
19. &#x20;
20. &#x20;   text = namespace.name.read()
21. &#x20;
22. &#x20;   print (text)

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=49)

Теперь, если пользователь попытается передать имя несуществующего файла, то он увидит не большой страшный листинг исключения, а внятную ошибку:

$ python coolprogram.py --name invalid.txt\
\
usage: coolprogram.py \[-h] \[-n NAME]\
coolprogram.py: error: argument -n/--name: can't open 'invalid.txt': \[Errno 2] No such file or directory: 'invalid.txt'[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=50)

Аналогично мы можем принимать имя файла для записи, при этом только надо не забыть передать в фукнцию _argparse.FileType_ в качестве первого параметра строку 'w'.

**Обязательные именованные параметры**

До сих пор мы говорили, что позиционные параметры у нас обязательные. Напомню, что позиционными считаются те параметры, имена которых не начинаются с символов "-" или "--". Почему они называются позиционными, поговорим чуть позже, когда перейдем к разделу про разбор нескольких параметров.

Т.е. следующая программа у нас должна обязательно получить один параметр:

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. &#x20;
8. def createParser ():
9. &#x20;   parser = argparse.ArgumentParser()
10. &#x20;   parser.add\_argument ('name')
11. &#x20;
12. &#x20;   return parser
13. &#x20;
14. &#x20;
15. if \_\_name\_\_ == '\_\_main\_\_':
16. &#x20;   parser = createParser()
17. &#x20;   namespace = parser.parse\_args()
18. &#x20;
19. &#x20;   print (namespace)
20. &#x20;
21. &#x20;   print ("Привет, {}!".format (namespace.name) )

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=51)

А поскольку параметр у нас не именованный (не начинается с символов "-" или "--"), то мы не должны в командной строке указывать имя параметра. Если же мы не укажем обязательный позиционный параметр, то _argparse_ нам выведет понятную ошибку, а не будет бросать исключения. В следующем блоке показаны два случая использования этого скрипта: с переданным параметром и без него.

$ python coolprogram.py Вася\
\
Namespace(name='Вася')\
Привет, Вася!\
\
\
$ python coolprogram.py\
\
usage: coolprogram.py \[-h] name\
coolprogram.py: error: the following arguments are required: name[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=52)

Теперь вернемся к примеру, где мы этот параметр сделали именованным:

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. &#x20;
8. def createParser ():
9. &#x20;   parser = argparse.ArgumentParser()
10. &#x20;   parser.add\_argument ('-n', '--name')
11. &#x20;
12. &#x20;   return parser
13. &#x20;
14. &#x20;
15. if \_\_name\_\_ == '\_\_main\_\_':
16. &#x20;   parser = createParser()
17. &#x20;   namespace = parser.parse\_args()
18. &#x20;
19. &#x20;   print (namespace)
20. &#x20;
21. &#x20;   print ("Привет, {}!".format (namespace.name) )

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=53)

Именованные параметры по умолчанию считаются необязательными, если мы его не укажем, то получим в качестве результата работы:

Namespace(name=None)\
Привет, None\![Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=54)

Если же нас такое поведение не устраивает, мы можем указать, что этот параметр является обязательным, для этого достаточно в метод _add\_argument_ добавить параметр _required=True_

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. &#x20;
8. def createParser ():
9. &#x20;   parser = argparse.ArgumentParser()
10. &#x20;   parser.add\_argument ('-n', '--name', required=True)
11. &#x20;
12. &#x20;   return parser
13. &#x20;
14. &#x20;
15. if \_\_name\_\_ == '\_\_main\_\_':
16. &#x20;   parser = createParser()
17. &#x20;   namespace = parser.parse\_args()
18. &#x20;
19. &#x20;   print (namespace)
20. &#x20;
21. &#x20;   print ("Привет, {}!".format (namespace.name) )

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=55)

Теперь при попытке запустить программу без параметров пользователь увидит ошибку:

$ python coolprogram.py\
\
usage: coolprogram.py \[-h] -n NAME\
coolprogram.py: error: the following arguments are required: -n/--name[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=56)

**Параметры как флаги**

Иногда может возникнуть желание добавить параметры, которые должны работать как флаги, т.е. они или были указаны, или нет. Например, пусть у нас есть простейшая программа "Hello World", но если мы ей укажем параметр "--goodbye" или "-g", то программа не только поздоровается, но и попрощается с миром. :)

$ python coolprogram.py\
\
Привет, мир!\
\
\
$ python coolprogram.py --goodbye\
\
Привет, мир!\
Прощай, мир\![Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=57)

Для того, чтобы смоделировать такое поведение нам понадобится еще один параметр метода _add\_argument_, который мы до этого не использовали. Этот аргумент называется _action_, который предназначен для выполнения некоторых действий над значениями переданного параметра. Мы не будем подробно рассматривать все возможные действия, поскольку многие из них довольно специфические и требуют подробного рассмотрения, и в двух словах их не объяснишь, поэтому рассмотрим только один из вариантов использования этого параметра.

С формальной точки зрения, если мы явно не указываем значения параметра _action_, то он равен строке _"store"_, это означает, что парсер должен просто хранить полученные значения переменных, это происходило во всех приведенных выше примерах.

Для начала мы воспользуемся другим значением параметра _action_, а именно строкой _"store\_const"_, которая обозначает, что если данный параметр указан, то он всегда будет принимать значение, указанное в другом параметре метода _add\_argument_ - _const_. Если этот параметр указан не будет, то его значение будет равно _None_.

Напишем наш новый Hello/goodbye World:

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. &#x20;
8. def createParser ():
9. &#x20;   parser = argparse.ArgumentParser()
10. &#x20;   parser.add\_argument ('-g', '--goodbye', action='store\_const', const=True)
11. &#x20;
12. &#x20;   return parser
13. &#x20;
14. &#x20;
15. if \_\_name\_\_ == '\_\_main\_\_':
16. &#x20;   parser = createParser()
17. &#x20;   namespace = parser.parse\_args()
18. &#x20;
19. &#x20;   print (namespace)
20. &#x20;
21. &#x20;   print ("Привет, мир!")
22. &#x20;   if namespace.goodbye:
23. &#x20;       print ("Прощай, мир!")

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=58)

Теперь посмотрим, как он работает, и проследим за значением параметра _goodbye_:

$ python coolprogram.py\
\
Namespace(goodbye=None)\
Привет, мир!\
\
\
$ python coolprogram.py --goodbye\
\
Namespace(goodbye=True)\
Привет, мир!\
Прощай, мир\![Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=59)

Если нас смущает, что без указания параметра _--goodbye_ его значение равно _None_, а не _False_, то мы можем воспользоваться уже знакомым нам параметром _default_:

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. &#x20;
8. def createParser ():
9. &#x20;   parser = argparse.ArgumentParser()
10. &#x20;   parser.add\_argument ('-g', '--goodbye', action='store\_const', const=True, default=False)
11. &#x20;
12. &#x20;   return parser
13. &#x20;
14. &#x20;
15. if \_\_name\_\_ == '\_\_main\_\_':
16. &#x20;   parser = createParser()
17. &#x20;   namespace = parser.parse\_args()
18. &#x20;
19. &#x20;   print (namespace)
20. &#x20;
21. &#x20;   print ("Привет, мир!")
22. &#x20;   if namespace.goodbye:
23. &#x20;       print ("Прощай, мир!")

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=60)

Теперь, если этот параметр не указан, то он будет равен _False_, в остальном поведение не изменилось:

$ python coolprogram.py\
\
Namespace(goodbye=False)\
Привет, мир!\
\
\
$ python coolprogram.py --goodbye\
\
Namespace(goodbye=True)\
Привет, мир!\
Прощай, мир\![Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=61)

Может возникнуть вопрос, почему бы просто не использовать параметр _default_, зачем нужен еще _action_? Но одним _default_ мы не обойдемся, поскольку _action_ со значением _store_ (значение по умолчанию для него) подразумевает, что если параметр (в данном случае _--goodbye_) указан, то после него должно идти какое-то значение, а параметр _default_ позволит указать значение этого параметра только при отсутствии _--goodbye_.

Поскольку используемый выше прием для создания флагов довольно часто используется, предусмотрено значение параметра _action_, позволяющее не указывать значение _const_ для булевых значений: _store\_true_ и _store\_false_. При использовании этих значений указанный параметр командной строки будет принимать соответственно _True_ или _False_, если он указан. В данном случае мы можем воспользоваться значением _action="store\_true"_:

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. &#x20;
8. def createParser ():
9. &#x20;   parser = argparse.ArgumentParser()
10. &#x20;   parser.add\_argument ('-g', '--goodbye', action='store\_true', default=False)
11. &#x20;
12. &#x20;   return parser
13. &#x20;
14. &#x20;
15. if \_\_name\_\_ == '\_\_main\_\_':
16. &#x20;   parser = createParser()
17. &#x20;   namespace = parser.parse\_args()
18. &#x20;
19. &#x20;   print (namespace)
20. &#x20;
21. &#x20;   print ("Привет, мир!")
22. &#x20;   if namespace.goodbye:
23. &#x20;       print ("Прощай, мир!")

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=62)

Результат работы скрипта не изменится.

**Использование нескольких параметров**

До сих пор во всех примерах мы задавали лишь один параметр, обязательный или необязательный, поскольку нас интересовало в первую очередь то, какие свойства могут быть у параметров. В этом разделе мы рассмотрим случаи, когда программа ожидает несколько параметров, что чаще всего и бывает.

Для добавления второго, третьего и т.д. параметра необходимо повторно вызвать метод _add\_argument_ класса _ArgumentParser_, описав с помощью переданных значений свойства ожидаемого параметра.

Например, следующий скрипт ожидает два позиционных параметра - имя приветствуемого человека и число, обозначающее, сколько раз его нужно поприветствовать. В данном случае оба позиционных параметра будут являться обязательными. Причем, первый параметр всегда задает имя, а второй - количество, в данном примере поменять местами мы их не можем, отсюда и название таких параметров - "позиционные".

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. def createParser ():
8. &#x20;   parser = argparse.ArgumentParser()
9. &#x20;   parser.add\_argument ('name')
10. &#x20;   parser.add\_argument ('count', type=int)
11. &#x20;
12. &#x20;   return parser
13. &#x20;
14. &#x20;
15. if \_\_name\_\_ == '\_\_main\_\_':
16. &#x20;   parser = createParser()
17. &#x20;   namespace = parser.parse\_args(sys.argv\[1:])
18. &#x20;
19. &#x20;   print (namespace)
20. &#x20;
21. &#x20;   for \_ in range (namespace.count):
22. &#x20;       print ("Привет, {}!".format (namespace.name) )

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=63)

Различные способы запуска такой программы показаны ниже. В пером случае все задано корректно, а в остальных случаях есть ошибки: сначала аргументы перепутаны местами, вследствие чего второй параметр не удалось преобразовать в целое число, затем забыли передать один из параметров:

$ python coolprogram.py Петя 3\
\
Namespace(count=3, name='Петя')\
Привет, Петя!\
Привет, Петя!\
Привет, Петя!\
\
\
$ python coolprogram.py 3 Петя\
\
usage: coolprogram.py \[-h] name count\
coolprogram.py: error: argument count: invalid int value: 'Петя'\
\
\
$ python coolprogram.py Петя\
\
usage: coolprogram.py \[-h] name count\
coolprogram.py: error: the following arguments are required: count[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=64)

Лично я недолюбливаю позиционные параметры как раз из-за того, что надо запоминать, в каком порядке они идут. Давайте сделаем их оба именованными, в этом случае порядок их передачи будет не важен, правда, при этом придется больше вводить символов, поскольку нужно будет вводить имена параметров.

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. def createParser ():
8. &#x20;   parser = argparse.ArgumentParser()
9. &#x20;   parser.add\_argument ('-n', '--name')
10. &#x20;   parser.add\_argument ('-c', '--count', type=int, default=1)
11. &#x20;
12. &#x20;   return parser
13. &#x20;
14. &#x20;
15. if \_\_name\_\_ == '\_\_main\_\_':
16. &#x20;   parser = createParser()
17. &#x20;   namespace = parser.parse\_args(sys.argv\[1:])
18. &#x20;
19. &#x20;   print (namespace)
20. &#x20;
21. &#x20;   for \_ in range (namespace.count):
22. &#x20;       print ("Привет, {}!".format (namespace.name) )

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=65)

Теперь параметры можно менять местами, ведь имена параметров все-равно заданы.

$ python coolprogram.py --name Петя --count 3\
\
Namespace(count=3, name='Петя')\
Привет, Петя!\
Привет, Петя!\
Привет, Петя!\
\
\
$ python coolprogram.py --count 3 --name Петя\
\
Namespace(count=3, name='Петя')\
Привет, Петя!\
Привет, Петя!\
Привет, Петя\![Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=66)

А как быть, если у нас имеются и позиционные, и необязательные именованные параметры?

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. def createParser ():
8. &#x20;   parser = argparse.ArgumentParser()
9. &#x20;   parser.add\_argument ('name')
10. &#x20;   parser.add\_argument ('-c', '--count', type=int, default=1)
11. &#x20;
12. &#x20;   return parser
13. &#x20;
14. &#x20;
15. if \_\_name\_\_ == '\_\_main\_\_':
16. &#x20;   parser = createParser()
17. &#x20;   namespace = parser.parse\_args(sys.argv\[1:])
18. &#x20;
19. &#x20;   print (namespace)
20. &#x20;
21. &#x20;   for \_ in range (namespace.count):
22. &#x20;       print ("Привет, {}!".format (namespace.name) )

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=67)

В каком порядке мы должны передавать параметры? Сначала позиционные параметры, а потом именованные или наоборот? На самом деле это не важно, оба варианта будут работать:

$ python coolprogram.py Петя --count 3\
\
Namespace(count=3, name='Петя')\
Привет, Петя!\
Привет, Петя!\
Привет, Петя!\
\
$ python coolprogram.py --count 3 Петя\
\
Namespace(count=3, name='Петя')\
Привет, Петя!\
Привет, Петя!\
Привет, Петя\![Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=68)

**Использование подпарсеров**

Как вы уже поняли, библиотека _argparse_ - достаточно мощная штука, выполняющая практически полностью разбор параметров командной строки и их проверку. Мы рассмотрели достаточно много примеров ее использования, но нельзя не упомянуть об еще одной возможности, а именно о подпарсерах (subparsers). Понять, что это такое проще на примере других программ.

Наверняка вы постоянно пользуетесь какой-нибудь системой контроля версий вроде svn, git, bzr или чем-то подобным. И хотя для них написано множество графических оболочек, надеюсь, вы хоть раз запускали их из консоли. Какие особенности у этих программ с точки зрения командной строки? А особенность заключается в том, что у них в качестве первого параметра всегда выступает имя команды, за которой следуют параметры, специфические именно для этой команды. Например:

$ git add -A\
$ git commit -a -m "Текст коммита"[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=69)

Причем для каждой такой команды свои параметры, они могут иметь одинаковые имена, но обозначать разные вещи. Вот такое разделение на команды и позволяют сделать подпарсеры.

Изменим нашу программу для вывода "Hello / goodbye world" таким образом, чтобы команда _hello_ говорила программе, что нужно поприветствовать тех людей, список которых передан в параметре _--names_ или _-n_, а команда _goodbye_ указывала программе, что нужно попрощаться с миром столько раз, какое число передано с помощью параметра _--count_ или _-c_. Поскольку у нас для каждой команды используется только один параметр, возможно, стоило бы его сделать позиционным, чтобы не набирать его имя, но именованные параметры более наглядны, особенно для демонстрации.

Чтобы реализовать такое поведение нужно выполнить несколько шагов:

1. Создать экземпляр класса _ArgumentParser_ (назовем его корневым парсером).
2. Создать хранилище подпарсеров с помощью метода _add\_subparsers_ класса _ArgumentParser_ (результатом будет экземпляр внутреннего класса _\_SubParsersAction_).
3. Создать вложенный парсер с помощью метода _add\_parser_ класса хранилища парсеров. Результатом вызова каждого метода _add\_parser_ будет экземпляр класса _ArgumentParser_, уже знакомый нам.
4. Для каждого созданного вложенного парсера заполнить информацию об ожидаемых параметрах с помощью все того же метода _add\_argument_.
5. Получить пространство имен с помощью метода _parse\_args_ класса _ArgumentParser_ (корневого парсера).
6. Использовать полученное пространство имен так же, как и раньше с той лишь разницей, что в нем будет храниться имя переданной команды и параметры, специфические для нее.

Вот как выглядит текст программы для описанного чуть выше "Hello / goodbye world":

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. def createParser ():
8. &#x20;   parser = argparse.ArgumentParser()
9. &#x20;   subparsers = parser.add\_subparsers (dest='command')
10. &#x20;
11. &#x20;   hello\_parser = subparsers.add\_parser ('hello')
12. &#x20;   hello\_parser.add\_argument ('--names', '-n', nargs='+', default=\['мир'])
13. &#x20;
14. &#x20;   goodbye\_parser = subparsers.add\_parser ('goodbye')
15. &#x20;   goodbye\_parser.add\_argument ('-c', '--count', type=int, default=1)
16. &#x20;
17. &#x20;   return parser
18. &#x20;
19. &#x20;
20. def run\_hello (namespace):
21. &#x20;   """
22. &#x20;  Выполнение команды hello
23. &#x20;  """
24. &#x20;   for name in namespace.names:
25. &#x20;       print ("Привет, {}!".format (name) )
26. &#x20;
27. &#x20;
28. &#x20;
29. def run\_goodbye (namespace):
30. &#x20;   """
31. &#x20;  Выполнение команды goodbye
32. &#x20;  """
33. &#x20;   for \_ in range (namespace.count):
34. &#x20;       print ("Прощай, мир!")
35. &#x20;
36. &#x20;
37. if \_\_name\_\_ == '\_\_main\_\_':
38. &#x20;   parser = createParser()
39. &#x20;   namespace = parser.parse\_args(sys.argv\[1:])
40. &#x20;
41. &#x20;   print (namespace)
42. &#x20;
43. &#x20;   if namespace.command == "hello":
44. &#x20;       run\_hello (namespace)
45. &#x20;   elif namespace.command == "goodbye":
46. &#x20;       run\_goodbye (namespace)
47. &#x20;   else:
48. &#x20;       print ("Что-то пошло не так...")

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=70)

В реальной программе каждую команду неплохо было бы обернуть в отдельный класс, но здесь для простоты мы обойдемся обычной функцией.

Думаю, что код достаточно лаконичный, чтобы по нему не было особых вопросов. Единственное, на что хочется обратить внимание, это то, что при создании хранилища подпарсеров с помощью метода _add\_subparsers_ мы использовали именованный параметр _dest = 'command_', который указывает, что имя переданной команды в пространстве имен будет храниться под именем _command_. В принципе, этот параметр является необязательным, и если его не указать, то имя переданной команды не попадет в пространство имен, но в этом случае получить имя команды будет затруднительно.

Давайте теперь посмотрим, как работает приведенная выше программа, особенно обратите внимание на содержимое пространства имен:

$ python coolprogram.py hello --names Татьяна Александр Иван\
\
Namespace(command='hello', names=\['Татьяна', 'Александр', 'Иван'])\
Привет, Татьяна!\
Привет, Александр!\
Привет, Иван!\
\
\
$ python coolprogram.py goodbye --count 3\
\
Namespace(command='goodbye', count=3)\
Прощай, мир!\
Прощай, мир!\
Прощай, мир!\
\
\
$ python coolprogram.py\
\
Namespace(command=None)\
Что-то пошло не так...[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=71)

**Оформление справки**

До сих пор мы старались как можно больше работы возложить на _argparse_ и при этом радовались, что эта стандартная библиотека может сама проверять переданные параметры на ошибки, и в случае их возникновения выдавать нам сообщения о них. Но, честно говоря, эти сообщения по умолчанию не особо информативны для конечного пользователя. К тому же он не всегда может помнить о требуемых параметрах. В этом случае правильный пользователь вызывает справку по программе. Так уж исторически сложилось, что в UNIX-подобных операционных системах для вызова справки по программе помимо знаменитой программы _man_ используется специальный параметр командной строки _--help_ или _-h_, причем эти справки как правило выглядят примерно одинаково по оформлению. Чтобы было понятно, о чем я говорю, вызовем такую справку для программы _htop_:

$ htop -h\
\
htop 1.0.2 - (C) 2004-2011 Hisham Muhammad\
Released under the GNU GPL.\
\
\-C --no-color               Use a monochrome color scheme\
\-d --delay=DELAY            Set the delay between updates, in tenths of seconds\
\-h --help                   Print this help screen\
\-s --sort-key=COLUMN        Sort by COLUMN (try --sort-key=help for a list)\
\-u --user=USERNAME          Show only processes of a given user\
\-p --pid=PID,\[,PID,PID...]  Show only the given PIDs\
\-v --version          Print version info\
\
Long options may be passed with a single dash.\
\
Press F1 inside htop for online help.\
See 'man htop' for more information.[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=72)

В эту справку входит название программы, ее версия, копирайт, возможно, описание того, что она делает, и список ожидаемых параметров командной строки - обязательные и не очень.

Нам, как программистам, будет приятно узнать, что библиотека _argparse_ для любой программы, ее использующей, автоматически создает такую справку, правда, по умолчанию она выглядит не самым впечатляющим образом, но ее можно (нужно) доработать напильником.

Далее мы будем заниматься оформлением нашего последнего скрипта. Даже если мы ничего не будем для этого предпринимать, то справка все-равно будет создана, чтобы ее увидеть запустим скрипт с параметром _--help_ (можно и просто _-h_):

$ python coolprogram.py --help\
\
usage: coolprogram.py \[-h] {hello,goodbye} ...\
\
positional arguments:\
&#x20; {hello,goodbye}\
\
optional arguments:\
&#x20; \-h, --help       show this help message and exit[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=73)

Мы также можем получить справку о каждой команде (подпарсере):

$ python coolprogram.py hello --help\
\
usage: coolprogram.py hello \[-h] \[--names NAMES \[NAMES ...]]\
\
optional arguments:\
&#x20; \-h, --help            show this help message and exit\
&#x20; \--names NAMES \[NAMES ...], -n NAMES \[NAMES ...]\
\
\
$ python coolprogram.py goodbye --help\
\
usage: coolprogram.py goodbye \[-h] \[-c COUNT]\
\
optional arguments:\
&#x20; \-h, --help            show this help message and exit\
&#x20; \-c COUNT, --count COUNT[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=74)

Для того, чтобы повлиять на внешний вид выводимой справки, используются дополнительные параметры конструкторе _ArgumentParser_, а также в методах _add\_argument_, _add\_subparsers_ и _add\_parser_.

Прежде чем браться за код, сведем в один список основные параметры, влияющие на внешний вид справки.

* Чтобы изменить выводимое имя программы, используется параметр _prog_ конструктора класса _ArgumentParser_. Если этот параметр не указан, в качестве имени программы берется значение _sys.argv\[0]_.
* Чтобы добавить краткое описание программы, используется параметр _description_ конструктора класса _ArgumentParser_.
* Чтобы добавить пояснения после списка всех возможных параметров, используется параметр _epilog_ конструктора класса _ArgumentParser_.
* Чтобы добавить описание параметра, используется параметр _help_ метода _add\_argument_.
* Чтобы изменить имя аргумента параметра (не имя самого параметра!), используется параметр _metavar_ метода _add\_argument_.
* Чтобы добавить описание к группе подпарсеров (мы до сих пор использовали только одну группу подпарсеров, создаваемых с помощью метода _add\_subparsers_, но, в принципе, их может быть несколько), используется параметр _description_ метода _add\_subparsers_.
* Чтобы изменить заголовок группы подпарсеров (по умолчанию используется имя "subcommands"), используется параметр _title_ метода _add\_subparsers_.
* Чтобы изменить оформление списка возможных команд подпарсера, используется параметр _metavar_ метода _add\_subparsers_. По умолчанию список команд выводится в фигурных скобках: {command1, command2, ...}

Это еще не полный список того, как можно изменять внешний вид справки, но это основные моменты, которые понадобятся в первую очередь.

Давайте постепенно изменять нашу программу и смотреть на полученный результат. Сначала добавим описание для программы в целом.

Чтобы не отвлекаться на реализацию отдельных команд, далее приведен только код создания парсера, остальная часть программы осталась неизменной.

1. def createParser ():
2. &#x20;   parser = argparse.ArgumentParser(
3. &#x20;           prog = 'coolprogram',
4. &#x20;           description = '''Это очень полезная программа,
5. которая позволяет поприветствовать нужных людей,
6. или попрощаться... со всеми.''',
7. &#x20;           epilog = '''(c) Jenyay 2014. Автор программы, как всегда,
8. не несет никакой ответственности ни за что.'''
9. &#x20;           )
10. &#x20;   subparsers = parser.add\_subparsers (dest='command')
11. &#x20;
12. &#x20;   hello\_parser = subparsers.add\_parser ('hello')
13. &#x20;   hello\_parser.add\_argument ('--names', '-n', nargs='+', default=\['мир'])
14. &#x20;
15. &#x20;   goodbye\_parser = subparsers.add\_parser ('goodbye')
16. &#x20;   goodbye\_parser.add\_argument ('-c', '--count', type=int, default=1)
17. &#x20;
18. &#x20;   return parser

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=75)

Посмотрим, как теперь выглядит справка по программе:

$ python coolprogram.py -h\
usage: coolprogram \[-h] {hello,goodbye} ...\
\
Это очень нужная программа, которая позволяет поприветствовать нужных людей,\
или попрощаться... со всеми.\
\
positional arguments:\
&#x20; {hello,goodbye}\
\
optional arguments:\
&#x20; \-h, --help       show this help message and exit\
\
(c) Jenyay 2014. Автор программы, как всегда, не несет никакой ответственности\
ни за что.[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=76)

Оставим в стороне философский вопрос о том, на каком языке писать справку по программе, для простоты и наглядности мы будем везде использовать русский язык. Еще можно обратить внимание на то, что при выводе описаний были проигнорированы все переносы строк внутри многострочной конструкции '''...''', так что можно смело разбивать описания на несколько строк, а не писать их в одну строку в тысячу сто символов.

Теперь добавим описания команд и их параметров.

1. def createParser ():
2. &#x20;   parser = argparse.ArgumentParser(
3. &#x20;           prog = 'coolprogram',
4. &#x20;           description = '''Это очень полезная программа,
5. которая позволяет поприветствовать нужных людей,
6. или попрощаться... со всеми.''',
7. &#x20;           epilog = '''(c) Jenyay 2014. Автор программы, как всегда,
8. не несет никакой ответственности ни за что.'''
9. &#x20;           )
10. &#x20;   subparsers = parser.add\_subparsers (dest = 'command',
11. &#x20;           title = 'Возможные команды',
12. &#x20;           description = 'Команды, которые должны быть в качестве первого параметра %(prog)s')
13. &#x20;
14. &#x20;   hello\_parser = subparsers.add\_parser ('hello')
15. &#x20;   hello\_parser.add\_argument ('--names', '-n', nargs='+', default=\['мир'],
16. &#x20;           help = 'Список приветствуемых людей')
17. &#x20;
18. &#x20;   goodbye\_parser = subparsers.add\_parser ('goodbye')
19. &#x20;   goodbye\_parser.add\_argument ('-c', '--count', type=int, default=1,
20. &#x20;           help = 'Сколько раз попрощаться с миром')
21. &#x20;
22. &#x20;   return parser

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=77)

Теперь справка по программе и по ее командам выглядит следующим образом:

$ python coolprogram.py -h\
usage: coolprogram \[-h] {hello,goodbye} ...\
\
Это очень полезная программа, которая позволяет поприветствовать нужных людей,\
или попрощаться... со всеми.\
\
optional arguments:\
&#x20; \-h, --help       show this help message and exit\
\
Возможные команды:\
&#x20; Команды, которые должны быть в качестве первого параметра coolprogram\
\
&#x20; {hello,goodbye}\
\
(c) Jenyay 2014. Автор программы, как всегда, не несет никакой ответственности\
ни за что.[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=78)$ python coolprogram.py hello -h\
usage: coolprogram hello \[-h] \[--names NAMES \[NAMES ...]]\
\
optional arguments:\
&#x20; \-h, --help            show this help message and exit\
&#x20; \--names NAMES \[NAMES ...], -n NAMES \[NAMES ...]\
&#x20;                       Список приветствуемых людей[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=79)$ python coolprogram.py goodbye -h\
usage: coolprogram goodbye \[-h] \[-c COUNT]\
\
optional arguments:\
&#x20; \-h, --help            show this help message and exit\
&#x20; \-c COUNT, --count COUNT\
&#x20;                       Сколько раз попрощаться с миром[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=80)

В коде выше мы использовали специальный символ подстановки _%(prog)s_, который заменяется в тексте на имя программы, указанное с помощью параметра _prog_ конструктора _ArgumentParser_.

Теперь обратим свой взор на имена значений параметров. По умолчанию они совпадают с именем параметра, но написаны заглавными буквами. В данной программе еще можно понять, что под этими названиями подразумевается, но лучше дать им более осмысленные имена. Для этого воспользуемся параметром _metavar_.

1. def createParser ():
2. &#x20;   parser = argparse.ArgumentParser(
3. &#x20;           prog = 'coolprogram',
4. &#x20;           description = '''Это очень полезная программа,
5. которая позволяет поприветствовать нужных людей,
6. или попрощаться... со всеми.''',
7. &#x20;           epilog = '''(c) Jenyay 2014. Автор программы, как всегда,
8. не несет никакой ответственности ни за что.'''
9. &#x20;           )
10. &#x20;   subparsers = parser.add\_subparsers (dest = 'command',
11. &#x20;           title = 'Возможные команды',
12. &#x20;           description = 'Команды, которые должны быть в качестве первого параметра %(prog)s')
13. &#x20;
14. &#x20;   hello\_parser = subparsers.add\_parser ('hello')
15. &#x20;   hello\_parser.add\_argument ('--names', '-n', nargs='+', default=\['мир'],
16. &#x20;           help = 'Список приветствуемых людей',
17. &#x20;           metavar = 'ИМЯ')
18. &#x20;
19. &#x20;   goodbye\_parser = subparsers.add\_parser ('goodbye')
20. &#x20;   goodbye\_parser.add\_argument ('-c', '--count', type=int, default=1,
21. &#x20;           help = 'Сколько раз попрощаться с миром',
22. &#x20;           metavar = 'КОЛИЧЕСТВО')
23. &#x20;
24. &#x20;   return parser

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=81)

Теперь справка стала чуть более симпатичной:

$ python coolprogram.py hello -h\
\
usage: coolprogram hello \[-h] \[--names ИМЯ \[ИМЯ ...]]\
\
optional arguments:\
&#x20; \-h, --help            show this help message and exit\
&#x20; \--names ИМЯ \[ИМЯ ...], -n ИМЯ \[ИМЯ ...]\
&#x20;                       Список приветствуемых людей[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=82)$ python coolprogram.py goodbye -h\
\
usage: coolprogram goodbye \[-h] \[-c КОЛИЧЕСТВО]\
\
optional arguments:\
&#x20; \-h, --help            show this help message and exit\
&#x20; \-c КОЛИЧЕСТВО, --count КОЛИЧЕСТВО\
&#x20;                       Сколько раз попрощаться с миром[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=83)

Однако, что-то справку все-таки портит. Мы практически все русифицировали, остались непереведенными только фразы "usage" и "optional arguments". Займемся сейчас второй фразой.

Ее перевести будет уже чуть сложнее, но не сильно. На самом деле все параметры по умолчанию не только принадлежат какому-то парсеру, но и могут объединяться по группам. Если группы явно не заданы, то параметры попадают в одну из групп: "positional arguments" или "optional arguments". Но мы можем создать и свою группу с помощью метода _add\_argument\_group_. Этот метод возвращает экземпляр класса группы (экземпляр внутреннего класса _\_ArgumentGroup_), после чего мы должны, как и прежде, вызывать метод _add\_argument_ для добавления параметров, но уже не парсера, а группы. Это показано в следующем примере:

1. def createParser ():
2. &#x20;   parser = argparse.ArgumentParser(
3. &#x20;           prog = 'coolprogram',
4. &#x20;           description = '''Это очень полезная программа,
5. которая позволяет поприветствовать нужных людей,
6. или попрощаться... со всеми.''',
7. &#x20;           epilog = '''(c) Jenyay 2014. Автор программы, как всегда,
8. не несет никакой ответственности ни за что.'''
9. &#x20;           )
10. &#x20;   subparsers = parser.add\_subparsers (dest = 'command',
11. &#x20;           title = 'Возможные команды',
12. &#x20;           description = 'Команды, которые должны быть в качестве первого параметра %(prog)s')
13. &#x20;
14. &#x20;   hello\_parser = subparsers.add\_parser ('hello')
15. &#x20;   hello\_group = hello\_parser.add\_argument\_group (title='Параметры')
16. &#x20;
17. &#x20;   hello\_group.add\_argument ('--names', '-n', nargs='+', default=\['мир'],
18. &#x20;           help = 'Список приветствуемых людей',
19. &#x20;           metavar = 'ИМЯ')
20. &#x20;
21. &#x20;   goodbye\_parser = subparsers.add\_parser ('goodbye')
22. &#x20;   goodbye\_group = goodbye\_parser.add\_argument\_group (title='Параметры')
23. &#x20;
24. &#x20;   goodbye\_group.add\_argument ('-c', '--count', type=int, default=1,
25. &#x20;           help = 'Сколько раз попрощаться с миром',
26. &#x20;           metavar = 'КОЛИЧЕСТВО')
27. &#x20;
28. &#x20;   return parser

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=84)

Теперь справка по параметрам будет выглядеть следующим образом:

$ python coolprogram.py hello -h\
\
usage: coolprogram hello \[-h] \[--names ИМЯ \[ИМЯ ...]]\
\
optional arguments:\
&#x20; \-h, --help            show this help message and exit\
\
Параметры:\
&#x20; \--names ИМЯ \[ИМЯ ...], -n ИМЯ \[ИМЯ ...]\
&#x20;                       Список приветствуемых людей[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=85)

Уже лучше, однако группа "optional arguments" никуда не делась, поскольку параметр _--help_ / _-h_ был добавлен библиотекой еще до того, как мы создали группу. Выход из этой ситуации простой. Во-первых, надо сказать классу _ArgumentParser_, что нам его встроенная справка не нужна, для этого в конструктор передадим еще один параметр _add\_help=False_. Также надо не забыть добавить параметр _add\_help = False_ в метод _add\_parser_ для каждого подпарсера. Затем добавим параметр _--help_ / _-h_ вручную. Самим нам оформлять справку не придется, поскольку при добавлении этого параметра мы передадим в метод _add\_argument_ параметр _action='help_', который указывает, что при получении этого параметра нужно отобразить справку.

Теперь наш код создания парсера выглядит следующим образом:

1. def createParser ():
2. &#x20;   \# Создаем класс парсера
3. &#x20;   parser = argparse.ArgumentParser(
4. &#x20;           prog = 'coolprogram',
5. &#x20;           description = '''Это очень полезная программа,
6. которая позволяет поприветствовать нужных людей,
7. или попрощаться... со всеми.''',
8. &#x20;           epilog = '''(c) Jenyay 2014. Автор программы, как всегда,
9. не несет никакой ответственности ни за что.''',
10. &#x20;           add\_help = False
11. &#x20;           )
12. &#x20;
13. &#x20;   \# Создаем группу параметров для родительского парсера,
14. &#x20;   \# ведь у него тоже должен быть параметр --help / -h
15. &#x20;   parent\_group = parser.add\_argument\_group (title='Параметры')
16. &#x20;
17. &#x20;   parent\_group.add\_argument ('--help', '-h', action='help', help='Справка')
18. &#x20;
19. &#x20;
20. &#x20;   \# Создаем группу подпарсеров
21. &#x20;   subparsers = parser.add\_subparsers (dest = 'command',
22. &#x20;           title = 'Возможные команды',
23. &#x20;           description = 'Команды, которые должны быть в качестве первого параметра %(prog)s')
24. &#x20;
25. &#x20;   \# Создаем парсер для команды hello
26. &#x20;   hello\_parser = subparsers.add\_parser ('hello',
27. &#x20;           add\_help = False)
28. &#x20;
29. &#x20;   \# Создаем новую группу параметров
30. &#x20;   hello\_group = hello\_parser.add\_argument\_group (title='Параметры')
31. &#x20;
32. &#x20;   \# Добавляем параметры
33. &#x20;   hello\_group.add\_argument ('--names', '-n', nargs='+', default=\['мир'],
34. &#x20;           help = 'Список приветствуемых людей',
35. &#x20;           metavar = 'ИМЯ')
36. &#x20;
37. &#x20;   hello\_group.add\_argument ('--help', '-h', action='help', help='Справка')
38. &#x20;
39. &#x20;
40. &#x20;   \# Создаем парсер для команды goodbye
41. &#x20;   goodbye\_parser = subparsers.add\_parser ('goodbye', add\_help = False)
42. &#x20;
43. &#x20;   \# Создаем новую группу параметров
44. &#x20;   goodbye\_group = goodbye\_parser.add\_argument\_group (title='Параметры')
45. &#x20;
46. &#x20;   \# Добавляем параметры
47. &#x20;   goodbye\_group.add\_argument ('-c', '--count', type=int, default=1,
48. &#x20;           help = 'Сколько раз попрощаться с миром',
49. &#x20;           metavar = 'КОЛИЧЕСТВО')
50. &#x20;
51. &#x20;   goodbye\_group.add\_argument ('--help', '-h', action='help', help='Справка')
52. &#x20;
53. &#x20;   return parser

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=86)

Выглядит почти идеально:

$ python coolprogram.py hello -h\
\
usage: coolprogram hello \[--names ИМЯ \[ИМЯ ...]] \[--help]\
\
Параметры:\
&#x20; \--names ИМЯ \[ИМЯ ...], -n ИМЯ \[ИМЯ ...]\
&#x20;                       Список приветствуемых людей\
&#x20; \--help, -h            Справка[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=87)$ py3 coolprogram.py goodbye -h\
\
usage: coolprogram goodbye \[-c КОЛИЧЕСТВО] \[--help]\
\
Параметры:\
&#x20; \-c КОЛИЧЕСТВО, --count КОЛИЧЕСТВО\
&#x20;                       Сколько раз попрощаться с миром\
&#x20; \--help, -h            Справка[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=88)

Не забыли мы и про справку для корневого парсера:

$ python coolprogram.py -h\
usage: coolprogram \[--help] {hello,goodbye} ...\
\
Это очень полезная программа, которая позволяет поприветствовать нужных людей,\
или попрощаться... со всеми.\
\
Параметры:\
&#x20; \--help, -h       Справка\
\
Возможные команды:\
&#x20; Команды, которые должны быть в качестве первого параметра coolprogram\
\
&#x20; {hello,goodbye}\
\
(c) Jenyay 2014. Автор программы, как всегда, не несет никакой ответственности\
ни за что.[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=89)

Осталось сделать самую малость - добавить описания для команд. Для этого воспользуемся параметрами _help_ и _description_ метода _add\_parser_.

1. def createParser ():
2. &#x20;   \# Создаем класс парсера
3. &#x20;   parser = argparse.ArgumentParser(
4. &#x20;           prog = 'coolprogram',
5. &#x20;           description = '''Это очень полезная программа,
6. которая позволяет поприветствовать нужных людей,
7. или попрощаться... со всеми.''',
8. &#x20;           epilog = '''(c) Jenyay 2014. Автор программы, как всегда,
9. не несет никакой ответственности ни за что.''',
10. &#x20;           add\_help = False
11. &#x20;           )
12. &#x20;
13. &#x20;   \# Создаем группу параметров для родительского парсера,
14. &#x20;   \# ведь у него тоже должен быть параметр --help / -h
15. &#x20;   parent\_group = parser.add\_argument\_group (title='Параметры')
16. &#x20;
17. &#x20;   parent\_group.add\_argument ('--help', '-h', action='help', help='Справка')
18. &#x20;
19. &#x20;
20. &#x20;   \# Создаем группу подпарсеров
21. &#x20;   subparsers = parser.add\_subparsers (dest = 'command',
22. &#x20;           title = 'Возможные команды',
23. &#x20;           description = 'Команды, которые должны быть в качестве первого параметра %(prog)s')
24. &#x20;
25. &#x20;   \# Создаем парсер для команды hello
26. &#x20;   hello\_parser = subparsers.add\_parser ('hello',
27. &#x20;           add\_help = False,
28. &#x20;           help = 'Запуск в режиме "Hello, world!"',
29. &#x20;           description = '''Запуск в режиме "Hello, world!".
30. В этом режиме программа приветствует список людей, переданных в качестве параметра.''')
31. &#x20;
32. &#x20;   \# Создаем новую группу параметров
33. &#x20;   hello\_group = hello\_parser.add\_argument\_group (title='Параметры')
34. &#x20;
35. &#x20;   \# Добавляем параметры
36. &#x20;   hello\_group.add\_argument ('--names', '-n', nargs='+', default=\['мир'],
37. &#x20;           help = 'Список приветствуемых людей',
38. &#x20;           metavar = 'ИМЯ')
39. &#x20;
40. &#x20;   hello\_group.add\_argument ('--help', '-h', action='help', help='Справка')
41. &#x20;
42. &#x20;
43. &#x20;   \# Создаем парсер для команды goodbye
44. &#x20;   goodbye\_parser = subparsers.add\_parser ('goodbye',
45. &#x20;           add\_help = False,
46. &#x20;           help = 'Запуск в режиме "Goodbye, world!"',
47. &#x20;           description = '''В этом режиме программа прощается с миром.''')
48. &#x20;
49. &#x20;   \# Создаем новую группу параметров
50. &#x20;   goodbye\_group = goodbye\_parser.add\_argument\_group (title='Параметры')
51. &#x20;
52. &#x20;   \# Добавляем параметры
53. &#x20;   goodbye\_group.add\_argument ('-c', '--count', type=int, default=1,
54. &#x20;           help = 'Сколько раз попрощаться с миром',
55. &#x20;           metavar = 'КОЛИЧЕСТВО')
56. &#x20;
57. &#x20;   goodbye\_group.add\_argument ('--help', '-h', action='help', help='Справка')
58. &#x20;
59. &#x20;   return parser

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=90)

Теперь справка выглядит полностью законченной:

$ python coolprogram.py -h\
\
usage: coolprogram \[--help] {hello,goodbye} ...\
\
Это очень полезная программа, которая позволяет поприветствовать нужных людей,\
или попрощаться... со всеми.\
\
Параметры:\
&#x20; \--help, -h       Справка\
\
Возможные команды:\
&#x20; Команды, которые должны быть в качестве первого параметра coolprogram\
\
&#x20; {hello,goodbye}\
&#x20;   hello          Запуск в режиме "Hello, world!"\
&#x20;   goodbye        Запуск в режиме "Goodbye, world!"\
\
(c) Jenyay 2014. Автор программы, как всегда, не несет никакой ответственности\
ни за что.[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=91)$ python coolprogram.py hello -h\
\
usage: coolprogram hello \[--names ИМЯ \[ИМЯ ...]] \[--help]\
\
Запуск в режиме "Hello, world!". В этом режиме программа приветствует список\
людей, переданных в качестве параметра.\
\
Параметры:\
&#x20; \--names ИМЯ \[ИМЯ ...], -n ИМЯ \[ИМЯ ...]\
&#x20;                       Список приветствуемых людей\
&#x20; \--help, -h            Справка[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=92)$ python coolprogram.py goodbye -h\
\
usage: coolprogram goodbye \[-c КОЛИЧЕСТВО] \[--help]\
\
В этом режиме программа прощается с миром.\
\
Параметры:\
&#x20; \-c КОЛИЧЕСТВО, --count КОЛИЧЕСТВО\
&#x20;                       Сколько раз попрощаться с миром\
&#x20; \--help, -h            Справка[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=93)

И чтобы окончательно навести порядок, не мешало бы добавить еще один параметр в родительский парсер, а именно параметр _--version_ для показа текущей версии программы. Это легко реализуется благодаря параметру _action='version_' метода _add\_argument_.

1. \#!/usr/bin/python
2. \# -\*- coding: UTF-8 -\*-
3. &#x20;
4. import sys
5. import argparse
6. &#x20;
7. version = "1.2.1"
8. &#x20;
9. &#x20;
10. def createParser ():
11. &#x20;   \# Создаем класс парсера
12. &#x20;   parser = argparse.ArgumentParser(
13. &#x20;           prog = 'coolprogram',
14. &#x20;           description = '''Это очень полезная программа,
15. которая позволяет поприветствовать нужных людей,
16. или попрощаться... со всеми.''',
17. &#x20;           epilog = '''(c) Jenyay 2014. Автор программы, как всегда,
18. не несет никакой ответственности ни за что.''',
19. &#x20;           add\_help = False
20. &#x20;           )
21. &#x20;
22. &#x20;   \# Создаем группу параметров для родительского парсера,
23. &#x20;   \# ведь у него тоже должен быть параметр --help / -h
24. &#x20;   parent\_group = parser.add\_argument\_group (title='Параметры')
25. &#x20;
26. &#x20;   parent\_group.add\_argument ('--help', '-h', action='help', help='Справка')
27. &#x20;
28. &#x20;   parent\_group.add\_argument ('--version',
29. &#x20;           action='version',
30. &#x20;           help = 'Вывести номер версии',
31. &#x20;           version='%(prog)s {}'.format (version))

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=94)

Для наглядности номер версии будет храниться в глобальной переменной _version_, а воспользуемся мы ей в строках 28-31, где опять используется символ подстановки _%(prog)s_, который, напомню, обозначает имя программы, после которого мы добавим номер версии. Воспользуемся этим параметром:

$ pythono coolprogram.py --version\
\
coolprogram 1.2.1[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=95)

А справка по параметрам теперь выглядит следующим образом:

$ python coolprogram.py -h\
\
usage: coolprogram \[--help] \[--version] {hello,goodbye} ...\
\
Это очень полезная программа, которая позволяет поприветствовать нужных людей,\
или попрощаться... со всеми.\
\
Параметры:\
&#x20; \--help, -h       Справка\
&#x20; \--version        Вывести номер версии\
\
Возможные команды:\
&#x20; Команды, которые должны быть в качестве первого параметра coolprogram\
\
&#x20; {hello,goodbye}\
&#x20;   hello          Запуск в режиме "Hello, world!"\
&#x20;   goodbye        Запуск в режиме "Goodbye, world!"\
\
(c) Jenyay 2014. Автор программы, как всегда, не несет никакой ответственности\
ни за что.[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=96)

И последнее, что нам осталось научиться делать со справкой - это принудительно ее выводить, например, при вводе пользователем каких-то неправильных комбинаций параметров (в том числе, когда он вводит взаимоисключающие параметры).

Сейчас у нас в случае, если пользователь не вводит никакую команду, выводится текст: "Что-то пошло не так...", но буквально одной строкой мы изменим это поведение таким образом, чтобы в этом случае выводилась справка. Для этого достаточно вызвать метод _print\_help_ класса _ArgumentParser_:

1. if \_\_name\_\_ == '\_\_main\_\_':
2. &#x20;   parser = createParser()
3. &#x20;   namespace = parser.parse\_args(sys.argv\[1:])
4. &#x20;
5. &#x20;   print (namespace)
6. &#x20;
7. &#x20;   if namespace.command == "hello":
8. &#x20;       run\_hello (namespace)
9. &#x20;   elif namespace.command == "goodbye":
10. &#x20;       run\_goodbye (namespace)
11. &#x20;   else:
12. &#x20;       parser.print\_help()

[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=97)

В остальном програма осталась без изменений. Попробуем запустить ее без параметров:

$ python coolprogram.py\
\
Namespace(command=None)\
usage: coolprogram \[--help] \[--version] {hello,goodbye} ...\
\
Это очень полезная программа, которая позволяет поприветствовать нужных людей,\
или попрощаться... со всеми.\
\
Параметры:\
&#x20; \--help, -h       Справка\
&#x20; \--version        Вывести номер версии\
\
Возможные команды:\
&#x20; Команды, которые должны быть в качестве первого параметра coolprogram\
\
&#x20; {hello,goodbye}\
&#x20;   hello          Запуск в режиме "Hello, world!"\
&#x20;   goodbye        Запуск в режиме "Goodbye, world!"\
\
(c) Jenyay 2014. Автор программы, как всегда, не несет никакой ответственности\
ни за что.[Исходник](https://jenyay.net/Programming/Argparse?action=sourceblock\&num=98)

Как видите, оформление справки занимает довольно много места, но по сути в этом нет ничего сложного, нужно просто заполнить нужные параметры.

#### Заключение <a href="#outro" id="outro"></a>

Наконец-то эта статья заканчивается. Интересно, много ли людей дочитали до этих строк, их хочется поблагодарить за терпение.

В этой статье мы рассмотрели основные возможности стандартной Python-библиотеки _argparse_, посмотрели, как она упрощает жизнь по сравнению с ручным разбором параметров, научились создавать именованные и позиционные параметры, рассмотрели различные варианты их использования, в том числе в виде флагов, а также научились создавать команды и оформлять справку.

Несмотря на большой объем статьи, некоторые вопросы все-равно остались "за бортом". Например, мы не рассмотрели "склеивание" параметров (когда вместо git add -a -m "..." вы пишете просто git -am "..."). Проигнорировали классы, позволяющие еще более углубиться в оформление справки, не рассмотрели некоторые возможности параметра _action_ метода _add\_argument_, а также не поговорили о том, что на самом деле можно изменить формат параметров вида "-h" на формат вида "/h", принятый в Windows. Но все эти вопросы описаны в справке, в которой только надо найти описание нужной возможности.

Не мешало бы еще рассмотреть альтернативные нестандартные библиотеки для разбора командной строки, но это уже тема для отдельной статьи.
