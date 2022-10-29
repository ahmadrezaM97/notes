# good sql
Created: 2022-10-29 12:24
Tags: 
____

```sql
select distinct city from station where mod(id, 2) =  0;
select count(city) - count(distinct city) from station;

select city, length(city) city_len from station order by city_len asc, city asc limit 1;
select city, length(city) city_len from station order by city_len desc, city asc limit 1;

select distinct city from station where substr(city, 1, 1) in ('A', 'E', 'I', 'O', 'U');

```


_____
##### References
1.

