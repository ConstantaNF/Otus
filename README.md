# ****Обновление ядра в базовой системе**** #

### Описание домашнего задания ###

1. Запустить ВМ с помощью Vagrant.
2. Обновить ядро ОС из репозитория ELRepo.
3. Оформить отчет в README-файле в GitHub-репозитории.

### **Выполнение** ###

Задание выполняется на рабочей станции с ОС Ubuntu 22.04.4 LTS с заранее установленными Vagrant 2.4.1 и VirtualBox 7.0. Перед выполнением предварительно подготовлен репозиторий <https://github.com/ConstantaNF/Otus> по методичке.

### Подготовка и запуск ВМ с помощью Vagrant ###

Перехожу в заранее подготовленный каталог для Vagrant и создаю каталог для будущей ВМ:

`cd ~/vagrantbox`

`mkdir centos8`

Перехожу в созданный каталог. Так как VPN пока не настроен (хотя есть WPS с Wireguard-сервером) загружаю из <https://app.vagrantup.com/boxes/search> box generic/centos8s:

`cd centos8`

`wget https://app.vagrantup.com/generic/boxes/centos8s/versions/4.3.12/providers/virtualbox/amd64/vagrant.box` 

Результат загрузки:

`--2024-03-10 01:16:24--  https://app.vagrantup.com/generic/boxes/centos8s/versions/4.3.12/providers/virtualbox/amd64/vagrant.box
Распознаётся app.vagrantup.com (app.vagrantup.com)… 54.204.238.15, 54.221.251.148, 54.209.91.188, ...
Подключение к app.vagrantup.com (app.vagrantup.com)|54.204.238.15|:443... соединение установлено.
HTTP-запрос отправлен. Ожидание ответа… 302 Found
Адрес: https://app.vagrantup.com/generic/boxes/centos8s/versions/4.3.12/providers/virtualbox/amd64/download/vagrant.box [переход]
--2024-03-10 01:16:25--  https://app.vagrantup.com/generic/boxes/centos8s/versions/4.3.12/providers/virtualbox/amd64/download/vagrant.box
Повторное использование соединения с app.vagrantup.com:443.
HTTP-запрос отправлен. Ожидание ответа… 302 Found
Адрес: https://archivist.vagrantup.com/v1/object/eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJrZXkiOiJib3hlcy8wM2JlM2JiNi0zMTMxLTQzOGYtYmZmOS0wYjEyYTZhMDAyZGIiLCJtb2RlIjoiciIsImV4cGlyZSI6MTcxMDAyMzQ4NX0.F9-iJWqND-fZNaDZdgl4kNv3huEZ1xUL-nmLl1ius2U [переход]
--2024-03-10 01:16:25--  https://archivist.vagrantup.com/v1/object/eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJrZXkiOiJib3hlcy8wM2JlM2JiNi0zMTMxLTQzOGYtYmZmOS0wYjEyYTZhMDAyZGIiLCJtb2RlIjoiciIsImV4cGlyZSI6MTcxMDAyMzQ4NX0.F9-iJWqND-fZNaDZdgl4kNv3huEZ1xUL-nmLl1ius2U
Распознаётся archivist.vagrantup.com (archivist.vagrantup.com)… 54.157.58.70, 52.204.242.176, 18.205.36.100, ...
Подключение к archivist.vagrantup.com (archivist.vagrantup.com)|54.157.58.70|:443... соединение установлено.
HTTP-запрос отправлен. Ожидание ответа… 307 Temporary Redirect
Адрес: https://vagrantcloud-files-production.s3-accelerate.amazonaws.com/archivist/boxes/03be3bb6-3131-438f-bff9-0b12a6a002db?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIA6NDPRW4B3M4V3Z62%2F20240309%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240309T221626Z&X-Amz-Expires=900&X-Amz-Security-Token=FwoGZXIvYXdzEBAaDBbgy%2BpVXlDFC02KECK3AddFfFnNn2NWoNDG0%2BDaQsBV31RU9Nu5Re0QJboZDZf1GDcE1gsw8YgF18Q7Yeu2Ixv3stivhQfDaWeCo%2BWlRcXAko5j4OcpQo7MJO7K2v3Crnr%2FUQ%2FG%2BvEdnln5MPOLYzg2tfIheWBoaOCIbPlcPkDvPcOuENTwVRGbLPBgPdDGgvP9ue5BcmpVzYH4fpklHRiZnwnhokUdv0XPQazc8LyEolgNFtb3vq5k4i0xA9ymk7rLF509DCjmubOvBjIt5jBgPMgKTqH%2BTSZYPwte%2B%2FfGQM9WFyVkQ8ExMqx%2FT47qdxhmX2RcNhVffLci&X-Amz-SignedHeaders=host&X-Amz-Signature=2c202734f0f4e568a43e4115cb1b74800f3f92943cff5cbbf40a771790b17853 [переход]
--2024-03-10 01:16:26--  https://vagrantcloud-files-production.s3-accelerate.amazonaws.com/archivist/boxes/03be3bb6-3131-438f-bff9-0b12a6a002db?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIA6NDPRW4B3M4V3Z62%2F20240309%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240309T221626Z&X-Amz-Expires=900&X-Amz-Security-Token=FwoGZXIvYXdzEBAaDBbgy%2BpVXlDFC02KECK3AddFfFnNn2NWoNDG0%2BDaQsBV31RU9Nu5Re0QJboZDZf1GDcE1gsw8YgF18Q7Yeu2Ixv3stivhQfDaWeCo%2BWlRcXAko5j4OcpQo7MJO7K2v3Crnr%2FUQ%2FG%2BvEdnln5MPOLYzg2tfIheWBoaOCIbPlcPkDvPcOuENTwVRGbLPBgPdDGgvP9ue5BcmpVzYH4fpklHRiZnwnhokUdv0XPQazc8LyEolgNFtb3vq5k4i0xA9ymk7rLF509DCjmubOvBjIt5jBgPMgKTqH%2BTSZYPwte%2B%2FfGQM9WFyVkQ8ExMqx%2FT47qdxhmX2RcNhVffLci&X-Amz-SignedHeaders=host&X-Amz-Signature=2c202734f0f4e568a43e4115cb1b74800f3f92943cff5cbbf40a771790b17853
Распознаётся vagrantcloud-files-production.s3-accelerate.amazonaws.com (vagrantcloud-files-production.s3-accelerate.amazonaws.com)… 52.85.48.12
Подключение к vagrantcloud-files-production.s3-accelerate.amazonaws.com (vagrantcloud-files-production.s3-accelerate.amazonaws.com)|52.85.48.12|:443... соединение установлено.
HTTP-запрос отправлен. Ожидание ответа… 200 OK
Длина: 1295213449 (1,2G) [binary/octet-stream]
Сохранение в: ‘vagrant.box’
vagrant.box                               100%[==================================================================================>]   1,21G  11,2MB/s    за 1m 53s  
2024-03-10 01:18:19 (11,0 MB/s) - ‘vagrant.box’ сохранён [1295213449/1295213449]`

