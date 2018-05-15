---
title: python 应用实例
date: 2018-05-02 21:33:43
tags: [python, 笔记]
categories: [python]
---

1. 如何从列表中随机抽取元素

   ```python
   import random
   print random.sample([1, 2, 3, 4, 5], 3) #不重复抽样3个元素
   ```

2. Anaconda3安装报错，failed to create anacoda menu，解决办法：
    http://blog.csdn.net/lixiangyong123/article/details/55816168	

3. spyder复制上一行代码到下一行的快捷键：ctrl+alt+'向下的箭头'

4. python远程调试：
    https://www.embeddedartists.com/sites/default/files/support/com/iMX_Developing_with_Python.pdf
    	http://o-u-u.com/?p=2015&pn=%E5%9C%A8eclipse%E4%B8%AD%E4%BD%BF%E7%94%A8pydev%E6%8F%92%E4%BB%B6%E8%BF%9B%E8%A1%8C%E8%BF%9C%E7%A8%8B%E8%B0%83%E8%AF%95python&cat=%E9%A6%96%E9%A1%B5-2%2Feclipse

    <!--more-->

5. elipse调试快捷键：
    单步跳入:step into调试，进入语句中的子函数进行步进调试
    	单步跳过:step over调试，跳过该行语句，进入下一行进行调试
    	单步返回:step out调试，跳出当前函数，进入调用源进行调试
    	全局 单步返回 F7 
    	全局 单步跳过 F6 
    	全局 单步跳入 F5 
    	全局 单步跳入选择 Ctrl+F5 
    	全局 调试上次启动 F11 
    	全局 继续 F8 
    	全局 使用过滤器单步执行 Shift+F5 
    	全局 添加/去除断点 Ctrl+Shift+B 
    	全局 显示 Ctrl+D 
    	全局 运行上次启动 Ctrl+F11 
    	全局 运行至行 Ctrl+R 
    	全局 执行 Ctrl+U

6. python3 sorted 多属性排序技巧

   ```python
   a = [['chr10', 'refSeq', 'CDS', '95372483', '95372962', '.', '+', '0', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95380389', '95380541', '.', '+', '0', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95380648', '95380737', '.', '+', '0', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95381689', '95381829', '.', '+', '0', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95385332', '95385406', '.', '+', '0', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95386397', '95386461', '.', '+', '0', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95386562', '95386628', '.', '+', '2', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95389015', '95389062', '.', '+', '0', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95394515', '95394664', '.', '+', '0', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95395254', '95395397', '.', '+', '0', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95396752', '95396820', '.', '+', '0', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95399827', '95399973', '.', '+', '0', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95400207', '95400314', '.', '+', '0', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95400677', '95400786', '.', '+', '0', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95405717', '95405804', '.', '+', '2', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95415517', '95415617', '.', '+', '0', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95418658', '95418765', '.', '+', '2', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95418861', '95418924', '.', '+', '2', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95421816', '95421890', '.', '+', '0', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95422317', '95422400', '.', '+', '0', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95422785', '95422935', '.', '+', '0', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '95425117', '95425175', '.', '+', '1', '6204'],
   	 ['chr10', 'refSeq', 'CDS', '13320301', '13320354', '.', '-', '0', '6214'],
   	 ['chr10', 'refSeq', 'CDS', '13322976', '13323110', '.', '-', '0', '6214'],
   	 ['chr10', 'refSeq', 'CDS', '13325690', '13325839', '.', '-', '0', '6214'],
   	 ['chr10', 'refSeq', 'CDS', '13330360', '13330541', '.', '-', '1', '6214'],
   	 ['chr10', 'refSeq', 'CDS', '13333831', '13333912', '.', '-', '0', '6214'],
   	 ['chr10', 'refSeq', 'CDS', '13336428', '13336596', '.', '-', '2', '6214'],
   	 ['chr10', 'refSeq', 'CDS', '13337496', '13337606', '.', '-', '2', '6214'],
   	 ['chr10', 'refSeq', 'CDS', '13340187', '13340245', '.', '-', '0', '6214'],
   	 ['chr10', 'refSeq', 'CDS', '13341968', '13342042', '.', '-', '0', '6214']]
   
   def myKey(tup): 
       mark = ''
       if  tup[6] == '-': 
           mark = '-'
       return (tup[0],tup[8],int(mark+tup[4])) ##分别按第1,9列排序，同时若第7列为‘-’号按第5列排序
   refGene = sorted(a, key=myKey)
   ```

