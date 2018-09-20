[toc]
## cssover查询地址
- http://www.xiaobinglaile.com:9081/hyc/statisticscount/getsales.do
- http://www.xiaobinglaile.com:9081/hyc/

## country字段处理
  http://gallery.echartsjs.com/editor.html?c=xSyKKAw97l

  说明数据库中 userservice中的electroniccardover和odmcard中的location的字段的意思  mcc_mnc_lac_cid
  mcc(mobile country code)  移动国家代码（中国为460）
  mnc(mobile network code) 移动网络号码 (中国移动0，中国联通1，中国电信2)
  lac(location area code) 位置区域码
  cid(cell identity) 基站编码

  http://www.gpsspg.com/bs/mcc.htm#MCC
  我在数据库中添加了一个新的表mcccode，根据location就可以修改原表中的country字段


## 前端优化
- 打开失败 耗时 36s
  - http://fonts.useso.com/css?family=Lato:300,400,700

- 15s
  - http://119.23.36.98:9081/hyc/jslib/world.json
  - http://wxpic.wangpanzan.com/css/json/world.json

## 引用cdn服务的时候，怎么做到cdn加载不到之后用本地的
- 参考方案
  ```
  <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.7/jquery.min.js"></script>
  <script>!window.jQuery && document.write(unescape('%3Cscript src="jquery/jquery-1.7.2.min.js"%3E%3C/script%3E'))</script>

  <script src="http://ajax.googleapis.com/ajax/libs/jqueryui/1.8/jquery-ui.min.js"></script>
  <script>!window.jQuery.ui && document.write(unescape('%3Cscript src="jquery/jquery-ui-1.8.21.custom.min.js"%3E%3C/script%3E'))</script>
  ```

## 登录功能？
  - 1.5秒
    - http://119.23.36.98:9081/hyc/salecount/getodmmonthcount.do

## 数据更新 以及 转换 [未完成]
- INSERT IGNORE INTO cssdata.overodmcard(odmid, phoneModel, imei, softwareVersion, platformModel, totalRam, totalRom, createTime, updateTime, warrantyDate, replaceDate, location, flash, lcm, mainCamera, subCamera, country, detail)  SELECT *  FROM userservice.odmcard WHERE userservice.odmcard.odmid > 395407;

- 437476
- delete from cssdata.overodmcard where odmid > 395407;

- UPDATE cssdata.overodmcard e, cssdata.mcccode m  SET e.country=m.country_en , e.countryzh=m.country  WHERE odmid > 497484 and SUBSTRING_INDEX(e.location, '_',1)=m.mcc

## 用户信息 优化
### 优化前
- 得到外销odm的机型  平均耗时6s左右
```
SELECT DISTINCT (SUBSTRING(SUBSTRING_INDEX(sfv, '_',2), 4)) AS devicemodel FROM
  (
    SELECT DATE_FORMAT(createTime,'%Y.%m') mon, COUNT(*) num,
           IF(ownversion IS NULL, softwareVersion, ownversion) sfv
    FROM odmcard LEFT JOIN odm_own ON odmcard.`softwareVersion` = odm_own.`odmversion`
    GROUP BY sfv,mon
    HAVING num > 10
  ) t ;
```

### 优化尝试：
#### 增加机型字段
```
alter table odmcard add pm char(16) not null default '' After odmid ;
```
#### 机型更新规则
odmcard 表
|pm(计算后的机型)|softwareVersion|
|---|---|
|E1_EN|Dialog_Infinity_Pro_V0.6|
|E2X_INT|KAAE2X_INT_EN_1.14.804|


odm_own 表
|ownversion|odmversion|
|---|---|
|KAAE1_EN_M0_0.10.317|Dialog_Infinity_Pro_V0.6|

#### 两次更新
```
update cssdata.overodmcard o , cssdata.odm_own t
	set o.pm=(SUBSTRING(SUBSTRING_INDEX(t.ownversion, '_',2), 4))  	
	where o.pm='' and ( o.softwareVersion=t.ownversion or o.softwareVersion=t.odmversion ) ;
```
> Query OK, 293319 rows affected (5.11 sec)
  Rows matched: 293320  Changed: 293319  Warnings: 0

```
update cssdata.overodmcard o , cssdata.odm_own t
   set o.pm=(SUBSTRING(SUBSTRING_INDEX(o.softwareVersion, '_',2), 4))  	
   where o.pm='' and
     LENGTH( SUBSTRING_INDEX(o.softwareVersion, '_',2) )<20 and
     left(o.softwareVersion, 3)='KAA' and o.softwareVersion!=t.ownversion and
     o.softwareVersion!=t.odmversion ;
```
> Query OK, 128883 rows affected (27.29 sec)
  Rows matched: 128883  Changed: 128883  Warnings: 0

```
select odmid, softwareVersion from odmcard where LENGTH( SUBSTRING_INDEX(o.softwareVersion, '_',2) )>20 ;
```

#### 外销自主机型数据
```
# alter table overelectroniccard drop column countryzh2 ;

INSERT IGNORE INTO cssdata.overelectroniccard
SELECT *  FROM userservice.electroniccardover WHERE userservice.electroniccardover.overid > 9503;

UPDATE cssdata.overelectroniccard e, cssdata.mcccode m  SET e.country=m.country_en , e.countryzh=m.country  WHERE  SUBSTRING_INDEX(e.location, '_',1)=m.mcc

```

#### 增加机型索引
```
ALTER TABLE `odmcard` ADD INDEX pm ( `pm` ) ;
```

#### 机型查询
```
select pm , count(pm) num from odmcard where pm!='' group by pm having num>100 ;
```
> 50 rows in set (0.60 sec)

#### 销量查询
```
select pm , count(pm) sales , DATE_FORMAT(createTime,'%Y-%m-%d') days from odmcard where pm='E1_USD' group by days ;
select pm , count(pm) sales , DATE_FORMAT(createTime,'%Y-%m-%d') days from odmcard where pm='E2X_BGG' group by days ;

select pm , count(pm) sales , DATE_FORMAT(createTime,'%Y-%uweek') as weeks from odmcard where pm='E2X_BGG' group by weeks ;
select pm , count(pm) sales , DATE_FORMAT(createTime,'%Y-%uweek') as weeks from odmcard where pm='E2X_CLA' group by weeks ;
```
