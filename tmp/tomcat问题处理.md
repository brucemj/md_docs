
183.239.173.98
################## localhost_access_log 数据样本
47.8.13.55 - - [05/Sep/2017:00:00:00 +0800] "POST /userservice/userInfoCollector/add.do HTTP/1.1" 200 15
180.151.95.82 - - [05/Sep/2017:00:00:00 +0800] "POST /userservice/userInfoCollector/add.do HTTP/1.1" 200 15

##################
cat localhost_access_log.2017-08-27.txt | grep POST | cut -f 7 -d ' ' | sort | uniq -c

  61756 /push/message/getMessage.do      
   1588 /userservice/electroniccardover/addover.do   
   2713 /userservice/electroniccardover/getImage.do
  14190 /userservice/electroniccardover/getServicePhone.do
  73069 /userservice/odm/addodm.do  
 908125 /userservice/userInfoCollector/add.do
   2712 /userservice/websiteover/getSiteData.do

功能，
	操作的table，table的大小，访问频率, 操作逻辑--同样数据 update ignore


	nginx http
	/userservice/odm/addodm.do --> css-waixiao
	/userservice/userInfoCollector/add.do --> css-xiaob


	css-waixiao /userservice/odm/addodm.do
	css-xiaob  	/userservice/userInfoCollector/add.do


head -n 100000 localhost_access_log.2017-08-27.txt | cut -f 4 -d ' ' | sort | uniq -c | sort | grep -v '      '

cat localhost_access_log.2017-08-27.txt | cut -f 4 -d ' ' | sort | uniq -c | sort | grep -v '      ' > ../bingfa.log

cat localhost_access_log.2017-09-05.txt | cut -f 4 -d ' ' | sort | uniq -c | sort | grep -v '      '


##################  访问错误
http://119.23.23.152:8080/userservice/odm/addodm.do

odm={"cid":"13677741","flash":"KMR31000BA_B614","imei":"356451080300622","lac":"1445","lcm":"ili9881c_hd720_dsi_vdo","mainCamera":"s5k3h7yxdl_mipi_raw","mcc":"410","md5":"352891725f196f763e556a134f76c94d","mnc":"1","phoneModel":"QMobile+E1","platformModel":"MT6735P","softwareVersion":"QMobileE1_MP_11_26","subCamera":"ov5670_mipi_raw","totalRam":"3GB","totalRom":"16GB"}


java.net.SocketTimeoutException: connect timed out
	at java.net.DualStackPlainSocketImpl.waitForConnect(Native Method)
	at java.net.DualStackPlainSocketImpl.socketConnect(DualStackPlainSocketImpl.java:85)
	at java.net.AbstractPlainSocketImpl.doConnect(AbstractPlainSocketImpl.java:339)
	at java.net.AbstractPlainSocketImpl.connectToAddress(AbstractPlainSocketImpl.java:200)
	at java.net.AbstractPlainSocketImpl.connect(AbstractPlainSocketImpl.java:182)
	at java.net.PlainSocketImpl.connect(PlainSocketImpl.java:172)
	at java.net.SocksSocketImpl.connect(SocksSocketImpl.java:392)
	at java.net.Socket.connect(Socket.java:579)
	at sun.net.NetworkClient.doConnect(NetworkClient.java:175)
	at sun.net.www.http.HttpClient.openServer(HttpClient.java:432)
	at sun.net.www.http.HttpClient.openServer(HttpClient.java:527)
	at sun.net.www.http.HttpClient.<init>(HttpClient.java:211)
	at sun.net.www.http.HttpClient.New(HttpClient.java:308)
	at sun.net.www.http.HttpClient.New(HttpClient.java:326)
	at sun.net.www.protocol.http.HttpURLConnection.getNewHttpClient(HttpURLConnection.java:997)
	at sun.net.www.protocol.http.HttpURLConnection.plainConnect(HttpURLConnection.java:933)
	at sun.net.www.protocol.http.HttpURLConnection.connect(HttpURLConnection.java:851)
	at org.apache.jmeter.protocol.http.sampler.HTTPJavaImpl.sample(HTTPJavaImpl.java:482)
	at org.apache.jmeter.protocol.http.sampler.HTTPSamplerProxy.sample(HTTPSamplerProxy.java:74)
	at org.apache.jmeter.protocol.http.sampler.HTTPSamplerBase.sample(HTTPSamplerBase.java:1166)
	at org.apache.jmeter.protocol.http.sampler.HTTPSamplerBase.sample(HTTPSamplerBase.java:1155)
	at org.apache.jmeter.threads.JMeterThread.executeSamplePackage(JMeterThread.java:475)
	at org.apache.jmeter.threads.JMeterThread.processSampler(JMeterThread.java:418)
	at org.apache.jmeter.threads.JMeterThread.run(JMeterThread.java:249)
	at java.lang.Thread.run(Thread.java:745)

######## INFO: Error parsing HTTP request header
	http://www.cnblogs.com/liqing1009/p/6253812.html
	server.xml
		maxHttpHeaderSize="你想要的大小"

######## 发现疑似问题
Sep 06, 2017 1:36:07 PM org.apache.tomcat.util.http.Parameters processParameters
INFO: Character decoding failed. Parameter [json] with value

######## tomcat catalina.out 时间配置， timezone
http://sharong.iteye.com/blog/2010373
添加了  -Duser.timezone=GMT+08
并做了内存配置的优化：
JAVA_OPTS="-Dfile.encoding=UTF-8 -XX:PermSize=128m -XX:MaxNewSize=256m -XX:MaxPermSize=256m -server -Xms1024m -Xmx1024m -Duser.timezone=GMT+08"

######## nginx 配置
http://119.23.36.98/nginx-status

http://119.23.36.98/userservice/odm/addodm.do

location = /userservice/odm/addodm.do {
                proxy_pass http://127.0.0.1:8080/userservice/odm/addodm.do;
                proxy_redirect     off;
                proxy_set_header   Host             $host;
                proxy_set_header   X-Real-IP        $remote_addr;
                proxy_set_header  X-Forwarded-For   $proxy_add_x_forwarded_for;
                client_max_body_size       20M;
                client_body_buffer_size    128k;

                proxy_connect_timeout      600;
                proxy_send_timeout         60;
                proxy_read_timeout         3600;
                }
        location /sts_service/ {
                proxy_pass http://127.0.0.1:9090/sts_service/;
                proxy_redirect     off;
                proxy_set_header   Host             $host;
                proxy_set_header   X-Real-IP        $remote_addr;
                proxy_set_header  X-Forwarded-For   $proxy_add_x_forwarded_for;
                client_max_body_size       20M;
                client_body_buffer_size    128k;

                proxy_connect_timeout      600;
                proxy_send_timeout         60;
                proxy_read_timeout         3600;
                }


service nginx restart
/bin/systemctl nginx restart
