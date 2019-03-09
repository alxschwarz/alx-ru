---
type: post
author: hrafn
tags:
- linux
- support
- thoughts
comments: false
date: 2011-01-12T09:42:47Z
published: true
title: LTS или не LTS. Вот в чем вопрос...
Slug: lts-или-не-lts-вот-в-чем-вопрос
url: /2011/01/lts-ili-ne-lts-vot-v-chem-vopros
---

Первый раз про Russian CentOS Remix я услышал достаточно давно, уже наверно
несколько месяцев прошло. Мысли свои по этому поводу я выскажу чуть позже в
этом посте. Но на днях встретил упоминание про Remix сначала в блоге у Аркадия
Шейна ([http://tigro.info/wp/?p=2100](http://tigro.info/wp/?p=2100)), а позже,
в этот же день, и в твиттере было обсуждение. Ссылку на обсуждение дать
затруднительно, поскольку треды в твиттере - это нечто :) Собственно, этот
пост пишется как раз по мотивам обсуждений в твиттере, а также в связи с тем,
что мне захотелось немного порассуждать о том, есть ли необходимость в
долгосрочной поддержке десктопных систем или нет, если есть, то кому это надо
и в каком виде.

Если мне память не отказывает, то фактически долгосрочная поддержка
дистрибутивов на данный момент присутствует у Novell (SLES & SLED), Canonical
(Ubuntu) и Red Hat. В связке с последним идет и CentOS, пересобирающий пакеты
от них. Стоит уточнить, что я не рассматриваю именно платную поддержку или
что-то подобное. Здесь имеется ввиду, по большей части, не платность или
бесплатность, а выпуск обновлений для продуктов. К числу дистрибутивов,
которые фактически являются долгосрочноподдерживаемыми, я бы причислил и
Debian, в основном за счет того, что выпускается в данное время по готовности,
и время между выпусками достаточно приличное, хотя и сильно меньше, чем у
других. Спорить о том, какая из систем поддерживается дольше и поддержку какой
из систем реально можно считать долгосрочной, я не буду. Разговор-то все же не
о том.

Я некоторое время использовал на своем десктопе, а позже и ноутбуке, SLED10 и
SLED11 от Novell. Да, многих веще не хватало, поскольку в первую очередь
данная система предназначена все же корпоративных пользователей. Однако
основные требующиеся мне пакеты там присутствовали, проблема была только с
поддержкой мультимедиа. Которая решалась подключением сначала репозиториев от
openSUSE, что неправильно, а потом репозитория от стороннего человека. К
сожалению, ссылки я сейчас не нашел, позже, если удастся найти - выложу. На
мой взгляд, система работала стабильнее, чем openSUSE, собственно по этой
причине она и была выбрана. Стоимость годовой поддержки и он-лайн доступа к
обновлениям достаточно невелика - порядка 1500 рублей на тот момент.

После моего перехода на redhat-based дистрибутивы я пробовал использовать
RHEL5 Desktop и CentOS на ноутбуке. Ммм, впечатления странные, честно говоря.
С одной стороны, недостаток опыта работы с ними на тот момент, с другой
стороны - откровенно устаревшее программное обеспечение, к которому привыкнуть
после SLED11 и Fedora было достаточно тяжело. Однако, мне известны люди,
которые используют CentOS в качестве десктопной системы и вполне довольны. Да
и сам я на Центе ни разу не встречал случаи неудачной загрузки после
обновления ядра, например, с которыми я сталкивался на Fedora.

Данный пост пишется с ноутбука, работающего под RHEL6 Desktop. И он устраивает
мне более чем полностью :) Не хватает пока репозитория RPMFusion для шестой
версии RHEL, но надеюсь, что он вскоре появится. Вопросы мультимедиа решаются
подключением дополнительных репозиториев, которых достаточно для комфортной
работы.

С Ubuntu я сталкивался неоднократно. Был период, когда я работал именно в этой
системе. Да и сейчас я периодически, после выхода новых версий, смотрю, что
нового появилось и каковы изменения. На мой взгляд, LTS-релизы Ubuntu, кроме
увеличенного срока поддержки, ни чем особо не отличаются от всех остальных
релизов. Хотя иногда складывается впечатление, что они, как ни странно,
получаются менее стабильными, чем обычные. Честно говоря, мне практически
неизестны люди, использующие именно Ubuntu LTS для десктопа.

