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
___
Выведите список комнат (все поля, таблица Rooms), которые по своим удобствам (has_tv, has_internet, has_kitchen, has_air_con) совпадают с комнатой с идентификатором "11".
~~~~sql
SELECT *
FROM Rooms
WHERE (
		Rooms.has_tv,
		Rooms.has_internet,
		Rooms.has_kitchen,
		Rooms.has_air_con
	) IN (
		SELECT Rooms.has_tv,
			Rooms.has_internet,
			Rooms.has_kitchen,
			Rooms.has_air_con
		FROM Rooms
		WHERE id = 11
	);
~~~~
___
С помощью коррелированного подзапроса выведите имена всех членов семьи (member_name) и цену их самого дорогого купленного товара.
Для вывода цены самого дорогого купленного товара используйте псевдоним good_price. Если такого товара нет, выведите NULL.
~~~~sql
SELECT member_name,
	(
		SELECT MAX(unit_price)
		FROM Payments
		WHERE Payments.family_member = FamilyMembers.member_id
	) AS good_price
FROM FamilyMembers;
~~~~
___
~~~~sql
-- Пример использования конструкции WITH
WITH Aeroflot_trips AS
    (SELECT TRIP.* FROM Company
        INNER JOIN Trip ON Trip.company = Company.id WHERE name = "Aeroflot")

SELECT plane, COUNT(plane) AS amount FROM Aeroflot_trips GROUP BY plane;
~~~~
~~~~sql
WITH название_cte [(столбец_1 [, столбец_2 ] …)] AS (подзапрос)
    [, название_cte [(столбец_1 [, столбец_2 ] …)] AS (подзапрос)] …
~~~~
~~~~sql
WITH Aeroflot_trips AS
    (SELECT TRIP.* FROM Company
        INNER JOIN Trip ON Trip.company = Company.id WHERE name = "Aeroflot"),
    Don_avia_trips AS
    (SELECT TRIP.* FROM Company
        INNER JOIN Trip ON Trip.company = Company.id WHERE name = "Don_avia")

SELECT * FROM Don_avia_trips UNION SELECT * FROM  Aeroflot_trips;
~~~~
___
> Результаты выполнения SQL запросов можно объединять. Для этого существует оператор UNION.
~~~~sql
SELECT поля_таблиц FROM список_таблиц ...
UNION [ALL]
SELECT поля_таблиц FROM список_таблиц ... ;
~~~~
UNION по умолчанию убирает повторения в результирующей таблице. Для отображения с повторением есть необязательный параметр ALL.
* Не путайте операции объединения запросов с операциями объединения таблиц. Для этого служит оператор JOIN.
* Не путайте операции объединения запросов с подзапросами. Подзапросы выполняются для связанных таблиц.
  
Существует два других оператора, чьё поведение крайне схоже с UNION:
* INTERSECT Комбинирует два запроса SELECT, но возвращает записи только первого SELECT, которые имеют совпадения во втором элементе SELECT.
* EXCEPT Комбинирует два запроса SELECT, но возвращает записи только первого SELECT, которые не имеют совпадения во втором элементе SELECT.

Выведите полные имена (поля first_name, middle_name и last_name) всех студентов и преподавателей.
~~~~sql
SELECT Teacher.first_name,
	Teacher.middle_name,
	Teacher.last_name
FROM Teacher
UNION
SELECT Student.first_name,
	Student.middle_name,
	Student.last_name
FROM Student;
~~~~
___
Под условной логикой понимается наличие у программы нескольких путей выполнения в зависимости от каких-то условий.
Например, в базе данных «Расписание» есть таблица Student с полем birthday, отражающим дату рождения студента. Допустим, в выборке необходимо отобразить не саму дату рождения, а текстовое значение «Совершеннолетний» или «Несовершеннолетний» в зависимости от того, есть ли студенту 18 лет. Это и есть пример условной логики, при которой должно вывестись либо одно значение, либо другое в зависимости от конкретного условия.
~~~~sql
SELECT first_name, last_name,
CASE
  WHEN TIMESTAMPDIFF(YEAR, birthday, NOW()) >= 18 THEN "Совершеннолетний"
  ELSE "Несовершеннолетний"
END AS status
FROM Student
~~~~
~~~~sql
CASE
    WHEN условие_1 THEN возвращаемое_значение_1
    WHEN условие_2 THEN возвращаемое_значение_2
    WHEN условие_n THEN возвращаемое_значение_n
    [ELSE возвращаемое_значение_по_умолчанию]
END
~~~~
Если условие_1 возвращает истинное значение, то выражение CASE вернёт возвращаемое_значение_1, иначе будет сделана проверка на условие_2 и т.д. Если ни одно из предложенных условий не будет выполнено, то вернётся NULL или возвращаемое_значение_по_умолчанию, если была использована конструкция ELSE.
~~~~sql
SELECT name,
CASE
  WHEN SUBSTRING(name, 1, INSTR(name, ' ')) IN (10, 11) THEN "Старшая школа"
  WHEN SUBSTRING(name, 1, INSTR(name, ' ')) IN (5, 6, 7, 8, 9) THEN "Средняя школа"
  ELSE "Начальная школа"
