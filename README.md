# «Резервное копирование баз данных» Акиньшин С.Г.

## Задание-1

### `Для повышения надежности работы баз данных и их резервного копирования в финансовой компании можно использовать следующие варианты:`

1.1. Полная ежедневная резервная копия:
   - Выполнять полную резервную копию баз данных ежедневно в конце рабочего дня.
   - Сохранять созданные резервные копии на отдельных надежных хранилищах данных, вне пределов основной инфраструктуры.
   - Убедиться в целостности и доступности резервных копий для восстановления данных в случае сбоя.

1.2. Часовая резервная копия:
   - Установить механизм резервного копирования, который будет создавать копии данных каждый час.
   - Сохранять резервные копии в надежном хранилище данных с возможностью быстрого восстановления.
   - В случае поломки базы данных, можно будет восстановить данные, находящиеся на момент, за час до возникновения сбоя.
     Для этого можно использовать инкрементное резервное копирование. При инкрементном резервном копировании создаются копии только измененных данных с момента последнего полного резервного копирования или предыдущего инкрементного копирования. Это помогает сократить время и объем резервного копирования. Можно выполнять инкрементное копирование на регулярной основе, например, каждый час.

1.3.* Моментальное переключение на работающую или починенную базу данных:
   - Реализовать механизм репликации базы данных, который будет поддерживать дублирующую (зеркальную) базу данных.
   - Обеспечить автоматическое переключение на работающую реплику в случае поломки основной базы данных.
   - Восстановление данных может происходить практически мгновенно, поскольку работающая реплика уже содержит актуальные данные.

Важно учитывать, что каждый из этих вариантов имеет свои преимущества и недостатки, и выбор подходящего метода резервного копирования должен основываться на требованиях к надежности, восстановлению данных и доступности системы в конкретном случае.

## Задание-2

### `Пример команды резервирования данных и восстановления базы данных с использованием утилиты pg_dump/pg_restore для PostgreSQL:`

Резервирование данных (pg_dump):
```
pg_dump -U <username> -h <hostname> -p <port> -d <database_name> -F c -f <backup_file_name>
```

- `<username>`: имя пользователя базы данных.
- `<hostname>`: имя хоста, на котором работает база данных (обычно localhost).
- `<port>`: номер порта, на котором работает база данных (обычно 5432).
- `<database_name>`: имя базы данных, которую необходимо скопировать.
- `<backup_file_name>`: имя файла, в который будет сохранена резервная копия базы данных.

Восстановление данных (pg_restore):
```
pg_restore -U <username> -h <hostname> -p <port> -d <database_name> <backup_file_name>
```

- `<username>`: имя пользователя базы данных.
- `<hostname>`: имя хоста, на котором работает база данных (обычно localhost).
- `<port>`: номер порта, на котором работает база данных (обычно 5432).
- `<database_name>`: имя базы данных, в которую будут восстановлены данные.
- `<backup_file_name>`: имя файла, из которого будет восстановлена база данных.

2.1.* Автоматизация процесса резервирования и восстановления данных возможна с помощью различных сценариев и инструментов. Некоторые из них включают:

- Написание скриптов на языке программирования (например, Python, Bash), которые будут выполнять соответствующие команды pg_dump и pg_restore с необходимыми параметрами и расписанием выполнения.
- Использование планировщика задач операционной системы (например, cron в Linux) для запуска автоматических заданий резервного копирования и восстановления данных.
- Использование специализированных инструментов управления базами данных, которые предоставляют функциональность автоматического резервного копирования и восстановления данных (например, pgBackRest, Barman).

Эти методы позволяют автоматизировать процесс создания резервных копий и восстановления баз данных, устанавливать расписание выполнения, управлять хранением резервных копий и обеспечивать надежность работы.

## Задание-3

### `Пример команды для инкрементного резервного копирования базы данных MySQL с использованием утилиты mysqldump:`

```
mysqldump --user=<username> --password=<password> --host=<hostname> --port=<port> --single-transaction --flush-logs --master-data=2 --databases <database_name> --default-character-set=utf8 --hex-blob --routines --triggers --events --result-file=<backup_file_name>
```

- `<username>`: имя пользователя базы данных.
- `<password>`: пароль пользователя базы данных.
- `<hostname>`: имя хоста, на котором работает база данных (обычно localhost).
- `<port>`: номер порта, на котором работает база данных (обычно 3306).
- `<database_name>`: имя базы данных, которую необходимо скопировать.
- `<backup_file_name>`: имя файла, в который будет сохранена резервная копия базы данных.

В случае использования инкрементного резервного копирования с помощью mysqldump, команда будет создавать резервную копию только измененных данных с момента предыдущего резервного копирования, что позволяет сократить время и объем резервного копирования.

3.1.* Использование реплики базы данных может быть более предпочтительным по сравнению с обычным резервным копированием в следующих случаях:

1. Высокая доступность: Репликация базы данных позволяет иметь актуальную копию данных, которая всегда доступна для чтения и может использоваться в случае сбоя основной базы данных. Это обеспечивает более быстрое восстановление и минимальное время простоя.

2. Отказоустойчивость: Если основная база данных выходит из строя, реплика может немедленно перехватить работу и стать новым источником данных без необходимости восстановления из резервной копии. Это позволяет минимизировать потерю данных и обеспечить непрерывность работы.

3. Масштабируемость чтения: Реплика базы данных может использоваться для распределения нагрузки на чтение между основной базой данных и репликами. Это увеличивает производительность системы и обеспечивает более эффективное использование ресурсов.

Однако, стоит отметить, что репликация не является заменой полноценного резервного копирования. В случае повреждения или нежелательных изменений данных, реплика также будет содержать эти проблемы. Поэтому рекомендуется использовать репликацию в сочетании с регулярным резервным копированием для обеспечения полной защиты данных.