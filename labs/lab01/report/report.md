---
## Front matter
title: "Отчет о лабораторной работе"
subtitle: "Лабораторная работа №1"
author: "Калашникова Дарья"

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

Установка и настройка GNS3 и сопутствующего программного обеспечения.

# Задание

1. Установить GNS3-all-in-one, GNS3 VM, проверить корректность запуска.
2. Импортировать в GNS3 образ маршрутизатора FRR.
3. Импортировать в GNS3 образ маршрутизатора VyOS.
4. Проверить корректность работы маршрутизаторов FRR и VyOS.

# Выполнение лабораторной работы

Запускаем VirtualBox и через меню «Файл → Импорт конфигураций» указываем заранее скачанный образ GNS3 VM (рис. [-@fig:001]).

![Импорт конфигурации GNS3 VM в VirtualBox](image/001.png){#fig:001 width=70%}

Уточняем параметры виртуальной машины GNS3 VM: выделяем 4096 МБ оперативной памяти для стабильной работы серверной части (рис. [-@fig:002]).

![Настройка объёма оперативной памяти GNS3 VM](image/002.png){#fig:002 width=70%}

Включаем вложенную виртуализацию — ставим галочку «Включить Nested VT-x/AMD-V», чтобы GNS3 VM могла запускать вложенные виртуальные устройства (рис. [-@fig:003]).

![Включение вложенной виртуализации Nested VT-x/AMD-V](image/003.png){#fig:003 width=70%}

Проверяем настройки сетевого адаптера и сохраняем изменения (рис. [-@fig:004]).

![Проверка сетевого адаптера GNS3 VM](image/004.png){#fig:004 width=70%}

Запускаем виртуальную машину GNS3 VM и дожидаемся её полной загрузки (рис. [-@fig:005]).

![Запуск виртуальной машины GNS3 VM](image/005.png){#fig:005 width=70%}

При первом запуске приложения GNS3 открывается мастер настройки, в котором выбираем вариант «Run appliance in a virtual machine» (рис. [-@fig:006]).

![Мастер настройки GNS3: выбор режима работы](image/006.png){#fig:006 width=70%}

Запускаем систему виртуализации VirtualBox, затем GNS3 VM, и уже после этого — приложение GNS3 из основной операционной системы (рис. [-@fig:007]).

![Совместный запуск VirtualBox, GNS3 VM и клиента GNS3](image/007.png){#fig:007 width=70%}

В окне мастера указываем рекомендуемое значение — установку образа с GNS3-сервера (рис. [-@fig:008]).

![Выбор источника образа: установка с GNS3-сервера](image/008.png){#fig:008 width=70%}

В следующем окне выбираем категорию Routers и образ FRR, после чего запускаем установку (рис. [-@fig:009]).

![Выбор образа маршрутизатора FRR в каталоге appliances](image/009.png){#fig:009 width=70%}

После завершения скачивания импортируем образ FRR и завершаем установку (рис. [-@fig:010]).

![Завершение импорта образа FRR](image/010.png){#fig:010 width=70%}

Настраиваем шаблон FRR: указываем параметры QEMU, объём памяти и способ доступа к консоли (рис. [-@fig:011]).

![Настройка шаблона FRR](image/011.png){#fig:011 width=70%}

Переходим к добавлению образа маршрутизатора VyOS — скачиваем его с GitHub (рис. [-@fig:012]).

![Загрузка образа VyOS с GitHub](image/012.png){#fig:012 width=70%}

Как и в случае с FRR, на левой панели GNS3 открываем список маршрутизаторов и нажимаем «+ New template» (рис. [-@fig:013]).

![Открытие мастера создания шаблона VyOS](image/013.png){#fig:013 width=70%}

В следующем окне выбираем категорию Routers и образ VyOS, нажимаем Install и настраиваем шаблон (рис. [-@fig:014]).

![Выбор и установка образа VyOS](image/014.png){#fig:014 width=70%}

Создаём новый проект и добавляем в него оба маршрутизатора — FRR и VyOS (рис. [-@fig:015]).

![Создание проекта с маршрутизаторами FRR и VyOS](image/015.png){#fig:015 width=70%}

Проверяем работоспособность каждого маршрутизатора: запускаем устройства и открываем их консоли (рис. [-@fig:016], [-@fig:017]).

![Проверка работы маршрутизатора FRR](image/016.png){#fig:016 width=70%}

![Проверка работы маршрутизатора VyOS](image/017.png){#fig:017 width=70%}

# Выводы

В ходе работы были установлены GNS3-all-in-one и GNS3 VM, настроено их взаимодействие через VirtualBox. Импортированы и проверены шаблоны маршрутизаторов FRR и VyOS. Цель работы достигнута.