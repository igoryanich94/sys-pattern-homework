# Домашнее задание к занятию "Docker часть 2" - Светиков Игорь Андреевич


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

Docker-Compose необходим для одновременного возведения и управления контейнерами и помогает
сэкономить очень много времени и сил

---

### Задание 2

# файл compose.yml

```
Поле для вставки кода...
services:

volumes:

networks:

volumes:


---

### Задание 3

# файл prometheusonly.yml - YAML файл
# ./prometheus/prometheus.yml - конфиг прометеуса
```
Поле для вставки кода...
#prometheusonly.yml

services:
  prometheus:
    image: prom/prometheus:v2.47.2
    container_name: svetikovia-netology-prometheus
    command: --web.enable-lifecycle --config.file=/etc/prometheus/prometheus.yml
    ports:
      - 9090:9090
    volumes:
      - ./prometheus:/etc/prometheus
      - prometheus-data:/prometheus
    networks:
      - monitoring-stack
    restart: always

volumes:
  prometheus-data:

networks:
  monitoring-stack:
     driver: bridge
     ipam:
       config:
       - subnet: 10.5.0.0/16
         gateway: 10.5.0.1


#prometheus.yml

# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
#             - alertmanager:9093
#  rule_files:
#    - alertmanager.yml
# alertls.yml
#  groups:
#    - name: Netology
#      rules:
#        - alert: Danger
#          expr: docker{job="netology"} > 1
#          for: 10s
# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:

  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  # - job_name: "docker-server"
    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.
  #   static_configs:
  #     - targets: ["172.17.0.1:9100"]

  - job_name: 'pushgateway'
    honor_labels: true
    static_configs:
      - targets: ["pushgateway:9091"]




```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`

### Задание 4

# файл pushgateway.yml - YAML файл

```
Поле для вставки кода...
#pushgateway.yml

services:
  prometheus:
    image: prom/prometheus:v2.47.2
    container_name: svetikovia-netilogy-prometheus
    command: --web.enable-lifecycle --config.file=/etc/prometheus/prometheus.yml
    ports:
      - 9090:9090
    volumes:
      - ./prometheus:/etc/prometheus
      - prometheus-data:/prometheus
    networks:
      - monitoring-stack
    restart: always


  pushgateway:
    image: prom/pushgateway:v1.6.2
    container_name: svetikovia-netology-pushgateway
    ports:
      - 9091:9091
    networks:
      - monitoring-stack
    depends_on:
      - prometheus
    restart: unless-stopped

volumes:
  prometheus-data:

networks:
  monitoring-stack:
     driver: bridge
     ipam:
       config:
       - subnet: 10.5.0.0/16
         gateway: 10.5.0.1


```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`


### Задание 5

# К сожалению на 80 порту засел апач и я не смог его победить и прекинул графану на 81 порт(
# файл grafana.yml - YAML файл
# файл ./grafana/custom.ini - файл инициализации (лог/пас)


#grafana.yml

services:
  prometheus:
    image: prom/prometheus:v2.47.2
    container_name: svetikovia-netilogy-prometheus
    command: --web.enable-lifecycle --config.file=/etc/prometheus/prometheus.yml
    ports:
      - 9090:9090
    volumes:
      - ./prometheus:/etc/prometheus
      - prometheus-data:/prometheus
    networks:
      - monitoring-stack
    restart: always


  pushgateway:
    image: prom/pushgateway:v1.6.2
    container_name: svetikovia-netology-pushgateway
    ports:
      - 9091:9091
    networks:
      - monitoring-stack
    depends_on:
      - prometheus
    restart: unless-stopped

  grafana:
    image: grafana/grafana
    container_name: svetikovia-netology-grafana
    environment:
      GF_PATHS_CONFIG: /etc/grafana/custom.ini
    ports:
      - 81:3000
    volumes:
      - ./grafana:/etc/grafana
      - grafana-data:/var/lib/grafana
    networks:
      - monitoring-stack
    depends_on:
      - prometheus
    restart: unless-stopped


volumes:
  prometheus-data:
  grafana-data:

networks:
  monitoring-stack:
     driver: bridge
     ipam:
       config:
       - subnet: 10.5.0.0/16
         gateway: 10.5.0.1


#./grafana/custom.ini

[security]

admin_user = netology
admin_password = netology


## Задание 6 и 7

# файл grafana.yml - YAML файл (Здесь поочередность и режимы запуска настроены, а также 
# настроено использование одной сети)



# detached режим docker-compose -f grafana.yml up -d

# Скриншот №1
![alt text](https://github.com/igoryanich94/sys-pattern-homework/tree/main/img/screen1.png)
# Скриншот №2
![alt text](https://github.com/igoryanich94/sys-pattern-homework/tree/main/img/screen2.png)


## Задание 8

docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)