Импортирую образ:

`vagrant box add —name 'generic/centos8s' vagrant.box`

Результат:

`==> box: Box file was not detected as metadata. Adding it directly...
==> box: Adding box 'generic/centos8s' (v0) for provider: 
    box: Unpacking necessary files from: file:///home/adminkonstantin/vagrantbox/centos8/vagrant.box
==> box: Successfully added box 'generic/centos8s' (v0) for ''!`

Создаю Vagrantfile:

`vagrant init generic/centos8s`

Следуя методичке по выполнению домашнего задания задаю конфигурацию будущей ВМ из шаблона vagrantfile в репозитории <https://github.com/Nickmob/vagrant_kernel_update>. Но в моём случае требуется корректировка шаблона. Так как я разворачиваю box, который загружен вручную и лежит локально на моём ПК, в моём vagrantfile необходимо удалить строку `:box_version => "4.3.4",` так как при её наличии vagrant пытается тянуть box из репозитория <https://vagrantcloud.com/generic/centos8s> и я, выполнив `vagrant up`, получаю ошибку:

`Bringing machine 'kernel-update' up with 'virtualbox' provider...
==> kernel-update: Box 'generic/centos8s' could not be found. Attempting to find and install...
    kernel-update: Box Provider: virtualbox
    kernel-update: Box Version: 4.3.4
The box 'generic/centos8s' could not be found or
could not be accessed in the remote catalog. If this is a private
box on HashiCorp's Vagrant Cloud, please verify you're logged in via 'vagrant login'. Also, please double-check the name. The expanded
URL and error message are shown below:
URL: ["https://vagrantcloud.com/generic/centos8s"]
Error: The requested URL returned error: 404`

