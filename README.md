﻿# za
EF相关
1. 谨慎在where里用Contains，linq2sql时会被转换成in，not in。当数据量大时会太慢。用Any替代，会被转换成exists, not exists。详见：
http://www.dbatodba.com/sql-server/how-tos/typical-solutions-to-avoid-using-not-in-on-sql-server/
https://stackoverflow.com/questions/33545439/ef-and-not-exists-select-1-with-entity-framework
2. 使用lazy loading时小心N+1问题。\
问题用法：
```
var galaxy = db.Galaxies.Find("yinhe");
foreach(var star in galaxy.Stars.ToList())
{
  print(star.Name);
}
```
Eagerly Loading:
```
var galaxy = db.Galaxies.Include(g => g.Stars).SingleOrDefault(g => g.Name.equals("yinhe"));
foreach(var star in galaxy.Stars.ToList())
{
  print(star.Name);
}
```
