## 简介

 &#91; [官方文档](https://pendulum.eustace.io/docs/)  &#93;		&#91; [Github](https://github.com/sdispater/pendulum) &#93;

> Pendulum 是一个用于简化日期时间操作的 Python 包
>
> 它提供的类是本机类的直接替换（它们从它们继承）
>
> 已特别注意确保正确处理时区，并基于底层 `tzinfo` 实现。例如，所有比较都在 `UTC` 或使用的日期时间的时区中完成
>
> 默认时区，除了使用 `now()` 时，方法将始终为 `UTC`

## 安装

```bash
$ pip install pendulum
# or
$ poetry add pendulum
```

## 方法

引入 `pendulum`

```python
import pendulum as pdl
```

### 获取日期/时间

```python
# 当前日期时间
now = pdl.now()

today = pdl.today()
tomorrow = pdl.tomorrow('Asia/Shanghai')
yesterday = pdl.yesterday().date()
# 相差时间
print(today.diff(yesterday).in_hours())
# output : 24

# 设定时间
dt1 = pdl.datetime(2022, 5, 1, tz='Asia/Shanghai')
dt2 = pdl.local(2022, 5, 1) # 等同于datetime(..., tz='local')
```

Pendulum 强制执行`时区`感知日期时间，使用它们是使用库的首选和推荐方式。但是，如果真的需要一个简单的 DateTime 对象，那么可以使用 naive() 助手

```python
navie = pdl.navie(2022, 5, 1)
print(naive.timezone)
# None
```

**例子**

```python
import pendulum as pdl

now = pdl.now()
now_london = pdl.now('Europe/London')

print(now_london.timezone_name)
# 'Europe/London'

week_start = now.start_of("week").to_date_string()
week_end = now.end_of("week").to_date_string()
print(f'周一{week_start}，周末{week_end }')
```

### 字符串格式化

```python
dt = pdl.now().to_datetime_string()
# '1975-12-25 14:15:16'
date = pdl.now().to_date_string()
# '1975-12-25'
time = pdl.now().to_time_string()
# '14:15:16'
fmd = pdl.now().to_formatted_date_string()
# 'Dec 25, 1975'
ddt = pdl.now().to_day_datetime_string()
# 'Thu, Dec 25, 1975 2:15 PM'
fmat = pdl.now().format('dddd Do [of] MMMM YYYY HH:mm:ss A')
# 'Thursday 25th of December 1975 02:15:16 PM'
```

自定义字符串格式化推荐使用 `format()` 而非 `strftime()`

### 格式化

**from_format**


```python
dt = pdl.from_format('2022-05-01 16', 'YYYY-MM-DD HH', tz='Asia/Shanghai')
print(dt)
# '2022-05-01T16:00:00+00:00'
```

**from_timestamp**

```python
dt = pdl.from_timestamp(-1, tz='Europe/London')
# '1970-01-01T00:59:59+01:00'
```

**instance**

```python
dt = datetime(2022, 1, 1)
p = pdl.instance(dt)
print(p)
# 2022-01-01T00:00:00+00:00
```

### 解析时间

该库本身支持 `RFC 3339` 格式、大多数 `ISO 8601` 格式和其他一些常用格式。

#### RFC 3339



```python
dtp = pdl.parse('2019-12-12')
print(dtp)
# 2019-12-12T00:00:00+00:00
```

### 属性

```python
init = pdl.now()

print(init.year)
print(init.month)
print(init.day)
print(init.hour)
print(init.minute)
print(init.second)
```

### 时间加减

```python
now = pdl.now()
print(now)
# 2021-10-12T14:15:22.083355+08:00

print(now.add(years=1))  # 加
# 2022-10-12T14:15:22.083355+08:00

print(now.subtract(years=1))  # 减
# 2020-10-12T14:15:22.083355+08:00

print(now.diff(now.add(years=1)).in_years())  # 时间跨度计算
# 1

pdl.set_locale('zh')
print(pdl.now().subtract(days=1).diff_for_humans())  # 人性化输出
#  1天前

print(pdl.now().subtract(hours=1).diff_for_humans())
# 1小时前
```

### 时间序列

```python
period = pdl.period(pdl.now(), pdl.now().add(days=3))

# years, months, weeks, days, hours, minutes and seconds
for dt in period.range('days'):
    print(dt)

# 2021-10-12T14:23:57.691088+08:00
# 2021-10-13T14:23:57.691088+08:00
# 2021-10-14T14:23:57.691088+08:00
# 2021-10-15T14:23:57.691088+08:00
```