После корректировки vagrantfile выполняю

`vagrant up`	

Результат:

`Bringing machine 'kernel-update' up with 'virtualbox' provider...
==> kernel-update: Importing base box 'generic/centos8s'...
==> kernel-update: Matching MAC address for NAT networking...
==> kernel-update: Setting the name of the VM: centos8_kernel-update_1710026221233_88785
==> kernel-update: Clearing any previously set network interfaces...
==> kernel-update: Preparing network interfaces based on configuration...
    kernel-update: Adapter 1: nat
==> kernel-update: Forwarding ports...
    kernel-update: 22 (guest) => 2222 (host) (adapter 1)
==> kernel-update: Running 'pre-boot' VM customizations...
==> kernel-update: Booting VM...
==> kernel-update: Waiting for machine to boot. This may take a few minutes...
    kernel-update: SSH address: 127.0.0.1:2222
    kernel-update: SSH username: vagrant
    kernel-update: SSH auth method: private key
    kernel-update: 
    kernel-update: Vagrant insecure key detected. Vagrant will automatically replace
    kernel-update: this with a newly generated keypair for better security.
    kernel-update: 
    kernel-update: Inserting generated public key within guest...
    kernel-update: Removing insecure key from the guest if it's present...
    kernel-update: Key inserted! Disconnecting and reconnecting using new SSH key...
==> kernel-update: Machine booted and ready!
==> kernel-update: Checking for guest additions in VM...
    kernel-update: The guest additions on this VM do not match the installed version of
    kernel-update: VirtualBox! In most cases this is fine, but in rare cases it can
    kernel-update: prevent things such as shared folders from working properly. If you see
    kernel-update: shared folder errors, please make sure the guest additions within the
    kernel-update: virtual machine match the version of VirtualBox you have installed on
    kernel-update: your host and reload your VM.
    kernel-update: 
    kernel-update: Guest Additions Version: 6.1.30 r148432
    kernel-update: VirtualBox Version: 7.0
==> kernel-update: Setting hostname…`

Проверяю статус ВМ:

`vagrant global-status`

Результат:

`id       name          provider   state   directory                                
                        286c067  kernel-update virtualbox running /home/adminkonstantin/vagrantbox/centos8 
 The above shows information about all known Vagrant environments
on this machine. This data is cached and may not be completely
up-to-date (use "vagrant global-status --prune" to prune invalid
entries). To interact with any of the machines, you can go to that
directory and run Vagrant, or you can use the ID directly with
Vagrant commands from any directory. For example:
"vagrant destroy 1a2b3c4d"`

### Обновление ядра ###

Подключаюсь к созданной ВМ:

`vagrant ssh`

Проверяю версию ядра ВМ:

`uname -r`

Результат:

`4.18.0-532.el8.x86_64`

Подключаю репозиторий для обновления ядра:

`sudo yum install -y https://www.elrepo.org/elrepo-release-8.el8.elrepo.noarch.rpm`

