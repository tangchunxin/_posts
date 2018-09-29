---
title: PC帖子详情变量
date: 2018-09-29 17:34:25
tags:
---
## 涉及的id

- $_G uid 用户id
- $_G fid 当前帖子属于的板块id
- $_G tid 帖子主题id
- $_G pid 帖子id
## 发帖按钮下拉菜单

### $_G group 会员用户组权限表

- allowpost 允许发帖
- allowposttrade 允许发表交易
- allowpostpoll 允许发表投票
- allowpostreward 允许发表悬赏
- allowpostactivity 允许发表活动
- allowpostdebate 允许发表辩论
- allowvote 允许投票
- allowvotepolled 已投过
- allowvotethread 该投票已经关闭或者过期，不能投票
- allwvoteusergroup 如果等于 0 所在的用户组没有投票权限
- allowrecommend 允许推荐
- allowgetattach 允许下载附件
- allowgetimage 允许查看图片
- raterange 当前用户组是否允许参与评分，及各设置
- allowthreadplugin 允许发布扩展特殊主题

## 帖子及评论部分

###  post 文章及评论列表

- $postlist_first[groupicon]：楼主用户组图标
- $postlist_first['medals']：楼主勋章
- $_G['medal_list']：楼主勋章弹窗信息列表 勋章名称 介绍
- $numbercard：楼主的积分、天眼币、主题、回帖
- $near_threads：楼主最新帖子
- $is_sign：是否签到 1签了 0没有签
- $showtime：当前日期（7.11）
- $showdata： 星期几
- $month_sign_num：累计签到天数
- $solrThread：相关帖子
- $latest_threads：最新帖子
- $solrArticle：相关文章
- $solrWd：相关问答
- $domain_body：平台关键字
- $platform_name：平台名称
- authorid 作者id
- username 当前登录用户用户名
- anonymous 是否匿名 默认0
- usesig 是否为移动端 1 为 PC
- author 层主/楼主昵称
- avatar 层主/楼主头像
- authortitle 评论用户组文字
- first 帖子
- warned 受到警告
- isstick 置顶评论
- rewardfloor 奖励楼层
- groupid 用户组id
- $needhiddenreply 是否需要隐藏回复
- memberstatus 判断用户是否被删除
- status 帖子状态
- deleted 帖子(评论)被删除
- number 楼层号
- postreview 回帖投票
- verifyicon 用户认证设置的信息
- allowcomment 允许评论
- invisible 0 正常 -3 已忽略 -2 待审核 -5 回收站 -1 主题帖在回收站中
- signature 签名信息
- adminid 管理组ID
- tags 帖子TAG列表
- imagelist 图片附件数组
- imagelistcount 图片附件数量
- ratelog 缓存的评分

## $_G setting 设置

- close_leftinfo_userctrl 是否显示 收起左侧用户信息栏按钮
- close_leftinfo 当前开启还是关闭 关闭 1 打开 0
- guesttipsinthread 游客在浏览主题时 在主题的顶部显示提示问题
- bannedmessages 隐藏敏感帖子内容:1帖子内容2用户头像4用户签名
- recommendthread 主题评价相关设置内容
- repliesrank 启用回帖投票，1是0否
- filterednovote 水帖不能参与回帖投票，1是0否
- imagelistthumb 帖内图片列表中图片横排显示条件，0或空为关闭横排显示
- commentnumber 开启帖子点评后，帖子中显示点评的条目数
- visitedforums 版块列表和帖子浏览中最近访问过的版块

## 文章标题

- $_G forum_threadstamp 图章
- $vid 认证序号对应的id
- $pluginhooks 插件
- threadmaxpages 帖子最大页数
- $_G forum_thread 主题表
- typeid 主题分类id
- sortid 分类信息id
- subject 标题
- displayorder 显示顺序
- authorid 作者id
- recommendlevel 推荐等级
- heatlevel 热度等级
- closed 是否关闭
- hidden 是否显示隐藏标签
- threadhidethreshold 设置是否允许帖子中使用 [hide] 隐藏标签
- replycredit 回帖获得积分记录
- replycredit_rule 回帖获得积分规则
- views 浏览量
- allreplies 评论量
- remaintime 剩余时间
- special 特殊主题,1:投票;2:商品;3:悬赏;4:活动;5:辩论贴;127:插件相关
- replies 回复次数
- archiveid 主题分区id
- is_archived 是否存在 主题分区id true false

## $_G forum 板块表

###  threadtypes 主题分类

- threadsorts 分类信息 板块不同下拉菜单不通
- status 显示状态 (0:隐藏 1:正常 3:群组)
- ismoderator 当前登录用户是否为版主
- disablecollect 是否禁用淘帖 1 禁用 0 不禁用
- picstyle 帖子所属版块 是否开启图片列表 1 开启 0 未开启(生效在旧版帖子列表页面 )
- alloweditpost 板块是否允许编辑帖子