Дебиан. Про Дебиан мне сказать практически нечего. Стабильные версии
используются, в основном, для серверов, но есть несколько известным мне людей,
которые именно стабильные релизы Дебиана используют для своего повседневного
использования. Устаревшие приложения их не пугают, поскольку от системы
требуется именно стабильность и конечный результат работы, а не новые функции
и свежее программное обеспечение.

Получился такой краткий обзорчик того, что я сам использовал на ноутбуке :) А
теперь поговорим собственно о самой теме. На примере CentOS.

Так нужен ли дистрибутив с продолжительным сроков выпуска обновлений
программного обеспечения в десктопном варианте? Я считаю, что нужен. Как
минимум, нужен мне. Поскольку удобно, стабильно и предсказуемо.

Нужен ли аналог Russian Fedora Remix, но только для CentOS? По мне - так
нужен. Когда я впервые прочитал про возможность такого варианта, мне это не
понравилось. Увеличенный срок поддержки добавляет нагрузку, поскольку придет
время, когда будет необходимо поддерживать сразу две, а потом и три версии
дистрибутива, если следовать срокам поддержки от Red Hat и CentOS. Кроме того,
не было никакой информации о том, что именно будет изменено в Russian CentOS
Remix, а для серверного использования мне изменения видеть не хотелось бы. Тут
важна совместимость с Red Hat.

Сейчас же мое мнение изменилось. В основном, по причине того, что изменения,
которые, я думаю, будут сделаны, касаются в целом десктопного варианта
использования системы: мультимедиа и исправление глупых ошибок, как написал
[@atigro](http://twitter.com/#!/atigro/status/24544772127457281). Если в итоге
это будет сделано по аналогии с RFR, но такой вариант пожалуй нужен даже
гораздо больше, чем сам RFR, уж пусть простят меня пользователи Русской Федоры
:)

Пока у меня еще была работа :), мне несколько раз приходилось решать, какой из
дистрибутивов Linux предложить для установки на обычные десктопные машины для
клиентов. В тот момент CentOS для десктопов я даже не рассматривал, по разным
причинам. И большие затраты по времени по настройке различных вещей для
нормального десктопного использования, включая мультимедиа, приведение
рабочего стола и шрифтов в приличный вид и т.д. Да, можно сделать свою
собственную сборку дистрибутива и устанавливать уже ее, но тут есть некоторые
моменты, которые заставили отказаться от этой идеи: затраты на дальнейшую
поддержку своей версии в актуальном состоянии плюс мой возможный уход на
другое место работы и соответственно опять-таки поддержка всего этого добра. В
итоге была выбрана Ubuntu, как наиболее приспособленный вариант для десктопа
по умолчанию.

Во время обсуждения в твиттере высказывалось мнение, что обновления
программного обеспечения для, например, секретарши не нужны совсем. На мой
взгляд, это слегка неверное мнение. Все-таки обновления приносят не только
расширение возможностей для приложений, но и исправляют ошибки, которые не
зависят от того, секретарша ли сидит на компьютером или кто-то другой :) И
исправлять их нужно. Поскольку на глазок определять, где может всплыть ошибка,
тяжело и бессмысленно. Посему, обновления нужны в любом случае.

Клиенты на моей бывшей работе не хотели платить лишние деньги за ОС,
устанавливаемые на рабочие машины. В основном, ради этого переход на Linux и
осуществлялся, поэтому дистрибутивы с платной поддержкой - SLE* и Red Hat - не
рассматривались. Как я уже написал ранее, была выбрана Ubuntu. Скажем так, я
был вынужден принять это решение, поскольку альтернативы предложить я не мог.
Про CentOS в нынешнем виде я уже тожу упоминал, хотя, при возможности, я бы
предпочел именно его, поскольку все сервера на CentOS и поддерживать единую
инфраструктуру сильно проще. Но Russian CentOS Remix на тот момент еще не было
:) Да и сейчас пока нет. Но очень хочется, чтобы было...

Сейчас перечитал написанное. Да уж, сумбурно, не совсем по теме местами, да и
конкретных доводов нет. Согласен. Это все моя ИМХА :)
