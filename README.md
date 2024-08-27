Вот исправленный код для файла `README.md`:

```markdown
# ClickHouse в Docker с настройкой прав доступа

Этот проект разворачивает ClickHouse в Docker, создает две базы данных (`test` и `main`) и настраивает роли для управления доступом к базе данных `main`.

## Шаги выполнения

1. **Создание директории проекта**
   ```bash
   mkdir clickhouse-docker-project
   cd clickhouse-docker-project
   ```

2. **Создание и настройка `docker-compose.yml`**
   - Создайте файл `docker-compose.yml` с конфигурацией для ClickHouse:
     ```yaml
     version: '3.7'

     services:
       clickhouse-server:
         image: yandex/clickhouse-server:latest
         container_name: clickhouse-server
         ports:
           - "8123:8123"
           - "9000:9000"
           - "9009:9009"
         volumes:
           - ./clickhouse_data:/var/lib/clickhouse
     ```

3. **Запуск контейнера**
   - Выполните команду для запуска ClickHouse в контейнере:
     ```bash
     docker-compose up -d
     ```

4. **Подключение к ClickHouse через DBeaver**
   - Установите DBeaver.
   - Настройте подключение к ClickHouse, используя следующие параметры:
     - **Host:** `localhost`
     - **Port:** `9000`
     - **Database:** `default`
     - **Username:** `default`
     - **Password:** оставьте пустым (по умолчанию).

5. **Создание баз данных и ролей**
   - В SQL-редакторе DBeaver создайте базы данных `test` и `main`:
     ```sql
     CREATE DATABASE test;
     CREATE DATABASE main;
     ```

   - Создайте роли для управления доступом к базе данных `main`:
     ```sql
     CREATE ROLE readonly;
     GRANT SELECT ON main.* TO readonly;

     CREATE ROLE writer;
     GRANT INSERT ON main.* TO writer;
     ```

6. **Создание пользователя и назначение роли**
   - Создайте пользователя и назначьте ему роль:
     ```sql
     CREATE USER test_user IDENTIFIED BY 'password';
     GRANT readonly TO test_user;
     ```

7. **Проверка работы ролей**
   - Подключитесь к ClickHouse под пользователем `test_user` через DBeaver.
   - Выполните SQL-запрос для подтверждения прав доступа:
     ```sql
     SHOW TABLES FROM main;
     ```
