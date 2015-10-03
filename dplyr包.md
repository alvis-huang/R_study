### 导入一些包

>install.packages("Lahman")
>install.packages("nycflights13")
>library("nycflights13")
>library("Lahman")
>library("dplyr")

### 选择
>batting <- select(tbl_df(Batting),playerID,yearID,teamID,G,AB:H)
>batting <- arrange(batting,playerID,yearID,teamID)
>players <- group_by(batting,playerID)

### 过滤
&或者"," 表示且， | 表示或 
>filter(players,min_rank(desc(H))<=2 & H>0)

### 生成新的列
>mutate(players,rs = min_rank(desc(H)))
>mutate(players,G_rank=min_rank(G))

### 引用前一个元素
>filter(players, G>lag(G))
>mutate(players, G_change = (G-log(G))/(yearID-lag(yearID)))

>filter(players, G>mean(G))
>mutate(players, G_z=(G-mean(G))/sd(G))

mutate(players, percent_rank(desc(G)))
D %>% group_by(x1) %>% mutate(n=percent_rank(x2))
D %>% group_by(x1) %>% mutate(n=percent_rank(desc(x2)))
D %>% group_by(x1) %>% mutate(n=cume_dist(desc(x2)))

by_team_player <- group_by(batting, teamID, playerID)
by_team <- summarise(by_team_player,G=sum(G))
by_team_quartile <- group_by(by_team,quartile=ntile(G,4))
summarise(by_team_quartile, mean(G))

##Lead and Lag
mutate(players, G_delta=G-lag(G))

D %>% group_by(x1) %>% 

df <- data.frame(year=2000:2005,value=(0:5)^2)
scrambled <- df[sample(nrow(df)),]
wrong <- mutate(scrambled, running=cumsum(value))
right <- mutate(scrambled, running=order_by(year,cumsum(value)))
arrange(right,year)

#Cumulative aggregates
x <- 1:10
y <- 10:1
order_by(y, cumsum(x))
D = data.frame(x1=c("a","b","a","a","b","b"),x2=c(1,2,3,4,4,4))
E = data.frame(y1=c(1,2,3),y2=c("aa","bb","cc"))
D %>% mutate(x3=lag(x2))
D %>% mutate(x3=lag(x2,2))


##Two-table verbs 
#Mutating joins
flights2 <- flights %>% select(year:day, hour, origin, dest, tailnum, carrier)
flights2 %>% left_join(airlines)
#Controlling how the tables are matched
flights2 %>% left_join(weather)
flights2 %>% left_join(planes, by ="tailnum")

D %>% left_join(E,c("x2"="y1"))
time_data = data.frame(time=c(201501,201502,201504,201503,201501,201502,201503,201504,201501,201502,201503,201504),
                       adcode=c("1","1","1","1","2","2","2","2","3","3","3","3"),
                       pv=c(1,3,4,6,10,20,40,60,100,200,400,600))
time_data %>% arrange(adcode,time) %>% group_by(adcode) %>% mutate(dpv=pv-lag(pv))

df1 <- data_frame(x=c(1,2),y=2:1)
df2 <- data_frame(x=c(1,3),a=10,b="a")
df1 %>% inner_join(df2) %>% knitr::kable()
df1 %>% left_join(df2)
df1 %>% right_join(df2)
df1 %>% full_join(df2)

df1 = data_frame(x=c(1,1,2,4),y=1:4)
df2 = data_frame(x=c(1,1,2,3),z=c("a","b","a","d"))
df1 %>% left_join(df2)
df1 %>% anti_join(df2)
df1 %>% semi_join(df2) %>% nrow()

df1 = data_frame(x=1:2,y=c(1L,1L))
df2 = data_frame(x=1:2,y=1:2)
intersect(df1,df2)
setdiff(df1,df2)
setdiff(df2,df1)
