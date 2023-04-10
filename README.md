# Баланс и гармония rrobin, Nginx и Ansible

## Что собственно происходит? 
nginx.yml - это YAML-сценарий для инструмента управления конфигурацией **Ansible**. В нем определены три задачи, которые нужно выполнить на разных компьютерах:
- Отключить SELinux на всех компьютерах с помощью роли selina.
- Установить пакет nginx на компьютерах с тегом web_servers с помощью роли customer.
- Установить nginx на компьютерах с тегом rrobin с помощью роли balancer.

Параметр become: true используется для выполнения этих задач с привилегиями суперпользователя (обычно root) на целевых компьютерах.

## Файл ansible.cfg
Это конфигурационный файл, который содержит несколько параметров для управления настройками работы с удаленными компьютерами.

Например, параметр "inventory" указывает на файл, в котором хранится список удаленных компьютеров, с которыми можно работать. Параметр "remote_user" указывает имя пользователя, которое будет использоваться для доступа к удаленным компьютерам.

Параметр ```"host_key_checking"``` отключает проверку ключей хостов при подключении к удаленным компьютерам, что может быть полезно для автоматического подключения без ввода паролей.

Параметр ```"transport"``` указывает на метод передачи данных между компьютерами, который будет использоваться по умолчанию.

## Файл inventory
Это файл, который содержит список серверов, которые могут быть управляемыми с помощью Ansible.

Секция [web_servers] содержит два сервера, названных web1 и web2, и каждый из них имеет свой IP-адрес **192.168.11.111** и **192.168.11.112** соответственно.

Секция [rrobin] содержит еще один сервер с названием rrobin и IP-адресом 192.168.11.113.

Таким образом, при использовании Ansible можно указать эти названия серверов в своих плейбуках и управлять ими с помощью Ansible. Например, плейбук может содержать инструкции для настройки веб-серверов web1 и web2, а также балансировщика нагрузки rrobin.

## Алгоритм работы:
1. Начните с того, чтобы добавить в свой инвентаризационный файл (inventory) IP-адреса ваших хостов и назначить им удобные имена.
2. Для удобства просмотра веб-страниц и обнаружения изменений на них, вам нужно изменить шаблон HTML-страницы в файле "index.html.j2" в папке "roles/templates/". Это поможет вам легко определять, какие хосты работают и какие страницы были изменены.
3. Затем запустите ваш playbook (nginx.yml), который выполнит все задачи (tasks) всех ролей, которые вы указали в playbook.
4. Чтобы проверить результат работы, введите в строку браузера IP-адрес балансировщика (load balancer), а затем добавьте порт 8080. Вы увидите свои измененные страницы, распределенные между вашими хостами.
![1_KjVC-dpwxQUokewQ18YX0A](https://user-images.githubusercontent.com/113093880/231010898-9e74739e-8300-469c-823a-573d2646d90a.png)

![230712777-ade0751d-e127-460b-b475-2e763ec9b573](https://user-images.githubusercontent.com/113093880/231010467-cd3797e1-2746-4ee6-b94e-967153d2264e.png) 
