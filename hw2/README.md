# Базы данных и SQL, Семинар 2, Домашнее задание

## Задача 1

### Описание:

Создать БД `vk`, исполнив скрипт `_vk_db_creation.sql` (в материалах к уроку):

Запускаем скрипт, а затем проверяем, была ли создана БД `vk`:

```SQL
SHOW DATABASES;
```

Проверяем, какие таблицы были созданы в БД `vk`:

```SQL
USE vk;

SHOW FULL TABLES;
```

## Задача 2

### Описание:

Написать скрипт, добавляющий в созданную БД `vk` две-три новые таблицы (с перечнем полей, указанием индексов и внешних ключей) используя `CREATE TABLE`:

```SQL
USE vk;

-- Таблица постов
DROP TABLE IF EXISTS posts;
CREATE TABLE posts (
    id SERIAL,
    user_id BIGINT UNSIGNED NOT NULL,
    body TEXT,
    created_at DATETIME DEFAULT NOW(),
    
    FOREIGN KEY (user_id) REFERENCES users(id)
);

-- Таблица комментариев к постам
DROP TABLE IF EXISTS comments;
CREATE TABLE comments (
    id SERIAL,
    user_id BIGINT UNSIGNED NOT NULL,
    post_id BIGINT UNSIGNED NOT NULL,
    body TEXT,
    created_at DATETIME DEFAULT NOW(),
    
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (post_id) REFERENCES posts(id)
);

-- Таблица лайков к постам
DROP TABLE IF EXISTS post_likes;
CREATE TABLE post_likes (
    id SERIAL,
    user_id BIGINT UNSIGNED NOT NULL,
    post_id BIGINT UNSIGNED NOT NULL,
    created_at DATETIME DEFAULT NOW(),
    
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (post_id) REFERENCES posts(id)
);
```

Проверяем, были ли созданы новые таблицы `posts`, `comments` и `post_likes` в БД `vk`:

```SQL
SHOW FULL TABLES;
```

## Задача 3

### Описание:

Заполнить две таблицы в БД `vk` данными (по десять записей в каждой таблице) используя `INSERT`:

Заполняем таблицу `users`:

```SQL
INSERT INTO users (firstname, lastname, email, password_hash, phone)
VALUES ('Иван', 'Давыдов', 'ivan@gmail.com', 'ltnfclvgkajrpo9udfxvsldkrn24l5456345t', 89236543210),
       ('Анна', 'Ковалева', 'anna_koval@mail.ru', 'ltnfclvgkajrpo9udfxvsldkrn24l5456345t', 89993056789),
       ('Ольга', 'Горцеркевич', 'gor.olga@yandex.ru', 'ltnfclvgkajrpo9udfxvsldkrn24l5456345t', 89130543210),
       ('Илья', 'Сидоров', 'ilya_sidorov@gmail.com', 'ltnfclvgkajrpo9udfxvsldkrn24l5456345t', 89133446789),
       ('Илья', 'Соколов', 'sokolov-ilya@yandex.ru', 'ltnfclvgkajrpo9udfxvsldkrn24l5456345t', 89996503210),
       ('Марина', 'Крылова', 'marina_krylova_89@mail.ru', 'ltnfclvgkajrpo9udfxvsldkrn24l5456345t', 89233450789),
       ('Андрей', 'Константинов', 'andrey_konstantinov@mail.ru', 'ltnfclvgkajrpo9udfxvsldkrn24l5456345t', 89234543010),
       ('Виктория', 'Фильцер', 'filzer_vika@gmail.com', 'ltnfclvgkajrpo9udfxvsldkrn24l5456345t', 89133456789),
       ('Сергей', 'Михалков', 'mikhailov_sergei@yandex.ru', 'ltnfclvgkajrpo9udfxvsldkrn24l5456345t', 89871513210),
       ('Надежда', 'Бахденберг', 'nadezhda-bah@mail.ru', 'ltnfclvgkajrpo9udfxvsldkrn24l5456345t', 89127456789);
```

