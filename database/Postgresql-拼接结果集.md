```
select
  array_to_string(
    array(
      select something from table
    ),
    ','
  )
```
