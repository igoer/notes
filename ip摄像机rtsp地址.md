# ip摄像机rtsp地址

本文主要记录当前市面上的网络摄像机rtsp流媒体地址的请求方式，将rtsp流媒体地址直接在VLC播放器中打开即可实时预览ip摄像机的实时画面。

## hikvision（海康威视）
rtsp流媒体地址格式如下：
```
rtsp://[username]:[password]@[ip]:[port]/[codec]/[channel]/[subtype]/av_stream
```
**说明：**
username: 用户名。例如admin。
password: 密码。例如12345。
ip: 为设备IP。例如 192.0.0.64。
port: 端口号默认为554，若为默认可不填写。
codec：有h264、MPEG-4、mpeg4这几种。
channel: 通道号，起始为1。例如通道1，则为ch1。
subtype: 码流类型，主码流为main，辅码流为sub。

**示例：**
请求海康摄像机通道1的主码流，Url如下
```
主码流：
rtsp://admin:12345@192.0.0.64:554/h264/ch1/main/av_stream
rtsp://admin:12345@192.0.0.64:554/MPEG-4/ch1/main/av_stream

子码流：
rtsp://admin:12345@192.0.0.64/mpeg4/ch1/sub/av_stream
rtsp://admin:12345@192.0.0.64/h264/ch1/sub/av_stream
```

## dahua（大华）
rtsp流媒体地址格式如下：
```
rtsp://[username]:[password]@[ip]:[port]/cam/realmonitor?channel=[chan]&subtype=[type]
```
**说明：**
username: 用户名。例如admin。
password: 密码。例如admin。
ip: 为设备IP。例如 10.7.8.122。
port: 端口号默认为554，若为默认可不填写。
channel: 通道号，起始为1。例如通道2，则为channel=2。
subtype: 码流类型，主码流为0（即subtype=0），辅码流为1（即subtype=1）。

**示例：**
例如，请求某设备的通道2的辅码流，Url如下
```
rtsp://admin:admin@10.12.4.84:554/cam/realmonitor?channel=2&subtype=1
```

## NV（NewVisioncctv）
rtsp流媒体地址格式如下：
```
rtsp://[ip]:[port]/user=[username]&password=[password]&channel=[chan]&stream=[stream].sdp?real_stream--rtp-caching=100
```
**说明：**
ip: 为设备IP。例如 10.7.8.122。
port: 端口号默认为554，若为默认可不填写。
username: 用户名。例如admin。
password: 密码。例如admin。
chan: 通道号，起始为1。例如通道2，则为channel=2。
stream: 码流类型，主码流为0（stream=0），辅码流为1（stream=1）。
real_stream--rtp-caching：该参数暂时不清楚，不是必须的参数。

**示例：**
例如，请求某设备的通道2的辅码流，Url如下
```
rtsp://192.168.1.10:554/user=admin&password=&channel=1&stream=0.sdp
rtsp://192.168.1.10:554/user=admin&password=&channel=1&stream=0.sdp?real_stream--rtp-caching=100
```
