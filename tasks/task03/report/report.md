---
title: "Отчёт о лабораторной работе"
subtitle: "Лабораторная работа 3"
author: "Калашникова Дарья Викторовна"
lang: ru-RU
toc-title: "Содержание"
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl
toc: true
toc-depth: 2
lof: true
lot: true
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
babel-lang: russian
babel-otherlangs: english
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
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float}
  - \floatplacement{figure}{H}
---

# Цель работы

Освоить работу с Wireshark: разобрать кадры Ethernet и проанализировать PDU транспортного и прикладного уровней стека TCP/IP.

# Выполнение

Сначала выполним ipconfig. В выводе увидим несколько подключений, среди них и активное — беспроводная сеть. Из него мы узнаём ipv6 и ipv4 адреса, маску подсети и адрес основного шлюза (рис. [-@fig:001]).

![ipconfig](image/1.png){#fig:001 width=70%}

Если добавить к ipconfig ключ /all, данных станет больше — в частности, появится физический (mac) адрес. У нас он равен 44-a3-bb-57-9b-03. Из этих шести байт первые три — идентификатор производителя, оставшиеся три — самого интерфейса. Старший байт 44 говорит о том, что адрес индивидуальный и глобально администрируемый: два младших бита этого байта при переводе из hex в двоичную дают нули. Также в выводе видны ipv4-адрес 192.168.1.76, маска 255.255.255.0, шлюз 192.168.1.1, аренда получена 6 октября 2026 г. до 8:54:45 (рис. [-@fig:002]).

![ipconfig /all](image/2.png){#fig:002 width=70%}

Открываем wireshark и выбираем активное сетевое подключение. У нас это беспроводная сеть (рис. [-@fig:003]).

![Выбор соединения](image/3.png){#fig:003 width=70%}

Так выглядит сам интерфейс (рис. [-@fig:004]).

![Wireshark](image/4.png){#fig:004 width=70%}

Попробуем отправить ping на шлюз 192.168.1.1. Все 4 пакета получили ответ с TTL=64 и временем 1 мс. Потерь нет (рис. [-@fig:005]).

![ping](image/5.png){#fig:005 width=70%}

Возвращаемся в wireshark и смотрим на icmp-пакеты. Поскольку ушло 4 пакета, видим 8 строк — по одной на отправку и получение для каждого пакета. Отправитель — 192.168.1.76, получатель — 192.168.1.1 (рис. [-@fig:006]).

![icmp](image/6.png){#fig:006 width=70%}

Разберём самый первый, исходящий пакет. Длина кадра — 74 байта, тип — ethernet 2; физический адрес отправителя (наш) — Intel_57:9b:03 (44:a3:bb:57:9b:03), получателя (шлюз) — Keentic_dd:e4:c6 (50:ff:20:dd:e4:c6). Оба адреса индивидуальные и глобально администрируемые. Внутри — IPv4 и ICMP (рис. [-@fig:007]).

![Анализ исходящего пакета](image/7.png){#fig:007 width=70%}

Теперь посмотрим на ответный фрейм. Он почти не отличается от исходящего — только адреса отправителя и получателя стоят наоборот: 50:ff:20:dd:e4:c6 → 44:a3:bb:57:9b:03 (рис. [-@fig:008]).

![Анализ входящего пакета](image/8.png){#fig:008 width=70%}

Переходим к arp-пакетам. Информации в них немного; задача этих пакетов — узнать физический адрес. Первый arp-пакет — запрос: `Who has 192.168.1.76? Tell 192.168.1.1`, то есть шлюз пытается определить адрес нашего устройства (рис. [-@fig:009]).

![arp](image/9.png){#fig:009 width=70%}

Второй arp-пакет — ответ: `192.168.1.76 is at 44:a3:bb:57:9b:03`. В обоих случаях в заголовке ethernet 2 лежит физический адрес обоих устройств, но в первом пакете тип — arp (request), во втором — arp (reply) (рис. [-@fig:010]).

![Второй arp пакет](image/10.png){#fig:010 width=70%}

Теперь пингуем rudn.ru — он не отвечает: превышен интервал ожидания для запроса, 4 отправлено, 0 получено, 100 % потерь. Зато ya.ru (77.88.44.242) отвечает: 4 отправлено, 4 получено (рис. [-@fig:011]).

![ping rudn.ru и ya.ru](image/11.png){#fig:011 width=70%}

Смотрим на arp-пакеты для ya.ru: снова запрос `Who has 192.168.1.76? Tell 192.168.1.1` и ответ `192.168.1.76 is at 44:a3:bb:57:9b:03` (рис. [-@fig:012]).

![arp пакеты](image/12.png){#fig:012 width=70%}

Второй arp-пакет, как обычно, отличается только направлением (рис. [-@fig:013]).

![Второй arp пакет](image/13.png){#fig:013 width=70%}

Теперь смотрим icmp-пакеты для ya.ru: их восемь. Обращаемся мы уже не к шлюзу, а напрямую к серверу Яндекса — ip-адрес 77.88.44.242. Однако на уровне ethernet 2 мы по-прежнему общаемся со шлюзом (рис. [-@fig:014]).

![Исходящий пакет icmp](image/14.png){#fig:014 width=70%}

Входящий icmp — ответный. Отличается от исходного тем, что адреса поменяны местами: источник 77.88.44.242, получатель 192.168.1.76 (рис. [-@fig:015]).

![Входящий пакет icmp](image/15.png){#fig:015 width=70%}

Откроем сайт по протоколу http — info.cern.ch (рис. [-@fig:016]).

![Подключение по http](image/16.png){#fig:016 width=70%}

Смотрим http-пакеты: GET /user/a03/www/default/NikhefGuide.html HTTP/1.1 уходит на 192.16.186.153 с порта 52037 на 80. Работает http поверх tcp — видим порты, длину сегмента, флаги (PSH, ACK) и чексумму (рис. [-@fig:017]).

![Исходящий http пакет](image/17.png){#fig:017 width=70%}

У входящего пакета источник — 192.16.186.153, порт 80, а получатель — 192.168.1.76, порт 52037. Длина сегмента 1189, ответ `HTTP/1.1 302 Found` (рис. [-@fig:018]).

![Входящий http пакет](image/18.png){#fig:018 width=70%}

Теперь рассмотрим dns-пакеты. Они работают по udp, порт источника 53 — то есть пакеты приходят от DNS-сервера 77.88.8.1. Видны порты, длина и чексумма (рис. [-@fig:019]).

![Исходящий dns пакет](image/19.png){#fig:019 width=70%}

Входящий dns-пакет на уровне udp отличается портами: источник 53, получатель 49664. Также отличаются длина и чексумма (рис. [-@fig:020]).

![Входящий dns пакет](image/20.png){#fig:020 width=70%}

Теперь посмотрим на quic. Он работает поверх udp: порт источника 54581, получателя — 443. Внутри udp содержится примерно та же информация, что и в dns: порты, длина, чексумма (рис. [-@fig:021]).

![Исходящий quic пакет](image/21.png){#fig:021 width=70%}

У входящего quic-пакета порты идут в обратном порядке: 443 → 54581. Также отличаются длина и чексумма (рис. [-@fig:022]).

![Входящий quic пакет](image/22.png){#fig:022 width=70%}

Теперь разберём процесс хендшейка. Для этого обновим http-страницу и посмотрим, что происходит перед http-запросом get. Сначала клиент (192.168.1.76, порт 57608) отправляет серверу (13.249.8.110, порт 443) syn-запрос. Во флагах tcp видим только SYN, sequence number — 0 (рис. [-@fig:023]).

![syn](image/23.png){#fig:023 width=70%}

Сервер отвечает syn и ack: источник 13.249.8.110, порт 443, получатель 192.168.1.76, порт 57608. Во флагах видим SYN, ACK, sequence number 0, acknowledgment number 1 (рис. [-@fig:024]).

![syn + ack](image/24.png){#fig:024 width=70%}

Клиенту остаётся пожать руку — отправить ack. Источник 192.168.1.76, порт 57608, получатель 13.249.8.110, порт 443. Во флагах только ACK, sequence number 1, acknowledgment number 1 (рис. [-@fig:025]).

![ack](image/25.png){#fig:025 width=70%}

Посмотрим на график потока. На нём хорошо видно хендшейк (SYN, SYN+ACK, ACK) прямо перед запросом GET (рис. [-@fig:026]).

![Хендшейк на графике потока](image/26.png){#fig:026 width=70%}

# Выводы

В результате выполнения работы были получены навыки анализа пакетов в wireshark.

# Список литературы {.unnumbered}

1. Wireshark Foundation. Wireshark User's Guide [Электронный ресурс]. — URL: https://www.wireshark.org/docs/wsug_html_chunked/ (дата обращения: 07.10.2026).

2. Wireshark Foundation. Wireshark Display Filter Reference [Электронный ресурс]. — URL: https://www.wireshark.org/docs/dfref/ (дата обращения: 07.10.2026).

3. IETF. RFC 826: An Ethernet Address Resolution Protocol [Электронный ресурс]. — URL: https://datatracker.ietf.org/doc/html/rfc826 (дата обращения: 07.10.2026).
