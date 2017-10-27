
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
WHERE SUBSTR(SUBSTRING_INDEX(softwareVersion, '_', 1), 4 ) in ('E2') and upbynetwork=1 and createtime > timestamp('2017-04-20')
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
# 新建表，插入数据
select top 5
I_ID , S_IMEI, S_SendMobileNumber,S_SMSCNumber , S_SMSCDateTime, D_UpdateTime, S_softwareVersion
from KKIMEI.dbo.t_mo_imei where S_SMSCDateTime>dateadd(day,0,'2017-4-20') ;

```

-
