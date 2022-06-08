# 序幕

> 榜樣很重要。<br>
> ——墨菲警官《機器戰警》

作為 Ruby 開發者，有一件總是令我煩心的事 &mdash; Python 開發者有一份好的程式風格參考指南([PEP-8](http://www.python.org/dev/peps/pep-0008/)) 而我們永遠沒有一份官方指南，一份記錄 Ruby 程式風格及最佳實踐的指南。而我們確信風格很重要。我也相信這些好傢伙們，像我們是 Ruby 開發者，應該可以自己產生一份這夢寐以求的文件。

這份指南開始是作為我們公司內部 Ruby 程式指南(由我所寫的)。進行到某個部分時，我決定要把我的成果貢獻給廣大的 Ruby 社群，而且這個世界需要來自另一個公司內部的一點幫助。然而這個世界也可以從由社群制定及驅動的一系列 Ruby 程式慣例、實踐及風格中受益。

在開始寫這份指南時，我收到世界上很多優秀 Ruby 社群用戶們的反饋。感謝所有的建議及幫助！我們同心協力創造一個能夠讓每一個 Ruby 開發者受益的資源。

順道一提，如果你對 Rails 有興趣，你可以看看這份與之互補的 [Ruby on Rails 風格指南](https://github.com/bbatsov/rails-style-guide)。

# Ruby 風格指南

這份 Ruby 風格指南向你推薦現實世界中的最佳實踐，Ruby 程式設計師如何寫出可被別的 Ruby 程式設計師維護的程式碼。一份風格指南反映出現實世界中的用法，並帶有一個理想，避免已經公認是危險的事物被人繼續使用 –– 不管看起來是多麼的好。

本指南依照相關規則分成數個小節。我試著在每個規則後面說明理由（如果省略的話，我相信理由是顯而易見的）。

我沒有想到所有的規則 &mdash; 他們大致上是基於，我作為一個專業軟體工程師的廣泛生涯，從 Ruby 社群成員所得到的反饋及建議，和數個高度評價的 Ruby 程式設計資源，像是 ["Programming Ruby 1.9"](http://pragprog.com/book/ruby4/programming-ruby-1-9-2-0) 以及 ["The Ruby Programming Language"](http://www.amazon.com/Ruby-Programming-Language-David-Flanagan/dp/0596516177)。

本指南仍在進行完善中 –– 某些規則缺乏實例，某些規則沒有例子來清楚地展示它們。在完稿時，將會解決這些議題 &mdash; 現在就先把他們記在心理吧。

你可以使用 [Pandoc](http://pandoc.org/) 來產生本指南的一份 PDF 或 HTML 副本。

[RuboCop](https://github.com/bbatsov/rubocop) 專案會自動檢查你的 Ruby 程式碼是否符合這份 Ruby 風格指南。目前這個專案尚有許多功能缺漏，不足以被正式地使用，歡迎有志之士協助改進。

本指南被翻譯成下列語言：

* [简体中文](https://github.com/JuanitoFatas/ruby-style-guide/blob/master/README-zhCN.md)
* [繁體中文](https://github.com/JuanitoFatas/ruby-style-guide/blob/master/README-zhTW.md)
* [阿拉伯語](https://github.com/HassanTC/ruby-style-guide/blob/master/README-EgAr.md)
* [法文](https://github.com/gauthier-delacroix/ruby-style-guide/blob/master/README-frFR.md)
* [日文](https://github.com/fortissimo1997/ruby-style-guide/blob/japanese/README.ja.md)
* [韓文](https://github.com/dalzony/ruby-style-guide/blob/master/README-koKR.md)
* [葡萄牙文](https://github.com/rubensmabueno/ruby-style-guide/blob/master/README-PT-BR.md)
* [俄文](https://github.com/arbox/ruby-style-guide/blob/master/README-ruRU.md)
* [西班牙文](https://github.com/alemohamad/ruby-style-guide/blob/master/README-esLA.md)
* [越南文](https://github.com/CQBinh/ruby-style-guide/blob/master/README-viVN.md)

## 目錄

* [原始碼排版](#原始碼排版)
* [語法](#語法)
* [命名](#命名)
* [註解](#註解)
  * [註釋](#註釋)
  * [魔法註解](#魔法註解)
* [類別與模組](#類別與模組)
* [異常](#異常)
* [集合](#集合)
* [數字](#數字)
* [字串](#字串)
* [日期與時間](#日期與時間)
* [正規表示法](#正規表示法)
* [百分比字面](#百分比字面)
* [元程式設計](#元程式設計)
* [其它](#其它)
* [工具](#工具)

## 原始碼排版

> 幾乎每人都深信，每一個除了自己的風格都又醜又難讀。把 "除了自己的" 拿掉，他們或許是對的...<br>
> -- Jerry Coffin (論縮排)

* <a name="utf-8"></a>
  使用 `UTF-8` 作為原始檔案的編碼。
<sup>[[link](#utf-8)]</sup>

* <a name="spaces-indentation"></a>
  每個縮排層級使用兩個**空格**。不要使用 Hard Tabs。
<sup>[[link](#spaces-indentation)]</sup>

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

* <a name="crlf"></a>
  使用 Unix 風格的行編碼（BSD/Solaris/Linux/OSX 的使用者不用擔心，Windows 使用者要特別小心。）
<sup>[[link](#crlf)]</sup>

    * 如果你使用 Git，你也許會想加入下面這個設定，避免專案被 Windows 的行編碼入侵：

      ```bash
      $ git config --global core.autocrlf true
      ```

* <a name="no-semicolon"></a>
  不要使用 `;` 分隔敘述與表達式。 由此推論 - 一行一個表達式。
<sup>[[link](#no-semicolon)]</sup>

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

* <a name="single-line-classes"></a>
  沒有內容的類別，偏好使用單行定義。
<sup>[[link](#single-line-classes)]</sup>

    ```Ruby
    # 不好
    class FooError < StandardError
    end

    # 還可以
    class FooError < StandardError; end

    # 好
    FooError = Class.new(StandardError)
    ```

* <a name="no-single-line-methods"></a>
  避免單行方法。 雖然這種方式蠻普遍的，但是因為有點怪異的語法，使用起來並不好用。 無論如何 - 單行方法不應該有一個以上的表達式。
<sup>[[link](#no-single-line-methods)]</sup>

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

* <a name="spaces-operators"></a>
  使用空格來圍繞運算元，逗點 `,` 、冒號 `:` 及分號 `;` 之後，圍繞 `{` 和 `}` 之前。
  空格可能對（大部分）Ruby 直譯器來說是無關緊要的，但正確的使用是寫出可讀性高的程式碼的關鍵。
<sup>[[link](#spaces-operators)]</sup>

    ```Ruby
    sum = 1 + 2
    a, b = 1, 2
    class FooError < StandardError; end
    ```

    有幾個例外狀況，其中一個是使用指數運算子時：

    ```Ruby
    # 不好
    e = M * c ** 2

    # 好
    e = M * c**2
    ```

    另一個例外是在有理數中的分線（slash）：

    ```ruby
    # 不好
    o_scale = 1 / 48r

    # 好
    o_scale = 1/48r
    ```

    最後一個例外狀況則是安全運算子（safe navigation operator）：

    ```ruby
    # 不好
    foo &. bar
    foo &.bar
    foo&. bar

    # 好
    foo&.bar
    ```

* <a name="spaces-braces"></a>
  不要有空格在 `(` 、 `[` 之後，或 `]` 、 `)` 之前。
  在 `{` 、 `}` 的前後加上空格。
<sup>[[link](#spaces-braces)]</sup>

    ```ruby
    # bad
    some( arg ).other
    [ 1, 2, 3 ].each{|e| puts e}

    # good
    some(arg).other
    [1, 2, 3].each { |e| puts e }
    ```

    由於 `{` 與 `}` 被用在區塊與雜湊的語法，以及嵌入字串的表達式裡，應該要被更清晰的表達。

    對於雜湊的語法，可以接受下列兩種撰寫方式。
    第一種寫法稍微容易閱讀（一般來說在 Ruby 社群裡也較為普及）。第二種寫法的優點是能夠在視覺上區分區塊與雜湊字面語法。不管你選擇哪一種方式——持續使用它。

    ```Ruby
    # 好 - 在 { 之後與 } 之前加上空格
    { one: 1, two: 2 }

    # 好 - 在 { 之後與 } 之前不加空格
    {one: 1, two: 2}
    ```

    至於嵌入的表達式則不在大括號中加入空格。

    ```Ruby
    # 不好
    "From: #{ user.first_name }, #{ user.last_name }"

    # 好
    "From: #{user.first_name}, #{user.last_name}"
    ```

* <a name="no-space-bang"></a>
  不要有空格在 `!` 之後.
<sup>[[link](#no-space-bang)]</sup>

  ```Ruby
  # 不好
  ! something

  # 好
  !something
  ```

* <a name="no-space-inside-range-literals"></a>
  範圍的字面語法中不要有空格
<sup>[[link](#no-space-inside-range-literals)]</sup>

  ```Ruby
  # 不好
  1 .. 3
  'a' ... 'z'

  # 好
  1..3
  'a'...'z'
  ```

* <a name="indent-when-to-case"></a>
  把 `when` 跟 `case` 縮排在同一層。我知道很多人不同意這一點，但這是 "The Ruby Programming Language" 及 "Programming Ruby" 所設立的風格。
<sup>[[link](#indent-when-to-case)]</sup>

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
* <a name="indent-conditional-assignment"></a>
  當條件表達式的結果被賦值給一個變數時，保持分支的對齊。
<sup>[[link](#indent-conditional-assignment)]</sup>


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

* <a name="empty-lines-between-methods"></a>
  在 `def` 之間使用空行，並且把方法分成合乎邏輯的段落。
<sup>[[link](#empty-lines-between-methods)]</sup>

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

* <a name="two-or-more-empty-lines"></a>
  不要在同一處中使用多個空行。
<sup>[[link](#two-or-more-empty-lines)]</sup>

  ```Ruby
  # 不好 - 兩個空行
  some_method


  some_method

  # 好
  some_method

  some_method
  ```

* <a name="empty-lines-around-access-modifier"></a>
  使用空行圍繞存取修飾符（Access modifier）。
<sup>[[link](#empty-lines-around-access-modifier)]</sup>

  ```Ruby
  # 不好
  class Foo
    attr_reader :foo
    def foo
      # do something...
    end
  end

  # 好
  class Foo
    attr_reader :foo

    def foo
      # do something...
    end
  end
  ```

* <a name="empty-lines-around-bodies"></a>
  不要用空行圍繞方法、類別、模組、程式碼區塊。
<sup>[[link](#empty-lines-around-bodies)]</sup>

  ```Ruby
  # 不好
  class Foo

    def foo

      begin

        do_something do

          something

        end

      rescue

        something

      end

    end

  end

  # 好
  class Foo
    def foo
      begin
        do_something do
          something
        end
      rescue
        something
      end
    end
  end
  ```

* <a name="no-trailing-params-comma"></a>
  避免在方法最後一個參數之後加逗號，尤其是參數沒有分佈在不同行的時候。
<sup>[[link](#no-trailing-params-comma)]</sup>

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

* <a name="spaces-around-equals"></a>
  當賦予預設值給方法參數時，使用空格圍繞 `=` 運算元：
<sup>[[link](#spaces-around-equals)]</sup>

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

* <a name="no-trailing-backslash"></a>
  避免在不需要的時候使用續行 `\`。在實務上，除了字串的連接，避免使用續行在任何地方。
<sup>[[link](#no-trailing-backslash)]</sup>

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

* <a name="consistent-multi-line-chains"></a>
  採用一貫的多行方法鏈結風格。在Ruby社群中有兩種普遍的風格，兩種都被認為是好的寫法 - 前置 `.`（選項 A） 與後置 `.` （選項 B）。
<sup>[[link](#consistent-multi-line-chains)]</sup>

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

* <a name="no-double-indent"></a>
  如果一個方法呼叫的參數擴展超過一行時，排列它們。當排列參數超過行寬限制，也可以讓第二行只縮排一層。
<sup>[[link](#no-double-indent)]</sup>

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

* <a name="align-multiline-arrays"></a>
  跨越多行來排列陣列的元素。
<sup>[[link](#align-multiline-arrays)]</sup>

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

* <a name="underscores-in-numerics"></a>
  在語法中加入底線來改進大數值的可讀性。
<sup>[[link](#underscores-in-numerics)]</sup>

    ```Ruby
    # 差勁 - 到底是有幾個零？
    num = 1000000

    # 良好 - 容易被人腦解讀。
    num = 1_000_000
    ```

* <a name="numeric-literal-prefixes"></a>
  使用數值前綴時偏好使用小寫字母。
  `0o` 用於八進位數、`0x` 用於十六進位數和 `0b` 用於二進位數。
  不要使用 `0d` 前綴表示十進位數。
<sup>[[link](#numeric-literal-prefixes)]</sup>

  ```ruby
  # 不好
  num = 01234
  num = 0O1234
  num = 0X12AB
  num = 0B10101
  num = 0D1234
  num = 0d1234

  # 好 - 容易從前綴區分出數字
  num = 0o1234
  num = 0x12AB
  num = 0b10101
  num = 1234
  ```

* <a name="rdoc-conventions"></a>
  使用 [RDoc](http://rdoc.sourceforge.net/doc/) 以及它的慣例來撰寫 API 文件。不要在註解區塊及 `def` 之前放一個空行。
<sup>[[link](#rdoc-conventions)]</sup>

* <a name="80-character-limits"></a>
  將每一行限制在最多 80 個字元。
<sup>[[link](#80-character-limits)]</sup>

* <a name="no-trailing-whitespace"></a>
  避免尾隨的空白（trailing whitesapce）。
<sup>[[link](#no-trailing-whitespace)]</sup>

* <a name="newline-eof"></a>
  每個檔案的最後以空行做結尾。
<sup>[[link](#newline-eof)]</sup>

* <a name="no-block-comments"></a>
  不要使用區塊註解，它們不能夠在開頭置入空白，而且無法像一般的註解一樣容易的被辨識出來。
<sup>[[link](#no-block-comments)]</sup>

    ```Ruby
    # 不好
    =begin
    comment line
    another comment line
    =end

    # 好
    # comment line
    # another comment line
    ```

## 語法

* <a name="double-colons"></a>
  只使用 `::` 去參照常數（這包括類別與模組）與建構式（像是 `Array()` 或 `Nokogiri::HTML()` ）。不要使用 `::` 作為一般的方法調用。
<sup>[[link](#double-colons)]</sup>

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

* <a name="colon-method-definition"></a>
  別使用 `::` 來定義類別方法。
<sup>[[link](#colon-method-definition)]</sup>

  ```ruby
  # 不好
  class Foo
    def self::some_method
    end
  end

  # 好
  class Foo
    def self.some_method
    end
  end
  ```

* <a name="method-parens"></a>
  使用 `def` 時，當有參數時使用括號。當方法不接受任何參數時，省略括號。
<sup>[[link](#method-parens)]</sup>

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

* <a name="method-invocation-parens"></a>
  在調用方法時在參數的外圍加上括號，特別是第一個參數是以左括號開頭，如 `(` 在 `f((3 + 2) + 1)`裡。
<sup>[[link](#method-invocation-parens)]</sup>

  ```ruby
  # 不好
  x = Math.sin y
  # 好
  x = Math.sin(y)

  # 不好
  array.delete e
  # 好
  array.delete(e)

  # 不好
  temperance = Person.new 'Temperance', 30
  # 好
  temperance = Person.new('Temperance', 30)
  ```

  在以下的情況下盡量移除括號

  * 不帶參數的呼叫方法：

    ```ruby
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

  * 方法是內部領域特定語言（internal DSL）的一部分（如： Rake, Rails, RSpec）：

    ```ruby
    # 不好
    validates(:name, presence: true)
    # 好
    validates :name, presence: true
    ```

  * 在 Ruby 中有 Methods 狀態的方法：

    ```ruby
    class Person
      # 不好
      attr_reader(:name, :age)
      # 好
      attr_reader :name, :age

      # body omitted
    end
    ```

  在以下的情況下可以移除括號

  * Methods that have "keyword" status in Ruby, but are not declarative:

    ```Ruby
    # 好
    puts(temperance.age)
    system('ls')
    # 也很好
    puts temperance.age
    system 'ls'
    ```

* <a name="optional-arguments"></a>
    將可選參數定義於參數排列的末端。
    Ruby 在呼叫可選參數放在前面的方法時會出現一些無法預期的結果。
<sup>[[link](#optional-arguments)]</sup>

  ```Ruby
  # 不好
  def some_method(a = 1, b = 2, c, d)
    puts "#{a}, #{b}, #{c}, #{d}"
  end

  some_method('w', 'x') # => '1, 2, w, x'
  some_method('w', 'x', 'y') # => 'w, 2, x, y'
  some_method('w', 'x', 'y', 'z') # => 'w, x, y, z'

  # 好
  def some_method(c, d, a = 1, b = 2)
    puts "#{a}, #{b}, #{c}, #{d}"
  end

  some_method('w', 'x') # => '1, 2, w, x'
  some_method('w', 'x', 'y') # => 'y, 2, w, x'
  some_method('w', 'x', 'y', 'z') # => 'y, z, w, x'
  ```

* <a name="parallel-assignment"></a>
    在定義變數時避免使用平行賦值（parallel assignment）。
    平行賦值僅能用於呼叫方法時的回傳值、與 * 星號運算符（splat operator）共同使用、或者是用於交換變數賦值。
    平行賦值的可讀性是低於分開賦值的。
<sup>[[link](#parallel-assignment)]</sup>

  ```Ruby
  # 不好
  a, b, c, d = 'foo', 'bar', 'baz', 'foobar'

  # 好
  a = 'foo'
  b = 'bar'
  c = 'baz'
  d = 'foobar'

  # 好 - 交換變數賦值
  # 交換變數賦值是一個特殊的例子，因為他允許你交換各個已經賦予給變數的值。
  a = 'foo'
  b = 'bar'

  a, b = b, a
  puts a # => 'bar'
  puts b # => 'foo'

  # 好 - 方法回傳
  def multi_return
    [1, 2]
  end

  first, second = multi_return

  # 好 - 與 * 運算符一起使用
  first, *list = [1, 2, 3, 4] # first => 1, list => [2, 3, 4]

  hello_array = *'Hello' # => ["Hello"]

  a = *(1..3) # => [1, 2, 3]
  ```

* <a name="trailing-underscore-variables"></a>
  避免在平行賦值時使用不必要的尾隨底線變數，比起單純使用底線變數，為底線變數命名更好，能夠提供更多的資訊。
  如果在左側的賦值有定義 * 星號變數（splat variable），且星號變數非底線變數時，尾隨底線變數是必要的。
<sup>[[link]](#trailing-underscore-variables)</sup>

  ```Ruby
  # 不好
  foo = 'one,two,three,four,five'
  # 不必要的賦值並沒有提供有用的資訊
  first, second, _ = foo.split(',')
  first, _, _ = foo.split(',')
  first, *_ = foo.split(',')


  # 好
  foo = 'one,two,three,four,five'
  # 必要的底線在此明確指出你需要除了最後一個元素以外的所有元素
  *beginning, _ = foo.split(',')
  *beginning, something, _ = foo.split(',')

  a, = foo.split(',')
  a, b, = foo.split(',')
  # 不必要的賦值給予未使用的變數，但賦值提供了我們有用的資訊
  first, _second = foo.split(',')
  first, _second, = foo.split(',')
  first, *_ending = foo.split(',')
  ```

* <a name="no-for-loops"></a>
  永遠不要使用 `for` ，除非你很清楚為什麼。大部分情況應該使用迭代器來取代。`for` 是由 `each` 所實作的（所以你加入了一層的迂迴），但出乎意料的是 — `for` 並沒有包含一個新的視野 (不像是 `each`）而在這個區塊中定義的變數將會被外部所看到。
<sup>[[link](#no-for-loops)]</sup>

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

* <a name="no-then"></a>
  永遠不要在多行的 `if/unless` 使用 `then`
<sup>[[link](#no-then)]</sup>

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

* <a name="same-line-condition"></a>
  在多行的條件判斷區域中，永遠將條件式與 `if`/`unless` 放在同一行。
<sup>[[link](#same-line-condition)]</sup>

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

* <a name="ternary-operator"></a>
  偏愛三元運算元 `? : ` 勝於 `if/then/else/end` 結構。它更為常見及更精準。
<sup>[[link](#ternary-operator)]</sup>


    ```Ruby
    # 不好
    result = if some_condition then something else something_else end

    # 好
    result = some_condition ? something : something_else
    ```
* <a name="no-nested-ternary"></a>
  使用一個表達式給一個三元運算元的分支。這也意味著三元運算符不要寫成巢狀式。巢狀情況使用 `if/else` 結構。
<sup>[[link](#no-nested-ternary)]</sup>

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
* <a name="no-semicolon-ifs"></a>
  永遠不要使用 `if x; ...`。使用三元運算元來取代。
<sup>[[link](#no-semicolon-ifs)]</sup>

    ```Ruby
    # 不好
    result = if some_condition; something else something_else end

    # 好
    result = some_condition ? something : something_else
    ```

* <a name="use-if-case-returns"></a>
  充分利用 `if` 與 `case` 會回傳結果的特性
<sup>[[link](#use-if-case-returns)]</sup>

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

* <a name="one-line-cases"></a>
  一行的情況使用 `when x then ...` 。替代方案的語法 `when x: ...` 已經在 Ruby 1.9 被移除了。
<sup>[[link](#one-line-cases)]</sup>

* <a name="no-when-semicolons"></a>
  永遠不要使用 `when x; ...` 。參考前一個規則。
<sup>[[link](#no-when-semicolons)]</sup>

* <a name="bang-not-not"></a>
  使用 `!` 而不是 `not`.
<sup>[[link](#bang-not-not)]</sup>

    ```Ruby
    # 差 - 因為運算子有優先序，需要用括號。
    x = (not something)

    # 好
    x = !something
    ```

* <a name="no-bang-bang"></a>
  避免使用 `!!`。
<sup>[[link](#no-bang-bang)]</sup>

  `!!` 會將數值轉換為布林值，但在流程控制上你不需要使用它，他只會混淆你的意圖。
  如果你需要檢查 `nil` 值，請使用 `nil?` 檢查。

  ```Ruby
  # 不好
  x = 'test'
  # 令人混淆的 nil 檢查
  if !!x
    # 省略主體
  end

  # 好
  x = 'test'
  if x
    # 省略主體
  end
  ```

* <a name="no-and-or-or"></a>
  禁止使用 `and` 與 `or`。這麼做不值得。永遠使用 `&&` 與 `||` 取代。
<sup>[[link](#no-and-or-or)]</sup>

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

* <a name="no-multiline-ternary"></a>
  避免多行的 `? : `（三元運算元）；使用 `if/unless` 來取代。
<sup>[[link](#no-multiline-ternary)]</sup>

* <a name="if-as-a-modifier"></a>
  當你有單行的主體時，偏愛 `if/unless` 修飾符。另一個好的方法是使用控制流程的 `and/or` 。
<sup>[[link](#if-as-a-modifier)]</sup>

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

* <a name="no-multiline-if-modifiers"></a>
  多行的複雜區塊避免使用 `if/unless` 修飾符。
<sup>[[link](#no-multiline-if-modifiers)]</sup>

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

* <a name="no-nested-modifiers"></a>
  避免巢狀使用 `if`/`unless`/`while`/`until` 修飾符，如果 `&&`/`||` 合適的話是更好的選擇。
<sup>[[link](#no-nested-modifiers)]</sup>

  ```Ruby
  # 不好
  do_something if other_condition if some_condition

  # 好
  do_something if some_condition && other_condition
  ```

* <a name="unless-for-negatives"></a>
  否定條件偏愛 `unless` 優於 `if` （或是控制流程 `||`）。
<sup>[[link](#unless-for-negatives)]</sup>

    ```Ruby
    # 不好
    do_something if !some_condition

    # 不好
    do_something if not some_condition

    # 好
    do_something unless some_condition

    # 另一個好方法
    some_condition || do_something
    ```
* <a name="no-else-with-unless"></a>
  永遠不要使用 `unless` 搭配 `else`。 將它們改寫成肯定條件。
<sup>[[link](#no-else-with-unless)]</sup>

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
* <a name="no-parens-if"></a>
  不要使用括號圍繞 `if/unless/while` 的條件式，除非這條件包含了一個賦值（見下面使用 `=` （一個賦值）的回傳值）。
<sup>[[link](#no-parens-if)]</sup>

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

* <a name="no-multiline-while-do"></a>
  多行的 `while/until` 不要使用 `while/until condition do`。
<sup>[[link](#no-multiline-while-do)]</sup>

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

* <a name="while-as-a-modifier"></a>
  當你有一個單行的主體時，偏愛使用 `while/until` 修飾子。
<sup>[[link](#while-as-a-modifier)]</sup>

    ```Ruby
    # 不好
    while some_condition
      do_something
    end

    # 好
    do_something while some_condition
    ```

* <a name="until-for-negatives"></a>
  負面條件偏愛 `until` 勝於 `while`。
<sup>[[link](#until-for-negatives)]</sup>

    ```Ruby
    # 不好
    do_something while !some_condition

    # 好
    do_something until some_condition
    ```

* <a name="infinite-loop"></a>
  當你需要使用 `Kernel#loop` 代替 `while/until`。
<sup>[[link](#infinite-loop)]</sup>

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

* <a name="loop-with-break"></a>
  迴圈後測試使用 `Kernel#loop` 搭配 `break`，而不是 `begin/end/until` 或
  `begin/end/while`。
<sup>[[link](#loop-with-break)]</sup>

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

* <a name="no-braces-opts-hash"></a>
  省略圍繞在隱式選項雜湊外圍的花括號。
<sup>[[link](#no-braces-opts-hash)]</sup>

  ```Ruby
  # 不好
  user.set({ name: 'John', age: 45, permissions: { read: true } })

  # 好
  user.set(name: 'John', age: 45, permissions: { read: true })
  ```

* <a name="no-dsl-decorating"></a>
  方法屬於內部DSL時，同時省略外圍的花括號與圓括號。
<sup>[[link](#no-dsl-decorating)]</sup>

  ```Ruby
  class Person < ActiveRecord::Base
    # 不好
    validates(:name, { presence: true, length: { within: 1..10 } })

    # 好
    validates :name, presence: true, length: { within: 1..10 }
  end
  ```

* <a name="single-action-blocks"></a>
  當調用函數為區塊中的唯一操作時，使用 proc 簡單表示。
<sup>[[link](#single-action-blocks)]</sup>

  ```Ruby
  # 不好
  names.map { |name| name.upcase }

  # 好
  names.map(&:upcase)
  ```

* <a name="single-line-blocks"></a>
  單行區塊喜好 `{...}` 勝於 `do..end`。多行區塊避免使用 `{...}`（多行串連總是醜陋）。在 `do...end` 、 "控制流程" 及"方法定義"，永遠使用 `do...end` （如 Rakefile 及某些 DSL）。串連時避免使用 `do...end`。
<sup>[[link](#single-line-blocks)]</sup>

    ```Ruby
    names = %w[Bozhidar Steve Sarah]

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

* <a name="block-argument"></a>
  考慮使用明確的區塊參數，避免只為了傳遞參數到另一個區塊而使用區塊語法。注意區塊轉換成 Proc 所造成的效能影響。
<sup>[[link](#block-argument)]</sup>

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

* <a name="no-explicit-return"></a>
  避免在不需要控制流程的場合時使用 `return` 。
<sup>[[link](#no-explicit-return)]</sup>

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

* <a name="no-self-unless-required"></a>
  避免在不需要的情況使用 `self` 。（只有在呼叫一個 self write 存取器時會需要用到。）
<sup>[[link](#no-self-unless-required)]</sup>

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

* <a name="no-shadowing"></a>
  避免使用帶有區域變數的 shadowing 方法，除非它們彼此相等。
<sup>[[link](#no-shadowing)]</sup>

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

* <a name="safe-assignment-in-condition"></a>
  不要在條件表達式中使用 `=`（賦值）回傳值，除非賦值被包在圓括號中。 這樣的做法在Ruby開發者（Rubyists）中被廣泛稱之為*條件式中的安全賦值（safe assignment in condition）*。
<sup>[[link](#safe-assignment-in-condition)]</sup>

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

* <a name="self-assignment"></a>
  盡量使用自我賦值的簡寫寫運算元。
<sup>[[link](#self-assignment)]</sup>

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

* <a name="double-pipe-for-uninit"></a>
  只有在變數尚未被初始化時，使用 `||=` 初始化變數。
<sup>[[link](#double-pipe-for-uninit)]</sup>

  ```Ruby
  # 不好
  name = name ? name : 'Bozhidar'

  # 不好
  name = 'Bozhidar' unless name

  # 好 - 只有在 name 是 nil 或 false 時，設定為 Bozhidar
  name ||= 'Bozhidar'
  ```

* <a name="no-double-pipes-for-bools"></a>
  不要使用 `||=` 來初始化布林變數。（想看看如果現在的值剛好是 `false` 時會發生什麼。）
<sup>[[link](#no-double-pipes-for-bools)]</sup>

    ```Ruby
    # 不好 — 會把 enabled 設成真，即便它本來是假。
    enabled ||= true

    # 好
    enabled = true if enabled.nil?
    ```

* <a name="double-amper-preprocess"></a>
  使用 `&&=` 預先處理變數是否存在。 `&&=` 只會更改已經存在的變數，所以能夠移除使用 `if` 確認變數是否存在。
<sup>[[link](#double-amper-preprocess)]</sup>

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

* <a name="no-case-equality"></a>
  避免直接使用型態相等運算元（case equality operator） `===`。它的名字暗示它絕對該被用在 `case` 表達式，不在此情形內則會產生一些令人困惑的程式碼。
<sup>[[link](#no-case-equality)]</sup>

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

* <a name="eql"></a>
  可以使用 `==` 則不要使用 `eql?`。實務中很少需要 `eql?` 所提供的嚴格比較。
<sup>[[link](#eql)]</sup>

  ```Ruby
  # 不好 - 對字串來說 eql? 與 == 作用一樣
  "ruby".eql? some_str

  # 好
  "ruby" == some_str
  1.0.eql? x # 使用 eql? 區別 Fixnum 與 Float 的 1 很合理
  ```

* <a name="no-cryptic-perlisms"></a>
  避免使用 Perl 風格的特別變數（像是 `$:` 、 `$;` 等等）。它們看起來非常神祕以及不鼓勵使用一行的腳本。使用函式庫 `English` 提供的可閱讀的別名。
<sup>[[link](#no-cryptic-perlisms)]</sup>

  ```Ruby
  # 不好
  $:.unshift File.dirname(__FILE__)

  # 好
  require 'English'
  $LOAD_PATH.unshift File.dirname(__FILE__)
  ```

* <a name="parens-no-spaces"></a>
  避免在方法名與左括號之間放一個空格。
<sup>[[link](#parens-no-spaces)]</sup>

    ```Ruby
    # 不好
    f (3 + 2) + 1

    # 好
    f(3 + 2) + 1
    ```

* <a name="always-warn-at-runtime"></a>
  總是使用 `-w` 來執行 Ruby 直譯器，如果你忘了某個上述的規則，它就會警告你！
<sup>[[link](#always-warn-at-runtime)]</sup>

* <a name="no-nested-methods"></a>
  不要使用巢狀定義方法，使用 lambda 取代。
  巢狀方法定義會在同一個範圍（例如 class）內實作出與外面相同的方法，而且， "巢狀方法" 在每一次呼叫包含定義的方法時都會重新定義。
<sup>[[link](#no-nested-methods)]</sup>

  ```ruby
  # 不好
  def foo(x)
    def bar(y)
      # 主體省略
    end

    bar(x)
  end

  # 好 - 與上一個例子相同，但是每次呼叫 foo 並不會重新定義 bar
  def bar(y)
    # 主體省略
  end

  def foo(x)
    bar(x)
  end

  # 也很好
  def foo(x)
    bar = ->(y) { ... }
    bar.call(x)
  end
  ```

* <a name="lambda-multi-line"></a>
  對單行區塊主體使用新的 lambda 字面語法，多行區塊使用 `lambda` 方法。
<sup>[[link](#lambda-multi-line)]</sup>

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

* <a name="stabby-lambda-with-args"></a>
  在定義 stabby lambda （使用 -> 箭頭符號）時別省略參數外的括號。
<sup>[[link](#stabby-lambda-with-args)]</sup>

  ```Ruby
  # 不好
  l = ->x, y { something(x, y) }

  # 好
  l = ->(x, y) { something(x, y) }
  ```

* <a name="stabby-lambda-no-args"></a>
  在定義 stabby lambda （使用 -> 箭頭符號）時，如果沒有參數的話，省略括號。
<sup>[[link](#stabby-lambda-no-args)]</sup>

  ```Ruby
  # 不好
  l = ->() { something }

  # 好
  l = -> { something }
  ```

* <a name="proc"></a>
  偏愛 `proc` 勝過 `Proc.new` 。
<sup>[[link](#proc)]</sup>

  ```Ruby
  # 不好
  p = Proc.new { |n| puts n }

  # 好
  p = proc { |n| puts n }
  ```

* <a name="proc-call"></a>
  對 lambdas 與 procs ，偏愛 `proc.call()` 勝過 `proc[]` 或是 `proc.()` 。
<sup>[[link](#proc-call)]</sup>

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

* <a name="underscore-unused-vars"></a>
  將沒有使用的區塊參數與區域變數加上前綴 `_` ，也可以只使用 `_` （雖然少了一點描述）。 Ruby 直譯器與工具像是 RuboCop 可以解析這個慣例，不會發出尚未使用變數的警告。
<sup>[[link](#underscore-unused-vars)]</sup>

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

* <a name="global-stdout"></a>
  使用 `$stdout/$stderr/$stdin` 而不是
  `STDOUT/STDERR/STDIN` 。 `STDOUT/STDERR/STDIN` 是常數， 在Ruby中的確可以將常數重新賦值（也許是想重新導向到某個串流）
  ，但直譯器會發出警告。
<sup>[[link](#global-stdout)]</sup>

* <a name="warn"></a>
  使用 `warn` 而不是 `$stderr.puts`。 除了比較清晰簡潔外， `warn` 允許停用警告（透過 `-W0` 設定警告級別為 0 ）。
<sup>[[link](#warn)]</sup>

* <a name="sprintf"></a>
  偏愛使用 `sprintf` 與它的別名 `format`，而不是使用相當晦澀的 `String#%` 方法。
<sup>[[link](#sprintf)]</sup>

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

* <a name="named-format-tokens"></a>
  使用插值於格式化字串時，偏好使用 `%<name>s` 勝於 `%{name}`，因為他帶有此數值類型的資訊。
<sup>[[link]](#named-format-tokens)</sup>

  ```ruby
  # 不好
  format('Hello, %{name}', name: 'John')

  # 好
  format('Hello, %<name>s', name: 'John')
  ```

* <a name="array-join"></a>
  偏愛使用 `Array#join` 而不是相當晦澀帶有字串參數的 `Array#*` 。
<sup>[[link](#array-join)]</sup>

    ```Ruby
    # 不好
    %w(one two three) * ', '
    # => 'one, two, three'

    # 好
    %w(one two three).join(', ')
    # => 'one, two, three'
    ```

* <a name="splat-arrays"></a>
  當想要使用作為陣列用途的變數，但並不確定它是否為陣列，利用 `[*var]` 或是 `Array()` 而不是直接進行 `Array` 檢查。
<sup>[[link](#splat-arrays)]</sup>

    ```Ruby
    # 不好
    paths = [paths] unless paths.is_a? Array
    paths.each { |path| do_something(path) }

    # 好
    [*paths].each { |path| do_something(path) }

    # 好 （更加可讀）
    Array(paths).each { |path| do_something(path) }
    ```

* <a name="ranges-or-between"></a>
  當可以使用範圍或是 `Comparable#between?` 就別使用複雜的邏輯比較。
<sup>[[link](#ranges-or-between)]</sup>

  ```Ruby
  # 不好
  do_something if x >= 1000 && x <= 2000

  # 好
  do_something if (1000..2000).include?(x)

  # 好
  do_something if x.between?(1000, 2000)
  ```

* <a name="predicate-methods"></a>
  偏愛使用述語方法（predicate methods）與 `==` 做出明確的區別。可以接受數字比較。
<sup>[[link](#predicate-methods)]</sup>

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

* <a name="no-non-nil-checks"></a>
  不使用明確的非- `nil` 檢查，除非是處理布林值。
<sup>[[link](#no-non-nil-checks)]</sup>

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

* <a name="no-BEGIN-blocks"></a>
  避免使用 `BEGIN` 區塊。
<sup>[[link](#no-BEGIN-blocks)]</sup>

* <a name="no-END-blocks"></a>
  不要使用 `END` 區塊。使用 `Kernel#at_exit` 。
<sup>[[link](#no-END-blocks)]</sup>

  ```ruby
  # 不好
  END { puts 'Goodbye!' }

  # 好
  at_exit { puts 'Goodbye!' }
  ```

* <a name="no-flip-flops"></a>
  避免使用範圍運算元（flip-flops）。
<sup>[[link](#no-flip-flops)]</sup>

* <a name="no-nested-conditionals"></a>
  避免在流程控制中使用巢狀的條件。
<sup>[[link](#no-nested-conditionals)]</sup>

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

* <a name="map-find-select-reduce-size"></a>
  偏好 `map` 比 `collect` 好， `find` 比 `detect` 好， `select` 比 `find_all` 好， `reduce` 比 `inject` 好， `size` 比 `length` 好。這並非是硬性的需求;如果使用別名能增強可讀性，那就用吧。 押韻方法（rhyming methods）沿襲自 Smalltalk ，這在其他程式語言中並不常見。鼓勵使用 `select` 大於 `find_all` ，因為能夠很好的與 `reject` 放在一起，而且它的名字能夠解釋自己的作用。
<sup>[[link](#map-find-select-reduce-size)]</sup>

* <a name="count-vs-size"></a>
  不要使用 `count` 作為 `size` 的替代方案。除了 `Enumerable` 物件之外還有 `Array` ，都會遍歷整個集合來取得它的大小。
<sup>[[link](#count-vs-size)]</sup>

  ```Ruby
  # 不好
  some_hash.count

  # 好
  some_hash.size
  ```

* <a name="flat-map"></a>
  使用 `flat_map` 而不是 `map` + `flatten`。這不適用於深度大於兩層的陣列，舉例來說，如果 `users.first.songs == ['a', ['b','c']]` ，則使用 `map + flatten` 而不是 `flat_map`。`flat_map` 扁平化第一層的陣列，然而 `flatten` 所有情形都會進行扁平化。
<sup>[[link](#flat-map)]</sup>

  ```Ruby
  # 不好
  all_songs = users.map(&:songs).flatten.uniq

  # 好
  all_songs = users.flat_map(&:songs).uniq
  ```

* <a name="reverse-each"></a>
  使用 `reverse_each` 而不是 `reverse.each` 。 `reverse_each` 不會製造一個新的陣列作為分配用途，這樣很好。
<sup>[[link](#reverse-each)]</sup>

  ```Ruby
  # 不好
  array.reverse.each { ... }

  # 好
  array.reverse_each { ... }
  ```

## 命名

> 程式設計的真正難題是替事物命名及無效的快取。<br>
> -- Phil Karlton

* <a name="english-identifiers"></a>
  識別字用英語命名。
<sup>[[link](#english-identifiers)]</sup>

  ```Ruby
  # 不好 - 識別字使用非 ASCII 字母
  заплата = 1_000

  # 不好 - 識別字使用保加利亞語英語拼音（相較前例使用斯拉夫拼音）
  zaplata = 1_000

  # 好
  salary = 1_000
  ```

* <a name="snake-case-symbols-methods-vars"></a>
  符號、方法與變數使用蛇底式小寫（snake_case）。
<sup>[[link](#snake-case-symbols-methods-vars)]</sup>

  ```Ruby
  # 不好
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

  some_var = 5
  var10    = 10

  def some_method
    ...
  end
  ```

* <a name="snake-case-symbols-methods-vars-with-numbers"></a>
  在命名符號、方法或變數時，不要分開字母跟數字。
<sup>[[link](#snake-case-symbols-methods-vars-with-numbers)]</sup>

  ```Ruby
  # 不好
  :some_sym_1

  some_var_1 = 1

  def some_method_1
    # 一些程式碼
  end

  # 好
  :some_sym1

  some_var1 = 1

  def some_method1
    # 一些程式碼
  end
  ```

* <a name="camelcase-classes"></a>
  類別與模組使用駝峰式大小寫（CamelCase）。（保留像是 HTTP、RFC、XML 這種縮寫為大寫）
<sup>[[link](#camelcase-classes)]</sup>

  ```Ruby
  # 不好
  class Someclass
    # 一些程式碼
  end

  class Some_Class
    # 一些程式碼
  end

  class SomeXml
    # 一些程式碼
  end

  class XmlSomething
    # 一些程式碼
  end

  # 好
  class SomeClass
    # 一些程式碼
  end

  class SomeXML
    # 一些程式碼
  end

  class XMLSomething
    # 一些程式碼
  end
  ```

* <a name="snake-case-files"></a>
  檔案命名使用蛇底式小寫（snake_case），例如： `hello_world.rb` 。
<sup>[[link](#snake-case-files)]</sup>

* <a name="snake-case-dirs"></a>
  資料夾命名亦使用蛇底式小寫（snake_case），例如： `lib/hello_world/hello_world.rb` 。
<sup>[[link](#snake-case-dirs)]</sup>

* <a name="one-class-per-file"></a>
  盡量讓每一個檔案內只有單一類別或模組。使用該類別或模組的名稱來替該檔案命名，但將名稱從駝峰式大小寫（CamelCase）置換成蛇底式小寫（snake_case）。
<sup>[[link](#one-class-per-file)]</sup>

* <a name="screaming-snake-case"></a>
  其他常數使用尖叫蛇底式大寫（SCREAMING_SNAKE_CASE）。
<sup>[[link](#screaming-snake-case)]</sup>

  ```Ruby
  # 不好
  SomeConst = 5

  # 好
  SOME_CONST = 5
  ```

* <a name="bool-methods-qmark"></a>
  判斷式方法的名字（回傳布林值的方法）應以問號結尾。(即 `Array#empty?` )。非回傳布林值的方法，不應以問號結尾。
<sup>[[link](#bool-methods-qmark)]</sup>

* <a name="bool-methods-prefix"></a>
  避免在使用動詞命名的判斷式加上前綴如 `is`、`does` 或是 `can`，這些前綴與 Ruby 核心函式庫內的判斷式如 `empty?` 和 `include?` 相較之下顯得多餘且邏輯不一致。
<sup>[[link](#bool-methods-prefix)]</sup>

  ```ruby
  # 不好
  class Person
    def is_tall?
      true
    end

    def can_play_basketball?
      false
    end

    def does_like_candy?
      true
    end
  end

  # 好
  class Person
    def tall?
      true
    end

    def basketball_player?
      false
    end

    def likes_candy?
      true
    end
  end
  ```

* <a name="dangerous-method-bang"></a>
  有潛在“危險性”的方法，若此 *危險* 方法有安全版本存在時，應以安全版本名加上驚嘆號結尾（即：改動 `self` 或參數、 `exit!` 等等方法）。
<sup>[[link](#dangerous-method-bang)]</sup>

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

* <a name="safe-because-unsafe"></a>
  如果可能的話，從危險方法（bang）的角度來定義對應的安全方法（non-bang）。
<sup>[[link](#safe-because-unsafe)]</sup>

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

* <a name="other-arg"></a>
  在定義二元操作符時，把參數命名為 `other` （`<<` 與 `[]` 是這條規則的例外，因為它們的語義不同）。
<sup>[[link](#other-arg)]</sup>

  ```Ruby
  def +(other)
    # 省略主體
  end
  ```

## 註解

> 良好的程式碼是最佳的文件。當你要加一個註解時，捫心自問，<br>
> "如何改善程式碼讓它不需要註解？" 改善程式碼然後記錄下來使它更簡潔。 <br>
> -- Steve McConnell

* <a name="no-comments"></a>
  撰寫自我記錄的程式碼並忽略之後的小節。我是認真的！
<sup>[[link](#no-comments)]</sup>

* <a name="english-comments"></a>
  用英文寫註解。
<sup>[[link](#english-comments)]</sup>

* <a name="hash-space"></a>
  在註解的 `#` 與註解文字之間使用一個空格。
<sup>[[link](#hash-space)]</sup>

* <a name="english-syntax"></a>
  比一個單字長的註解要大寫及使用標點符號。句號後使用[一個空格](http://en.wikipedia.org/wiki/Sentence_spacing)。
<sup>[[link](#english-syntax)]</sup>

* <a name="no-superfluous-comments"></a>
  避免多餘的註解
<sup>[[link](#no-superfluous-comments)]</sup>

    ```Ruby
    # 不好
    counter += 1 # 把計數器加一
    ```
* <a name="comment-upkeep"></a>
  保持現有的註解是最新的。過時的註解比沒有註解還差。
<sup>[[link](#comment-upkeep)]</sup>

> 好的程式碼就像是好的笑話 -- 它不需要解釋<br>
> -- Russ Olsen

* <a name="refactor-dont-comment"></a>
  避免替爛程式碼寫註解。重構程式碼讓它們看起來一目了然。（要嘛就做，要嘛不做 ― 不要只是試試看。-- Yoda）
<sup>[[link](#refactor-dont-comment)]</sup>

### 註釋

* <a name="annotate-above"></a>
  註釋應該直接寫在相關程式碼那行之後。
<sup>[[link](#annotate-above)]</sup>

* <a name="annotate-keywords"></a>
  註釋關鍵字後方伴隨著一個冒號及空白，接著一個描述問題的記錄。
<sup>[[link](#annotate-keywords)]</sup>

* <a name="indent-annotations"></a>
  如果需要用多行來描述問題，之後的行要放在 `#` 號後面並縮排兩個空白。
<sup>[[link](#indent-annotations)]</sup>

    ```Ruby
    def bar
      # FIXME: 這在 v3.2.1 版本之後會異常當掉，或許與
      #   BarBazUtil 的版本更新有關
      baz(:quux)
    end
    ```
* <a name="rare-eol-annotations"></a>
  在問題是顯而易見的情況下，任何的文件會是多餘的，註釋應該要留在可能有問題的那行。這個用法是例外而不是規則。
<sup>[[link](#rare-eol-annotations)]</sup>

    ```Ruby
    def bar
      sleep 100 # OPTIMIZE
    end
    ```
* <a name="todo"></a>
  使用 `TODO` 來標記之後應被加入的未實現功能或特色。
<sup>[[link](#todo)]</sup>

* <a name="fixme"></a>
  使用 `FIXME` 來標記一個需要修復的程式碼。
<sup>[[link](#fixme)]</sup>

* <a name="optimize"></a>
  使用 `OPTIMIZE` 來標記可能影響效能的緩慢或效率低落的程式碼。
<sup>[[link](#optimize)]</sup>

* <a name="hack"></a>
  使用 `HACK` 來標記代碼異味，其中包含了可疑的編碼實踐以及應該需要重構。
<sup>[[link](#hack)]</sup>

* <a name="review"></a>
  使用 `REVIEW` 來標記任何需要審視及確認正常動作的地方。舉例來說：`REVIEW: 我們確定用戶現在是這麼做的嗎？`
<sup>[[link](#review)]</sup>

* <a name="document-annotations"></a>
  如果你覺得適當的話，使用其他你習慣的註釋關鍵字，但記得把它們記錄在專案的 `README` 或類似的地方。
<sup>[[link](#document-annotations)]</sup>

### 魔法註解

* <a name="magic-comments-first"></a>
  在代碼或是文件的最上方加入魔法註解（除了 shebangs ，我們等等會討論到他）。
<sup>[[link](#magic-comments-first)]</sup>

  ```Ruby
  # 好
  # frozen_string_literal: true

  # 有關 Person 的一些文件
  class Person
  end

  # 不好
  # 有關 Person 的一些文件

  # frozen_string_literal: true
  class Person
  end
  ```

* <a name="below-shebang"></a>
  當魔法註解與 shebangs 同時出現的話，將前者至於後者之後。
<sup>[[link](#below-shebang)]</sup>

  ```Ruby
  # 好
  #!/usr/bin/env ruby
  # frozen_string_literal: true

  App.parse(ARGV)

  # 不好
  # frozen_string_literal: true
  #!/usr/bin/env ruby

  App.parse(ARGV)
  ```

* <a name="one-magic-comment-per-line"></a>
  你如果需要多個的話，使用一行一個魔法註解。
<sup>[[link](#one-magic-comment-per-line)]</sup>

  ```Ruby
  # 好
  # frozen_string_literal: true
  # encoding: ascii-8bit

  # 不好
  # -*- frozen_string_literal: true; encoding: ascii-8bit -*-
  ```

* <a name="separate-magic-comments-from-code"></a>
  使用一個空行分隔魔法註解與程式碼或是文件。
<sup>[[link](#separate-magic-comments-from-code)]</sup>

  ```Ruby
  # 好
  # frozen_string_literal: true

  # 有關 Person 的一些文件
  class Person
    # Some code
  end

  # 不好
  # frozen_string_literal: true
  # 有關 Person 的一些文件
  class Person
    # Some code
  end
  ```

## 類別與模組

* <a name="consistent-classes"></a>
  在類別定義裡使用一致的結構。
<sup>[[link](#consistent-classes)]</sup>

    ```Ruby
    class Person
      # 首先是 extend 與 include
      extend SomeModule
      include AnotherModule

      # 內部類別
      CustomError = Class.new(StandardError)

      # 接著是常數
      SOME_CONSTANT = 20

      # 接下來是屬性巨集
      attr_reader :name

      # 跟著是其它的巨集（如果有的話）
      validates :name

      # 公開的類別方法接在下一行
      def self.some_method
      end

      # 初始化方法置於類別方法與其他實例方法之中
      def initialize
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

* <a name="mixin-grouping"></a>
  引入多個 mixin 時分開聲明。
<sup>[[link](#mixin-grouping)]</sup>

  ```ruby
  # 不好
  class Person
    include Foo, Bar
  end

  # 好
  class Person
    # 把多個 mixin 分開聲明
    include Foo
    include Bar
  end
  ```

* <a name="file-classes"></a>
  別在同一個類別中巢狀多個類別，試著在各別的類別檔案中分開包裝類別。
<sup>[[link](#file-classes)]</sup>

  ```Ruby
  # 不好

  # foo.rb
  class Foo
    class Bar
      # 裡面有 30 個方法
    end

    class Car
      # 裡面有 20 個方法
    end

    # 裡面有 30 個方法
  end

  # 好

  # foo.rb
  class Foo
    # 裡面有 30 個方法
  end

  # foo/bar.rb
  class Foo
    class Bar
      # 裡面有 30 個方法
    end
  end

  # foo/car.rb
  class Foo
    class Car
      # 裡面有 20 個方法
    end
  end
  ```

* <a name="namespace-definition"></a>
  使用明確的巢狀定義（和重啟）命名類別和模組，因為 Ruby 的[詞法作用域（lexical scoping）](https://cirw.in/blog/constant-lookup.html)，使用範圍解析運算符（::）時，根據巢狀模組定義的方式，可能導致意料之外的常數查找。
  <sup>[[link](#namespace-definition)]</sup>

  ```Ruby
  module Utilities
    class Queue
    end
  end

  # 不好
  class Utilities::Store
    Module.nesting # => [Utilities::Store]

    def initialize
      # 參考到最高層級 ::Queue 類別，因為 Utilities 並不在當前的巢狀鏈中
      @queue = Queue.new
    end
  end

  # 好
  module Utilities
    class WaitingList
      Module.nesting # => [Utilities::WaitingList, Utilities]

      def initialize
        @queue = Queue.new # 參考 Utilities::Queue
      end
    end
  end
  ```

* <a name="modules-vs-classes"></a>
  偏好模組，勝過只有類別方法的類。類別應該只在產生實例是合理的時候使用。
<sup>[[link](#modules-vs-classes)]</sup>

    ```Ruby
    # 差
    class SomeClass
      def self.some_method
        # 主體省略
      end

      def self.some_other_method
      end
    end

    # 好
    module SomeClass
      module_function

      def some_method
        # 主體省略
      end

      def some_other_method
      end
    end
    ```

* <a name="module-function"></a>
  當你想將模組的實例方法變成類別方法時，偏愛使用 `module_function` 勝過 `extend self` 。
<sup>[[link](#module-function)]</sup>

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

* <a name="liskov"></a>
  當設計類別階層時，確認它們符合 [Liskov 代換原則](http://en.wikipedia.org/wiki/Liskov_substitution_principle)。
<sup>[[link](#liskov)]</sup>

* <a name="solid-design"></a>
  盡可能讓你的類別越[堅固](http://en.wikipedia.org/wiki/SOLID_(object-oriented_design))越好。
<sup>[[link](#solid-design)]</sup>

* <a name="define-to-s"></a>
  永遠替類別提供一個適當的 `to_s` 方法來表示領域模型（domain model）。
<sup>[[link](#define-to-s)]</sup>

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def to_s
        "#{@first_name} #{@last_name}"
      end
    end
    ```

* <a name="attr_family"></a>
  使用 `attr` 這類函數來定義瑣碎的 accessor 或 mutators。
<sup>[[link](#attr_family)]</sup>

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

* <a name="accessor_mutator_method_names"></a>
  使用存取器（accessors）和修改器（mutators）時，避免在方法名稱前面加上 `get_` 與 `set_` 的前綴。在 Ruby 中，慣例上使用屬性名稱當作存取器（讀）和使用 `attr_name=` 當修改器（寫）。
<sup>[[link](#accessor_mutator_method_names)]</sup>

  ```ruby
  # 不好
  class Person
    def get_name
      "#{@first_name} #{@last_name}"
    end

    def set_name(name)
      @first_name, @last_name = name.split(' ')
    end
  end

  # 好
  class Person
    def name
      "#{@first_name} #{@last_name}"
    end

    def name=(name)
      @first_name, @last_name = name.split(' ')
    end
  end
  ```

* <a name="attr"></a>
  避免使用 `attr`。 使用 `attr_reader` 和 `attr_accessor` 。
<sup>[[link](#attr)]</sup>

  ```Ruby
  # 不好 - 產生一個屬性存取器 (Ruby 1.9 時已移除)
  attr :something, true
  attr :one, :two, :three # 與 attr_reader 作用相同

  # 好
  attr_accessor :something
  attr_reader :one, :two, :three
  ```

* <a name="struct-new"></a>
  考慮使用 `Struct.new`，它替你定義了那些瑣碎的存取器（accessors），建構式（constructor）以及比較運算元（comparison operators）。
<sup>[[link](#struct-new)]</sup>

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
    ```

* <a name="no-extend-struct-new"></a>
  別使用 `Struct.new` 擴展一個實例初始化，如果檔案引入太多次，在帶入多餘的類別來擴展可能也會同時來入了奇怪的錯誤。
<sup>[[link](#no-extend-struct-new)]</sup>

  ```Ruby
  # 不好
  class Person < Struct.new(:first_name, :last_name)
  end

  # 好
  Person = Struct.new(:first_name, :last_name)
  ```

* <a name="factory-methods"></a>
  考慮加入工廠方法來提供額外合理的方式，來創造一個特定類別的實體。
<sup>[[link](#factory-methods)]</sup>

  ```Ruby
  class Person
    def self.create(options_hash)
      # 主體省略
    end
  end
  ```

* <a name="duck-typing"></a>
  偏好[鴨子類型](http://en.wikipedia.org/wiki/Duck_typing)勝於繼承。
<sup>[[link](#duck-typing)]</sup>

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
* <a name="no-class-vars"></a>
  由於繼承中 "討厭的" 行為，避免使用類別變數 (`@@`)。
<sup>[[link](#no-class-vars)]</sup>

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

* <a name="visibility"></a>
  依據方法的目的用途指定適當的可視層級 (`private` ,`protected` )。別把所有方法都設為 `public` （方法的預設值）。我們現在是在寫 *Ruby* ，不是 *Python* 。
<sup>[[link](#visibility)]</sup>

* <a name="indent-public-private-protected"></a>
  將 `public`, `protected`, `private` 和被應用的方法定義保持一致的縮排。在上下各留一行，來強調在這些方法的特性（公有的、受保護的、私有的）。
<sup>[[link](#indent-public-private-protected)]</sup>

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

* <a name="def-self-class-methods"></a>
  使用 `def self.method` 來定義 singleton 方法。由於類別名稱不重複的關係，這使得代碼更容易重構。
<sup>[[link](#def-self-class-methods)]</sup>

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

* <a name="alias-method-lexically"></a>
  當要幫在同一個類別作用域（lexical class scope）的方法使用別名時，
  比起使用 `self` 的解法，偏好使用 `alias` ，
  它更加清楚的告訴使用者，除非明確聲明，否則你的別名不會被任何子類別修改，運行時亦同。
<sup>[[link](#alias-method-lexically)]</sup>

  ```ruby
  class Westerner
    def first_name
      @names.first
    end

    alias given_name first_name
  end
  ```

  由於 `alias` 如同 `def`，是個關鍵字，所以偏好無型態符號的參數（bareword arguments）勝過
  符號或是字串，換個說法，使用 `alias foo bar` 而不是 `alias :foo :bar`。

  同時注意 Ruby 如何處理別名和繼承：當別名被定義時，別名的參照已經決定了，而非動態發出。

  ```ruby
  class Fugitive < Westerner
    def first_name
      'Nobody'
    end
  end
  ```

  在這個例子裡 `Fugitive#given_name` 仍會呼叫 `Westerner#first_name` 方法，
  而非 `Fugitive#first_name`. 想要複寫 `Fugitive#given_name` 的行為，你必須重新定義初始的類別。

  ```ruby
  class Fugitive < Westerner
    def first_name
      'Nobody'
    end

    alias given_name first_name
  end
  ```

* <a name="alias-method"></a>
  當別名使用在模組、類別或是運行中單例類別的方法時，使用 `alias_method` ，
  在這些情境下，使用作用域中的 `alias` 將會使運行無法預期。
<sup>[[link](#alias-method)]</sup>

  ```Ruby
  module Mononymous
    def self.included(other)
      other.class_eval { alias_method :full_name, :given_name }
    end
  end

  class Sting < Westerner
    include Mononymous
  end
  ```

* <a name="class-and-self"></a>
  當一個類別（或是模組）方法呼叫其他類似的方法時，隱藏開頭的 `self` 或是隱藏在 `.` 之後自己的名字。
  這在服務類別（service classes）或是其他把類別當作函式使用的概念中常見，這樣的習慣會減少這種類別出現重複樣板。
  <sup>[[link](#class-and-self)]</sup>

  ```Ruby
  class TestClass
    # 不好 -- 類別重新命名或是方法變更時需要花更多功夫
    def self.call(param1, param2)
      TestClass.new(param1).call(param2)
    end

    # 不好 -- 過於詳細
    def self.call(param1, param2)
      self.new(param1).call(param2)
    end

    # 好
    def self.call(param1, param2)
      new(param1).call(param2)
    end

    # 其他方法...
  end
  ```

## 異常

* <a name="prefer-raise-over-fail"></a>
  對於異常建議使用 `raise` 勝過 `fail`。
  <sup>[[link](#prefer-raise-over-fail)]</sup>

  ```Ruby
  # bad
  fail SomeException, 'message'

  # good
  raise SomeException, 'message'
  ```

* <a name="no-explicit-runtimeerror"></a>
  當使用兩個參數的 `raise` 時，別明確指出 `RuntimeError`。
<sup>[[link](#no-explicit-runtimeerror)]</sup>

  ```ruby
  # 不好
  raise RuntimeError, 'message'

  # 好 - 單個參數預設為 RuntimeError
  raise 'message'
  ```

* <a name="exception-class-messages"></a>
  偏好提供給 `raise` 異常類別和訊息共兩個分開的參數，勝於直接給異常實例。
<sup>[[link](#exception-class-messages)]</sup>

  ```Ruby
  # 不好
  raise SomeException.new('message')
  # 這邊是沒有辦法使用 `raise SomeException.new('message'), backtrace` 的.

  # 好
  raise SomeException, 'message'
  # 與 `raise SomeException, 'message', backtrace` 一致.
  ```

* <a name="no-return-ensure"></a>
  永遠不要從 `ensure` 區塊返回。如果你顯式地從 `ensure` 區塊中的一個方法返回，那麼這方法會如同沒有異常般的返回。實際上，異常會被默默地丟掉。
<sup>[[link](#no-return-ensure)]</sup>

    ```Ruby
    def foo
      begin
        fail
      ensure
        return 'very bad idea'
      end
    end
    ```

* <a name="begin-implicit"></a>
  盡可能使用隱式的 `begin` 區塊。
<sup>[[link](#begin-implicit)]</sup>

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

* <a name="contingency-methods"></a>
  透過 *contingency* 方法 (一個由 Avdi Grimm 創造的詞）來減少 `begin` 區塊的使用。
<sup>[[link](#contingency-methods)]</sup>

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

* <a name="dont-hide-exceptions"></a>
  不要封鎖異常。
<sup>[[link](#dont-hide-exceptions)]</sup>

    ```Ruby
    begin
      # 這裡發生了一個異常
    rescue SomeError
      # 救援子句完全沒有做事
    end

    # bad
    do_something rescue nil
    ```

* <a name="no-rescue-modifiers"></a>
  避免以修飾符的形式使用 `rescue` 。
<sup>[[link](#no-rescue-modifiers)]</sup>

    ```Ruby
    # 不好 - 這會捕捉 StandardError 類別和其所有子孫類別
    read_file rescue handle_error($!)

    # 好 - 這只會捕捉 Errno::ENOENT 類別和其所有子孫類別
    def foo
      read_file
    rescue Errno::ENOENT => ex
      handle_error(ex)
    end
    ```

* <a name="no-exceptional-flows"></a>
  不要為了控制流程而使用異常。
<sup>[[link](#no-exceptional-flows)]</sup>

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

* <a name="no-blind-rescues"></a>
  避免救援 `Exception` 類別。這會困住信號與對 `exit` 的呼叫，導致你需要 `kill -9` 進程。
<sup>[[link](#no-blind-rescues)]</sup>

    ```Ruby
    # 不好
    begin
      # 呼叫 exit 及 kill 信號會被捕捉（除了 kill -9）
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

* <a name="exception-ordering"></a>
  把較具體的異常放在救援串連的較上層，不然它們永遠不會被救援。
<sup>[[link](#exception-ordering)]</sup>

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
* <a name="release-resources"></a>
  在 ensure 區塊中釋放你程式的外部資源。
<sup>[[link](#release-resources)]</sup>

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

* <a name="auto-release-resources"></a>
盡量使用可自動清理資源的取得方法。
<sup>[[link](#auto-release-resources)]</sup>

  ```ruby
  # 不好 - 你需要明確關閉檔案描述符（file descriptor）
  f = File.open('testfile')
  # 檔案裡的一些動作
  f.close

  # 好 - 檔案描述符會自行關閉
  File.open('testfile') do |f|
    # 檔案裡的一些動作
  end
  ```

* <a name="standard-exceptions"></a>
  偏愛使用標準函式庫的異常處理勝於導入新的異常類別。
<sup>[[link](#standard-exceptions)]</sup>

## 集合

* <a name="literal-array-hash"></a>
  偏好陣列及雜湊的字面表示法（除非你需要給建構子傳入參數）。
<sup>[[link](#literal-array-hash)]</sup>

    ```Ruby
    # 不好
    arr = Array.new
    hash = Hash.new

    # 好
    arr = []
    arr = Array.new(10)
    hash = {}
    hash = Hash.new(0)
    ```

* <a name="percent-w"></a>
  當陣列由兩個以上的字串組成，而且字串不包含空格及特別字元時，偏好使用 `%w` 創建陣列。
<sup>[[link](#percent-w)]</sup>

    ```Ruby
    # 不好
    STATES = ['draft', 'open', 'closed']

    # 好
    STATES = %w(draft open closed)
    ```

* <a name="percent-i"></a>
  當陣列由兩個以上的 symbol 組成，而且你不需要維護 Ruby 1.9 兼容性時，偏好使用 `%i` 創建陣列。
<sup>[[link](#percent-i)]</sup>

    ```Ruby
    # bad
    STATES = [:draft, :open, :closed]

    # good
    STATES = %i(draft open closed)
    ```

* <a name="no-trailing-array-commas"></a>
  避免在 `Array` 或 `Hash` 的最後一個項目後使用逗號，特別是他們不是分行表示時。
<sup>[[link](#no-trailing-array-commas)]</sup>

  ```ruby
  # 不好 - 易於移動、增加、移除項目，但仍不是好選擇
  VALUES = [
             1001,
             2020,
             3333,
           ]

  # 不好
  VALUES = [1001, 2020, 3333, ]

  # 好
  VALUES = [1001, 2020, 3333]
  ```

* <a name="no-gappy-arrays"></a>
  避免在陣列中創造巨大的間隔。
<sup>[[link](#no-gappy-arrays)]</sup>

    ```Ruby
    arr = []
    arr[100] = 1 # 現在你有一個很多 nil 的陣列
    ```

* <a name="first-and-last"></a>
  當需要取得陣列第一個或是最後一個元素時，偏好
  使用 `first` 或 `last` 勝過 `[0]` 或 `[-1]`。
<sup>[[link](#first-and-last)]</sup>

* <a name="set-vs-array"></a>
  當處理獨一無二的元素時，使用 `Set` 來替代 `Array` 。`Set` 實現了不重複的無序數值集合。`Set` 是陣列直觀的內部操作功能與雜湊的快速存取的混合體。
<sup>[[link](#set-vs-array)]</sup>

* <a name="symbols-as-keys"></a>
  偏好用符號取代字串作為雜湊的鍵。
<sup>[[link](#symbols-as-keys)]</sup>

    ```Ruby
    # 不好
    hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

    # 好
    hash = { one: 1, two: 2, three: 3 }
    ```

* <a name="no-mutable-keys"></a>
  避免使用可變的物件作為鍵值。
<sup>[[link](#no-mutable-keys)]</sup>

* <a name="hash-literals"></a>
  當你的 hash 鍵為符號時，使用雜湊的字面語法。
<sup>[[link](#hash-literals)]</sup>

    ```Ruby
    # 不好
    hash = { :one => 1, :two => 2, :three => 3 }

    # 好
    hash = { one: 1, two: 2, three: 3 }
    ```

* <a name="no-mixed-hash-syntaces"></a>
  別在同一個雜湊表示中混用 Ruby 1.9 雜湊表示式和火箭表示式（=>）。
  當你的鍵值不是符號時，他就是雜湊火箭表示式
<sup>[[link](#no-mixed-hash-syntaces)]</sup>

  ```Ruby
  # 不好
  { a: 1, 'b' => 2 }

  # 好
  { :a => 1, 'b' => 2 }
  ```

* <a name="hash-key"></a>
  使用 `Hash#key?` 取代 `Hash#has_key?` 和
  使用 `Hash#value?` 取代 `Hash#has_value?`。
<sup>[[link](#hash-key)]</sup>

  ```ruby
  # 不好
  hash.has_key?(:test)
  hash.has_value?(value)

  # 好
  hash.key?(:test)
  hash.value?(value)
  ```

* <a name="hash-each"></a>
  使用 `Hash#each_key` 取代 `Hash#keys.each`
  和使用 `Hash#each_value` 取代 `Hash#values.each` 。
<sup>[[link](#hash-each)]</sup>

  ```ruby
  # 不好
  hash.keys.each { |k| p k }
  hash.values.each { |v| p v }
  hash.each { |k, _v| p k }
  hash.each { |_k, v| p v }

  # 好
  hash.each_key { |k| p k }
  hash.each_value { |v| p v }
  ```

* <a name="hash-fetch"></a>
  在處理需要出現的雜湊鍵時，使用 `fetch` 。
<sup>[[link](#hash-fetch)]</sup>

    ```Ruby
    heroes = { 蝙蝠俠: 'Bruce Wayne', 超人: 'Clark Kent' }
    # 差勁 - 如果我們打錯字的話，我們就無法找到對的英雄了
    heroes[:蝙蝠俠] # => "Bruce Wayne"
    heroes[:超女] # => nil

    # 棒 - fetch 會拋出一個 KeyError 來體現這個問題
    heroes.fetch(:supermann)
    ```

* <a name="hash-fetch-defaults"></a>
  透過 `Hash#fetch` 來引入雜湊的值，而非使用邏輯符。
<sup>[[link](#hash-fetch-defaults)]</sup>

  ```Ruby
  batman = { name: 'Bruce Wayne', is_evil: false }

  # 不好 - 如果只使用 || 運算符與假值，我們不會得到預期的結果
  batman[:is_evil] || true # => true

  # 好 - fetch 與假值正常運作中
  batman.fetch(:is_evil, true) # => false
  ```

* <a name="use-hash-blocks"></a>
  如果程式碼需要求值，可能會導致副作用或是成本過高時，偏好使用程式區塊（block）取代 `Hash#fetch` 裡的預設值。
  <sup>[[link](#use-hash-blocks)]</sup>

  ```ruby
  batman = { name: 'Bruce Wayne' }

  # 不好 - 如果我們使用預設值，便急於對他求值
  # 發生多次的話它將會拖慢程式
  batman.fetch(:powers, obtain_batman_powers) # obtain_batman_powers 是個非常重量級的呼叫

  # 好 - 程式區塊為惰性求值，他僅會在 KeyError 異常時作動
  batman.fetch(:powers) { obtain_batman_powers }
  ```

* <a name="hash-values-at"></a>
  當你需要從雜湊中連續檢索多個值時，使用`Hash#values_at`。
<sup>[[link](#hash-values-at)]</sup>

  ```ruby
  # 不好
  email = data['email']
  username = data['nickname']

  # 好
  email, username = data.values_at('email', 'nickname')
  ```


* <a name="ordered-hashes"></a>
  相信這個事實吧，Ruby 1.9 的雜湊是有序的。
<sup>[[link](#ordered-hashes)]</sup>

* <a name="no-modifying-collections"></a>
  在遍歷一個集合時，不要改動它。
<sup>[[link](#no-modifying-collections)]</sup>

* <a name="accessing-elements-directly"></a>
  當要取用集合的元素時，如有提供的話盡量使用讀取器的替代方法，避免直接使用 `[n]` 取用。
  這可以讓你避免從 `nil` 上呼叫 `[]`。
<sup>[[link](#accessing-elements-directly)]</sup>

  ```ruby
  # 不好
  Regexp.last_match[1]

  # 好
  Regexp.last_match(1)
  ```

* <a name="provide-alternate-accessor-to-collections"></a>
  要為一個集合提供存取器時，提供一個替代方法在存取時幫使用者檢查是否存取到 `nil`。
<sup>[[link](#provide-alternate-accessor-to-collections)]</sup>

  ```ruby
  # 不好
  def awesome_things
    @awesome_things
  end

  # 好
  def awesome_things(index = nil)
    if index && @awesome_things
      @awesome_things[index]
    else
      @awesome_things
    end
  end
  ```

## 數字

* <a name="integer-type-checking"></a>
  使用 `Integer` 來檢查整數的類型，因為 `Fixnum` 的定義取決於平台，在 32 位元與 64 位元的機器上會回傳不同的結果。
<sup>[[link](#integer-type-checking)]</sup>

  ```ruby
  timestamp = Time.now.to_i

  # 不好
  timestamp.is_a? Fixnum
  timestamp.is_a? Bignum

  # 好
  timestamp.is_a? Integer
  ```

  * <a name="random-numbers"></a>
    在生成隨機數時偏好使用範圍（ranges），而不是帶偏移量的整數，因為他明確的展現你的意圖，想像一下你真的在骰一個骰子。
  <sup>[[link](#random-numbers)]</sup>

    ```ruby
    # 不好
    rand(6) + 1

    # 好
    rand(1..6)
    ```

## 字串

* <a name="string-interpolation"></a>
  偏好字串插值 (interpolation)，而不是字串串接 (concatenation)。
<sup>[[link](#string-interpolation)]</sup>

    ```Ruby
    # 不好
    email_with_name = user.name + ' <' + user.email + '>'

    # 好
    email_with_name = "#{user.name} <#{user.email}>"

    # 好
    email_with_name = format('%s <%s>', user.name, user.email)
    ```

* <a name="consistent-string-literals"></a>
  選擇一個統一的字串風格吧！在 Ruby 社群中有兩種普遍的方法，分別是預設使用單引號（選項一）跟預設使用雙引號（選項二）。
<sup>[[link](#consistent-string-literals)]</sup>

  * **(選項一)** 當你不需要插入特殊符號如 `\t`, `\n`, `'`, 等等時，偏好單引號的字串。

    ```Ruby
    # 不好
    name = "Bozhidar"

    name = 'De\'Andre'

    # 好
    name = 'Bozhidar'

    name = "De'Andre"
    ```

  * **(選項二)** 偏好使用雙引號，除非你的字串內包含 `"` 或是你必須要避免跳脫字元。

    ```Ruby
    # 不好
    name = 'Bozhidar'

    sarcasm = "I \"like\" it."

    # 好
    name = "Bozhidar"

    sarcasm = 'I "like" it.'
    ```

  在這個指南中，字串風格是採用前者。

* <a name="no-character-literals"></a>
  別用字元表示語法 `?x`，從 Ruby 1.9 之後他已經是多餘的，
  `?x` 會被解譯成 `'x'`（內有一個字元的字串）。
<sup>[[link](#no-character-literals)]</sup>

  ```Ruby
  # 不好
  char = ?c

  # 好
  char = 'c'
  ```

* <a name="curlies-interpolate"></a>
  別忘了使用 `{}` 圍繞要被插入字串的實體與全域變數。
<sup>[[link](#curlies-interpolate)]</sup>

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

* <a name="no-to-s"></a>
  別使用 `Object#to_s` 在字串的插入中，他會自動調用的。
<sup>[[link](#no-to-s)]</sup>

  ```Ruby
  # 不好
  message = "This is the #{result.to_s}."

  # 好
  message = "This is the #{result}."
  ```

* <a name="concat-strings"></a>
  當你需要建構龐大的資料區段（chunk）時，避免使用 `String#+` 。
  使用 `String#<<` 來替代。字串串接在對的地方改變字串實體，並且永遠比 `String#+` 來得快，`String#+` 創造了一堆新的字串物件。
<sup>[[link](#concat-strings)]</sup>

    ```Ruby
    # 不好
    html = ''
    html += '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html += "<p>#{paragraph}</p>"
    end

    # 好又快
    html = ''
    html << '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```

* <a name="dont-abuse-gsub"></a>
  如果你有更好更快的選擇，別使用 `String#gsub`。
<sup>[[link](#dont-abuse-gsub)]</sup>

    ```ruby
    url = 'http://example.com'
    str = 'lisp-case-rules'

    # 不好
    url.gsub('http://', 'https://')
    str.gsub('-', '_')

    # 好
    url.sub('http://', 'https://')
    str.tr('-', '_')
    ```

* <a name="heredocs"></a>
  當使用多行字串的 heredocs 時，請記得他們保留空白字符的事實，
  使用一些修剪空白的邊距是很好的做法。
<sup>[[link](#heredocs)]</sup>

  ```ruby
  code = <<-END.gsub(/^\s+\|/, '')
    |def test
    |  some_method
    |  other_method
    |end
  END
  # => "def test\n  some_method\n  other_method\nend\n"
  ```

* <a name="squiggly-heredocs"></a>
  使用 Ruby 2.3 新增的`<<~` 操作符來縮排 heredocs 中的多行文本。
<sup>[[link](#squiggly-heredocs)]</sup>

  ```Ruby
  # 差 — 使用 Powerpack 專案的 String#strip_margin
  code = <<-RUBY.strip_margin('|')
    |def test
    |  some_method
    |  other_method
    |end
  RUBY

  # 差
  code = <<-RUBY
  def test
    some_method
    other_method
  end
  RUBY

  # 好
  code = <<~RUBY
    def test
      some_method
      other_method
    end
  RUBY
  ```

* <a name="heredoc-delimiters"></a>
  為 heredocs 使用描述性分隔符並且添加有關 heredoc 內容的信息，
  附帶的好處是，如果使用正確的分隔符，某些編輯器可以突出顯示 heredocs 中的程式碼。
<sup>[[link](#heredoc-delimiters)]</sup>

  ```Ruby
  # 不好
  code = <<~END
    def foo
      bar
    end
  END

  # 好
  code = <<~RUBY
    def foo
      bar
    end
  RUBY

  # 好
  code = <<~SUMMARY
    An imposing black structure provides a connection between the past and
    the future in this enigmatic adaptation of a short story by revered
    sci-fi author Arthur C. Clarke.
  SUMMARY
  ```

## 日期與時間

* <a name="time-now"></a>
  當需要現在系統時間時，偏好使用 `Time.now` 勝於 `Time.new` 。
<sup>[[link](#time-now)]</sup>

* <a name="no-datetime"></a>
  除非你的計算需要考慮歷史上的曆法變革，否則避免使用 `DateTime` 。但如果你使用了，請確實設置 `start` 參數來清楚描述你的意圖。
<sup>[[link](#no-datetime)]</sup>

  ```Ruby
  # 不好 - 對於現在時間使用 DateTime
  DateTime.now

  # 好 - 對於現在時間使用 Time
  Time.now

  # 不好 - 對於近代日期使用 DateTime
  DateTime.iso8601('2016-06-29')

  # 好 - 對於近代日期使用 Date
  Date.iso8601('2016-06-29')

  # 好 - 對於歷史日期使用 DateTime 且明確定義 start 參數為 ENGLAND 曆法
  DateTime.iso8601('1751-04-23', Date::ENGLAND)
  ```

## 正規表示法

> 有些人在面對問題時，不經大腦便認為，「我知道，這裡該用正規表示法」。現在問題反倒變成兩個了。<br>
> -- Jamie Zawinski

* <a name="no-regexp-for-plaintext"></a>
  如果你只需要在字串中簡單的搜索文字，不要使用正規表示法：`string['text']`
<sup>[[link](#no-regexp-for-plaintext)]</sup>

* <a name="regexp-string-index"></a>
  針對簡單的字串查詢，你可以直接在字串索引中直接使用正規表示法。
<sup>[[link](#regexp-string-index)]</sup>

    ```Ruby
    match = string[/regexp/]             # 獲得匹配正規表示法的內容
    first_group = string[/text(grp)/, 1] # 或得分組的內容
    string[/text (grp)/, 1] = 'replace'  # string => 'text replace'
    ```
* <a name="non-capturing-regexp"></a>
  當你不需要替結果分組時，使用非分組的群組。
<sup>[[link](#non-capturing-regexp)]</sup>

    ```Ruby
    # 不好
    /(first|second)/

    # 好
    /(?:first|second)/
    ```

* <a name="no-perl-regexp-last-matchers"></a>
  別用難以理解的老式 Perl 變數來表示最後的正規表示式匹配值（像是 `$1`、`$2` 等等)，使用 `Regexp.last_match(n)` 取代。
<sup>[[link](#no-perl-regexp-last-matchers)]</sup>


    ```Ruby
    /(regexp)/ =~ string
    ...

    # 不好
    process $1

    # 好
    process Regexp.last_match(1)
    ```

* <a name="no-numbered-regexes"></a>
  避免使用編號群組，因為它們很難追蹤它們包含什麼。可以使用命名群組來替代。
<sup>[[link](#no-numbered-regexes)]</sup>

  ```Ruby
  # 不好
  /(regexp)/ =~ string
  # 一些程式碼
  process Regexp.last_match(1)

  # 好
  /(?<meaningful_var>regexp)/ =~ string
  # 一些程式碼
  process meaningful_var
  ```

* <a name="limit-escapes"></a>
  字元類別只有幾個你需要關心的特殊字元：`^`, `-`, `\`, `]`，所以你不用逃脫字元 `.` 或在 `[]` 的中括號。
<sup>[[link](#limit-escapes)]</sup>

* <a name="caret-and-dollar-regexp"></a>
  小心使用 `^` 與 `$` ，它們匹配的是一行的開始與結束，不是字串的開始與結束。如果你想要匹配整個字串，使用 `\A` 與 `\z`。(譯註：`\Z` 實為 `/\n?\z/`，使用 `\z` 才能匹配到有含新行的字串的結束)
<sup>[[link](#caret-and-dollar-regexp)]</sup>

    ```Ruby
    string = "some injection\nusername"
    string[/^username$/]   # 匹配
    string[/\Ausername\z/] # 無匹配
    ```
* <a name="comment-regexes"></a>
  針對複雜的正規表示法，使用 `x` 修飾符。這讓它們的可讀性更高並且你可以加入有用的註解。只是要小心忽略的空白。
<sup>[[link](#comment-regexes)]</sup>

    ```Ruby
    regexp = %r{
      start         # 一些文字
      \s            # 空白字元
      (group)       # 第一組
      (?:alt1|alt2) # 一些替代方案
      end
    }x
    ```

* <a name="gsub-blocks"></a>
  針對複雜的替換，`sub` 或 `gsub` 可以與區塊或雜湊來使用。
<sup>[[link](#gsub-blocks)]</sup>

  ```Ruby
  words = 'foo bar'
  words.sub(/f/, 'f' => 'F') # => 'Foo bar'
  words.gsub(/\w+/) { |word| word.capitalize } # => 'Foo Bar'
  ```

## 百分比字面

* <a name="percent-q-shorthand"></a>
  使用 `%()`（比 `%Q` 還短) 給需要插值與嵌入雙引號的單行字串。多行字串，偏好使用 heredocs 。
<sup>[[link](#percent-q-shorthand)]</sup>

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

* <a name="percent-q"></a>
  避免使用 `%()` 或是 `%q()` 除非你的的字串中同時擁有 `'` 與
  `"`。偏好可讀性高的一般字串表示法，除非其中有太多跳脫字元。
<sup>[[link](#percent-q)]</sup>

    ```Ruby
    # 不好
    name = %q(Bruce Wayne)
    time = %q(8 o'clock)
    question = %q("What did you say?")

    # 好
    name = 'Bruce Wayne'
    time = "8 o'clock"
    question = '"What did you say?"'
    quote = %q(<p class='quote'>"What did you say?"</p>)
    ```

* <a name="percent-r"></a>
  正規表示法要匹配多於一個的 `/` 字元時，使用 `%r`。
<sup>[[link](#percent-r)]</sup>

    ```Ruby
    # 不好
    %r{\s+}

    # 好
    %r{^/(.*)$}
    %r{^/blog/2011/(.*)$}
    ```

* <a name="percent-x"></a>
  避免使用 `%x` 除非你要調用帶有 `` ` `` 的指令（這不太可能）。
<sup>[[link](#percent-x)]</sup>

  ```Ruby
  # 不好
  date = %x(date)

  # 好
  date = `date`
  echo = %x(echo `date`)
  ```

* <a name="percent-s"></a>
  避免使用 `%s`，社群似乎一致認同 `:"some string"` 是建立有空格符號的最佳解。
<sup>[[link](#percent-s)]</sup>

* <a name="percent-literal-braces"></a>
  偏好 `()` 作為各種 `%` 字面的最佳分隔符。
  <sup>[[link](#percent-literal-braces)]</sup>
  - `()` 用於字串的字面表示（`%q`, `%Q`）。
  - `[]` 用於陣列的字面表示（`%w`, `%i`, `%W`, `%I`）如同一般陣列一樣對齊。
  - `{}` 用於正規表示式（`%r`），因為括號（`()`）在正規表示式中經常出現，因此使用少見的 `{` 在 `%r` 中是最好的選擇。
  - `()` 用於其他的字面表示（像是 `%s`, `%x`）。

  ```Ruby
  # 不好
  %q{"Test's king!", John said.}

  # 好
  %q("Test's king!", John said.)

  # 不好
  %w(one two three)
  %i(one two three)

  # 好
  %w[one two three]
  %i[one two three]

  # 不好
  %r((\w+)-(\d+))
  %r{\w{1,2}\d{2,5}}

  # 好
  %r{(\w+)-(\d+)}
  %r|\w{1,2}\d{2,5}|
  ```

## 元程式設計

* <a name="no-needless-metaprogramming"></a>
  避免無謂的元程式設計。
<sup>[[link](#no-needless-metaprogramming)]</sup>

* <a name="no-monkey-patching"></a>
  寫一個函式庫時不要在核心類別搗亂（不要替它們加 monkey patch。）
<sup>[[link](#no-monkey-patching)]</sup>

* <a name="block-class-eval"></a>
  偏好區塊形式的 `class_eval` 勝於字串插值 (string-interpolated)的形式。
<sup>[[link](#block-class-eval)]</sup>

  - 當你使用字串插值形式時，總是提供 `__FILE__` 及 `__LINE__`，使你的 backtrace 看起來有意義：

    ```ruby
    class_eval "def use_relative_model_naming?; true; end", __FILE__, __LINE__
    ```

  - 偏好 `define_method` 勝於 `class_eval{ def ... }`

* <a name="eval-comment-docs"></a>
  當使用 `class_eval` （或其它的 `eval`）搭配字串插值時，添加一個註解區塊，來顯示如果做了插值的樣子（我從 Rails 程式碼學來的一個實踐）：
<sup>[[link](#eval-comment-docs)]</sup>

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

* <a name="no-method-missing"></a>
  元程式設計避免使用 `method_missing`。會讓 Backtraces 變得很凌亂；行為沒有列在 `#methods` 裡；拼錯的方法呼叫可能默默的工作（`nukes.launch_state = false`)。考慮使用 delegation, proxy, 或是 `define_method` 來取代。如果你必須使用 `method_missing`，
<sup>[[link](#no-method-missing)]</sup>

  - 確保也定義了 [`respond_to_missing?`](http://blog.marc-andre.ca/2010/11/methodmissing-politely.html)
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

* <a name="prefer-public-send"></a>
  偏好使用 `public_send` 更勝於 `send`，如此一來可以避開 `private`/`protected` 的洩漏。
<sup>[[link](#prefer-public-send)]</sup>

  ```Ruby
  # 我們有一個包含 Activatable 模組的 ActiveModel Organization
  module Activatable
    extend ActiveSupport::Concern

    included do
      before_create :create_token
    end

    private

    def reset_token
      # 一些程式碼
    end

    def create_token
      # 一些程式碼
    end

    def activate!
      # 一些程式碼
    end
  end

  class Organization < ActiveRecord::Base
    include Activatable
  end

  linux_organization = Organization.find(...)
  # 不好 - 私有方法被使用
  linux_organization.send(:reset_token)
  # 好 - 會拋出例外
  linux_organization.public_send(:reset_token)
  ```

* <a name="prefer-__send__"></a>
  偏好 `__send__` 勝於 `send`，後者可能會與現有方法重疊。
<sup>[[link](#prefer-__send__)]</sup>

  ```Ruby
  require 'socket'

  u1 = UDPSocket.new
  u1.bind('127.0.0.1', 4913)
  u2 = UDPSocket.new
  u2.connect('127.0.0.1', 4913)
  # 並不會傳遞訊息給接收器物件
  # 取而代之的是他會經由 UDP socket 傳遞訊息
  u2.send :sleep, 0
  # 以下的方法才會確實的傳遞訊息給接收器物件
  u2.__send__ ...
  ```

## 其它

* <a name="always-warn"></a>
  `ruby -w` 寫出安全的程式碼。
<sup>[[link](#always-warn)]</sup>

* <a name="no-optional-hash-params"></a>
  避免使用雜湊作為選擇性參數。這個方法是不是做太多事情了？（物件初始器是本規則的例外）
<sup>[[link](#no-optional-hash-params)]</sup>

* <a name="short-methods"></a>
  避免方法長於 10 行程式碼（LOC）。理想上，大部分的方法會小於5行。空行不算進 LOC 裡。
<sup>[[link](#short-methods)]</sup>

* <a name="too-many-params"></a>
  避免參數列表長於三或四個參數。
<sup>[[link](#too-many-params)]</sup>

* <a name="private-global-methods"></a>
  如果你真的需要全域方法，把它們加入到 Kernel 並設為私有的。
<sup>[[link](#private-global-methods)]</sup>

* <a name="instance-vars"></a>
  使用模組變數取代全域變數。
<sup>[[link](#instance-vars)]</sup>

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

* <a name="optionparser"></a>
  使用 `OptionParser` 來解析複雜的命令行選項及 `ruby -s` 來處理瑣碎的命令行選項。
<sup>[[link](#optionparser)]</sup>

* <a name="functional-code"></a>
  用函數式的方法寫程式，在有意義的情況下避免賦值。
<sup>[[link](#functional-code)]</sup>

* <a name="no-param-mutations"></a>
  不要變動參數，除非那是方法的目的。
<sup>[[link](#no-param-mutations)]</sup>

* <a name="three-is-the-number-thou-shalt-count"></a>
  避免超過三行的巢狀區塊。
<sup>[[link](#three-is-the-number-thou-shalt-count)]</sup>

* <a name="be-consistent"></a>
  保持一致性。在理想的世界裡，遵循這些準則。
<sup>[[link](#be-consistent)]</sup>

* <a name="common-sense"></a>
  使用常識。
<sup>[[link](#common-sense)]</sup>

## 工具

以下是一些工具，讓你自動檢查 Ruby 程式碼是否符合本指南。

### RuboCop

[RuboCop](https://github.com/bbatsov/rubocop) 是一個基於本指南的 Ruby 語言風格檢查器，幾乎包含了此指南重要的部分，支援 MRI 1.9、MRI 2.0 還有良好的 Emacs 協作。

### RubyMine

[RubyMine](http://www.jetbrains.com/ruby/)的程式碼檢查[部分基於](http://confluence.jetbrains.com/display/RUBYDEV/RubyMine+Inspections)本指南。

# 貢獻

本指南仍處於不斷更新的狀態 &mdash; 有些規則缺乏範例、有些規則則是缺乏清楚的例子來說明他，協助改進這些規則是一個很好（同時也簡單）的方法來幫助 Ruby 社群。

時機成熟時，（希望）這些問題將會被解決 &mdash; 記著吧！

在本指南所寫的每個東西都不是定案。這只是我渴望想與同樣對 Ruby 程式設計風格有興趣的大家一起工作，以致於最終我們可以替整個 Ruby 社群創造一個有益的資源。

歡迎開票或發送一個帶有改進的更新請求。在此提前感謝你的幫助！

你同時也可以透過[Gratipay](https://gratipay.com/~bbatsov/)來為本指南（還有 RuboCop）提供一些財務上的貢獻。

[![Support via Gratipay](https://cdn.rawgit.com/gratipay/gratipay-badge/2.3.0/dist/gratipay.png)](https://gratipay.com/~bbatsov/)

## 如何貢獻？

真的很簡單，只要照著[貢獻指南](https://github.com/bbatsov/ruby-style-guide/blob/master/CONTRIBUTING.md)即可。

# 授權

![Creative Commons License](http://i.creativecommons.org/l/by/3.0/88x31.png)
This work is licensed under a [Creative Commons Attribution 3.0 Unported License](http://creativecommons.org/licenses/by/3.0/deed.zh_TW)

# 口耳相傳

一份社群策動的風格指南，對一個社群來說，只是讓人知道有這個社群。推特這個指南，分享給你的朋友或同事。我們得到的每個註解、建議或意見都可以讓這份指南變得更好一點。而我們想要擁有的是最好的指南，不是嗎？

共勉之，<br>
[Bozhidar](https://twitter.com/bbatsov)