END AS stage
FROM Class
~~~~
___
Из таблицы Reviews выведите идентификаторы отзывов (поле id) и их категорию: для рейтинга 4-5 проставьте категорию «positive», для 3 проставьте «neutral», а для 1-2 - «negative».
Для вывода категории рейтинга используйте псевдоним rating.
~~~~sql
SELECT id,
    CASE
		WHEN rating IN (4, 5) THEN "positive"
		WHEN rating = 3 THEN "neutral"
		ELSE "negative"
	END AS rating
FROM Reviews;
~~~~
___
~~~~sql
SELECT id, price,
    IF(price >= 150, "Комфорт-класс", "Эконом-класс") AS category
    FROM Rooms
~~~~
~~~~sql
SELECT id, price,
    IF(price >= 200, "Бизнес-класс",
        IF(price >= 150,
            "Комфорт-класс", "Эконом-класс")) AS category
    FROM Rooms
~~~~
~~~~sql
IFNULL(значение, альтернативное_значение);
~~~~
~~~~sql
SELECT IFNULL("SQL Academy", "Альтернатива SQL Academy") AS sql_trainer;
~~~~
___
Из таблицы Rooms выведите идентификаторы сдаваемых жилых помещений (поле id) и наличие телевизора в помещении: если телевизор присутствует выведите «YES», иначе «NO». Для вывода наличия телевизора используйте псевдоним has_tv.
~~~~sql
SELECT id,
	IF(has_tv = 1, "YES", "NO") AS has_tv
FROM Rooms;
~~~~
___
Из таблицы Teacher выведите имена (поле first_name), отчества (поле middle_name) и фамилии (поле last_name) учителей. Если отчество у учителя отсутствует, выведите в поле middle_name значение «Empty».
~~~~sql
SELECT first_name,
	middle_name,
	last_name,
	IFNULL(middle_name, "Empty") AS middle_name
FROM Teacher;
~~~~
___
Для добавления новых записей в таблицу предназначен оператор INSERT.
~~~~sql
INSERT INTO имя_таблицы [(поле_таблицы, ...)]
VALUES (значение_поля_таблицы, ...)
| SELECT поле_таблицы, ... FROM имя_таблицы ...
~~~~
___
Добавьте новый товар в таблицу Goods с именем «Table» и типом «equipment».
В качестве первичного ключа (good_id) укажите количество записей в таблице + 1.
~~~~sql
INSERT INTO Goods
SELECT COUNT(*) + 1,
	'Table',
	(
		SELECT good_type_id
		FROM GoodTypes
		WHERE good_type_name = 'equipment'
	)
FROM Goods;
~~~~
___
Для редактирования существующих записей в таблицах существует SQL оператор UPDATE.
~~~~sql
UPDATE имя_таблицы
SET поле_таблицы1 = значение_поля_таблицы1,
    поле_таблицыN = значение_поля_таблицыN
[WHERE условие_выборки]
~~~~
> Будьте внимательны, когда обновляете данные. Если вы пропустите оператор WHERE, то будут обновлены все записи в таблице.
В запросах на обновление данных можно менять значения, опираясь на предыдущее значение.
~~~~sql
UPDATE Payments
SET unit_price = unit_price * 2;
~~~~
___
Измените имя у "Wednesday Addams" на новое "Tuesday Addams".
~~~~sql
UPDATE FamilyMembers
SET member_name = "Tuesday Addams"
WHERE member_name = "Wednesday Addams";
~~~~
___
Обновите стоимость всех комнат в таблице (Rooms), добавив к текущей 10 единиц
~~~~sql
UPDATE Rooms
SET price = price + 10;
~~~~
___
Время от времени возникает задача удаления записей из таблицы. Для этого в SQL предусмотрены операторы DELETE и TRUNCATE, из которых наиболее универсальным и безопасным является первый вариант.
~~~~sql
DELETE FROM имя_таблицы
[WHERE условие_отбора_записей];
~~~~
Если условие отбора записей WHERE отсутствует, то будут удалены все записи указанной таблицы.
Эту же операцию (удаления всех записей) можно сделать также с помощью оператора TRUNCATE. Он выполнит удаление таблицы и пересоздаст её заново - этот вариант работает гораздо быстрее, чем удаление всех записей одна за другой (как в случае с DELETE) особенно для больших таблиц.
~~~~sql
TRUNCATE TABLE имя_таблицы;
~~~~
Если в DELETE запросе используется JOIN, то необходимо указать из каких(ой) именно таблиц(ы) требуется удалять записи.
~~~~sql
DELETE имя_таблицы_1 [, имя_таблицы_2] FROM
имя_таблицы_1 JOIN имя_таблицы_2
ON имя_таблицы_1.поле = имя_таблицы_2.поле
[WHERE условие_отбора_записей];
DELETE Reservations FROM
Reservations JOIN Rooms ON
Reservations.room_id = Rooms.id
WHERE Rooms.has_kitchen = false;
~~~~
Если бы, помимо удаления бронирования, нам нужно было также удалить и жилье, то запрос приобрёл бы следующий вид:
~~~~sql
DELETE Reservations, Rooms FROM
Reservations JOIN Rooms ON
Reservations.room_id = Rooms.id
WHERE Rooms.has_kitchen = false;
~~~~
___
Удалите все записи из таблицы Payments, используя оператор DELETE.
~~~~sql
DELETE FROM Payments;
~~~~
___
Удалить запись из таблицы Goods, у которой поле good_name равно "milk"
~~~~sql
DELETE FROM Goods
WHERE good_name = 'milk';
~~~~
___
Измените запрос так, чтобы удалить товары (Goods), имеющие тип деликатесов (delicacies).
~~~~sql
DELETE Goods
FROM Goods
	JOIN GoodTypes ON Goods.type = GoodTypes.good_type_id
