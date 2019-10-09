在update语句中不应该通过join来进行多表关联，而是要通过from来多表关联，如下：
```
update a
set value = 'test'
from b,c
where
a.b_id = b.id
and b.c_id = c.id
and a.key = 'test'
and c.value = 'test';
```

通过from来多表关联，而关联条件则是放到了where中，这样就可以达到我们想要的效果了。另外补充一句，对于set xxx = 'xxx'这个update的部分，是不可以在column字段前加上表前缀的，比如下边的写法就是有语法错误的：
```
update a
set a.value = 'test';
```
