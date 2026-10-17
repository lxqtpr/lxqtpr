# Outbox Recovery Fault Lab

[Репозиторий](https://github.com/lxqtpr/outbox-recovery-fault-lab) · [Профиль](../README.md)

**Задача:** показать, что происходит между commit, публикацией события и подтверждением обработки — и проверить восстановление после сбоя в этих точках.

## Два режима

**Модель в памяти:** producer, relay и consumer; текстовые сценарии, повторная доставка и проверки состояния. Наивный `DB + send` показывает, как после commit можно потерять намерение отправить событие.

**PostgreSQL/JDBC:** реальные транзакции `order + outbox` и `effect + dedup`. Стабильный `eventId`, проверка совпадения payload, отдельные commit доставки и ack. Доставки хранятся в SQL-очереди PostgreSQL. Для проверки восстановления worker JVM завершается через `Runtime.halt(72)`, затем запускается новый процесс.

## Проверенные результаты

- В модели проходят 14 групп проверок, 48 заданных сочетаний сбоев и шесть воспроизводимых fixtures.
- С PostgreSQL 17.11 и pgJDBC 42.7.12 проходят по восемь интеграционных групп на JDK 21.0.10 и 25.0.2.
- После потерянного подтверждения публикации SQL показывает две доставки, одну запись дедупликации, один бизнес-эффект и ноль ожидающих или неподтверждённых сообщений.

## Быстрый просмотр

Модель запускается без внешних сервисов:

```sh
sh scripts/test.sh
sh scripts/run.sh fixtures/01-naive-lost-intent.txt
sh scripts/run.sh fixtures/03-lost-publish-ack.txt
```

Отдельный PostgreSQL-стенд:

```sh
export PGJDBC_JAR=/absolute/path/postgresql-42.7.12.jar
sh scripts/pg-lab.sh up
sh scripts/pg-lab.sh test
sh scripts/pg-lab.sh inspect
sh scripts/pg-lab.sh down
```

Для стенда нужны уже доступный локальный образ PostgreSQL и JDBC JAR. Тест использует отдельную лабораторную БД и сбрасывает её схему. Порт по умолчанию — `127.0.0.1:55435`; команда `down` удаляет данные стенда.

## Исходники и результаты

- [README: устройство и команды](https://github.com/lxqtpr/outbox-recovery-fault-lab#readme).
- [Статус реализации и границы проверки](https://github.com/lxqtpr/outbox-recovery-fault-lab/blob/main/IMPLEMENTATION_STATUS.md).
- [Описание PostgreSQL-проверки](https://github.com/lxqtpr/outbox-recovery-fault-lab/blob/main/research-results/postgres-2026-10-17/README.md).
- [Лог JDK 21](https://github.com/lxqtpr/outbox-recovery-fault-lab/blob/main/research-results/postgres-2026-10-17/postgres-jdk21.log) и [лог JDK 25](https://github.com/lxqtpr/outbox-recovery-fault-lab/blob/main/research-results/postgres-2026-10-17/postgres-jdk25.log).
- [Итоговая SQL-проверка](https://github.com/lxqtpr/outbox-recovery-fault-lab/blob/main/research-results/postgres-2026-10-17/sql-inspection.txt).
