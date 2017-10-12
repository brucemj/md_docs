
## country字段处理
  http://gallery.echartsjs.com/editor.html?c=xSyKKAw97l

  说明数据库中 userservice中的electroniccardover和odmcard中的location的字段的意思  mcc_mnc_lac_cid
  mcc(mobile country code)  移动国家代码（中国为460）
  mnc(mobile network code) 移动网络号码 (中国移动0，中国联通1，中国电信2)
  lac(location area code) 位置区域码
  cid(cell identity) 基站编码

  http://www.gpsspg.com/bs/mcc.htm#MCC
  我在数据库中添加了一个新的表mcccode，根据location就可以修改原表中的country字段


## 优化
- 打开失败 耗时 36s
  - http://fonts.useso.com/css?family=Lato:300,400,700

- 15s
  - http://119.23.36.98:9081/hyc/jslib/world.json
  - http://wxpic.wangpanzan.com/css/json/world.json

## 登录功能？
- 1.5s
  - http://119.23.36.98:9081/hyc/salecount/getodmmonthcount.do


## 引用cdn服务的时候，，怎么做到cdn加载不到之后用本地的
  ```
  <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.7/jquery.min.js"></script>
  <script>!window.jQuery && document.write(unescape('%3Cscript src="jquery/jquery-1.7.2.min.js"%3E%3C/script%3E'))</script>

  <script src="http://ajax.googleapis.com/ajax/libs/jqueryui/1.8/jquery-ui.min.js"></script>
  <script>!window.jQuery.ui && document.write(unescape('%3Cscript src="jquery/jquery-ui-1.8.21.custom.min.js"%3E%3C/script%3E'))</script>
  ```


- http://www.xiaobinglaile.com:9081/hyc/statisticscount/getsales.do
- http://www.xiaobinglaile.com:9081/hyc/
