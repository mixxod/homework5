
Home work 5 (sql)
```
Задачи:
установить postgresql
создать базу
создать несколько таблиц и наполнить данными
сделать дамп базы
проверить восстановление (предварительно удалив базу)
Выложить на GIT список команд и файл с дампом базы данных
```
```
# получаем права суперпользователя и обновляем репозиторий 
sudo su
apt update
```
```
# устанавливаем необходимые пакеты
apt install postgresql
apt install postgresql-contrib
apt install pg-activity (pgbench в нем уже имеется)
```
```
# создание пользователя, базы и проверки
Можем убедиться, что создался пользбхователь postgresql
cat /etc/passwd | grep pos
Переходим под пользователя postgres
su postgres
psql
Для просмотра списка баз данных вводим команду
\l или /l+ для расширеного вида
\du+ (описание ролей)
Создаем пользователя под постгрес (postgres-#)
create user mixxod with password 'pass';
проверяем 
\du+
Создаем базу данных
create database mikedb;
Подключаемся к БД
\c mikedb
Убeждаемся, что база пуста и выходим из postgres
```
```
# клонируем базу из github
git clone https://github.com/pthom/northwind_psql.git
# Перенос содержимога базы sql в базу mikedb
Меняем пользователя
su postgres
```
```
# заливаем содержимое в БД
psql -d mikedb -f ./northwind.sql
ИЛИ (равнозначные команды)
psql -d mikedb -f /home/red/northwind_psql/northwind.sql
Убедимся, чтобаза наполнилась
psql 
\l
Подключаемся к БД
\с mikedb
Смотрим содержимое
\dt+
```
```
# делаем дамп БД
Т.к для директорий из под разных пользователей нужны свои права.
Создаем директрию в пользователе postgres 
mkdir /tmp/dump_backup
Копируем содержимое БД mikedb 
pg_dump mikedb > /tmp/dump_backup/prod.dump.sql
```
```
# входим в PostgreSQL и удаляем БД 
psql
drop database mikedb;
```
```
# создаем БД
Работаем в psql
create database mikedb2;
\q
```
```
# восстанавливаем содержимое БД из дампа
psql mikedb2 < /tmp/dump_backup/prod.dump.sql
```
```
# Выполним проверку восстановленой БД
psql
\c mikedb2
\dt+
```
