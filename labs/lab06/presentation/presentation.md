---
## Front matter
lang: ru-RU
title: Лабораторная работа №6
author:
  - Кичигина Полина Евгеньевна
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 2 мая 2026

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
toc-title: Содержание
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

## Цель работы

Развить навыки администрирования ОС Linux. Получить первое прак-
тическое знакомство с технологией SELinux.
Проверить работу SELinx на практике совместно с веб-сервером
Apache.

## Выполнение лабораторной работы

Войдите в систему с полученными учётными данными и убедитесь, что
SELinux работает в режиме enforcing политики targeted. Обратитесь с помощью браузера к веб-серверу, запущенному на вашем
компьютере, и убедитесь, что последний работает.

![Выполняем команды](image/1.png){#fig:001 width=50%}

##

Определите круг пользователей, которым разрешено создание файлов в
директории /var/www/html. Изучите справку man httpd_selinux и выясните, какие контек-
сты файлов определены для httpd. Измените контекст файла /var/www/html/test.html с
httpd_sys_content_t на любой другой, к которому процесс httpd не
должен иметь доступа.

![Выполняем](image/2.png){#fig:002 width=50%}

##


Просмотрите log-файлы веб-сервера Apache. Также просмотрите системный лог-файл.
Если в системе окажутся запущенными процессы setroubleshootd и
audtd, то вы также сможете увидеть ошибки, аналогичные указанным
выше, в файле /var/log/audit/audit.log.

![Выполняем задание](image/3.png){#fig:003 width=70%}

##

Выполните перезапуск веб-сервера Apache Верните контекст httpd_sys_cоntent__t к файлу /var/www/html/ test.html.
 Исправьте обратно конфигурационный файл apache, вернув Listen 80. Удалите привязку http_port_t к 81 порту и проверьте, что порт 81 удалён. Удалите файл /var/www/html/test.html.

![Удаляем файл](image/4.png){#fig:004 width=50%}








## Выводы

Мы развили навыки администрирования ОС Linux. Получили первое практическое знакомство с технологией SELinux.
Проверили работу SELinx на практике совместно с веб-сервером
Apache.

