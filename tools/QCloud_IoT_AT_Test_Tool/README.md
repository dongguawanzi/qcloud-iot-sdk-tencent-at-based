## 腾讯云IoT AT指令模组测试工具使用说明

### 1. 使用环境：
本工具基于python3和pyserial/paho-mqtt模块，安装python3和pip之后，执行下面命令安装模块
```
pip install pyserial
pip install paho-mqtt
```

### 2. 工具配置文件
工具配置文件默认为：Tool_Config.ini
配置文件里面包括了测试使用的设备信息，产品信息，每个模组的配置信息如转义字符处理，AT指令长度限制等等
可以在配置文件里面填写多个设备及多个模组的配置信息，并在工具启动时通过命令行参数指定

### 3. 创建设备和权限
测试之前需要先在腾讯云物联平台创建设备和权限，可以参考这个文档[物联网通信](https://github.com/tencentyun/qcloud-iot-sdk-embedded-c/blob/master/docs/%E7%89%A9%E8%81%94%E7%BD%91%E9%80%9A%E4%BF%A1%E5%B9%B3%E5%8F%B0.md)

测试工具主要针对IoT Hub平台-普通产品-密钥认证的设备进行测试，请到控制台创建一个密钥设备，并将设备的产品ID/设备名称/设备密钥填写到工具配置文件里面`[HUB-DEV1]`部分下列字段：

##### Product_ID / Device_Name / Device_Key

IoT Hub平台设备测试还需要到控制台权限列表创建一个具备发布和订阅权限的“data” topic
并确保“control” topic只有订阅权限，“event” topic只有发布权限

| MQTT Topic       | 操作权限                                  |
| ---------------- | --------------------------------------- |
| control          | 订阅权限 |
| event            | 发布权限 |
| data             | 订阅和发布权限 |

对于支持证书设备指令的模组测试，还需要创建证书设备，并到工具配置文件里面`[HUB-CERT1]`部分进行配置
对于支持动态注册指令的模组测试，还需要将设备的产品密钥及新设备名字配置到文件里面`[HUB-PRD1]`部分

如果需要对IoT Explorer平台设备进行测试，还需要参考[物联网开发平台](https://github.com/tencentyun/qcloud-iot-sdk-embedded-c/blob/master/docs/%E7%89%A9%E8%81%94%E7%BD%91%E5%BC%80%E5%8F%91%E5%B9%B3%E5%8F%B0.md) 创建设备并到配置文件里面`[IE-DEV1]`部分进行配置

### 4. 测试模式说明
#### CLI: 原始AT命令行模式
以原始命令行交互方式进行手动测试，类似于一个串口测试工具

#### MQTT: MQTT指令验证测试
对腾讯云IoT MQTT相关包括设备信息的AT命令进行自动化测试，并输出测试报告
测试内容包括设备信息设置和读取，MQTT连接/状态查询/断开连接，MQTT订阅/取消订阅/订阅查询，MQTT发布和接收（包括JSON格式数据，多行数据），MQTT发布订阅权限（QoS0/1）测试，断线自动重连测试。
该项测试为模组必须通过的测试，可以验证模组对腾讯云IoT MQTT相关AT指令的实现在功能上和规范上是否合格，如果有测试项失败，需要检查AT指令的格式以及返回值是否符合《腾讯云IoT AT指令集》文档规定，也可以与文件“AT-test-report-sample.md”进行参照对比

#### IOT: 腾讯云IoT AT指令验证测试
在MQTT测试模式基础上，增加了对其他腾讯云IoT AT命令包括（OTA，动态注册，证书设备，入网指令等）的自动化测试，并输出测试报告
如果模组实现了相关的功能指令，则对应的测试项也需要通过

#### HUB: IoT Hub测试
对在IoT Hub创建的设备进行测试，在循环模式下，会以QoS1循环收发指定数量的报文，并统计成功率和测试时间，可以依此进行稳定性测试

#### IE: IoT Explorer测试
对在IoT Explorer创建的设备进行测试，在循环模式下，可以循环进行发送接收template和event消息的测试，并统计成功率和测试时间，可以依此进行稳定性测试

#### WIFI: WIFI配网测试
对WiFi模组进行配网及绑定测试。须结合腾讯云物联app进行。

#### OTA: 固件下载及读取测试
对IoT Hub设备进行OTA升级测试，并将成功获取到的固件保存到本地文件

### 5. AT模组配置:
配置文件里面包括了部分已验证模组的配置信息，如新的模组在转义字符处理及AT指令长度限制等等与不一致，则需要在配置文件中增加模组配置
对于转义字符处理，建议的处理是对payload部分的双引号进行转义，如按以下AT指令方式进行MQTT消息发布
AT+TCMQTTPUB="S3EUVBRJLB/device1/data",0,"{\"action\":\"publish_test\",\"count\":1}"
模组须确保云端收到的payload为{"action":"publish_test","count":1}

工具目前支持三种转义字符处理方式

| 转义字符方式                   | 含义                                          |
| --------------------------- | --------------------------------------------- |
| add_no_escapes              | 对源数据不加任何转义处理 |
| add_escapes_for_quote       | 对源数据里面的双引号“前面添加转义字符\ |
| add_escapes_for_quote_comma | 对源数据里面的双引号“和逗号,前面添加转义字符\ |

### 6. 使用示例：
工具帮助信息：
  python QCloud_IoT_AT_Test_Tool.py -h

对连接到串口COM5的N720模组进行IoT AT全指令集自动化测试：
  python QCloud_IoT_AT_Test_Tool.py -p COM5 -a N720 -m IOT

对连接到串口COM5的M5311模组进行MQTT AT指令集自动化测试：
  python QCloud_IoT_AT_Test_Tool.py -p COM5 -a M5311 -m MQTT  

对连接到串口COM5的L206模组进行IoT Hub平台100次MQTT收发消息循环测试：
  python QCloud_IoT_AT_Test_Tool.py -p COM5 -a L206 -m HUB -n 100

对连接到串口/dev/ttyUSB0的ESP8266模组进行WiFi配网及IoT Explorer设备绑定测试：
  python QCloud_IoT_AT_Test_Tool.py -p /dev/ttyUSB0 -a ESP8266 -m WIFI

### 7. 注意事项
1. 本工具可以覆盖大部分指令的自动化测试，但是对于部分指令功能，仍需要通过CLI模式进行手动测试，比如AT命令参数的及返回数据的格式校验等，以及设备信息设置成功之后，需保存在FLASH中，在断电后仍然可以读取到，该项操作需要手动进行测试
2. 对本工具有使用疑问，可以直接阅读修改python源代码，或者联系Spike Lin(spikelin@tencent.com)