---
type: post
author: hrafn
tags:
- samba
- howto
- translate
comments: false
date: 2010-02-01T08:37:27Z
published: true
title: '[RH Knowledgebase] Корзина в Samba как на Рабочем Столе'
Slug: rh-knowledgebase-корзина-в-samba-как-на-рабочем-столе
url: /2010/02/korzina-v-samba-kak-na-rabochem-stole
---

Перевод [статьи](http://kbase.redhat.com/faq/docs/DOC-4802).

**Можно ли сделать так, чтобы в Samba была Корзина вроде той, что есть на Рабочем Столе?**

Да. У Samba есть возможность перемещать удаленные объекты в специально созданную папку вроде той, которая используется для удаленных объектов на локальной машине. Что является преимуществом по сравнению с обычной конфигурацией Samba, поскольку обычно, если файл удаляется пользователем, то удаляется окончательно. Единственный очевидный недостаток - это увеличение количества дискового пространства в связи с хранением удаленных файлов. Для решения этой проблемы будет необходимо периодически очищать эту Корзину.

В этой статье предполагается, что у становлена samba 3-x.x. Это можно проверить следующей командой:

	# rpm -q samba

Если необходимая версия Samba не установлена, обратитесь к статьям, коих множество, описывающих установку/обновление корректной версии Samba.

Для реализации сетевой корзины Samba используется модуль Virtual File System (VFS). Различные модули VFS, которые могут использоваться Samba, расположены в локальной директории: `/usr/lib/samba/vfs`.

Документация по опциям модуля recycle.so, а также другим модулям VFS, может найдена в директории `/usr/share/doc/samba-x.x/docs/Samba-HOWTO-collection.pdf` в главе 19 или на сайте [http://www.samba.org/samba/docs/man/Samba-HOWTO-Collection/VFS.html](http://www.samba.org/samba/docs/man/Samba-HOWTO-Collection/VFS.html).

Для реализации Корзины Samba необходимо просто отредактировать один из ваших общих ресурсов, пример такой редакции указан ниже:

	# cat /etc/samba/smb.conf
	#============= Share Definitions =============
	[SambaShare]
	path = /home/scripts
	public = yes
	writable = yes
	browsable = yes
	vfs object = recycle
	recycle:repository = .deleted/%U
	recycle:keeptree = Yes
	recycle:touch = Yes
	recycle:versions = Yes
	recycle:maxsize = 0
	recycle:exclude = *.tmp
	recycle:exclude_dir = /tmp
	recycle:noversions = *.doc

Эти настройки только осуществляют создание Корзины в каталоге "Samba Share". Эти опции должны быть указаны для каждой расшаренной папки Samba, для которой необходимо включить данную функциональность.

Самая интересная опция указана на самом верху:

	recycle:repository = .deleted/%U

В этой строке определяется, где будут храниться удаленные файлы. Указывается относительный путь касательно расшаренного каталога. В указанном примере - это `/home/scripts`. Таким образом, все удаленное будет перемещено в директорию `.deleted` по этому пути. Переменная `%U` - имя пользователя, в данное время просматривающего данный расшаренный каталог. Для каждого пользователя, удаляющего файл, существует каталог с его именем пользователя, в котором
хранятся все файлы, которые он удалил.

Например:

Скотт просматривается каталог "Samba Share" и удаляет файл. Удаленный файл можно будет найти в `/home/scripts/.deleted/Scott`.

Брэд просматривает каталог "Samba Share" и удаляет файл. Удаленный файл можно найти в `/home/scripts/.deleted/Brad`.

Важно заметить, что каталог .deleted должен быть создан заранее. Это позволит затем пользователям записывать в эту директорию. Это просто пример и настройка может быть изменена в зависимости от конкретной ситуации.
