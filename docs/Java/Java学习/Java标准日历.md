# Java标准日历

- ISO日历遵守the proleptic Gregorian rules，Gregorian rules于1582年被提出，预期日期从那个时间向后延伸以创建一致的、统一的时间轴并简化日期计算

- 概述

  - 时间包括两种——人类时间和机器时间

    - 人类时间：年月日，时分秒

    - 机器时间：以纳秒为单位，沿着时间线从被称为Epoch（1970年1月1日00:00:00 UTC）的起点开始不断计时

  - 要使用时间，先选择时人类时间还是机器时间，如果是人类时间，再考虑时区，再考虑日期还是时间，如果是日期，选年月日中的哪几个？

- 两个枚举：DayOfWeek和Month

  - DayOfWeek

    - 七个常量：MONDAY到SUNDAY

      - 每个常量对应一个整数，MONDAY对应1，SUNDAY对应7

    - 有许多类似核心类的方法

      - 如MONDAY.plus(3)返回THURSDAY

      - getDisplayName(TextStyle, Locale)

      - TextStyle是一个枚举类型，预定义好了时间表示格式：FULL，NARROW，SHORT，STANDALONE（不常用）。

  - Month

    - 分别包括十二个月的常量，从JANUARY到DECEMBER

      - 每个常量对应一个整数，从1到12

    - 有许多方法

      - maxLength()方法返回一个整数，代表这个月的最大天数

      - 也有getDisplayName()方法，用法和DayOfWeek类似。

- 日期类

  - 日期时间API有四个类专门处理日期信息，不带时间或时区：LocalDate，YearMonth，MonthDay和Year

    - LocalDate以ISO格式表示年月日，不带时间。

      - 可以用LocalDate来代表生日和结婚日这样的事件。

      - of()方法：LocalDate date = LocalDate.of(2000, Month.NOVEMBER, 20);

      - with()方法：LocalDate nextWed = date.with(TemporalAdjusters.next(DayOfWeek.WEDNESDAY));

      - 有许多getter方法返回给定日期的信息，如getDayOfWeek()。

    - YearMonth类代表指定年的某一月

      - now()方法和lengthOfMonth()方法：YearMonth.now().lengthOfMonth()

    - MonthDay类代表特定月的某一天

      - isValidYear()方法：此方法检查此月和日与输入的年是否构成有效日期。只有当MonthDay是2月29日时才可能返回false。

    - Year类代表某一年

      - isLeap()方法判断是不是闰年。

- 日期和时间类

  - LocalTime类和其他以Local为前缀的类相似，但它只有时间

    - LocalTime类用来表示人类时间，如电影开播时间，图书馆开门关门时间。

    - 三个方法：getHour()，getMinute()，getSecond()。

    - LocalTime不存储时区和夏令时信息。

  - LocalDateTime类同时处理日期和时间，但没有时区。

    - 这个类是LocalDate和LocalTime的结合，用来表示某个特殊的事件，如纪念九一八事变防空警报是在9月18日9时18分拉响的。

      - 特别注意，像这个9时18分是当地时间。

        - 使用ZonedDateTime或OffsetDateTime类的对象来表示时区时间。

    - 基础的now()方法，of()方法

    - from()方法：将其他时间格式转成LocalDateTime实例

    - plueMonth()，minusMonth()

