# lab
Created: 2023-01-05 12:06
Tags: 
____

```sql

create table if not exists person (
	id bigserial primary key,
	created_at timestamp not null default now(),
	name varchar(128),
	age int
);

insert into person (name, age) values ('ahmadreza', 25);
insert into person (name, age) values ('reza', 27);
```

_____
##### References
1.

