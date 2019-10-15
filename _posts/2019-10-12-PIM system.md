---
layout: post
title: "个人信息管理系统初探"
subtitle: 'A preliminary design of Personal Information System '
author: "Hao"
header-style: text
catalog: true
tags:
  - PIM

---



![uL1XqJ.md.jpg](https://s2.ax1x.com/2019/10/12/uL1XqJ.md.jpg)
>  像大树一样，地平线以下扎根千丈；像冰山一样，海平面以下冰封万里



## As-Is： Dynalist 为主的个人卡片系统

### 1. 设计原则:

#### 1.1. 时间线
不同任务在时间轴上任务量和颗粒度
![uL1gxS.png](https://s2.ax1x.com/2019/10/12/uL1gxS.png)

#### 1.2. 空间及变量关系

从信息公开程度和信息本身性质（质、量）拆解
![uL1c28.png](https://s2.ax1x.com/2019/10/12/uL1c28.png)

### 2. 具体设计方案:

#### 2.1. 软件选型

- 最小全局
    - 官网说明书，官网浏览量
    - 专业选择网站比对
- 交叉验证
    - 看高质量点评；大v使用文章
- 批量测试使用，保持迭代，保持开放（方便退出和迁移），不定期关注基于新理念的效率软件
- 专业软件和通用软件
    - 基于使用成本和每个软件对于自身的定位。
      比如todolist 是整合到Dynalist 还是用专业软件Trello和Google Tasks，综合使用成本，用专用软件成本更低；同时基于Dynalist定位也建议剥离出该部分功能。
    - 一般建议优先专业软件，用户群、生态对于该软件加持和个人反向塑造会更强。

#### 2.2. 软件定位和功能

- 笔记类：Dynalist
  需要沉淀，便于以后搜索

    - 功能：卡片（Review周报以上、学习卡片、读书卡片、Principles卡片、人脉卡）
- 剪切板类：石墨文档
  
    - 功能：皓月长天blog（语音写作周报、TM周报）、家庭微信公众号 模板格式
- 文档协作类：石墨文档
- Todo list
    - 日级别、重复项：Google Calendar/Tasks
        - Google Calendar
            - Event：要参加的活动，比如看表演等。
            - Reminder：被动需要提醒小事情，比如家庭事情。
        - Google Tasks
            - 日课，周课，临时项目
            - 定位：主动个人要去做的事，比如日课，周课
    - 项目级别：
        - Dynalist：需要追踪记录，比如时间统计（时间点，时长），融入Dynalist
        - Trello：大周期项目化看板，特别是需要协作
- 便笺类：Google Keep
  
    - 阅后即焚，最低成本记录，然后kill notes，分类规整。
- 记忆学习类：Anki
  
    - 英语学习、学习卡片

#### 2.3. 软件Semantic Key设计：采用人，事，知行合一

  - 人
      - 人脉卡
  - 事
      - 时间：
          - Todo list
              - 日级别、重复项（Google Calendar/Tasks）
              - 大周期项目化看板（Trello）
          - 时间记录
              - OLTP：时间记录（Time meter）
              - OLAP：时间分析（Power BI）
      - 空间
          - 定期review：日（每日语音写作）、周、月、季、半年、年 Review（Dynalist）
          - Principles 卡片（Dynalist）
      - 变量关系
          - 家庭事项：家庭微信公众号
  - 知行合一
      - 学习卡片（Dynalist）
      - 读书卡片（Dynalist）

#### 2.4. 信息模型架构分析

- 元数据管理
  各种模板
- 数据获取
  数据迁移、卡片创建
    - 便笺类：（Google Keep）
        - Hot：马上处理
        - Warm：更新notes、项目计划（注意跟踪、闭环）
    - 卡片类：Principles、人脉卡、学习卡片、读书卡片（Dynalist）
- 数据整理
  整理、分类
    - Dynalist按calendar归类
- 数据加工
  慢改
    - 慢改：卡片
- 数据展现
  分享、输出整合文章，形成闭环
    - 卡片级别（Dynalist）
    - 个人：
        - 语音写作周报（皓月长天Blog）
        - 时间记录周报（皓月长天Blog）
        - 主题文章（皓月长天Blog、皓月长天微信公众号）
    - 家庭：家庭微信公众号

#### 2.5. OLTP（事物系统） & OLAP（分析系统）

- OLTP
    - 便笺类、卡片类（学习卡片、读书卡片）
- OLAP
    - 周报：语音写作、时间记录
    - 卡片类（Principles、人脉卡）

### 3. Dynalist实现

#### 3.1. Calender设计

​      利用日期进行卡片式设计
![uL1yPP.png](https://s2.ax1x.com/2019/10/12/uL1yPP.png)

#### 3.2. 卡片设计

每日展开
![uL168f.png](https://s2.ax1x.com/2019/10/12/uL168f.png)

### 4. 细节设计

**Q2：为什么读书卡片快写的时候只需要时间戳，不需要额外内容标题？**      

  A:

**示例如下：**

  ![uL1RKg.png](https://s2.ax1x.com/2019/10/12/uL1RKg.png)



首先追本溯源，时间戳是软件系统里非常重要的一个概念，是一条记录物理意义上的唯一key，也就是logical key，程序员和顾问在对数据进行追踪和修改判定的时候，都要靠时间戳这个唯一记录标识。

与之对应的就是业务意义上的key，也就是semantic key，比如物料主数据的semantic key是物料编号，人事主数据的semantic key 是员工编号等。

回到读书卡片，没有这个概念的人，在创建一张卡片，首先自然而然的想到的key 一定是semantic key，也就是我们常说的表达内容主题的标题。

但是这样有2个问题，一是不能唯一确定一张卡片，另外一个更要命，不能确保快速输入。所以阳老师推荐直接用类时间戳标记，从数据库的设计理念角度也正好做个交叉验证，采用logic key，也就是时间戳来唯一标识这个卡片，同时最大程度的实现快写，降低输入端的认知负荷。

这样其实就是跨领域知识理念的迁移，从一个领域提炼出的模型，在其他领域同样适用。

那么问题来了，时间戳作为标题，好是好，快是快，但是因为没有设置semantic key，也就是内容含义的标题，我无法快速的知道这张卡片的主题，用起来不顺手。

一个答案是我们根本不需要了解这个卡片的主题是什么，只需要利用强大的搜索功能即可。

但是，有的时候你并不能通过合适的关键词搜索到对应的内容，尤其是在卡片数量越来越多的时候，你的搜索的有效性就可能大打折扣。那么这个时候，有什么好的办法解决这个问题么？

这里还要类比数据库的另外一个重要的概念，也就是index 索引，它是在传统非内存计算数据库里，进行数据sql查询加速的最重要的手段，甚至没有之一。那么，迁移到个人知识笔记系统，在Dynalist也有雷类似的功能，就是tag标签。

快写阶段，你依然只需打上时间戳。而在慢改阶段，你可以加上tag标签。这样其实就是培养你的关键词意识。

当然，如果这还不够，最后，你还可以在慢改阶段给加上semantic key，也就是一般而言的内容标题。



## To-be：Zotero信息仓库+Dynalist 个人卡片系统

| 软件 | Zotero                         | Dynalist                       |
| ---- | ------------------------------ | ------------------------------ |
| 定位 | 多格式信息仓库                 | 个人卡片系统                   |
| 特点 | 重度，Translator协议，最小全局 | 轻度，极度灵活输入、处理和输出 |
| 方向 | 向外链接                       | 向内消化                       |
| 隐喻 | 眼耳鼻口舌                     | 消化道肠胃                     |



## Changelog

- !(2019-10-12) V1.0 初稿
