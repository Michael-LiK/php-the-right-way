---
layout: page
title:  The Basics
sitemap: true
---

# 基础

## 比较运算符

比较运算符在PHP中经常容易被忽略,但是它常常会导致一些意想不到的输出结果。下面是一个常见的例子，在使用比较运算符等级，比较整型变量和布尔值。

{% highlight php %}
<?php
$a = 5;   // 5 as an integer

var_dump($a == 5);       // compare value; return true
var_dump($a == '5');     // compare value (ignore type); return true
var_dump($a === 5);      // compare type/value (integer vs. integer); return true
var_dump($a === '5');    // compare type/value (integer vs. string); return false

//Equality comparisons
if (strpos('testing', 'test')) {    // 'test' is found at position 0, which is interpreted as the boolean 'false'
    // code...
}

// vs. strict comparisons
if (strpos('testing', 'test') !== false) {    // true, as strict comparison was made (0 !== false)
    // code...
}
{% endhighlight %}

* [比较运算符](http://php.net/language.operators.comparison)
* [PHP 类型比较表](http://php.net/types.comparisons)
* [比较运算符示例清单](http://phpcheatsheets.com/index.php?page=compare)

## 条件语句

### If 条件判断语句

当我们在函数或类方法中使用 'if/else' 条件判断语句时，存在一个常见的误解,else语句是必须使用的,以保证其他的执行结果得到声明。然而，如果我们的输出结果是去定义返回值，那么else语句就不是必须的，我们可以直接通过return进行返回，使用多余的else语句将变得没有意义。

{% highlight php %}
<?php
function test($a)
{
    if ($a) {
        return true;
    } else {
        return false;
    }
}

// vs.

function test($a)
{
    if ($a) {
        return true;
    }
    return false;    // else is not necessary
}

// or even shorter:

function test($a)
{
    return (bool) $a;
}

{% endhighlight %}

* [If statements](http://php.net/control-structures.if)

### Switch 多重选择语句

Switch多重选择语句是一个非常好的方式去避免使用大量的else/if,else,但是使用时也仍需注意以下几点：

- Switch 多重选择语句只能进行“值”的比较，不能进行类型的比较。 (相当于'==')
- 语句将遍历每一种case(情况)，直到找到匹配值位置.如果没有找到匹配值,将会执行默认的设置 (前提是已设置默认值)
- 进入匹配条件后，如果没有break;(中断退出语句), 将会继续执行匹配，直到找到第一个break;或return;退出方法。
- 在一个函数,使用'return'代替了使用'break'的必要性,因为它结束了当前函数。

{% highlight php %}
<?php
$answer = test(2);    // the code from both 'case 2' and 'case 3' will be implemented

function test($a)
{
    switch ($a) {
        case 1:
            // code...
            break;             // break is used to end the switch statement
        case 2:
            // code...         // with no break, comparison will continue to 'case 3'
        case 3:
            // code...
            return $result;    // within a function, 'return' will end the function
        default:
            // code...
            return $error;
    }
}
{% endhighlight %}

* [Switch statements](http://php.net/control-structures.switch)
* [PHP switch](http://phpswitch.com/)

## 全局命名空间

当我们使用命名空间的时候，你可能想要找到你写过的被隐藏的方法，通过在调用方法前加反斜杠（\），你讲解决这个问题

{% highlight php %}
<?php
namespace phptherightway;

function fopen()
{
    $file = \fopen();    // Our function name is the same as an internal function.
                         // Execute the function from the global space by adding '\'.
}

function array()
{
    $iterator = new \ArrayIterator();    // ArrayIterator is an internal class. Using its name without a backslash
                                         // will attempt to resolve it within your namespace.
}
{% endhighlight %}

* [全局空间](http://php.net/language.namespaces.global)
* [全局规则](http://php.net/userlandnaming.rules)

## 字符串

### 字符串连接符

- 如果你想一行记录超过120个字符的，建议你使用字符串连接符。
- 为了更好地可读性，最好使用字符串连接符而不是赋值运算符。
- 当变量在原始范围内,使用字符串连接符新起一行时要对代码进行缩进。


{% highlight php %}
<?php
$a  = 'Multi-line example';    // concatenating assignment operator (.=)
$a .= "\n";
$a .= 'of what not to do';

// vs

$a = 'Multi-line example'      // concatenation operator (.)
    . "\n"                     // indenting new lines
    . 'of what to do';
{% endhighlight %}

* [String Operators](http://php.net/language.operators.string)

### 字符串类型

字符串是一系列字符，听起来应该很简单。也就是说，有一些不同类型的字符串，它们提供略有不同的语法，行为略有不同。

#### 单引号

Single quotes are used to denote a "literal string". Literal strings do not attempt to parse special characters or
variables.

If using single quotes, you could enter a variable name into a string like so: `'some $thing'`, and you would see the
exact output of `some $thing`. If using double quotes, that would try to evaluate the `$thing` variable name and show
errors if no variable was found.


{% highlight php %}
<?php
echo 'This is my string, look at how pretty it is.';    // no need to parse a simple string

/**
 * Output:
 *
 * This is my string, look at how pretty it is.
 */
{% endhighlight %}

* [Single quote](http://php.net/language.types.string#language.types.string.syntax.single)

#### 双引号

Double quotes are the Swiss Army Knife of strings. They will not only parse variables as mentioned above, but all sorts
of special characters, like `\n` for newline, `\t` for a tab, etc.

{% highlight php %}
<?php
echo 'phptherightway is ' . $adjective . '.'     // a single quotes example that uses multiple concatenating for
    . "\n"                                       // variables and escaped string
    . 'I love learning' . $code . '!';

// vs

echo "phptherightway is $adjective.\n I love learning $code!"  // Instead of multiple concatenating, double quotes
                                                               // enables us to use a parsable string
{% endhighlight %}

Double quotes can contain variables; this is called "interpolation".

{% highlight php %}
<?php
$juice = 'plum';
echo "I like $juice juice";    // Output: I like plum juice
{% endhighlight %}

When using interpolation, it is often the case that the variable will be touching another character. This will result
in some confusion as to what is the name of the variable, and what is a literal character.

To fix this problem, wrap the variable within a pair of curly brackets.

{% highlight php %}
<?php
$juice = 'plum';
echo "I drank some juice made of $juices";    // $juice cannot be parsed

// vs

$juice = 'plum';
echo "I drank some juice made of {$juice}s";    // $juice will be parsed

/**
 * Complex variables will also be parsed within curly brackets
 */

$juice = array('apple', 'orange', 'plum');
echo "I drank some juice made of {$juice[1]}s";   // $juice[1] will be parsed
{% endhighlight %}

* [Double quotes](http://php.net/language.types.string#language.types.string.syntax.double)

#### Nowdoc syntax

Nowdoc syntax was introduced in 5.3 and internally behaves the same way as single quotes except it is suited toward the
use of multi-line strings without the need for concatenating.

{% highlight php %}
<?php
$str = <<<'EOD'             // initialized by <<<
Example of string
spanning multiple lines
using nowdoc syntax.
$a does not parse.
EOD;                        // closing 'EOD' must be on it's own line, and to the left most point

/**
 * Output:
 *
 * Example of string
 * spanning multiple lines
 * using nowdoc syntax.
 * $a does not parse.
 */
{% endhighlight %}

* [Nowdoc syntax](http://php.net/language.types.string#language.types.string.syntax.nowdoc)

#### Heredoc syntax

Heredoc syntax internally behaves the same way as double quotes except it is suited toward the use of multi-line
strings without the need for concatenating.

{% highlight php %}
<?php
$a = 'Variables';

$str = <<<EOD               // initialized by <<<
Example of string
spanning multiple lines
using heredoc syntax.
$a are parsed.
EOD;                        // closing 'EOD' must be on it's own line, and to the left most point

/**
 * Output:
 *
 * Example of string
 * spanning multiple lines
 * using heredoc syntax.
 * Variables are parsed.
 */
{% endhighlight %}

* [Heredoc syntax](http://php.net/language.types.string#language.types.string.syntax.heredoc)

### 哪一种更快?

There is a myth floating around that single quote strings are fractionally quicker than double quote strings. This is
fundamentally not true.

If you are defining a single string and not trying to concatenate values or anything complicated, then either a single
or double quoted string will be entirely identical. Neither are quicker.

If you are concatenating multiple strings of any type, or interpolate values into a double quoted string, then the
results can vary. If you are working with a small number of values, concatenation is minutely faster. With a lot of
values, interpolating is minutely faster.

Regardless of what you are doing with strings, none of the types will ever have any noticeable impact on your
application. Trying to rewrite code to use one or the other is always an exercise in futility, so avoid this micro-
optimization unless you really understand the meaning and impact of the differences.

* [Disproving the Single Quotes Performance Myth](http://nikic.github.io/2012/01/09/Disproving-the-Single-Quotes-Performance-Myth.html)


## 三元运算符 

三元运算符是精简代码的好方法，但也往往存在着过度使用.当三元运算符可 堆叠/嵌套时，建议保持每一行的可读性。

{% highlight php %}
<?php
$a = 5;
echo ($a == 5) ? 'yay' : 'nay';
{% endhighlight %}

相比之下，这里有一个例子，为了缩减代码量而牺牲了所有形式的的代码可读性。

{% highlight php %}
<?php
echo ($a) ? ($a == 5) ? 'yay' : 'nay' : ($b == 10) ? 'excessive' : ':(';    // excess nesting, sacrificing readability
{% endhighlight %}

使用三元运算符的正确语法，来获得返回值。

{% highlight php %}
<?php
$a = 5;
echo ($a == 5) ? return true : return false;    // this example will output an error

// vs

$a = 5;
return ($a == 5) ? 'yay' : 'nope';    // this example will return 'yay'

{% endhighlight %}

有一点需要被提醒，你不需要使用三元运算符来进行布尔值的判断和返回。例子如下：

{% highlight php %}
<?php
$a = 3;
return ($a == 3) ? true : false; // Will return true or false if $a == 3

// vs

$a = 3;
return $a == 3; // Will return true or false if $a == 3

{% endhighlight %}

同理适用于以下运算符(===, !==, !=, == etc).

#### Utilising brackets with ternary operators for form and function

When utilising a ternary operator, brackets can play their part to improve code readability and also to include unions
within blocks of statements. An example of when there is no requirement to use bracketing is:

{% highlight php %}
<?php
$a = 3;
return ($a == 3) ? "yay" : "nope"; // return yay or nope if $a == 3

// vs

$a = 3;
return $a == 3 ? "yay" : "nope"; // return yay or nope if $a == 3
{% endhighlight %}

Bracketing also affords us the capability of creating unions within a statement block where the block will be checked
as a whole. Such as this example below which will return true if both ($a == 3 and $b == 4) are true and $c == 5 is
also true.

{% highlight php %}
<?php
return ($a == 3 && $b == 4) && $c == 5;
{% endhighlight %}

Another example is the snippet below which will return true if ($a != 3 AND $b != 4) OR $c == 5.

{% highlight php %}
<?php
return ($a != 3 && $b != 4) || $c == 5;
{% endhighlight %}

Since PHP 5.3, it is possible to leave out the middle part of the ternary operator.
Expression "expr1 ?: expr3" returns expr1 if expr1 evaluates to TRUE, and expr3 otherwise.

* [Ternary operators](http://php.net/language.operators.comparison)

## Variable declarations

At times, coders attempt to make their code "cleaner" by declaring predefined variables with a different name. What
this does in reality is to double the memory consumption of said script. For the example below, let us say an example
string of text contains 1MB worth of data, by copying the variable you've increased the scripts execution to 2MB.

{% highlight php %}
<?php
$about = 'A very long string of text';    // uses 2MB memory
echo $about;

// vs

echo 'A very long string of text';        // uses 1MB memory
{% endhighlight %}

* [Performance tips](http://web.archive.org/web/20140625191431/https://developers.google.com/speed/articles/optimizing-php)
