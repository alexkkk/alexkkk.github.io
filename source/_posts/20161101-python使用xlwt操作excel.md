---
title: python使用xlwt操作excel
date: 2016-11-01 20:16:10
tags: python 
---

用xlwt操作excel的代码如下：
```python
# create workbook
workbook = xlwt.Workbook()

# create sheet
sheet = workbook.add_sheet("sheet1") 

# write to cell (x, y, value)
sheet.write(0, 0, "query")
sheet.write(0, 1, "old(strict)")
sheet.write(0, 2, "new(strict)")

# save to file
workbook.save("compare.xls")
```


我写了下面这段代码
```python
#!/usr/local/bin/python
import xlwt

workbook = xlwt.Workbook()

sheet = workbook.add_sheet("sheet1")

sheet.write(0, 0, "aa")
sheet.write(0, 1, u'中文')

workbook.save('test.xls')
```

但是上面这段代码写入的有中文的时候，始终报如下的错误：
```bash
SyntaxError: Non-ASCII character '\xe4' in file xx.py on line 10, but no encoding declared; see http://python.org/dev/peps/pep-0263/ for details
```

改成下面这样就可以了，增加了指定文件编码格式的
```python
#!/usr/local/bin/python
# coding=utf-8
import xlwt


workbook = xlwt.Workbook()

sheet = workbook.add_sheet("sheet1")

sheet.write(0, 0, "aa")
sheet.write(0, 1, u'中文')

workbook.save('test.xls')


```


