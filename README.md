# Ansible Role: `vector-role`

[![GitHub tag](https://img.shields.io/github/v/tag/Dmitriy-py/vector-role?sort=semver&color=blue)](https://github.com/Dmitriy-py/vector-role/releases)

Устанавливает и конфигурирует **Vector** (современный, высокопроизводительный сборщик логов и метрик). Роль настраивает Vector для сбора системных метрик хоста и их последующей пересылки в указанный приемник (Sink), который по умолчанию настроен на ClickHouse через HTTP API.

**Внимание:** Роль использует метод установки через скачивание и распаковку бинарного файла (tarball).

## Поддерживаемые операционные системы

*   Debian (Buster, Bullseye)
*   Ubuntu (Focal, Jammy, и новее)

---

## Переменные Роли (Role Variables)

Настройка роли осуществляется через следующие переменные, которые должны быть определены в `vars` или `defaults/main.yml`.

| Имя Переменной | Тип | Значение по умолчанию | Описание |
| :--- | :--- | :--- | :--- |
| `vector_version` | String | `"0.37.1"` | **Версия Vector**, которую нужно установить. Используется для формирования URL скачивания бинарного файла. |
| `vector_path` | String | `/opt/vector` | **Базовый каталог установки Vector.** Сюда скачивается и распаковывается архив с исполняемыми файлами. |
| `vector_data_path` | String | `/var/lib/vector` | Каталог для логов, данных и кэша Vector. |
| `vector_config` | String | `/etc/vector/vector.toml` | Полный путь к основному файлу конфигурации. |
| `vector_service_state` | String | `started` | Желаемое состояние сервиса Vector после применения роли. Допустимые значения: `started`, `stopped`, `restarted`. |
| `vector_sink_host` | String | `127.0.0.1` | **IP-адрес или FQDN сервера ClickHouse** (или другого приемника). |
| `vector_sink_port` | Integer | `8123` | **HTTP порт ClickHouse** (или другого приемника). |

---

## Пример использования (Example Usage)

Пример использования в главном плейбуке (`site.yml`), где мы переопределяем значения по умолчанию, чтобы указать, куда отправлять данные:

```yaml
---
- name: Setup Vector collector
  hosts: my_application_servers
  become: yes
  
  roles:
    - role: vector-role
      # Обязательно укажите версию для установки
      vector_version: "0.38.0" 
      vector_sink_host: "10.10.1.100" 
      vector_sink_port: 8123
Лицензия
MIT

Автор
`" Дмитрий Климов “`
