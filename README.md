# Bike Startup & Co. Ruby スタイルガイド

株式会社 自転車創業 の Rubyスタイルガイドです。
modified Airbnb Style Guide

#目次

1. [空白](#空白)
  - [インデント](#インデント)
  - [インライン](#インライン)
  - [改行](#改行)
2. [コードの長さ](#コードの長さ)
3. [コメント](#コメント)
  - [ファイルやクラスに対するコメント](#ファイルやクラスに対するコメント)
  - [機能のコメント](#機能のコメント)
  - [ブロックやインラインのコメント](#ブロックやインラインのコメント)
  - [句読法、綴り、文法について](#句読法、綴り、文法について)
  - [TODO管理](#TODO管理)
  - [コメントアウトしたコード](#コメントアウトしたコード)
4. [メソッド](#メソッド)
  - [メソッドの定義](#メソッドの定義)
  - [メソッドの呼び出し方](#メソッドの呼び出し方)
  - [条件式](#条件式)
5. [条件式のキーワード](#条件式のキーワード)
  - [三項演算子](#三項演算子)
  - [ネストされた条件式について](#ネストされた条件式について)
6. [構文](#構文)
7. [命名規則](#命名規則)
8. [クラス](#クラス)
9. [例外処理](#例外処理)
10. [データ格納](#データ格納)
11. [文字列](#文字列)
12. [正規表現](#正規表現)
13. [%記法](#%記法)
  - [スコープ](#スコープ)


#空白
##インデント

・２つインデントを開ける際には、ソフトタブを使う
・caseとwhenは同じインデントの深さで表現すること

```ruby

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

・引数は全て同じ列、もしくは横一列目に記述すること

```ruby
# bad
def self.create_translation(phrase_id, phrase_key, target_locale,
                            value, user_id, do_xss_check, allow_verification)
  ...
end

# good
def self.create_translation(phrase_id,
                            phrase_key,
                            target_locale,
                            value,
                            user_id,
                            do_xss_check,
                            allow_verification)
  ...
end

# good
def self.create_translation(
  phrase_id,
  phrase_key,
  target_locale,
  value,
  user_id,
  do_xss_check,
  allow_verification
)
  ...
end
```

・複数行における真偽表現は、インデントの後に記述する

```ruby
# bad
def is_eligible?(user)
  Trebuchet.current.launch?(ProgramEligibilityHelper::PROGRAM_TREBUCHET_FLAG) &&
  is_in_program?(user) &&
  program_not_expired
end

# good
def is_eligible?(user)
  Trebuchet.current.launch?(ProgramEligibilityHelper::PROGRAM_TREBUCHET_FLAG) &&
    is_in_program?(user) &&
    program_not_expired
end

```

##インライン
・末尾の空白スペースを残さないこと
・インラインコメント（単一行のコメント）をする際には、コードの最後とコメントの間に空白を１つ挟むこと

```ruby
# bad
result = func(a, b)# we might want to change b to c

# good
result = func(a, b) # we might want to change b to c
```

・コンマの後、コロンの後、セミコロンの後や{の周りと、}の前では空白を１つ使うこと

```ruby
sum = 1 + 2
a, b = 1, 2
1 > 2 ? true : false; puts 'Hi'
[1, 2, 3].each { |e| puts e }
```
・コンマの前には空白を含めてはならない

```ruby
result = func(a, b)
```
・パラメーター間には空白を１つ開けること。しかし、ブロック内、パラメーターの外側は空白を開けないこと。
さらに、ブロックの外側は空白を開けること。

```ruby
# bad
{}.each { | x,  y |puts x }

# good
{}.each { |x, y| puts x }
```

・！とその引数の間には空白を開けないこと

```ruby
!something
```

・[]、()の周りには空白を作らないこと

```ruby
some(arg).other
[1, 2, 3].length
```
・文字列補完を行う場合は、空白を省くこと

```ruby
# bad
var = "This #{ foobar } is interpolated."

# good
var = "This #{foobar} is interpolated."
```

・範囲リテラルにおいては空白を追加しないこと

```ruby
# bad
(0 ... coll).each do |item|

# good
(0...coll).each do |item|
```

##改行
・実際の処理文章とif条件文の違いをわかりやすくするため、複数行に渡るif条件文を書く際には、
改行を行うこと。

```ruby
if @reservation_alteration.checkin == @reservation.start_date &&
   @reservation_alteration.checkout == (@reservation.start_date + @reservation.nights)

  redirect_to_alteration @reservation_alteration
end
```

・条件文、ブロック、case文の後は改行すること。

```ruby
if robot.is_awesome?
  send_robot_present
end

robot.add_trait(:human_like_intelligence)
```

・異なるインデントが存在する際（クラスの周辺や処理文本体など）には改行をしてはならない。

```ruby
# bad
class Foo

  def bar
    # body omitted
  end

end

# good
class Foo
  def bar
    # body omitted
  end
end
```

・メソッド間においては、行を１つ挟まなければならないが、２つ以上挟んではならない。

```ruby
def a
end

def b
end
```

・メソッド内部において、コードをわかりやすく細分化する際には、行を１行開けること。

```ruby
def transformorize_car
  car = manufacture(options)
  t = transformer(robot, disguise)

  car.after_market_mod!
  t.transform(car)
  car.assign_cool_name!

  fleet.add(car)
  car
end
```

・ファイルの最後は行を1行開けること。複数行開けてはならない。

#コードの長さ
・特別な理由がない限り、コードは読みやすいように心がけること。　1行の長さは１００単語以内にすること。
#コメントの仕方
この章はgoogleのc++とpythonのスタイルガイドから、学んでいる部分が多いです。

##ファイルやクラスに関するコメント
クラスを作る際には、そのクラスがどう使われ、何のために作られたのかをコメントとして明記すること。
クラスがあろうとなかろうと、全てのファイルにはその最上部にそのファイルの内容を記すこと。

```ruby
# Automatic conversion of one locale to another where it is possible, like
# American to British English.
module Translation
  # Class for converting between text between similar locales.
  # Right now only conversion between American English -> British, Canadian,
  # Australian, New Zealand variations is provided.
  class PrimAndProper
    def initialize
      @converters = { :en => { :"en-AU" => AmericanToAustralian.new,
                               :"en-CA" => AmericanToCanadian.new,
                               :"en-GB" => AmericanToBritish.new,
                               :"en-NZ" => AmericanToKiwi.new,
                             } }
    end

  ...

  # Applies transforms to American English that are common to
  # variants of all other English colonies.
  class AmericanToColonial
    ...
  end

  # Converts American to British English.
  # In addition to general Colonial English variations, changes "apartment"
  # to "flat".
  class AmericanToBritish < AmericanToColonial
    ...
  end
```

コンフィグやデータも含む全てのファイルは、ファイルレベルでのコメントが必要。

```ruby
# List of American-to-British spelling variants.
#
# This list is made with
# lib/tasks/list_american_to_british_spelling_variants.rake.
#
# It contains words with general spelling variation patterns:
#   [trave]led/lled, [real]ize/ise, [flav]or/our, [cent]er/re, plus
# and these extras:
#   learned/learnt, practices/practises, airplane/aeroplane, ...

sectarianizes: sectarianises
neutralization: neutralisation
...

```

##機能に関するコメント
全ての機能宣言は、その宣言のすぐ前にコメントとして、説明を入れること。その内容はその機能が何であり、
何に使われるのか、である。
これらのコメントは命令的であるよりも、とにかくわかりやすくすることに注力すること。
すなわち、機能の前に「ファイルを開け」と書くよりも「これはファイルを開くもの」と書くこと。
機能を説明するものであり、その機能に何をするべきかを伝えるものではない。

また、これらのコメントはその機能がどうタスクと処理するかを明記してはならない。どう処理するかに関しては、
コードの中に散りばめられているコメントにて対応する。

全ての機能は、入力が何であり、出力が何かについて詳細に言及しなければならない。
しかし、その機能が下記条件に当てはまる場合はその限りではない。

・外部からは見えない
・とても短い
・明らかである

どんなフォーマットでも利用することが可能だが、rubyでは有名な２つのフォーマットがあり、TomDocとYARDである。
これらを使うことで下記のコードのように、簡潔にコーディングできる。

```ruby
# Returns the fallback locales for the_locale.
# If opts[:exclude_default] is set, the default locale, which is otherwise
# always the last one in the returned list, will be excluded.
#
# For example:
#   fallbacks_for(:"pt-BR")
#     => [:"pt-BR", :pt, :en]
#   fallbacks_for(:"pt-BR", :exclude_default => true)
#     => [:"pt-BR", :pt]
def fallbacks_for(the_locale, opts = {})
  ...
end
```

##ブロックとインラインに関するコメントについて
コメントの最後の部分は、コーディングの中でも扱いづらい部分である。
もし、次のコードレビューにて、そのコードについて説明しなければならない時、
次ではなくその場でコメントし、説明するべきである。

複雑な処理はその処理が始まる前に、何行かのコメントを要するものである。
明らかではない処理は、その行の終わりにコメントを持たせるべきである。

```ruby
def fallbacks_for(the_locale, opts = {})
  # dup() to produce an array that we can mutate.
  ret = @fallbacks[the_locale].dup

  # We make two assumptions here:
  # 1) There is only one default locale (that is, it has no less-specific
  #    children).
  # 2) The default locale is just a language. (Like :en, and not :"en-US".)
  if opts[:exclude_default] &&
      ret.last == default_locale &&
      ret.last != language_from_locale(the_locale)
    ret.pop
  end

  ret
end
```

一方で、そのコードの中身を詳細に明記してはならない。あなたの書いたそのコードの意味合いを、あなた以上によく理解してくれる人がいるかもしれないからである。

関連情報として、ブロック型のコメントは使うことができない。空白(ホワイトスペース)の後にそれらがあってはならないし、普通のコメントと同じくらい見やすくあってもいけない。

```ruby
# bad
=begin
comment line
another comment line
=end

# good
# comment line
# another comment line
```

##句読法、綴り、文法について
句読法、綴り、文法に関しては注意を払うべきである。そうすれば悪く書かれたコードよりも、よりよく書かれたコードを読みやすくなる。
コメントは適切な大文字や句読法を利用することにより、説明文と同じくらい良いやすくなければならない。
多くの場合において、完成された文章は断片的な文章よりもわかり易いものである。
コードの最後にあるような短いコメントは時として型崩れになることがあるが、それでも書き方に関しては一貫性を持たせる必要がある。
セミコロンを使うべきところにあなたがコロンを使っている時、それを指摘するレビュワーがいると、イライラするかもしれない。
だが、コードをより見やすくわかりやすく維持するためには絶対に必要である。
適切な句読法や綴り、文法はその目標を達成するための助けとなるだろう。

##TODO管理
TODOコメントは、一時的、短期間の解決策になりうる、もしくは完璧とはいえないまでも十分に良いコードのために使われるべきだ。
TODOコメントは、大文字のTODOという文字を含めなければならない。またそのコメントによって参照できる問題に関して、もっともうまくその事情を説明できている人のフルネームも付け加えるべきである。また、そのフルネームはカッコで囲うべきである。コロンは任意だ。
コメントは何がなされるべきかを説明するコメントでなければならない。
要求に対して、より多くの詳細情報を提供できる人を探しやすくするために、一貫したTODO管理フォーマットを持つことが主な理由である。
TODOコメントにおいては、その名前を書かれた人が必ずその問題を解決しなければならないというわけではない。
従って、もしTODOコメントをあなたが作れば、ほぼ全てあなたの名前がつけられることになる。

```ruby
# bad
  # TODO(RS): Use proper namespacing for this constant.

  # bad
  # TODO(drumm3rz4lyfe): Use proper namespacing for this constant.

  # good
  # TODO(Ringo Starr): Use proper namespacing for this constant.
```

##コメントアウトしたコード
・コメントアウトしたコードは決してそのままにはしておかないこと。


#メソッド
##メソッドの定義
・引数がある場合は、括弧付きでdefを利用するべき。引数がないときは括弧は省くべき。

```ruby
def some_method
  # body omitted
end

def some_method_with_parameters(arg1, arg2)
  # body omitted
end
```
・デフォルトの固定引数を使ってはならない。キーワード引数か（Ruby2.0もしくはそれ以降それ以降の場合）、もしくはオプションハッシュを使うべきである。

```ruby
# bad
def obliterate(things, gently = true, except = [], at = Time.now)
  ...
end

# good
def obliterate(things, gently: true, except: [], at: Time.now)
  ...
end

# good
def obliterate(things, options = {})
  options = {
    :gently => true, # obliterate with soft-delete
    :except => [], # skip obliterating these things
    :at => Time.now, # don't obliterate them until later
  }.merge(options)

  ...
end
```

・1行でのメソッド定義は避けるべきである。1行メソッドは現在でもまま使われることがあるが、
メソッドの利用を望ましくないものにする文法である。

```ruby
# bad
def too_much; something; something_else; end

# good
def some_method
  # body
end
```

##メソッドの呼び出し
メソッドを呼び出す際には、下記ルールに従い括弧を利用すること。
・メソッドが値を返す時

```ruby
# bad
@current_user = User.find_by_id 1964192

# good
@current_user = User.find_by_id(1964192)
```

・１つ目の引数が括弧を利用している時

```ruby
# bad
put! (x + y) % len, value

# good
put!((x + y) % len, value)
```
・括弧とメソッドの間には空白を開けてはならない

```ruby
# bad
f (3 + 2) + 1

# good
f(3 + 2) + 1
```

・引数が存在しない場合は、括弧は省かなければならない

```ruby
# bad
nil?()

# good
nil?
```

・もし、メソッドが値を返さない場合、もしくは返り値を気にしない場合、括弧は任意である。
（ただもし引数が複数行に渡る場合には、括弧があったほうが読みやすくはなるだろう。）

```ruby
# okay
render(:partial => 'foo')

# okay
render :partial => 'foo'
```
・もしメソッドが、オプションハッシュを最後の引数として受け取るのであれば、呼び出し内においては{}は使ってはならない。

#条件式
##条件式のキーワード
・複数行のifやunlessを使うときは、thenを使わない。

```ruby
# bad
if some_condition then
  ...
end

# good
if some_condition
  ...
end
```
・複数行のwhileやuntilにはdoは使わない。


```ruby
# bad
while x > 5 do
  ...
end

until x > 5 do
  ...
end

# good
while x > 5
  ...
end

until x > 5
  ...
end
```


・andやorやnotは禁止。&&、||や！を利用すること。
中身のコードや条件式が単純な場合、もしくは中身全てが1行にまとまるのであれば、後事修飾であるifやunlessの利用は可能である。そうでない場合は利用はやめるべきである。

```ruby
# bad - this doesn't fit on one line
add_trebuchet_experiments_on_page(request_opts[:trebuchet_experiments_on_page]) if request_opts[:trebuchet_experiments_on_page] && !request_opts[:trebuchet_experiments_on_page].empty?

# okay
if request_opts[:trebuchet_experiments_on_page] &&
     !request_opts[:trebuchet_experiments_on_page].empty?

  add_trebuchet_experiments_on_page(request_opts[:trebuchet_experiments_on_page])
end

# bad - this is complex and deserves multiple lines and a comment
parts[i] = part.to_i(INTEGER_BASE) if !part.nil? && [0, 2, 3].include?(i)

# okay
return if reconciled?
```
・else付きのunlessを利用することはできない。elseを使う場合は肯定系に直すこと。

```ruby
# bad
unless success?
  puts 'failure'
else
  puts 'success'
end

# good
if success?
  puts 'success'
else
  puts 'failure'
end
```

・複数の条件式がある場合はunlessは使えない

```ruby
# bad
  unless foo? && bar?
    ...
  end

  # okay
  if !(foo? && bar?)
    ...
  end
```

・比較演算子を使ってifを利用する際には、わざわざunlessを利用して反対の意味の比較演算子を作らないこと。

```ruby
# bad
  unless x == 10
    ...
  end

  # good
  if x != 10
    ...
  end

  # bad
  unless x < 10
    ...
  end

  # good
  if x >= 10
    ...
  end

  # ok
  unless x === 10
    ...
  end
```

・ifやunless、whileなどの条件式の周囲には括弧を使ってはならない。

```ruby
# bad
if (x > 10)
  ...
end

# good
if x > 10
  ...
end
```

##三項演算子

・非常に些細な内容のコーディングを除けば、三項演算子(?:)の利用は避けるべきである。
しかし、if、then、else、end構造である1行の条件式に関しては三項演算子(?:)を利用するべきである。

```ruby
# bad
result = if some_condition then something else something_else end

# good
result = some_condition ? something : something_else
```

・三項演算子はネストされるべきではない。もしそうしたいならば、ifやelseの構造が望ましい。

```ruby
# bad
some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

# good
if some_condition
  nested_condition ? nested_something : nested_something_else
else
  something_else
end
```
・複数行に渡る条件式の場合は三項演算子は避けるべきである。三項演算子は1行の条件式の場合に利用するべきである。
・複数行に渡る三項演算子は避け、代わりにif、then、else、endを利用するべきである。

```ruby
# bad
some_really_long_condition_that_might_make_you_want_to_split_lines ?
  something : something_else

# good
if some_really_long_condition_that_might_make_you_want_to_split_lines
  something
else
  something_else
end
```


##ネストされた条件式について
・フロー制御においてネストされた条件式の利用は避けるべきである。
もし無効なデータを利用する場合には、ガード節を利用することが望ましい。ガード節は最も早く返り値を返せる機能である。
一般条項は結局以下のようになる。
・あなたの生成した機能がこれ以上何も行うことができないと知れた場合は、すぐに返り値を返す。
・早く返り値を返すために、ネストやインデントの利用は減らすこと。これはコードを読みやすくする他、elseの行を読んでそれを記憶しておくという精神的な負担を減らせるからである。
・コアな、もしくは重要なフローに関しては最小限のインデントにするべきである。

```ruby
# bad
def compute
  server = find_server
  if server
    client = server.client
    if client
      request = client.make_request
      if request
         process_request(request)
      end
    end
  end
end

# good
def compute
  server = find_server
  return unless server
  client = server.client
  return unless client
  request = client.make_request
  return unless request
  process_request(request)
end
```
・ループ処理の条件式ブロックにおいては、nextの利用が望ましい。

```ruby
# bad
[0, 1, 2, 3].each do |item|
  if item > 1
    puts item
  end
end

# good
[0, 1, 2, 3].each do |item|
  next unless item > 1
  puts item
end
```

 Beck KentのImplementation Patternsという文書のP６８〜７０の『ガード節』という章を読んでほしい。
ここを我々は参考にしている。

#構文
理由を説明できない場合は、for文を利用してはならない。ほとんどの場合はイテレータが使われるべきである。
for文はeach文の観点から利用されるが、（そしてあなたは間接参照レベルを追加するだろう。）しかし、ひねりを加えて、すなわちforはeachとは異なり、新しいスコープにはならないため、ブロック内に定義されている変数は外から見えるようになる。

```ruby
arr = [1, 2, 3]

# bad
for elem in arr do
  puts elem
end

# good
arr.each { |elem| puts elem }
```

1行のブロックにはdo...endよりも{...}が好ましい。{...}は複数行のブロック（複数行のチェーンはいつも醜い）には使われるべきではない。do...endは制御フローやメソッド定義にて常に使われるべきである（例えばrakefileやDSLなど）。チェインする際にはdo...endの利用は避けるべきである。

```ruby
names = ["Bozhidar", "Steve", "Sarah"]

# good
names.each { |name| puts name }

# bad
names.each do |name| puts name end

# good
names.each do |name|
  puts name
  puts 'yay!'
end

# bad
names.each { |name|
  puts name
  puts 'yay!'
}

# good
names.select { |name| name.start_with?("S") }.map { |name| name.upcase }

# bad
names.select do |name|
  name.start_with?("S")
end.map { |name| name.upcase }
```

多くの人は{...}を利用することで、複数行に渡るチェインは問題ないと判断するが、ここで今一度問い直してほしい。
そのコードは本当に読みやすいか、そのブロックの中身は素晴らしいメソッドに抽出されうるか。

・簡略化した代入演算子は常に利用されなければならない。

```ruby
# bad
x = x + y
x = x * y
x = x**y
x = x / y
x = x || y
x = x && y

# good
x += y
x *= y
x **= y
x /= y
x ||= y
x &&= y
```

・1行のクラス定義内を除いては、セミコロンの利用は避けなければならない。セミコロンの利用が適切な際には、コーディングに直接隣接するべき、すなわちセミコロンの前に空白があってはならない。

```ruby
# bad
puts 'foobar'; # superfluous semicolon
puts 'foo'; puts 'bar' # two expressions on the same line

# good
puts 'foobar'

puts 'foo'
puts 'bar'

puts 'foo', 'bar' # this applies to puts in particular
```

::は定数（この定数にはクラスやモジュールも含まれている）、もしくはコンストラクタ（Array() or Nokogiri::HTML()）を参照する際にのみ使われる。通常のメソッド呼び出しに使われるべきではない。

```ruby
# bad
SomeClass::some_method
some_object::some_method

# good
SomeClass.some_method
some_object.some_method
SomeModule::SomeClass::SOME_CONST
SomeModule::SomeClass()
```

・必要ないところでreturnの利用は避けるべきである。

```ruby
# bad
def some_method(some_arr)
  return some_arr.size
end

# good
def some_method(some_arr)
  some_arr.size
end
```

・条件文内においては、=の返り値は使われるべきではない。

```ruby
# bad - shows intended use of assignment
if (v = array.grep(/foo/))
  ...
end

# bad
if v = array.grep(/foo/)
  ...
end

# good
v = array.grep(/foo/)
if v
  ...
end
```

・変数を初期化するために||=は自由に利用してよい。

```ruby
# set name to Bozhidar, only if it's nil or false
name ||= 'Bozhidar'
```

・真偽値の変数においては、||=は利用してはならない。(値がfalseだった場合を考えてみるといいだろう。)

```ruby
# bad - would set enabled to true even if it was false
enabled ||= true

# good
enabled = true if enabled.nil?
```

・ラムダ式を呼び出す際には、明示的に.callを使うべきである。

```ruby
# bad
lambda.(x, y)

# good
lambda.call(x, y)
```

・Perlの特別変数($0-9や$など)は使ってはならない。それらは非常に不可解であるし、1行スクリプト以外での無制限な使用はやめさせられるべきである。$PROGRAM_NAMEなどの長いバージョンが使われるべきである。

・メソッドブロックが１つの引数しか取らない場合、その中身は属性を単に読み込むこと、もしくは引数のない１つのメソッドを呼び出すことで構成される。簡略化した、&:が使われるべきである。

```ruby
# bad
bluths.map { |bluth| bluth.occupation }
bluths.select { |bluth| bluth.blue_self? }

# good
bluths.map(&:occupation)
bluths.select(&:blue_self?)
```

・現在のインスタンスを呼び出す際にはself.some_methodよりもsome_methodが好まれる。

```ruby
# bad
def end_date
  self.start_date + self.nights
end

# good
def end_date
  start_date + nights
end
```
下記３つのケースにおいては、self.は言語のルールとして、また使いやすさから利用されるべきである。

1.クラスメソッドを定義するとき。
2.割当メソッドを呼び出す際の左辺における利用。selfがActiveRecord modelである際(self.guest = 　　user)の属性割当も含まれる。
3.現在のインスタンスクラス(self.class)を参照するとき。

・定数であるように作られた、変わりやすいタイプのオブジェクトを定義する際にはfreezeを利用すること。共通の例で言えば、文字列と配列とハッシュである。

その理由はrubyの定数は実際には変わりやすいのである。
freezeを呼び出せばそれらは全て一定になるし、修正しようとすれば例外となる。rubyver2.2以下のものはfreezeを使うことによる固定化が許されている。

```ruby
# bad
class Color
  RED = 'red'
  BLUE = 'blue'
  GREEN = 'green'

  ALL_COLORS = [
    RED,
    BLUE,
    GREEN,
  ]

  COLOR_TO_RGB = {
    RED => 0xFF0000,
    BLUE => 0x0000FF,
    GREEN => 0x00FF00,
  }
end

# good
class Color
  RED = 'red'.freeze
  BLUE = 'blue'.freeze
  GREEN = 'green'.freeze

  ALL_COLORS = [
    RED,
    BLUE,
    GREEN,
  ].freeze

  COLOR_TO_RGB = {
    RED => 0xFF0000,
    BLUE => 0x0000FF,
    GREEN => 0x00FF00,
  }.freeze
end
```

#命名規則
・メソッドや変数にはスネークケースを使うこと。
・クラスやモジュールにはキャメルケースを使うこと。
・他の定数にはスクリーミングスネークケースを使うこと。
・述語メソッド(真偽値を返すメソッド)は？で終わること。
・潜在的に危険なメソッド(selfの中身や引数を修正する)は最後に!をつけること。
  非破壊的メソッドが存在するときのみ、破壊的メソッドは利用できる。
・スローアウェイメソッドには＿を命名すること。

```ruby
version = '3.2.1'
major_version, minor_version, _ = version.split('.')
```

#クラス
・継承において扱いにくい挙動を避けるため、クラス変数の利用は避けるべきである。

```ruby
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
見ればわかるとおり、階層内における全てのクラスは、一つのクラス変数を共有している。
クラスインスタンス変数はクラス変数よりも、多く使われるべきである。

・シングルトンメソッドを定義するために、def self.methodは利用しなければならない。
こうすることでリファクタリングに対して、メソッドを変更しにくいものにする。

```ruby
class TestClass
  # bad
  def TestClass.some_method
    ...
  end

  # good
  def self.some_other_method
    ...
  end
```

・必要がない限り、class << selfの利用は避けるべきである。
例えば、単一のアクセサやエイリアス化された属性などである。

```ruby
class TestClass
  # bad
  class << self
    def first_method
      ...
    end

    def second_method_etc
      ...
    end
  end

  # good
  class << self
    attr_accessor :per_page
    alias_method :nwo, :find_by_name_with_owner
  end

  def self.first_method
    ...
  end

  def self.second_method_etc
    ...
  end
end
```

・publicやprotected、privateに関してはインデントをつける。その深さはそれらを適用しているメソッドと同じである。その上下にはブランクを作ること。

```ruby
class SomeClass
  def public_method
    # ...
  end

  private

  def private_method
    # ...
  end
end
```

#例外

・フロー制御に関しては、例外を使ってはならない。

```ruby
# bad
begin
  n / d
rescue ZeroDivisionError
  puts "Cannot divide by 0!"
end

# good
if d.zero?
  puts "Cannot divide by 0!"
else
  n / d
end
```

・例外クラスのレスキューは避けるべきである。

```ruby
# bad
begin
  # an exception occurs here
rescue Exception
  # exception handling
end

# good
begin
  # an exception occurs here
rescue StandardError
  # exception handling
end

# acceptable
begin
  # an exception occurs here
rescue
  # exception handling
end
```

・raiseの２パターンの引数に関しては、明示的にRuntimeerrorを特定してはならない。
サブクラスを用意し、エラー作成を行うべきである。

```ruby
# bad
raise RuntimeError, 'message'

# better - RuntimeError is implicit here
raise 'message'

# best
class MyExplicitError < RuntimeError; end
raise MyExplicitError
```

・例外インスタンスの代わりに、raiseするための２つの分割された引数として、例外クラスとメッセージを作成することが好まれる。

```ruby
# bad
raise SomeException.new('message')
# Note that there is no way to do `raise SomeException.new('message'), backtrace`.

# good
raise SomeException, 'message'
# Consistent with `raise SomeException, 'message', backtrace`.
```

・修飾子フォームにおいては、レスキューを使うことは避けるべきである。

```ruby
# bad
read_file rescue handle_error($!)

# good
begin
  read_file
rescue Errno:ENOENT => ex
  handle_error(ex)
end
```

#データ格納

・collectよりもmapが好まれる。
・findよりもdetectが好まれる。ActiveRecordのfindメソッドによれば、findの利用は曖昧なものである。
 detectを利用することで、ActiveRecordではなく、Rubyを利用しているということが明確になる。
・injectよりもreduceが好まれる。
・挙動をよりよくするために、lengthやcountよりもsizeが好まれる。
・パラメーターをコンストラクタに通す必要がない限り、ハッシュと配列リテラルの表記法が好まれる。

```ruby
# bad
arr = Array.new
hash = Hash.new

# good
arr = []
hash = {}

# good because constructor requires parameters
x = Hash.new { |h, k| h[k] = {} }
```

#文字列
・文字列連結よりも文字列補完の方が好ましい。

```ruby
# bad
email_with_name = user.name + ' <' + user.email + '>'

# good
email_with_name = "#{user.name} <#{user.email}>"
```

・さらにいうならば、Ruby1.9のスタイルを覚えておくと良い。
例えば、キャッシュのキーを構成を下記のようにしたいとするならば。。。。

```ruby

CACHE_KEY = '_store'

cache.write(@user.id + CACHE_KEY)

```

下記のようにしなければならない。

```ruby
CACHE_KEY = '%d_store'

cache.write(CACHE_KEY % @user.id)

```

・大きなデータを構築する必要がある際には、string#+の利用は避けるべき。
代わりに、String#<<を使うべきである。連結することで、所定の位置にある文字列が変化し、String#+よりは確実に早い。そしてそれは、新しい文字列オブジェクトの塊を生み出す。

```ruby
# good and also fast
html = ''
html << '<h1>Page title</h1>'

paragraphs.each do |paragraph|
  html << "<p>#{paragraph}</p>"
end

```

・複数行における文字列の補完を行う際には、+や<<の利用はせず、最後に\をつけること。

```ruby
# bad
"Some string is really long and " +
  "spans multiple lines."

"Some string is really long and " <<
  "spans multiple lines."

# good
"Some string is really long and " \
  "spans multiple lines."

```

#正規表現
・$１〜９の利用は避けるべきである。それらが含んでいるものをトラッキングすることが難しいからである。
代わりにグループ名をつけることで、対応する必要がある。

```ruby
# bad
/(regexp)/ =~ string
...
process $1

# good
/(?<meaningful_var>regexp)/ =~ string
...
process meaningful_var

```

・^と$の利用は気をつけなければならない。 それらは文字列の最後に合致するものを取るのではなく、行の最初と最後に合致するものを取ってくるからである。もし、もし文字列全体で合うものを探したければ、\A と\zを使うべきである。

```ruby
string = "some injection\nusername"
string[/^username$/]   # matches
string[/\Ausername\z/] # don't match
```

・複雑なregexp（正規表現）に関しては、修飾子xを利用しよう。読みやすくなるし、有用なコメントをつけることもできる。空白が無視されるので気をつけよう。

```ruby
regexp = %r{
  start         # some text
  \s            # white space char
  (group)       # first group
  (?:alt1|alt2) # some alternation
  end
}x
```

#%記法

・一貫性を持たせるため、また％記法の挙動はメソッド呼び出しに近いため、％気泡を使う際には中かっこや大かっこ、｜｜よりも、小かっこが好まれる。

```ruby
# bad
%w[date locale]
%w{date locale}
%w|date locale|

# good
%w(date locale)

```
・%wは自由に使って良い

```ruby
STATES = %w(draft open closed)
```

・埋め込みダブルクォーテーションと補間が必要な1行の文字列に関しては、%()を利用する。
複数行に渡る場合はヒアドキュメントが良い。

```ruby

# bad - no interpolation needed
%(<div class="text">Some text</div>)
# should be '<div class="text">Some text</div>'

# bad - no double-quotes
%(This is #{quality} style)
# should be "This is #{quality} style"

# bad - multiple lines
%(<div>\n<span class="big">#{exclamation}</span>\n</div>)
# should be a heredoc.

# good - requires interpolation, has quotes, single line
%(<tr><td class="name">#{name}</td>)

```

・'/'が２本以上使われる正規表現においてのみ、%rを使う。

```ruby
# bad
%r(\s+)

# still bad
%r(^/(.*)$)
# should be /^\/(.*)$/

# good
%r(^/blog/2011/(.*)$)

```

・バッククォートがあるコマンドを呼び出さない限りは、％xの利用は避けるべきである（ほとんどないとは思われるが）

```ruby
# bad
date = %x(date)

# good
date = `date`
echo = %x(echo `date`)
```


#Railsにおける規則
・renderやredirect_toを読んだ後に、すぐに返りが欲しい際にはreturnを同じ行ではなく、次に行に書くべきである。

```ruby
# bad
render :text => 'Howdy' and return

# good
render :text => 'Howdy'
return

# still bad
render :text => 'Howdy' and return if foo.present?

# good
if foo.present?
  render :text => 'Howdy'
  return
end
```

##スコープ
アクティブレコードにスコープを定義する際には、ラムダ式を利用すること。普通のリレーションではデータベースの連動時間がクラスの読み込み時間と同等になってしまう。(インスタンス起動)

```ruby

# bad
scope :foo, where(:bar => 1)

# good
scope :foo, -> { where(:bar => 1) }

```


## 変更履歴(Changelog)

  - 2018/05/30 共鳴者(In the Wild)を削除
  - 2018/05/30 翻訳(Translation)を削除
  - 2018/05/30 他のスタイルガイドたちを移動