## 特殊类型帖子

###  辩论帖子
- $debate

- endtime 结束时间 等于0 代表没填写
- umpire 裁判
- username 当前登录的用户名
- dbendtime 辩论结束时间
- umpirepoint 裁判观点
- affirmvotes 正方支持人数
- affirmpoint 正方观点
- affirmvoteswidth 正方支持百分比
- affirmdebaters 正方辩手数量
- affirmavatars 正方辩手头像信息
- negavotes 反方支持人数
- negapoint 反方观点
- negavoteswidth 反方支持百分比
- negadebaters 反方辩手数量
- negaavatars 反向辩手头像信息
- winner 获胜方 (0:平局 1:为正方 2:为反方) 裁判评判结果
- bestdebater 最佳辩手用户名

###  辩论帖子表 debate $post

- stand 立场 (0:中立 1:正方 2:为反方)
- uid 发起人id
- pid 帖子id
- tid 主题id
- dateline 发表的时间
- voters 投票人数
- voterids 投票人的 id 集合
- releatcollectionmore 收入该淘帖主题的专辑列表
- sourcecollection 访问来源：您是从淘专辑xxx访问到本帖的

###  投票帖子

单独变量

- $expiration 过期时间
- $multiple 非0 为多选
- $maxchoices 最多可选
- $visiblepoll 投票是否可见
- $overt 是否公开参与人 0 不公开 1 公开
- $isimagepoll 是否为图片投票
- aid 图片附件id
- poid 选项id
- filename 原文件名
- filesize 文件大小
- attachment 附件,0无附件 1普通附件 2有图片附件
- width 附件宽度
- dateline 上传时间
- percent 投票分布百分比
- votes 当前投票
- $optiontype checkbox 时为多选
- $hiddenreplies 回帖是否仅楼主可见
- $rushreply 是否为抢楼帖子

单独变量

- $userRewardList 打赏用户列表
- $allowpostreply 允许回复
- $multipage 分页
- $postshowavatars 是否显示头像
- $showavatars 是否显示作者头像，1是0否，后台设置
- $threadplughtml 其他特殊帖子的输出
- $showsignatures 是否显示作者签名，1是0否
- $alloweditpost_status 当前帖子类型是否允许用户随时编辑 1是0否
- $_G['inajax'] 当前ajax请求的值【无-0 有-1】
- $_G['adminid'] 当前登录ID管理组ID
- $_G['member'] Array 当前登录用户个人信息
- $_G['cookie'] 客户端cookie
- $_GET['highlight'] 搜索词

### 楼中楼

- 楼中楼模板： source/plugin/dxksst_floor/template/list_newvesion.htm
- 楼中楼回复： source/plugin/dxksst_floor/template/ajax.htm
- 楼中楼翻页： source/plugin/dxksst_floor/template/page.htm

- have == 1 : 没有回复 , $have == 2 有回复
- $upid 楼中楼所在楼层的 id

###  楼中楼的楼层循环：

- $v[message] 回复内容
- $v[dateline] 回复时间
- $v['pid'] 楼层用户ID
- $v[username] 楼层用户名
- $del==1
- $v[self]
- $smiley 楼中楼表情
- $sk 表情组
- $sv 表情图片名
2.快速回复

快速回复所在模板 template/131120/forum/viewthread_newversion_article.htm
快速回复DZ后台开关 界面 --> 界面设置 --> 帖子内容页 --> 开启帖子快捷回复
附件QQ登录微信登录
模板：source/plugin/qqconnect/template/module.htm
相关模板及引用
帖子部分(帖子、评论)
|——/template/131120/forum/viewthread_newversion_index.htm
　　　|——template/131120/forum/viewthread_newversion_article.htm
　　　　|——/template/131120/forum/viewthread_newversion_discuss_art.htm 帖子内容
　　　　　|——template/131120/forum/viewthread_newversion_nodebody.htm 特殊帖子
　　　　　　|——template/131120/forum/viewthread_newversion_poll.htm 投票
　　　　　　|——template/131120/forum/viewthread_newversion_debate.htm pk帖子
　　　　|——/template/131120/forum/viewthread_newversion_discuss.htm 评论内容
　　　　　|——template/131120/forum/viewthread_newversion_nodebody.htm 特殊帖子
　　　　|——/template/131120/forum/viewthread_newversion_fastpost.htm 底部回复
　　　|——/template/131120/forum/viewthread_newversion_related.htm 相关文章 相关问答
　　　|——/template/131120/forum/viewthread_newversion_floorhost.htm 楼主信息 签到奖励
　　　|——/template/131120/forum/viewthread_newversion_sidelist.htm 相关帖子 最新帖子

