# SQL-academy
Мои решенные задачи с сайта sql-academy, а также некоторые выжимки из учебного материала
___ 
Выведите имена first_name и фамилии last_name студентов из таблицы Student, у кого отсутствует отчество middle_name
~~~~sql
SELECT first_name,
	last_name
FROM Student
WHERE middle_name IS NULL
~~~~
___
Выведите резервации комнат (поля room_id, start_date, end_date) из таблицы Reservations, у которых итоговая стоимость аренды (поле total) находится в промежутке от 500 до 1200 включительно.
~~~~sql
SELECT room_id,
	start_date,
	end_date
FROM Reservations
WHERE total BETWEEN 500 AND 1200
~~~~
___
Выведите информацию о студентах из таблицы Student, у которых год рождения соответствует одному из перечисленных: 2000, 2002 и 2004.
~~~~sql
SELECT *
FROM Student
WHERE year(birthday) IN (2000, 2002, 2004)
~~~~
___
Найдите все жилые помещения (таблица Rooms), в адресе которых содержится строка «Avenue». В результирующей выборке выведите поля id и address.
~~~~sql
SELECT id,
	address
FROM Rooms
WHERE address LIKE "%Avenue%"
~~~~
___
Выведите name, email пользователей, чей адрес электронной почты заканчивается на «@outlook.com» или «@live.com».
~~~~sql
SELECT name,
	email
FROM Users
WHERE email REGEXP "@(outlook.com|live.com)$"
~~~~
___
Для каждого отдельного платежа выведите идентификатор товара и сумму, потраченную на него, в отсортированном по убыванию этой суммы виде. Список платежей находится в таблице Payments. 
Для вывода суммы используйте псевдоним sum.
~~~~sql
SELECT good,
	amount * unit_price AS sum
FROM Payments
ORDER BY sum DESC;
~~~~
___
Выведите все данные членов семьи с фамилией Quincey из таблицы FamilyMembers и отсортируйте их по возрастанию сначала по столбцу status, а затем по member_name.
~~~~sql
SELECT *
FROM FamilyMembers
WHERE member_name LIKE "%Quincey"
ORDER BY STATUS,
	member_name;
~~~~
___
Подсчитайте количество учеников в каждом классе, а также отсортируйте их по убыванию количества учеников. Принадлежность ученика к конкретному классу вы можете получить из таблицы Student_in_class. В качестве результата необходимо вывести идентификатор класса (поле class) и количество учеников в этом классе.
Для вывода количества учеников используйте псевдоним count.
~~~~sql
SELECT class,
	COUNT(*) AS COUNT
FROM Student_in_class
GROUP BY class
ORDER BY COUNT DESC;
~~~~
___
Для каждого из существующих статусов (поле status) найдите самого старого человека (используйте поле birthday). Выведите статус и дату рождения.
Для вывода даты рождения используйте псевдоним birthday.
~~~~sql
SELECT STATUS,
	min(birthday) AS birthday
FROM FamilyMembers
GROUP BY STATUS;
~~~~
___
Получите среднее время полётов, совершённых на каждой из моделей самолёта. Выведите поле plane и среднее время полёта в секундах.
Для вывода времени используйте псевдоним time.
Используйте функцию TIMESTAMPDIFF(second, time_out, time_in), чтобы получить разницу во времени в секундах между двумя датами.
~~~~sql
SELECT plane,
	avg(timestampdiff(SECOND, time_out, time_in)) AS time
FROM Trip
GROUP BY plane;
~~~~
___
Выведите идентификатор комнаты (поле room_id), среднюю стоимость за один день аренды (поле price, для вывода используйте псевдоним avg_price), а также количество резерваций этой комнаты (используйте псевдоним count). Полученный результат отсортируйте в порядке убывания сначала по количеству резерваций, а потом по средней стоимости.
~~~~sql
SELECT room_id,
	AVG(price) AS avg_price,
	COUNT(*) AS COUNT
FROM Reservations
GROUP BY room_id
ORDER BY COUNT DESC,
	avg_price DESC;
