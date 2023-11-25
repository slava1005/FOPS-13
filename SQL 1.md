## Домашнее задание к занятию «SQL. Часть 1» -"Вячеслав Савилов"

### Задание 1
Получите уникальные названия районов из таблицы с адресами, которые начинаются на “K” и заканчиваются на “a” и не содержат пробелов.
```
SELECT DISTINCT district 
FROM address 
WHERE district  LIKE 'k%a' and district not LIKE  '% %';
```
![001](https://github.com/slava1005/FOPS-13/assets/114395964/816609b5-3231-4d50-91f1-7dcd968da3bf)

### Задание 2
Получите из таблицы платежей за прокат фильмов информацию по платежам, которые выполнялись в промежуток с 15 июня 2005 года по 18 июня 2005 года включительно и стоимость которых превышает 10.00.
```
SELECT * FROM payment WHERE payment_date BETWEEN '2005-06-15 00:00:00' AND '2005-06-18 23:59:59' AND amount > 10;
```
![2a](https://github.com/slava1005/FOPS-13/assets/114395964/51920d26-bb7e-4dd1-ab3b-ff6f6a198fa2)

### Задание 3
Получите последние пять аренд фильмов.
```
SELECT *  
FROM rental   
ORDER by rental_date DESC 
LIMIT 5;
```
![004](https://github.com/slava1005/FOPS-13/assets/114395964/dc094c3a-86b3-46ea-89b9-65d598234855)

### Задание 4
Одним запросом получите активных покупателей, имена которых Kelly или Willie.

Сформируйте вывод в результат таким образом:

все буквы в фамилии и имени из верхнего регистра переведите в нижний регистр, замените буквы 'll' в именах на 'pp'.
```
SELECT LOWER(REPLACE(first_name, 'L', 'p')), LOWER(last_name) 
FROM customer
WHERE first_name LIKE 'Willie' OR first_name  LIKE 'Kelly';
```
![007](https://github.com/slava1005/FOPS-13/assets/114395964/630065dd-0154-4289-8592-c68899b0326c)
