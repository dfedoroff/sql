# Базы данных и SQL, Семинар 1, Домашнее задание

## Задача 1

### Описание:

Создайте таблицу с мобильными телефонами, используя графический интерфейс. Необходимые поля таблицы: `product_name` (название товара), `manufacturer` (производитель), `product_count` (количество), `price` (цена). Заполните БД произвольными данными.

Создаем базу данных `sql_hw1`:

```SQL
CREATE DATABASE sql_hw1;

USE sql_hw1;
```

Создаем таблицу `smartphones` с необходимыми полями:

```SQL
CREATE TABLE smartphones (
    product_name  VARCHAR(30)  NOT NULL,
    manufacturer  VARCHAR(20)  NOT NULL,
    product_count INT          DEFAULT(0),
    price         DECIMAL(8,2) DEFAULT (0.0)
);
```

Проверяем правильность созданной таблицы:

```SQL
DESCRIBE smartphones;
```

Заполняем поля таблицы:

```SQL
INSERT INTO smartphones (product_name, manufacturer, product_count, price)
VALUES ('Galaxy S20', 'Samsung', 8000, 60000),
       ('Galaxy S21', 'Samsung', 10000, 80000),
       ('Galaxy S22', 'Samsung', 5000, 100000),
       ('Galaxy S23', 'Samsung', 3000, 120000),
       ('iPhone 12', 'Apple', 7000, 80000),
       ('iPhone 13', 'Apple', 8000, 100000),
       ('iPhone 14', 'Apple', 4000, 120000),
       ('iPhone 15', 'Apple', 2000, 140000),
       ('Pocophone', 'POCO', 1, 15000);
```

Просматриваем содержание всей таблицы:

```SQL
SELECT * FROM smartphones;
```

## Задача 2

### Описание:

Напишите `SELECT`-запрос, который выводит `название товара`, `производителя` и `цену` для товаров, количество которых превышает `2`:

```SQL
SELECT product_name, manufacturer, price
  FROM smartphones
 WHERE product_count > 2;
```

## Задача 3

### Описание:

Выведите `SELECT`-запросом весь ассортимент товаров марки `Samsung`:

```SQL
SELECT product_name, manufacturer
  FROM smartphones
 WHERE manufacturer = 'Samsung';
```

## Задача *4

### Описание:

С помощью `SELECT`-запроса с оператором `LIKE` / `REGEXP` найти:

*4.1. Товары, в которых есть упоминание `Iphone`:

```SQL
SELECT product_name
  FROM smartphones
 WHERE product_name LIKE '%Iphone%';
```

*4.2. Товары, в которых есть упоминание `Samsung`:

```SQL
SELECT product_name
  FROM smartphones
 WHERE product_name LIKE '%Samsung%';
```

*4.3. Товары, в названии которых есть `цифры`:

```SQL
SELECT product_name
  FROM smartphones
 WHERE product_name RLIKE '[0-9]';
```

*4.4. Товары, в названии которых есть цифра `8`:

```SQL
SELECT product_name
  FROM smartphones
 WHERE product_name LIKE '%8%';
```