~~~~
___
![alt text](https://github.com/kkwicklss/SQL-academy/blob/main/IMG_7544.webp)
Выведите типы комнат (поле home_type) и разницу между самым дорогим и самым дешевым представителем данного типа. В итоговую выборку включите только те типы жилья, количество которых в таблице Rooms больше или равно 2.
Для вывода разницы стоимости используйте псевдоним difference.
~~~~sql
SELECT home_type,
	max(price) - min(price) AS difference
FROM Rooms
GROUP BY home_type
HAVING COUNT(*) >= 2;
~~~~
___
![alt text](https://github.com/kkwicklss/SQL-academy/blob/main/IMG_7559.jpeg)
Объедините таблицы Class и Student_in_class с помощью внутреннего соединения по полям Class.id и Student_in_class.class. Выведите название класса (поле Class.name) и идентификатор ученика (поле Student_in_class.student).
~~~~sql
SELECT Class.name,
	Student_in_class.student
FROM Class
	JOIN Student_in_class ON Class.id = Student_in_class.class;
~~~~
___
Дополните запрос из предыдущего задания, добавив ещё одно внутреннее соединение с таблицей Student. Объедините по полям Student_in_class.student и Student.id и вместо идентификатора ученика выведите его имя (поле first_name).
~~~~sql
SELECT Class.name,
	first_name
FROM Class
	JOIN Student_in_class ON Class.id = Student_in_class.class
	JOIN Student ON Student_in_class.student = Student.id;
~~~~
___
Выведите названия продуктов, которые покупал член семьи со статусом "son". Для получения выборки вам нужно объединить таблицу Payments с таблицей FamilyMembers по полям family_member и member_id, а также с таблицей Goods по полям good и good_id.
~~~~sql
SELECT good_name
FROM Goods
	JOIN Payments ON Goods.good_id = Payments.good
	JOIN FamilyMembers ON Payments.family_member = FamilyMembers.member_id
WHERE STATUS = "son";
~~~~
___
Выведите идентификатор (поле room_id) и среднюю оценку комнаты (поле rating, для вывода используйте псевдоним avg_score), составленную на основании отзывов из таблицы Reviews.
Данная таблица связана с Reservations (таблица, где вы можете взять идентификатор комнаты) по полям reservation_id и Reservations.id.
~~~~sql
SELECT room_id,
	AVG(rating) AS avg_score
FROM Reservations
	JOIN Reviews ON Reservations.id = Reviews.reservation_id
GROUP BY room_id;
~~~~
___
![alt text](https://github.com/kkwicklss/SQL-academy/blob/main/IMG_7560.jpeg)
___
![alt text](https://github.com/kkwicklss/SQL-academy/blob/main/IMG_7561.jpeg)
___
![alt text](https://github.com/kkwicklss/SQL-academy/blob/main/img.png)  
Выведите имя first_name и фамилию last_name каждого учителя из таблицы Teacher, а также количество занятий, в которых он был назначен преподавателем. Если преподаватель не был назначен ни на одно занятие, то выведите 0.
Для вывода количества занятий используйте псевдоним amount_classes.
~~~~sql
SELECT Teacher.first_name,
	Teacher.last_name,
	COUNT(Schedule.teacher) AS amount_classes
FROM Teacher
	LEFT JOIN Schedule ON Teacher.id = Schedule.teacher
GROUP BY Teacher.id;
~~~~
___
> Подзапрос — это запрос, использующийся в другом SQL запросе. Подзапрос всегда заключён в круглые скобки и обычно выполняется перед основным запросом.
Скалярный подзапрос - подзапрос, возвращающий одну строку и один столбец
___
Выведите всю информацию о пользователе из таблицы Users, кто является владельцем самого дорогого жилья (таблица Rooms).
~~~~sql
SELECT Users.*
FROM Users
	LEFT JOIN Rooms ON Users.id = Rooms.owner_id
WHERE price = (
		SELECT MAX(price)
		FROM Rooms
	);
~~~~
Если подзапрос возвращает более одной строки, его нельзя просто использовать с операторами сравнения, как это можно было делать со скалярными подзапросами.
Однако c подзапросами, возвращающими несколько строк и один столбец, можно использовать 3 дополнительных оператора. -> ALL, IN, ANY
Необходимо найти имена всех владельцев жилья, которые сами при этом никогда не снимали жилье. Чтобы получить данный список, мы можем действовать следующим образом:
Получить список имён всех владельцев жилья
~~~~sql
SELECT DISTINCT name FROM Users INNER JOIN Rooms
ON Users.id = Rooms.owner_id
~~~~
Получить список идентификаторов всех пользователей, снимавших жилье
~~~~sql
SELECT DISTINCT user_id FROM Reservations
~~~~
Отфильтровать первый список всех владельцев по условию, что идентификатор владельца жилья не равен ни одному из идентификаторов пользователей, когда-либо снимавших жилье
~~~~sql
SELECT DISTINCT name FROM Users INNER JOIN Rooms
    ON Users.id = Rooms.owner_id
    WHERE Users.id <> ALL (
        SELECT DISTINCT user_id FROM Reservations
    )
~~~~
~~~~sql
SELECT * FROM Users WHERE id IN (
    SELECT DISTINCT owner_id FROM Rooms WHERE price >= 150
)
~~~~
~~~~sql
SELECT * FROM Users WHERE id = ANY (
    SELECT DISTINCT owner_id FROM Rooms WHERE price >= 150
)
~~~~
___
Выведите названия товаров из таблицы Goods (поле good_name), которые ещё ни разу не покупались ни одним из членов семьи (таблица Payments).
~~~~sql
SELECT good_name
FROM Goods
WHERE Goods.good_id NOT IN (
		SELECT good
		FROM Payments
	);
~~~~
