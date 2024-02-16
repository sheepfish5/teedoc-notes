# Java日期和时间API（java.time）

- 日期时间API使用ISO-8601日历系统，基于公历日历系统，全球范围内被用作事实上的日期时间标准

  - 核心类包括LocalDateTime，ZonedDateTime，OffsetDateTime

  - 日期时间API使用 Unicode Common Locale Data Repository (CLDR)，包括了全世界最大的地区数据集合，其中的信息被本地化为了上百种语言

  - 还使用了时区数据库（TZDB），提供了自时区这一概念提出以来的所有信息。

- API设计原则

  - 明确

    - API的表现是明确且可预期的

  - 流畅

    - 大部分方法不允许传入null且不返回null；方法可以链式调用；结果代码能被快速理解

  - 不可变

    - 大部分对象是不可变的，常用带有of，from，with的方法创建对象，没有set方法

  - 可扩展

    - 可定义自己的时间调整器，甚至日历系统

- 日期时间包

  - 整个API由java.time及其四个字包组成

    - Java.time

      - 核心包，有各种类

    - java.time.chrono

      - 代表ISO-8601以外的日历系统

    - java.time.format

      - 有用来格式化和解析的类

    - java.time.temporal

      - 扩展API，主要用于框架和库编写器，允许日期和时间类之间的互操作、查询和调整。字段（TemporalField和ChronoField）和单位（TemporalUnit和ChronoUnit）在此包中定义。

    - java.time.zone

      - 支持时区、时区偏移量和时区规则的类。如果使用时区，大多数开发人员只需要使用ZonedDateTime和ZoneId或ZoneOffset。

- 方法命名规范

  - Date-Time API在一组丰富的类中提供了一组丰富的方法。只要可能，类之间的方法名就保持一致。例如，许多类提供了now方法，该方法捕获与该类相关的当前时刻的日期或时间值。有一些from方法允许从一个类转换到另一个类。
