---
type: post
tags:
- Fedora 18
- SELinux
- Security
- LXC
comments: false
date: 2012-11-13T00:00:00Z
title: 'Security in Fedora 18 Part 7: Secure Linux Containers'
Slug: security-in-fedora-18-part-7-secure-linux-containers
url: /2012/11/13/new-security-feature-in-fedora-18-part-7-secure-linux-containers/
---

И вот очередная порция. [Оригинал][1]

[В Fedora 18 мы добавили в пакет libvirt-sandbox возможность простого создания Безопасных Контейнеров.][2]
Контейнеры - это форма изоляции одного или более процессов от всей остальной системы. Иногда контейнеры описываются как облегченная виртуализация. На самом деле, контейнеры просто представление пространства пользователя. В ядре Linux нет концепции контейнера. Ядро обеспечивает пространство имен и cgroups. Инструменты пространства пользователя могут объединять эти инструменты в "контейнеры".

#### Пространства имен

Пространства имен - это способ изменения представления процессов своего окружения со стороны его родительских процессов. Например, пространство имен файловой системы позволяет мне изменять отображение процессов иерархии файловой системы. `pam_namespace`, давно представленный в Fedora6/RHEL5, позволяет программе входа в систему создать пространство имен и монтировать файловую систему, которая не будет видна процессам предка. Значит, у меня могло бы быть множество процессов с различными директориями `/tmp` и множество домашних директорий, смонтированных в `/home/dwalsh`.

В настоящее время ядро предоставляет 5 пространств имен.

1. **mount** - монтирование и отмонтирование файловых систем не влияет на всю систему целиком.
2. **UTS** - установка имени хоста, доменного имени не влияет на всю систему целиком.
3. **IPC** - у процесса будет независимое пространство имен для очереди сообщений System V, набора семафоров и сегментов разделяемой памяти.
4. **network** - у процесса будет независимые IPv4 и IPv6 стэк, таблица маршрутизации IP, правила межсетевого экрана, дерево директорий `/proc/net` и `/sys/class/net`, сокеты и т.д.
5. **pid** - процессы имеют PIDs, независимые от остальной системы. Каждое пространство имен может иметь свой собственный pid 1.

**Заметка**: Пространство имен UID разработано, но пока не готово для использования, а у меня есть представление о том, насколько хорошо это все будет работать. Наши инструменты в настоящее время не используют пространство имен UID.

`pam_namespace`, `sandbox -X`, `unshare`, `systemd` позволяют получить преимущества пространств имен.

#### [CGROUPS][3]

Википедиа описывает cgroups так:

- cgroups (контрольные группы) - это возможность ядла Linux ограничивать, составлять и изолировать использование ресурсов (CPU, память, I/O диска) групп процессов.

В основном, cgroups используются для контроля количества ресурсов процесса или группы процессов, выделяемых системой.
[Я сделал небольшой скринкаст cgroups, чтобы продемонстрировать их мощь.][4]

#### LXC

Инструменты вроде LXC временно существуют для того, чтобы позволить пользователям создавать контейнеры, но эти инструменты низкоуровневые.

#### Libvirt-lxc

"Libvirt - это инструмент разработчика на С для взаимодействия с возможностями виртуализации последних версий Linux (и других ОС). Основной пакет включает сервер libvirtd, использующий поддержку виртуализации."

libvirt-lxc был представлен в Fedora 16. Он расширяет libvirt API для предоставления пользователям возможности создавать контейнеры, используя libvirt. Что позволяет управлять виртуализацией kvm/qemu совместно с контейнерами Linux, все в пределах одного фреймворка. Единственная проблема в том, что настройка контейнера с использованием libvirt api сказочно сложна.

#### [libvirt-sandbox][5]

[Dan Berrange][6] создал в Fedora 17 новый пакет, названный libvirt-sandbox. Это пакет предоставляет библиотеку для разработки приложений (libvirt-sandbox) для облегчения встраивания виртуализации в приложения. Одно из основных преимуществ нового инструмента состоит в том, что оно сильно упростило API для создания виртуальных машин и контейнеров.

#### SELinux

Само по себе использование контейнеров не дает хорошего безопасного разделения. Причина этого в том, что файловые системы ядра, типа `/proc`, `/sys`, `cgroupfs` и `selinuxfs`, не находятся в контейнерах. Привилегированные процессы, запущенные внутри контейнера, могут повлиять на другие процессы за пределами контейнера или на процессы, запущенные в других контейнерах. В libvirt-sandbox и libvirt-lxc вы можете использовать SELinux Labelling для блокировки привилегированных процессов, например, для предотвращения монтирования случайных файловых систем или от выключения SELinux процессами.

#### virt-sandbox-service

