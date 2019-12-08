## 读取数据：
1. 从数据库读取数据：
```python
def getDataFromSql(idcard, startDate, endDate):
    con = pymysql.connect(host='10.205.11.225', user='root',
                          passwd='2018@password', db='timecard', charset='utf8')
    sql = "select * from time_card where swipe_date between '{}' and '{}' and staff_id_no='{}'"
    sql = sql.format(startDate, endDate, idcard)
    return getTimcardData(pd.read_sql(sql, con))
```
2. 从csv，TAB文件读取数据：
```python
data = pd.read_csv(path, sep="\t", encoding="GBK")
data = pd.read_csv("./test.csv")
```
## 过滤数据
1. 分组和条件筛选：
```python
# 过滤并从新生成index
filterData = filterData[filterData.date_occurred >= startDateNo]
filterData = filterData[filterData['date_occurred'] <= endDateNo]
filterData = filterData.reset_index(drop=True)
```
2. 分组后过滤
```python
# 先分组取后取每组最小值
data1 = dataSet.groupby('swipe_date').apply(lambda t: t[
        t.swipe_time == t.swipe_time.min()]).reset_index(drop=True)
# 先分组取后取每组最大值
data2 = dataSet.groupby('swipe_date').apply(lambda t: t[
        t.swipe_time == t.swipe_time.max()]).reset_index(drop=True)
# 和并两组数据，并从新生成index
 data = pd.concat([data1, data2]).reset_index(drop=True)
# 取出每天的最早时间和昨晚时间。
```
3. 去重：
```python
 data = data.drop_duplicates("swipe_time")
```
4. 排序：
```python
data = data.sort_values(by=["swipe_time"]).reset_index(drop=True)
```
## 取值
```python
# index 行staff_name列数据
timecards.iloc[index]["staff_name"]
```
