---
title: discuz 帖子评论
date: 2018-08-30 10:24:05
tags:
---


#### discuz帖子评论 整个流程涉及到discuz后台的控制,各种条件判断比较多,暂时先梳理一下涉及到的流程

1.我是在ajax_littleprocedure.php项目中直接引用的h5页面,否则逻辑太多,很难弄清楚流程,这此文件中,初始化discuz一些参数,否则无法引用forum_post.php,

```
require_once './source/class/class_core.php';
require_once './source/function/function_forum.php';
global $_G;
C::app()->cachelist = $cachelist;
C::app()->init();
$_G['uid'] = C::app()->var['member']['uid'] = intval($_POST['uid']);
$_G['username'] = C::app()->var['member']['username'] = trim($_POST['username']);

loadforum();
set_rssauth();
$_GET['action'] = 'reply';
$_G['group']['allowreply'] = $access['allowreply'];

$_G['check_version'] = true;

$_G['setting']['threadhidethreshold'] = 1;
//帖子评论 刷新缓存
$_G['form_wechat_cache']=1;
require_once './source/module/forum/forum_post.php';

```

2.discuz底层的forum_post.php这里面主要是调用 post_newreply.php进行写表操作

```
source\module\forum\forum_post.php
if($_GET['action'] == 'reply') {

    ...............
    
  require_once libfile('post/newreply', 'include');  
    
}


```

3.post_newreply.php这里的写表操作之前,要对是否允许发帖做了验证
```
source\include\post\post_newreply.php
```

调用newreplay方法进行写表,而这个方法在source\class\model\model_forum_post.php中
```
$return = $modpost->newreply($params);

```

4.在调用newreplay方法 其中还涉及对invisible字段进行验证
```
if(strpos($this->member['extgroupids'],"42")===false){
$res = TyLib_Antispam_Text::main($data);
if($res['code']!=200){
    $yundun=$res['msg'];
    list(, $this->param['modnewreplies']) =array("",1);
}else{
    $yundun="";
    list(, $this->param['modnewreplies']) = threadmodstatus($this->param['subject']."\t".$this->param['message'].$this->param['extramessage']);
}
}else{
$yundun="";
list(, $this->param['modnewreplies']) = threadmodstatus($this->param['subject']."\t".$this->param['message'].$this->param['extramessage']);

}

```
```
Discuz pre_forum_post invisible字段
invisible ：0 正常
invisible ：-3  已忽略
invisible ：-2  待审核
invisible ：-5  回收站
invisible : -1 主题帖在回收站中

```

5.这个invisible字段的验证,还涉及到

```
source\class\discuz\discuz_model.php

source\function\function_forum.php
```

中的方法,比如$_G['group']['allowdirectpost']是否允许直接发帖,这个是在discuz后台中设置的

```
function threadmodstatus($string) {
	global $_G;
	$postmodperiods = periodscheck('postmodperiods', 0);
	if($postmodperiods) {
		$modnewthreads = $modnewreplies = 1;
	} else {
		$censormod = censormod($string);
		$modnewthreads = (!$_G['group']['allowdirectpost'] || $_G['group']['allowdirectpost'] == 1) && $_G['forum']['modnewposts'] || $censormod ? 1 : 0;
		$modnewreplies = (!$_G['group']['allowdirectpost'] || $_G['group']['allowdirectpost'] == 2) && $_G['forum']['modnewposts'] == 2 || $censormod ? 1 : 0;

		if($_G['forum']['status'] == 3) {
			$modnewthreads = !$_G['group']['allowgroupdirectpost'] || $_G['group']['allowgroupdirectpost'] == 1 || $censormod ? 1 : 0;
			$modnewreplies = !$_G['group']['allowgroupdirectpost'] || $_G['group']['allowgroupdirectpost'] == 2 || $censormod ? 1 : 0;
		}
	}

	$_G['group']['allowposturl'] = $_G['forum']['status'] != 3 ? $_G['group']['allowposturl'] : $_G['group']['allowgroupposturl'];
	if($_G['group']['allowposturl'] == 1) {
		if(!$postmodperiods) {
			$censormod = censormod($string);
		}
		if($censormod) {
			$modnewthreads = $modnewreplies = 1;
		}
	}
	return array($modnewthreads, $modnewreplies);
}

```
