# Базы данных и SQL, Семинар 3, Домашнее задание

`Select`-запросы необходимо составлять на основе БД `vk`, которую можно восстановить из скрипта `vk_db_seed.sql` (в материалах к уроку).

## Задача 1

### Описание:

Напишите скрипт, который возвращает список имен (только `firstname`) пользователей без повторений в алфавитном порядке используя `ORDER BY`:

```SQL
SELECT DISTINCT firstname
  FROM users
 ORDER BY firstname ASC;
```

## Задача 2

### Описание:

Напишите скрипт, который выводит количество мужчин старше `35` лет (таблица `profiles`) используя `COUNT`:

```SQL
SELECT COUNT(*)
  FROM profiles
 WHERE gender = 'm' AND birthday < DATE_SUB(NOW(), INTERVAL 35 YEAR);
```

## Задача 3

### Описание:

Напишите скрипт, который показывает количество заявок в друзья в каждом статусе (таблица `friend_requests`) используя `GROUP BY`:

```SQL
SELECT status, COUNT(*) as count
  FROM friend_requests
 GROUP BY status;
```

## Задача 4

### Описание:

Напишите скрипт, который выводит номер пользователя, который отправил больше всех заявок в друзья (таблица `friend_requests`) используя `LIMIT`:

```SQL
SELECT initiator_user_id, COUNT(*) as requests_sent
  FROM friend_requests
 GROUP BY initiator_user_id
 ORDER BY requests_sent DESC
 LIMIT 1;
```

## Задача *5

### Описание:

Напишите скрипт, который выводит названия и номера групп, имена которых состоят из 5 символов (таблица `communities`) используя `LIKE`:

```SQL
SELECT name, id
  FROM communities
 WHERE name LIKE '_____';
```

