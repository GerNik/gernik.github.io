**Vagrant** - инструмент для конфигурирования и управления виртуальными машинами с открытым исходным кодом. 

#### Установка vagrant

Vagrant требует установки VirtualBox. Скачать можно [отсюда] (https://www.virtualbox.org)
или установить с помощью менеджера пакетов   
```bash
$ brew cask install virtualbox
```
А затем и сам [vagrant] (https://www.vagrantup.com/)
```bash
$ brew cask install vagrant
```
Проверка, что vagrant установился и работает
```
$ vagrant -v
Vagrant 2.1.2
```
#### Инициализация
Необходимо перейти в нужную директорию и выполнить команду 
```bash
$ vagrant init ubuntu/trusty64
```
В результате выполнения этой команды Vagrant создаст файл Vagrantfile.
Внутри файла описана конфигурация виртуальной машины на языке Ruby. Не страшно, 
если вы не знакомы с ним, содержимое файла интуитивно понятно.

Кроме создания файла, вагрант скачает на вашу машину образ (image) операционной системы, 
хранящийся под именем ubuntu/trusty64. В данном случае речь идет про Ubuntu Server 14.04. Vagrant 
поддерживает каталог образов, созданных специально под работу с ним. При необходимости 
можно выбрать другой, посмотрев его имя в [каталоге.] (https://app.vagrantup.com/boxes/search) Если открыть файл Vagrantfile, то
 можно увидеть что имя образа прописано внутри конфигурации:
 ```ruby
VAGRANT_FILE_API_VERSION = "2"
Vagrant.configure(VAGRANT_FILE_API_VERSION) do |config|
    config.vm.box = "ubuntu/trusty64"
end
```
#### Запуск
Старт выполняется командой `vagrant up`. Она создает виртуальную машину и запускает ее. 
Если машина уже была создана (предыдущими запусками), то произойдет только старт.

#### Подключение по ssh
После запуска можно попробовать подключиться по ssh с помощью команды `vagrant ssh`. 
Узнать детали ssh подключения можно с помощью `vagrant ssh-config`
```bash
$ vagrant ssh-config
Host default
  HostName 127.0.0.1
  User vagrant
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile /Users/gernik/develop/ansible/playbooks/.vagrant/machines/default/virtualbox/private_key
  IdentitiesOnly yes
  LogLevel FATAL
```
Также можно подключиться, используя утилиту ssh и приватный ключ, хранящийся в файле
```bash
$ ssh vagrant@127.0.0.1 -p 2222 -i /Users/gernik/develop/ansible/playbooks/.vagrant/machines/default/virtualbox/private_key
```
Дальше можно действовать по старинке. Поставить все необходимые пакеты и зависимости, а затем запустить проект. 
Если это веб-сайт, то он запустится на определенном порту. По умолчанию все, что стартует внутри виртуальной 
машины, доступно только внутри. Для возможности обращаться к сайту с хост машины (через ваш любимый 
браузер), необходимо прокинуть соответствующий порт наружу.   

 ```ruby
VAGRANT_FILE_API_VERSION = "2"
Vagrant.configure(VAGRANT_FILE_API_VERSION) do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.network "forwarded_port", guest: 80,  host: 8080
    config.vm.network "forwarded_port", guest: 443,  host: 8443
end
```
Предположим, что внутри виртуальной машины сайт стартует на 80 порту, снаружи он будет доступен 
на порту 8080.  
Изменения применяются после перезагрузки машины. Для этого достаточно выполнить команду `vagrant reload`.

#### Остановка
В конце работы не забудьте выполнить `vagrant halt`, иначе машина останется запущенной и будет потреблять 
ресурсы. Если вы решили, что пора полностью все удалить и поставить заново, воспользуйтесь командой 
`vagrant destroy`.