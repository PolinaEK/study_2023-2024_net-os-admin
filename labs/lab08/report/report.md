---
## Front matter
title: "Отчёт по лабораторной работе №8"
subtitle: "Отчет"
author: "Кичигина Полина Евгеньевна"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Получение навыков работы с планировщиками событий cron и at.

# Задание

1. Выполните задания по планированию задач с помощью crond.

2. Выполните задания по планированию задач с помощью atd.

# Выполнение лабораторной работы

1. Запустите терминал и получите полномочия администратора. Посмотрите статус демона crond. Посмотрите содержимое файла конфигурации. Посмотрите список заданий в расписании.
Ничего не отобразится, так как расписание ещё не задано.
Откройте файл расписания на редактирование.
Команда запустит интерфейс редактора (по умолчанию используется vi). Добавь-
те следующую строку в файл расписания (запись сообщения в системный журнал),
используя Ins для перехода в vi в режим ввода:
*/1 * * * * logger This message is written from root cron

Закройте сеанс редактирования vi и сохраните изменения. Посмотрите список заданий в расписании.
В расписании должна появиться запись о запланированном событии.
 Не выключая систему, через некоторое время (2–3 минуты) просмотрите журнал
системных событий. (рис. [-@fig:001])

![Смотрим журнал событий](image/1.png){#fig:001 width=70%}

2.  Посмотрите список заданий в расписании. Перейдите в каталог /etc/cron.hourly и создайте в нём файл сценария с именем
eachhour. Откройте файл eachhour для редактирования и пропишите в нём скрипт. Сделайте файл сценария eachhour исполняемым. Теперь перейдите в каталог /etc/crond.d и создайте в нём файл с расписанием
eachhour.
Откройте этот файл для редактирования и поместите в него следующее содержимое:
11 * * * * root logger This message is written from /etc/cron.d
Сохраните изменения. Не выключая систему, через некоторое время (2–3 часа) просмотрите журнал систем-
ных событий. (рис. [-@fig:002])

![Смотрим изменения](image/2.png){#fig:002 width=70%}

3. Планирование заданий с помощью at.

Запустите терминал и получите полномочия администратора. Проверьте, что служба atd загружена и включена. Задайте выполнение команды logger message from at в 9:30 (или замените на
любое другое время, когда вы работаете над этим упражнением). Убедитесь, что задание действительно запланировано.
С помощью команды grep 'from at' /var/log/messages посмотрите, появилось
ли соответствующее сообщение в лог-файле в указанное вами время. (рис. [-@fig:003])

![Проверяем запланировалось ли задание](image/3.png){#fig:003 width=70%}


# Выводы

Мы получили навыки работы с планировщиками событий cron и at.



