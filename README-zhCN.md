# 序幕

> 是风格区分了好和伟大。 <br/>
> -- Bozhidar Batsov

作为 Ruby 开发者，有一件总是令我烦心的事 &mdash; Python 开发者有一份好的编程风格参考指南([PEP-8](http://www.python.org/dev/peps/pep-0008/)) 而我们永远没有一份官方指南，一份记录 Ruby 编程风格及最佳实践的指南。而我们确信风格很重要。我也相信这些好家伙们，像我们这些 Ruby 开发者，应该可以自己写一份这个梦寐以求的文档。

这份指南开始是作为我们公司内部 Ruby 编程指南(由我所写的)。进行到某个部分时，我决定要把我的成果贡献给广大的 Ruby 社区，而且这个世界需要从另一个公司内部的一点帮助。然而这个世界也可以从由社区制定及策动的一系列 Ruby 编程惯例、实践及风格中受益。

在开始写这份指南时，我收到世界上很多优秀 Ruby 社区用户们的反馈。感谢所有的建议及帮助！我们同心协力创造一个能够让每一个 Ruby 开发者受益的资源。

顺道一提，如果你对 Rails 感兴趣，你可以看看这份互补的 [Ruby on Rails 3 风格指南](https://github.com/bbatsov/rails-style-guide)。

# Ruby 风格指南

这份 Ruby 风格指南向你推荐现实世界中的最佳实践，Ruby 程序员如何写出可被别的 Ruby 程序员维护的代码。一份风格指南反映出现实世界中的用法，并带有一个理想，避免已经公认是危险的事物不被人继续使用 –– 不管看起来是多么的好。

本指南依照相关规则分成数个小节。我尽力在规则后面说明理由（如果省略的话，我相信理由是显而易见的）。

我没有想到所有的规则 &mdash; 他们大致上是基于，我作为一个专业软体工程师的广泛生涯，从 Ruby 社区成员所得到的反馈及建议，和数个高度评价的 Ruby 编程资源，像是 ["Programming Ruby 1.9"](http://pragprog.com/book/ruby4/programming-ruby-1-9-2-0) 以及 ["The Ruby Programming Language"](http://www.amazon.com/Ruby-Programming-Language-David-Flanagan/dp/0596516177)。

本指南仍在完善中 — 某些规则缺乏实例，某些规则没有例子来清楚地演示它们。在最后交付时，将会解决这些议题 — 现在就先把它们记在心理吧。

你可以使用 [Transmuter](https://github.com/TechnoGate/transmuter) 来产生本指南的一份 PDF 或 HTML 复本。

[rubocop](https://github.com/bbatsov/rubocop) 项目会自动检查你的 Ruby 代码是否符合这份 Ruby 风格指南。目前这个项目尚有许多功能缺漏，不足以被正式地使用，欢迎有志之士协助改进。

本指南被翻译成下列语言：

* [简体中文](https://github.com/JuanitoFatas/ruby-style-guide/blob/master/README-zhCN.md)
* [繁體中文](https://github.com/JuanitoFatas/ruby-style-guide/blob/master/README-zhTW.md)
* [法文](https://github.com/porecreat/ruby-style-guide/blob/master/README-frFR.md)

## 目录

* [源代码排版](#-2)
* [语法](#-3)
* [命名](#-4)
* [注释](#-5)
    * [注解](#-6)
* [类别](#-7)
* [异常](#-8)
* [集合](#-9)
* [字符串](#-10)
* [正则表达式](#-11)
* [百分比字面](#-12)
* [元编程](#-13)
* [其它](#-14)
* [工具](#-15)

## 源代码排版

> 几乎每人都深信，每一个除了自己的风格都又丑又难读。把 "除了自己的" 拿掉，他们或许是对的...<br/>
> -- Jerry Coffin (论缩排)

* 使用 `UTF-8` 作为源文件的编码。
* 每个缩排层级使用两个 **空格**。不要使用 Hard Tabs。

    ```Ruby
    # 差 - 四个空格
    def some_method
        do_something
    end

    # 好
    def some_method
      do_something
    end
    ```

* 使用 Unix 风格的行编码(BSD/Solaris/Linux/OSX 的用户不用担心，Windows 用户要格外小心。)
    * 如果你使用 Git ，你也许会想加入下面这个配置，来保护你的项目不被 Windows 的行编码侵入：

      ```bash
      $ git config --global core.autocrlf true
      ```

* 不要使用`;`来隔开语句和表达式。推论 - 每一行使用一条语句。

    ```Ruby
    # bad
    puts 'foobar'; # 不必要的分号

    puts 'foo'; puts 'bar' # 同一行里有两个表达式

    # good
    puts 'foobar'

    puts 'foo'
    puts 'bar'

    puts 'foo', 'bar' # 特别的，这个适用于puts
    ```

* 对于没有成员的类，尽可能使用单行类定义。

    ```Ruby
    # bad
    class FooError < StandardError
    end

    # good
    class FooError < StandardError; end
    ```

* 避免使用单行方法。尽管它们在圈子里有那么一点点流行，但是它们的定义语法有一些诡异的特性导致使用它们时并不尽如人意。无论如何 - 一个单行方法里的表达式不应该多于 1 个。

    ```Ruby
    # 差
    def too_much; something; something_else; end

    # 勉强可以 - 注意第一个 ; 是必需的
    def no_braces_method; body end

    # 勉强可以 - 注意第二个 ; 是可选的
    def no_braces_method; body; end

    # 勉强可以 - 语法上正确，但是没有 ; 让它有些难读
    def some_method() body end

    # 好
    def some_method
      body
    end
    ```

    这个规则的一个例外是空方法。

    ```Ruby
    # good
    def no_op; end
    ```

* 使用空格来围绕操作符，逗号 `,` 、冒号 `:` 及分号 `;` 之后，围绕在 `{` 和 `}` 之前。
  空格可能对（大部分）Ruby 直译器来说是无关紧要的，但正确的使用是写出可读性高的代码的关键。

    ```Ruby
    sum = 1 + 2
    a, b = 1, 2
    1 > 2 ? true : false; puts 'Hi'
    [1, 2, 3].each { |e| puts e }
    ```
    （针对操作符）唯一的例外是当使用指数操作符时：

    ```Ruby
    # 差
    e = M * c ** 2

    # 好
    e = M * c**2
    ```

    `{` and `}` deserve a bit of clarification, since they are used
    for block and hash literals, as well as embedded expressions in
    strings. For hash literals two styles are considered acceptable.

    ```Ruby
    # good - space after { and before }
    { one: 1, two: 2 }

    # good - no space after { and before }
    {one: 1, two: 2}
    ```

    The first variant is slightly more readable (and arguably more
    popular in the Ruby community in general). The second variant has
    the advantage of adding visual difference between block and hash
    literals. Whichever one you pick - apply it consistently.

    As far as embedded expressions go, there are also two acceptable
    options:

    ```Ruby
    # good - no spaces
    "string#{expr}"

    # ok - arguably more readable
    "string#{ expr }"
    ```

    The first style is extremely more popular and you're generally
    advised to stick with it. The second, on the other hand, is
    (arguably) a bit more readable. As with hashes - pick one style
    and apply it consistently.

* 不要有空格在 `(` 、 `[` 之后，或 `]` 、 `)` 之前。

    ```Ruby
    some(arg).other
    [1, 2, 3].length
    ```
* 把 `when` 跟 `case` 缩排在同一层。我知道很多人不同意这一点，但这是 "The Ruby Programming Language" 及 "Programming Ruby" 所使用的风格。

    ```Ruby
    case
    when song.name == 'Misty'
      puts 'Not again!'
    when song.duration > 120
      puts 'Too long!'
    when Time.now.hour > 21
      puts "It's too late"
    else
      song.play
    end

    kind = case year
           when 1850..1889 then 'Blues'
           when 1890..1909 then 'Ragtime'
           when 1910..1929 then 'New Orleans Jazz'
           when 1930..1939 then 'Swing'
           when 1940..1950 then 'Bebop'
           else 'Jazz'
           end
    ```

* 在 `def` 之间使用空行，并且把方法分成合乎逻辑的段落。

    ```Ruby
    def some_method
      data = initialize(options)

      data.manipulate!

      data.result
    end

    def some_method
      result
    end
    ```

* 当给方法的参数赋默认值时，在 `=` 两边使用空格：

    ```Ruby
    # 差
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # do something...
    end

    # 好
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # do something...
    end
    ```

    While several Ruby books suggest the first style, the second is much more prominent
    in practice (and arguably a bit more readable).

* 避免在不需要的时候使用行继续符 (\\) 。实际编码时，尽力避免使用行继续符。

    ```Ruby
    # 差
    result = 1 - \
             2

    # 好 (但是仍然丑到爆)
    result = 1 \
             - 2
    ```

* 当一个链式方法调用需要在另一行继续时，将 `.` 放在第二行

    ```Ruby
    # 差 - 为了理解第二行需要去查阅第一行
    one.two.three.
      four

    # 好 - 第二行在做什么立刻变得很清晰
    one.two.three
      .four
    ```

* 当一个方法呼叫的参数扩展超过一行时，排列它们。

    ```Ruby
    # 一开始（一行太长）
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
    end

    # 差（一般的缩排）
    def send_mail(source)
      Mailer.deliver(
        to: 'bob@example.com',
        from: 'us@example.com',
        subject: 'Important message',
        body: source.text)
    end

    # 差（两倍缩排）
    def send_mail(source)
      Mailer.deliver(
          to: 'bob@example.com',
          from: 'us@example.com',
          subject: 'Important message',
          body: source.text)
    end

    # 好
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com',
                     from: 'us@example.com',
                     subject: 'Important message',
                     body: source.text)
    end
    ```
* 长的常量数字添加下划线来改善可读性

    ```Ruby
    # 差 - 有几个零？
    num = 1000000

    # 好 - 更容易被人脑解析。
    num = 1_000_000
    ```

* 使用 RDoc 以及它的惯例来撰写 API 文档。不要在注解区块及 `def` 之前放一个空行。
* 将每一行最多限制在 80 个字符。
* 避免尾随的空白。
* 不要使用区块注释。它们不能由空白引导，并且不如普通注释容易辨认。

    ```Ruby
    # 差
    == begin
    comment line
    another comment line
    == end

    # 好
    # comment line
    # another comment line
    ```

## 语法

* 使用 `::` 只能引用常量（包括类和模块）。永远不要使用 `::` 来调用方法。

    ```Ruby
    # 差
    SomeClass::some_method
    some_object::some_method

    # 好
    SomeClass.some_method
    some_object.some_method
    SomeModule::SomeClass::SOME_CONST
    ```

* 使用 `def` 时，当有参数时使用括号。当方法不接受任何参数时，省略括号。

     ```Ruby
     def some_method
       # body omitted
     end

     def some_method_with_arguments(arg1, arg2)
       # body omitted
     end
     ```

* 永远不要使用 `for` ，除非你很清楚为什么。大部分情况应该使用迭代器来取代。 `for` 是由 `each` 所实现的（所以你加入了一层的迂回），但出乎意料的是 — `for` 并没有包含一个新的作用域(不像是 `each` ）而在这个区块中定义的变量将会被外部所看到。

    ```Ruby
    arr = [1, 2, 3]

    # 差
    for elem in arr do
      puts elem
    end

    # 好
    arr.each { |elem| puts elem }
    ```

* 永远不要在多行的 `if/unless` 使用 `then`

    ```Ruby
    # 差
    if some_condition then
      # body omitted
    end

    # 好
    if some_condition
      # body omitted
    end
    ```

* 偏爱三元操作符 `? : ` 胜于 `if/then/else/end` 结构
* 它更为常见及更精准。

    ```Ruby
    # 差
    result = if some_condition then something else something_else end

    # 好
    result = some_condition ? something : something_else
    ```
* 使用一个表达式给一个三元操作符的分支。这也意味着三元操作符不要嵌套。嵌套情况使用 `if/else` 结构。

    ```Ruby
    # 差
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

    # 好
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```
* 永远不要使用 `if x: ...` — 它已经在 Ruby 1.9 被移除了。使用三元操作符来取代。

    ```Ruby
    # 差
    result = if some_condition: something else something_else end

    # 好
    result = some_condition ? something : something_else
    ```
* 永远不要使用 `if x: ...` 使用三元操作符来取代。

* 一行的情况使用 `when x then ...`。替代方案的语法 `when x: ...` 已经在 Ruby 1.9 被移除了。

* 永远不要使用 `when x: ...`。参考前一个规则。

* 使用 `!` 替代 `not`.

    ```Ruby
    # 差 - 因为操作符有优先级，需要用括号。
    x = (not something)

    # 好
    x = !something
    ```

* 布尔表达式使用 `&&/||`，控制流程使用 `and/or`。 （经验法则：如果你需要使用外部括号，你正在使用错误的操作符。）

    ```Ruby
    # 布尔表达式
    if some_condition && some_other_condition
      do_something
    end

    # 控制流程
    document.saved? or document.save!
    ```
* 避免多行的 `? : `（三元操作符）；使用 `if/unless` 来取代。

* 偏爱 `if/unless` 修饰符当你有单行的主体。另一个好的方法是使用控制流程的 `and/or`。

    ```Ruby
    # 差
    if some_condition
      do_something
    end

    # 好
    do_something if some_condition

    # 另一个好方法
    some_condition and do_something
    ```

* 否定条件偏爱 `unless` 优于 `if`（或是控制流程 `or`）。

    ```Ruby
    # 差
    do_something if !some_condition

    # 好
    do_something unless some_condition

    # 另一个好方法
    some_condition or do_something
    ```
* 永远不要使用 `unless` 搭配 `else` 。将它们改写成肯定条件。

    ```Ruby
    # 差
    unless success?
      puts 'failure'
    else
      puts 'success'
    end

    # 好
    if success?
      puts 'success'
    else
      puts 'failure'
    end
    ```
* 不要使用括号围绕 `if/unless/while` 的条件式，除非这条件包含了一个赋值（见下面使用 `=` （一个赋值）的返回值）。

    ```Ruby
    # 差
    if (x > 10)
      # body omitted
    end

    # 好
    if x > 10
      # body omitted
    end

    # 好
    if (x = self.next_value)
      # body omitted
    end
    ```

* 当你有单行主体时，偏爱使用 `while/until` 修饰符。

    ```Ruby
    # bad
    while some_condition
      do_something
    end

    # good
    do_something while some_condition
    ```

* 负面条件偏爱 `until` 胜于 `while` 。

    ```Ruby
    # bad
    do_something while !some_condition

    # good
    do_something until some_condition
    ```

* Use Kernel#loop with break rather than `begin/end/until` or `begin/end/while` for post-loop tests.

   ```Ruby
   # bad
   begin
     puts val
     val += 1
   end while val < 0

   # good
   loop do
     puts val
     val += 1
     break unless val < 0
   end
   ```

* 忽略围绕方法参数的括号，如内部 DSL (如：Rake, Rails, RSpec)，Ruby 中带有 "关键字" 状态的方法（如：`attr_reader`, `puts`）以及属性存取方法。所有其他的方法呼叫使用括号围绕参数。

    ```Ruby
    class Person
      attr_reader :name, :age

      # 忽略
    end

    temperance = Person.new('Temperance', 30)
    temperance.name

    puts temperance.age

    x = Math.sin(y)
    array.delete(e)
    ```

* 单行区块喜好 `{...}` 胜于 `do..end`。多行区块避免使用 `{...}`（多行串连总是​​丑陋）。在 `do...end` 、 "控制流程" 及 "方法定义" ，永远使用 `do...end` （如 Rakefile 及某些 DSL）。串连时避免使用 `do...end`。

    ```Ruby
    names = ['Bozhidar', 'Steve', 'Sarah']

    # 差
    names.each do |name|
      puts name
    end

    # 好
    names.each { |name| puts name }

    # 差
    names.select do |name|
      name.start_with?('S')
    end.map { |name| name.upcase }

    # 好
    names.select { |name| name.start_with?('S') }.map { |name| name.upcase }
    ```
    某些人会争论多行串连时，使用 `{...}` 看起来还可以，但他们应该扪心自问— 这样代码真的可读吗？难道不能把区块内容取出来放到小巧的方法里吗？

* 避免在不需要控制流程的场合时使用 `return` 。


    ```Ruby
    # 差
    def some_method(some_arr)
      return some_arr.size
    end

    # 好
    def some_method(some_arr)
      some_arr.size
    end
    ```

* 避免在不需要的情况使用 `self` 。（只有在调用一个 self write 访问器时会需要用到。）

    ```Ruby
    # 差
    def ready?
      if self.last_reviewed_at > self.last_updated_at
        self.worker.update(self.content, self.options)
        self.status = :in_progress
      end
      self.status == :verified
    end

    # 好
    def ready?
      if last_reviewed_at > last_updated_at
        worker.update(content, options)
        status = :in_progress
      end
      status == :verified
    end
    ```

* 避免使用带有局域变量的 shadowing 方法，除非它们彼此相等。

    ```Ruby
    class Foo
      attr_accessor :options

      # ok
      def initialize(options)
        self.options = options
        # both options and self.options are equivalent here
      end

      # 差
      def do_something(options = {})
        unless options[:when] == :later
          output(self.options[:message])
        end
      end

      # 好
      def do_something(params = {})
        unless params[:when] == :later
          output(options[:message])
        end
      end
    end
    ```

* 当赋予缺省值给方法参数时，使用空格围绕 `=` 操作符。

    ```Ruby
    # 差
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # 做些什么...
    end

    # 好
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # 做些什么...
    end
    ```

    然而几本 Ruby 书建议第一个风格，第二个风格在实践中更为常见（并可争议地可读性更高一点）。

* 避免在不需要的场合使用续行 `\` 。在实践中，尽量避免使用续行。

    ```Ruby
    # 差
    result = 1 - \
             2

    # 好 (但仍丑的跟地狱一样）
    result = 1 \
             - 2
    ```

* 不要在条件表达式里使用 `=` （赋值）的返回值。

    ```Ruby
    # bad (+ a warning)
    if (v = array.grep(/foo/))
      do_something(v)
      ...
    end

    # bad (+ a warning)
    if v = array.grep(/foo/)
      do_something(v)
      ...
    end

    # good
    v = array.grep(/foo/)
    if v
      do_something(v)
      ...
    end
    ```

* 放心地使用 `||=` 来初始化变量

    ```Ruby
    # 仅在 name 为 nil 或 false 时，把名字设为 Bozhidar。
    name ||= 'Bozhidar'
    ```

* 不要使用 `||=` 来初始化布尔变量。 （想看看如果现在的值刚好是 `false` 时会发生什么。）

    ```Ruby
    # 差 — 会把 enabled 设成真，即便它本来是假。
    enabled ||= true

    # 好
    enabled = true if enabled.nil?
    ```
* 避免使用 Perl 风格的特殊变量（像是 `$0-9`, `$`, 等等）。它们看起来非常神秘而不鼓励使用，除非用于单行脚本。

* 避免在方法名与左括号之间放一个空格。

    ```Ruby
    # 差
    f (3 + 2) + 1

    # 好
    f(3 + 2) + 1
    ```
* 如果方法的第一个参数由左括号开始的，则此方法调用应该使用括号。举个例子，如 `f((3+2) + 1)`。

* 总是使用 `-w` 来执行 Ruby 解释器，如果你忘了某个上述的规则，它就会警告你！

* 使用新的 lambda 字面语法。

    ```Ruby
    # 差
    lambda = lambda { |a, b| a + b }
    lambda.call(1, 2)

    # 好
    lambda = ->(a, b) { a + b }
    lambda.(1, 2)
    ```
* 未使用的区块参数使用 `_` 。

    ```Ruby
    # 差
    result = hash.map { |k, v| v + 1 }

    # 好
    result = hash.map { |_, v| v + 1 }
    ```

* Use `$stdout/$stderr/$stdin` instead of
  `STDOUT/STDERR/STDIN`. `STDOUT/STDERR/STDIN` are constants, and
  while you can actually reassign (possibly to redirect some stream)
  constants in Ruby, you'll get an interpreter warning if you do so.

* Use `warn` instead of `$stderr.puts`. Apart from being more concise
and clear, `warn` allows you to suppress warnings if you need to (by
setting the warn level to 0 via `-W0`).

* Favor the use of `sprintf` over the fairly cryptic `String#%` method.

    ```Ruby
    # bad
    '%d %d' % [20, 10]
    # => '20 10'

    # good
    sprintf('%d %d', 20, 10)
    # => '20 10'
    ```

* Favor the use of `Array#join` over the fairly cryptic `Array#*` with
  a string argument.

    ```Ruby
    # bad
    %w(one two three) * ', '
    # => 'one, two, three'

    # good
    %w(one two three).join(', ')
    # => 'one, two, three'
    ```

* Use `[*var]` or `Array()` instead of explicit `Array` check, when dealing with a
  variable you want to treat as an Array, but you're not certain it's
  an array.

    ```Ruby
    # bad
    paths = [paths] unless paths.is_a? Array
    paths.each { |path| do_something(path) }

    # good
    [*paths].each { |path| do_something(path) }

    # good (and a bit more readable)
    Array(paths).each { |path| do_something(path) }
    ```

* 如果可能，使用范围来替换复杂的逻辑比较。

    ```Ruby
    # 差
    do_something if x >= 1000 && x < 2000

    # 好
    do_something if (1000...2000).include?(x)
    ```

## 命名

> 程式设计的真正难题是替事物命名及使缓存失效。 <br/>
> -- Phil Karlton

* 标识符用英语命名。

    ```Ruby
    # 差 - 變數名用帶有拉丁文的保加利亞語寫成。
    zaplata = 1_000

    # 好
    salary = 1_000
    ```

* 符号、方法与变量使用蛇底式小写（snake_case）。

    ```Ruby
    # 差
    :'some symbol'
    :SomeSymbol
    :someSymbol

    someVar = 5

    def someMethod
      ...
    end

    def SomeMethod
     ...
    end

    # 好
    :some_symbol

    def some_method
      ...
    end
    ```

* 类别与模组使用驼峰式大小写（CamelCase）。 （保留类似 HTTP、RFC、XML 这种缩写为大写）

    ```Ruby
    # 差
    class Someclass
      ...
    end

    class Some_Class
      ...
    end

    class SomeXml
      ...
    end

    # 好
    class SomeClass
      ...
    end

    class SomeXML
      ...
    end
    ```

* 其他常数使用尖叫蛇底式大写（SCREAMING_SNAKE_CASE）。

    ```Ruby
    # 差
    SomeConst = 5

    # 好
    SOME_CONST = 5
    ```

* 判断式方法的名字（返回布尔值的方法）应以问号结尾。 (即 `Array#empty?` )
* 有潜在“危险性”的方法，若此 *危险* 方法有安全版本存在时，应以安全版本名加上惊叹号结尾（即：改动 `self` 或参数、 `exit!` 等等方法）。

* The names of potentially *dangerous* methods (i.e. methods that
  modify `self` or the arguments, `exit!` (doesn't run the finalizers
  like `exit` does), etc.) should end with an exclamation mark if
  there exists a safe version of that *dangerous* method.


    ```Ruby
    # 不好 - 没有对应的安全方法
    class Person
      def update!
      end
    end

    # 好
    class Person
      def update
      end
    end

    # 好
    class Person
      def update!
      end

      def update
      end
    end
    ```

* 如果可能的话，根据危险方法（bang）来定义对应的安全方法（non-bang）。

    ```Ruby
    class Array
      def flatten_once!
        res = []

        each do |e|
          [*e].each { |f| res << f }
        end

        replace(res)
      end

      def flatten_once
        dup.flatten_once!
      end
    end
    ```

* 在短的区块使用 `reduce` 时，把参数命名为 `|a, e|` (累加器，元素)
* 在定义二元操作符时，把参数命名为 `other` （`<<` 与 `[]` 是这条规则的例外，因为它们的语义不同）。

    ```Ruby
    def +(other)
      # body omitted
    end
    ```
* When defining binary operators, name the argument `other`(`<<` and
  `[]` are exceptions to the rule, since their semantics are different).

    ```Ruby
    def +(other)
      # body omitted
    end
    ```


* 偏好 `map` 胜于 `collect` ， `find` 胜于 `detect` ， `select` 胜于 `find_all` ， `reduce` 胜于 `inject` 以及 `size` 胜于 `length` 。这不是一个硬性要求；如果使用别名增加了可读性，使用它没关系。这些有押韵的方法名是从 Smalltalk 继承而来，在别的语言不通用。鼓励使用 `select` 而不是 `find_all` 的理由是它跟 `reject` 搭配起来是一目了然的。

* 使用 `flat_map` 胜于使用 `map` + `flatten` 的组合。
  这并不适用于深度大于 2 的数组，举个例子，如果 `users.first.songs == ['a', ['b', 'c']]` ，则使用 `map + flatten` 的组合，而不是使用 `flat_map` 。
  `flat_map` 将数组变平坦一个层级，而 `flatten` 会将整个数组变平坦。

    ```Ruby
    # bad
    all_songs = users.map(&:songs).flatten.uniq

    # good
    all_songs = users.flat_map(&:songs).uniq
    ```

## 注释

> 良好的代码是最佳的文档。当你要加一个注释时，扪心自问，<br/>
> "如何改善代码让它不需要注释？" 改善代码然后文档化它们以使之更清楚。<br/>
> -- Steve McConnell

* 编写自我解释含义的代码然后忽略这一节的其它部分。我是认真的！
* 用英语写注释。
* 在注释的 `#` 与注释文字之间使用一个空格。
* 比一个单词长的注释要大写开头并使用标点符号。句号后使用[一个空格](http://en.wikipedia.org/wiki/Sentence_spacing)。
* 避免冗赘的注释

    ```Ruby
    # 差
    counter += 1 # increments counter by one
    ```
* 保持现有的注释是最新的。过时的注解比没有注解还差。
> 好代码就像是好的笑话 - 它不需要解释 <br/>
> -- Russ Olsen
* 避免替烂代码写注释。重构代码让它们看起来一目了然。 （要嘛就做，要嘛不做― 不要只是试试看。-- Yoda）

### 注解

* 注解应该直接写在相关代码那行之前。
* 注解关键字后面，跟着一个冒号及空格，接着是一个描述问题的说明。
* 如果需要用多行来描述问题，之后的行要放在 `#` 号后面并缩排两个空格。

    ```Ruby
    def bar
      # FIXME: 这在v3.2.1 版本之后会异常崩溃，或许与
      #   BarBazUtil 的版本更新有关
      baz(:quux)
    end
    ```
* 在问题是显而易见的情况下，任何的文档会是多余的，注解应放在有问题的那行的最后，并且不需更多说明。这个用法应该是例外而不是规则。

    ```Ruby
    def bar
      sleep 100 # OPTIMIZE
    end
    ```
* 使用 `TODO` 来标记以后应被加入的未实现特征与功能。
* 使用 `FIXME` 来标记一个需要修复的会引起错误的代码。
* 使用 `OPTIMIZE` 来标记可能影响性能的缓慢或效率低下的代码。
* 使用 `HACK` 来标记代码异味，其中包含了可疑的编码实践以及应该需要重构。
* 使用 `REVIEW` 来标记任何需要审查及确认正常动作的地方。举例来说： `REVIEW: 我们确定用户现在是这么做的吗？ `
* 如果你觉得恰当的话，可以使用其他定制的注解关键字，但记得把它们文档化在项目的 `README` 或类似的地方。

## 类与模块

* 在类别定义里使用一致的结构。

    ```Ruby
    class Person
      # 首先是 extend 与 include
      extend SomeModule
      include AnotherModule

      # 接着是常量
      SOME_CONSTANT = 20

      # 接下来是属性宏
      attr_reader :name

      # 跟着是其它的宏（如果有的话）
      validates :name

      # 公开的类别方法接在下一行
      def self.some_method
      end

      # 跟着是公开的实例方法
      def some_method
      end

      # 受保护及私有的方法，一起放在接近结尾的地方
      protected

      def some_protected_method
      end

      private

      def some_private_method
      end
    end
    ```

* 偏好模块，胜过只有类别方法的类。类别应该只在产生实例是合理的时候使用。

    ```Ruby
    # 差
    class SomeClass
      def self.some_method
        # body omitted
      end

      def self.some_other_method
      end
    end

    # 好
    module SomeClass
      module_function

      def some_method
        # body omitted
      end

      def some_other_method
      end
    end
    ```

* 当你想将模块的实例方法变成类别方法时，偏爱使用 `module_function` 胜过 `extend self` 。

    ```Ruby
    # 差
    module Utilities
      extend self

      def parse_something(string)
        # do stuff here
      end

      def other_utility_method(number, string)
        # do some more stuff
      end
    end

    # 好
    module Utilities
      module_function

      def parse_something(string)
        # do stuff here
      end

      def other_utility_method(number, string)
        # do some more stuff
      end
    end
    ```

* 当设计类型层级时，确认它们符合[Liskov 替换原则](http://en.wikipedia.org/wiki/Liskov_substitution_principle)。
* 尽可能让你的类型越[SOLID](http://en.wikipedia.org/wiki/SOLID_(object-oriented_design\))越好。
* 永远替类型提供一个适当的 `to_s` 方法给来表示领域模型。

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def to_s
        "#{@first_name #@last_name"}
      end
    end
    ```
* 使用 `attr` 系列函数来定义琐碎的 accessor 或 mutators。

    ```Ruby
    # 差
    class Person
      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def first_name
        @first_name
      end

      def last_name
        @last_name
      end
    end

    # 好
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end
    ```

* 考虑使用 `Struct.new`，它替你定义了那些琐碎的存取器（accessors），构造器（constructor）以及比较操作符（comparison operators）。

    ```Ruby
    # 好
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end

    # 更好
    Person = Struct.new(:first_name, :last_name) do
    end
    ````
* 考虑加入工厂方法以提供附加的有意义的方式来生成一个特定的类实例。

    ```Ruby
    class Person
      def self.create(options_hash)
        # body omitted
      end
    end
    ```

* 偏好[Duck Typing](http://en.wikipedia.org/wiki/Duck_typing)胜于继承。

    ```Ruby
    ## 差
    class Animal
      # 抽象方法
      def speak
      end
    end

    # 继承超类
    class Duck < Animal
      def speak
        puts 'Quack! Quack'
      end
    end

    # 继承超类
    class Dog < Animal
      def speak
        puts 'Bau! Bau!'
      end
    end

    ## 好
    class Duck
      def speak
        puts 'Quack! Quack'
      end
    end

    class Dog
      def speak
        puts 'Bau! Bau!'
      end
    end
    ```

* 由于类变量在继承中产生的 "讨厌的" 行为，避免使用类变量( `@@` )。

    ```Ruby
    class Parent
      @@class_var = 'parent'

      def self.print_class_var
        puts @@class_var
      end
    end

    class Child < Parent
      @@class_var = 'child'
    end

    Parent.print_class_var # => will print "child"
    ```

    如同你所看到的，在类型层级中的所有类其实都共享单独一个类变量。通常情况下应该偏好使用实例变量而不是类变量。

* 依据方法的目的用途指定适当的可见层级(`private` ,`protected` )。别把所有方法都设为 `public` （方法的缺省值）。我们现在是在写 *Ruby* ，不是 *Python* 。
* 将 `public`, `protected`, `private` 和被应用的方法定义保持一致的缩排。在上下各留一行来强调这个可见性应用于之后的所有方法。）

    ```Ruby
    class SomeClass
      def public_method
        # ...
      end

      private

      def private_method
        # ...
      end

      def another_private_method
        # ...
      end
    end
    ```

* 使用`def self.method` 来定义 singleton 方法。由于类的名称不会重复的关系，这使得代码更容易重构。

    ```Ruby
    class TestClass
      # 差
      def TestClass.some_method
        # body omitted
      end

      # 好
      def self.some_other_method
        # body omitted
      end

      # 也有可能及当你要定义多个
      # singleton时的便利方法
      class << self
        def first_method
          # body omitted
        end

        def second_method_etc
          # body omitted
        end
      end
    end
    ```

## 异常

* 使用 `fail` 方法来抛出异常。仅在捕捉到异常时使用 `raise` 来重新抛出异常（因为没有失败，所以只是显式地有目的性地抛出一个异常）

    ```Ruby
    begin
     fail 'Oops';
    rescue => error
      raise if error.message != 'Oops'
    end
    ```

* 永远不要从 `ensure` 区块返回。如果你显式地从 `ensure` 区块中的一个方法返回，那么这方法会如同没有异常般的返回。实际上，异常会被默默丢掉。

    ```Ruby
    def foo
      begin
        fail
      ensure
        return 'very bad idea'
      end
    end
    ```

* 尽可能使用隐式的 `begin` 区块。

    ```Ruby
    # bad
    def foo
      begin
        # main logic goes here
      rescue
        # failure handling goes here
      end
    end

    # good
    def foo
      # main logic goes here
    rescue
      # failure handling goes here
    end
    ```

* 通过 *contingency* 方法 (一个由 Avdi Grimm 创造的词)来减少 `begin` 区块的使用。

    ```Ruby
    # 差
    begin
      something_that_might_fail
    rescue IOError
      # handle IOError
    end

    begin
      something_else_that_might_fail
    rescue IOError
      # handle IOError
    end

    # 好
    def with_io_error_handling
       yield
    rescue IOError
      # handle IOError
    end

    with_io_error_handling { something_that_might_fail }

    with_io_error_handling { something_else_that_might_fail }
    ```

* 不要封锁异常。

    ```Ruby
    begin
      # 这里发生了一个异常
    rescue SomeError
      # 拯救子句完全没有做事
    end

    # bad
    do_something rescue nil
    ```

* 避免使用 `rescue` 的修饰符形式。

    ```Ruby
    # 差 - 这捕捉了所有的 StandardError 异常。
    do_something rescue nil
    ```

* 不要为了控制流程而使用异常。

    ```Ruby
    # 差
    begin
      n / d
    rescue ZeroDivisionError
      puts 'Cannot divide by 0!'
    end

    # 好
    if d.zero?
      puts 'Cannot divide by 0!'
    else
      n / d
    end
    ```

* 避免救援 `Exception` 类别。这会把信号困住，并呼叫 `exit`，导致你需要 `kill -9` 进程。

    ```Ruby
    # 差
    begin
      # 呼叫 exit 及杀掉信号会被捕捉（除了 kill -9）
      exit
    rescue Exception
      puts "you didn't really want to exit, right?"
      # 异常处理
    end

    # 好
    begin
      # 一个不明确的rescue子句捕捉的是StandardError，
      #   而不是许多编程者所设想的Exception。
    rescue => e
      # 异常处理
    end

    # 也好
    begin
      # 这里发生一个异常

    rescue StandardError => e
      # 异常处理
    end
    ```

* 把较具体的异常放在救援串连的较上层，不然它们永远不会被拯救。

    ```Ruby
    # 差
    begin
      # 一些代码
    rescue Exception => e
      # 一些处理
    rescue StandardError => e
      # 一些处理
    end

    # 好
    begin
      # 一些代码
    rescue StandardError => e
      # 一些处理
    rescue Exception => e
      # 一些处理
    end
    ```
* 在 ensure 区块中释放你的程式的外部资源。

    ```Ruby
    f = File.open('testfile')
    begin
      # .. 处理
    rescue
      # .. 错误处理
    ensure
      f.close unless f.nil?
    end
    ```
* 偏爱使用标准库的异常类胜于导入新的异常类。

## 集合

* 偏好数组及哈希的字面表示法（除非你需要给构造器传入参数）。

    ```Ruby
    # bad
    arr = Array.new
    hash = Hash.new

    # good
    arr = []
    hash = {}
    ```

* Prefer `%w` to the literal array syntax when you need an array of
words(non-empty strings without spaces and special characters in them).
Apply this rule only to arrays with two or more elements.

    ```Ruby
    # 差
    STATES = ['draft', 'open', 'closed']

    # 好
    STATES = %w(draft open closed)
    ```

* Prefer `%i` to the literal array syntax when you need an array of
symbols(and you don't need to maintain Ruby 1.9 compatibility). Apply
this rule only to arrays with two or more elements.

    ```Ruby
    # bad
    STATES = [:draft, :open, :closed]

    # good
    STATES = %i(draft open closed)
    ```

* 避免在数组中创造巨大的间隔。

    ```Ruby
    arr = []
    arr[100] = 1 # 现在你有一个很多 nil 的数组
    ```
* 当处理独一无二的元素时，使用 `Set` 来替代 `Array` 。 `Set` 实现了不重复的无序数值集合。 `Set`是数组直观的内部操作功能与哈希的快速存取的混合体。
* 偏好用符号来取代字符串作为哈希的键。

    ```Ruby
    # 差
    hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

    # 好
    hash = { one: 1, two: 2, three: 3 }
    ```

* 避免使用可变的对象作为键值。
* 当哈希的键为符号时，使用哈希的字面语法。

    ```Ruby
    # 差
    hash = { :one => 1, :two => 2, :three => 3 }

    # 好
    hash = { one: 1, two: 2, three: 3 }
    ```

* 在处理应该存在的哈希键时，使用`fetch` 。

    ```Ruby
    heroes = { 蝙蝠侠: 'Bruce Wayne', 超人: 'Clark Kent' }
    # 差 - 如果我们打错字的话，我们就无法找到对的英雄了
    heroes[:蝙蝠侠] # => "Bruce Wayne"
    heroes[:超女] # => nil

    # 好 - fetch 会抛出一个 KeyError 来使这个问题明显
    heroes.fetch(:超女)
    ```

* Use `fetch` with second argument to set a default value

   ```Ruby
   batman = { name: 'Bruce Wayne', is_evil: false }

   # bad - if we just use || operator with falsy value we won't get the expected result
   batman[:is_evil] || true # => true

   # good - fetch work correctly with falsy values
   batman.fetch(:is_evil, true) # => false
   ```

* 依赖这个事实， Ruby 1.9 的哈希是有序的。
* 在遍历一个集合时，不要改动它。

## 字符串

* 偏好字符串插值（interpolation），而不是字符串连接（concatenation）。

    ```Ruby
    # 差
    email_with_name = user.name + ' <' + user.email + '>'

    # 好
    email_with_name = "#{user.name} <#{user.email}>"
    ```
* 考虑替字符串插值留白。這使插值在字符串里看起來更清楚。

    ```Ruby
    "#{ user.last_name }, #{ user.first_name }"
    ```

* 当你不需要插入特殊符号如 `\t`, `\n`, `'`, 等等时，偏好单引号的字符串。

    ```Ruby
    # 差
    name = "Bozhidar"

    # 好
    name = 'Bozhidar'
    ```
* 别忘了使用 `{}` 来围绕被插入字符串的实例与全局变量。

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      # 差 - 有效，但难看
      def to_s
        "#@first_name #@last_name"
      end

      # 好
      def to_s
        "#{@first_name} #{@last_name}"
      end
    end

    $global = 0
    # 差
    puts "$global = #$global"

    # 好
    puts "$global = #{$global}"
    ```
* 当你需要建构庞大的数据块（chunk）时，避免使用 `String#+` 。
  使用 `String#<<` 来替代。字符串用`<<`连接能就地改变字符串实例，并且永远比 `String#+` 来得快，`String#+` 创造了一堆新的字符串对象。

    ```Ruby
    # 好也比较快
    html = ''
    html << '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```

## 正则表达式

> 有些人在面对问题时，不经大脑便认为，「我知道，这里该用正则表达式」。现在问题反倒变成两个了。<br/>
> -- Jamie Zawinski

* 如果你只需要在字符串中简单的搜索文字，不要使用正则表达式：`string['text']`
* 针对简单的字符串查询，你可以直接在字符串索引中直接使用正则表达式。

    ```Ruby
    match = string[/regexp/] # 获得匹配正则表达式的内容
    first_group = string[/text(grp)/, 1] # 或得分组的内容
    string[/text (grp)/, 1] = 'replace' # string => 'text replace'
    ```
* 当你不需要替结果分组时，使用非分组的群组。

    ```Ruby
    /(first|second)/ # 差
    /(?:first|second)/ # 好
    ```
* 避免使用 `$1-9`，因为它们很难追踪它们包含什么。可以使用命名群组来替代。

    ```Ruby
    # 差
    /(regexp)/ =~ string
    ...
    process $1

    # 好
    /(?<meaningful_var>regexp)/ =~ string
    ...
    process meaningful_var
    ```
* 字符类别只有几个你需要关心的特殊字元：`^`, `-`, `\`, `]`，所以你不用逃脱字元 `.` 或在 `[]` 的中括号。
* 小心使用 `^` 与 `$` ，它们匹配的是一行的开始与结束，不是字符串的开始与结束。如果你想要匹配整个字符串，使用 `\A` 与 `\z`。(译注：`\Z` 实为 `/\n?\z/`，使用 `\z` 才能匹配到有含新行的字符串的结束)

    ```Ruby
    string = "some injection\nusername"
    string[/^username$/] # matches
    string[/\Ausername\z/] # don't match
    ```
* 针对复杂的正则表达式，使用 `x` 修饰符。这让它们的可读性更高并且你可以加入有用的注释。只是要小心忽略的空白。

    ```Ruby
    regexp = %r{
      start # 一些文字
      \s # 空白字元
      (group) # 第一组
      (?:alt1|alt2) # 一些替代方案
      end
    }x
    ```

* 针对复杂的替换，`sub` 或 `gsub` 可以与区块或哈希来使用。

## 百分比字面

* 使用 `%()` 给需要插值与嵌入双引号的单行字符串。多行字符串，偏好使用 heredocs 。

    ```Ruby
    # 差（不需要插值）
    %(<div class="text">Some text</div>)
    # 应该使用'<div class="text">Some text</div>'

    # 差（没有双引号）
    %(This is #{quality} style)
    # 应该使用 "This is #{quality} style"

    # 差（多行）
    %(<div>\n<span class="big">#{exclamation}</span>\n</div>)
    # 应该是一个 heredoc

    # 好（需要插值、有双引号以及单行）
    %(<tr><td class="name">#{name}</td>)
    ```
* 正则表达式要匹配多于一个的 `/` 字元时，使用 `%r`。

    ```Ruby
    # 差
    %r(\s+)

    # 仍然差
    %r(^/(.*)$)
    # 应当是 /^\/(.*)$/

    # 好
    %r(^/blog/2011/(.*)$)
    ```
* 避免 `%q`, `%Q`, `%x`, `%s` 以及 `%W`。
* 偏好 `()` 作为所有 `%` 字面的分隔符。

## 元编程

* 避免无谓的元编程。

* 写一个函式库时不要在核心类别捣乱（不要替它们打 monkey patch。）

* 偏好区块形式的 `class_eval` 胜于字符串插值(string-interpolated)的形式。
  - 当你使用字符串插值形式时，总是提供 `__FILE__` 及 `__LINE__`，使你的 backtrace 看起来有意义：

    ```ruby
    class_eval "def use_relative_model_naming?; true; end", __FILE__, __LINE__
    ```

  - 偏好 `define_method` 胜于 `class_eval{ def ... }`

* 当使用 `class_eval` （或其它的`eval`）搭配字符串插值时，添加一个注解区块，来演示如果做了插值的样子（我从 Rails 代码学来的一个实践）：

    ```ruby
    # 从 activesupport/lib/active_support/core_ext/string/output_safety.rb
    UNSAFE_STRING_METHODS.each do |unsafe_method|
      if 'String'.respond_to?(unsafe_method)
        class_eval <<-EOT, __FILE__, __LINE__ + 1
          def #{unsafe_method}(*args, &block) # def capitalize(*args, &block)
            to_str.#{unsafe_method}(*args, &block) # to_str.capitalize(*args, &block)
          end # end

          def #{unsafe_method}!(*args) # def capitalize!(*args)
            @dirty = true # @dirty = true
            super # super
          end # end
        EOT
      end
    end
    ```
* 元编程避免使用 `method_missing`。会让 Backtraces 变得很凌乱；行为没有列在 `#methods` 里；拼错的方法调用可能默默的工作（`nukes.launch_state = false`)。考虑使用 delegation, proxy, 或是 `define_method` 来取代。如果你必须使用 `method_missing`，
  - 确保[也定义了`respond_to_missing?`](http://blog.marc-andre.ca/2010/11/methodmissing-politely.html)
  - 仅捕捉字首定义良好的方法，像是 `find_by_*` ― 让你的代码愈肯定(assertive)愈好。
  - 在语句的最后调用 `super`
  - delegate 到确定的，非魔术方法中:

    ```ruby
    # 差
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        # ... lots of code to do a find_by
      else
        super
      end
    end

    # 好
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        find_by(prop, *args, &block)
      else
        super
      end
    end

    # 最好的方式，可能是每个可找到的属性被声明后，使用 define_method。
    ```

## 其它

* `ruby -w` 写安全的代码。
* 避免使用哈希作为可选参数。这个方法是不是做太多事了？（对象初始器是本规则的例外）
* 避免方法长于 10 行代码（LOC）。理想上，大部分的方法会小于5行。空行不算进LOC里。
* 避免参数列表长于三或四个参数。
* 如果你真的需要“全局”方法，把它们加到 Kernel 并设为私有的。
* 使用模块变量代替全局变量。

    ```Ruby
    # 差
    $foo_bar = 1

    # 好
    module Foo
      class << self
        attr_accessor :bar
      end
    end

    Foo.bar = 1
    ```
* 当 `alias_method` 可以做到时，避免使用 `alias` 。
* 使用 `OptionParser` 来解析复杂的命令行选项及 `ruby -s` 来处理琐碎的命令行选项。
* 用函数式的方法编程，在有意义的情况下避免赋值(mutation)。
* 不要改变参数，除非那是方法的目的。
* 避免超过三层的区块嵌套。
* 保持一致性。在理想的世界里，遵循这些准则。
* 使用常识。

## 工具

以下是一些工具，让你自动检查 Ruby 代码是否符合本指南。

### RuboCop

[RuboCop](https://github.com/bbatsov/rubocop) is a Ruby code style
checker based on this style guide. RuboCop already covers a
significant portion of the Guide, supports both MRI 1.9 and MRI 2.0
and has good Emacs integration.

### RubyMine

[RubyMine](http://www.jetbrains.com/ruby/)'s code inspections are
[partially based](http://confluence.jetbrains.com/display/RUBYDEV/RubyMine+Inspections)
on this guide.

# 贡献

在本指南所写的每个东西都不是定案。这只是我渴望想与同样对 Ruby 编程风格有兴趣的大家一起工作，以致于最终我们可以替整个 Ruby 社区创造一个有益的资源。

欢迎 open tickets 或 push 一个带有改进的更新请求。在此提前感谢你的帮助！

# 授权

![Creative Commons License](http://i.creativecommons.org/l/by/3.0/88x31.png)
This work is licensed under a [Creative Commons Attribution 3.0 Unported License](http://creativecommons.org/licenses/by/3.0/deed.zh)

# 口耳相传

一份社区驱动的风格指南，如果没多少人知道，对一个社区来说就没有多少用处。微博转发这份指南，分享给你的朋友或同事。我们得到的每个评价、建议或意见都可以让这份指南变得更好一点。而我们想要拥有的是最好的指南，不是吗？

共勉之，<br/>
[Bozhidar](https://twitter.com/bbatsov)
