# Базы данных и SQL, Семинар 4, Домашнее задание

`Select`-запросы необходимо составлять на основе БД `vk` и с данными, которые сгенерировали на прошлом уроке из скрипта `_vk_db_seed.sql` ([в материалах к прошлому уроку](https://github.com/dfedoroff/sql/tree/main/hw3 "Ссылка на материалы прошлого урока")).

## Задача 1

### Описание:

Напишите скрипт, который подсчитывает количество групп, в которые вступил каждый пользователь:

```SQL
SELECT users.id, users.firstname, users.lastname,
       COUNT(users_communities.community_id) AS community_count
       FROM users
  LEFT JOIN users_communities ON users.id = users_communities.user_id
 GROUP BY users.id;
```

## Задача 2

### Описание:

Напишите скрипт, который подсчитывает количество пользователей в каждом сообществе:

```SQL
SELECT community_id,
       COUNT(user_id) AS user_count 
       FROM users_communities 
 GROUP BY community_id;
```

## Задача 3

### Описание:

Пусть задан некоторый пользователь. Из всех пользователей соц. сети найдите человека, который больше всех общался с выбранным пользователем (написал ему сообщений):

```SQL
SELECT u.firstname, u.lastname
  FROM users u
  JOIN messages m ON u.id = m.from_user_id
 WHERE m.to_user_id = 1
 GROUP BY u.id
 ORDER BY COUNT(*) DESC
 LIMIT 1;
```

## Задача *4

### Описание:

Напишите скрипт, который подсчитывает общее количество лайков, которые получили пользователи младше `18` лет:

```SQL
SELECT SUM(CASE
       WHEN YEAR(CURRENT_DATE()) - YEAR(birthday) < 18 THEN 1
       ELSE 0
       END) AS total_likes_under_18
  FROM likes
  JOIN profiles ON likes.user_id = profiles.user_id;
```

## Задача *5

### Описание:

Напишите скрипт, который определяет кто больше поставил лайков (всего): мужчины или женщины:

```SQL
SELECT CASE
       WHEN profiles.gender = 'f' THEN 'women'
       WHEN profiles.gender = 'm' THEN 'men'
       END AS gender,
       COUNT(likes.id) AS likes_count
  FROM likes
       INNER JOIN profiles
       ON likes.user_id = profiles.user_id
 GROUP BY profiles.gender
 ORDER BY likes_count DESC
 LIMIT 1;
```
