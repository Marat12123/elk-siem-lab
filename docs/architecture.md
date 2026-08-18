markdown
# Архитектура стенда

## Топология сети
Все три виртуальные машины находятся в подсети `192.168.56.0/24` (Host-Only):

| ВМ | Роль | IP-адрес |
|----|------|----------|
| ВМ1 | SIEM (ELK) | 192.168.56.10 |
| ВМ2 | WAF + Веб-приложение | 192.168.56.11 |
| ВМ3 | Linux Audit | 192.168.56.12 |

Для доступа в интернет у каждой ВМ есть второй адаптер – NAT.

## Потоки логов
ВМ2 (WAF) → Filebeat → Logstash (5044) → Elasticsearch → Kibana
ВМ3 (Audit) → rsyslog → Logstash (514) → Elasticsearch → Kibana

text

## Компоненты ВМ1
| Компонент | Назначение | Порт |
|-----------|------------|------|
| Elasticsearch | Хранение и индексация | 9200 |
| Logstash | Приём, нормализация, передача | 514 (TCP/UDP), 5044 (Beats) |
| Kibana | Визуализация | 5601 |

## Нормализация
В Logstash используется фильтр `grok` для парсинга syslog-сообщений:
%{SYSLOGTIMESTAMP:syslog_timestamp} %{SYSLOGHOST:host} %{DATA:program}(?:
)?: %{GREEDYDATA:syslog_message}

text
Извлекаются поля: `@timestamp`, `host`, `program`, `pid`, `syslog_message`.

## Алертинг
ElastAlert2: при 5 неудачных SSH-попытках за 10 минут отправляется email-оповещение.
