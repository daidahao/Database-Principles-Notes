# Lab 4 批改情况

今天花了一个下午改大家没有通过自动批改程序的Lab4，总结了一下大家普遍遇到的几个问题，希望大家下次能够避免。

### 1. 函数名字写错
Lab4要求的是一个script函数，有不少同学函数名字写错了，SQL脚本可以正常执行，但是程序无法调用这个函数。

### 2. 函数没有默认返回值
通常很多逻辑写对了，但是对于未知的情况，函数没有返回值。SQL脚本虽然可以正常执行，但是程序在执行时，会在一个test_table上进行查询。因为对于'Other'的那一行，函数没有返回值，数据库会返回错误信息，其他行的结果也无法返回，程序也就没法给分。

还有些同学是返回的'Other'拼错，拼成‘Others’或者'Ohter'的，emmmmmm...

### 3. SQL脚本里包含`//`和查询语句
很多同学的脚本里，包含了`//`分隔符。因为这个是我们自己定义的分隔符，但对于PostgreSQL来说是没有意义的，是不合法的，SQL脚本不能正常执行。还有同学的脚本里包含了用于测试的查询语句，这些都是没有必要的，而且很有可能因为缺少alt_tables导致脚本执行错误。

### 4. 使用`DROP FUNCTION script(title varchar)`
直接使用`DROP FUNCTION script(title varchar)`是不对的，且有可能导致不可预料的错误。第一，我们不知道script这个函数是否存在；第二，即便script存在，我们也不知道它的输入的变量名是不是title。第三，批改程序在执行脚本前，会删除script函数，我们没必要写到脚本里。如果一定要DROP FUNCTION，可以用`drop function if exists script(varchar)`。

### 5. SQL脚本没有以`.sql`结尾
批改程序似乎只会执行.sql结尾的文件，但我手上没有程序，所以没法确认。

### 6. 函数结尾没有加分号
很多同学的函数结尾的`$function$`后面没有加分号`;`，导致SQL脚本不能正常执行。

### 7. 其他

- SQL里面没有`&&`，没有`||`，请用`and`和`or`代替。

- SQL里面没有`Integer.MAX_VALUE`，在PostgreSQL里integer的范围是-2147483648到+2147483647。[Numeric Types](https://www.postgresql.org/docs/9.1/static/datatype-numeric.html)

最后建议以后大家在写好SQL脚本后，DROP之前建的所有函数，选中脚本中的所有语句后执行，来确保脚本没有语法错误，且可以完整、独立运行。另外建立一个文件，用于存放用到的测试语句。

最终提交的脚本里不要包含自定义的分隔符（如`//`）和不必要的测试语句。

- 下面是我在评分时候用到的查询语句，供大家参考，test_table里面一共有20个测试用例：
```sql
select s1, s2, s1!=s2 diff
from (
  SELECT
    script(title)    s1,
    script_ok(title) s2
  FROM test_table
) test;
```
