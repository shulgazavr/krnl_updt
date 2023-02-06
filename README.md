## krnl_updt
# Тема работы: Обновление ядра.
### Описание домашнего задания:
#### 1. Обновить ядро ОС из репозитория ELRepo
#### 2. Создать Vagrant box c помощью Packer
#### 3. Загрузить Vagrant box в Vagrant Cloud



В ходе выполнения работ, при помощи ПО Vagrant и конфигурации Vagrantfile, представленной в описании домашнего задания, была развёрнута виртуальная машина с ОС CentOS 8 Stream.
Версия ядра установленной системы:
```sh
$ uname -r
4.18.0-277.el8.x86_64
```
Далее, на этой системе, была обновлено ядро:
```sh
$ sudo yum install -y https://www.elrepo.org/elrepo-release-8.el8.elrepo.noarch.rpm
$ sudo yum --enablerepo elrepo-kernel install kernel-ml -y
$ sudo grub2-mkconfig -o /boot/grub2/grub.cfg
$ sudo grub2-set-default 0
$ sudo reboot 
```

После обновления ядра:
```sh
$ uname -r
6.1.10-1.el8.elrepo.x86_64
```

Следующим шагом, используя методическое пособие по домашнему заданию, были созданы каталоги и файлы, необходимые для реализации второй задачи:
```sh
# tree
.
├── packer
│   ├── centos.json
│   ├── http
│   │   └── ks.cfg
│   ├── scripts
│   │   ├── stage-1-kernel-update.sh
│   │   └── stage-2-clean.sh
└── Vagrantfile

3 directories, 5 files
```

После успешного создания образа в Packer, в текущем каталоге был создан файл сentos-8-kernel-6-x86_64-Minimal.box.
Импортировав полученный vagrant box в Vagrant, была проверена вверсия ядра:
```sh
uname -r
6.1.10-1.el8.elrepo.x86_64
```

Далее публикуем наш образ в vagrant cloud: 
```sh
vagrant cloud publish --release shulgazavr /centos8-kernel6 1.0 virtualbox centos-8-kernel-6-x86_64-Minimal.box
```

### Примечания
> Потратил некоторое время на попытку запустить packer build по ssh на удалённой машине. Не удалось, и, к ожалению, не смог разобраться почему. Запуск packer непосредственно на этой же машине прошёл успешно.
