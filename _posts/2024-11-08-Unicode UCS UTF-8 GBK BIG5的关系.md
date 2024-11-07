---
published: true
---
简单来说，unicode，gbk和大五码big5就是编码的值，而utf-8,uft-16之类就是这个值的表现形式
utf-8码完全只针对uncode来组织的，如果gbk要转utf-8必须先转uncode码，再转utf-8就Ok了．

问题一：
使用Windows记事本的“另存为”，可以在GBK、Unicode、Unicode big endian和UTF-8这几种编码方式间相互转换。同样是txt文件，Windows是怎样识别编码方式的呢？

Unicode、Unicode big endian和UTF-8编码的txt文件的开头会多出几个字节，分别是FF、FE（Unicode）,FE、FF（Unicode big endian）,EF、BB、BF（UTF-8）。但这些标记是基于什么标准呢？

0、big endian和little endian
big endian和little endian是CPU处理多字节数的不同方式。
例如“汉”字的Unicode编码是6C49。如果将6C写在前面，就是big endian。如果将49写在前面，就是little endian。

1、字符编码、内码，顺带介绍汉字编码
5、UTF的字节序和BOM
UTF-8以字节为编码单元，没有字节序的问题。
UTF-16以两个字节为编码单元，在解释一个UTF-16文本前，首先要弄清楚每个编码单元的字节序。例如“奎”的Unicode编码是594E，“乙”的Unicode编码是4E59。如果我们收到UTF-16字节流“594E”，那么这是“奎”还是“乙”？

Unicode规范中推荐的标记字节顺序的方法是BOM。Byte order Mark。

BOM是一个有点小聪明的想法：

在UCS编码中有一个叫做"ZERO WIDTH NO-BREAK SPACE"的字符，它的编码是FEFF。而FFFE在UCS中是不存在的字符，所以不应该出现在实际传输中。UCS规范建议我们在传输字节流前，先传输字符"ZERO WIDTH NO-BREAK SPACE"。

这样如果接收者收到FEFF，就表明这个字节流是Big-Endian的；如果收到FFFE，就表明这个字节流是Little-Endian的。因此字符"ZERO WIDTH NO-BREAK SPACE"又被称作BOM。

UTF-8不需要BOM来表明字节顺序，但可以用BOM来表明编码方式。字符"ZERO WIDTH NO-BREAK SPACE"的UTF-8编码是EF BB BF（读者可以用我们前面介绍的编码方法验证一下）。所以如果接收者收到以EF BB BF开头的字节流，就知道这是UTF-8编码了。

Windows就是使用BOM来标记文本文件的编码方式的。
  https://blog.csdn.net/hczhiyue/article/details/8053120
