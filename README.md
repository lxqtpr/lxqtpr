# Даниил Гадельшин

Java backend-разработчик. Занимаюсь надёжностью Java-приложений и инструментами для воспроизведения сбоев и проверки восстановления.

**Java · JUnit 5 · Maven · PostgreSQL · JDBC · Docker Compose**

## Проекты

### [Java Async Replay Lab](https://github.com/lxqtpr/java-async-replay-lab)

Управляемые часы и исполнитель для асинхронной Java-логики: сохранение решений планировщика, повтор исполнения по трассе, адаптер JUnit 5 и сокращение заданного workload с сохранением сигнатуры ошибки.

**CI прошёл на JDK 17/21:** тесты, replay, corpus и reducer — [запуск](https://github.com/lxqtpr/java-async-replay-lab/actions/runs/39054600394). Локально проверены 46 тестов на JDK 21/25. В reducer-эксперименте сконструированные входы из 18, 54 и 198 событий сокращены до шести; исходные и сокращённые варианты проверены свежим replay.

[Карточка проекта](projects/java-async-replay-lab.md) · [README и запуск](https://github.com/lxqtpr/java-async-replay-lab#readme) · [Сохранённые результаты](https://github.com/lxqtpr/java-async-replay-lab/tree/main/research-results/reducer-2026-10-16)

### [Outbox Recovery Fault Lab](https://github.com/lxqtpr/outbox-recovery-fault-lab)

Java-модель и отдельный PostgreSQL/JDBC-стенд для проверки повторной доставки и восстановления worker-процесса. Заказ и outbox фиксируются одной транзакцией, эффект и запись дедупликации — другой.

**CI модели прошёл на JDK 17/21** — [запуск](https://github.com/lxqtpr/outbox-recovery-fault-lab/actions/runs/39054601432). PostgreSQL/JDBC проверен локально: по 8 интеграционных групп на JDK 21/25. В PostgreSQL-сценарии потерянного подтверждения получены **две доставки и один бизнес-эффект**.

[Карточка проекта](projects/outbox-recovery-fault-lab.md) · [README и запуск](https://github.com/lxqtpr/outbox-recovery-fault-lab#readme) · [Логи и SQL-проверка](https://github.com/lxqtpr/outbox-recovery-fault-lab/tree/main/research-results/postgres-2026-10-17)

## Резюме и контакты

[Текущее резюме (PDF)](https://github.com/lxqtpr/lxqtpr/blob/main/%D0%93%D0%B0%D0%B4%D0%B5%D0%BB%D1%8C%D1%88%D0%B8%D0%BD%20%D0%94%D0%B0%D0%BD%D0%B8%D0%B8%D0%BB.pdf). Обновлено 17 октября 2026.

[Telegram](https://t.me/lxqtpr) · [Email](mailto:lxqtpr@gmail.com)
