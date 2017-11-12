# sql-queries
this repository contains complex sql queries.

#1.(Join using two tables)
use sakila;
select first_name , last_name , email  from customer as c inner join address as a
#on c.address_id = a.address_id where a.city_id = 312;

#2.(Join using more then two tables)
use sakila;
select ffc.film_id ,  title , description , release_year , rating , special_features , c.name , c.category_id from category as c inner join 
(select f.film_id , title , description , release_year , rating , category_id, special_features from film as f inner join film_category as fc 
on f.film_id = fc.film_id) as ffc
on c.category_id = ffc.category_id
where c.name ="Comedy" ;

#3.
SELECT actor_id , sfa.film_id , title , description , release_year  FROM sakila.film_actor as sfa
inner join film as f 
on sfa.film_id = f.film_id
where actor_id = 5 ;

#4.
SELECT customer_id , first_name , last_name , email from customer as c
inner join 
(SELECT store_id , a.address_id , city_id from store as s 
inner join address as a 
on s.address_id = a.address_id
where store_id = 1) as temp
on c.address_id = temp.address_id
where temp.city_id IN (1,42,312,459)

#5.
use sakila;
select title , description , release_year, rating , special_features  from film as f 
inner join film_actor as fa
on f.film_id = fa.film_id
where actor_id = 15  and special_features LIKE '%Behind the Scenes' 

#6.
SELECT c.actor_id , c.first_name , c.last_name , newTemp.film_id , newTemp.title from actor as c
inner join 
(SELECT f.film_id , title , actor_id from film as f
inner join
(SELECT actor_id ,fa.film_id FROM film_actor as fa
where fa.film_id = 369) as temp
on f.film_id = temp.film_id
where f.film_id = '369') as newTemp
on c.actor_id = newTemp.actor_id

#7.
use sakila; 
select tempnewnew.film_id , tempnewnew.description , tempnewnew.title , tempnewnew.inventory_id , tempnewnew.rental_id , temppayment.amount
FROM
(select tempnew.film_id , tempnew.description , tempnew.title , tempnew.inventory_id , rental_id from
(SELECT a.film_id , a.description , a.title , inven.inventory_id from (
select ffc.film_id ,  title , description , release_year , rating , special_features , c.name , c.category_id from category as c inner join 
(select f.film_id , title , description , release_year , rating , category_id, special_features from film as f inner join film_category as fc 
on f.film_id = fc.film_id) as ffc
on c.category_id = ffc.category_id
where c.name ="Drama") as a inner join 
(select * from inventory ) as inven
on a.film_id = inven.film_id) as tempnew
inner join (select * from rental) as r
on tempnew.inventory_id = r.inventory_id) as tempnewnew
inner join (select * from payment) as temppayment
on temppayment.rental_id = tempnewnew.rental_id
where temppayment.amount =2.99
