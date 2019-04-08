# 关于VScode控制台运行C++程序输出中文乱码问题(win10)

> 原因：VScode调用的是windows命令行，默认是gbk编码，但是一般VScode打开文件使用的是utf-8编码

* 解决方案：
  * 点击要运行文件右下角的`UTF-8`
  * 在弹出框选择`Reopen with Encoding`
  * 找到下面的`GBK`重新打开文件
  * 将页面上显示的乱码修改之后重新运行即可