Dan Berrange и я вместе работали над улучшениями libvirt-sandbox. Мы добавили инструмент командной строки под названием `virt-sandbox-service`, который позволяет пользователю легко создавать песочницу для приложения. `virt-sandbox-service` дает возможность администратору запускать множество служб на одной и той же машине, а каждую службу в безопасном контейнере. Вот некоторые основные возможности контейнеров `virt-sandbox-service`:

- использование systemd внутри контейнера в качестве процессов init
- использование стандартных файлов для запуска и остановки контейнеризованных приложений
- общий раздел `/usr`, означающий, что если у вас запущены сотни контейнеров Apache, а код Apache обновится, каждый контейнер будет сразу же использовать новую версию.
- используется SELinux MCS Labelling для разделения каждого контейнера, предотвращая вмешательство даже процессов root в работу хоста или других контейнеров.

Назначение этого инструмента - не в том, чтобы позволять основным целевым приложениям запускать внутри контейнера, несмотря на то, что мы будем работать над тем, чтобы большая часть служб могла быть запущена. Инструмент нацелен не на запуск в песочнице ОС целиком, а на наиболее часто используемые приложения.

У меня есть предварительные тесты по запуску `httpd`, `mysql`, `ostgresql`, `dovecot` внутри таких контейнеров. Я призываю людей поиграть с этим инструментом и помочь нам расширить список приложений, которые можно будет запускать внутри контейнера. Также вы можете запустить множество приложений внутри контейнера в одно и то же время. Например, я тестировал `httpd` и `mysql`, запущенные внутри одного и того же контейнера.

#### Как использовать:

	# yum install libvirt-sandbox httpd

Прямо сейчас в инструменте существует ошибка, из-за которой он не может работать при отсутствии файла `/selinux`.

	# touch /selinux

Используйте `virt-sandbox-service` для создания контейнера:

	virt-sandbox-service create -C -l s0:c1,c2 -u httpd.service container1
	Created sandbox container dir /var/lib/libvirt/filesystems/container1
	Created sandbox config /etc/libvirt-sandbox/services/container1.sandbox
	Created unit file /etc/systemd/system/container1_sandbox.service

Манипулируйте данные внутри контейнера из-за пределов контейнера

	cd /var/lib/libvirt/filesystems/container1/var/log
	touch content
	ls -lZ content
	# Убедитесь, что контент был создан с корректной меткой MCS.
	# Контент должен быть помечен s0:c1,c2, а не s0
	# Теперь создадим файл с неправильной для контейнера меткой.
	cat "Secret" > badcontent
	chcon -l s0:c3,c4 badcontent

Запустите контейнер:

	virt-sandbox-service start container1

в другом окне.

Убедитесь, что процессы запущены с соответствующей меткой SELinux - `ps -eZ | grep svirt_lxc`. Вы должны увидеть процессы вроде `systemd`, `systemd-journal`, `dhclient` и `httpd`, запущенные внутри контейнера с меткой MCS - `s0:c1,c2`.

Подключитесь к контейнеру:

	virt-sandbox-service connect container1
	id 
	getenforce   # Должно сообщить, что SELinux выключен.
	setenforce 1 # Должно быть запрещено
	touch /file  # Должно быть запрещено создавать этот файл
	touch /var/www/html/content  # Должно быть разрешено
	cat /var/www/html/badcontent # Должно быть щапрещено
	# Настройте сервер apache любым способом и поиграйтесь с html-страницами
	ifconfig eth0  # Вытащите IP адрес для использования в следующем тесте
	^] 

На вашем хосте в Firefox укажите IP внутри контейнера

	firefox $IP # Используйте IP адрес контейнера, убедитесь, что видите контент.

Остановите контейнер

	virt-sandbox-service stop container1

Теперь давайте попробйем сделать то же самое, но запуск и остановку контейнера будем производить с помощью systemctl.

	systemctl start container1_sandbox.service
	systemctl enable container1_sandbox.service

Убедимся, что контейнер запущен

	virt-sandbox-service connect container1
	ps -eZ
	^]

Я был бы рад услышал, что вы думаете. Какие улучшения вы хотели бы увидеть? Какие приложения вы хотели бы запустить внутри контейнера?

Поскольку это первая версия, мы думаем, что тут может быть еще множество проблем, так что используйте на свой страх и риск, но мы хотели бы работать вместе с сообществом над улучшением этих инструментов.

[1]: http://danwalsh.livejournal.com/59144.html
[2]: https://fedoraproject.org/wiki/Features/Securecontainers
[3]: https://en.wikipedia.org/wiki/Cgroups
[4]: http://people.fedoraproject.org/~dwalsh/SELinux/Presentations/cgroups.ogv
[5]: https://fedoraproject.org/wiki/Features/VirtSandbox
[6]: http://berrange.com/