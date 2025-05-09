# pr_09
# Геопространственный анализ
# Цель
Научиться создавать временные таблицы, работать с ними, а также строить карту клиентов и сервисных центров в облачной визуализации Yandex DataLence.
# Задание
Определение ближайшего дилерского центра для каждого клиента. Маркетологи компании пытаются повысить вовлеченность клиентов, необходимо им помочь найти ближайший дилерский центр. Разработчикам продукта также интересно знать, каково среднее расстояние между каждым клиентом и ближайшим к нему дилерским центром.
# Ход работы 
1.Проверить наличие геопространственных данных в базе данных.

2.Создать временную таблицу с координатами долготы и широты для каждого клиента.

3.Создать аналогичную таблицу для каждого дилерского центра.

4.Соединить эти таблицы, чтобы рассчитать расстояние от каждого клиента до каждого дилерского центра (в киллометрах).

5.Определить ближайший дилерский центр для каждого клиента.

6.Провести выгрузку полученного результата из временной таблицы в CSV.

7.Построить карту клиентов и сервисных центров в облачной визуализации Yandex DataLence.

8.Удалить временные таблицы
# 1. Создаем временную таблицу с координатами долготы и широты для каждого клиента.
```
create temp table customer_points as(
select customer_id, 
point(longitude, latitude) as lng_lat_point
from customers
where longitude is not null
and latitude is not null);
select * from customer_points
```
![image](https://github.com/user-attachments/assets/f1f38881-17f6-4ce0-a917-3008e2c51dea)
# 2. Создаем аналогичную таблицу для каждого дилерского центра.
```
create temp table dealership_points as(
select dealership_id,
point(longitude, latitude) as lng_lat_point
from dealerships);
select * from dealership_points
```
![image](https://github.com/user-attachments/assets/8222df3a-ce00-42b9-be6b-7df54d43f171)
# 3. Соединяем эти таблицы, чтобы рассчитать расстояние от каждого клиента до каждого дилерского центра.
```
create temp table customer_dealership_distance as(
select customer_id,
dealership_id,
c.lng_lat_point <@> d.lng_lat_point as distance
from customer_points c
cross join dealership_points d);
select * from customer_dealership_distance
```
![image](https://github.com/user-attachments/assets/269eada9-2b12-4791-adb0-399c9194617a)

# 4. Определить ближайший дилерский центр для каждого клиента
```
create temp table closest_dealerships as(
select distinct on (customer_id)
customer_id,
dealership_id,
distance
from customer_dealership_distance
order by customer_id, distance);
select * from closest_dealerships
```
![image](https://github.com/user-attachments/assets/fdaf9116-f039-44a9-a956-57a6f44a3b2d)

# 5. Провести выгрузку полученного результата из временной таблицы в CSV
![image](https://github.com/user-attachments/assets/e05edaa7-36b7-4d5a-af1a-a39c15110260)
# 6. Построить карту клиентов и сервисных центров в облачной визуализации Yandex DataLence
# 7.
Подключаемся к базе данных Postgres. Создаем датасет и добавляем туда таблицы customers, dealerships и sales. Далее добавляем поля customers и dealerships.Создаем чарт (тип чарта карта).Добавляем два слоя customers и dealerships и создаем тултипы.
![image](https://github.com/user-attachments/assets/53491947-da81-42f7-90d5-324361818f2b)
# Вывод
Научились создавать временные таблицы, выполнили запросы и постороили карту клиентов и сервисных центров в облачной визуализации Yandex DataLence.




