# infra
## Запуск
```console
docker-compose up setup
```
```console
docker-compose up --build -d
```
## Настройка доступа
Привидённые ниже команды сбрасывают стандартные пароли пользователей и устанавливает для них новые.
Пользователь `elastic` используется для входа в elasticsearch через kibana:
```sh
docker-compose exec elasticsearch bin/elasticsearch-reset-password --batch --user elastic
```
Пользователь `logstash_internal` используется для отправки логов из logstash в elasticsearch:
```sh
docker-compose exec elasticsearch bin/elasticsearch-reset-password --batch --user logstash_internal
```
Пользователь `kibana_system` используется для подключения kibana к elasticsearch:
```sh
docker-compose exec elasticsearch bin/elasticsearch-reset-password --batch --user kibana_system
```
После чего новый пароль нужно записать в файл `.env` и перезапустить сервис в котором поменяли пароль:
```sh
docker-compose up -d logstash kibana
```
## Запуск filebeat
Перед запуском нужно задать пароль для пользователей `FILEBEAT_INTERNAL` и `BEATS_SYSTEM` в файле `.env`.
Запуск filebeat:
```console
docker-compose -f docker-compose.yml -f extensions/filebeat/filebeat-compose.yml up
```
При изменениях в конфиге filebeat `config/filebeat.yml` нужно перезапустить сервис:
```console
docker-compose -f docker-compose.yml -f extensions/filebeat/filebeat-compose.yml restart filebeat
```

Для получения полее подробных инструкций:
https://github.com/deviantony/docker-elk/tree/main