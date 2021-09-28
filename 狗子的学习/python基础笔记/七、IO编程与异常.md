# IO编程与异常

## 一、os模块的使用

### 1、基础操作

**查看系统平台**

```python
import os
print(os.name) 
# nt---Windows   posix---Linux
```



**获取当前系统平台路径分隔符**

```
os.sep

\ ——Windows    / ——Linux
```



**获取当前工作目录**

```
os.getcwd

#访问当前工作目录下data子目录中的test.dat文件
'data'+os.sep+'test.dat'
'data{sep}test.dat'.format(sep=os.sep)
os.getcwd()+os.sep+'data'+os.sep+'test.dat'
'{cwd}{sep}data{sep}test.dat'.format(cwd=os.getcwd(),sep=os.sep)
```



**获取环境变量值**

```
os.environ  包含所有环境变量值的映射对象

查看某一个环境变量值
os.environ[key]  
os.getenv(key)
```

```python
import os
print(os.environ['JAVA_HOME'])
print(os.getenv('JAVA_HOME'))
```

<img src="C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210920134704361.png" alt="image-20210920134704361" style="zoom:50%;" />



**获取文件和目录列表**

```python
os.listdir(path='.')
#其中，path是要获取文件和目录名字的路径，默认值'.'表示获取当前路径下的所有文件和目录的名字。
```

```python
import os
print(os.listdir()) #获取当前路径下所有文件和目录名字组成的列表
print(os.getenv(os.getcwd()+os.sep+'test')) #获取当前路径的test目录下所有文件和目录名字组成的列表
```

![image-20210920135758029](C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210920135758029.png)

### 2、目录创建和删除

**创建目录**

```python
os.mkdir(path)
os.makedirs(path) 
# os.mkdir只能用于创建路径中的最后一个目录，即要求路径中除最后一个目录外前面的目录应该都存在
#os.makedirs能够用于依次创建路径中所有不存在的目录
```

```python
os.mkdir(os.getcwd()+os.sep+'newdir')
os.makedirs(os.getcwd()+os.sep+'subdir1'+os.sep+'subdir2')
```



**删除目录**

```
os.rmdir(path) #只能删除空目录
--- os.redir(os.getcwd()+os.sep+'newdir')

os.removedirs(path)
```



### 3、获取绝对路径、路径分离和连接

`.`表示当前目录，`..`表示上一层目录

**获取绝对路径**

```python
os.path.abspath(path) #获取指定路径的相对路径
```



**获取文件所在目录的路径**

```python
os.path.dirname(path) #获取文件所在目录的路径，返回path中去除文件名后的路径
```

```python
print(os.path.dirname('E:\py_test\\test.py'))
```

<img src="C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210922094003737.png" alt="image-20210922094003737" style="zoom:50%;" />



**获取文件名**

```python
os.path.basename(path) #获取指定路径中的文件名，返回path中的文件名
```



**获取指定路径的目录名或文件名**

```python
os.path.split(path) #返回一个由path分解得到的路径名和目录/文件名组成的元组
#若指定路径中不包含文件名，会将指定路径分成两部分：最后一个目录名和由前面所有目录组成的路径名
```



**分离文件扩展名**

```python
os.path.splitext(path) #将path所指定的路径分解成一个元组(root,ext)，其中ext是扩展名，root是扩展名前面的内容
```



**路径连接**

```python
os.path.join(path,*paths) #将各参数用系统路径分隔符连接得到的结果返回
```



### 3、条件判断

```
os.path.isfile(path)
os.path.isdir(path)
os.path.exists(path)
os.path.isabs(path)
```



## 二、文件

### 1、文件打开和关闭

**文件打开**

```
open(filename,mode='r')
```

文件打开方式：

![image-20210922100054652](C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210922100054652.png)

在打开文件时，如果打开方式中不包括'a'，则文件指针指向文件首的位置;随着读/写操作，文件指针顺序向后移动，直至读写完毕。如果打开方式中包括'a'，则文件指针指向文件尾的位置，此时向文件中写数据时就会在已有数据后写入新数据。



常用文件打开方式组合：

![image-20210922100237268](C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210922100237268.png)



**文件关闭**

```
f.close()
```



**with语句**

使用with语句可以让系统在文件操作完毕后自动关闭文件，从而避免忘记调用close方法而不能及时释放文件资源的问题。

```python
with open('E:\\py_test\\test\\test.txt','wt') as f:
	pass
print('文件已关闭：',f.closed)
```

