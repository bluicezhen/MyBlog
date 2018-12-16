---
title: Django SQL 初级性能优化
date: 2018-12-16 19:28:31
tags: 
- Django
- SQL
- MYSQL
- Python
summary:  最近开始使用Django，Django的易用性是非常棒的，功能齐全令人惊讶。可与算是简单的CMS了而不是一个Web Freamwork了，但是大量的封装也屏蔽了几乎所有的内部细节
---

> 本文中使用`Python 2.7.15`、`Django 1.8.19`、`MariaDB 5.5.62`，本文设计大量Django生成的SQL，为方便阅读，对SQL进行了部分简化。

最近开始使用Django，Django的Model功能性非常简单，可以完全不关心SQL，与另一个流行的Python ORM工具SQLAlchyma形成强烈的对比。但是如果只使用Django Model的简单特性，其SQL性能是惨不忍睹的。所幸的是，Django还是提供了一些进阶工具给大家使用。

如果想优化Django的SQL性能，第一步是打开SQL日志，打开SQL日志需要在`serrings.LOGGING.loggers`字典中加如如下字典项：

```python
'django.db.backends': {
    'handlers': ['console'],
    'propagate': True,
    'level':'DEBUG',
},
```

在本例中，创建两个简单的模型如下：

```python
class City(models.Model):
    name = models.CharField(max_length=100)

class Person(models.Model):
    name = models.CharField(max_length=100)
    city = models.ForeignKey(City, related_name="person")
```

并在MySQL中写入如下测试数据：

```
demo_city表
+----+----------+
| id | name     |
+----+----------+
| 1  | ShenZhen |
| 2  | BeiJing  |
+----+----------+
demo_person表
+----+--------+---------+
| id | name   | city_id |
+----+--------+---------+
| 1  | Tom    | 2       |
| 2  | Jerry  | 2       |
| 3  | Snow   | 2       |
| 4  | Frank  | 2       |
| 5  | Bluice | 2       |
| 6  | Dave   | 1       |
| 7  | Alen   | 1       |
| 8  | Iris   | 1       |
| 9  | Ella   | 1       |
| 10 | Eva    | 1       |
+----+--------+---------+
```

如果我们要获取`id=1`的用户的城市信息，我们一般很容易写出如下的代码。

```python
person = Person.objects.filter(id=1).first()
print person.city.name
```

虽然我们看起来只做了一次查询，此时请求了两次数据库：

```sql
SELECT * FROM `demo_person` WHERE `demo_person`.`id` = 1 LIMIT 1;
SELECT * FROM `demo_city` WHERE `demo_city`.`id` = 2 LIMIT 1
```

如果我们要打印所有的人和城市的列表，用一个循环可以很好的解决：

```Python
persons = Person.objects.all()
for person in persons:
    print person.name, person.city.name
```

我们不幸的请求了11次数据库：

```sql
SELECT * FROM `demo_person`;
SELECT * WHERE `demo_city`.`id` = 2;
SELECT * WHERE `demo_city`.`id` = 2;
SELECT * WHERE `demo_city`.`id` = 2;
SELECT * WHERE `demo_city`.`id` = 2;
SELECT * WHERE `demo_city`.`id` = 2;
SELECT * WHERE `demo_city`.`id` = 1;
SELECT * WHERE `demo_city`.`id` = 1;
SELECT * WHERE `demo_city`.`id` = 1;
SELECT * WHERE `demo_city`.`id` = 1;
SELECT * WHERE `demo_city`.`id` = 1;
```

而且其中有相当多的完全一样的SQL，列表查询中这是一个不可忽视的性能杀手。熟悉SQL应该了解，对多个关联表进行查询可以使用`join`关键字，在Django中我们可以使用`.select_related()`方法：

```python
persons = Person.objects.select_related("city").all()
for person in persons:
    print person.name, person.city.name
```

几乎和原来一模一样的代码，这次只生成了一个SQL：

```sql
 SELECT `* FROM `demo_person` 
 INNER JOIN `demo_city` ON ( `demo_person`.`city_id` = `demo_city`.`id` );
```

> `.select_related()`通常情况下可以不加参数，Django的“懒查询”机制会自动会自动处理关联的表，但是有时（尤其是多表递归关联和Django多对多关系）会无效，需要手动处理。[点击查看详情](https://docs.djangoproject.com/en/2.1/ref/models/querysets/#select-related)

上面的的还有一种预取查询的方案，写法如下：

```python
person = Person.objects.prefetch_related('city').all()
for person in persons:
	print person.name, person.city.name
```

生成的SQL如下：

```sql
SELECT * FROM `demo_person`;
SELECT * FROM `demo_city` WHERE `demo_city`.`id` IN (1, 2);
```

> prefetch_related可以控制“子查询”的行为，[点击查看详情](https://docs.djangoproject.com/en/2.1/ref/models/querysets/#prefetch-related)

和子查询的语法非常类似，性能应该也类似于子查询，也大大减少了查询的数量。组合这两种方法，可以获得数倍的性能提升。尤其是在查询

虽然`join`、`in`在复杂查询时也是性能杀手，但比起循环查询数据库带来的IO开销，就是小巫见大巫了。