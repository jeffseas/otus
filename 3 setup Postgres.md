# 3. Установка Postgres
1.поставить на нем Docker Engine
Произведена установка десктопного Docker в Windows

2. сделать каталог /var/lib/postgres
команда docker run с расширением - v /var/lib/postgres

3. развернуть контейнер с PostgreSQL 15 смонтировав в него /var/lib/postgresql
docker run --rm --name postgres -e POSTGRES_PASSWORD=my_pass -d -p 5432:5432 -v /var/lib/postgres:/var/lib/postgresql/data postgres 

4. развернуть контейнер с клиентом postgres
postgres latest container_id: 20b75527fd6c

5. подключится из контейнера с клиентом к контейнеру с сервером и сделать таблицу с парой строк
```sql
create table test
(id integer,
tst text);

insert into public.test_table (id,txt) values (1,'raz');
insert into public.test_table (id,txt) values (2,'dva');
```

6. подключится к контейнеру с сервером с ноутбука/компьютера извне инстансов ЯО/места установки докера
psql -p 5432 -U postgres -h 192.168.65.0 -d postgres -W

7. удалить контейнер с сервером
docker rm 20b75527fd6c

8. создать его заново
docker run --rm --name postgres -e POSTGRES_PASSWORD=my_pass -d -p 5432:5432 -v /var/lib/postgres:/var/lib/postgresql/data postgres 

9. подключится снова из контейнера с клиентом к контейнеру с сервером
   проверить, что данные остались на месте
   
остались на месте