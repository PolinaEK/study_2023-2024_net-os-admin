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

Приобретение практических навыков по установке и конфигурированию SMTP-
сервера.

# Задание

1. Установите на виртуальной машине server SMTP-сервер postfix.
2. Сделайте первоначальную настройку postfix при помощи утилиты postconf, задав
отправку писем не на локальный хост, а на сервер в домене.
3. Проверьте отправку почты с сервера и клиента.
4. Сконфигурируйте Postfix для работы в домене. Проверьте отправку почты с сервера
и клиента.
5. Напишите скрипт для Vagrant, фиксирующий действия по установке и настройке
Postfix во внутреннем окружении виртуальной машины server. Соответствующим
образом внесите изменения в Vagrantfile.

# Выполнение лабораторной работы

Установка Postfix

1. На виртуальной машине server войдите под вашим пользователем и откройте тер-
минал. Перейдите в режим суперпользователя:
sudo -i
2. Установите необходимые для работы пакеты:
dnf -y install postfix
dnf -y install s-nail
3. Сконфигурируйте межсетевой экран, разрешив работать службе протокола SMTP:
firewall-cmd --add-service=smtp
firewall-cmd --add-service=smtp --permanent
firewall-cmd --list-services
4. Восстановите контекст безопасности в SELinux:
restorecon -vR /etc
5. Запустите Postfix(рис. [-@fig:001])

