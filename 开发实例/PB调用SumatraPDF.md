# PB调用SumatraPDF

## 1、下载 SumatraPDF

https://www.sumatrapdfreader.org/download-free-pdf-viewer

下载好以后放到程序根目录就可以了

## 2、查看 SumatraPDF 命令行参数

https://www.sumatrapdfreader.org/docs/Command-line-arguments

## 3、各参数的含义说明

`-print-to-default` 将此命令行上指示的所有文件打印到系统默认打印机。

`-print-to <printer-name>` 将此命令行上指示的所有文件打印到指定的打印机。

`-silent` 将与命令行打印相关的任何错误消息静音。

`-print-settings <settings-list>`：启用打印设置可选值为以下几个参数

​	纸张方向：portrait（纵向），landscape（横向）

​	缩放比例：

​		缩⼩到合适⼤⼩（默认）：shrink

​		⽆边框：noscale

​		兼容模式：fit/compat

​	纸张大小：paper=<page size> 可选值为（A2、A3、A4、A5、A6、letter、legal、tabloid、statement）

其他参数可以根据自己需要配置...

## 4、在 PB 中调用

```c
string ls_run,ls_filepath
ls_filepath = getcurrentdirectory() + '\1.pdf'
ls_run = "SumatraPdf.exe -print-to '打印机名称' -silent -print-settings 'shrink',landscape,paper=A4 " + ls_filepath
run(ls_run)
```