- 时区和偏移类

  - 每个时区都有一个标识符，格式是region/city（如Asia/Tokyo），还有一个UTC偏移量，如Tokyo的偏移量是+09:00。

  - 日期时间API提供两个类来指定一个时区或偏移量：ZoneId类和ZoneOffset类

    - 前者指定一个时区标识符，并提供了Instant和LocalDateTime对象之间转换的规则

    - 后者指定了时间的偏移量

    - 一般偏移量都是整数个小时，但也有些例外

  - 日期时间类

    - 有三个基于时间的类用来处理时区

      - ZonedDateTime处理日期和时间，并带有一个对应的时区和偏移量

      - OffsetDateTime处理日期和时间并带有一个时间偏移量，没有时区ID

      - OffsetTime只有时间和时间偏移量

      - 何时使用OffsetDateTime代替ZonedDateTime？如果您正在编写复杂的软件，该软件基于地理位置为自己的日期和时间计算规则建模，或者如果您正在将时间戳存储在仅跟踪相对于格林威治/UTC时间的绝对偏移量的数据库中，则可能需要使用OffsetDateTime。此外，XML和其他网络格式将日期时间传输定义为OffsetDateTime或OffsetTime。

    - ZonedDateTime在效果上联合了LocalDateTime和ZoneId，被用来表示完整的时间

    - 配合ZoneRules对象的isDaylightSavings()方法来判断夏令时

    - OffsetDateTime类联合了LocalDateTime类和ZoneOffset类，被用来表示完整的时间（年月日时分秒还有偏移量）

    - OffsetTime类联合了LocalTime和ZoneOffset类，存储了时间和时间偏移量

- Instant类（即时类）

  - 存储的是从epoch以来的纳秒。

    - 这个类可用来生成时间戳和表示机器时间。

  - 在epoch之前的Instant存的是负值，之后的存的是正值

  - 各种方法

    - MAX和MIN常量表示最小时间和最大时间

    - toString()方法返回ISO-8601规范的时间字符串

    - plus()方法和minus()方法，增减时间

    - isAfter()方法和isBefore()方法用来比较

    - until()方法返回两个Instant之间差的时间

  - 如果要算年月日时分秒，需要转成其他对象，转换的时候需要提供Instant的时区

    - 使用其他类的ofInstant()方法可以转成其他类

    - ZonedDateTime和OffsetDateTime对象可以直接转成Instant对象，但反过来不可以，除非提供时区或偏移量信息。

- 解析和格式化

  - 基于时间的类提供了解析字符串中时间信息的方法；也提供了格式化为字符串的方法。

    - 两个过程是类似的：向DateTimeFormatter提供一个模式字符串来创建一个格式对象，然后将格式对象传给解析方法或格式化方法。

  - DateTimeFormatter类提供了许多预先定义好的格式对象。当然我们也可以定义我们自己的。

    - 当在转换过程中发生问题时，解析和格式化方法会抛出异常。

  - DateTimeFormatter类是不可变的，并且是线程安全的。

    - 合适的话，应把它赋值给一个静态常量。

    - java.time日期-时间对象可以直接与java.util.Formatter和String.format一起使用，方法是使用与遗留java.util.Date和java.util.Calendar类一起使用的熟悉的基于模式的格式。

  - parse()

    - LocalDateTime中的一个参数的parse()方法使用ISO_LOCAL_DATE格式对象

      - 要使用不同的格式对象，就用两个参数的parse()方法，第一个参数是模式字符串，第二个参数就是格式对象了。

    - 使用静态方法ofPattern()创建格式对象，传入类似"MMM d yyyy"的模式字符串

      - 模式字符串详细信息：         <https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html#patterns>

    - parse()一般是某个日期时间类的静态方法，传入输入字符串和格式对象，传出对应的日期时间对象

  - format()

    - format()是某个日期时间类的实例方法，传入格式对象，返回字符串。

- java.time.temporal包

  - 这个包提供了许多最底层的接口，我们最好是直接用LocalDate，ZonedDateTime这样的类，而不是实例化那些底层接口

  - Temporal接口

    - 该接口提供了访问日期时间的对象，被那些日期时间的类实现了。

    - 这个接口提供了加减时间单元的方法，使时间算术简单且在各个类之间一致。

    - TemporalAccessor接口提供了只读版本

    - 。。。

  - ChronoField枚举类实现了TemporalField接口

    - 提供了许多常量来访问日期和时间值

    - 如CLOCK_HOUR_OF_DAY，NANO_OF_DAY和DAY_OF_YEAR

    - 这个枚举表示如今年的第三周，今天的第十一各小时这样的时间概念。

      - 日期时间对象.get(枚举值)，返回对应的时间对象

      - 使用对象.isSupported(枚举值)，判断该对象是否支持对应的枚举值

    - 这个枚举提供了基于字段操作日期时间对象的方式

  - ChronoUnit枚举实现了Temporal接口，提供了一系列基于日期时间的从毫秒到千年的单元。

    - 注意不是所有的枚举值都被所有的类支持

      - 类对象.isSupported(枚举值)，判断对象是否支持枚举值

