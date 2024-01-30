
[aliyun设备接入](https://help.aliyun.com/zh/iot/getting-started/using-mqtt-fx-to-access-iot-platform?spm=a2c4g.11174283.0.0.6ff8b5811XrkjP)

物模型
[设备属性上报](https://help.aliyun.com/zh/iot/user-guide/device-properties-events-and-services?spm=a2c4g.11186623.0.0.76577cec5I6T4c#section-g4j-5zg-12b)
`/sys/a2iFUuJvc94/esp32/thing/event/property/post`
可简化上报数据
```
{
    "params": {
        "point": {
            "value":288
        }
    }
}
```

[获取期望属性值](https://help.aliyun.com/zh/iot/user-guide/desired-device-property-values?spm=a2c4g.11186623.0.0.35ed436ajbUKib)

发布topic: `/sys/a2iFUuJvc94/esp32/thing/property/desired/get`
数据:
```
{
 "params" : [
        "point"
    ]
}
```

[设置设备期望](SetDeviceDesiredProperty)
云端调用更新

~~使用消息转发实现上报属性时，更新属性期望~~

[清空期望属性值](https://help.aliyun.com/zh/iot/user-guide/desired-device-property-values?spm=a2c4g.11186623.0.i30#section-kqt-y5y-zgb)

`/sys/a2iFUuJvc94/esp32/thing/property/desired/delete`
```
{
"params": {
        "point":{}
    },
}
```

[获取属性快照QueryDevicePropertyStatus](https://help.aliyun.com/zh/iot/developer-reference/api-querydevicepropertystatus-2018-01-20?spm=a2c4g.11186623.0.i174)

[物联网API调试](https://next.api.aliyun.com/api/Iot/2018-01-20/QueryDevicePropertyStatus?tab=DEBUG&params={%22DeviceName%22:%22esp32%22,%22ProductKey%22:%22a2iFUuJvc94%22})



| Supported Targets | ESP32 | ESP32-C2 | ESP32-C3 | ESP32-C6 | ESP32-H2 | ESP32-S2 | ESP32-S3 |
| ----------------- | ----- | -------- | -------- | -------- | -------- | -------- | -------- |

# ESP-MQTT MQTT over WSS Sample application
(See the README.md file in the upper level 'examples' directory for more information about examples.)

This example connects to the broker mqtt.eclipseprojects.io over secure websockets and as a demonstration subscribes/unsubscribes and send a message on certain topic.
(Please note that the public broker is maintained by the community so may not be always available, for details please see this [disclaimer](https://iot.eclipse.org/getting-started/#sandboxes))

It uses ESP-MQTT library which implements mqtt client to connect to mqtt broker.

## How to use example

### Hardware Required

This example can be executed on any ESP32 board, the only required interface is WiFi and connection to internet.

### Configure the project

* Open the project configuration menu (`idf.py menuconfig`)
* Configure Wi-Fi or Ethernet under "Example Connection Configuration" menu. See "Establishing Wi-Fi or Ethernet Connection" section in [examples/protocols/README.md](../../README.md) for more details.

Note how to create a PEM certificate for mqtt.eclipseprojects.io:

PEM certificate for this example could be extracted from an openssl `s_client` command connecting to mqtt.eclipseprojects.io.
In case a host operating system has `openssl` and `sed` packages installed, one could execute the following command to download and save the root certificate to a file (Note for Windows users: Both Linux like environment or Windows native packages may be used).
```
echo "" | openssl s_client -showcerts -connect mqtt.eclipseprojects.io:443 | sed -n "1,/Root/d; /BEGIN/,/END/p" | openssl x509 -outform PEM >mqtt_eclipse_org.pem
```
Please note that this is not a general command for downloading a root certificate for an arbitrary host;
this command works with mqtt.eclipseprojects.io as the site provides root certificate in the chain, which then could be extracted
with text operation.

### Build and Flash

Build the project and flash it to the board, then run monitor tool to view serial output:

```
idf.py -p PORT flash monitor
```

(To exit the serial monitor, type ``Ctrl-]``.)

See the Getting Started Guide for full steps to configure and use ESP-IDF to build projects.

## Example Output

```
I (3714) event: sta ip: 192.168.0.139, mask: 255.255.255.0, gw: 192.168.0.2
I (3714) system_api: Base MAC address is not set, read default base MAC address from BLK0 of EFUSE
I (3964) MQTT_CLIENT: Sending MQTT CONNECT message, type: 1, id: 0000
I (4164) MQTTWSS_EXAMPLE: MQTT_EVENT_CONNECTED
I (4174) MQTTWSS_EXAMPLE: sent publish successful, msg_id=41464
I (4174) MQTTWSS_EXAMPLE: sent subscribe successful, msg_id=17886
I (4174) MQTTWSS_EXAMPLE: sent subscribe successful, msg_id=42970
I (4184) MQTTWSS_EXAMPLE: sent unsubscribe successful, msg_id=50241
I (4314) MQTTWSS_EXAMPLE: MQTT_EVENT_PUBLISHED, msg_id=41464
I (4484) MQTTWSS_EXAMPLE: MQTT_EVENT_SUBSCRIBED, msg_id=17886
I (4484) MQTTWSS_EXAMPLE: sent publish successful, msg_id=0
I (4684) MQTTWSS_EXAMPLE: MQTT_EVENT_SUBSCRIBED, msg_id=42970
I (4684) MQTTWSS_EXAMPLE: sent publish successful, msg_id=0
I (4884) MQTT_CLIENT: deliver_publish, message_length_read=19, message_length=19
I (4884) MQTTWSS_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/topic/qos0
DATA=data
I (5194) MQTT_CLIENT: deliver_publish, message_length_read=19, message_length=19
I (5194) MQTTWSS_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/topic/qos0
DATA=data
```



