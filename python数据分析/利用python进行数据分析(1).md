# 第一章
一. 数据的类型
1. 表格数据
2. 多维数据(矩阵)
3. 多张表数据(主外键关联)
4. 时间序列

二. 重要的Python库
1. NumPy 基础数据结构和函数
2. pandas 高级数据结构和函数
3. matplotlib 二维数据可视化
4. IPython和Jupyter 交互
5. Scipy 科学计算领域
6. scikit-learn 机器学习包
7. statsmodels 统计分析包
# 第二章 Python基础 略
# 第三章 NumPy
[菜鸟教程比书详细](https://www.runoob.com/numpy/numpy-tutorial.html)
#第四章 pandas
[易百教程](https://www.yiibai.com/pandas/python_pandas_environment_setup.html)
1. 最简单数据清理：
```python
import pandas as pd

df = pd.read_csv("./API_CHN_DS2_zh_csv_v2_10578406.csv", ',')
print("清除删除无用列")
df.pop("Country Name")
df.pop("Unnamed: 63")
df.pop("2018")

print("清除部分无用数据")
new_df = df.dropna(thresh=61) //确保每条数据至少61
print("写入到文件")
new_df.to_csv("./test.csv", index=False)
```
如上：最简单的数据清理，只取完整的数据。

