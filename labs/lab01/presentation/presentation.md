---
## Front matter
lang: ru-RU
title: Отчёт по лабораторной работе №1
subtitle: Подготовка среды моделирования GNS3
author:
  - Калашникова Д. В.
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 10 сентября 2026

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
 - '\makeatletter'
 - '\beamer@ignorenonframefalse'
 - '\makeatother'

## Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
---

# Информация

## Докладчик

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

  * Калашникова Дарья Викторовна
  * Студент
  * Российский университет дружбы народов
  * [1132243108@pfur.ru](mailto:1132243108@pfur.ru)

:::
::: {.column width="30%"}

![](image/kalashnikova.jpeg)

:::
::::::::::::::

## Цель

Установка и настройка GNS3 и сопутствующего программного обеспечения

## Задачи

1. Установить GNS3-all-in-one, GNS3 VM, проверить корректность запуска
2. Импортировать образ маршрутизатора FRR
3. Импортировать образ маршрутизатора VyOS
4. Проверить корректность работы FRR и VyOS

## Импорт GNS3 VM

Запускаем VirtualBox и через меню «Файл → Импорт конфигураций» указываем образ GNS3 VM

![Импорт GNS3 VM](image/001.png){height=70%}

## Настройка памяти GNS3 VM

Выделяем 4096 МБ оперативной памяти для стабильной работы серверной части

![Настройка памяти](image/002.png){height=50%}

## Вложенная виртуализация

Включаем «Nested VT-x/AMD-V», чтобы GNS3 VM могла запускать вложенные устройства

![Nested VT-x/AMD-V](image/003.png){height=50%}

## Сетевой адаптер

Проверяем настройки сетевого адаптера и сохраняем изменения

![Сетевой адаптер](image/004.png){height=70%}

## Запуск GNS3 VM

Запускаем виртуальную машину GNS3 VM и дожидаемся её полной загрузки

![Запуск GNS3 VM](image/005.png){height=70%}

## Мастер настройки GNS3

При первом запуске выбираем вариант «Run appliance in a virtual machine»

![Мастер настройки](image/006.png){height=70%}

## Совместный запуск

Запускаем VirtualBox, затем GNS3 VM, и уже после этого — приложение GNS3

![Совместный запуск](image/007.png){height=70%}

## Выбор источника образа

В окне мастера указываем установку образа с GNS3-сервера

![Источник образа](image/008.png){height=70%}

## Импорт FRR

Выбираем категорию Routers и образ FRR, запускаем установку

![Выбор FRR](image/009.png){height=70%}

## Завершение импорта FRR

После завершения скачивания импортируем образ FRR и завершаем установку

![Импорт FRR](image/010.png){height=70%}

## Настройка шаблона FRR

Указываем параметры QEMU, объём памяти и способ доступа к консоли

![Шаблон FRR](image/011.png){height=70%}

## Загрузка VyOS

Переходим к добавлению образа VyOS — скачиваем его с GitHub

![Загрузка VyOS](image/012.png){height=70%}

## Создание шаблона VyOS

На левой панели GNS3 открываем список маршрутизаторов и нажимаем «+ New template»

![Шаблон VyOS](image/013.png){height=50%}

## Установка VyOS

Выбираем категорию Routers и образ VyOS, нажимаем Install и настраиваем шаблон

![Установка VyOS](image/014.png){height=70%}

## Создание проекта

Создаём новый проект и добавляем в него оба маршрутизатора — FRR и VyOS

![Создание проекта](image/015.png){height=70%}

## Проверка FRR

Запускаем FRR и открываем его консоль

![Проверка FRR](image/016.png){height=70%}

## Проверка VyOS

Запускаем VyOS и открываем его консоль

![Проверка VyOS](image/017.png){height=70%}

## Выводы

В ходе работы были установлены GNS3-all-in-one и GNS3 VM, настроено их взаимодействие через VirtualBox. Импортированы и проверены шаблоны маршрутизаторов FRR и VyOS. Цель работы достигнута.
