Задание 1. Elasticsearch
Установите и запустите Elasticsearch, после чего поменяйте параметр cluster_name на случайный.

Приведите скриншот команды 'curl -X GET 'localhost:9200/_cluster/health?pretty', сделанной на сервере с установленным Elasticsearch. Где будет виден нестандартный cluster_name.

![1_1](https://github.com/slava1005/FOPS-13/assets/114395964/bef3acf3-0135-4098-b0bb-e358f206abc6)

Задание 2. Kibana
Установите и запустите Kibana.

Приведите скриншот интерфейса Kibana на странице http://<ip вашего сервера>:5601/app/dev_tools#/console, где будет выполнен запрос GET /_cluster/health?pretty.

![2](https://github.com/slava1005/FOPS-13/assets/114395964/8666a3fb-8898-4321-abb7-37bd05ab9d67)

Задание 3. Logstash
Установите и запустите Logstash и Nginx. С помощью Logstash отправьте access-лог Nginx в Elasticsearch.

![3](https://github.com/slava1005/FOPS-13/assets/114395964/5ce29fd2-ed82-4f5b-8a69-977d301aa8ae)

Содержимое файла /etc/elasticsearch/elasticsearch.yml
```
cluster.name: netology-savilov
network.host: localhost
http.port: 9200
```
Содержимое файлов /etc/logstash/conf.d/input.conf ...filter.conf, ...output.conf

```
input {
  file {
    path => "/var/log/nginx/access.log"
    start_position => "beginning"
    type => "nginx"
   }
 }
```
 ```
 filter {
    grok {
        match => { "message" => "%{IPORHOST:remote_ip} - %{DATA:user_name} \[%{HTTPDATE:access_time}\] \"%{WORD:h$    }
    mutate {
        remove_field => [ "host" ]
    }
}
```
```
output {
    elasticsearch {
        hosts => "127.0.0.1"
        index => "nginx-%{+YYYY.MM.dd}"
    }
}
```

Приведите скриншот интерфейса Kibana, на котором видны логи Nginx.

Задание 4. Filebeat.
Установите и запустите Filebeat. Переключите поставку логов Nginx с Logstash на Filebeat.

![4](https://github.com/slava1005/FOPS-13/assets/114395964/e8155c6d-1bb5-4f57-987b-cae6a341ca09)

Приведите скриншот интерфейса Kibana, на котором видны логи Nginx, которые были отправлены через Filebeat.
