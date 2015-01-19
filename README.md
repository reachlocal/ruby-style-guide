# The Ruby Style Guide

This Ruby style guide outlines our recommended best practices so that our
developers can write code that can be maintained by other (future) developers.
This is meant to be a style guide that reflects real-world usage, as well as a
guide that holds to an ideal that has been agreed upon by many of the people
it was intended to be used by.

## How to Read This Guide

The guide is separated into sections based on the different places ruby code
is generally written. There was an attempt to omit all obvious information,
so if anything is unclear feel free to open an issue asking for further
clarity.

## A Living Document

Per the comment above, this guide is a work in progress - some rules are
simply lacking thorough examples, but some things in the Ruby world change
week by week or month by month. With that said, as the standard changes this
guide is meant to be able to change with it.

## Formatting

* <a name="utf-8"></a>
  Use `UTF-8` as the source file encoding.
<sup>[[link](#utf-8)]</sup>

* <a name="spaces-indentation"></a>
  Use two **spaces** per indentation level (aka soft tabs). No hard tabs.
<sup>[[link](#spaces-indentation)]</sup>

  ```Ruby
  # bad - four spaces
  def some_method
      do_something
  end

  # good
  def some_method
    do_something
  end
  ```

* <a name="crlf"></a>
  Use Unix-style line endings. (\*BSD/Solaris/Linux/OS X users are covered by
  default, Windows users have to be extra careful.)
<sup>[[link](#crlf)]</sup>

  * <a name="end-each-file-with-a-blank-newline"></a>
    [End each file with a blank newline](http://stackoverflow.com/questions/729692/why-should-files-end-with-a-newline)
    <sup>[[link](#end-each-file-with-a-blank-newline)]</sup>

  * If you're using Git you might want to add the following
    configuration setting to protect your project from Windows line
    endings creeping in:

    ```bash
    $ git config --global core.autocrlf true
    ```

* <a name="spaces-around-operators"></a>
  Use spaces around operators, after commas, colons and semicolons, around
  `{` and before `}`.
<sup>[[link](#spaces-around-operators)]</sup>

  ```Ruby
  # bad
  some_method(arg1,arg2,arg3) # No spaces after comma

  some_method do {|a| puts a} # No spaces inside `{` and `}`

  # good
  some_method(arg1, arg2, arg3)

  some_method do { |a| puts a }
  ```

* <a name="no-spaces-inside-parenthesis"></a>
  No spaces after `(`, `[` and before `]`, `)`.
<sup>[[link](#no-spaces-inside-parenthesis)]</sup>

  ```Ruby
  # bad
  some_method( arg1, arg2 ) # Spaces inside parenthesis

  a = [ "item1", "item2" ] # Spaces inside hard brackets

  # good
  some_method(arg1, arg2) # No spaces inside parenthesis

  a = ["item1", "item2"] # No spaces inside hard brackets
  ```

* <a name="no-semicolon"></a>
  Don't use `;` to separate statements and expressions. As a corollary - use one
  expression per line.
<sup>[[link](#no-semicolon)]</sup>

  ```Ruby
  # bad
  puts 'foobar'; # superfluous semicolon

  puts 'foo'; puts 'bar' # two expressions on the same line

  # good
  puts 'foobar'

  puts 'foo'
  puts 'bar'

  puts 'foo', 'bar' # this applies to puts in particular
  ```

* <a name="single-line-classes"></a>
  Prefer a single-line format for class definitions with no body.
<sup>[[link](#single-line-classes)]</sup>

  ```Ruby
  # bad
  class FooError < StandardError
  end

  # okish
  class FooError < StandardError; end

  # good
  FooError = Class.new(StandardError)
  ```

* <a name="no-single-line-methods"></a>
  Avoid single-line methods. Although they are somewhat popular in the wild,
  there are a few peculiarities about their definition syntax that make their
  use undesirable. At any rate - there should be no more than one expression in
  a single-line method.
<sup>[[link](#no-single-line-methods)]</sup>

  ```Ruby
  # bad
  def too_much; something; something_else; end

  # okish - notice that the first ; is required
  def no_braces_method; body end

  # okish - notice that the second ; is optional
  def no_braces_method; body; end

  # okish - valid syntax, but no ; makes it kind of hard to read
  def some_method() body end

  # good
  def some_method
    body
  end
  ```

  One exception to the rule are empty-body methods.

  ```Ruby
  # good
  def no_op; end
  ```

* <a name="case-statement-indention"></a>
  Case statements conditions should be the same level as the case.
<sup>[[link](#case-statement-indention)]</sup>

  ```Ruby
  case a
  when 1..5
    puts "It's between 1 and 5"
  when 6
    puts "It's 6"
  when String
    puts "You passed a string"
  else
    puts "You gave me #{a} -- I have no idea what to do with that."
  end
  ```

* <a name="empty-line-before-return"></a>
  Use an empty line before the return value of a method (unless it only has
  one line).
<sup>[[link](#empty-line-before-return)]</sup>

  ```Ruby
  # bad
  def something
    a = do_something
    a.call_a_method
    return a
  end

  # good
  def something
    a = do_something
    a.call_a_method

    return a
  end
  ```

* <a name="empty-line-betweed-defs"></a>
  Use an empty line between method definitions.
<sup>[[link](#empty-line-betweed-defs)]</sup>

  ```Ruby
  # bad
  def something
    call_a_method
  end
  def another
    call_another_method
  end
  def third
    call_a_method
  end

  # good
  def something
    call_a_method
  end

  def another
    call_another_method
  end

  def third
    call_a_method
  end
  ```

* <a name="do-not-align-operators-like-equals"></a>
  Do not align operators like equals(`=`).
<sup>[[link](#do-not-align-operators-like-equals)]</sup>

  ```Ruby
  # bad
  def something
    foo     = bar
    humdrum = an_operation
    pi      = 3.14159265359
    show    = "Firefly"
  end

  # better
  def something
    foo = value
    humdrum = an_operation
    pi = 3.14159265359
    show = "Firefly"
  end
  ```
  NOTE: There's a good chance that too many assignments could be a smell for another
problem. A known exception to this rule is in a Gems gemspec file.

* <a name="rdoc-style-api-documentation"></a>
  Use RDoc and its conventions for API documentation. Don't put an empty line
  between the comment block and the def.
<sup>[[link](#rdoc-style-api-documentation)]</sup>

* <a name="empty-lines-in-long-methods"></a>
  Use empty lines to break up a long method into logical paragraphs.
<sup>[[link](#empty-lines-in-long-methods)]</sup>

* <a name="fewer-than-80-characters"></a>
  Keep lines fewer than 80 characters.
<sup>[[link](#fewer-than-80-characters)]</sup>

* <a name="no-double-indent"></a>
    Align the parameters of a method call if they span more than one
    line. When aligning parameters is not appropriate due to line-length
    constraints, single indent for the lines after the first is also
    acceptable.
<sup>[[link](#no-double-indent)]</sup>

  ```Ruby
  # starting point (line is too long)
  def send_mail(source)
    Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
  end

  # bad (double indent)
  def send_mail(source)
    Mailer.deliver(
        to: 'bob@example.com',
        from: 'us@example.com',
        subject: 'Important message',
        body: source.text)
  end

  # good
  def send_mail(source)
    Mailer.deliver(to: 'bob@example.com',
                   from: 'us@example.com',
                   subject: 'Important message',
                   body: source.text)
  end

  # good (normal indent)
  def send_mail(source)
    Mailer.deliver(
      to: 'bob@example.com',
      from: 'us@example.com',
      subject: 'Important message',
      body: source.text
    )
  end
  ```

* <a name="avoid-trailing-whitespace"></a>
  Avoid trailing whitespace.
<sup>[[link](#avoid-trailing-whitespace)]</sup>

* <a name="anchor"></a>
  Use def with parentheses *only* when there are arguments.
<sup>[[link](#anchor)]</sup>

  ```Ruby
  # bad
  def some_method()
    do_something
  end

  # good (when there are no arguments)
  def some_method
    do_something
  end

  # good (when there are arguments)
  def some_method(arg1, arg2)
    do_something
  end
  ```

* <a name="never-use-for"></a>
  Never use `for`, unless you exactly know why.
<sup>[[link](#never-use-for)]</sup>

* <a name="never-use-then"></a>
  Never use `then`.
<sup>[[link](#never-use-then)]</sup>

* <a name="and-and-or"></a>
  Use `&&`/`||` for boolean expressions, `and`/`or` for control flow. (Rule
  of thumb: If you have to use outer parentheses, you are using the wrong
  operators.)
<sup>[[link](#and-and-or)]</sup>

* <a name="multi-line-ternary"></a>
  Avoid multiline ternary operators (`?:`), use `if` instead.
<sup>[[link](#multi-line-ternary)]</sup>

  ```Ruby
  # bad
  some_variable ?
    do_positive :
    do_negative

  # good - multi-line if
  if some_variable
    do_positive
  else
    do_negative
  end

  # also good - single-line ternary
  some_variable ? do_positive : do_negative
  ```

* <a name="no-nested-ternary"></a>
  Do not nest ternary operators.
<sup>[[link](#no-nested-ternary)]</sup>

* <a name="avoid-simple-iterators"></a>
  Avoid simple iterator blocks unless something complicated is able to be
  avoided.
<sup>[[link](#avoid-simple-iterators)]</sup>

  ```Ruby

  # bad
  [klass1, klass2, klass3].each { |klass| klass.new.ping }

  # good
  klass1.new.ping
  klass2.new.ping
  klass3.new.ping

  # bad
  foo = klass1.new()
  foo.bar
  foo.tar
  foo.car
  hoo = klass2.new()
  hoo.bar
  hoo.tar
  hoo.car
  tel = klass3.new()
  tel.bar
  tel.tar
  tel.car

  # good
  [klass1, klass2, klass3].each do |klass|
    foo = klass.new
    foo.bar
    foo.tar
    foo.car
  end
  ```


* <a name="suppress-superfluous-parenthesis"></a>
  Suppress superfluous parentheses when calling methods, but keep them when
  calling "functions", i.e. when you use the return value in the same line.
<sup>[[link](#suppress-superfluous-parenthesis)]</sup>

  ```Ruby
  # bad
  x = Math.sin y
  array.delete()

  # good
  x = Math.sin(y)
  array.delete
  ```

* <a name="do-end-for-multi-line-blocks"></a>
  Use `do`/`end` for multi-line blocks.
<sup>[[link](#do-end-for-multi-line-blocks)]</sup>

* <a name="curlies-for-single-line-blocks"></a>
  Use `{` and `}` for single line blocks.
<sup>[[link](#curlies-for-single-line-blocks)]</sup>

* <a name="call-return-in-methods"></a>
  Explicitly call `return` in method definitions.
<sup>[[link](#call-return-in-methods)]</sup>

* <a name="non-required-line-continuation"></a>
  Avoid line continuation where not required.
<sup>[[link](#non-required-line-continuation)]</sup>

  ```Ruby
  # bad
  if foo == bar &&\
    baz == booze
    do_something
  end

  # good
  if foo == bar && baz == booze
    do_something
  end
  ```

* <a name="equals-return-value"></a>
  Using the return value of `=` is NOT okay.
<sup>[[link](#equals-return-value)]</sup>

  ```Ruby
  # bad
  if grepped = array.grep(/foo/)
    do_something
  end

  # good
  grepped = array.grep(/foo/)
  if grepped
    do_something
  end
  ```

* <a name="or-equals"></a>
  Use `||=` freely.
<sup>[[link](#or-equals)]</sup>

* <a name="regex-expressions"></a>
  Freely use the following when utilizing regular expressions.
<sup>[[link](#regex-expressions)]</sup>

  ```Ruby
  =~
  $0-9
  $~
  $`
  $'
  ```

* <a name="oo-regex"></a>
  Avoid OO regexps (they won't make the code better).
<sup>[[link](#oo-regex)]</sup>

* <a name="explicitly-access-instance-methods"></a>
  Prefer explicitly accessing instance methods with self.
<sup>[[link](#explicitly-access-instance-methods)]</sup>

  ```Ruby
  # bad
  class Person
    def some_method
      an_instance_method
    end

    def an_instance_method
      do_something
    end
  end

  # good
  class Person
    def some_method
      self.an_instance_method
    end

    def an_instance_method
      do_something
    end
  end
  ```

* <a name="explicitly-access-instance-vars"></a>
  Within models (or model-like classes) you should always access attributes
  via their instance variable instead of getter/setters.
<sup>[[link](#explicitly-access-instance-vars)]</sup>

  ```ruby
  # bad
  class Person
    attr_accessor :name

    def greeting
      "Hello, #{self.name}"
    end
  end

  # good
  class Person
    attr_accessor :name

    def greeting
      "Hello, #{@name}"
    end
  end
  ```

* <a name="interpolation-over-concatenation"></a>
  Always favor string interpolation over concatenation.
<sup>[[link](#interpolation-over-concatenation)]</sup>

  ```ruby
  # bad
  full_name = first_name + ', ' + last_name

  # good
  full_name = "#{first_name}, #{last_name}"
  ```

* <a name="use-correct-string-type"></a>
  Use single quotes as a default for string. Use double quotes when there is
  interpolation and either double quotes OR single quotes, or there are
  special characters that need to be escaped. Use [percent notation](http://en.wikibooks.org/wiki/Ruby_Programming/Syntax/Literals#The_.25_Notation) when
  there is interpolation, single quotes, AND double quotes.

  ```ruby
  # bad
  value1 = "test"
  value2 = %(isn't this the best)
  value3 = %(isn't this the #{description})
  value4 = "\"She isn't my #{noun}\", he said."
  value5 = 'hello\nworld'

  # good
  value1 = 'test'
  value2 = "isn't is the best"
  value3 = "isn't this the #{description}"
  value4 = %("She isn't my #{noun}", he said.)
  value5 = "hello\nworld"
  ```

## Naming
  * <a name="lower-snake-case"></a>
    Use lower case snake_case for methods.
  <sup>[[link](#lower-snake-case)]</sup>

  * <a name="camel-case-classes"></a>
    Use CamelCase for classes and modules. (Keep acronyms like HTTP, RFC, XML uppercase.)
  <sup>[[link](#camel-case-classes)]</sup>

  * <a name="screaming-snake-case"></a>
    Use SCREAMING_SNAKE_CASE for other constants.
  <sup>[[link](#screaming-snake-case)]</sup>

  * <a name="underscore-unused-vars"></a>
    Use `_` or names prefixed with `_` for unused variables.
  <sup>[[link](#underscore-unused-vars)]</sup>

## Comments
  * <a name="long-comments-punctuation"></a>
    Comments longer than a word are capitalized and use punctuation.
  <sup>[[link](#long-comments-punctuation)]</sup>

  * <a name="avoid-superfluous-comments"></a>
    Avoid superfluous comments.
  <sup>[[link](#avoid-superfluous-comments)]</sup>

## The Rest
  * <a name="ruby-w-safe-code">
    </a>Write ruby -w safe code.
  <sup>[[link](#ruby-w-safe-code)]</sup>

  * <a name="avoid-hashes-as-params">
    </a>Avoid hashes-as-optional-parameters. Does the method do too much?
  <sup>[[link](#avoid-hashes-as-params)]</sup>

  * <a name="avoid-long-methods">
    </a>Avoid long methods.
  <sup>[[link](#avoid-long-methods)]</sup>

  * <a name="avoid-long-parameter-lists">
    </a>Avoid long parameter/argument lists.
  <sup>[[link](#avoid-long-parameter-lists)]</sup>

  * <a name="use-self-method-definitions">
    </a>Use `def self.method` to define class level methods.
  <sup>[[link](#use-self-method-definitions)]</sup>

  * <a name="avoid-alias">
    </a>Avoid alias when alias_method will do.
  <sup>[[link](#avoid-alias)]</sup>

  * <a name="avoid-metaprogramming">
    </a>Avoid needless meta-programming, it just adds unneeded confusion.
  <sup>[[link](#avoid-metaprogramming)]</sup>

## DON'T BE CLEVER
## When in doubt, use the conventions

# Contributing

Open tickets or send pull requests with improvements. Please write [good commit messages](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html) or your pull requests will be closed.

# Credit
Inspiration and some examples were taken from the following:

[bbatsov's ruby style guide](https://github.com/bbatsov/ruby-style-guide)
