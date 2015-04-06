# 序幕

> 風格是從偉大事物中萃取出的美好事物。 <br/>
> -- Bozhidar Batsov

作為 Ruby 開發者，有一件總是令我煩心的事 &mdash; Python 開發者有一份好的程式風格參考指南([PEP-8](http://www.python.org/dev/peps/pep-0008/)) 而我們永遠沒有一份官方指南，一份記錄 Ruby 程式風格及最佳實踐的指南。而我們確信風格很重要。我也相信這些好傢伙們，像我們是 Ruby 開發者，應該可以自己產生一份這夢寐以求的文件。

這份指南開始是作為我們公司內部 Ruby 程式指南(由我所寫的)。進行到某個部分時，我決定要把我的成果貢獻給廣大的 Ruby 社群，而且這個世界需要來自另一個公司內部的一點幫助。然而這個世界也可以從由社群制定及驅動的一系列 Ruby 程式慣例、實踐及風格中受益。

在開始寫這份指南時，我收到世界上很多優秀 Ruby 社群用戶們的反饋。感謝所有的建議及幫助！我們同心協力創造一個能夠讓每一個 Ruby 開發者受益的資源。

順道一提，如果你對 Rails 有興趣，你可以看看這份與之互補的 [Ruby on Rails 風格指南](https://github.com/bbatsov/rails-style-guide)。

# Ruby 風格指南

這份 Ruby 風格指南向你推薦現實世界中的最佳實踐，Ruby 程式設計師如何寫出可被別的 Ruby 程式設計師維護的程式碼。一份風格指南反映出現實世界中的用法，並帶有一個理想，避免已經公認是危險的事物被人繼續使用 –– 不管看起來是多麼的好。

本指南依照相關規則分成數個小節。我試著在每個規則後面說明理由（如果省略的話，我相信理由是顯而易見的）。

我沒有想到所有的規則 &mdash; 他們大致上是基於，我作為一個專業軟體工程師的廣泛生涯，從 Ruby 社群成員所得到的反饋及建議，和數個高度評價的 Ruby 程式設計資源，像是 ["Programming Ruby 1.9"](http://pragprog.com/book/ruby4/programming-ruby-1-9-2-0) 以及 ["The Ruby Programming Language"](http://www.amazon.com/Ruby-Programming-Language-David-Flanagan/dp/0596516177)。

本指南仍在進行完善中 –– 某些規則缺乏實例，某些規則沒有例子來清楚地展示它們。在完稿時，將會解決這些議題 &mdash; 現在就先把他們記在心理吧。

你可以使用 [Transmuter](https://github.com/TechnoGate/transmuter) 來產生本指南的一份 PDF 或 HTML 副本。

[RuboCop](https://github.com/bbatsov/rubocop) 專案會自動檢查你的 Ruby 程式碼是否符合這份 Ruby 風格指南。目前這個專案尚有許多功能缺漏，不足以被正式地使用，歡迎有志之士協助改進。

本指南被翻譯成下列語言：

* [简体中文](https://github.com/JuanitoFatas/ruby-style-guide/blob/master/README-zhCN.md)
* [繁體中文](https://github.com/JuanitoFatas/ruby-style-guide/blob/master/README-zhTW.md)
* [法文](https://github.com/porecreat/ruby-style-guide/blob/master/README-frFR.md)
* [德文](https://github.com/arbox/ruby-style-guide/blob/master/README-deDE.md)
* [日文](https://github.com/fortissimo1997/ruby-style-guide/blob/japanese/README.ja.md)
* [韓文](https://github.com/dalzony/ruby-style-guide/blob/master/README-koKR.md)
* [葡萄牙文](https://github.com/rubensmabueno/ruby-style-guide/blob/master/README-PT-BR.md)
* [俄文](https://github.com/arbox/ruby-style-guide/blob/master/README-ruRU.md)
* [西班牙文](https://github.com/alemohamad/ruby-style-guide/blob/master/README-esLA.md)
* [越南文](https://github.com/scrum2b/ruby-style-guide/blob/master/README-viVN.md)

## 目錄

* [原始碼排版](#-2)
* [語法](#-3)
* [命名](#-4)
* [註解](#-5)
    * [註釋](#-6)
* [類別](#-7)
* [異常](#-8)
* [集合](#-9)
* [字串](#-10)
* [正規表示法](#-11)
* [百分比字面](#-12)
* [元程式設計](#-13)
* [其它](#-14)
* [工具](#-15)

## 原始碼排版

> 幾乎每人都深信，每一個除了自己的風格都又醜又難讀。把 "除了自己的" 拿掉，他們或許是對的...<br/>
> -- Jerry Coffin (論縮排)

* 使用 `UTF-8` 作為原始檔案的編碼。
* 每個縮排層級使用兩個**空格**。不要使用 Hard Tabs。

    ```Ruby
    # 不好 - 四個空格
    def some_method
        do_something
    end

    # 好
    def some_method
      do_something
    end
    ```

* 使用 Unix 風格的行編碼 (BSD/Solaris/Linux/OSX 的使用者不用擔心，Windows 使用者要特別小心。)
    * 如果你使用 Git ，你也許會想加入下面這個配置設定，來保護你的專案被 Windows 的行編碼侵入：

      ```bash
      $ git config --global core.autocrlf true
      ```

* 不要使用 `;` 分隔敘述與表達式。 由此推論 - 一行一個表達式。

    ```Ruby
    # 不好
    puts 'foobar'; # 多餘的分號

    puts 'foo'; puts 'bar' # 同一行有兩個表達式

    # 好
    puts 'foobar'

    puts 'foo'
    puts 'bar'

    puts 'foo', 'bar' # 僅對 puts 適用
    ```

* 沒有內容的類別，偏好使用單行定義。

    ```Ruby
    # 不好
    class FooError < StandardError
    end

    # 還可以
    class FooError < StandardError; end

    # 好
    FooError = Class.new(StandardError)
    ```

* 避免單行方法。 雖然這種方式蠻普遍的，但是因為有點怪異的語法，使用起來並不好用。 無論如何 - 單行方法不應該有一個以上的表達式。

    ```Ruby
    # 不好
    def too_much; something; something_else; end

    # 還可以 - 注意第一個 ; 是必要的
    def no_braces_method; body end

    # 還可以 - 注意第二個 ; 是可選的
    def no_braces_method; body; end

    # 還可以 - 語法正確, 但沒有 ; 難以閱讀
    def some_method() body end

    # 好
    def some_method
      body
    end
    ```

    唯一的例外是沒有內容的方法

    ```Ruby
    # 好
    def no_op; end
    ```

* 使用空格來圍繞運算元，逗點 `,` 、冒號 `:` 及分號 `;` 之後，圍繞 `{` 和 `}` 之前。
  空格可能對（大部分）Ruby 直譯器來說是無關緊要的，但正確的使用是寫出可讀性高的程式碼的關鍵。

    ```Ruby
    sum = 1 + 2
    a, b = 1, 2
    [1, 2, 3].each { |e| puts e }
    class FooError < StandardError; end
    ```
    （針對運算子）唯一的例外是當使用指數運算子時：

    ```Ruby
    # 不好
    e = M * c ** 2

    # 好
    e = M * c**2
    ```

    由於 `{` 與 `}` 被用在區塊與雜湊的語法，以及嵌入字串的表達式裡，應該要被更清晰的表達。對於雜湊的語法，可以接受下列兩種撰寫方式。

    ```Ruby
    # 好 - 在 { 之後與 } 之前加上空格
    { one: 1, two: 2 }

    # 好 - 在 { 之後與 } 之前不加空格
    {one: 1, two: 2}
    ```

    第一種寫法稍微容易閱讀（一般來說在Ruby社群裡也較為普及）。第二種寫法的優點是能夠在視覺上區分區塊與雜湊字面語法。不管你選擇哪一種方式 - 持續使用它。

    至於嵌入的表達式，一樣有兩種可行的選擇：

    ```Ruby
    # 好 - 沒有空格
    "string#{expr}"

    # ok - 也許比較好閱讀
    "string#{ expr }"
    ```

    第一種風格非常普遍，原則上建議你持續使用它。第二種風格稍微容易閱讀（可議）。與雜湊一樣 - 選擇其中一種風格並持續使用。

* 不要有空格在 `(` 、 `[` 之後，或 `]` 、 `)` 之前。

    ```Ruby
    some(arg).other
    [1, 2, 3].size
    ```

* 不要有空格在 `!` 之後.

  ```Ruby
  # 不好
  ! something

  # 好
  !something
  ```

* 範圍的字面語法中不要有空格

  ```Ruby
  # 不好
  1 .. 3
  'a' ... 'z'

  # 好
  1..3
  'a'...'z'
  ```

* 把 `when` 跟 `case` 縮排在同一層。我知道很多人不同意這一點，但這是 "The Ruby Programming Language" 及 "Programming Ruby" 所設立的風格。

    ```Ruby
    # 不好
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

    # 好
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
    ```
* 當條件表達式的結果被賦值給一個變數時，保持分支的對齊。

    ```Ruby
    # 不好 - 非常複雜
    kind = case year
    when 1850..1889 then 'Blues'
    when 1890..1909 then 'Ragtime'
    when 1910..1929 then 'New Orleans Jazz'
    when 1930..1939 then 'Swing'
    when 1940..1950 then 'Bebop'
    else 'Jazz'
    end

    result = if some_cond
      calc_something
    else
      calc_something_else
    end

    # 好 - 清楚暸解做什麼事
    kind = case year
           when 1850..1889 then 'Blues'
           when 1890..1909 then 'Ragtime'
           when 1910..1929 then 'New Orleans Jazz'
           when 1930..1939 then 'Swing'
           when 1940..1950 then 'Bebop'
           else 'Jazz'
           end

    result = if some_cond
               calc_something
             else
               calc_something_else
             end

    # 好 (稍微減少行寬)
    kind =
      case year
      when 1850..1889 then 'Blues'
      when 1890..1909 then 'Ragtime'
      when 1910..1929 then 'New Orleans Jazz'
      when 1930..1939 then 'Swing'
      when 1940..1950 then 'Bebop'
      else 'Jazz'
      end

    result =
      if some_cond
        calc_something
      else
        calc_something_else
      end
    ```

* 在 `def` 之間使用空行，並且把方法分成合乎邏輯的段落。

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

* 避免在方法最後一個參數之後加逗號，尤其是參數沒有分佈在不同行的時候。

  ```Ruby
  # 不好 - 比較容易移動/新增/移除參數，但還是不偏好這種做法
  some_method(
               size,
               count,
               color,
             )

  # 不好
  some_method(size, count, color, )

  # 好
  some_method(size, count, color)
  ```

* 當賦予預設值給方法參數時，使用空格圍繞 `=` 運算元：

    ```Ruby
    # 不好
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # do something...
    end

    # 好
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # do something...
    end
    ```

    雖然幾本Ruby的書籍建議第一種風格，不過第二種風格在實務上更為常見（並應該會較容易閱讀）。

* 避免在不需要的時候使用續行 `\`。在實務上，除了字串的連接，避免使用續行在任何地方。

    ```Ruby
    # 不好
    result = 1 - \
             2

    # 好 (但仍然醜到爆)
    result = 1 \
             - 2

    long_string = 'First part of the long string' \
                ' and second part of the long string'
    ```

* 採用一貫的多行方法鏈結風格。在Ruby社群中有兩種普遍的風格，兩種都被認為是好的寫法 - 前置 `.`（選項 A） 與後置 `.` （選項 B）。

  * **（選項 A）** 當另一行繼續調用鏈結式方法時，把 `.` 放在第二行。

    ```Ruby
    # 不好 - 必須要查看第一行才能了解第二行
    one.two.three.
      four

    # 好 - 第二行發生什麼事可以立即地了解
    one.two.three
      .four
    ```

  * **（選項 B）** 當另一行繼續調用鏈結式方法時，將 `.` 放在第一行，暗示下一行還有表達式。

    ```Ruby
    # 不好 - 需要讀到第二行，才知道方法有繼續鏈結
    one.two.three
      .four

    # 好 - 馬上知道還有表達式接續在第一行之後
    one.two.three.
      four
    ```

  關於這兩種可供替換的寫法，可以在[這裡](https://github.com/bbatsov/ruby-style-guide/pull/176)找到討論。

* 如果一個方法呼叫的參數擴展超過一行時，排列它們。當排列參數超過行寬限制，也可以讓第二行只縮排一層。

    ```Ruby
    # 一開始 (一行太長)
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
    end

    # 不好 (兩倍縮排)
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

    # 好 (一般的縮排)
    def send_mail(source)
      Mailer.deliver(
        to: 'bob@example.com',
        from: 'us@example.com',
        subject: 'Important message',
        body: source.text
      )
    end
    ```

* 跨越多行來排列陣列的元素。

  ```Ruby
  # 不好 - 單層縮排
  menu_item = ['Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
    'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam']

  # 好
  menu_item = [
    'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
    'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam'
  ]

  # 好
  menu_item =
    ['Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
     'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam']
  ```

* 在語法中加入底線來改進大數值的可讀性。

    ```Ruby
    # 差勁 - 到底是有幾個零？
    num = 1000000

    # 良好 - 容易被人腦解讀。
    num = 1_000_000
    ```

* 使用 RDoc 以及它的慣例來撰寫 API 文件。不要在註解區塊及 `def` 之前放一個空行。

* 將每一行限制在最多 80 個字元。

* 避免尾隨的空白（trailing whitesapce）。

* 不要使用區塊註解，它們不能夠在開頭置入空白，而且無法像一般的註解一樣容易的被辨識出來。

    ```Ruby
    # 不好
    == begin
    comment line
    another comment line
    == end

    # 好
    # comment line
    # another comment line
    ```

## 語法

* 只使用 `::` 去參照常數（這包括類別與模組）與建構式（像是 `Array()` 或 `Nokogiri::HTML()` ）。不要使用 `::` 作為一般的方法調用。

    ```Ruby
    # 不好
    SomeClass::some_method
    some_object::some_method

    # 好
    SomeClass.some_method
    some_object.some_method
    SomeModule::SomeClass::SOME_CONST
    SomeModule::SomeClass()
    ```

* 使用 `def` 時，當有參數時使用括號。當方法不接受任何參數時，省略括號。

     ```Ruby
     # 不好
     def some_method()
       # 省略主體
     end

     # 好
     def some_method
       # 省略主體
     end

     # 不好
     def some_method_with_parameters param1, param2
       # 省略主體
     end

     # 好
     def some_method_with_parameters(param1, param2)
       # 省略主體
     end
     ```

* 永遠不要使用 `for` ，除非你很清楚為什麼。大部分情況應該使用迭代器來取代。`for` 是由 `each` 所實作的（所以你加入了一層的迂迴），但出乎意料的是 — `for` 並沒有包含一個新的視野 (不像是 `each`）而在這個區塊中定義的變數將會被外部所看到。

    ```Ruby
    arr = [1, 2, 3]

    # 不好
    for elem in arr do
      puts elem
    end

    # 注意，在迴圈外一樣可以存取該元素
    elem # => 3

    # 好
    arr.each { |elem| puts elem }

    # 該元素不能在 each 的區塊之外被存取
    elem # => NameError: undefined local variable or method `elem'
    ```

* 永遠不要在多行的 `if/unless` 使用 `then`

    ```Ruby
    # 不好
    if some_condition then
      # 省略主體
    end

    # 好
    if some_condition
      # 省略主體
    end
    ```

* 在多行的條件判斷區域中，永遠將條件式與 `if`/`unless` 放在同一行。

  ```Ruby
  # 不好
  if
    some_condition
    do_something
    do_something_else
  end

  # 好
  if some_condition
    do_something
    do_something_else
  end
  ```

* 偏愛三元運算元 `? : ` 勝於 `if/then/else/end` 結構
* 它更為常見及更精準。

    ```Ruby
    # 不好
    result = if some_condition then something else something_else end

    # 好
    result = some_condition ? something : something_else
    ```
* 使用一個表達式給一個三元運算元的分支。這也意味著三元運算符不要寫成巢狀式。巢狀情況使用 `if/else` 結構。

    ```Ruby
    # 不好
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

    # 好
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```
* 永遠不要使用 `if x; ...`。使用三元運算元來取代。

    ```Ruby
    # 不好
    result = if some_condition: something else something_else end

    # 好
    result = some_condition ? something : something_else
    ```

* 充分利用 `if` 與 `case` 會回傳結果的特性

  ```Ruby
  # 不好
  if condition
    result = x
  else
    result = y
  end

  # 好
  result =
    if condition
      x
    else
      y
    end
  ```

* 一行的情況使用 `when x then ...` 。替代方案的語法 `when x: ...` 已經在 Ruby 1.9 被移除了。

* 永遠不要使用 `when x; ...` 。參考前一個規則。

* 使用 `!` 而不是 `not`.

    ```Ruby
    # 差 - 因為運算子有優先序，需要用括號。
    x = (not something)

    # 好
    x = !something
    ```

* 避免使用 `!!`。

  ```Ruby
  # 不好
  x = 'test'
  # 令人混淆的 nil 檢查
  if !!x
    # 省略主體
  end

  x = false
  # 雙重否定對布林值來說是不必要的
  !!x # => false

  # 好
  x = 'test'
  unless x.nil?
    # 省略主體
  end
  ```

* 禁止使用 `and` 與 `or`。這麼做不值得。永遠使用 `&&` 與 `||` 取代。

  ```Ruby
  # 不好
  # 布林表達式
  if some_condition and some_other_condition
    do_something
  end

  # 控制流程
  document.saved? or document.save!

  # 好
  # 布林表達式
  if some_condition && some_other_condition
    do_something
  end

  # 控制流程
  document.saved? || document.save!
  ```

* 避免多行的 `? : `（三元運算元）；使用 `if/unless` 來取代。

* 當你有單行的主體時，偏愛 `if/unless` 修飾符。另一個好的方法是使用控制流程的 `and/or` 。

    ```Ruby
    # 不好
    if some_condition
      do_something
    end

    # 好
    do_something if some_condition

    # 另一個好方法
    some_condition && do_something
    ```

* 多行的複雜區塊避免使用 `if/unless` 修飾符。

  ```Ruby
  # 不好
  10.times do
    # 多行主體省略
  end if some_condition

  # 好
  if some_condition
    10.times do
      # 多行主體省略
    end
  end
  ```

* 否定條件偏愛 `unless` 優於 `if` （或是控制流程 `||`）。

    ```Ruby
    # 不好
    do_something if !some_condition

    # 不好
    do_something if not some_condition

    # 好
    do_something unless some_condition

    # 另一個好方法
    some_condition or do_something
    ```
* 永遠不要使用 `unless` 搭配 `else`。 將它們改寫成肯定條件。

    ```Ruby
    # 不好
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
* 不要使用括號圍繞 `if/unless/while` 的條件式，除非這條件包含了一個賦值（見下面使用 `=` （一個賦值）的回傳值）。

    ```Ruby
    # 不好
    if (x > 10)
      # 省略主體
    end

    # 好
    if x > 10
      # 省略主體
    end
    ```

注意這項規則有個例外，[條件式中的安全賦值](#safe-assignment-in-condition)。

* 多行的 `while/until` 不要使用 `while/until condition do`。

  ```Ruby
  # 不好
  while x > 5 do
    # 主體省略
  end

  until x > 5 do
    # 主體省略
  end

  # 好
  while x > 5
    # 主體省略
  end

  until x > 5
    # 主體省略
  end
  ```

* 當你有一個單行的主體時，偏愛使用 `while/until` 修飾子。

    ```Ruby
    # 不好
    while some_condition
      do_something
    end

    # 好
    do_something while some_condition
    ```

* 負面條件偏愛 `until` 勝於 `while`。

    ```Ruby
    # 不好
    do_something while !some_condition

    # 好
    do_something until some_condition
    ```

* 當你需要使用 `Kernel#loop` 代替 `while/until`。

    ```ruby
    # 不好
    while true
      do_something
    end

    until false
      do_something
    end

    # 好
    loop do
      do_something
    end
    ```

* 迴圈後測試使用 `Kernel#loop` 搭配 `break`，而不是 `begin/end/until` 或
  `begin/end/while`。

  ```Ruby
  # 不好
  begin
    puts val
    val += 1
  end while val < 0

  # 好
  loop do
    puts val
    val += 1
    break unless val < 0
  end
  ```

* 忽略圍繞方法參數的括號，如內部 DSL (如：Rake, Rails, RSpec)，Ruby 中帶有 "關鍵字" 狀態的方法（如：`attr_reader`, `puts`）以及屬性存取方法。所有其他的方法呼叫，使用括號圍繞參數。

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

    bowling.score.should == 0
    ```

* 省略圍繞在隱式選項雜湊外圍的花括號。

  ```Ruby
  # 不好
  user.set({ name: 'John', age: 45, permissions: { read: true } })

  # 好
  user.set(name: 'John', age: 45, permissions: { read: true })
  ```

* 方法屬於內部DSL時，同時省略外圍的花括號與圓括號。

  ```Ruby
  class Person < ActiveRecord::Base
    # 不好
    validates(:name, { presence: true, length: { within: 1..10 } })

    # 好
    validates :name, presence: true, length: { within: 1..10 }
  end
  ```

* 方法呼叫不包含參數時，省略圓括號。

  ```Ruby
  # 不好
  Kernel.exit!()
  2.even?()
  fork()
  'test'.upcase()

  # 好
  Kernel.exit!
  2.even?
  fork
  'test'.upcase
  ```

* 單行區塊喜好 `{...}` 勝於 `do..end`。多行區塊避免使用 `{...}`（多行串連總是醜陋）。在 `do...end` 、 "控制流程" 及"方法定義"，永遠使用 `do...end` （如 Rakefile 及某些 DSL）。串連時避免使用 `do...end`。

    ```Ruby
    names = ['Bozhidar', 'Steve', 'Sarah']

    # 不好
    names.each do |name|
      puts name
    end

    # 好
    names.each { |name| puts name }

    # 不好
    names.select do |name|
      name.start_with?('S')
    end.map { |name| name.upcase }

    # 好
    names.select { |name| name.start_with?('S') }.map { |name| name.upcase }
    ```
    某些人會爭論多行串連時，使用`{...}`看起來還可以，但他們應該捫心自問 — 這樣程式碼真的可讀嗎？難道不能把區塊內容取出來放到絕妙的方法裡嗎？

* 考慮使用明確的區塊參數，避免只為了傳遞參數到另一個區塊而使用區塊語法。注意區塊轉換成 Proc 所造成的效能影響。

  ```Ruby
  require 'tempfile'

  # 不好
  def with_tmp_dir
    Dir.mktmpdir do |tmp_dir|
      Dir.chdir(tmp_dir) { |dir| yield dir }  # block just passes arguments
    end
  end

  # 好
  def with_tmp_dir(&block)
    Dir.mktmpdir do |tmp_dir|
      Dir.chdir(tmp_dir, &block)
    end
  end

  with_tmp_dir do |dir|
    puts "dir is accessible as a parameter and pwd is set: #{dir}"
  end
  ```

* 避免在不需要控制流程的場合時使用 `return` 。

    ```Ruby
    # 不好
    def some_method(some_arr)
      return some_arr.size
    end

    # 好
    def some_method(some_arr)
      some_arr.size
    end
    ```

* 避免在不需要的情況使用 `self` 。（只有在呼叫一個 self write 存取器時會需要用到。）

    ```Ruby
    # 不好
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

* 避免使用帶有區域變數的 shadowing 方法，除非它們彼此相等。

    ```Ruby
    class Foo
      attr_accessor :options

      # ok
      def initialize(options)
        self.options = options
        # options 與 self.options 相等
      end

      # 不好
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

* 不要在條件表達式中使用 `=`（賦值）回傳值，除非賦值被包在圓括號中。 這樣的做法在Ruby開發者（Rubyists）中被廣泛稱之為＊條件式中的安全賦值（safe assignment in condition）*。

  ```Ruby
  # 不好 (＋ 會出現警告)
  if v = array.grep(/foo/)
    do_something(v)
    ...
  end

  # 好 (MRI 還是會出現警告, 但 RuboCop 不會)
  if (v = array.grep(/foo/))
    do_something(v)
    ...
  end

  # 好
  v = array.grep(/foo/)
  if v
    do_something(v)
    ...
  end
  ```

* 盡量使用自我賦值的簡寫寫運算元。

  ```Ruby
  # 不好
  x = x + y
  x = x * y
  x = x**y
  x = x / y
  x = x || y
  x = x && y

  # 好
  x += y
  x *= y
  x **= y
  x /= y
  x ||= y
  x &&= y
  ```

* 只有在變數尚未被初始化時，使用 `||=` 初始化變數。

  ```Ruby
  # 不好
  name = name ? name : 'Bozhidar'

  # 不好
  name = 'Bozhidar' unless name

  # 好 - 只有在 name 不是 nil 或 false 時，設定為 Bozhidar
  name ||= 'Bozhidar'
  ```

* 不要使用 `||=` 來初始化布林變數。（想看看如果現在的值剛好是 `false` 時會發生什麼。）

    ```Ruby
    # 不好 — 會把 enabled 設成真，即便它本來是假。
    enabled ||= true

    # 好
    enabled = true if enabled.nil?

* 使用 `&&=` 預先處理變數是否存在。 `&&=` 只會更改已經存在的變數，所以能夠移除使用 `if` 確認變數是否存在。

  ```Ruby
  # 不好
  if something
    something = something.downcase
  end

  # 不好
  something = something ? something.downcase : nil

  # 還可以
  something = something.downcase if something

  # 好
  something = something && something.downcase

  # 更好
  something &&= something.downcase
  ```

* 避免直接使用型態相等運算元（case equality operator） `===`。它的名字暗示它絕對該被用在 `case` 表達式，不在此情形內則會產生一些令人困惑的程式碼。

  ```Ruby
  # 不好
  Array === something
  (1..100) === 7
  /something/ === some_string

  # 好
  something.is_a?(Array)
  (1..100).include?(7)
  some_string =~ /something/
  ```

* 可以使用 `==` 則不要使用 `eql?`。實務中很少需要 `eql?` 所提供的嚴格比較。

  ```Ruby
  # 不好 - 對字串來說 eql? 與 == 作用一樣
  "ruby".eql? some_str

  # 好
  "ruby" == some_str
  1.0.eql? x # 使用 eql? 區別 Fixnum 與 Float 的 1 很合理
  ```

* 避免使用 Perl 風格的特別變數（像是 `$:` 、 `$;` 等等）。它們看起來非常神祕以及不鼓勵使用一行的腳本。使用函式庫 `English` 提供的可閱讀的別名。

  ```Ruby
  # 不好
  $:.unshift File.dirname(__FILE__)

  # 好
  require 'English'
  $LOAD_PATH.unshift File.dirname(__FILE__)
  ```

* 避免在方法名與左括號之間放一個空格。

    ```Ruby
    # 不好
    f (3 + 2) + 1

    # 好
    f(3 + 2) + 1
    ```

* 如果方法的第一個參數由左括號開始，永遠在這個方法呼叫裡使用括號。舉個例子，寫 `f((3+2) + 1)`。

* 總是使用 `-w` 來執行 Ruby 直譯器，如果你忘了某個上述的規則，它就會警告你！

* 對單行區塊主體使用新的 lambda 字面語法，多行區塊使用 `lambda` 方法。

    ```Ruby
    # 不好
    lambda = lambda { |a, b| a + b }
    lambda.call(1, 2)

    # 正確，但使用非常不方便
    l = ->(a, b) do
      tmp = a * 7
      tmp * b / 50
    end

    # 好
    l = ->(a, b) { a + b }
    l.call(1, 2)

    l = lambda do |a, b|
      tmp = a * 7
      tmp * b / 50
    end
    ```

* 偏愛 `proc` 勝過 `Proc.new` 。

  ```Ruby
  # 不好
  p = Proc.new { |n| puts n }

  # 好
  p = proc { |n| puts n }
  ```

* 對 lambdas 與 procs ，偏愛 `proc.call()` 勝過 `proc[]` 或是 `proc.()` 。

  ```Ruby
  # 不好 - 與列舉（Enumeration）的存取相似
  l = ->(v) { puts v }
  l[1]

  # 還是不好 - 不常見的語法
  l = ->(v) { puts v }
  l.(1)

  # 好
  l = ->(v) { puts v }
  l.call(1)
  ```

* 將沒有使用的區塊參數與區域變數加上前綴 `_` ，也可以只使用 `_` （雖然少了一點描述）。 Ruby 直譯器與工具像是 RuboCop 可以解析這個慣例，不會發出尚未使用變數的警告。

  ```Ruby
  # 不好
  result = hash.map { |k, v| v + 1 }

  def something(x)
    unused_var, used_var = something_else(x)
    # ...
  end

  # 好
  result = hash.map { |_k, v| v + 1 }

  def something(x)
    _unused_var, used_var = something_else(x)
    # ...
  end

  # 好
  result = hash.map { |_, v| v + 1 }

  def something(x)
    _, used_var = something_else(x)
    # ...
  end
  ```

* 使用 `$stdout/$stderr/$stdin` 而不是
  `STDOUT/STDERR/STDIN` 。 `STDOUT/STDERR/STDIN` 是常數， 在Ruby中的確可以將常數重新賦值（也許是想重新導向到某個串流）
  ，但直譯器會發出警告。

* 使用 `warn` 而不是 `$stderr.puts`。 除了比較清晰簡潔外， `warn` 允許停用警告（透過 `-W0` 設定警告級別為 0 ）。

* 偏愛使用 `sprintf` 與它的別名 `format`，而不是使用相當晦澀的 `String#%` 方法。

  ```Ruby
  # 不好
  '%d %d' % [20, 10]
  # => '20 10'

  # 好
  sprintf('%d %d', 20, 10)
  # => '20 10'

  # 好
  sprintf('%{first} %{second}', first: 20, second: 10)
  # => '20 10'

  format('%d %d', 20, 10)
  # => '20 10'

  # 好
  format('%{first} %{second}', first: 20, second: 10)
  # => '20 10'
  ```

* 偏愛使用 `Array#join` 而不是相當晦澀帶有字串參數的 `Array#*` 。

    ```Ruby
    # 不好
    %w(one two three) * ', '
    # => 'one, two, three'

    # 好
    %w(one two three).join(', ')
    # => 'one, two, three'
    ```

* 當想要使用作為陣列用途的變數，但並不確定它是否為陣列，利用 `[*var]` 或是 `Array()` 而不是直接進行 `Array` 檢查。

    ```Ruby
    # 不好
    paths = [paths] unless paths.is_a? Array
    paths.each { |path| do_something(path) }

    # 好
    [*paths].each { |path| do_something(path) }

    # 好 （更加可讀）
    Array(paths).each { |path| do_something(path) }
    ```

* 當可以使用範圍或是 `Comparable#between?` 就別使用複雜的邏輯比較。

  ```Ruby
  # 不好
  do_something if x >= 1000 && x <= 2000

  # 好
  do_something if (1000..2000).include?(x)

  # 好
  do_something if x.between?(1000, 2000)
  ```

* 偏愛使用述語方法（predicate methods）與 `==` 做出明確的區別。可以接受數字比較。

  ```Ruby
  # 不好
  if x % 2 == 0
  end

  if x % 2 == 1
  end

  if x == nil
  end

  # 好
  if x.even?
  end

  if x.odd?
  end

  if x.nil?
  end

  if x.zero?
  end

  if x == 0
  end
  ```

* 不使用明確的非- `nil` 檢查，除非是處理布林值。

    ```ruby
    # 不好
    do_something if !something.nil?
    do_something if something != nil

    # 好
    do_something if something

    # 好 - 處理布林值
    def value_set?
      !@some_boolean.nil?
    end
    ```

* 避免使用 `BEGIN` 區塊。

* 不要使用 `END` 區塊。使用 `Kernel#at_exit` 。

  ```ruby
  # 不好
  END { puts 'Goodbye!' }

  # 好
  at_exit { puts 'Goodbye!' }
  ```

* 避免使用範圍運算元（flip-flops）。

* 避免在流程控制中使用巢狀的條件。

  當你能斷定無效的資料時，則偏好使用守衛語句（guard clause）。 守衛語句是在函式最上方的條件句，在必要的時候能夠盡快地脫離方法。

  ```Ruby
  # 不好
  def compute_thing(thing)
    if thing[:foo]
      update_with_bar(thing)
      if thing[:foo][:bar]
        partial_compute(thing)
      else
        re_compute(thing)
      end
    end
  end

  # 好
  def compute_thing(thing)
    return unless thing[:foo]
    update_with_bar(thing[:foo])
    return re_compute(thing) unless thing[:foo][:bar]
    partial_compute(thing)
  end
  ```

  偏愛在迴圈中使用 `next` 而不是條件區塊。

  ```Ruby
  # 不好
  [0, 1, 2, 3].each do |item|
    if item > 1
      puts item
    end
  end

  # 好
  [0, 1, 2, 3].each do |item|
    next unless item > 1
    puts item
  end
  ```

* 偏好 `map` 比 `collect` 好， `find` 比 `detect` 好， `select` 比 `find_all` 好， `reduce` 比 `inject` 好， `size` 比 `length` 好。這並非是硬性的需求;如果使用別名能增強可讀性，那就用吧。 押韻方法（rhyming methods）沿襲自 Smalltalk ，這在其他程式語言中並不常見。鼓勵使用 `select` 大於 `find_all` ，因為能夠很好的與 `reject` 放在一起，而且它的名字能夠解釋自己的作用。

* 不要使用 `count` 作為 `size` 的替代方案。除了 `Enumerable` 物件之外還有 `Array` ，都會遍歷整個集合來取得它的大小。

  ```Ruby
  # 不好
  some_hash.count

  # 好
  some_hash.size
  ```

* 使用 `flat_map` 而不是 `map` + `flatten`。這不適用於深度大於兩層的陣列，舉例來說，如果 `users.first.songs == ['a', ['b','c']]` ，則使用 `map + flatten` 而不是 `flat_map`。`flat_map` 扁平化第一層的陣列，然而 `flatten` 所有情形都會進行扁平化。

  ```Ruby
  # 不好
  all_songs = users.map(&:songs).flatten.uniq

  # 好
  all_songs = users.flat_map(&:songs).uniq
  ```

* 使用 `reverse_each` 而不是 `reverse.each` 。 `reverse_each` 不會製造一個新的陣列作為分配用途，這樣很好。

  ```Ruby
  # 不好
  array.reverse.each { ... }

  # 好
  array.reverse_each { ... }
  ```

## 命名

> 程式設計的真正難題是替事物命名及無效的快取。<br/>
> -- Phil Karlton

* 識別字用英語命名。

    ```Ruby
    # bad - variable name written in Bulgarian with latin characters
    zaplata = 1_000

    # good
    salary = 1_000
    ```

* 符號、方法與變數使用蛇底式小寫（snake_case）。

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

* 類別與模組使用駝峰式大小寫（CamelCase）。（保留像是 HTTP、RFC、XML 這種縮寫為大寫）

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

* 其他常數使用尖叫蛇底式大寫（SCREAMING_SNAKE_CASE）。

    ```Ruby
    # 差
    SomeConst = 5

    # 好
    SOME_CONST = 5
    ```

* 判斷式方法的名字（回傳布林值的方法）應以問號結尾。(即 `Array#empty?` )
* 有潛在“危險性”的方法，若此 *危險* 方法有安全版本存在時，應以安全版本名加上驚嘆號結尾（即：改動 `self` 或參數、 `exit!` 等等方法）。

    ```Ruby
    # 不好 - 沒有對應的安全方法
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

* 如果可能的話，從危險方法（bang）的角度來定義對應的安全方法（non-bang）。

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

* 在短的區塊使用 `reduce` 時，把參數命名為 `|a, e|` (累加器，元素)
* 在定義二元操作符時，把參數命名為 `other` （`<<` 與 `[]` 是這條規則的例外，因為它們的語義不同）。

    ```Ruby
    def +(other)
      # 省略主體
    end
    ```
* 偏好 `map` 勝於 `collect` ， `find` 勝於 `detect` ， `select` 勝於 `find_all` ， `reduce` 勝於 `inject` 以及 `size` 勝於 `length` 。這不是一個硬性要求；如果使用別名增加了可讀性，使用它沒關係。這些有押韻的方法名是從 Smalltalk 繼承而來，在別的語言不常見。鼓勵使用 `select` 而不是 `find_all` 的理由是它跟 `reject` 搭配起來是一目了然的。

* 使用 `flat_map` 勝於使用 `map` + `flatten` 的組合。
  這並不適用於深度大於 2 的陣列，舉個例子，如果 `users.first.songs == ['a', ['b', 'c']]` ，則使用 `map + flatten` 的組合，而不是使用 `flat_map` 。
  `flat_map` 將陣列變平坦一個層級，而 `flatten` 會將整個陣列變平坦。


    ```Ruby
    # bad
    all_songs = users.map(&:songs).flatten.uniq

    # good
    all_songs = users.flat_map(&:songs).uniq
    ```

## 註解

> 良好的程式碼是最佳的文件。當你要加一個註解時，捫心自問，<br/>
> "如何改善程式碼讓它不需要註解？" 改善程式碼然後記錄下來使它更簡潔。 <br/>
> -- Steve McConnell

* 撰寫自我記錄的程式碼並忽略之後的小節。我是認真的！
* 用英文寫註解。
* 在註解的 `#` 與註解文字之間使用一個空格。
* 比一個單字長的註解要大寫及使用標點符號。句號後使用[一個空格](http://en.wikipedia.org/wiki/Sentence_spacing)。
* 避免多餘的註解

    ```Ruby
    # 不好
    counter += 1 # 把計數器加一
    ```
* 保持現有的註解是最新的。過時的註解比沒有註解還差。
> 好的程式碼就像是好的笑話 -- 它不需要解釋<br/>
> -- Russ Olsen
* 避免替爛程式碼寫註解。重構程式碼讓它們看起來一目了然。（要嘛就做，要嘛不做 ― 不要只是試試看。-- Yoda）

### 註釋

* 註釋應該直接寫在相關程式碼那行之後。
* 註釋關鍵字後方伴隨著一個冒號及空白，接著一個描述問題的記錄。
* 如果需要用多行來描述問題，之後的行要放在 `#` 號後面並縮排兩個空白。

    ```Ruby
    def bar
      # FIXME: 這在 v3.2.1 版本之後會異常當掉，或許與
      #   BarBazUtil 的版本更新有關
      baz(:quux)
    end
    ```
* 在問題是顯而易見的情況下，任何的文件會是多餘的，註釋應該要留在可能有問題的那行。這個用法是例外而不是規則。

    ```Ruby
    def bar
      sleep 100 # OPTIMIZE
    end
    ```
* 使用 `TODO` 來標記之後應被加入的未實現功能或特色。
* 使用 `FIXME` 來標記一個需要修復的程式碼。
* 使用 `OPTIMIZE` 來標記可能影響效能的緩慢或效率低落的程式碼。
* 使用 `HACK` 來標記代碼異味，其中包含了可疑的編碼實踐以及應該需要重構。
* 使用 `REVIEW` 來標記任何需要審視及確認正常動作的地方。舉例來說：`REVIEW: 我們確定用戶現在是這麼做的嗎？`
* 如果你覺得適當的話，使用其他你習慣的註釋關鍵字，但記得把它們記錄在專案的 `README` 或類似的地方。

## 類別與模組

* 在類別定義裡使用一致的結構。

    ```Ruby
    class Person
      # 首先是 extend 與 include
      extend SomeModule
      include AnotherModule

      # 接著是常數
      SOME_CONSTANT = 20

      # 接下來是屬性巨集
      attr_reader :name

      # 跟著是其它的巨集（如果有的話）
      validates :name

      # 公開的類別方法接在下一行
      def self.some_method
      end

      # 跟著是公開的實體方法
      def some_method
      end

      # 受保護及私有的方法，一起放在接近結尾的地方
      protected

      def some_protected_method
      end

      private

      def some_private_method
      end
    end
    ```

* 偏好模組，勝過只有類別方法的類。類別應該只在產生實例是合理的時候使用。

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

* 當你想將模組的實例方法變成類別方法時，偏愛使用 `module_function` 勝過 `extend self` 。

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

* 當設計類別階層時，確認它們符合 [Liskov 代換原則](http://en.wikipedia.org/wiki/Liskov_substitution_principle)。
* 盡可能讓你的類別越[堅固](http://en.wikipedia.org/wiki/SOLID_(object-oriented_design\))越好。
* 永遠替類別提供一個適當的 `to_s` 方法來表示領域模型（domain model）。

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

* 使用 `attr` 這類函數來定義瑣碎的 accessor 或 mutators。

    ```Ruby
    # 不好
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

* 考慮使用 `Struct.new`，它替你定義了那些瑣碎的存取器（accessors），建構式（constructor）以及比較運算元（comparison operators）。

    ```Ruby
    # 好
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end

    # 較佳
    Person = Struct.new(:first_name, :last_name) do
    end
    ````
* 考慮加入工廠方法來提供額外合理的方式，來創造一個特定類別的實體。

    ```Ruby
    class Person
      def self.create(options_hash)
        # 省略主體
      end
    end
    ```
* 偏好[鴨子類型](http://en.wikipedia.org/wiki/Duck_typing)勝於繼承。

    ```Ruby
    # 不好
    class Animal
      # 抽象方法
      def speak
      end
    end

    # 繼承高層次的類別 (superclass)
    class Duck < Animal
      def speak
        puts 'Quack! Quack'
      end
    end

    # 繼承高層次的類別 (superclass)
    class Dog < Animal
      def speak
        puts 'Bau! Bau!'
      end
    end

    # 好
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
* 由於繼承中 "討厭的" 行為，避免使用類別變數 (`@@`)。

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

    如同你所看到的，在類別階級中的所有類別其實都共享一個類別變數。應該通常偏好使用實體變數而不是類別變數。

* 依據方法的目的用途指定適當的可視層級 (`private` ,`protected` )。別把所有方法都設為 `public` （方法的預設值）。我們現在是在寫 *Ruby* ，不是 *Python* 。
* 將 `public`, `protected`, `private` 和被應用的方法定義保持一致的縮排。在上下各留一行，來強調在這些方法的特性（公有的、受保護的、私有的）。

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

* 使用 `def self.method` 來定義 singleton 方法。由於類別名稱不重複的關係，這使得代碼更容易重構。

    ```Ruby
    class TestClass
      # 不好
      def TestClass.some_method
        # 省略主體
      end

      # 好
      def self.some_other_method
        # 省略主體
      end

      # 也有可能且當你要定義多個
      # singleton時的便利方法
      class << self
        def first_method
          # 省略主體
        end

        def second_method_etc
          # 省略主體
        end
      end
    end
    ```

## 異常

* 使用 `fail` 方法來捕捉異常。僅在捕捉到異常時使用 `raise` 並重新拋出異常（因為沒有失敗，所以顯式地拋出異常）

    ```Ruby
    begin
     fail 'Oops';
    rescue => error
      raise if error.message != 'Oops'
    end
    ```

* 永遠不要從 `ensure` 區塊返回。如果你顯式地從 `ensure` 區塊中的一個方法返回，那麼這方法會如同沒有異常般的返回。實際上，異常會被默默地丟掉。

    ```Ruby
    def foo
      begin
        fail
      ensure
        return 'very bad idea'
      end
    end
    ```

* 盡可能使用隱式的 `begin` 區塊。

    ```Ruby
    # 不好
    def foo
      begin
        # main logic goes here
      rescue
        # failure handling goes here
      end
    end

    # 好
    def foo
      # main logic goes here
    rescue
      # failure handling goes here
    end
    ```

* 透過 *contingency* 方法 (一個由 Avdi Grimm 創造的詞）來減少 `begin` 區塊的使用。

    ```Ruby
    # 不好
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
    rescue
      # handle IOError
    end

    with_io_error_handling { something_that_might_fail }

    with_io_error_handling { something_else_that_might_fail }
    ```

* 不要封鎖異常。

    ```Ruby
    begin
      # 這裡發生了一個異常
    rescue SomeError
      # 救援子句完全沒有做事
    end

    # bad
    do_something rescue nil
    ```

* 避免在 modifier 形式裡使用 `rescue` 。

    ```Ruby
    # 差勁 - 這捕捉了所有的 StandardError 異常。
    do_something rescue nil
    ```

* 不要為了控制流程而使用異常。

    ```Ruby
    # 不好
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

* 避免救援 `Exception` 類別。這會把信號困住，並呼叫 `exit`，導致你需要 `kill -9` 進程。

    ```Ruby
    # 不好
    begin
      # 呼叫 exit 及殺掉信號會被捕捉（除了 kill -9）
      exit
    rescue Exception
      puts "you didn't really want to exit, right?"
      # 異常處理
    end

    # 好
    begin
      # 從 StandardError 中救援一個救援子句，
      # 不是許多程式設計師所假定的異常。
    rescue => e
      # 異常處理
    end

    # 也很好
    begin
      # 這裡發生一個異常

    rescue StandardError => e
      # 異常處理
    end
    ```

* 把較具體的異常放在救援串連的較上層，不然它們永遠不會被救援。

    ```Ruby
    # 不好
    begin
      # 一些程式碼
    rescue Exception => e
      # 一些處理
    rescue StandardError => e
      # 一些處理
    end

    # 好
    begin
      # 一些程式碼
    rescue StandardError => e
      # 一些處理
    rescue Exception => e
      # 一些處理
    end
    ```
* 在 ensure 區塊中釋放你程式的外部資源。

    ```Ruby
    f = File.open('testfile')
    begin
      # .. 處理
    rescue
      # .. 錯誤處理
    ensure
      f.close unless f.nil?
    end
    ```
* 偏愛使用標準函式庫的異常處理勝於導入新的異常類別。

## 集合

* 偏好陣列及雜湊的字面表示法（除非你需要給建構子傳入參數）。

    ```Ruby
    # 不好
    arr = Array.new
    hash = Hash.new

    # 好
    arr = []
    hash = {}
    ```

* Prefer `%w` to the literal array syntax when you need an array of
words(non-empty strings without spaces and special characters in them).
Apply this rule only to arrays with two or more elements.

    ```Ruby
    # 不好
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

* 避免在陣列中創造巨大的間隔。


    ```Ruby
    arr = []
    arr[100] = 1 # 現在你有一個很多 nil 的陣列
    ```
* 當處理獨一無二的元素時，使用 `Set` 來替代 `Array` 。`Set` 實現了不重複的無序數值集合。`Set` 是陣列直觀的內部操作功能與雜湊的快速存取的混合體。
* 偏好用符號取代字串作為雜湊的鍵。

    ```Ruby
    # 不好
    hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

    # 好
    hash = { one: 1, two: 2, three: 3 }
    ```
* 在處理需要出現的雜湊鍵時，使用 `fetch` 。

    ```Ruby
    heroes = { 蝙蝠俠: 'Bruce Wayne', 超人: 'Clark Kent' }
    # 差勁 - 如果我們打錯字的話，我們就無法找到對的英雄了
    heroes[:蝙蝠俠] # => "Bruce Wayne"
    heroes[:超女] # => nil

    # 棒 - fetch 會拋出一個 KeyError 來體現這個問題
    heroes.fetch(:supermann)
    ```

* 避免使用可變的物件作為鍵值。
* 當你的 hash 鍵為符號時，使用雜湊的字面語法。

    ```Ruby
    # 不好
    hash = { :one => 1, :two => 2, :three => 3 }

    # 好
    hash = { one: 1, two: 2, three: 3 }
    ```

* Use `fetch` when dealing with hash keys that should be present.

    ```Ruby
    heroes = { batman: 'Bruce Wayne', superman: 'Clark Kent' }
    # bad - if we make a mistake we might not spot it right away
    heroes[:batman] # => "Bruce Wayne"
    heroes[:supermann] # => nil

    # good - fetch raises a KeyError making the problem obvious
    heroes.fetch(:supermann)
    ```
* Use `fetch` with second argument to set a default value

   ```Ruby
   batman = { name: 'Bruce Wayne', is_evil: false }

   # bad - if we just use || operator with falsy value we won't get the expected result
   batman[:is_evil] || true # => true

   # good - fetch work correctly with falsy values
   batman.fetch(:is_evil, true) # => false
   ```

* 相信這個事實吧，Ruby 1.9 的雜湊是有序的。
* 在遍歷一個集合時，不要改動它。

## 字串

* 偏好字串插值 (interpolation)，而不是字串串接 (concatenation)。

    ```Ruby
    # 不好
    email_with_name = user.name + ' <' + user.email + '>'

    # 好
    email_with_name = "#{user.name} <#{user.email}>"
    ```

* 考慮替字串插值留白。這使插值在字串裡看起來更清楚。

    ```Ruby
    "#{ user.last_name }, #{ user.first_name }"
    ```

* 當你不需要插入特殊符號如 `\t`, `\n`, `'`, 等等時，偏好單引號的字串。

    ```Ruby
    # 不好
    name = 'Bozhidar'

    # 好
    name = 'Bozhidar'
    ```
* 別忘了使用 `{}` 圍繞要被插入字串的實體與全域變數。

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      # 差 – 有效，但難看
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
* 當你需要建構龐大的資料區段（chunk）時，避免使用 `String#+` 。
  使用 `String#<<` 來替代。字串串接在對的地方改變字串實體，並且永遠比 `String#+` 來得快，`String#+` 創造了一堆新的字串物件。

    ```Ruby
    # 好也比較快
    html = ''
    html << '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```

## 正規表示法

> 有些人在面對問題時，不經大腦便認為，「我知道，這裡該用正規表示法」。現在問題反倒變成兩個了。<br/>
> -- Jamie Zawinski

* 如果你只需要在字串中簡單的搜索文字，不要使用正規表示法：`string['text']`
* 針對簡單的字串查詢，你可以直接在字串索引中直接使用正規表示法。

    ```Ruby
    match = string[/regexp/]             # 獲得匹配正規表示法的內容
    first_group = string[/text(grp)/, 1] # 或得分組的內容
    string[/text (grp)/, 1] = 'replace'  # string => 'text replace'
    ```
* 當你不需要替結果分組時，使用非分組的群組。

    ```Ruby
    /(first|second)/   # 不好
    /(?:first|second)/ # 好
    ```
* 避免使用 `$1-9`，因為它們很難追蹤它們包含什麼。可以使用命名群組來替代。

    ```Ruby
    # 不好
    /(regexp)/ =~ string
    ...
    process $1

    # 好
    /(?<meaningful_var>regexp)/ =~ string
    ...
    process meaningful_var
    ```
* 字元類別只有幾個你需要關心的特殊字元：`^`, `-`, `\`, `]`，所以你不用逃脫字元 `.` 或在 `[]` 的中括號。
* 小心使用 `^` 與 `$` ，它們匹配的是一行的開始與結束，不是字串的開始與結束。如果你想要匹配整個字串，使用 `\A` 與 `\z`。(譯註：`\Z` 實為 `/\n?\z/`，使用 `\z` 才能匹配到有含新行的字串的結束)

    ```Ruby
    string = "some injection\nusername"
    string[/^username$/]   # 匹配
    string[/\Ausername\z/] # 無匹配
    ```
* 針對複雜的正規表示法，使用 `x` 修飾符。這讓它們的可讀性更高並且你可以加入有用的註解。只是要小心忽略的空白。

    ```Ruby
    regexp = %r{
      start         # 一些文字
      \s            # 空白字元
      (group)       # 第一組
      (?:alt1|alt2) # 一些替代方案
      end
    }x
    ```

* 針對複雜的替換，`sub` 或 `gsub` 可以與區塊或雜湊來使用。

## 百分比字面

* 使用 `%()` 給需要插值與嵌入雙引號的單行字串。多行字串，偏好使用 heredocs 。

    ```Ruby
    # 不好（不需要插值）
    %(<div class="text">Some text</div>)
    # 應該使用 '<div class="text">Some text</div>'

    # 不好（沒有雙引號）
    %(This is #{quality} style)
    # 應該使用 "This is #{quality} style"

    # 不好（多行）
    %(<div>\n<span class="big">#{exclamation}</span>\n</div>)
    # 應該是一個 heredoc

    # 好（需要插值、有雙引號以及單行）
    %(<tr><td class="name">#{name}</td>)
    ```
* 正規表示法要匹配多於一個的 `/` 字元時，使用 `%r`。

    ```Ruby
    # 不好
    %r(\s+)

    # 仍不好
    %r(^/(.*)$)
    # 應當是 /^\/(.*)$/

    # 好
    %r(^/blog/2011/(.*)$)
    ```
* 避免 `%q`, `%Q`, `%x`, `%s` 以及 `%W`。
* 偏好 `()` 作為所有 `%` 字面的分隔符。

## 元程式設計

* 避免無謂的元程式設計。

* 寫一個函式庫時不要在核心類別搗亂（不要替它們加 monkey patch。）

* 偏好區塊形式的 `class_eval` 勝於字串插值 (string-interpolated)的形式。
  - 當你使用字串插值形式時，總是提供 `__FILE__` 及 `__LINE__`，使你的 backtrace 看起來有意義：

    ```ruby
    class_eval "def use_relative_model_naming?; true; end", __FILE__, __LINE__
    ```

  - 偏好 `define_method` 勝於 `class_eval{ def ... }`

* 當使用 `class_eval` （或其它的 `eval`）搭配字串插值時，添加一個註解區塊，來顯示如果做了插值的樣子（我從 Rails 程式碼學來的一個實踐）：

    ```ruby
    # 從 activesupport/lib/active_support/core_ext/string/output_safety.rb
    UNSAFE_STRING_METHODS.each do |unsafe_method|
      if 'String'.respond_to?(unsafe_method)
        class_eval <<-EOT, __FILE__, __LINE__ + 1
          def #{unsafe_method}(*args, &block)       # def capitalize(*args, &block)
            to_str.#{unsafe_method}(*args, &block)  #   to_str.capitalize(*args, &block)
          end                                       # end

          def #{unsafe_method}!(*args)              # def capitalize!(*args)
            @dirty = true                           #   @dirty = true
            super                                   #   super
          end                                       # end
        EOT
      end
    end
    ```
* 元程式設計避免使用 `method_missing`。會讓 Backtraces 變得很凌亂；行為沒有列在 `#methods` 裡；拼錯的方法呼叫可能默默的工作（`nukes.launch_state = false`)。考慮使用 delegation, proxy, 或是 `define_method` 來取代。如果你必須使用 `method_missing`，
  - 確保[也定義了 `respond_to_missing?`](http://blog.marc-andre.ca/2010/11/methodmissing-politely.html)
  - 僅捕捉字首定義良好的方法，像是 `find_by_*` ― 讓你的程式碼愈肯定(assertive)愈好。
  - 在最後的敘述句(statement)呼叫 `super`
  - 從 delegate 到 assertive, 不神奇的(non-magical)方法：

    ```ruby
    # 不好
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

    # 而最好是在每個可找到的屬性被宣告時，使用 define_method。
    ```

## 其它

* `ruby -w` 寫出安全的程式碼。
* 避免使用雜湊作為選擇性參數。這個方法是不是做太多事情了？（物件初始器是本規則的例外）
* 避免方法長於 10 行程式碼（LOC）。理想上，大部分的方法會小於5行。空行不算進 LOC 裡。
* 避免參數列表長於三或四個參數。
* 如果你真的需要全域方法，把它們加入到 Kernel 並設為私有的。
* 使用模組變數取代全域變數。

    ```Ruby
    # 不好
    $foo_bar = 1

    # 好
    module Foo
      class << self
        attr_accessor :bar
      end
    end

    Foo.bar = 1
    ```

* 當 `alias_method` 可以做到時，避免使用 `alias` 。
* 使用 `OptionParser` 來解析複雜的命令行選項及 `ruby -s` 來處理瑣碎的命令行選項。
* 用函數式的方法寫程式，在有意義的情況下避免賦值。
* 不要變動參數，除非那是方法的目的。
* 避免超過三行的巢狀區塊。
* 保持一致性。在理想的世界裡，遵循這些準則。
* 使用常識。

## Tools

以下是一些工具，讓你自動檢查 Ruby 程式碼是否符合本指南。

### RuboCop

[RuboCop](https://github.com/bbatsov/rubocop) is a Ruby code style
checker based on this style guide. RuboCop already covers a
significant portion of the Guide, supports both MRI 1.9 and MRI 2.0
and has good Emacs integration.

### RubyMine

[RubyMine](http://www.jetbrains.com/ruby/)'s code inspections are
[partially based](http://confluence.jetbrains.com/display/RUBYDEV/RubyMine+Inspections)
on this guide.

# 貢獻

在本指南所寫的每個東西都不是定案。這只是我渴望想與同樣對 Ruby 程式設計風格有興趣的大家一起工作，以致於最終我們可以替整個 Ruby 社群創造一個有益的資源。

歡迎開票或發送一個帶有改進的更新請求。在此提前感謝你的幫助！

# 授權

![Creative Commons License](http://i.creativecommons.org/l/by/3.0/88x31.png)
This work is licensed under a [Creative Commons Attribution 3.0 Unported License](http://creativecommons.org/licenses/by/3.0/deed.zh_TW)

# 口耳相傳

一份社群策動的風格指南，對一個社群來說，只是讓人知道有這個社群。推特這個指南，分享給你的朋友或同事。我們得到的每個註解、建議或意見都可以讓這份指南變得更好一點。而我們想要擁有的是最好的指南，不是嗎？

共勉之，<br/>
[Bozhidar](https://twitter.com/bbatsov)
