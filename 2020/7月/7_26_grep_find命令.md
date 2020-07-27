## 1 grep命令
查找文件内的文本: `grep -r "struct task_struct {" /usr/src`
加入后缀<kbd>-n </kbd> 可以显示行号

## 2 find命令

* 常用后缀

  1. <kbd>-name</kbd>

     `find ./ -name *.mp3`

  2. <kbd>-type</kbd>

     找普通文件：`find ./ -type f`

     找字符设备：`find ./ -type c`

     类型： f`文件`/d`目录`/p`管道`/c`字符设备`/b`块设备`/s`socket`/l`符号链接`

  3. <kbd>-size</kbd>

  4. <kbd>-maxpath</kbd>

  5. <kbd>-exec</kbd>

  6. <kbd>-print</kbd>

  7. <kbd>xargs</kbd>

  8. <kbd>-atime</kbd>

     <kbd>-amin</kbd>

     <kbd>-mtime</kbd>

     <kbd>-mmin</kbd>

     

     

 