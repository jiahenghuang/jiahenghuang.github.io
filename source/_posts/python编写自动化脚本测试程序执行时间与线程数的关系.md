---
title: python编写自动化脚本测试程序执行时间与线程数的关系
date: 2018-05-15 17:52:18
tags: [python, 自动化脚本]
categories: python
---

利用python3编写自动脚本记录程序运行时间与对应的线程数

```python
import os
import time

with open('/mnt/liusheng_test/work_v1/hdcancer/bqsr/time.txt','w') as f:
    f.write("td_num\tread_num\tseconds\tminutes\n")
    min_time_lst = [0, 0, 0, 0]
    min_time = float('inf')
    for br_td_num in range(10, 30):
        for br_read_num in range(3000, 10000, 100):
            os.system("sed -i '" + str(71) + "c #define br_read_num " + str(br_read_num) + " //e_batch_num_br = 10000' common.hpp")
            os.system("sed -i '" + str(72) + "c #define br_td_num " + str(br_td_num) + " //td_num_br = 20' common.hpp")
            begin = time.time()
            os.system("make run")
            end = time.time()
            if end < min_time:
                min_time = end - begin
                min_time_lst[0] = br_td_num 
                min_time_lst[1] = br_read_num 
                min_time_lst[2] = min_time
                min_time_lst[3] = min_time / 60

            f.write(str(br_td_num) + '\t' + str(br_read_num) + '\t' + str(end-begin) + '\t' + str((end-begin)/60) + '\n')
            f.flush()        
    f.write('\t'.join(min_time_lst) + '\n')
```

