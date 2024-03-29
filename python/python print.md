
默认清库下 print 的内容是会放在 buffer 里的, 也就是 buffer 满了才会一次性打印出来. 但是用 flush 参数, 可以强制要求 buffer 不留存, 全部输出

 