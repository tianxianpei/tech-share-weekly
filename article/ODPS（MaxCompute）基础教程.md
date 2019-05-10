# ODPS（MaxCompute）基础教程

**项目名称：天鲜配**<br />**修订历史：v1.00    **

| 版本号 | 作者 | 时间 | 内容 |
| --- | --- | --- | --- |
| v1.00 | 赵林刚 | 2018-12-31 | dataWorks 使用简单教程 |
| v2.00 | 赵林刚 | 2019-05-10 | odps使用过程分享 |


<a name="f3b45366"></a>
## 1. 什么是ODPS

- 简单讲就是数据仓库，可以存储海量数据，可针对海量数据进行分析、计算。
  - 本命其实叫 MaxCompute ，本文介绍统称为ODPS 
  - 官方文档链接：[https://help.aliyun.com/document_detail/27800.html?spm=a2c4g.11186623.6.542.17ae65d4wAeKXV](https://help.aliyun.com/document_detail/27800.html?spm=a2c4g.11186623.6.542.17ae65d4wAeKXV)
  - DataWorks 开发套件
    - 是数据工场，对ODPS数据进行加工处理，主要提供了：[数据集成](https://help.aliyun.com/document_detail/72961.html#concept-dr3-k2v-42b)、[数据开发](https://help.aliyun.com/document_detail/74423.html#concept-ykq-3zb-p2b)、[数据管理](https://help.aliyun.com/document_detail/73833.html#concept-sgr-5rt-q2b)、[数据治理](https://help.aliyun.com/document_detail/73660.html#concept-zsz-44h-r2b)、[数据分享](https://help.aliyun.com/document_detail/73263.html#concept-ewh-bsh-r2b)等功能。
    - 官方文档链接：[https://help.aliyun.com/document_detail/73015.html?spm=a2c4g.11186623.2.13.5ef65b9cBmTZdQ#concept-wqv-qbp-r2b](https://help.aliyun.com/document_detail/73015.html?spm=a2c4g.11186623.2.13.5ef65b9cBmTZdQ#concept-wqv-qbp-r2b)

<a name="A1sBG"></a>
## [](#9qfmgf)2. 登录篇（阿里云子账号）

- 子账号登录地址：[https://signin.aliyun.com/login.htm](https://signin.aliyun.com/login.htm)

- 产品列表：数加 · DataWorks
- 账号赋权：如需要进行数据开发，需要根据业务需求，赋对应的工作空间的对应权限。
- 进入DataWorks > 工作空间列表页面，单击对应项目中的进入工作区，即可进入数据开发页面。（如下图）


![](https://cdn.nlark.com/yuque/0/2018/png/174740/1546215971019-d2a880e6-f0ad-4b7b-bada-f304212fabd7.png#align=left&display=inline&height=143&originHeight=558&originWidth=3234&status=done&width=827)

- 子账号登录问题：使用过程中有任何子账号登录问题，请及时@赵林刚 解决



<a name="19782537"></a>
## 2.使用篇

- 目前数据仓库的整体概况
  - ![image.png](https://cdn.nlark.com/yuque/0/2019/png/174740/1557460021784-213c9b9e-3dc8-4a44-9534-8a383b178339.png#align=left&display=inline&height=586&name=image.png&originHeight=586&originWidth=952&size=68906&status=done&width=952)
- 目前承载的业务
  - 数据地图货品维度数据统计
  - 数据埋点分析
  - 财务报表
  - 客服报表
  - 运营周报
  - 数据备份
  - 业务操作日志备份分析
  - 其他日志：系统运行日志
  - BI 数据分析相关（市场部BI）
- 开发前环境准备
  - 开通DataWorks 权限的子账号
  - 创建项目（1）
    - 官方的文档：[https://help.aliyun.com/document_detail/27815.html?spm=a2c4g.11186623.6.568.60d01df0XvZAoh](https://help.aliyun.com/document_detail/27815.html?spm=a2c4g.11186623.6.568.60d01df0XvZAoh)
    - 目前我们的工作空间
      - ![image.png](https://cdn.nlark.com/yuque/0/2019/png/174740/1557467509553-9a5ffa4b-6df3-41d9-af5a-2d6faab6366a.png#align=left&display=inline&height=441&name=image.png&originHeight=882&originWidth=3236&size=777729&status=done&width=1618)
  - 新建调度资源（2）
    - 一般进行简单的数据分析只需要默认的调度资源就满足业务需求（目前的模式就是按量付费）
    - 需要进行特殊的数据集成、数据操作时会用到自定义资源
      - PyOdps 资源组：执行py脚本的资源组
      - mongoDB 资源组：进行MongDb --> ODPS 时会用到资源进行数据同步。
  - 新增数据源（3）
    - 路径：选择项目 -> 选择数据集成 -> 同步资源管理 -> 数据源
      - 按照官方文档新增即可
      - 数据源列表
      - ![image.png](https://cdn.nlark.com/yuque/0/2019/png/174740/1557468319814-8a6570fb-c3a1-457e-af1a-642951224453.png#align=left&display=inline&height=331&name=image.png&originHeight=661&originWidth=1703&size=509825&status=done&width=851.5)
  - 批量数据上云（4）
    - 路径：选择项目 -> 选择数据集成 -> 同步资源管理 -> 数据源 -> 整库数据迁移
      - ![image.png](https://cdn.nlark.com/yuque/0/2019/png/174740/1557468426464-d6ae6c20-2668-4aee-9fa4-35dfe7f92229.png#align=left&display=inline&height=476&name=image.png&originHeight=951&originWidth=1730&size=291047&status=done&width=865)
  - 数据开发前准备工作完成，可以进入开发阶段。
<a name="edf991e7"></a>
## 3 开发篇

- 数据开发
  - 基本概念：
    - 业务流程：解决一个业务的抽象模型，可以是一个问题的处理流程。
    - 解决方案：多个业务流程组合成一个解决方案，在同一个解决方案里面可以复用相同的业务流程。
    - 其他的概念：[https://help.aliyun.com/document_detail/73017.html?spm=a2c4g.11186623.6.543.3b757c78aHPhAD](https://help.aliyun.com/document_detail/73017.html?spm=a2c4g.11186623.6.543.3b757c78aHPhAD)
  - 数据开发流程：
    - ![image.png](https://cdn.nlark.com/yuque/0/2019/png/174740/1557468757049-15f2f416-048a-4fa8-a5a2-c1b47fb58e5d.png#align=left&display=inline&height=346&name=image.png&originHeight=346&originWidth=301&size=40992&status=done&width=301)
- 数据开发流程：
  - 选取两个现有的业务进行数据开发演示
    - 财务部门需求
    - 数据埋点分析
      - 流程图如下
      - ![image.png](https://cdn.nlark.com/yuque/0/2019/png/174740/1557469147900-73973cb0-9415-470e-bc26-ac067abbde16.png#align=left&display=inline&height=259&name=image.png&originHeight=517&originWidth=1124&size=140577&status=done&width=562)
      - ![image.png](https://cdn.nlark.com/yuque/0/2019/png/174740/1557469185601-73bc7d6b-f9f2-4932-9161-c43c412ca090.png#align=left&display=inline&height=227&name=image.png&originHeight=419&originWidth=1045&size=96819&status=done&width=567)
<a name="dcbd2def"></a>
## 4 运维

- 运维中心：
  - ![image.png](https://cdn.nlark.com/yuque/0/2019/png/174740/1557469503297-aa03a075-09cb-4323-a061-53b6d6b79bf5.png#align=left&display=inline&height=379&name=image.png&originHeight=758&originWidth=1693&size=889401&status=done&width=846.5)

