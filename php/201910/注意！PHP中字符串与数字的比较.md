# 注意！PHP中字符串与数字的比较

在日常开发过程中，==运算符是我们每天都会接触到的。这个运算符中其实埋了非常多的坑，今天我们就来看下字符串和数字用==比较需要注意的问题。

首先来看看这些代码：

```php

echo '"1234" == " 1234" is ' . ('1234' == ' 1234'), PHP_EOL;
echo '"1234" == "\n1234" is ' . ('1234' == "\n1234"), PHP_EOL;
echo '"1234" == "1234" is ' . ('1234' == '1234'), PHP_EOL;
echo '"1234" == "1234 " is ' . ('1234' == '1234 '), PHP_EOL;
echo '"1234" == "1234\n" is ' . ('1234' == "1234\n"), PHP_EOL;

```

都是字符串的==操作，它们的结果会是什么呢？

```php

"1234" == " 1234" is 1
"1234" == "\n1234" is 1
"1234" == "1234" is 1
"1234" == "1234 " is 
"1234" == "1234\n" is 

```

没错，空格或者制表符号在前的会忽略掉这些符号，也就是说，这些字符串在对比的时候进行了类型转换，都被强转成了int型。而特殊字符在后的，则会按照字符串类型进行比对，那么，纯字符类型呢？

```php

echo '"aa" == " aa" is ' . ('aa' == ' aa'), PHP_EOL;
echo '"aa" == "\naa" is ' . ('a' == '\naa'), PHP_EOL;
echo '"aa" == "aa" is ' . ('aa' == 'aa'), PHP_EOL;
echo '"aa" == "aa " is ' . ('aa' == 'aa '), PHP_EOL;
echo '"aa" == "aa\n" is ' . ('aa' == "aa\n"), PHP_EOL;

```

这时候的结果就符合我们的预期了，他们本身就是字符串的比对，不会进行任何类型的转换：

```php

"aa" == " aa" is 
"aa" == "\naa" is 
"aa" == "aa" is 1
"aa" == "aa " is 
"aa" == "aa\n" is 

```

综上实验结果得知，当字符串的内容都是int数据时，字符串的==比较会忽略在字符串前面出现的空格或者制表符号将它们强制转换成int类型。而只要字符串中包含文本或者特殊符号在数字的后面，就会以文本方式进行比较，如纯文本或者混合文本（"11aa"、"11\n"、"aa11 ")。

测试代码：[https://github.com/zhangyue0503/dev-blog/blob/master/php/201910/source/%E6%B3%A8%E6%84%8F%EF%BC%81PHP%E4%B8%AD%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%8E%E6%95%B0%E5%AD%97%E7%9A%84%E6%AF%94%E8%BE%83.php](https://github.com/zhangyue0503/dev-blog/blob/master/php/201910/source/%E6%B3%A8%E6%84%8F%EF%BC%81PHP%E4%B8%AD%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%8E%E6%95%B0%E5%AD%97%E7%9A%84%E6%AF%94%E8%BE%83.php)

参考链接：[https://www.php.net/manual/zh/language.operators.comparison.php](https://www.php.net/manual/zh/language.operators.comparison.php)