## Домашнее задание к занятию "08.01 Введение в Ansible"
### Подготовка к выполнению
Установите ansible версии 2.10 или выше. ``` $ ansible --version ansible 
[core 2.12.10] ``` Создайте свой собственный публичный репозиторий на 
github с произвольным именем. 
https://github.com/Bora2k3/08-ansible-01-base Скачайте playbook из 
репозитория с домашним заданием и перенесите его в свой репозиторий.
### Основная часть
Попробуйте запустить playbook на окружении из test.yml, зафиксируйте 
какое значение имеет факт some_fact для указанного хоста при выполнении 
playbook'a. ``` root@debian:~/08-ansible-01# ansible-playbook -i 
inventory/test.yml site.yml PLAY [Print os facts] 
********************************************************** TASK 
[Gathering Facts] 
********************************************************* ok: 
[localhost] TASK [Print OS] 
**************************************************************** ok: 
[localhost] => {
    "msg": "Debian"
}
TASK [Print fact] 
************************************************************** ok: 
[localhost] => {
    "msg": 12
}
PLAY RECAP 
********************************************************************* 
localhost : ok=3 changed=0 unreachable=0 failed=0 s kipped=0 rescued=0 
ignored=0 root@debian:~/08-ansible-01# ``` Найдите файл с переменными 
(group_vars) в котором задаётся найденное в первом пункте значение и 
поменяйте его на 'all default fact'.
% cat group_vars/all/examp.yml
``` --- some_fact: all default fact ``` Воспользуйтесь подготовленным 
(используется docker) или создайте собственное окружение для проведения 
дальнейших испытаний. docker-compose.yml ``` version: '3' services:
  centos7: image: pycontribs/centos:7 container_name: centos7 restart: 
    unless-stopped entrypoint: "sleep infinity"
  ubuntu: image: pycontribs/ubuntu container_name: ubuntu restart: 
    unless-stopped entrypoint: "sleep infinity"
``` Проведите запуск playbook на окружении из prod.yml. Зафиксируйте 
полученные значения some_fact для каждого из managed host. ``` 
root@debian:~/08-ansible-01# ansible-playbook -i inventory/prod.yml -v 
site.yml Using /etc/ansible/ansible.cfg as config file PLAY [Print os 
facts] ********************************************************** TASK 
[Gathering Facts] 
********************************************************* ok: [ubuntu] 
ok: [centos7] TASK [Print OS] 
**************************************************************** ok: 
[centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => { "msg": "Ubuntu"
}
TASK [Print fact] 
************************************************************** ok: 
[centos7] => {
    "msg": "el"
}
ok: [ubuntu] => { "msg": "deb"
}
PLAY RECAP 
********************************************************************* 
centos7 : ok=3 changed=0 unreachable=0 failed=0 skipped=0 rescued=0 
ignored=0 ubuntu : ok=3 changed=0 unreachable=0 failed=0 skipped=0 
rescued=0 ignored=0 root@debian:~/08-ansible-01# ``` Добавьте факты в 
group_vars каждой из групп хостов так, чтобы для some_fact получились 
следующие значения: для deb - 'deb default fact', для el - 'el default 
fact'. ``` $ cat group_vars/deb/examp.yml ;echo "" ---
  some_fact: "deb default fact" $ cat group_vars/el/examp.yml ;echo "" 
