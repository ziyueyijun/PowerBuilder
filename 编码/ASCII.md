# ASCII

## 控制字符

| 二进制    | 十进制 | 十六进制 | 缩写 | 可以显示的表示法 | 名称/意义                           |
| --------- | ------ | -------- | ---- | ---------------- | ----------------------------------- |
| 0000 0000 | 0      | 00       | NUL  | ␀                | 空字符（Null）                      |
| 0000 0001 | 1      | 01       | SOH  | ␁                | 标题开始                            |
| 0000 0010 | 2      | 02       | STX  | ␂                | 本文开始                            |
| 0000 0011 | 3      | 03       | ETX  | ␃                | 本文结束                            |
| 0000 0100 | 4      | 04       | EOT  | ␄                | 传输结束                            |
| 0000 0101 | 5      | 05       | ENQ  | ␅                | 请求                                |
| 0000 0110 | 6      | 06       | ACK  | ␆                | 确认回应                            |
| 0000 0111 | 7      | 07       | BEL  | ␇                | 响铃                                |
| 0000 1000 | 8      | 08       | BS   | ␈                | 退格                                |
| 0000 1001 | 9      | 09       | HT   | ␉                | 水平定位符号                        |
| 0000 1010 | 10     | 0A       | LF   | ␊                | 换行键                              |
| 0000 1011 | 11     | 0B       | VT   | ␋                | 垂直定位符号                        |
| 0000 1100 | 12     | 0C       | FF   | ␌                | 换页键                              |
| 0000 1101 | 13     | 0D       | CR   | ␍                | 归位键                              |
| 0000 1110 | 14     | 0E       | SO   | ␎                | 取消变换（Shift out）               |
| 0000 1111 | 15     | 0F       | SI   | ␏                | 启用变换（Shift in）                |
| 0001 0000 | 16     | 10       | DLE  | ␐                | 跳出数据通讯                        |
| 0001 0001 | 17     | 11       | DC1  | ␑                | 设备控制一（XON 启用软件速度控制）  |
| 0001 0010 | 18     | 12       | DC2  | ␒                | 设备控制二                          |
| 0001 0011 | 19     | 13       | DC3  | ␓                | 设备控制三（XOFF 停用软件速度控制） |
| 0001 0100 | 20     | 14       | DC4  | ␔                | 设备控制四                          |
| 0001 0101 | 21     | 15       | NAK  | ␕                | 确认失败回应                        |
| 0001 0110 | 22     | 16       | SYN  | ␖                | 同步用暂停                          |
| 0001 0111 | 23     | 17       | ETB  | ␗                | 区块传输结束                        |
| 0001 1000 | 24     | 18       | CAN  | ␘                | 取消                                |
| 0001 1001 | 25     | 19       | EM   | ␙                | 连接介质中断                        |
| 0001 1010 | 26     | 1A       | SUB  | ␚                | 替换                                |
| 0001 1011 | 27     | 1B       | ESC  | ␛                | 跳出                                |
| 0001 1100 | 28     | 1C       | FS   | ␜                | 文件分割符                          |
| 0001 1101 | 29     | 1D       | GS   | ␝                | 组群分隔符                          |
| 0001 1110 | 30     | 1E       | RS   | ␞                | 记录分隔符                          |
| 0001 1111 | 31     | 1F       | US   | ␟                | 单元分隔符                          |
| 0111 1111 | 127    | 7F       | DEL  | ␡                | 删除                                |

## 可显示字符

| 二进制    | 十进制 | 十六进制 | 图形        |
| --------- | ------ | -------- | ----------- |
| 0010 0000 | 32     | 20       | （空格）(␠) |
| 0010 0001 | 33     | 21       | !           |
| 0010 0010 | 34     | 22       | "           |
| 0010 0011 | 35     | 23       | #           |
| 0010 0100 | 36     | 24       | $           |
| 0010 0101 | 37     | 25       | %           |
| 0010 0110 | 38     | 26       | &           |
| 0010 0111 | 39     | 27       | '           |
| 0010 1000 | 40     | 28       | (           |
| 0010 1001 | 41     | 29       | )           |
| 0010 1010 | 42     | 2A       | *           |
| 0010 1011 | 43     | 2B       | +           |
| 0010 1100 | 44     | 2C       | ,           |
| 0010 1101 | 45     | 2D       | -           |
| 0010 1110 | 46     | 2E       | .           |
| 0010 1111 | 47     | 2F       | /           |
| 0011 0000 | 48     | 30       | 0           |
| 0011 0001 | 49     | 31       | 1           |
| 0011 0010 | 50     | 32       | 2           |
| 0011 0011 | 51     | 33       | 3           |
| 0011 0100 | 52     | 34       | 4           |
| 0011 0101 | 53     | 35       | 5           |
| 0011 0110 | 54     | 36       | 6           |
| 0011 0111 | 55     | 37       | 7           |
| 0011 1000 | 56     | 38       | 8           |
| 0011 1001 | 57     | 39       | 9           |
| 0011 1010 | 58     | 3A       | :           |
| 0011 1011 | 59     | 3B       | ;           |
| 0011 1100 | 60     | 3C       | <           |
| 0011 1101 | 61     | 3D       | =           |
| 0011 1110 | 62     | 3E       | >           |
| 0011 1111 | 63     | 3F       | ?           |



