---
title: PHP 生成一定数量的不重复随机数
author: James Shih
layout: post
permalink: /2011/06/07/php-unique-random-numbers/
tagazine-media:
  - 'a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:8:"13298777";s:7:"blog_id";s:8:"12911913";s:9:"mod_stamp";s:19:"2011-06-07 06:55:30";}'
jabber_published:
  - 1307429730
categories:
  - PHP
  - Web 开发
tags:
  - PHP
  - 随机数
---
将随机数存入数组，再在数组中去除重复的值，即可生成一定数量的不重复随机数。

```php
/*
* array unique_rand( int $min, int $max, int $num )
* 生成一定数量的不重复随机数
* $min 和 $max: 指定随机数的范围
* $num: 指定生成数量
*/
function unique_rand($min, $max, $num) {
    $count = 0;
    $return = array();
    while ($count < $num) {
        $return[] = mt_rand($min, $max);
        $return = array_flip(array_flip($return));
        $count = count($return);
    }
    shuffle($return);
    return $return;
}
```

生成随机数时用了 <a href="http://docs.php.net/manual/zh/function.mt-rand.php" target="_blank">mt_rand()</a> 函数。这个函数生成随机数的平均速度要比 <a href="http://docs.php.net/manual/zh/function.rand.php" target="_blank">rand()</a> 快四倍。

去除数组中的重复值时用了“翻翻法”，就是用 <a href="http://docs.php.net/manual/zh/function.array-flip.php" target="_blank">array_flip()</a> 把数组的 key 和 value 交换两次。这种做法比用 <a href="http://docs.php.net/manual/zh/function.array-unique.php" target="_blank">array_unique()</a> 快得多。

返回数组前，先使用 <a href="http://cn2.php.net/manual/zh/function.shuffle.php" target="_blank">shuffle()</a> 为数组赋予新的键名，保证键名是 0-n 连续的数字。如果不进行此步骤，可能在删除重复值时造成键名不连续，给遍历带来麻烦。

**Update(03/20/2012):** 添加了 shuffle() 步骤