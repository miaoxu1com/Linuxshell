linux 600条命令总结
https://www.cnblogs.com/TheMagicalRainbowSea/p/17303384.html

https://blog.csdn.net/hongweigg/article/details/77948664
shell引用另一个命令行的返回值是在变量定义时就已经解析了
status=$(kubectl get po -A)
代表返回的值组成一个数组
($(cat /root/images_name | grep -v ingressproxy | awk -F: '{printf("%s ",$1)}'))
