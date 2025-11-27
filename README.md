# Ansible Role: `vector-role`

[![GitHub tag](https://img.shields.io/github/v/tag/Dmitriy-py/vector-role?sort=semver&color=blue)](https://github.com/Dmitriy-py/vector-role/releases)

Устанавливает и конфигурирует **Vector** (современный, высокопроизводительный сборщик логов и метрик). Роль настраивает Vector для сбора системных метрик хоста и их последующей пересылки в указанный приемник (Sink), который по умолчанию настроен на ClickHouse через HTTP API.

## Поддерживаемые операционные системы

*   Debian (Buster, Bullseye)
*   Ubuntu (Focal, Jammy)

---

## Переменные Роли (Role Variables)

Настройка роли осуществляется через следующие переменные, которые должны быть определены в `vars` или `defaults/main.yml`.

| Имя Переменной | Тип | Значение по умолчанию | Описание |
| :--- | :--- | :--- | :--- |
| `vector_service_state` | String | `started` | Желаемое состояние сервиса Vector после применения роли. Используется в модуле `ansible.builtin.service`. Допустимые значения: `started`, `stopped`, `reloaded`, `restarted`. |
| `vector_sink_host` | String | `127.0.0.1` | **IP-адрес или FQDN сервера ClickHouse**. Это адрес, куда Vector будет отправлять собранные метрики. Используется в шаблоне `vector.toml.j2`. |
| `vector_sink_port` | Integer | `8123` | **HTTP порт ClickHouse**. Vector использует этот порт для взаимодействия с ClickHouse HTTP API при вставке данных. |

---

## Пример использования (Example Usage)

Пример использования в главном плейбуке (`site.yml`), где мы переопределяем значения по умолчанию, чтобы указать, куда отправлять данные:

```yaml
---
- name: Setup Vector collector
  hosts: my_application_servers
  become: yes
  
  roles:
    - role: vector
      vector_sink_host: "10.10.1.100" 
      vector_sink_port: 8123
```