Результат:

`Failed to set locale, defaulting to C.UTF-8
CentOS Stream 8 - AppStream                                                                                                                                    4.7 MB/s |  28 MB     00:05    
CentOS Stream 8 - BaseOS                                                                                                                                       2.1 MB/s |  10 MB     00:04    
CentOS Stream 8 - Extras                                                                                                                                        43 kB/s |  18 kB     00:00    
CentOS Stream 8 - Extras common packages                                                                                                                        14 kB/s | 7.2 kB     00:00    
Extra Packages for Enterprise Linux 8 - x86_64                                                                                                                 5.5 MB/s |  16 MB     00:02    
Extra Packages for Enterprise Linux 8 - Next - x86_64                                                                                                          446 kB/s | 368 kB     00:00    
elrepo-release-8.el8.elrepo.noarch.rpm                                                                                                                         9.4 kB/s |  13 kB     00:01    
Dependencies resolved.
Package                                         Architecture                            Version                                           Repository                                     Size
Installing:
 elrepo-release                                  noarch                                  8.3-1.el8.elrepo                                  @commandline                                   13 k
Transaction Summary
Install  1 Package
Total size: 13 k
Installed size: 5.0 k
Downloading Packages:
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                                       1/1 
  Installing       : elrepo-release-8.3-1.el8.elrepo.noarch                                                                                                                                1/1 
  Verifying        : elrepo-release-8.3-1.el8.elrepo.noarch                                                                                                                                1/1 
Installed:
  elrepo-release-8.3-1.el8.elrepo.noarch                                                                                                                                                       
Complete!`

Устанавливаю последнюю версию ядра:

`sudo yum --enablerepo elrepo-kernel install kernel-ml -y`

Результат:

`Failed to set locale, defaulting to C.UTF-8
ELRepo.org Community Enterprise Linux Repository - el8                                                                                                         203 kB/s | 203 kB     00:00    
ELRepo.org Community Enterprise Linux Kernel Repository - el8                                                                                                  2.1 MB/s | 2.2 MB     00:01    
Dependencies resolved.
 Package                                          Architecture                          Version                                             Repository                                    Size
Installing:
 kernel-ml                                        x86_64                                6.7.9-1.el8.elrepo                                  elrepo-kernel                                121 k
Installing dependencies:
 kernel-ml-core                                   x86_64                                6.7.9-1.el8.elrepo                                  elrepo-kernel                                 39 M
 kernel-ml-modules                                x86_64                                6.7.9-1.el8.elrepo                                  elrepo-kernel                                 34 M
Transaction Summary
Install  3 Packages
Total download size: 73 M
Installed size: 115 M
Downloading Packages:
(1/3): kernel-ml-6.7.9-1.el8.elrepo.x86_64.rpm                                                                                                                 419 kB/s | 121 kB     00:00    
(2/3): kernel-ml-modules-6.7.9-1.el8.elrepo.x86_64.rpm                                                                                                         2.2 MB/s |  34 MB     00:15    
(3/3): kernel-ml-core-6.7.9-1.el8.elrepo.x86_64.rpm                                                                                                            2.2 MB/s |  39 MB     00:17    
Total                                                                                                                                                          4.1 MB/s |  73 MB     00:18     
ELRepo.org Community Enterprise Linux Kernel Repository - el8                                                                                                  1.6 MB/s | 1.7 kB     00:00    
Importing GPG key 0xBAADAE52:
 Userid     : "elrepo.org (RPM Signing Key for elrepo.org) <secure@elrepo.org>"
 Fingerprint: 96C0 104F 6315 4731 1E0B B1AE 309B C305 BAAD AE52
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-elrepo.org
Key imported successfully
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                                       1/1 
  Installing       : kernel-ml-core-6.7.9-1.el8.elrepo.x86_64                                                                                                                              1/3 
  Running scriptlet: kernel-ml-core-6.7.9-1.el8.elrepo.x86_64                                                                                                                              1/3 
  Installing       : kernel-ml-modules-6.7.9-1.el8.elrepo.x86_64                                                                                                                           2/3 
  Running scriptlet: kernel-ml-modules-6.7.9-1.el8.elrepo.x86_64                                                                                                                           2/3 
  Installing       : kernel-ml-6.7.9-1.el8.elrepo.x86_64                                                                                                                                   3/3 
  Running scriptlet: kernel-ml-core-6.7.9-1.el8.elrepo.x86_64                                                                                                                              3/3 
dracut: Disabling early microcode, because kernel does not support it. CONFIG_MICROCODE_[AMD|INTEL]!=y
dracut: Disabling early microcode, because kernel does not support it. CONFIG_MICROCODE_[AMD|INTEL]!=y
  Running scriptlet: kernel-ml-6.7.9-1.el8.elrepo.x86_64                                                                                                                                   3/3 
  Verifying        : kernel-ml-6.7.9-1.el8.elrepo.x86_64                                                                                                                                   1/3 
  Verifying        : kernel-ml-core-6.7.9-1.el8.elrepo.x86_64                                                                                                                              2/3 
  Verifying        : kernel-ml-modules-6.7.9-1.el8.elrepo.x86_64                                                                                                                           3/3 
Installed:
  kernel-ml-6.7.9-1.el8.elrepo.x86_64                        kernel-ml-core-6.7.9-1.el8.elrepo.x86_64                        kernel-ml-modules-6.7.9-1.el8.elrepo.x86_64                       
Complete!`