<img src="C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210922100748317.png" alt="image-20210922100748317" style="zoom:50%;" />



### 2、文件对象的write和read方法

**write**

```python
f.write(str) #使用文件对象的write方法可以将字符串写入到文件中
#其中，f是open函数返回的文件对象，str是要写入到文件中的字符串。f.write函数执行完毕后将返回写入到文件中的字符数。
```

```python
charnum=0
with open('E:\\py_test\\test\\test.txt','w+') as f:
    charnum+=f.write('Python是一门流行的编程语言！\n')
    charnum+=f.write('我喜欢学习Python语言！')
print('写入的字符数：%d'%charnum)
```

<img src="C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210922101558700.png" alt="image-20210922101558700" style="zoom:50%;" />

使用write方法向文件中写入一个字符串后并不会自动在字符串后加换行。如果加换行的话，需要人为向文件中写入换行符`\n `。write方法返回的写入文件的字符数包括换行符`\n`。



**read**

```python
f.read(n=-1)
#其中，f是open函数返回的文件对象;n指定了要读取的字节数，默认值-1表示读取文件中的所有数据。read方法将从文件中读取的数据返回
```

```python
with open('E:\\py_test\\test\\test.txt', 'r',encoding='utf-8') as f:
    content1 = f.read()
    content2 = f.read()
print('content1:\n%s' % content1)
print('content2:\n%s' % content2)
```

<img src="C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210922110444851.png" alt="image-20210922110444851" style="zoom:50%;" />

### 3、文件对象的readline、readlines、seek方法

**readline**

```python
f.readline() 
#其中，f是open函数返回的文件对象。readline方法将从文件中读取的一行数据返回。
```

```python
ls=[]
with open('E:\\py_test\\test\\test.txt', 'r',encoding='utf-8') as f:
    ls.append(f.readline())
    ls.append(f.readline())
print(ls)
```

<img src="C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210922110856922.png" alt="image-20210922110856922" style="zoom:50%;" />



**readlines**

```python
f.readlines()
#readlines方法将从文件中按行读取的所有数据以列表形式返回。
```

```python
ls=[]
with open('E:\\py_test\\test\\test.txt', 'r',encoding='utf-8') as f:
    ls=f.readlines()
print(ls)
```

<img src="C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210922111055879.png" alt="image-20210922111055879" style="zoom:50%;" />



**seek**

 ``` python
 f.seek(pos,whence=0)
 #其中，f是open函数返回的文件对象;pos是要移动的字节数;whence是参照位置，默认值0表示以文件首作为参照位置，1和2分别表示以当前文件指针位置和文件尾作为参照位置。seek方法没有返回值。
 ```

```python
with open('E:\\py_test\\test\\test.txt', 'r') as f:
    f.seek(2,0)
    print(f.readline())
```

<img src="C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210922113637532.png" alt="image-20210922113637532" style="zoom:50%;" />

当以文本方式打开文件后，只支持以文件首作为参照位置进行文件指针的移动;而以二进制方式打开文件后，可以支持全部的三种参照位置。通过seek方法实现的文件随机读写主要用于二进制文件，建议尽量不对文本文件进行随机读写。



## 三、一维数据、二维数据

**一维数据**

一维有序数据——可用列表存储

一维无序数据——集合



**二维数据**

二维列表



**CSV操作一维、二维数据**

+ CSV是—种国际通用的一维、二维数据存储格式，其对应文件的扩展名为.csv，可使用Excel软件直接打开。

+ CSV文件中每行对应一个一维数据，一维数据的各数据元素之间用英文半角逗号分隔（逗号两边不需要加额外的空格）;对于缺失元素，也要保留逗号，使得元素的位置能够与实际数据对应。

+ CSV文件中的多行形成了一个二维数据，即一个二维数据由多个一维数据组成;二维数据中的第一行可以是列标题，也可以直接存储数据（即没有列标题）。



**CSV文件的写操作**

```python
csv.writer(csvfile)
#csv模块的writer方法可以生成一个writer对象，使用该对象可以将数据以逗号分隔的形式写入到CSV文件中。
#其中，csvfile是一个具有write方法的对象。如果将open函数返回的文件对象作为实参传给csvfile，则调用open函数打开文件时必须加上一个关键字参数newline="
```

```python
writer.writerow(row)
writer.writerows(rows)
#writer是csv.writer方法返回的writer对象；row是要写入到CSV文件中的一行数据（如一维列表）；rows是要写入到CSV文件中的多行数据（如二维列表）
```



