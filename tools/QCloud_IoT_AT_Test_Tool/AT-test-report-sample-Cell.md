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
AT+TCMQTTCONN=1,30000,240,1,1
OK
+TCMQTTCONN:OK
AT+TCMQTTSTATE?
+TCMQTTSTATE: 1
OK
AT+TCMQTTDISCONN
+TCMQTTDISCON:0
OK
AT+TCMQTTSTATE?
+TCMQTTSTATE: 0
OK
AT+TCMQTTCONN=1,30000,60,1,1
OK
+TCMQTTCONN:OK
AT+TCMQTTSTATE?
+TCMQTTSTATE: 1
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
AT+TCMQTTPUB="S3EUVBRJLB/demo-device/data",0,"Hello_From_M6315"
OK
+TCMQTTPUB:OK
+TCMQTTRCVPUB:"S3EUVBRJLB/demo-device/data",16,"Hello_From_M6315"
AT+TCMQTTPUB="$sys/operation/S3EUVBRJLB/demo-device",0,"{\"type\": \"get\", \"resource\": [\"time\"]}"
OK
+TCMQTTPUB:OK
+TCMQTTRCVPUB:"$sys/operation/result/S3EUVBRJLB/demo-device",32,"{"type":"get","time":1567514535}"
AT+TCMQTTPUB="S3EUVBRJLB/demo-device/data",1,"{\"action\":\"test\",
\"time\":1567514536,
\"position\":\"pos-3237\"}"
OK
+TCMQTTPUB:OK
+TCMQTTRCVPUB:"S3EUVBRJLB/demo-device/data",59,"{"action":"test",
"time":1567514536,
"position":"pos-3237"}"
AT+TCMQTTPUBL="S3EUVBRJLB/demo-device/data",1,383
OK
>
{\"action\":\"test\",
\"time\":1567514538,
\"text\":\"1HELLO1234567890987654321world1234567890987654321QAZWSXEDCRFVTGBYHNUJMIKOPqazwsxedcrfvtgbyhnujmikolp2HELLO1234567890987654321world1234567890987654321QAZWSXEDCRFVTGBYHNUJMIKOPqazwsxedcrfvtgbyhnujmikolp3HELLO1234567890987654321world1234567890987654321QAZWSXEDCRFVTGBYHNUJMIKOPqazwsxedcrfvtgbyhnujmikolp\"
\"position\":\"pos-4811\"}
OK
+TCMQTTPUBL:OK
+TCMQTTRCVPUB:"S3EUVBRJLB/demo-device/data",369,"{"action":"test",
"time":1567514538,
"text":"1HELLO1234567890987654321world1234567890987654321QAZWSXEDCRFVTGBYHNUJMIKOPqazwsxedcrfvtgbyhnujmikolp2HELLO1234567890987654321world1234567890987654321QAZWSXEDCRFVTGBYHNUJMIKOPqazwsxedcrfvtgbyhnujmikolp3HELLO1234567890987654321world1234567890987654321QAZWSXEDCRFVTGBYHNUJMIKOPqazwsxedcrfvtgbyhnujmikolp"
"position":"pos-4811"}"
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
+TCMQTTDISCON:0
+TCMQTTRECONNECTING
+TCMQTTRECONNECTED
+TCMQTTSUB:OK
+TCMQTTSUB:OK
AT+TCMQTTSTATE?
+TCMQTTSTATE: 1
OK
AT+TCMQTTSUB?
+TCMQTTSUB:"$sys/operation/result/S3EUVBRJLB/demo-device",0
+TCMQTTSUB:"S3EUVBRJLB/demo-device/data",0
OK
AT+TCMQTTPUB="S3EUVBRJLB/demo-device/data",0,"Hello_From_M6315"
OK
+TCMQTTPUB:OK
+TCMQTTRCVPUB:"S3EUVBRJLB/demo-device/data",16,"Hello_From_M6315"
```
- ####MQTT重连测试: 通过
--------------------------------------------
--------------------------------------------
###7.OTA升级及固件读取指令测试
```
AT+TCOTASET=?
+TCOTASET: "ENABLE/DISABLE"， "FW_version"
OK
AT+TCMQTTSTATE?
+TCMQTTSTATE: 1
OK
AT+TCMQTTDISCONN
+TCMQTTDISCON:0
OK
AT+TCDEVINFOSET=1,"S3EUVBRJLB","demo-device","**************"
OK
+TCDEVINFOSET:OK
AT+TCOTASET=1,"1.0.0"
OK
+TCOTASET:OK
+TCOTASTATUS: ENTERUPDATE
+TCOTASTATUS: UPDATESUCCESS
AT+TCOTASET=0,"1.0.0"
OK
+TCOTASET:OK
AT+TCFWINFO?
OK
+TCFWINFO: "1.2.0",17300,"a2aa3c261ebfc1322edafd37edb6b183",307200
```
- ####OTA升级及固件读取指令测试: 通过
--------------------------------------------
--------------------------------------------
###8.产品信息设置/动态注册指令测试
```
AT+TCPRDINFOSET=?
+TCPRDINFOSET:"TLS_MODE(0/1/2)","PRODUCT_ID","PRODUCT_SECRET_BCC","DEVICE_NAME"
OK
AT+TCDEVREG=?
OK
AT+TCMQTTSTATE?
+TCMQTTSTATE: 0
OK
AT+TCPRDINFOSET=1,"S3EUVBRJLB","**************","test-dev-reg1"
OK
+TCPRDINFOSET:OK
AT+TCDEVREG
+TCDEVREG:OK
AT+TCDEVINFOSET?
+TCDEVINFOSET:1,"S3EUVBRJLB","test-dev-reg1",15
OK
AT+TCMQTTCONN=1,30000,240,1,1
OK
+TCMQTTCONN:OK
```
- ####产品信息设置/动态注册指令测试: 通过
--------------------------------------------
--------------------------------------------
###9.证书设备指令测试
```
AT+TCCERTADD=?
AT+TCCERTADD:"CERT_NAME", CERT_SIZE (1-4096)
OK
```
```
AT+TCCERTADD?
+TCCERTADD: "test_VM_1.crt",1212
+TCCERTADD: "test_VM_1.key",1708
OK
AT+TCCERTCHECK="test_VM_1.crt"
OK
+TCCERTCHECK: OK
AT+TCCERTCHECK="test_VM_1.key"
OK
+TCCERTCHECK: OK
AT+TCDEVINFOSET=2,"83X1JNA86X","test_VM_1","test_VM_1.crt"
OK
+TCDEVINFOSET:OK
AT+TCMQTTCONN=2,30000,240,1,1
OK
+TCMQTTCONN:OK
AT+TCMQTTSTATE?
+TCMQTTSTATE: 1
OK
AT+TCMQTTSUB="83X1JNA86X/test_VM_1/data",0
OK
+TCMQTTSUB:OK
AT+TCMQTTPUB="83X1JNA86X/test_VM_1/data",1,"{\"action\":\"test\",\"time\":1567514708,\"position\":\"pos-3797\"}"
OK
+TCMQTTPUB:OK
+TCMQTTRCVPUB:"83X1JNA86X/test_VM_1/data",57,"{"action":"test","time":1567514708,"position":"pos-3797"}"
AT+TCMQTTDISCONN
+TCMQTTDISCON:0
OK
AT+TCMQTTSTATE?
+TCMQTTSTATE: 0
OK
AT+TCCERTDEL="test_VM_1.crt"
OK
+TCCERTDEL: OK
AT+TCCERTDEL="test_VM_1.key"
OK
+TCCERTDEL: OK
```
- ####证书设备指令测试: 通过
--------------------------------------------
--------------------------------------------
###10.网络注册指令测试
```
AT+TCREGNET=?
+TCDEVINFO: "MODULE_TYPE (0/1)","ACTION(0/1)","STATE(0/1)","IP", [,"SSID","PSW"(WIFI_MODULE)] [,"CSQ"(CELLULAR_MODULE)]
OK
AT+TCREGNET?
+TCREGNET: 0,1,"10.168.88.168",24
OK
AT+TCREGNET=0,0
OK
+TCREGNET: 0,NET_CLOSE
AT+TCREGNET=0,1,CMNET
OK
+TCREGNET: 0,NET_OK
AT+TCREGNET?
+TCREGNET: 0,1,"10.118.201.107",24
OK
```
- ####网络注册指令测试: 通过
--------------------------------------------
--------------------------------------------
###11.模组信息读取指令测试
```
AT+TCMODULE
Module HW name: M6315_MOD_BOM_R001_20190508@H1
Module FW version: M6315-PTCH0S00
Module IMEI: 863924040143031
Module FW compiled time: 20190827_152209
Module Flash size:4MB
OK
```
- ####模组信息读取指令测试: 通过
--------------------------------------------
--------------------------------------------
--------------------------------------------
## 腾讯云IoT AT指令模组测试
#### 测试时间： 2019-09-03 20:45:14
#### 模组名称： M6315
- #####模组硬件信息:  M6315_MOD_BOM_R001_20190508@H1
- #####模组固件版本:  M6315-PTCH0S00

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
- ####证书设备指令测试: 通过
- ####网络注册指令测试: 通过
