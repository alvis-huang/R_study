#R语言常用包

##数据读取

* 读取Excel

>library(xlsx)
>read.xlsx2(file,1,colClasses=c("character","Date",rep("numeric",17)))
>write.xlsx2(res,outfile,row.names=FALSE)