**CSV文件的读操作**

```python
csv.reader(csvfile)
#csvfile要求传入一个迭代器，open函数返回的文件对象除了是可迭代对象，同时也是迭代器。如果将文件对象作为实参传给csvfile，则调用open函数打开文件时应加上一个关键字参数newline="
#返回的reader对象是一个可迭代对象，因此可以使用for循环直接遍历CSV文件中的每一行数据，每次遍历会返回一个由字符串组成的列表。
```

```python
import csv
data2D=[[90,98,87],[70,89,82],[65,80,97]]
with open('E:\\py_test\\test\\test.csv','w',newline='') as f:
    csvwriter=csv.writer(f)
    csvwriter.writerow(['语文','数学','英语'])
    csvwriter.writerows(data2D)
ls2=[]
with open('E:\\py_test\\test\\test.csv','r',newline='') as f:
    csvreader=csv.reader(f)
    for line in csvreader:
        ls2.append(line)
print(ls2)
```

![image-20210925084433642](C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210925084433642.png)

<img src="C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210925084448432.png" alt="image-20210925084448432" style="zoom:50%;" />



## 四、异常的定义和分类

### 1、异常的分类

语法错误  逻辑错误

![image-20210925090633672](C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210925090633672.png)

![image-20210925090712567](C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210925090712567.png)



### 2、异常处理try except

```python
for i in range(3):
    try:
        num=int(input('请输入一个数字：'))
        print(10/num)
    except ValueError:
        print('值错误')
    except:
        print('其他异常')
```

<img src="C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210925091127907.png" alt="image-20210925091127907" style="zoom:50%;" />

except子句后面的异常类型，既可以是单个异常类型，如“except ValueError:";也可以是由多个异常类型组成的元组，如“except(TypeError, ZeroDivisionError):” ;
还可以为空，即“except:"，表示捕获所有的异常。



### 3、异常处理 else、finally、raise

**else**

else子句是try except语句中的一个可选项。

如果try子句执行时没有发生异常，则在try子句执行结束后会执行else子句；否则，如果发生异常，则else子句不会执行。

```python
for i in range(3):
    try:
        num=int(input('请输入一个数字：'))
        print(10/num)
    except ValueError:
        print('值错误')
    except:
        print('其他异常')
    else:
        print('else子句被执行')
```

<img src="C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210925091714774.png" alt="image-20210925091714774" style="zoom:50%;" />

**finally**

finally子句是try except语句中另一个可选项。

无论try子句执行时是否发生异常，finally子句都会被执行。

```python
for i in range(3):
    try:
        num=int(input('请输入一个数字：'))
        print(10/num)
    except ValueError:
        print('值错误')
    except:
        print('其他异常')
    finally:
        print('finally子句被执行')
```

<img src="C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210925092006376.png" alt="image-20210925092006376" style="zoom:50%;" />



**raise**

除了系统遇到错误产生异常外，也可以使用raise产生异常

```python
for i in range(2):
    try:
        num=int(input('请输入一个数字：'))
        if num==0:
            raise ValueError('输入数字不能为0')
        print(10/num)
    except ValueError as e:
        print('值错误',e)
```

<img src="C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210925092358265.png" alt="image-20210925092358265" style="zoom:50%;" />



### 4、异常处理 断言和自定义异常

**断言**

使用assert可以判断一个条件是否成立，如果成立则继续执行后面的语句；如果不成立则会引发AssertionError异常

```python
for i in range(2):
    try:
        num=int(input('请输入一个数字：'))
        assert num!=0
        print(10/num)
    except AssertionError:
        print('断言失败')
```

<img src="C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210925092807596.png" alt="image-20210925092807596" style="zoom:50%;" />



**自定义异常**

实际上就是以BaseException类作为父类创建一个子类

```python
class ScoreError(BaseException):
    def __init__(self,msg):
        self.msg=msg
    def __str__(self): #定义__str__方法，将ScoreError类对象转换为字符串时自动调用
        return self.msg

if __name__=='__main__':
    for i in range(2):
        try:
            score=int(input('请输入一个成绩：'))
            if score<0 or score>100:
                raise ScoreError('输入成绩为%d,成绩应在0-100之间'%score)
            print('输入成绩为%d'%score)
        except ScoreError as e:
            print('分数错误：',e)
```

<img src="C:\Users\leenco\AppData\Roaming\Typora\typora-user-images\image-20210925093708197.png" alt="image-20210925093708197" style="zoom:50%;" />

