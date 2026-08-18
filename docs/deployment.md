markdown
# Алгоритм развёртывания ELK

## Предварительные требования
- ВМ1 с Kali Linux / Ubuntu Server, 4 ГБ ОЗУ.
- Docker и Docker Compose установлены.
- Статический IP: `192.168.56.10`.

## Шаг 1. Установка Docker
```bash
sudo apt update
sudo apt install docker.io docker-compose -y
sudo usermod -aG docker $USER
newgrp docker
Шаг 2. Создание структуры
bash
mkdir -p ~/elk-stack/logstash/pipeline
cd ~/elk-stack
Шаг 3. Конфигурационные файлы
docker-compose.yml – см. /config

logstash.conf – см. /config

Шаг 4. Запуск
bash
docker-compose up -d
docker-compose ps
Шаг 5. Проверка
Elasticsearch: curl http://localhost:9200

Kibana: http://192.168.56.10:5601

Шаг 6. Тестовое сообщение
bash
docker exec -it logstash bash
echo "<14>Aug 15 12:00:00 test-host test-program[123]: Test from ELK" > /dev/tcp/localhost/514
exit
Шаг 7. Создание Data View в Kibana
Management → Stack Management → Data Views → Create data view

Имя: logs-*, поле времени: @timestamp

Шаг 8. Проверка в Discover
Выбрать Data View, установить Today, нажать Refresh.