Просматриваем содержание таблицы `users`:

```SQL
SELECT * FROM users;
```

Заполняем таблицу `profiles`:

```SQL
INSERT INTO profiles (user_id, gender, birthday, hometown)
VALUES (1, 'M', '2000-09-04', 'Москва'),
       (2, 'F', '1992-01-03', 'Санкт-Петербург'),
       (3, 'F', '1994-03-07', 'Казань'),
       (4, 'M', '1998-09-04', 'Нижний Новгород'),
       (5, 'M', '1996-05-05', 'Екатеринбург'),
       (6, 'F', '2006-10-08', 'Сочи'),
       (7, 'M', '2002-08-01', 'Ростов-на-Дону'),
       (8, 'F', '2004-05-03', 'Красноярск'),
       (9, 'M', '1990-09-01', 'Владивосток'),
       (10, 'F', '2008-10-10', 'Пермь');
```

Проверяем содержание таблицы `profiles`:

```SQL
SELECT * FROM profiles;
```

Заполняем таблицу `posts`:

```SQL
INSERT INTO posts (user_id, body)
VALUES (1, 'Всем привет! Как дела?'),
       (2, 'Сегодня отличный день!'),
       (3, 'Ура! Я нашел время для отдыха'),
       (4, 'Новое видео на моем канале!'),
       (5, 'Сегодняшний просто волшебный день'),
       (6, 'Я сегодня к сожалению работаю'),
       (7, 'Всем хорошего дня!'),
       (8, 'Сегодня читал интересные новости. Вот ссылка'),
       (9, 'Очень классное новое кафе в центре'),
       (10, 'Скоро выходит мой новый проект!');
```

Проверяем содержание таблицы `posts`:

```SQL
SELECT * FROM posts;
```

Заполняем таблицу `comments`:

```SQL
INSERT INTO comments (user_id, post_id, body)
VALUES (1, 1, 'Привет! У нас все ок, как сам?'),
       (2, 3, 'Отдыхать полезно. Пойдешь на пляж?'),
       (3, 4, 'Супер! Посмотрю, но позже'),
       (4, 6, 'Печалька'),
       (5, 8, 'Гляну позже'),
       (6, 10, 'Ждем-с с нетерпением'),
       (7, 5, 'Согласен'),
       (8, 2, 'Да, согласен. Собираемся на пляж'),
       (9, 7, 'Тебе тоже!'),
       (10, 9, 'Ага, отличное место! Был уже там');
```

Проверяем содержание таблицы `comments`:

```SQL
SELECT * FROM comments;
```

## Задача *4

### Описание:

Написать скрипт, отмечающий несовершеннолетних пользователей как неактивных (поле `is_active = true`). При необходимости предварительно добавить такое поле в таблицу `profiles` со значением по умолчанию `false` (или `0`) используя `ALTER TABLE` и `UPDATE`:

```SQL
USE vk;

ALTER TABLE profiles
ADD COLUMN is_active BOOLEAN NOT NULL DEFAULT false;

UPDATE profiles
   SET is_active = true
 WHERE TIMESTAMPDIFF(YEAR, birthday, CURDATE()) < 18;
```

## Задача *5

### Описание:

Написать скрипт, удаляющий сообщения "из будущего" (дата позже сегодняшней) используя `DELETE`:

Сначала увеличим дату сообщения созданного, например, `user_id = 10` на один день, а  уже затем удалим это сообщение "из будущего":

```SQL
USE vk;

UPDATE comments
   SET created_at = DATE_ADD(created_at, INTERVAL 1 DAY)
 WHERE user_id = 8;

DELETE FROM comments
 WHERE created_at > CURRENT_TIMESTAMP();
```

Проверяем содержание таблицы `comments`:

```SQL
SELECT * FROM comments;
```

