# Java Async Replay Lab

[Репозиторий](https://github.com/lxqtpr/java-async-replay-lab) · [Профиль](../README.md)

**Задача:** сделать ошибку порядка асинхронных событий повторяемой и удобной для разбора.

Пример: операция уже завершилась по таймауту, а поздний callback всё ещё добавляет побочный эффект. В проекте есть ошибочный вариант и вариант с проверкой результата завершения.

## Устройство

- `ControlledRuntime`: явно внедряемые `Clock` и `Executor`, виртуальное время, отложенные callback и отмена ожидающих задач.
- `RunTrace` / `TraceFiles`: сохранение решений планировщика и strict replay.
- `async-replay-junit5`: отдельный runtime на тест и проверка оставшихся задач при teardown.
- `async-replay-reducer`: два ddmin-style режима сокращения упорядоченного workload с объявленными зависимостями и сохранением конкретной сигнатуры ошибки.

Исполнение callback кооперативное: исследуемые операции должны использовать внедрённые часы и исполнитель.

## Проверенные результаты

- [GitHub CI](https://github.com/lxqtpr/java-async-replay-lab/actions/runs/39054600394) прошёл на JDK 17 и 21: тесты, replay примеров, corpus из 32 seeds и reducer.
- Локально 46 тестов проходят на JDK 21.0.10 и 25.0.2; сохранённые исследовательские результаты относятся к указанным в них локальным версиям исходников.
- В reducer-эксперименте девять сконструированных workload проверены в двух режимах: по 18 строк результатов на каждом JDK. Входы 18/54/198 событий сокращены до шести целевых событий.
- Для исходных и сокращённых входов сохранены трассы, выполнен свежий strict replay и пройдены исправленные контрольные варианты.

## Быстрый просмотр

Из каталога `java-async-replay-lab`:

```sh
sh scripts/verify.sh -o
sh scripts/demo.sh replay examples/timeout-bug.trace
sh scripts/demo.sh replay examples/timeout-guarded.trace guarded
sh scripts/run_reducer.sh target/reducer-local
```

Для offline-проверки Maven-зависимости должны быть доступны в локальном кэше. Runner reducer требует пустой или ещё не существующий выходной каталог.

## Исходники и результаты

- [README: устройство и команды](https://github.com/lxqtpr/java-async-replay-lab#readme).
- [Статус реализации и проверок](https://github.com/lxqtpr/java-async-replay-lab/blob/main/IMPLEMENTATION_STATUS.md).
- [Описание reducer-эксперимента и его границ](https://github.com/lxqtpr/java-async-replay-lab/blob/main/docs/reducer-baseline-ru.md).
- [Сводка JDK 21](https://github.com/lxqtpr/java-async-replay-lab/blob/main/research-results/reducer-2026-10-16/jdk21/summary.json) и [JDK 25](https://github.com/lxqtpr/java-async-replay-lab/blob/main/research-results/reducer-2026-10-16/jdk25/summary.json).
- [Сырые строки JDK 21](https://github.com/lxqtpr/java-async-replay-lab/blob/main/research-results/reducer-2026-10-16/jdk21/raw.csv) и [сохранённые входы и трассы обоих JDK](https://github.com/lxqtpr/java-async-replay-lab/tree/main/research-results/reducer-2026-10-16).
