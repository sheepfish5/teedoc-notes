# Java 基础IO

- IO流（java.io）

  - 字节流

    - 所有字节流类继承自InputStream和OutputStream（如FileInputStream和FileOutputStream）

      - InputStream.read()和OutputStream.write()都是一次读或写一个字节，可以用一个int接收read()的字节，占用int的最后8位

      - 记得关闭流——close()，关闭之前还要判null

      - 其他流都建立在字节流之上

      - 使用流会引发IOException

  - 字符流

    - Java使用Unicode标准。字符流自动在内部格式和电脑本地字符集之间转换。

      - 所有字符流类继承自Reader和Writer（如FileReader和FileWriter）

      - read()和write()一次读写一个字符，可以用一个int来接收read()的字符，占用int的最后16位

      - 字符流是字节流的包装器

        - 字节流使用字符流做物理IO，再在字符与字节之间进行转换。

        - 桥流：InputStreamReader和OutputStreamWriter。用他们来定制想要的字符流。

      - 面向行的IO。使用readline()和println()来读写行。

        - CRLF：carriage-return/line-feed，\r\n

  - 缓冲流

    - 之前的非缓冲流的读写请求直接由操作系统处理，触发磁盘访问等，开销很大。

    - 缓冲输入流从被称为缓冲的内存区域读入，原生输入API只在缓冲为空时调用一次。

    - 缓冲输出流也将数据写入缓冲，原生API只在缓冲为空时调用一次。

    - 使用包装器将非缓冲流转换成缓冲流。

      - var inputStream = new BufferedReader(new FileReader("xxx.txt");

      - var outputStream = new BufferedWriter(new FileWriter("xxx.txt");

    - 刷新缓冲流

      - 不等我们将缓冲流用满，就写出它，这被称为刷新缓冲。

      - 有些输出缓冲流支持自动刷新，可以在构造缓冲流时通过一个参数来指定。自动刷新启用时，当某个关键事件被触发时，缓冲就被自动刷新

        - 比如自动刷新的PrintWriter对象，每次调用println或format方法时，它会自动刷新。

      - 要手动刷新缓冲，调用flush方法。flush方法对任何输出流有效。但如果输出流不是缓冲流，那flush方法就没效。

  - 扫描与格式化

    - 扫描scanning（将输入分解成各种数据）

      - 默认情况下，scanner使用空白分隔输入数据

        - 空白包括空格，制表符和换行，可以用Characer.isWhitespace()判断空白。

        - Scanner对象构造器需要一个输入流对象，而且Scanner类在java.util包中。

        - 有hasNext()，next()方法

        - 需要用close方法关闭Scanner以关闭其底层的输入流。

        - 使用useDelimiter()方法改变默认的分隔符，参数是一个正则表达式。

      - 除了将输入分隔数据看成字符串以外，还能看成其他类型

        - useLocale()方法设置Scanner的地区：s.useLocale(Locale.US);

          - 可以将32,767识别为整数

        - hasNextDouble()，nextDouble()

    - 格式化formatting（将输出组织成适当的格式）

      - 实现格式设置的流对象是PrintWriter（字符流类）或PrintStream（字节流类）的实例。

        - 这两个类除了基本的write()方法外，还实现了两种格式化输出方法

          - 一种是print()和println()，用标准的方式格式化输出独立值

            - print(i)：重载print()方法，自动使用对应的print()函数输出i

            - print("string" + i)：先自动调用i.toString()，再输出合成的字符串。

          - 一种是format()方法，提供许多选项精准格式化

            - 详细信息参见：               <https://docs.oracle.com/javase/8/docs/api/java/util/Formatter.html#syntax>

  - 命令行的IO

    - 标准流

      - java的System.in对应系统的标准输入流，System.out对应系统的标准输出流，System.err对应系统的标准错误流。

        - 这些对象都是自动定义好了的。

        - 分开的标准输出流和错误流可以让我们把输出重定向到文件里，但直接显示错误。

        - 由于历史原因，这三个对象都是字节流，System.out和System.err定义为PrintWriter对象，尽管是字节流，但PrintWriter内部使用了一个字符流来效仿字符流的特性。

        - System.in是纯字节流，可用包装器转成字符流。

          - 但更一般的使用方法是：             Scanner in = new Scanner(System.in);

    - 控制台

      - 系统有预定义好的Console类对象，不仅有大多数标准流提供的特性，还有其他特性，尤其是安全密码输入。Console类对象同时提供了输入流和输出流，并且是真正的字符流。

        - 在使用Console类对象之前，必须检索操作系统提供的Console对象：           Console c = System.console();

          - 如果Console操作不可用，返回null

        - 使用readPassword()方法进行密码输入

          - 这时密码在控制台是不可见的

          - 返回一个char数组，使用完后记得手动把这个数组覆盖

- 文件IO（java.nio.file）
