用一个sql分组查询各分组内的前n项

```
select 
  * 
from (
  select 
    i_name, rank, 
    row_number() over(partition by i_name) as row from t1
) t
where 
  row < = n
```
```
select 
  * 
from (
  select 
    i_name, rank, 
    row_number() over(partition by i_name order by i_name) as row from t1
) t
where 
  row < = n
```
