转载：https://www.cnblogs.com/traditional/p/11967360.html

首先pandas中的Series对象有一个方法叫做unstack，调用一个具有二级索引的Series对象的unstack方法，会得到一个DataFrame对象。其索引就是Series对象的一级索引，列就是Series对象的二级索引。
```
print(df)

     姓名   科目   分数
0  古明地觉  语文   90
1  古明地觉  数学   95
2  古明地觉  英语   96
3  芙兰朵露  语文   87
4  芙兰朵露  数学   92
5  芙兰朵露  英语   98
6   琪露诺   语文   100
7   琪露诺   数学    9
8   琪露诺   英语   91
```

将"姓名"和"科目"设置为索引, 然后取出"分数"这一列, 得到对应的具有二级索引的 Series 对象  
two_level_index_series = df.set_index(["姓名", "科目"])["分数"]  
此时得到的Series就是一个具有二级索引的Series  
一级索引就是"姓名"这一列, 二级索引就是"科目"这一列  

```
print(two_level_index_series)

姓名       科目
古明地觉    语文     90
           数学     95
           英语     96
芙兰朵露    语文     87
           数学     92
           英语     98
琪露诺      语文    100
           数学      9
           英语     91
Name: 分数, dtype: int64
```

调用具有二级索引的Series的unstack, 会得到一个DataFrame  
并会自动把一级索引变成DataFrame的索引, 二级索引变成DataFrame的列

```
new_df = two_level_index_series.unstack()
print(new_df)

科目    数学  英语   语文
姓名               
古明地觉  95  96   90
琪露诺    9  91  100
芙兰朵露  92  98   87
```

是不是改回来了呢? 但是还有不完美的地方, 这个DataFrame的索引和列都有一个名字  
索引的名字叫"姓名", 列的名字叫"科目", 因为原来Series的两个索引就叫"姓名"和"科目"  
可以通过 rename_axis(index=, columns=) 来给坐标轴重命名  
new_df = new_df.rename_axis(columns=None)  

这里我们只给列重命名, 没有给索引重命名, 至于原因请往下看
```
new_df = new_df.reset_index()
print(new_df)

     姓名  数学  英语   语文
0  古明地觉  95  96   90
1   琪露诺   9   91  100
2  芙兰朵露  92  98   87
```

大功告成, 如果index的名字变为空的话, 那么reset_index之后, 列名就会变成index  
但如果原来的索引有名字, 那么reset_index之后, 列名就是原来的索引名

