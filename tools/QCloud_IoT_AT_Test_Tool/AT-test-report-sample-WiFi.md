## 腾讯云IoT AT指令模组测试
--------------------------------------------
###1.设备信息(PSK方式)设置/查询指令测试
```
AT+TCDEVINFOSET=1,"S3EUVBRJLB","demo-device","**************"
OK
+TCDEVINFOSET:OK
AT+TCDEVINFOSET?
+TCDEVINFOSET:1,"S3EUVBRJLB","demo-device",78
OK
```
- ####设备信息(PSK方式)设置/查询指令测试: 通过
--------------------------------------------
--------------------------------------------
###2.MQTT连接/查询状态/断开指令测试
```
AT+TCMQTTCONN=1,10000,240,1,1
OK
+TCMQTTCONN:OK
AT+TCMQTTSTATE?
+TCMQTTSTATE:1
OK
AT+TCMQTTDISCONN
+TCMQTTDISCON,2
OK
AT+TCMQTTSTATE?
+TCMQTTSTATE:0
OK
AT+TCMQTTCONN=1,10000,60,1,1
OK
+TCMQTTCONN:OK
AT+TCMQTTSTATE?
+TCMQTTSTATE:1
OK
```
- ####MQTT连接/查询状态/断开指令测试: 通过
--------------------------------------------
--------------------------------------------
###3.MQTT订阅/取消订阅指令测试
```
AT+TCMQTTSUB="$sys/operation/result/S3EUVBRJLB/demo-device",0
OK
+TCMQTTSUB:OK
AT+TCMQTTSUB="$shadow/operation/result/S3EUVBRJLB/demo-device",0
OK
+TCMQTTSUB:OK
AT+TCMQTTSUB="S3EUVBRJLB/demo-device/control",0
OK
+TCMQTTSUB:OK
AT+TCMQTTSUB?
+TCMQTTSUB:"$sys/operation/result/S3EUVBRJLB/demo-device",0
+TCMQTTSUB:"$shadow/operation/result/S3EUVBRJLB/demo-device",0
+TCMQTTSUB:"S3EUVBRJLB/demo-device/control",0
OK
AT+TCMQTTUNSUB="$shadow/operation/result/S3EUVBRJLB/demo-device"
OK
+TCMQTTUNSUB:OK
AT+TCMQTTUNSUB="S3EUVBRJLB/demo-device/control"
OK
+TCMQTTUNSUB:OK
AT+TCMQTTSUB?
+TCMQTTSUB:"$sys/operation/result/S3EUVBRJLB/demo-device",0
OK
```
- ####MQTT订阅/取消订阅指令测试: 通过
--------------------------------------------
--------------------------------------------
###4.MQTT发布/接收消息指令测试
```
AT+TCMQTTSUB="S3EUVBRJLB/demo-device/data",0
OK
+TCMQTTSUB:OK
AT+TCMQTTPUB="S3EUVBRJLB/demo-device/data",0,"Hello_From_ESP8266"
OK
+TCMQTTPUB:OK
+TCMQTTRCVPUB:"S3EUVBRJLB/demo-device/data",18,"Hello_From_ESP8266"
AT+TCMQTTPUB="$sys/operation/S3EUVBRJLB/demo-device",0,"{\"type\": \"get\"\, \"resource\": [\"time\"]}"
OK
+TCMQTTPUB:OK
+TCMQTTRCVPUB:"$sys/operation/result/S3EUVBRJLB/demo-device",32,"{"type":"get","time":1567515312}"
AT+TCMQTTPUB="S3EUVBRJLB/demo-device/data",1,"{\"action\":\"test\"\,
\"time\":1567515313\,
\"position\":\"pos-4693\"}"
OK
+TCMQTTRCVPUB:"S3EUVBRJLB/demo-device/data",59,"{"action":"test",
"time":1567515313,
"position":"pos-4693"}"
+TCMQTTPUB:OK
AT+TCMQTTPUBL="S3EUVBRJLB/demo-device/data",1,385
OK
>
{\"action\":\"test\"\,
\"time\":1567515313\,
\"text\":\"1HELLO1234567890987654321world1234567890987654321QAZWSXEDCRFVTGBYHNUJMIKOPqazwsxedcrfvtgbyhnujmikolp2HELLO1234567890987654321world1234567890987654321QAZWSXEDCRFVTGBYHNUJMIKOPqazwsxedcrfvtgbyhnujmikolp3HELLO1234567890987654321world1234567890987654321QAZWSXEDCRFVTGBYHNUJMIKOPqazwsxedcrfvtgbyhnujmikolp\"
\"position\":\"pos-5792\"}
+TCMQTTRCVPUB:"S3EUVBRJLB/demo-device/data",385,"{ "action" : "test"  ,
 "time" :1567515313 ,
 "text" : "1HELLO1234567890987654321world1234567890987654321QAZWSXEDCRFVTGBYHNUJMIKOPqazwsxedcrfvtgbyhnujmikolp2HELLO1234567890987654321world1234567890987654321QAZWSXEDCRFVTGBYHNUJMIKOPqazwsxedcrfvtgbyhnujmikolp3HELLO1234567890987654321world1234567890987654321QAZWSXEDCRFVTGBYHNUJMIKOPqazwsxedcrfvtgbyhnujmikolp" 
 "position" : "pos-5792" }"
+TCMQTTPUBL:OK
```
- ####MQTT发布/接收消息指令测试: 通过
--------------------------------------------
--------------------------------------------
###5.MQTT订阅/发布权限测试
```
AT+TCMQTTSUB="S3EUVBRJLB/demo-device/event",0
OK
+TCMQTTSUB:FAIL,-108
AT+TCMQTTSUB?
+TCMQTTSUB:"$sys/operation/result/S3EUVBRJLB/demo-device",0
+TCMQTTSUB:"S3EUVBRJLB/demo-device/data",0
OK
AT+TCMQTTPUB="S3EUVBRJLB/demo-device/control",0,"hello"
OK
+TCMQTTPUB:OK
AT+TCMQTTPUB="S3EUVBRJLB/demo-device/control",1,"hello"
OK
+TCMQTTPUB:FAIL,202
```
- ####MQTT订阅/发布权限测试: 通过
--------------------------------------------
--------------------------------------------
###6.MQTT重连测试
```
+TCMQTTDISCON,-103
+TCMQTTRECONNECTING
+TCMQTTRECONNECTED
AT+TCMQTTSTATE?
+TCMQTTSTATE:1
OK
AT+TCMQTTSUB?
+TCMQTTSUB:"$sys/operation/result/S3EUVBRJLB/demo-device",0
+TCMQTTSUB:"S3EUVBRJLB/demo-device/data",0
OK
AT+TCMQTTPUB="S3EUVBRJLB/demo-device/data",0,"Hello_From_ESP8266"
OK
+TCMQTTPUB:OK
+TCMQTTRCVPUB:"S3EUVBRJLB/demo-device/data",18,"Hello_From_ESP8266"
```
- ####MQTT重连测试: 通过
--------------------------------------------
--------------------------------------------
###7.OTA升级及固件读取指令测试
```
AT+TCOTASET=?
+TCOTASET:1(ENABLE)/0(DISABLE),"FW_version"
OK
AT+TCMQTTSTATE?
+TCMQTTSTATE:1
OK
AT+TCMQTTDISCONN
+TCMQTTDISCON,2
OK
AT+TCDEVINFOSET=1,"S3EUVBRJLB","demo-device","**************"
OK
+TCDEVINFOSET:OK
AT+TCMQTTCONN=1,10000,240,1,1
OK
+TCMQTTCONN:OK
AT+TCOTASET=1,"1.0.0"
OK
+TCOTASET:OK
+TCOTASTATUS:ENTERUPDATE
+TCOTASTATUS:UPDATESUCCESS
AT+TCOTASET=0,"1.0.0"
OK
+TCOTASET:OK
AT+TCMQTTDISCONN
+TCMQTTDISCON,2
OK
AT+TCFWINFO?
OK
+TCFWINFO:"1.2.0",17300,"a2aa3c261ebfc1322edafd37edb6b183",716800
```
- ####OTA升级及固件读取指令测试: 通过
--------------------------------------------
--------------------------------------------
###8.产品信息设置/动态注册指令测试
```
AT+TCPRDINFOSET=?
+TCPRDINFOSET:"TLS_MODE(1)","PRODUCT_ID","PRODUCT_SECRET_BCC","DEVICE_NAME"
OK
AT+TCDEVREG=?
OK
AT+TCMQTTSTATE?
+TCMQTTSTATE:0
OK
AT+TCPRDINFOSET=1,"S3EUVBRJLB","**************","test-dev-reg1"
OK
+TCPRDINFOSET:OK
AT+TCDEVREG
OK
+TCDEVREG:OK
AT+TCDEVINFOSET?
+TCDEVINFOSET:1,"S3EUVBRJLB","test-dev-reg1",70
OK
AT+TCMQTTCONN=1,10000,240,1,1
OK
+TCMQTTCONN:OK
```
- ####产品信息设置/动态注册指令测试: 通过
--------------------------------------------
--------------------------------------------
###9.证书设备指令测试
```
AT+TCCERTADD=?
ERR CODE:0x01090000
ERROR
```
- ####证书设备指令测试: 指令不存在
--------------------------------------------
--------------------------------------------
###10.网络注册指令测试
```
AT+TCREGNET=?
ERR CODE:0x01090000
ERROR
```
- ####网络注册指令测试: 指令不存在
--------------------------------------------
--------------------------------------------
###11.模组信息读取指令测试
```
AT+TCMODULE
Module HW name: ESP-LAUNCHER
Module FW version: QCloud_AT_ESP_WiFi_v1.1.0
Module Mac addr: 80:7d:3a:58:e3:70
Module FW compiled time: Aug 12 2019 19:45:07
Module Flash size: 4MB
OK
```
- ####模组信息读取指令测试: 通过
--------------------------------------------
--------------------------------------------
--------------------------------------------
## 腾讯云IoT AT指令模组测试
#### 测试时间： 2019-09-03 20:55:53
#### 模组名称： ESP8266
- #####模组硬件信息:  ESP-LAUNCHER
- #####模组固件版本:  QCloud_AT_ESP_WiFi_v1.1.0

--------------------------------------------
#### MQTT AT指令测试: 通过
- ####设备信息(PSK方式)设置/查询指令测试: 通过
- ####MQTT连接/查询状态/断开指令测试: 通过
- ####MQTT订阅/取消订阅指令测试: 通过
- ####MQTT发布/接收消息指令测试: 通过
- ####MQTT订阅/发布权限测试: 通过
- ####MQTT重连测试: 通过
--------------------------------------------
#### 其他AT指令测试:
- ####OTA升级及固件读取指令测试: 通过
- ####产品信息设置/动态注册指令测试: 通过
- ####证书设备指令测试: 指令不存在
- ####网络注册指令测试: 指令不存在