- TemporalAdjuster接口

  - 该接口提供许多方法：传入一个时间值，返回调整后的值。

    - 调整器能被任何日期时间对象使用

    - 如果调制器和ZonedDateTime一起使用，返回新对象，以保留原来的对象

  - 预定义好的调整器

    - TemporalAdjusters（注意复数）类提供了预定义好的调整器

      - 包括找出月的第一天和最后一天，一年的第一天和最后一天，一个月的最后一个星期三，或指定日期之后的第一个星期四。

      - 预定义好的调整期被定义为静态方法，并设计成以静态导入的形式使用。

      - 使用方法是配合对象的with方法使用：date.with(adjuster)

    - 定制自己的调整器

      - TemporalAdjuster是一个函数式接口，要定制调整器，只要编写实现了该接口的类。

        - 调用时再传入实例对象到with()方法中去，和调用预定义调整器相同。

- 时间查询TemporalQuery

  - 时间查询类用来从日期时间对象中检索信息

  - 预定义的查询

    - 如precision查询，返回日期时间对象支持的最小时间单位（如LocalDate最小支持年）

      - TemporalQuery<查询信息类型> query = TemporalQueries.precision();         LocalDate.now().query(query);

    - TemporalQueries类以静态工厂方法的形式预定义了许多时间查询

  - 定制查询

    - 编写实现了TemporalQuery接口的类来定制查询

    - 或者写一个合适的方法，以方法引用的方式调用这个方法。

- 周期和持续时间（Period和Duration）

  - Period类和Duration类，还有ChronoUnit.between()方法可用来表示一段时间

    - Duration基于机器时间，使用纳秒计数。它提供转换为时分秒的方法。

      - 当被创建时使用的时间终点发生在开始点之前时，Duration为负值

    - Duration不和时间线保持联系，它没有时区或夏令时相关信息。

      - 用Duration给ZonedDateTime加一天，就是相当于直接加24小时，直接无视夏令时的某些特殊情况

  - ChronoUnit枚举类预定了常量来计量时间。实例方法between()返回两个时间点之间时间单位的个数

    - 如ChronoUnit.Month((代表2月18号的对象), (4月23号))返回2

  - Period类用来定义一定数量的基于日期的时间值

    - 如果说ChronoUnit以单个时间单位表示时间段，那Period就是以年月日为单位表示时间段，比如用Period表示年龄多大。

    - 如果不要求时区和夏令时等，直接使用LocalDateTime配合Period，如果涉及到时区和夏令时等，使用ZonedDateTime

- Clock

  - 大部分类有无参数方法now()，提供了系统时钟的日期和时间，还有默认的时区。

    - 这些类还有一个now(Clock)方法，我们可以传入一个Clock对象

  - 现在的时间依靠时区，对于全球化的应用，要用Clock类确保日期时间有正确的时区。

    - 虽然Clock是可选的，但这个特性让我们能使用不同的时区测试我们的代码。或使用固定的时钟，让测试时的时间不变

  - Clock是个抽象类，不能创建它的实例对象。有一些工厂方法

    - Clock.offset(Clock，Duration)返回一个偏移了指定Duration的时钟。       Clock.systemUTC()返回表示格林威治/UTC时区的时钟。       Clock.fixed(Instant, ZoneId)始终返回相同的即时。对于这个钟来说，时间静止了。

- 非ISO日期转换

- Java日期时间API总结：

  - Java日期时间API是用来处理日期，时间，时区以及其他历法的一系列包，类和接口的总和。

    - 基于时间的类有许多特定前缀开头的方法，命名规范将前缀和功能紧密联系起来。
