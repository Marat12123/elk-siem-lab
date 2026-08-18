# Проект 1: SIEM на базе ELK Stack

## Краткое описание
Развёрнут централизованный SIEM-стенд для сбора, нормализации, хранения и визуализации логов безопасности. Стек ELK (Elasticsearch, Logstash, Kibana) работает в Docker-контейнерах на ВМ1 с фиксированным IP `192.168.56.10`. Kibana доступна по адресу `http://192.168.56.10:5601`.

## Технические детали
- **Elasticsearch 8.11.1** – хранение и индексация логов.
- **Logstash 8.11.1** – приём логов по TCP/UDP (порт 514) и от Beats (порт 5044), нормализация через grok.
- **Kibana 8.11.1** – визуализация и анализ.
- **ElastAlert2** – алертинг (правило на 5 неудачных SSH-попыток за 10 минут).

## Архитектура
Топология и потоки логов описаны в `/docs/architecture.md`.

## Результаты
- ✅ Логи с ВМ2 (WAF) и ВМ3 (auditd) успешно принимаются и нормализуются.
- ✅ Данные отображаются в Kibana.
- ✅ Настроено оповещение о подозрительной активности.

## Структура репозитория
elk-siem-lab/
├── README.md
├── docs/
│ ├── architecture.md
│ └── deployment.md
├── config/
│ ├── docker-compose.yml
│ ├── logstash.conf
│ └── elastalert_rule.yaml
├── report/
│ └── REPORT.md
└── screenshots/
├── 01_elk_containers.png
├── 02_kibana_test_message.png
├── 03_kibana_waf_events.png
└── 04_kibana_audit_events.png