---
  some_fact: "el default fact" ``` Повторите запуск playbook на 
окружении prod.yml. Убедитесь, что выдаются корректные значения для всех 
хостов. ``` root@debian:~/08-ansible-01# ansible-playbook -i 
inventory/prod.yml -v site.yml Using /etc/ansible/ansible.cfg as config 
file PLAY [Print os facts] 
******************************************************************************************************************************************************************************************************** 
TASK [Gathering Facts] 
******************************************************************************************************************************************************************************************************* 
ok: [ubuntu] ok: [centos7] TASK [Print OS] 
************************************************************************************************************************************************************************************************************** 
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => { "msg": "Ubuntu"
}
TASK [Print fact] 
************************************************************************************************************************************************************************************************************ 
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => { "msg": "deb default fact"
}
PLAY RECAP 
******************************************************************************************************************************************************************************************************************* 
centos7 : ok=3 changed=0 unreachable=0 failed=0 skipped=0 rescued=0 
ignored=0 ubuntu : ok=3 changed=0 unreachable=0 failed=0 skipped=0 
rescued=0 ignored=0 root@debian:~/08-ansible-01# ``` При помощи 
ansible-vault зашифруйте факты в group_vars/deb и group_vars/el с 
паролем netology. ``` root@debian:~/08-ansible-01# ansible-vault encrypt 
group_vars/deb/examp.yml New Vault password: Confirm New Vault password: 
Encryption successful root@debian:~/08-ansible-01# ansible-vault encrypt 
group_vars/el/examp.yml New Vault password: Confirm New Vault password: 
Encryption successful ``` Запустите playbook на окружении prod.yml. При 
запуске ansible должен запросить у вас пароль. Убедитесь в 
работоспособности. ``` root@debian:~/08-ansible-01# ansible-playbook -i 
inventory/prod.yml site.yml PLAY [Print os facts] 
******************************************************************************************************************************************************************************************************** 
ERROR! Attempting to decrypt but no vault secrets found 
root@debian:~/08-ansible-01# ansible-playbook -i inventory/prod.yml 
site.yml --ask-vault-pass Vault password: PLAY [Print os facts] 
******************************************************************************************************************************************************************************************************** 
TASK [Gathering Facts] 
******************************************************************************************************************************************************************************************************* 
ok: [ubuntu] ok: [centos7] TASK [Print OS] 
************************************************************************************************************************************************************************************************************** 
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => { "msg": "Ubuntu"
}
TASK [Print fact] 
************************************************************************************************************************************************************************************************************ 
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => { "msg": "deb default fact"
}
PLAY RECAP 
******************************************************************************************************************************************************************************************************************* 
centos7 : ok=3 changed=0 unreachable=0 failed=0 skipped=0 rescued=0 
ignored=0 ubuntu : ok=3 changed=0 unreachable=0 failed=0 skipped=0 
rescued=0 ignored=0 root@debian:~/08-ansible-01# ``` Посмотрите при 
помощи ansible-doc список плагинов для подключения. Выберите подходящий 
для работы на control node. нужен "local" В prod.yml добавьте новую 
группу хостов с именем local, в ней разместите localhost с необходимым 
типом подключения. ``` root@debian:~/08-ansible-01# cat 
inventory/prod.yml ; echo "" ---
  el: hosts: centos7: ansible_connection: docker deb: hosts: ubuntu: 
        ansible_connection: docker
  local: hosts: localhost: ansible_connection: local 
root@debian:~/08-ansible-01# ``` Запустите playbook на окружении 
prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь 
что факты some_fact для каждого из хостов определены из верных 
group_vars. без создания отдельного group_vars -> получил для local из 
all ``` root@debian:~/08-ansible-01# ansible-playbook -i 
inventory/prod.yml site.yml --ask-vault-pass Vault password: PLAY [Print 
os facts] 
******************************************************************************************************************************************************************************************************** 
TASK [Gathering Facts] 
******************************************************************************************************************************************************************************************************* 
ok: [localhost] ok: [ubuntu] ok: [centos7] TASK [Print OS] 
************************************************************************************************************************************************************************************************************** 
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => { "msg": "Ubuntu"
}
ok: [localhost] => { "msg": "Ubuntu"
}
TASK [Print fact] 
************************************************************************************************************************************************************************************************************ 
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => { "msg": "deb default fact"
}
ok: [localhost] => { "msg": "all default fact"
}
PLAY RECAP 
******************************************************************************************************************************************************************************************************************* 
centos7 : ok=3 changed=0 unreachable=0 failed=0 skipped=0 rescued=0 
ignored=0 localhost : ok=3 changed=0 unreachable=0 failed=0 skipped=0 
rescued=0 ignored=0 ubuntu : ok=3 changed=0 unreachable=0 failed=0 
skipped=0 rescued=0 ignored=0 ``` создаем отдельный group-vars для local 
-> получаем для local из local ``` root@debian:~/08-ansible-01# 
ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass Vault 
password: PLAY [Print os facts] 
******************************************************************************************************************************************************************************************************** 
TASK [Gathering Facts] 
******************************************************************************************************************************************************************************************************* 
ok: [localhost] ok: [ubuntu] ok: [centos7] TASK [Print OS] 
************************************************************************************************************************************************************************************************************** 
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => { "msg": "Ubuntu"
}
ok: [localhost] => { "msg": "Ubuntu"
}
TASK [Print fact] 
************************************************************************************************************************************************************************************************************ 
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => { "msg": "deb default fact"
}
ok: [localhost] => { "msg": "local default fact"
}
PLAY RECAP 
******************************************************************************************************************************************************************************************************************* 
centos7 : ok=3 changed=0 unreachable=0 failed=0 skipped=0 rescued=0 
ignored=0 localhost : ok=3 changed=0 unreachable=0 failed=0 skipped=0 
rescued=0 ignored=0 ubuntu : ok=3 changed=0 unreachable=0 failed=0 
skipped=0 rescued=0 ignored=0 ```
Заполните README.md ответами на вопросы. Сделайте git push в ветку master. В ответе отправьте ссылку на ваш открытый репозиторий с изменённым playbook и заполненным README.md.
