---
title: defined安全判断
date: 2018-04-25 21:41:13
tags:
---

## 一般用于安全判断，放于被包含脚本中

- 判断：如果没有定义IN_PISCES常量，则直接退出脚本，并提示ACCESS DENIED文字

- 举例：

#### a.php
```
<?php
define('IN_PISCES',1);
include 'b.php';
echo 'a';
?>
```

#### b.php

```
<?php
if(!defined('IN_PISCES')){
    exit('非法访问');
}
```

#### echo 'b';

#### 这时候，你直接访问b.php是会提示非法访问的，只有访问a.php才能输出"b"。