WHERE GoodTypes.good_type_name = "delicacies";
~~~~
___
## Продвинутый SQL
|  Имя функции  |  	        Описание	                 |
|---------------|------------------------------------------------|
|POW(num, power)|Вычисляет число в указанной степени             |
|SQRT(num)      |Вычисляет квадратный корень числа               |
|LOG(base, num) |Вычисляет логарифм числа по указанному основанию|
|EXP(num)       |Вычисляет e^num				 |
|SIN(num)       |Вычисляет синус числа				 |
|COS(num)       |Вычисляет косинус числа			 |
|TAN(num)       |Вычисляет тангенс числа			 |

Для округления числовых данных в SQL предусмотрены следующие 4 функции: CEIL, FLOOR, ROUND, TRUNCATE.

Функции CEIL, FLOOR направлены на то, чтобы округлять число к ближайшему целому числу в большую и в меньшую сторону соответственно.
~~~~sql
SELECT CEILING(69.69) AS ceiling, FLOOR(69.69) AS floor;
~~~~
Для округления к ближайшему целому числу есть функция ROUND, которая любое число, десятичная часть которого больше или равна 0.5, округляет в большую сторону, иначе в меньшую.
~~~~sql
SELECT ROUND(69.499), ROUND(69.5), ROUND(69.501);
~~~~
~~~~sql
SELECT ROUND(1691.7,-1), ROUND(1691.7,-2), ROUND(1691.7,-3);
~~~~
Функция TRUNCATE аналогична функции ROUND, она также способна принимать 2-й необязательный параметр, только вместо округления она просто отбрасывает ненужные цифры.
~~~~sql
SELECT TRUNCATE(69.7979,1), TRUNCATE(69.7979,2), TRUNCATE(69.7979,3);
~~~~
Функция SIGN возвращает значение -1, если число отрицательно, 0, если число нулевое и 1, если число положительное.
Функция ABS возвращает абсолютное значение числа
___
Выведите список цен всего доступного жилья (из таблицы Rooms), округлённых к ближайшему кратному 10-ти числу. Например, если цена равна "9676", то после округления она будет равна "9680".

Для вывода цены используйте псевдоним rounded_price.
~~~~sql
SELECT ROUND(price, -1) AS rounded_price
FROM Rooms;
~~~~
___
~~~~sql
SELECT  CAST("2022-06-16 16:37:23" AS DATETIME) AS datetime_1,
        CAST("2014/02/22 16*37*22" AS DATETIME) AS datetime_2,
        CAST("20220616163723" AS DATETIME) AS datetime_3,
        CAST("2021-02-12" AS DATE) AS date_1,
        CAST("160:23:13" AS TIME) AS time_1,
        CAST("89" AS YEAR) AS year

SELECT STR_TO_DATE('November 13, 1998', '%M %d, %Y') AS date;

SELECT CURDATE(), CURTIME(), NOW();
~~~~
___
|Тип|Формат по умолчанию|
|---|---|
|DATE|YYYY-MM-DD|
|DATETIME|YYYY-MM-DD hh:mm:ss|
|TIMESTAMP|YYYY-MM-DD hh:mm:ss|
|TIME|hhh:mm:sss|
|YEAR|YYYY - полный формат YY или Y - сокращённый формат, который возвращает год в пределах 2000-2069 для значений 0-69 и год в пределах 1970-1999 для значений 70-99|

|Критерий|DATETIME|TIMESTAMP|
|Диапазон|от 1000-01-01 00:00:00 
до 9999-12-31 23:59:59||
