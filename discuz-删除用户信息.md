---
title: discuz 删除用户信息
date: 2018-08-24 12:23:01
tags:
---
#### 删除用户手机号码,使得用户能再次注册

- uc用户表:t_ucenter_members

- DELETE FROM  `t_ucenter_members`  WHERE `uid` = 161816 AND  `username` = 2118463822; 

- 项目用户表:t_common_member

- DELETE FROM  `t_common_member`  WHERE `uid` = 161816 AND  `username` = 2118463822; 

- 手机号绑定关系:t_common_member_profile

- DELETE FROM  `t_common_member_profile`  WHERE `uid` = 161816 AND  `mobile` = ******; 

- touyouquan  open_member_bind表moblie字段