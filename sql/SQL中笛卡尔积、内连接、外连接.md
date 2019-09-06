# [【SQL】SQL中笛卡尔积、内连接、外连接的数据演示](https://www.cnblogs.com/nick-huang/p/4919178.html)



##  笛卡尔积

```
select * from t_user, t_address;
或
select * from t_user inner join t_address;
```

## 内连接

```
-- 例3.1
select * from t_user u, t_address a
where u.id = a.user_id;

-- 例3.2
select * from t_user u 
inner join t_address a
where u.id = a.user_id;

-- 例3.3
select * from t_user u
inner join t_address a on u.id = a.user_id;
```

