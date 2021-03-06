---
title: 字符串格式化
date: 2019-09-02 14:57:18
tags: ["python"]
---
python 中的字符串格式化可以以两种方式实现
1. 字符串格式化表达式 : `'...%s...' % (values)`
    这是从Python诞生的时候就有的技术，这一形式是基于C语言的`"printf"`模型，并且在大多数现有的代码中广泛使用.
3. 字符串格式化方法调用: `'...{}...'.format(values)`
    这是Python2.6和Python3.0新增加的技术，这一形式部分地起源于C#/.NET中同名工具，并且和字符串格式化表达式的功能有很大重叠.
<!--more-->

#### 字符串格式化表达式
- 字符串格式化表达式基本定义
  - 基本语法： `'...%s...' % (values)`
  - 基本用法：
    1. 在 `%` 运算符左侧放置一个需要进行格式化的字符串，这个字符串带有一个或多个内嵌的转换目标，都以 `%` 开头(如`%d`)
    2. 在 `%` 运算符右侧放置一个(或多个，内嵌在元组中的)对象，这些对象将会插入到左侧字符串中，并替换一个(或多个)转换目标.
  - 简单示例：
    ```python
    >>> 'That is %d %s bird!' % (1, 'dead')
    'That is 1 dead bird!'
    ```
   该示例中， 运算符 `%` 左侧字符串 `'That is %d %s bird!'` 中的 `%d` 被右侧整数型常量 `1` 替换， `%s` 被右侧字符串常量 `'dead'` 替换，替换后的字符串为 `'That is 1 dead bird!'`
  
- 高级格式化表达式：
  在 `%` 运算符左侧字符串中的替换目标以 `%` 字符为开头后跟类型码来定义替换的数据类型，如 `%d` 用于十进制数字，`%s` 用于字符串等，python所支持的字符串格式化类型码下表所示： 
  
  代码|定义 
  -- | --
  s | 字符串(或任何对象的 `str(x)` 字符串)
  r | 与 `s` 相同，使用 `repr`，而不是 `str`
  c | 字符( `int`或 `str`)
  d | 十进制数字(以 `10` 为底的整数)
  i | 整数
  u | 与 `d` 相同(已废弃，不在是无符号整数)
  o | 八进制整数(以 `8` 为底)
  x | 十六进制整数(以 `16` 为底)
  X | 与 `x` 相同，但使用大写字母
  e | 带有指数的浮点数，小写
  E | 与 `e` 相同，但使用大写字母
  f | 十进制浮点数
  F | 与 `f` 相同，但使用大写字母
  g | 浮点数 `e` 或 `f`
  G | 浮点数 `E` 或 `F`
  % | 字面量(编码为 `%%`)
  
  字符格式化代码格式：
  `%[(keyname)][flags][width][.precision]typecode`
  - `%` 定义开始
  - `(keyname)` 为索引在表达式右侧使用的字典提供键名称(用于基于字典的格式化表达式中)
  - `[flags]` 罗列出说明格式的标签，如左对齐(`-`),数值符号(`+`),整数前的空白以及负数前的`-`(空格)和补充(`0`)
  -  `[width]` 为被替换的文本给出总的最小字段宽度
  -  `[.precision]` 为浮点数设置小数点后显示的位数(精度)
  -  `typecode`   
  
  `[width]` 和 `[.precision]` 部分都可编写为一个 `*`,以指定它们应该从表达式右侧的输入值中的下一项取值(当运行时才会得知这两参数，很有用)
  
- 示例
  1. 简单替换
  ```python
  >>> x = 1234
  >>> 'integers: ...%d...%-6d...%06d' % (x, x, x)
  'integers: ...1234...1234  ...001234'
  ```
  `%d` 默认格式化，`%-6d` 最小宽度为6位字符的左对齐格式化，`%06d` 6位字符补零格式化  
  
  2. 基于字典的格式化表达式
  ```python
  >>> '%(qty)d more %(food)s' % {'qty': 1, 'food': 'spam'}
  '1 more spam'
  ```

#### 字符串格式化方法调用

- 字符串格式化方法基础

  - 定义

    Python2.6、2.7 和 3.x 中可用的字符串对象的 `format` 方法，是基于正常的函数调用语法，而不是表达式语法。特别的它使用主体字符串作为模板，并接受任意多个参数，用来表示将要根据模板替换的值。

    它的使用要求具备函数和调用的知识、但其使用多半是清晰易懂的。在主体字符串中，花括号的位置 (如 {1})、关键字 (如 {food})，或 Python 2.7、3.1 以及以后版本中的相对位置 （{}）来指定替换目标及将要插入的参数。

  - 示例

    \# By position

    ```python
    >>> template = '{0}, {1} and {2}'
    >>> template.format('spam', 'ham', 'eggs')
    'spam, ham and eggs'
    ```

    \# By keyword

    ```python
    >>> template = '{motto}, {pork} and {food}'
    >>> template.format(motto='spam', pork='ham', food='eggs')
    'spam, ham and eggs'
    ```

- 高级格式化方法语法

  另一种和 `%` 表达方式类似的是，可在格式化字符串中添加额外的语法来实现更具体的层级。

  对于格式化方法，我们可能为空的替换目标的识别码之后使用一个冒号，后面跟着可以指定的字段大小，对齐方式和特定类型编码的格式说明符。

  

  **语法**：

  `{fieldname component !conversionflag :formatspec}`

  - `fieldname` 是辨识参数的一个可选的数字或关键字

  - `component` 是有着大于等于零个 `.name` 或 `[index]` 引用字符串，它可以被省略以使用完整的参数值。其中的引用用来获取参数的属性或索引值。

  - `conversionflag` 如果出现则以`!` 开始，后面跟着`r`、`s` 或者`a` , 在这个值上分别调用 `repr` 、`str` 或 `ascii` 内置函数.

  - `formatspec` 如果出现则以`:`开始，后面跟着文本，指定了如何表示该值，包括字段宽、对其方式、补零、小数精度等细节，并且以一个可选的数据类型码结束。

    格式：

    `[[fill]align][sign][#][0][width][,][.precision][typecode]`

    - `fill` 可以是除`{` 或`}` 之外的任意字符填充
    - `align` 可以是 `<` 、 `>` 、`=` 或 `^` , 分别表示 左对齐、右对齐、符号字符后的填充，或居中对齐。
    - `sign` 可以是 `+` 、 `-`  或空格
    - `width` 、 `precision` 和 `typecode`  与  `%` 表达式中时一样

  **示例**

  \# 字段宽度

  ```python
  >>> '{0:10} = {1:10}'.format('spam', 123.4567)
  'spam       =   123.4567'
  ```

  \# 对齐方式

  ```python
  >>> '{0:>10} = {1:<10}'.format('spam', 123.4567)
  '      spam = 123.4567  '
  ```

  \# 参数索引

  ```python
  >>> import sys
  >>> '{0.platform:>10} = {1[kind]:<10}'.format(sys, dict(kind='laptop'))
  '     linux = laptop    '
  ```

  

