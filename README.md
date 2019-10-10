# 杂记
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

神奇的东西
1. 知乎上看到的金额转换成人民币读法：\
作者：苏博\
链接：https://www.zhihu.com/question/37926400/answer/74142424 \
来源：知乎\
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。\
```
public static String ConvertToChinese(Decimal number)
{
    var s = number.ToString("#L#E#D#C#K#E#D#C#J#E#D#C#I#E#D#C#H#E#D#C#G#E#D#C#F#E#D#C#.0B0A");
    var d = Regex.Replace(s, @"((?<=-|^)[^1-9]*)|((?'z'0)[0A-E]*((?=[1-9])|(?'-z'(?=[F-L\.]|$))))|((?'b'[F-L])(?'z'0)[0A-L]*((?=[1-9])|(?'-z'(?=[\.]|$))))", "${b}${z}");
    var r = Regex.Replace(d, ".", m => "负元空零壹贰叁肆伍陆柒捌玖空空空空空空空分角拾佰仟万亿兆京垓秭穰"[m.Value[0] - '-'].ToString());
    if (r.EndsWith("元"))//这两行是我加的
        r += "整";//感觉我拉低了前边代码的逼格……很惭愧
    return r;
}
```

其他
1. -int.MinValue 的值还是int.MinValue \
int.MinValue == -2^31 \
int.MaxValue == 2^31-1 \
-int.MinValue == 2^31 == int.MaxValue + 1, Overflows and become int.MinValue \
