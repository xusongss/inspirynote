
 <center> <font size=7>蓝牙透传协议 </font></center >

<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

* [第一节 链路数据包结构（Base Package）](#第一节-链路数据包结构base-package)
  * [Base Package结构](#base-package结构)
* [第二节 指令传输（TYPE=0x02）](#第二节-指令传输type0x02)
  * [修订记录](#修订记录)
  * [Package基本结构：](#package基本结构)
    * [TYPE := audio](#type-audio)
    * [TYPE := get (属性列表)](#type-get-属性列表属性列表)
    * [TYPE := set  (属性列表)](#type-set-属性列表属性列表)
    * [TYPE := ack](#type-ack)
  * [属性列表](#span-id-属性列表属性列表span)
* [CRC16-CCITT](#span-idcrc16-ccittcrc16-ccittspan)

<!-- tocstop -->

@import "BaseProtocol.md"
# 第二节 指令传输（TYPE=0x02）
- 本节定义的结构对应第一节DATA部分
## 修订记录
|协议版本|日期|章节|说明|
| ------ | ------ | ------ | ------ |
|0x01| 2017/03/08| | 增加了get，set方法，以及ack|
## Package基本结构：
```
------------------------
| TYPE VER (1) | ......|
------------------------
TYPE：四个bit，数据类型
VER：四个bit，版本号
```
```
TYPE    := audio | get | set | ack
VER     := 0x01
audio   := 0x01
get     := 0x02
set     := 0x03
ack     := 0x04
ERRCODE := success | fail
success :=0x01
fail    :=0xFF
```
### TYPE := audio
```
-------------------------------------
| TYPE VER (1) |  COMMAND (1 BYTE)  |
-------------------------------------
TYPE := audio
COMMAND := audio_N
audio_N  := N
```
>* audio 类型package 表示播放指定语音，COMMAND表示语音号，没有DATA字段
>* audio类型包没有ack
### TYPE := get ([属性列表](#属性列表))
```
----------------------------------------
| TYPE VER (1) |  SEQ (1 BYTE)  | DATA |
----------------------------------------
TYPE := get
SEQ := seq
DATA := OCTETSTRING "\r\n"
```
>* get 类型package 表示获取设备某一属性， SEQ 字段为随机值seq， DATA表示要获取的属性名称
>* get 类型package 需要ack回复
### TYPE := set  ([属性列表](#属性列表))
```
----------------------------------------
| TYPE VER (1) |  SEQ (1 BYTE)  | DATA |
----------------------------------------
TYPE := set
SEQ := seq
DATA := KEY "\r\n" VALUE "\r\n"
KEY := OCTETSTRING
VALUE :=OCTETSTRING
```
>* set 类型package 表示设置设备某一属性， SEQ 字段为随机值seq， DATA表示要获取的属性名称和值
>* set 类型package 需要ack回复
### TYPE := ack
```
---------------------------------------------------
| TYPE VER (1) |  SEQ (1 BYTE)  | ERRCODE | DATA |
--------------------------------------------------
TYPE := ack
SEQ := seq
DATA := OCTETSTRING "\r\n"
```
>* get set 类型package 需要ack类型package回复， SEQ需要保持一致
>* 响应get类型pkg时 DATA := OCTETSTRING "\r\n"
>* 相应set类型pkg时 不需要DATA字段
## <span id= "属性列表">属性列表</span>
|属性名称|读写|类型|
| ------ | ------ | ------ |
|Device.DeviceInfo.SerialNumber|R|STRING|



@import "CRC16-CCITT.md"
