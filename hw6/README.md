# Базы данных и SQL, Семинар 6, Домашнее задание

`Select`-запросы необходимо составлять на основе БД `vk` и с данными, которые сгенерировали на прошлом уроке из скрипта `_vk_db_seed.sql` ([в материалах к 3-му уроку](https://github.com/dfedoroff/sql/tree/main/hw3 "Ссылка на материалы 3-го урока")).

## Задача 1

### Описание:

Написать функцию, которая удаляет всю информацию об указанном пользователе из БД vk. Пользователь задается по id. Удалить нужно все сообщения, лайки, медиа записи, профиль и запись из таблицы users. Функция должна возвращать номер пользователя.

```SQL
DROP FUNCTION IF EXISTS delete_user_from_vk;

DELIMITER $$

CREATE FUNCTION delete_user_from_vk(id_to_delete INT) RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE num_deleted INT;

    SET FOREIGN_KEY_CHECKS = 0;

    DELETE FROM messages WHERE from_user_id = id_to_delete OR to_user_id = id_to_delete;
    DELETE FROM likes WHERE user_id = id_to_delete;
    DELETE FROM media WHERE user_id = id_to_delete;
    DELETE FROM profiles WHERE user_id = id_to_delete;
    DELETE FROM users WHERE id = id_to_delete;

    SET num_deleted = ROW_COUNT();

    IF num_deleted = 0 THEN
        SIGNAL SQLSTATE '45000'
            SET MESSAGE_TEXT = 'Пользователь с таким id  в базе не найден';
    END IF;

    SET FOREIGN_KEY_CHECKS = 1;

    RETURN id_to_delete;
END$$

DELIMITER ;
```

Для вызова этой функции необходимо использовать следующий `sql`-запрос:

```SQL
SELECT delete_user_from_vk(id_to_delete);
```

## Задача 2

### Описание:

Предыдущую задачу решить с помощью процедуры и обернуть используемые команды в транзакцию внутри процедуры:

```SQL
DROP PROCEDURE IF EXISTS delete_user_from_vk;

DELIMITER $$

CREATE PROCEDURE delete_user_from_vk(id_to_delete INT)
BEGIN
    DECLARE num_deleted INT;
    DECLARE rollbacked INT DEFAULT 0;

    START TRANSACTION;
    SET FOREIGN_KEY_CHECKS = 0;
    DELETE FROM messages WHERE from_user_id = id_to_delete OR to_user_id = id_to_delete;
    DELETE FROM likes WHERE user_id = id_to_delete;
    DELETE FROM media WHERE user_id = id_to_delete;
    DELETE FROM profiles WHERE user_id = id_to_delete;
    DELETE FROM users WHERE id = id_to_delete;
    SET num_deleted = ROW_COUNT();
    IF num_deleted = 0 THEN
        ROLLBACK;
        SET rollbacked = 1;
        SIGNAL SQLSTATE '45000'
            SET MESSAGE_TEXT = 'Пользователь с таким id в базе не найден';
    END IF;
    SET FOREIGN_KEY_CHECKS = 1;
    IF rollbacked = 0 THEN
        COMMIT;
    END IF;
END$$

DELIMITER ;
```

Для вызова этой процедуры необходимо использовать следующий `sql`-запрос:

```SQL
CALL delete_user_from_vk(id_to_delete);
```

## Задача *3

### Описание:

Написать триггер, который проверяет новое появляющееся сообщество. Длина названия сообщества (поле `name`) должна быть не менее `5` символов. Если требование не выполнено, то выбрасывать исключение с пояснением.

```SQL
DELIMITER //

CREATE TRIGGER check_community_name
BEFORE INSERT ON communities
FOR EACH ROW
BEGIN
  IF CHAR_LENGTH(NEW.name) < 5 THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Название сообщества должно содержать не менее 5 символов';
  END IF;
END//

DELIMITER ;
```

Для проверки работоспособности триггера необходимо использовать следующий `sql`-запрос:

```SQL
INSERT INTO communities (name) VALUES ('qwer');
```

```SQL
SELECT * FROM communities;
```

