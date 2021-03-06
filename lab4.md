# Lab 4 批改情况

Author: Zhihao Dai, Student Assistance

Your grades for Lab 4 have been released. Since most submissions are graded by the program, if your function fails to pass the program, you will receive comments by me in red font and possibly an sql file attachment of the "fixed" version of the function, which I used to run against 20 test cases.

If your function is graded by the program, you will not receive my comments in red font, but only comments produced by the program in black font.

You may find all 20 test cases and their expected scripts name in file `sample.sql` and `answer.png` respectively under the directory `test case` on Sakai.

For those whose submissions are graded by me (that is, receive comments in red font) and have questions regrarding the grades, please send an E-mail to 11510415@mail.sustc.edu.cn.

For those with doubts whose submissions are grade by the program, please talk to TA in lab section next week.

The following content is about several common problems I have encountered when grading your submissions manually and provided only in Chinese.

今天花了一个下午改大家没有通过自动批改程序的Lab4，总结了一下大家普遍遇到的几个问题，希望大家下次能够避免。

### 1. 函数名字写错
Lab4要求的是一个`script`函数，有不少同学函数名字写错了，写成了`scriiipt`或`identify`，SQL脚本可以正常执行，但是程序无法调用这个函数。

### 2. 函数没有默认返回值
函数的逻辑写对了，但是对于未知的情况，函数没有返回值。

SQL脚本虽然可以正常执行，但是程序在运行时，会在一个`test_table`上进行查询。因为对于未知情况的那一行，函数没有返回值，数据库会返回错误信息，其他行的结果也无法返回，程序也就没法给分。

还有些同学返回的'Other'拼错，拼成了‘Others’或者'Ohter'，emmmmmm...

### 3. SQL脚本里包含`//`和查询语句
很多同学的脚本里，包含了`//`分隔符。因为这个是我们自己定义的分隔符，但对于PostgreSQL来说是没有意义的，是不合法的，SQL脚本不能正常执行。

还有同学的脚本里包含了用于测试的查询语句，这些都是没有必要的，而且很有可能导致脚本执行错误。

例如`select script(titles) from alt_tables`，就可能因为缺少`alt_tables`出现错误。

### 4. 使用`DROP FUNCTION script(title varchar)`
直接使用`DROP FUNCTION script(title varchar)`是不对的，且有可能导致不可预料的错误。

第一，我们不知道script这个函数是否存在；第二，即便script存在，我们也不知道它的输入的变量名是不是title。第三，批改程序在执行脚本前，会删除script函数，我们没必要写到脚本里。

如果一定要DROP FUNCTION，可以用`drop function if exists script(varchar)`。

### 5. SQL脚本没有以`.sql`作为后缀
批改程序似乎只会执行`.sql`作为后缀的文件，但我手上没有程序，所以没法确认。

### 6. 函数结尾没有加分号
很多同学的函数结尾的`$function$`后面没有加分号`;`，导致SQL脚本不能正常执行。

### 7. 其他

- SQL里面没有`&&`，没有`||`，请用`and`和`or`代替。

- SQL里面没有`Integer.MAX_VALUE`，在PostgreSQL里integer的范围是-2147483648到+2147483647。[Numeric Types](https://www.postgresql.org/docs/9.1/static/datatype-numeric.html)

## 建议

最后建议以后大家在写好SQL脚本后，DROP之前建的所有函数，选中脚本中的所有语句后执行，来确保脚本没有语法错误，且可以完整、独立运行。另外建立一个文件，用于存放用到的测试语句，不需要提交。

最终提交的脚本里不要包含自定义的分隔符（如`//`）和不必要的测试语句。

大家如果对自己的SQL语法不是特别自信，可以使用有语法检查的IDE，例如[DataGrip](https://www.jetbrains.com/datagrip/)，避免脚本运行错误的出现。

## 批改过程

在Lab 4所有人工批改的反馈里，**我都用<span style="color:red">红字</span>标明了扣分原因**，部分标明了每一项的扣分分数。部分同学提交的脚本有语法错误，我都尽量修改，以在测试集合上进行测试，并返回了我修改过后可以正常运行的脚本。

若作业中出现了上面提到的问题1-7，我都只做了 <span style="font-size:0.3em">微小</span> 的扣分，希望大家下次能够避免。

**对于那些语法错误太多，或者逻辑明显错误，或者提交的文件格式不对的作业（不要交WORD文档！！！），我都做了适当程度的扣分。**

- 下面是我在评分时候用到的查询语句，供大家参考，test_table里面一共有20个测试用例，`script_ok`是正确的`script`函数：
```sql
select s1, s2, s1!=s2 diff
from (
  SELECT
    script(title)    s1,
    script_ok(title) s2
  FROM test_table
) test;
```

测试用例（`sample.sql`）：
```sql
create table test_table(id serial primary key,
                        title varchar(100) unique);

insert into test_table(title) values('चालबाज़');
insert into test_table(title) values('Бриллиантовая рука');
insert into test_table(title) values('Man Bites Dog');
insert into test_table(title) values('జయభేర கலைவாணன');
insert into test_table(title) values('Необычайные приключения мистера Веста в стране Большевиков');
insert into test_table(title) values('ฝันบ้าคาราโอเกะ');
insert into test_table(title) values('冷血十三鷹');
insert into test_table(title) values('廣西電影製片廠');
insert into test_table(title) values('歩いても 歩いても');
insert into test_table(title) values('ᐊᑕᓈᕐᔪᐊᑦ');
insert into test_table(title) values('4인용 식탁');
insert into test_table(title) values('사람의 아들');
insert into test_table(title) values('فروشنده');
insert into test_table(title) values('엽기적인 그녀');
insert into test_table(title) values('Börn náttúrunnar');
insert into test_table(title) values('शोले');
insert into test_table(title) values('邋遢大王奇遇记');
insert into test_table(title) values('阿飛正傳');
insert into test_table(title) values('ラスト・ブラッド');
insert into test_table(title) values('I-4:Λούφα και Απαλλαγή');
```

`script_ok`函数（暂未公开）：


## FAQ

1. 我的评语里没有红字，只有黑字的程序的输出，且没有标明扣分点，我想知道我的扣分点都有哪些？

评语里面没有红字，表明你的作业是由老师的批改程序自动批改的。针对这种情况，你可以在提供的20个测试用例上，测试你写的函数。如果正确的比例和得分的比例相差比较大，你可以在下周的实验课上向TA提出异议。（如果给分高了，就不用找了。）

2. 我的评语里有红字，但没有标明扣分点，我想知道我的扣分点都有哪些？

评语里有红字的作业都是由手工我批改的，大部分都有标明详细扣分点，少部分还返回了我修改后的没有语法错误的脚本，但部分前期批改的没有标注扣分点。遇到这种情况，你可以直接给我的邮箱 11510415@mail.sustc.edu.cn 发邮件。

3. 批改的评语 “There are multiple typos in your function ...”里的typo(s)指的是拼写错误吗？

typo指的就是**语法错误**。因为Stephan原本要求有语法错误不能执行的，分数都要打对折，考虑到一次LAB的比重较大，为了避免过多的50分惨案，我都用typo指代语法错误，这样大家也能少扣点分。