7. python3计算时间差、从字符串中匹配出时间信息

   ```python
   import time
   start = '2017-10-24 17:42:36'
   end = '2017-10-24 17:43:36'
   start_time_str = time.strptime(start, '%Y-%m-%d %H:%M:%S')
   end_time_str = time.strptime(end, '%Y-%m-%d %H:%M:%S')
   print(time.mktime(end_time_str) - time.mktime(start_time_str)) ##求时间差
   str_ = 'INFO  INFO  2017-10-25 14:04:36 snp'
   print(re.match(r".*([0-9]{4}-[0-9]{2}-[0-9]{2} [0-9]{2}:[0-9]{2}:[0-9]{2})", str_).group(1)) ##从字符串中匹配出时间信息:2017-10-25 14:04:36
   ```

8. 列表(list)元素取交集

   ```python
   dupPath = r'd:\dup.txt'
   with open(dupPath) as f:
   	dup = [line.split('\t')[0] for line in f]
   
   strangeIntronPath = r'd:\refGene_v1_1.txt'
   with open(strangeIntronPath) as f:
   	strangeIntron = [line.split('\t')[0] for line in f]
   
   set(dup) & set(strangeIntron)
   ```

9. 浅拷贝与深拷贝的用法

   ```python
   #浅拷贝
   >>> a = {1:{1:2},2:3}
   >>> b=a.copy()
   >>> a.pop(1)
   >>> a
   {2: 3}
   >>> b
   {1: {1: 2}, 2: 3}
   
   >>> a = {1:{1:2},2:3}
   >>> b=a.copy()
   >>> a[1].pop(1)
   >>> a
   {1: {}, 2: 3}
   >>> b
   {1: {}, 2: 3}
   #深拷贝
   >>> import copy
   >>> a = {1:{1:{1:2},2:3},2:3}
   >>> b=copy.deepcopy(a)
   >>> a[1][1].pop(1)
   >>> a
   {1: {1: {}, 2: 3}, 2: 3}
   >>> b
   {1: {1: {1: 2}, 2: 3}, 2: 3}
   ```

10. python计数

  ```python
  try:
      from collections import Counter
  except ImportError:
      from counter import Counter
  d = Counter('simsalabim')
  c = Counter('abcdeabcdabcaba')
  c
  #Counter({'a': 5, 'b': 4, 'c': 3, 'd': 2, 'e': 1})
  c.update(d)
  c['a']
  #7
  ```

11. Eclipse美化中文字体：http://www.everycoding.com/coding/299.html

   把eclipse放在这里是因为我的python ide用的就是eclipse+pydev.

12. python统计程序执行时间

   > 在 Unix 系统中，建议使用 time.time()，在 Windows 系统中，建议使用 time.clock()
   > if sys.platform == "win32":
   >
   > On Windows, the best timer is time.clock()
   >
   > default_timer = time.clock
   > else:
   >
   > On most other platforms the best timer is time.time()
   >
   > default_timer = time.time
   > 使用 timeit 代替 time，这样就可以实现跨平台的精度性：
   >
   > start = timeit.default_timer()
   > ... do something
   > elapsed = (timeit.default_timer() - start)

   ```python
   import timeit

   def timeTest():
       start = timeit.default_timer()
       print("Start: " + str(start))
       for i in range(1, 100000000):
           pass
       stop = timeit.default_timer()
       print("Stop: " + str(stop))
       print(str(stop-start) + "秒")

   def main():
       timeTest()

   if __name__=='__main__':
       main()
   ```


参考：[[python计算程序运行时间](http://www.cnblogs.com/youxin/p/3157099.html)](https://www.cnblogs.com/youxin/p/3157099.html)

13. python无穷大的表示

    ```python
    >>> a = float('inf') #正无穷大
    >>> b = float('-inf') #负无穷大
    >>> c = float('nan')  #未定义的数字
    >>> a
    inf
    >>> b
    -inf
    >>> c
    nan
    >>> a > 3
    True
    >>> a < 3
    False
    >>> b < -9999
    True
    ```

    参考: [无穷大与NaN](http://python3-cookbook.readthedocs.io/zh_CN/latest/c03/p07_infinity_and_nan.html)

14. python调用外部程序

    ```python
    #使用os模块调用ls命令
    >>> import os
    >>> os.system('ls')
    1.txt            baqtools.cpp             bgitools.cpp       exc.e.bak  globalb.cpp                    grptools.hpp [...]
    ```

    ​