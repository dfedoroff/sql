# Базы данных и SQL, Семинар 5, Домашнее задание

`Select`-запросы необходимо составлять на основе БД `vk` и с данными, которые сгенерировали на прошлом уроке из скрипта `_vk_db_seed.sql` ([в материалах к 3-му уроку](https://github.com/dfedoroff/sql/tree/main/hw3 "Ссылка на материалы 3-го урока")).

## Задача 1

### Описание:

Создайте представление используя `CREATE VIEW` с произвольным `SELECT`-запросом из прошлых уроков:

```SQL
CREATE OR REPLACE VIEW friends_per_community AS
SELECT c.id, c.name, COUNT(DISTINCT fr.initiator_user_id) as approved_friends_count
  FROM communities c
  LEFT JOIN friend_requests fr ON c.id = fr.target_user_id AND fr.status = 'approved'
 GROUP BY c.id;
```

Созданное представление `friends_per_community`, соединяет таблицы `communities` и `friend_requests` по полю `id`, и выбирает имя и идентификатор каждого сообщества, а также подсчитывает количество уникальных идентификаторов пользователей, которые являются подтвержденными друзьями в сообществе. Оператор `LEFT JOIN` позволяет включить в результаты каждое сообщество даже, если в нем не было подтвержденных друзей.

## Задача 2

### Описание:

Выведите данные, используя написанное представление используя `SELECT`:

```SQL
SELECT id, name, approved_friends_count
  FROM friends_per_community
 ORDER BY approved_friends_count DESC
 LIMIT 3;
```

Этот `SELECT`-запрос, проверяет работу представления `friends_per_community`. Он выбирает три строки из представления `friends_per_community`, отсортированные по количеству подтвержденных друзей `approved_friends_count` в порядке убывания. Оператор `LIMIT` ограничивает результаты только `3` строками, которые представляют `3` сообщества с наибольшим количеством подтвержденных друзей.

## Задача 3

### Описание:

Удалите представление используя `DROP VIEW`:

```SQL
DROP VIEW friends_per_community;
```

Проверяем, было ли удалено представление `friends_per_community` из БД `vk`:

```SQL
SHOW FULL TABLES IN vk WHERE TABLE_TYPE LIKE 'VIEW';
```

## Задача *4

### Описание:

Сколько новостей (записей в таблице `media`) у каждого пользователя? Вывести поля: `news_count` (количество новостей), `user_id` (номер пользователя), `user_email` (email пользователя). Попробовать решить с помощью `CTE` или с помощью обычного `JOIN`.

Используя общие табличные выражения `CTE`:

```SQL
WITH media_count AS (
     SELECT user_id, COUNT(*) AS news_count
     FROM media
     GROUP BY user_id
)

SELECT mc.news_count, u.id AS user_id, u.email AS user_email
  FROM media_count mc
  JOIN users u ON mc.user_id = u.id;
```

Используя оператор `JOIN`:

```SQL
SELECT COUNT(*) as news_count, m.user_id, u.email as user_email
  FROM media m
  JOIN users u ON m.user_id = u.id
 GROUP BY m.user_id;
```

