## String 及文本
1. char
定长，最大只有256bytes。
存入内容长度大于指定长度，严格模式报错，否则截取定长报错。
存入内容小于指定长度，空位使用空格填充。
2. varchar
长度可变，最大需要计算。满足行的最大长度65535bytes。使用TEXT将会单独存储不会再行内。
存入内容小于指定长度，不做填充。

3. 长文本Text

|类型|bytes||
|--|--|--|
|TINYTEXT    |256 bytes        |~1kb|
|TEXT        |65,535 bytes       |	~64kb|
|MEDIUMTEXT	 |16,777,215 bytes|	~16MB
|LONGTEXT    |4,294,967,295 bytes	|~4GB|

> 需要做一个记录调用第三方接口request和response的记录。结果发现如果请求数据过多，数据库就会报错。。。

### 日期时间
`time` hh:mm:ss
`date` yyyy-mm-dd
`datetime` yyyy-mm-dd hh:mm:ss
`timestamp` yyyymmddhhmmss
`year` yyyy

### 关于mysql列类型转换
在MySQL中用来判断是否需要进行对据列类型转换的规则

  1、在一个数据表里，如果每一个数据列的长度都是固定的，那么每一个数据行的长度也将是固定的．

  2、只要数据表里有一个数据列的长度的可变的，那么各数据行的长度都是可变的．

  3、如果某个数据表里的数据行的长度是可变的，那么，为了节约存储空间，MySQL会把这个数据表里的固定长度类型的数据列转换为相应的可变长度类型．

例外：长度小于4个字符的char数据列不会被转换为varchar类型
