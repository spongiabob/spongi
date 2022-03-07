---
title: python pycharm虚拟环境问题
author: 
  name: nancyxia
  link: https://github.com/Spongiabob
date: 2022-03-07 18:32:00 +8
categories: [Blogging, Tool]
tags: [Pyton相关]
---

python 工具pycharm用到国密加密方式，必须使用python3.6.8版本



### 问题现象

使用接口脚本涉及到国密加密方式，而国密加密接口依赖库依赖python3.6.8版本，本地原本使用的是3.8版本，导致加密接口不可用


### 问题解决

1.python官网安装python3.6.8
2.重新安装python3.6.8的虚拟环境
![pycharm]({{"/assets/img/blog/pycharm1.jpeg"|absolute_url}}){: width="100" height="100"}
3.新增虚拟环境目录，选中3.6.8版本
![pycharm]({{"/assets/img/blog/pycharm2.jpeg"|absolute_url}}){: width="100" height="100"}
4.如果本地有多个虚拟环境env，请到env的bin 目录下进行source activite 激活（此步骤不操作将会导致5步骤失败，创景重复的env lib）
source env/bin/activate 
![pycharm]({{"/assets/img/blog/pycharm3.jpeg"|absolute_url}}){: width="100" height="100"}
5.创建env需要的依赖库（requirements.txt包含安装所需要的Python库目录）
python3 -m pip install -r requirements.txt




