# ansible-test
Приветсвую, будущие коллеги!
Здесь происходит следующее:

1. Устанавливается Ansible на облачный сервер с CentOS и еще один сервер для экспериментов.
В group_vars/servers указывается, что пользователь, выполняющий на целевой системе - root.

2. Создается файл hosts, проект маленький сделал только одну группу из одного хоста
3. По условию скопировал наработки из Galaxy:

ansible-galaxy install -r requirements.yml

4. bertvv.mariadb и bertvv.httpd конфигурируется, как в соответсвующих дирекориях в папке roles.
В роли bertvv.mariadb создается база для wordpress и пользователь работающий с базой CMS.
5. Создаётся дополнительная роль для получения нужной версии php (php_depend):

cd /etc/ansible/roles
ansible-galaxy init php_depend

Роль добавляет дополнительные репозитории и указывает на желаемую версию php 5.6, затем устанавливается php и php-mysql

6. Создаются роли для установки wordpress:

ansible-galaxy init wp-dependencies
ansible-galaxy init wp-install-config

- wp-dependencies хранит в себе данные для подключения (остался исторически от примера, что я брал за основу)
- wp-install-config создает папку с исходниками под wordpress , тянет по url последнюю версию 
wordpress, копирует в /var/www/html файлы CMS-ки, копирует заранее приготовленный конфиг, настраивает права доступа 
 и затем ждет 20 секунд и перезагружает сервер, чтобы убедиться, что он работает и после перезагрузки не перестанет работать.
 В конце один fail, это из-за перезагрузки сервера.

7. Для запуска всех ролей последовательно необходимо запустить playbook_total.yaml:

ansible-playbook -i hosts playbook_total.yaml

8. Заходим по адресу в hosts: http://142.93.226.6/ и видим пустой сайт на wordpress. 
Первого пользователя Wordpress я настроил сам, больше там ничего интересного нет.