| 二进制    | 十进制 | 十六进制 | 图形 |
| --------- | ------ | -------- | ---- |
| 0100 0000 | 64     | 40       | @    |
| 0100 0001 | 65     | 41       | A    |
| 0100 0010 | 66     | 42       | B    |
| 0100 0011 | 67     | 43       | C    |
| 0100 0100 | 68     | 44       | D    |
| 0100 0101 | 69     | 45       | E    |
| 0100 0110 | 70     | 46       | F    |
| 0100 0111 | 71     | 47       | G    |
| 0100 1000 | 72     | 48       | H    |
| 0100 1001 | 73     | 49       | I    |
| 0100 1010 | 74     | 4A       | J    |
| 0100 1011 | 75     | 4B       | K    |
| 0100 1100 | 76     | 4C       | L    |
| 0100 1101 | 77     | 4D       | M    |
| 0100 1110 | 78     | 4E       | N    |
| 0100 1111 | 79     | 4F       | O    |
| 0101 0000 | 80     | 50       | P    |
| 0101 0001 | 81     | 51       | Q    |
| 0101 0010 | 82     | 52       | R    |
| 0101 0011 | 83     | 53       | S    |
| 0101 0100 | 84     | 54       | T    |
| 0101 0101 | 85     | 55       | U    |
| 0101 0110 | 86     | 56       | V    |
| 0101 0111 | 87     | 57       | W    |
| 0101 1000 | 88     | 58       | X    |
| 0101 1001 | 89     | 59       | Y    |
| 0101 1010 | 90     | 5A       | Z    |
| 0101 1011 | 91     | 5B       | [    |
| 0101 1100 | 92     | 5C       | \    |
| 0101 1101 | 93     | 5D       | ]    |
| 0101 1110 | 94     | 5E       | ^    |
| 0101 1111 | 95     | 5F       | _    |

| 二进制    | 十进制 | 十六进制 | 图形 |
| --------- | ------ | -------- | ---- |
| 0110 0000 | 96     | 60       | `    |
| 0110 0001 | 97     | 61       | a    |
| 0110 0010 | 98     | 62       | b    |
| 0110 0011 | 99     | 63       | c    |
| 0110 0100 | 100    | 64       | d    |
| 0110 0101 | 101    | 65       | e    |
| 0110 0110 | 102    | 66       | f    |
| 0110 0111 | 103    | 67       | g    |
| 0110 1000 | 104    | 68       | h    |
| 0110 1001 | 105    | 69       | i    |
| 0110 1010 | 106    | 6A       | j    |
| 0110 1011 | 107    | 6B       | k    |
| 0110 1100 | 108    | 6C       | l    |
| 0110 1101 | 109    | 6D       | m    |
| 0110 1110 | 110    | 6E       | n    |
| 0110 1111 | 111    | 6F       | o    |
| 0111 0000 | 112    | 70       | p    |
| 0111 0001 | 113    | 71       | q    |
| 0111 0010 | 114    | 72       | r    |
| 0111 0011 | 115    | 73       | s    |
| 0111 0100 | 116    | 74       | t    |
| 0111 0101 | 117    | 75       | u    |
| 0111 0110 | 118    | 76       | v    |
| 0111 0111 | 119    | 77       | w    |
| 0111 1000 | 120    | 78       | x    |
| 0111 1001 | 121    | 79       | y    |
| 0111 1010 | 122    | 7A       | z    |
| 0111 1011 | 123    | 7B       | {    |
| 0111 1100 | 124    | 7C       | \|   |
| 0111 1101 | 125    | 7D       | }    |
| 0111 1110 | 126    | 7E       | ~    |