Перезагружаю ВМ: 

`sudo reboot`

Проверяю версию ядра:

`uname -r`

Результат:

`6.7.9-1.el8.elrepo.x86_64`

### Загрузка домашнего задания в репозиторий GitHub ###

Оформляю отчёт по домашнему заданию в текстовом редакторе с использованием языка разметки Markdown. Создаю в каталоге с vagrantfile файл README.md  и переношу сюда отчёт из текстового редактора. Файл README.md готов к загрузке в репозиторий. Добавляю в папку с файлом README.md и vagrantfile систему контроля версий git:

`git init`

Результат:

`подсказка: Using 'master' as the name for the initial branch. This default branch name
подсказка: is subject to change. To configure the initial branch name to use in all
подсказка: of your new repositories, which will suppress this warning, call:
подсказка: 
подсказка: 	git config --global init.defaultBranch <name>
подсказка: 
подсказка: Names commonly chosen instead of 'master' are 'main', 'trunk' and
подсказка: 'development'. The just-created branch can be renamed via this command:
подсказка: 
подсказка: 	git branch -m <name>
Инициализирован пустой репозиторий Git в /home/adminkonstantin/vagrantbox/centos8/.git/`

Добавляю файлы в коммит:

`git add README.md Vagrantfile`

Зафиксируем изменения:

`git commit -m «kernel-update»`

Результат:

`[master (корневой коммит) fb84208] «kernel-update»
 2 files changed, 23 insertions(+)
 create mode 100644 README.md
 create mode 100644 Vagrantfile`

Отправляю файлы в удаленный репозиторий GitHub:

`git push -u origin master`

Результат:

`Username for 'https://github.com': ConstantaNF
Password for 'https://ConstantaNF@github.com': 
Перечисление объектов: 4, готово.
Подсчет объектов: 100% (4/4), готово.
При сжатии изменений используется до 4 потоков
Сжатие объектов: 100% (3/3), готово.
Запись объектов: 100% (4/4), 570 байтов | 570.00 КиБ/с, готово.
Всего 4 (изменений 0), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
remote: 
remote: Create a pull request for 'master' on GitHub by visiting:
remote:      https://github.com/ConstantaNF/Otus/pull/new/master
remote: 
To https://github.com/ConstantaNF/Otus
[new branch]      master -> master Ветка «master» отслеживает внешнюю ветку «master» из «origin».`
