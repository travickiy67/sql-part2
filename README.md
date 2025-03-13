# Травицкий Сергей
# Домашнее задание к занятию «SQL. Часть 2»

### Инструкция по выполнению домашнего задания

1. Сделайте fork [репозитория c шаблоном решения](https://github.com/netology-code/sys-pattern-homework) к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw).
2. Выполните клонирование этого репозитория к себе на ПК с помощью команды `git clone`.
3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
   - впишите вверху название занятия и ваши фамилию и имя;
   - в каждом задании добавьте решение в требуемом виде: текст/код/скриншоты/ссылка;
   - для корректного добавления скриншотов воспользуйтесь инструкцией [«Как вставить скриншот в шаблон с решением»](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md);
   - при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в [инструкции по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md).
4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`).
5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
6. Любые вопросы задавайте в чате учебной группы и/или в разделе «Вопросы по заданию» в личном кабинете.

Желаем успехов в выполнении домашнего задания.

---

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

```
SELECT CONCAT(s.first_name, ' ', s.last_name) AS "Сотрудник", a.address AS Адрес, 
COUNT(c.store_id) AS "Количество покупателей"
FROM staff s
JOIN store ss ON ss.manager_staff_id = s.staff_id 
JOIN customer c ON ss.store_id = c.store_id 
JOIN address a ON ss.address_id = a.address_id 
GROUP BY ss.manager_staff_id 
HAVING COUNT(c.customer_id) > 300;
```
<details>
<summary>Скрин</summary>  

![img](https://github.com/travickiy67/sql-part2/blob/main/img/img1.1.png)  

</details>

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

```
SwELECT(SELECT MAX(`length`) FROM film) AS MAX, (SELECT MIN(`length`) FROM film) AS MIN,
(SELECT AVG (`length`) FROM film) AS Average, (SELECT COUNT(1) FROM film) AS All_films, 
COUNT(1) AS Long_films
FROM film
WHERE `length` > (SELECT AVG(`length`) from film);
```

<details>
<summary>Скрин</summary>  

![img](https://github.com/travickiy67/sql-part2/blob/main/img/img2.1.png)  

</details>

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

```
SELECT DATE_FORMAT(payment_date, '%Y   %m') AS Дата, COUNT(payment_id) As Платежи, SUM(amount) AS Количество
FROM payment
GROUP BY DATE_FORMAT(payment_date, '%Y   %m')
ORDER BY COUNT(payment_id)  DESC
LIMIT 1;

```
<details>
<summary>Скрин</summary>  

![img](https://github.com/travickiy67/sql-part2/blob/main/img/img3.1.png)  

</details>


## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 4*

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

```
SELECT CONCAT(s.first_name, ' ', s.last_name) AS Сотрудники, COUNT(1) AS Продажи,
	CASE
		WHEN COUNT(1) > 8000 THEN 'Да'
		ELSE 'Нет'
	END AS Премия
FROM staff s  
JOIN payment p ON p.staff_id = s.staff_id 
GROUP BY p.staff_id;
```
<details>
<summary>Скрин</summary>  

![img](https://github.com/travickiy67/sql-part2/blob/main/img/img4.1.png)  

</details>

### Задание 5*

Найдите фильмы, которые ни разу не брали в аренду.

**Это решение есть в призентации.
```
SELECT f.title
FROM film f
LEFT JOIN inventory i ON i.film_id = f.film_id
LEFT JOIN rental r ON r.inventory_id = i.inventory_id
WHERE r.rental_id IS NULL;
```
<details>
<summary>Скрин</summary>  

![img](https://github.com/travickiy67/sql-part2/blob/main/img/img5.1.png)  

</details>
