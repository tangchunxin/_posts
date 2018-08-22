---
title: php-iconv
date: 2018-04-23 22:28:43
tags:
---

## PHP mb_convert_encoding()函数 与 iconv 
函数 为php内部多字节字符串编码转换函数，可以在有需要的场合进行编码转换，如：解决 
在GB2312编码环境下使用Ajax产生的中文 字符乱码 问题。支持几乎所有编码，版本支持 PHP 4 >= 4.0.6、PHP 5。
mb_开头的字符串函数需要PHP开启 对应的扩展mb_string => [开启方法](https://blog.csdn.net/yumenshizhongjingjie/article/details/50569889)

#### 语法：
```
mb_convert_encoding ( string str, string to_encoding [, mixed from_encoding] )
```
- string str 需要进行编码转换的字符串; 
-  string to_encoding 指定转换为某种编码，如：gb2312、gbk、utf-8等;

#### 1、把 GBK 编码字串转换成 UTF-8 编码字串; 
Php代码 收藏代码
```
<?php  
header("content-Type: text/html; charset=Utf-8");  
echo mb_convert_encoding("你是我的好朋友", "UTF-8", "GBK");  
?>
```

#### 2、把 UTF-8 编码字串转换成 GB2312 编码字串 
Php代码 收藏代码 
```
// 注意将此文件存盘成 utf-8 编码格式文件再测试

<?php  
header("content-Type: text/html; charset=gb2312");  
echo mb_convert_encoding("你是我的好朋友", "gb312", "utf-8");  
```
## iconv — 字符串按要求的字符编码来转换
#### 说明
string iconv ( string $in_charset , string $out_charset , string $str )
将字符串 str 从 in_charset 转换编码到 out_charset。

#### 参数
#### in_charset
- 输入的字符集。

#### out_charset
- 输出的字符集。

```
如果你在 out_charset 后添加了字符串 //TRANSLIT，将启用转写（transliteration）功能。
这个意思是，当一个字符不能被目标字符集所表示时，它可以通过一个或多个形似的字符来近似表达。
如果你添加了字符串 //IGNORE，不能以目标字符集表达的字符将被默默丢弃。 
否则，会导致一个 E_NOTICE并返回 FALSE。

Caution
//TRANSLIT 运行细节高度依赖于系统的 iconv() 实现（参见 ICONV_IMPL）。
据悉，某些系统上的实现会直接忽略 //TRANSLIT，
所以转换也有可能失败，out_charset 会是不合格的。
```
#### str
- 要转换的字符串。

#### 例子
```
<?php
$text = "This is the Euro symbol '€'.";

echo 'Original : ', $text, PHP_EOL;
echo 'TRANSLIT : ', iconv("UTF-8", "ISO-8859-1//TRANSLIT", $text), PHP_EOL;
echo 'IGNORE   : ', iconv("UTF-8", "ISO-8859-1//IGNORE", $text), PHP_EOL;
echo 'Plain    : ', iconv("UTF-8", "ISO-8859-1", $text), PHP_EOL;
```

#### 数据转码
    /**
	 * 数据转码
	 * @param unknown $str
	 * @param unknown $code 目标编码
	 * @return unknown
	 */
	public function conversion_coding($str, $code){
		$encode = mb_detect_encoding($str, array('GB2312','GBK','UTF-8','BIG5','ASCII'));
		if($encode == $code or !$encode){
			return $str;
		}
		if(!is_string($str)){
			return $str;
		}
		if($encode === 'GB2312' or $encode === 'GBK'){
			$encode = $encode . '//IGNORE';
		}
		return iconv($encode, $code, $str);
	}
	
#### mb_detect_encoding

   当在php中使用mb_detect_encoding函数进行编码识别时，很多人都碰到过识别编码有误的问题，例如对与GB2312和UTF- 8，或者UTF-8和GBK(这里主要是对于cp936的判断),网上说是由于字符短是，mb_detect_encoding会出现误判。 
例如: 

复制代码 代码如下:

```
$encode = mb_detect_encoding($keytitle, array("ASCII",'UTF-8′,"GB2312′,"GBK",'BIG5′)); 
if ($encode == “UTF-8″){ 
$keytitle = iconv("UTF-8″,"GBK",$keytitle); 
} 
```
这段代码的作用是检测字符串的编码是否UTF-8，是的话就转换为GBK。 
可是当 $keytitle = “%D0%BE%C6%AC”;时。检测结果却是UTF-8.这个bug其实不算是bug，写程序时也不应当过于依赖mb_detect_encoding，当字符串较短时，检测结果产生偏差的可能性很大。 
怎么解决呢，我的办法是： 
复制代码 代码如下:

```
$encode = mb_detect_encoding($keytitle, array('ASCII','GB2312′,'GBK','UTF-8'); 
```

三个参数分别是：被检测的输入变量、编码方式的检测顺序(一旦为真，后面自动忽略)、strict模式 
对编码检测的顺序进行调整，将最大可能性放在前面，这样减少被错误转换的机会。 
一般要先排gb2312，当有GBK和UTF-8时，需要将常用的排列到前面。