![Запускаем Postfix](image/1.png){#fig:001 width=70%}

Изменение параметров Postfix с помощью postconf

Первоначальную настройку Postfix осуществите, используя postconf.
1. Для просмотра списка текущих настроек Postfix введите:
postconf
2. Посмотрите текущее значение параметра myorigin:
postconf myorigin
3. Посмотрите текущее значение параметра mydomain:
postconf mydomain
Должно быть указано mydomain = user.net, где вместо user указан ваш логин.
4. Замените значение параметра myorigin на значение параметра mydomain:
postconf -e ‘myorigin = $mydomain’
5. Повторите команду
postconf myorigin
Убедитесь, что замена параметра была произведена.
6. Проверьте корректность содержания конфигурационного файла main.cf:
postfix check
7. Перезагрузите (перечитайте) конфигурационные файлы Postfix:
systemctl reload postfix
8. Просмотрите все параметры с значением, отличным от значения по умолчанию:
postconf -n
9. Задайте жёстко значение домена (вместо user укажите свой логин):
postconf -e 'mydomain = user.net'
10. Отключите IPv6 в списке разрешённых в работе Postfix протоколов и оставьте только
IPv4:
postconf inet_protocols
postconf -e 'inet_protocols = ipv4'
11. Перезагрузите конфигурацию Postfix. (рис. [-@fig:002])

![Перезагружаем конфигурацию Postfix](image/2.png){#fig:002 width=70%}

Проверка работы Postfix

1. На сервере под учётной записью пользователя отправьте себе письмо, используя
утилиту mail:
echo .| mail -s test1 user@server.user.net
(вместо user укажите свой логин).
2. На втором терминале запустите мониторинг работы почтовой службы и посмотрите,
что произошло с вашим сообщением:
tail -f /var/log/maillog
В отчёте отразите, доставлено сообщение или нет с пояснением, на основе каких
строк лога вы сделали вывод. Дополнительно посмотрите содержание каталога
/var/spool/mail на предмет того, появился ли там каталог вашего пользователя
с отправленным письмом.
3. На виртуальной машине client войдите под вашим пользователем и откройте тер-
минал. Перейдите в режим суперпользователя:
sudo -i
4. На клиенте установите необходимые для работы пакеты:
dnf -y install postfix
dnf -y install s-nail
68 Лабораторная работа № 8
5. Отключите IPv6 в списке разрешённых в работе Postfix протоколов и оставьте только
IPv4:
postconf inet_protocols
postconf -e 'inet_protocols = ipv4'
6. На клиенте запустите Postfix:
systemctl enable postfix
systemctl start postfix
7. На клиенте под учётной записью пользователя аналогичным образом отправьте
себе второе письмо, используя утилиту mail. Сравните результат мониторинга поч-
товой службы на сервере при отправке сообщения с сервера и с клиента. В отчёте
отразите, доставлено сообщение или нет.
8. На сервере в конфигурации Postfix посмотрите значения параметров сетевых ин-
терфейсов inet_interfaces и сетевых адресов mynetworks:
postconf inet_interfaces
postconf mynetworks
9. Разрешите Postfix прослушивать соединения не только с локального узла, но и с дру-
гих интерфейсов сети:
postconf -e 'inet_interfaces = all'
10. Добавьте адрес внутренней сети, разрешив таким образом пересылку сообщений
между узлами сети:
postconf -e 'mynetworks = 127.0.0.0/8, 192.168.0.0/16'
11. Перезагрузите конфигурацию Postfix и перезапустите Postfix:
postfix check
systemctl reload postfix
systemctl stop postfix
systemctl start postfix
12. Повторите отправку сообщения с клиента. В отчёте отразите, что произошло с ва-
шим сообщением.(рис. [-@fig:003])

![Перезапускаем Postfix](image/3.png){#fig:003 width=70%}

Конфигурация Postfix для домена

1. С клиента отправьте письмо на свой доменный адрес:
echo .| mail -s test2 user@user.net
(вместо user укажите свой логин).
2. Запустите мониторинг работы почтовой службы и посмотрите, что произошло
с вашим сообщением:
tail -f /var/log/maillog
В отчёте отразите, что произошло с вашим сообщением.
3. Дополнительно посмотрите, какие сообщения ожидают в очереди на отправление:
postqueue -p
4. Для настройки возможности отправки сообщений не на конкретный узел сети,
а на доменный адрес пропишите MX-запись с указанием имени почтового сервера
mail.user.net (вместо user укажите свой логин) в файле прямой DNS-зоны.
5. В конфигурации Postfix добавьте домен в список элементов сети, для которых дан-
ный сервер является конечной точкой доставки почты:
postconf -e 'mydestination = $myhostname, localhost.$mydomain, localhost,
$mydomain'↪
6. Перезагрузите конфигурацию Postfix:
postfix check
systemctl reload postfix
7. Восстановите контекст безопасности в SELinux:
restorecon -vR /etc
restorecon -vR /var/named
8. Перезапустите DNS:
systemctl restart named
9. Попробуйте отправить сообщения, находящиеся в очереди на отправление:
postqueue -f
10. Проверьте отправку почты с клиента на доменный адрес.(рис. [-@fig:004])

![Отправляем сообщения](image/4.png){#fig:004 width=70%}

Внесение изменений в настройки внутреннего окружения
виртуальной машины

1. На виртуальной машине server перейдите в каталог для внесения изменений в на-
стройки внутреннего окружения /vagrant/provision/server/.
2. Замените конфигурационные файлы DNS-сервера:
cd /vagrant/provision/server/dns/var/named
cp -R /var/named/* /vagrant/provision/server/dns/var/named
3. В каталоге /vagrant/provision/server создайте исполняемый файл mail.sh.
4. На виртуальной машине client перейдите в каталог для внесения изменений в на-
стройки внутреннего окружения /vagrant/provision/client/:
cd /vagrant/provision/client
5. В каталоге /vagrant/provision/client создайте исполняемый файл mail.sh.
6. Для отработки созданного скрипта во время загрузки виртуальной машины server
в конфигурационном файле Vagrantfile необходимо добавить в разделе конфигу-
рации для сервера.
7. Для отработки созданного скрипта во время загрузки виртуальной машины client
в конфигурационном файле Vagrantfile необходимо добавить в разделе конфигу-
рации для клиента.(рис. [-@fig:005])

![Добавляем изменения](image/5.png){#fig:005 width=70%}



# Выводы

Мы приобрели практические навыки по установке и конфигурированию SMTP-
сервера.



