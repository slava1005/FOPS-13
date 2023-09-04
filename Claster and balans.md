# Домашнее задание к занятию 2 
«Кластеризация и балансировка нагрузки»

## Задание 1
Запустите два simple python сервера на своей виртуальной машине на разных портах
![сервер 1 8888](https://github.com/slava1005/FOPS-13/assets/114395964/3d84a03f-d0ed-4e92-a4d9-6edfdf2288dd)
![сервер 2 9999](https://github.com/slava1005/FOPS-13/assets/114395964/15b14ea5-f73b-4ca6-8003-f6264448db13)


Установите и настройте HAProxy, воспользуйтесь материалами к лекции по ссылке
Настройте балансировку Round-robin на 4 уровне.
На проверку направьте конфигурационный файл haproxy,  https://github.com/slava1005/FOPS-13/blob/main/haproxy.cfg
![вывод  haproxy](https://github.com/slava1005/FOPS-13/assets/114395964/5cb21e43-968c-4cf5-873b-08655d6a3915)


скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy.
![статистика haproxy 1 задание](https://github.com/slava1005/FOPS-13/assets/114395964/787702fe-e3ca-440e-af81-19e859af6caf)


## Задание 2
Запустите три simple python сервера на своей виртуальной машине на разных портах
![сервер 1 8888](https://github.com/slava1005/FOPS-13/assets/114395964/228534aa-53d5-4777-8742-390e8b3225fe)
![сервер 2 9999](https://github.com/slava1005/FOPS-13/assets/114395964/68934dae-9454-42d5-a567-4dd179ae9b19)
![сервер 3 7777](https://github.com/slava1005/FOPS-13/assets/114395964/243b4510-e6b0-48bf-8dce-f2fbef83e024)

Настройте балансировку Weighted Round Robin на 7 уровне, чтобы первый сервер имел вес 2, второй - 3, а третий - 4
HAproxy должен балансировать только тот http-трафик, который адресован домену example.local
На проверку направьте конфигурационный файл haproxy, https://github.com/slava1005/FOPS-13/blob/main/haproxy_2.cfg
скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy c использованием домена example.local и без него.
![Задание 2 вывод команды curl](https://github.com/slava1005/FOPS-13/assets/114395964/23a22407-75bc-46dc-b108-6bb72fbcc51c)
![статистика haproxy задание 2](https://github.com/slava1005/FOPS-13/assets/114395964/13b9623f-e093-4097-87d5-5240a4ebc4d8)


