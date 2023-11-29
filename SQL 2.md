## Домашнее задание к занятию «SQL. Часть 2»


Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1
Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:

фамилия и имя сотрудника из этого магазина;
город нахождения магазина;
количество пользователей, закреплённых в этом магазине.

Ответ
```
select concat(sotr.first_name , ' ', sotr.last_name) as сотрудник,  c2.city as город, COUNT(c.customer_id) as "покупатели"
from staff sotr
join store s2 on s2.store_id = sotr.store_id 
join customer c on c.store_id = s2.store_id
join address a on a.address_id = s2.address_id 
join city c2 on c2.city_id = a.city_id 
group by sotr.staff_id, c2.city_id 
having COUNT(c.customer_id) > 300;
```
![1](https://github.com/slava1005/FOPS-13/assets/114395964/9f9eaa40-5c5b-4601-98a9-1ed55a1c19f4)

### Задание 2
Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

Ответ
```
select count(film_id) as "кол-во фильмов" from film 
where length > (select AVG(length) from film);
```
![2](https://github.com/slava1005/FOPS-13/assets/114395964/1cad286d-4f6d-4632-b07e-0cdb9f73375b)

### Задание 3
Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

Ответ
```
select month(payment_date) as месяц, SUM(p.amount) "сумма платежей", COUNT(p.rental_id) "кол-во аренд" 
from payment p
group by MONTH(payment_date)
order by SUM(p.amount ) 
desc limit 1;
```
![3](https://github.com/slava1005/FOPS-13/assets/114395964/04d0b061-affc-4c40-a94a-7b7e295d650b)

Новая команда:
```
SELECT DATE_FORMAT(p.payment_date, '%Y-%M') AS Дата , (sum(p.amount )) AS Сумма , count((p.rental_id )) AS Аренд
FROM payment p 
GROUP BY Дата
ORDER BY Сумма DESC
LIMIT 1;
```
