---
layout: post
title:	python处理excel文件
---

<p class="meta">10 Jul 2012 - Nanjing</p>

断断续续地帮朋友用python做了一些excel处理。发现每次都得查文档回忆方法的具体用法，很是浪费时间，故在此把常用的属性和方法记下来。

处理excel的文档有三个：xlrd, xlwt, xlutils.分别负责读，写和拷贝。xlrd只能读excel文件，xlwt只能写excel文件。但是xlrd打开的文件对象是无法写入的，所以要修改一个已存在文件的话需要xlutils.copy在其中把read的文件对象拷贝成write的文件对象，才能用xlwt的write方法写入（修改）。
	
	:::python
	import xlrd
	rb = xlrd.open_workbook('test.xls')
	sheet = rb.sheet_by_index(0)	#选择第一个标签页
	sheet = rb.sheet_by_name(sheetName)
	sheet.nrows, sheet.ncols, sheet.col(n), sheet.row(m), sheet.cell(m, n)
	sheet.cell(m, n).value

	import xlwt
	wt = xlwt.Workbook()
	sheet = wt.add_sheet('sheet name')
	sh = wt.get_sheet(0)

	from xlutils.copy import copy
	wt = copy(rb)
	sh = wt.get_sheet(0)
	sh.write(m, n, value)
	row1 = sh.row(1)
	row1.write(rowNum, value)
	wt.save('output.xls')

tips:

1.	对编码的处理，最好是把所有传给xlwt相关的内容都转为unicode编码。

好像很少啊，真是为我的大脑内存捉急啊。