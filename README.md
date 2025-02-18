# Домашнее задание к занятию "`sql2`" - `Татаринцев Алексей`



### Задание 1

1. `Разворачиваю базу данных sakila и выполняю запросы по заданию`
`Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию`
2. `фамилия и имя сотрудника из этого магазина`
```
SELECT
    e.first_name,
    e.last_name
FROM
    staff e
WHERE
    e.store_id IN (
        SELECT
            s.store_id
        FROM
            store s
        JOIN
            customer c ON s.store_id = c.store_id
        GROUP BY
            s.store_id
        HAVING
            COUNT(c.customer_id) > 300
    );

```
![1](https://github.com/Foxbeerxxx/sql2/blob/main/img/img1.png)`

3. `город нахождения магазина`
```
SELECT DISTINCT
    ci.city
FROM
    store s
JOIN
    address a ON s.address_id = a.address_id
JOIN
    city ci ON a.city_id = ci.city_id
WHERE
    s.store_id IN (
        SELECT
            s.store_id
        FROM
            store s
        JOIN
            customer c ON s.store_id = c.store_id
        GROUP BY
            s.store_id
        HAVING
            COUNT(c.customer_id) > 300
    );

```
![2](https://github.com/Foxbeerxxx/sql2/blob/main/img/img2.png)`

4. `количество пользователей, закреплённых в этом магазине`
```
SELECT
    COUNT(c.customer_id) AS customer_count
FROM
    store s
JOIN
    customer c ON s.store_id = c.store_id
WHERE
    s.store_id IN (
        SELECT
            s.store_id
        FROM
            store s
        JOIN
            customer c ON s.store_id = c.store_id
        GROUP BY
            s.store_id
        HAVING
            COUNT(c.customer_id) > 300
    );

```
![3](https://github.com/Foxbeerxxx/sql2/blob/main/img/img3.png)`


### Задание 2

`Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.`

1. `Формирую запрос в базу данных`
```
SELECT COUNT(*) AS film_count
FROM film
WHERE length > (SELECT AVG(length) FROM film);

```
![4](https://github.com/Foxbeerxxx/sql2/blob/main/img/img4.png)`


### Задание 3

`Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.`

1. `Составил запрос`

```
SELECT
    MONTH(p.payment_date) AS payment_month,
    YEAR(p.payment_date) AS payment_year,
    SUM(p.amount) AS total_payment_amount,
    COUNT(r.rental_id) AS total_rentals
FROM
    payment p
JOIN
    rental r ON p.rental_id = r.rental_id
GROUP BY
    payment_month, payment_year
ORDER BY
    total_payment_amount DESC
LIMIT 1;

```
![5](https://github.com/Foxbeerxxx/sql2/blob/main/img/img5.png)`


