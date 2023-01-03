# django select_for_update
Created: 2023-01-03 04:06
Tags: 
____
The select_for_update method offered by the Django ORM solves the problem of concurrency by returning a queryset that locks all the rows that belong to this queryset until the outermost transaction it is inside gets committed thus preventing data corruption.


https://www.sankalpjonna.com/learn-django/managing-concurrency-in-django-using-select-for-update


_____
##### References
1.

