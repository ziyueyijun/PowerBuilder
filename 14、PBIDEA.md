# PBIDEA

## 1、uo_json

### 1.1、基础

```c
string ls_json_data,ls_json_value
uo_json  js
js = create uo_json
ls_json_data = '{"a":{"b":"123"},"c":"456","d":"789"}'
//1、构造json数据
js.parse(ls_json_data) 
//2、设置json
js.set("/a/b",'456')
//3、获取json
js.get("/a/b",ref ls_json_value)
//4、替换json
js.replace("c","789")
//5、删除json 
js.remove("d")
//6、返回json对象
messagebox("6",js.tostring(true,false,true))
//7、使用js对象的值，构造js2对象
uo_json  js2
js2 =  create uo_json
js.get("a",ref js2)
messagebox("7",js2.tostring(true,false,true))
//8、使用js2对象，构造js对象中的a属性的值
js2.set("b",'123')
js2.set("xx",'456')
js.replace("a",js2)
messagebox("8",js.tostring(true,false,true))
//9、构造json数组
integer i
uo_json  js_arr
js_arr =  create uo_json
for i=1 to  5
 js_arr.set("item" + string(i))
next
js.set("d",js_arr)
messagebox("9",js.tostring(true,false,true))
//10、获取js_arr对象数组的总个数
messagebox("10",js_arr.arrcount())
//11、获取js对象节点的总个数
messagebox("11",js.getitemcount())
//12、显示js对象所有节点和节点类型
long ll_count
string ls_keys[],ls_types[],ls_text
ll_count = js.GetKeys(ref ls_keys[],ref ls_types[])
ls_text = ""
for i=1 to  ll_count
 ls_text =  ls_text + ls_keys[i] + "(" + ls_types[i] + ")~r~n"
next
MessageBox("12",ls_text)
//13、在dw_1数据窗口中导入js对象
js.dwImportJson(dw_1)
//14、将js对象写入到json文件中
js.writejsonfile("Json构造与修改.json",true,false)
//15、清空json
js.clear()
```

### 1.2、将 JSON 文件导入到  DS 中

```c
boolean bSuccess
string  ls_name,ls_gh
long ll_rows,i
uo_json js
datastore i_ds
i_ds = create datastore
js = create  uo_json
//通过文件来构造json
js.parseFile('Json构造与修改.txt')
//设置将要赋值的窗口
dw_1.dataobject = "d_json"
//使用不可使数据窗口
i_ds.dataobject = ""
ll_rows =  js.dwImportJson(i_ds)
i_ds.sharedata(dw_1)
//显示jsong数据
mle_1.text = "rows : " + string(ll_rows) + "~r~n" +  js.ToString(true,false,true)
destroy js
GarbageCollect()
```

### 1.3、将 JSON 文件导入到 DW 中

```c
uo_json js,j,jarr
boolean  bSuccess
string ls_name,ls_gh,ls_json
long ll_rows
js  = create uo_json
//通过文件来构造json
js.parseFile('Json构造与修改.txt')
//将json构造与修改.txt里面的数据，导入到dw_1中
ll_rows = js.dwImportJson(dw_1) 
messagebox('提示','共导入' + string(ll_rows) + '行')
//显示导入的json数据
ls_json = js.tostring(true,false,true)
messagebox('',ls_json)
destroy js
//垃圾回收机制(英文简写GC):当需要分配的内存空间不再使用的时候，将调用垃圾回收机制来回收内存空间。
GarbageCollect()
```

### 1.4、将  DW 数据导出为 JSON 文件

```c
uo_json js
js = create  uo_json
//设置不导出rowid
js.setoption("rowid",false)
//所有数字型的值，全部转成字符串型
//js.setoption("valueasstring",true) 
//导入文本数据到dw_1窗口
js.parseFile("Json构造与修改.txt")
js.dwImportJson(dw_1)
js.clear()
//设置要导出的字段
string ls_columns[]
ls_columns[1] =  "a"
ls_columns[2] = "c"
ls_columns[2] = "d"
//导出所有行数所有列数
long ll_rows
ll_rows =  js.dwExportJson(dw_1,ls_columns[],"导出的数据.json") 
//显示导出的JSON数据
mle_1.text = js.toString(true,true,true)
destroy js
```

### 1.5、通过 SQL 语句生成 JSON 数据

```c
//连接数据库
SQLCA.DBMS = "MSS Microsoft SQL Server"
SQLCA.Database = "" //数据库名
SQLCA.LogPass = ''  //数据库密码
SQLCA.ServerName = "" //IP地址
SQLCA.LogId = "sa"  //数据库登录名
SQLCA.AutoCommit = False
SQLCA.DBParm = ""
connect using  sqlca;
//通过sql语句构造json
uo_json js
js = create uo_json
js.FromSQL(sqlca,"select * from 表名")
//显示json数据
mle_1.text = js.ToString(true,false,true)
destroy js
GarbageCollect()
```

### 1.6、多数据窗口生成 JSON

```c
uo_json jsMain,jsSub
int id
string ls_key
//将dw_1的数据转成json数据
jsMain = create uo_json
jsMain.dwExportJson(dw_1)
for id = 1 to dw_1.RowCount()
    
 dw_2.Reset()
    
 //改变dw_2中的数据
 int i
 for i = 1 to 1
  dw_2.InsertRow(0)
  dw_2.SetItem(i,1,string(i))
  dw_2.SetItem(i,2,string(i+1))
  dw_2.SetItem(i,3,string(i+2))
 next
     
 //将dw_2的数据转成json数据
 ls_key = "/" + string(i - 1) + "/sub"
 jsSub = Create uo_json
 jsSub.dwExportJson(dw_2)
 jsMain.Set(ls_key,jsSub)
 destroy  jsSub
     
next
//显示拼接的json
mle_1.text = jsMain.toString(TRUE,false,TRUE)
destroy jsMain
```

### 1.7、压缩与解压缩

```c
uo_json js
long ll_rows
js = create uo_json
js.parseFile('Json构造与修改.txt')
blob blbCompress
string  ls_text
//压缩
uo_compress compress
compress =  create uo_compress
blbCompress = compress.zCompress(blob(js.ToString(TRUE,FALSE,TRUE)),5)
//解压缩
ls_text = string(compress.zUnCompress(blbCompress))
mle_1.text = ls_text
destroy compress
MessageBox("","正常大小"+ string(len(ls_text)) +",压缩大小:"+string(len(blbCompress)))
destroy js
```

### 1.8、导出指定列

```c
uo_json  js
js = create uo_json
string ls_cols[]
ls_cols[1] = "a"
ls_cols[2] = "c"
dw_1.dataobject = ""
//通过文件构建JSON
js.parseFile('Json构造与修改.txt')
//将json数据导入到dw_1中
js.dwImportjson(dw_1)
//最后一个参数，如果是TRUE，就是排除这些列，为FALSE，只包含这些列
js.dwExportJson(dw_1,ls_cols,false) 
mle_1.text = js.toString(true,false)
destroy js
```

### 1.9、XML 生成 JSON

```c
uo_json  json
json =  create uo_json
string xml
xml = '<?xml version="1.0" encoding="UTF-8"?>'
xml += '<a>1</a>'
xml  += '<c>2</c>'
//xml 转 json
json.jsonFromXML(xml)
mle_1.text = json.toString(true,false,true)
//将json数据导入到dw_1中
json.dwImportJson(dw_1)
destroy json
```

### 1.10、JSON 转 数组

```c
string ls_json
ls_json = '{"name":"黑黑","age":964086664302283}'
uo_json js
//构造json
js = create uo_json
js.parse(ls_json)
//构造数组
uo_json  jsArr[]
int i
for i = 1 to 10
 jsArr[i] = create uo_json
 jsArr[i].set("key",i+1)
next
js.set("arr",jsArr[])
mle_1.text = js.ToString(true,false,true)
destroy  js
```

### 1.11、生成父子结构 JSON

```c
uo_json js,jsTree
js = create uo_json
//构造JSON
js.parseFile("..\demores\json\t_sysmenu.json")
//参数1、自增编号
//参数2、上级编号
//参数3、下级类别名称
//参数4、设置指定的上级编号值（0:全部显示）
jsTree = js.JsonTableToTree("id","pid","child","0")
mle_1.text = jsTree.ToString(true,false,true)
dw_1.dataobject = ""
js.dwimportjson(dw_1)
destroy js
destroy jsTree
```

### 1.12、数组解析

```c
uo_json js
js = create uo_json
uo_json jsarr
jsarr = create uo_json
uo_json jsarr2
jsarr2 = create uo_json
//构造json数据
js.parse('{"a":[{"b":"1"},{"c":"2"}]}')
//通过json对象解析节点a下面的数组
js.get("/a",jsarr)
//现在jsarr等于（[{"b":"1"},{"c":"2"}]）
//获取数组总个数
messagebox("数组总个数",jsarr.arrcount())
//取数组下的第1个值（返回值：{"b":"1"}）
jsarr.get(1,jsarr2)
messagebox('数组中第1个json值',jsarr2.tostring())
//取数组下的第2个值（返回值：{"c":"2"}）
jsarr.get(2,jsarr2)
messagebox('数组中第1个json值',jsarr2.tostring())
destroy js
destroy jsarr
destroy jsarr2
```

## 2、uo_xml

### 2.1、获取节点

```c
//方法1
uo_xml xml
uo_xml_node node,nodeChild
string ls_text
string ls_name,ls_value
xml = create uo_xml
//解析XML文件
xml.parsefile( "treebuild.xml")
//获取第一个节点，并返回该节点下的内容
node = xml.GetNode(xml.Node_First)
if node.get(ls_name,ls_value) then
 ls_text = ls_text + node.GetContent(true) + "~r~n"
end if
//获取xml中library下的第二个property节点
node = node.GetNode("/library/property[2]")
if node.get(ls_name,ls_value) then
 ls_text = ls_text + node.GetContent(true) + "~r~n"
end if
//获取xml中library下的第一个property节点
node = xml.GetNode("/package/library/property")
if node.get(ls_name,ls_value) then
 ls_text = ls_text + node.GetContent(true) + "~r~n"
end if
destroy xml
messagebox('提示',ls_text)
```

```c
//方法2
uo_xml xml
uo_xml_node node,nodeChild
xml = create uo_xml
xml.parsefile( "4c.xml")
//1、根据路径获取xml节点和节点值
string ls_name,ls_value
node = xml.GetNode("/ufinterface/bill/billhead/pk_group") //获取节点
node.Get(ref ls_name,ref ls_value)  //节点名/节点值
messagebox('1、返回的节点名+节点值',ls_name + " : " + ls_value)
//2、根据链式操作获取xml节点和节点值
xml.GetNode("ufinterface").getnode("bill").getnode("billhead").getnode("pk_group").Get(ref ls_name,ref ls_value)
messagebox('2、返回的节点名+节点值',ls_name + " : " + ls_value)
//3、节点判断
int li_value
node = xml.GetNode("/ufinterface/bill/billhead/cgeneralbid/item/crowno")
if node.Get(ref ls_name,ref li_value) then
 messagebox('3、返回的节点名+节点值',ls_name + " : " + ls_value)
end if
destroy xml
```

### 2.2、获取字符串

```c
uo_xml xml
uo_xml_node node,nodeChild
xml = create uo_xml
//解析xml文件
xml.parsefile( "JTG.xml")
//获取xml字符串
messagebox('',xml.ToString(true))
destroy xml
```

### 2.3、保存 XML 文件

```c
uo_xml xml
uo_xml_node  node,nodeChild
string ls_name,ls_value
xml = create uo_xml
xml.parsefile("JTG.xml")
if xml.Save("xx.xml") then
 MessageBox("","JTG.xmll 另存为 xx.xml 成功!")
else
 MessageBox("","JTG.xmll 另存为 xx.xml 失败!")
end if
destroy xml
```

### 2.4、JSON 生成 XML

```c
uo_xml xml
xml = create uo_xml
uo_json js
js = create  uo_json
//构造json
js.parsefile("export.txt" )  
//json 转 xml
xml.fromJson(js.toString()) 
//显示xml字符串
mle_1.text = xml.tostring(true) 
destroy js
destroy xml
```

### 2.5、将 XML 文件导入到 DW 中

```c
long ll_count
uo_xml xml
dw_1.SetRedraw(false)
xml = create  uo_xml
//解析xml文件
xml.parsefile( "jtgm.xml") 
//将xml文件导入到dw_1数据窗口
ll_count = xml.dwImport(dw_1)
destroy xml
dw_1.SetRedraw(true) 
//弹出导入总条数
messagebox('',ll_count)
GarbageCollect()
```

### 2.6、清空 XML

```c
uo_xml xml
uo_xml_node  node,nodeChild
string ls_name,ls_value
xml = create uo_xml
//解析xml
xml.parsefile( "JTG.xml") 
//清空xml
xml.Clear() 
messagebox('',xml.ToString())
destroy xml
dw_1.dataobject =  ""
```

### 2.7、构造 XML

```c
//方法1
uo_xml xml
uo_xml_node nodeRoot,node,node1
uo_xml_attribute attr
string ls_name,ls_value
int i
xml = create uo_xml
//逐个添加
nodeRoot = xml.addnode("root") //添加root节点
attr = nodeRoot.addattribute("name") //添加name属性
attr.set("张三") //设置name属性值
attr = nodeRoot.addattribute("age") //添加age属性
attr.set(65) //设置age属性值
//循环添加
for i = 1 to 10
node = nodeRoot.addnode("content") //添加节点
node.set("这是记录 " + string(i))
attr = node.addattribute("record") //添加属性
attr.set(i)
node = nodeRoot.addnode("len") //添加节点
node.set(i)
next
messagebox('提示',xml.ToString(true))
destroy xml
```

```c
//方法2
uo_xml xml
uo_xml_node node,nodeChild
xml = create uo_xml
string ls_xml
ls_xml = '<?xml version="1.0" encoding="UTF-8"?><serviceesn>0001</serviceesn><userid>admin</userid><acountid>10010001</acountid><serviceid>100100000031</serviceid><result> <code>EA000110</code> <message>命令字符串为空</message></result><returntime>2020-07-30 19:04:04.139</returntime><returndata />'
xml.parse(ls_xml)
messagebox('提示',xml.toString())
destroy xml
```

## 3、uo_httpclient

### 1、文件下载

方法1：

```c
//下载一个文件
uo_httpclient ht_1
ht_1.DownLoadFile("http://mirrors.163.com/ubuntu-releases/21.04/ubuntu-21.04-live-server-amd64.iso","",true)
destroy ht_1
//参数1：下载地址
//参数2：下载文件指定的目录，不指定目录时，下载到当前目录的.\download\下面
//参数3：是否触发uo_httpclient中的ondownload事件
```

方法2：

```c
//下载一个blob文件
uo_httpclient ht_1
blob blbData
blbData = ht_1.DownLoad("http://pbwebapi.net:8090/download/readme.txt")
destroy ht_1
//将blob文件转为utf8格式
messagebox('',ht_1.FromUtf8(blbData))
```

### 2、文件上传

```c
uo_httpclient ht_1
ht_1 = create  uo_httpclient
//设置提交方式
ht_1.SetHeader("accept-encoding","gzip, deflate")
ht_1.Setheader("cache-control","no-cache")
ht_1.SetHeader("uploadpath","tmp")
//设置上传目录
ht_1.SetForm("uploadpath","tmp")
//循环上传文件
long i
for i = 1 to 10
 ht_1.SetForm("filelist","..\demores\json\json" + string(i)  + ".txt",true)
next
//发送请求
ht_1.Request(ht_1.HttpPost, "[http://218.94.103.156:8090/pbvs/upload.php",TRUE](http://218.94.103.156:8090/pbvs/upload.php))
//接收返回信息
messagebox('',ht_1.response.text)
destroy ht_1
```

### 3、post请求

方法1：发送JSON对象（POST）

```c
string ls_json,ls_url
ls_json = '{"ID": "GetData","method": "[PBPlugin].f_getdata_json","Params": ["a1", "a2"]}'
ls_url = "http://pbwebapi.net:8090/pbvs/json"
//构造json对象
uo_json js
js = create uo_json
js.parse(ls_json) 
//httpclient发送json对象
uo_httpclient ht_1
ht_1 = create uo_httpclient
ht_1.Request(ht_1.HttpPost,ls_url,js)
//如果返回的json对象为空，那么就取文本数据，否则就取json数据
if ht_1.response.json.IsEmpty() then
 messagebox('text',ht_1.response.text)
else
 messagebox('json',ht_1.response.json.toString(true,false,true))
end if
destroy js
destroy ht_1
```

方法2：发送JSON对象2（POST）

```c
//构造JSON
uo_json js
js = create uo_json
js.Set("test","testvalue")
js.Set("age",10)
//发送请求
uo_httpclient ht_1
ht_1 = create uo_httpclient
ht_1.Request(ht_1.HttpPost,"http://pbwebapi.net:8090/pbvs/json",js)
//接收返回信息
messagebox('',ht_1.response.text)
destroy js
destroy ht_1
```

方法3：发送JSON字符串（POST）

```c
string ls_json,ls_url
ls_json = '{"ID": "GetData","method": "[PBPlugin].f_getdata_json","Params": ["a1", "a2"]}'
ls_url = "http://pbwebapi.net:8090/pbvs/json"
//httpclient发送json字符串（Request：参数4，发送utf-8格式）
uo_httpclient ht_1
ht_1 = create uo_httpclient
ht_1.SetHeader("Content-Type", "application/json")
ht_1.Request( ht_1.HttpPost,ls_url,ls_json,true)
//如果返回的json对象为空，那么就取文本数据，否则就取json数据
if ht_1.response.json.IsEmpty() then
 messagebox('text',ht_1.response.text)
else
 messagebox('json',ht_1.response.json.toString(true,false,true))
end if
destroy ht_1
```

### 4、get请求

方法1：发送get请求

```c
//注意：ht_1.response.data同样存有blob数据
blob blbData
uo_httpclient ht_1
ht_1 = create uo_httpclient
blbData = ht_1.Request(ht_1.HttpGet,"https://www.cnblogs.com/mfrbuaa/p/3779396.html")
messagebox('',ht_1.FromUtf8(blbData))
destroy ht_1
```

方法2：发送GET请求并返回JSON

```c
blob blbData
string ls_url
//淘宝查询
uo_crypto crypto
crypto = create uo_crypto
ls_url = "https://suggest.taobao.com/sug?code=utf-8&q=" +  crypto.URLEncode("啤酒",true)
destroy crypto
//发送请求
uo_httpclient ht_1
ht_1 = create uo_httpclient
blbData = ht_1.Request(ht_1.HttpGet,ls_url)
//接收json
uo_json js
js = create uo_json
js = ht_1.BlobToJson(blbData)
if not isvalid(js) then 
 MessageBox("","js是无效的")
 return
end if
//弹出json数据
messagebox('提示',js.toString(TRUE,false,true))
destroy js
destroy ht_1
```

方法3：设置输出模式并发送请求（GET）

```c
String ls_url
uo_httpclient ht_1
ht_1.SetTimeout( 6000)
//设置数据模式并发送GET请求
ls_url = "http://218.94.103.156:8090/download/documents/rapidjson-zh-cn.pdf"
ht_1.SetOutputMode(ht_1.ModeFile,"tmp\rapidjson-zh-cn.pdf")
ht_1.Request(ht_1.HttpGet,ls_url)
//获取返回信息
messagebox('',ht_1.response.text)
//获取错误信息
messagebox('',string( ht_1.response.errtext))
```

### 5、设置请求头

```c
uo_httpclient ht_1
ht_1 = create uo_httpclient
ht_1.SetHeader("Content-Length", "198") //设置资源大小
ht_1.SetHeader("Content-Type:", "application/x-javascript; charset=utf-8") //设置请求类型为javascript
ht_1.ClearHeader() //清除请求头信息
ht_1.SetHeader("Content-Type::::", "application/json; charset=utf-8") //设置请求类型为JSON
ht_1.SetHeader("User","huoguiwen") //设置请求授权用户
ht_1.SetHeader("Pass","adminstrator") //设置请求授权密码
ht_1.SetHeader("Accept-Encoding:","gzip") //声明浏览器支持的编码类型
ht_1.SetTimeout(600000) //设置超时时间
string ls_json
ls_json = '{"serviceesn": "0001","userid": "admin","resformat": "xml","acountid": "10010001","token": "","serviceid": "10010000001","argcounts": "1","args": [{"one": "","true": "","three": "","foure": "","file": ""}]}'
ht_1.Request(ht_1.HttpPost,"http://39.100.40.235:8808/host/dcip_call",ls_json,true)
if not ht_1.response.json.IsEmpty() then
 //如果返回的json数据不为空
 messagebox('json',ht_1.response.json.toString(true,false,true))
elseif not ht_1.response.xml.IsEmpty() then
 //如果返回的xml数据不为空
 messagebox('xml',ht_1.response.xml.toString(true))
elseif len(ht_1.response.text) > 0 then
 //如果返回了文本数据
 messagebox('xml', ht_1.response.text)
else
 //如果返回了二进制数据
 messagebox('blob',string(ht_1.response.data))
end if
destroy ht_1
```

### 6、表单请求

```c
uo_httpclient ht_1
ht_1 = create  uo_httpclient
//设置请求头
ht_1.SetHeader("accept-encoding","gzip, deflate")
ht_1.Setheader("cache-control","no-cache")
//设置表单信息
ht_1.SetForm("folderName","xuexiaojianjie")
ht_1.SetForm("schoolCode","1101053001")
ht_1.SetForm("type","1")
ht_1.SetForm("fileList","..\demores\json\0a538e49069e4e338faf3c134edeb2cb6.jpg",true)
//发送请求
ht_1.Request(ht_1.HttpPost, "http://ms.do-ok.com:18070/api/operatefile/v1/uploadFile")
//接收返回信息
messagebox('',ht_1.response.text)
destroy ht_1
```

### 7、数据窗口请求

```c
//1、数据窗口
int i
dw_1.Reset()
for i = 1 to 10
 dw_1.InsertRow(0)
next
//清除数据行的更新标志。
dw_1.ResetUpdate()
//发送请求
uo_httpclient ht_1
ht_1 = create uo_httpclient
ht_1.Request(ht_1.HttpPost,"http://pbwebapi.net:8090/pbvs/json",dw_1)
messagebox('datawindow',ht_1.response.text)
//2、不可视数据窗口
datastore ds
ds = create datastore
ds.dataobject = "d_master_table"
for i = 1 to 10
 ds.InsertRow(0)
next
//发送请求
ht_1.Request(ht_1.HttpPost,"http://pbwebapi.net:8090/pbvs/json",ds)
messagebox('datastore',ht_1.response.text)
destroy ds
```

## 4、uo_curl

### 1、post请求

方法1：

```c
uo_json jsData
jsData = create uo_json
jsData.set("/data/psn_cert_type","2")
jsData.set("/data/certno","510000202001010000")
jsData.set("/data/psn_name","李四")
jsData.set("/data/begntime","2020-01-01")
uo_curl curl
curl = create uo_curl
//设置 header
curl.setHeader("content-type", "application/json; charset=utf-8")
//设置超时时间
curl.SetTimeout(6000)
//设置url和传输方式
curl.setUrl("https://gz.echase.cn/gzdev/api/gateway/echasePay/pay","POST")
//设置debug日志
curl.setDebug(true,"tmp\post.txt")
//发送请求
curl.request(jsData)
messagebox('json',curl.response.json.toString(true,false,true))
messagebox('header',curl.response.headers.toString(true,false,true))
destroy jsData
destroy curl
```

方法2：

```c
uo_json jsData
jsData = create uo_json
jsData.set("/data/psn_cert_type","2")
jsData.set("/data/certno","510000202001010000")
jsData.set("/data/psn_name","李四")
jsData.set("/data/begntime","2020-01-01")
uo_curl curl
curl = create uo_curl
//设置 header
curl.setHeader("content-type", "application/json; charset=utf-8")
//设置超时时间
curl.SetTimeout(6000)
//设置debug日志
curl.setDebug(true,"tmp\post.txt")
//发送请求
curl.request(curl.HttpPost,"http://127.0.0.1:9000/test.html",jsData)
messagebox('json',curl.response.json.toString(true,false,true))
messagebox('header',curl.response.headers.toString(true,false,true))
destroy jsData
destroy curl
```

方法3：

```c
uo_curl curl
curl = create uo_curl
blob data
curl.SetUtf8(true)
//设置debug日志
curl.setDebug(true,"tmp\post.txt")
//设置用户代理
curl.SetUserAgent("httpclient chrome")
//设置认证账户密码
curl.SetBasicAuth("admin","12345")
//设置post表单
curl.SetForm("name","王三")
curl.SetForm("photo","..\demores\images\ha.bmp",true)
data = blob("hello buffer")
curl.SetForm("buffer",data)
//设置url
curl.SetUrl("Post","http://127.0.0.1:9000/get/test")
//发送请求
curl.request()
messagebox('json',curl.response.json.toString(true,false,true))
messagebox('header',curl.response.headers.toString(true,false,true))
destroy curl
```

### 2、get请求

方法1：

```c
uo_curl curl
curl = create uo_curl
curl.setUtf8(true)
//设置debug日志
curl.setDebug(true,"tmp\get.txt")
//设置get请求传递的参数
curl.setParam("hello","baby")
curl.setParam("国家","中国",true)
curl.setParam("address","南京",true)
curl.setParam("price","3+2=5",true)
//发送请求
curl.request(curl.HttpGet,"http://127.0.0.1:9000/get")
messagebox('json',curl.response.json.toString(true,false,true))
messagebox('header',curl.response.headers.toString(true,false,true))
destroy curl
```

方法2：

```c
uo_curl curl
curl = create uo_curl
//直接发送get请求
curl.request(curl.HttpGet,"https://weixin.huamianwuliu.cn/fileupload/health")
messagebox('json',curl.response.json.toString(true,false,true))
messagebox('header',curl.response.headers.toString(true,false,true))
destroy curl
```

### 3、文件上传

```c
uo_curl curl
curl = create uo_curl
//设置超时时间
curl.SetTimeout(60000)
//设置字符集
curl.setUtf8(true)
//设置debug日志
curl.setDebug(true,"tmp\upload.txt")
//设置请求头
curl.SetHeader("accept-encoding","gzip, deflate, br")
curl.Setheader("cache-control","no-cache")
//设置表单内容
curl.SetForm("type","image")  //文件类型
//curl.SetForm("path","/my-images/abc") //要上传的文件路径
curl.SetForm("file","d:\temp\splash.png",true) //要上传的文件名
//发送请求
curl.Request(curl.HttpPost, "https://weixin.huamianwuliu.cn/fileupload/file-upload")
//构造json
uo_json js
js = create uo_json
js.parse(curl.response.data)
messagebox('json',curl.response.json.toString(true,false,true))
messagebox('header',curl.response.headers.toString(true,false,true))
destroy curl
```

### 4、文件下载

方法1：

```c
uo_curl curl
curl = create uo_curl
curl.SetDownloadFile("tmp\_down_.pdf") //设置下载到本地的文件路径
curl.request(curl.HttpGet,"http://218.94.103.156:8090/download/documents/rapidjson-zh-cn.pdf") //发送请求
messagebox('json',curl.response.json.toString(true,false,true))
messagebox('header',curl.response.headers.toString(true,false,true))
destroy curl
```

方法2：异步

```c
uo_curl_async curl_async
curl_async = create uo_curl_async
//判断，如果正在运行中，返回
if curl_async.isBusy() then
 MessageBox("","is busy")
 return
end if
uo_curl curl
curl = uo_curl
string ls_id
//清除掉原有的列表
curl_async.clear_task()
//增加请求任务
ls_id = "rapidjson-zh-cn.pdf"
curl = curl_async.add_task(ls_id)
curl.SetDownloadFile("tmp\rapidjson-zh-cn.pdf")
curl.SetUrl("http://218.94.103.156:8090/download/documents/rapidjson-zh-cn.pdf","GET")
//增加请求任务
ls_id = "FreeImage3180.pdf"
curl = curl_async.add_task(ls_id)
curl.SetDownloadFile("tmp\FreeImage3180.pdf")
curl.SetUrl("http://218.94.103.156:8090/download/documents/FreeImage3180.pdf","GET")
//增加请求任务
ls_id = "msvisualc"
curl = curl_async.add_task(ls_id)
curl.SetDownloadFile("tmp\msvisualc.rar")
curl.SetUrl("http://218.94.103.156:8090/download/developer/msvisualc.rar","GET")
//增加请求任务
ls_id = "10201_gateways_win32.zip"
curl = curl_async.add_task(ls_id)
curl.SetDownloadFile("tmp\10201_gateways_win32.zip")
curl.SetUrl("http://218.94.103.156:8090/download/NoSQL/Redis-x64-5.0.10.zip","GET")
//增加请求任务
ls_id = "apache-tomcat-8.0.20-windows-x64.zip"
curl = curl_async.add_task(ls_id)
curl.SetDownloadFile("tmp\apache-tomcat-8.0.20-windows-x64.zip")
curl.SetUrl("http://218.94.103.156:8090/download/server/apache-tomcat-8.0.20-windows-x64.zip","GET")
//启动批量任务
curl_async.start_task()
destroy curl_async
```

## 5、uo_zip

### 1、压缩

```c
string is_password
uo_zip zip
zip = create uo_zip
//1、打开test.zip (文件路径+文件名，是否只读，zip压缩密码)
zip.open("test.zip",false,is_password)
//打开二进制数据压缩包
//blob buffer
//buffer = zip.ReadFile("test.zip")
//zip.open(buffer,is_password)
//2、判断压缩包下有没有1.txt文件
boolean lb_exist
lb_exist = zip.EntryExist("test/1.txt")
messagebox('',lb_exist)
//3、添加2.txt文件到压缩包
if zip.Add(".\add\2.txt") then
 messagebox('提示','添加2.txt到test.zip文件成功!')
end if
//4、添加blob
blob data
data = blob("this is text to add to zip")
if zip.Add("data",data) then
 messagebox('提示','添加blob数据成功!')
end if
//5、添加test2文件夹下的所有文件
long ll_count
ll_count = zip.AddPath("test2")
messagebox('',ll_count)
//关闭test.zip，并commit
zip.close()
//关闭test.zip，并roolback
//zip.Discard()
destroy zip
```

### 2、解压缩

```c
string is_password
uo_zip zip
zip = create uo_zip
//1、打开test.zip (文件路径+文件名，是否只读，zip压缩密码)
zip.open("test.zip",true,is_password)
//2、读取test.zip中某个文件的数据
blob data1,data2
data1 = zip.readBlob(1) //按索引号读
data2 = zip.readBlob("hello") //按名称读
//3、用不同的方式解压test.zip
long ll_count
ll_count = zip.extract("d:\temp\myzip",1) //按索引号解压（方式1）
//ll_count = zip.extract("d:\测试\myzip","hello") //按名称解压（方式2）
//ll_count = zip.extract("d:\temp\myzip\测试") //解压所有（方式3）
messagebox('',"已解压 " + string(ll_count) + " 个文件" + "~r~n")
//关闭test.zip，并commit
zip.close()
//关闭test.zip，并roolback
//zip.Discard()
destroy zip
```

## 6、uo_string

### 6.1、基础使用

#### 6.1.1、字符串拼接

```c
long ll_start,ll_end
uo_string str
string ls_a,ls_b
ls_a = '123;'
ls_b = '456;'
//拼接普通的字符串
str.append("a=")
str.append(ls_a)
str.append("b=")
str.append(ls_b)
//拼接 33的16进制（21）
str.append(33," {:02X}")
messagebox('',str.ToString())
```

#### 6.1.2、字符串判断是否为NULL

```c
uo_string str
//1、追加字符串
str.append("abc")
//2、判断对象是否是NULL
if str.IsNull() then
 messagebox('提示','NULL')
else
 messagebox('提示','不是NULL')
end if
//3、设置对象为NULL
str.SetNull()
//4、判断对象是否为NULL
if str.IsNull() then
 messagebox('提示','NULL')
else
 messagebox('提示','不是NULL')
end if
```

#### 6.1.3、重置字符串长度

```c
uo_string str
//追加字符串
str.append("abc123")
MessageBox("",str.toString())
//重置字符串长度
str.Resize(3)
MessageBox("",str.toString())
```

#### 6.1.4、字符串格式

```c
//示例1
uo_string str
//Format格式字符串语法(格式参考 https://fmt.dev/dev/syntax.html)
string ls_string
ls_string = "this {} is uo_string"
str.fromString(ls_string)
MessageBox("",str.toString())
str.format("{}.{}",'Zisay','QQ:15593838')
MessageBox("",str.toString())
//示例2
uo_string str
blob b
b = blob("hello")
str.format("16进制: {:08X}" , 88888)
str.format("10进制: {}" , 123)
str.format("日期: {}" , today())
str.format("时间: {}" , now())
str.format("字符串: {}" , 'hello world')
str.format("longlong: {}" , 32012400001)
str.format("blob: {}" , b)
messagebox('',str.toString())
```

#### 6.1.5、字符串颠倒顺序

```c
uo_string str
//构造字符串
str.append("abcd123")
//字符串颠倒顺序
str.Reverse()
MessageBox("",str.toString( ))
```

#### 6.1.6、字符串查找

```c
long ll_pos
uo_string str
str.append("abcd123aBcd123")
//从前往后查找（1、要查找的字符，2、从第0位开始查找，3、是否忽略大小写）
ll_pos = str.pos("bc",0,TRUE)
MessageBox("","pos : " + string(ll_pos))
//从后往前查找（1、要查找的字符，2、从第0位开始查找，3、是否忽略大小写）
ll_pos = str.lastpos("bc",0,TRUE)
MessageBox("","pos : " + string(ll_pos))
```

#### 6.1.7、字符串截取

```c
long ll_pos
string ls_str
uo_string str
str.append("abcd123aBcd123")
//字符串截取（1、从第几位开始截取）
ls_str = str.substr(2)
MessageBox("",ls_str)
//字符串截取（1、从第几位开始截取，截取多少位）
ls_str = str.substr(8,3)
MessageBox("",ls_str)
```

#### 6.1.8、字符串替换

```c
int n
uo_string str
//替换指定字符
str.append("abcd123aBcd123")
str.replace("ab"," hello ")
messagebox('1',str.toString())
//根据位置进行替换（1、从多少位开始，2、取多少位，3、要替换的字符）
str.fromString("abcd123aBcd123")
str.replace(9,4,"[hello]")
messagebox('2',str.toString())
```

#### 6.1.9、字符串插入

```c
uo_string str
//在指定位置插入字符串(1、从第几位开始插入，2、插入的字符)
str.append("abcd123aBcd123")
str.InsertStr(5,"[hello]").InsertStr(0,"[head]")
messagebox('',str.toString())
```

#### 6.1.10、字符串切割

```c
long i,ll_Count
uo_string str,item
string ls_items[]
//构造格式化字符串
str.FromString("aaa,bbb;ccc;dddd,eeeee")
//字符串切割（1、要切割的字符，2返回的数组）
ll_Count = str.Split(",;",ref ls_items[])
for i = 1 to ll_Count
 item.format("{}. {}",i,ls_items[i])
 messagebox('',item.toString())
next
```

#### 6.1.11、字符串进制转换

```c
uo_string str
int li_v
long ll_v
//十进制字符串转换成十进制数
str.FromString("1234.56")
if str.ToNumber(10,ref li_v) then
 MessageBox("1",li_v)
end if
//十六进制字符串转换成十进制数
str.FromString("A0") //或 "0xa0"
if str.ToNumber(16,ref ll_v) then
 MessageBox("",ll_v)
end if
```

#### 6.1.12、地址引用

```c
1、定义dll
//实际函数其实就是WINAPI GetWindowText 的封装
//可以声明为
//FUNCTION ulong GetText(ulong hwnd,ref string strText,ulong cch) LIBRARY "PbIdea.dll" ALIAS FOR "GetText"
//ref string strText 这个参数，实际上可以声明为一个字符串的地址
FUNCTION ulong GetText(ulong hwnd,ulong address,ulong cch) LIBRARY "PbIdea.dll" ALIAS FOR "GetText"
2、创建方法
uo_string str
GetText(handle(w_demo),str.GetAddress(),255)
messagebox('地址',str.GetAddress())
MessageBox("窗口",str.toString())
```

### 6.2、大文本处理

#### 6.2.1、按行读取

```c
long i,ll_lineCount
uo_string str
uo_string line
//通过文件构造字符串
str.ReadFile("json11.txt")
//计算总行数
ll_lineCount = str.GetLineCount()
for i = 1 to ll_lineCount
 line.format("{:<4} {}",i,str.GetLine(i))
 messagebox('',line.toString())
next
```

#### 6.2.2、一次性读取

```c
uo_string str
//读取ansi格式的文件
if str.readFile("text_ansi.txt") then
 MessageBox("",str.toString())
end if
//读取utf8格式的文件
if str.readFile("text_utf8.txt") then
 MessageBox("",str.toString())
end if
//读取utf16格式的文件
if str.readFile("text_utf16.txt") then
 MessageBox("",str.toString())
end if
```

#### 6.2.3、文件写入

```c
uo_string str
//写入ansi
str.FromString("我是ansi文件内容")
str.WriteFile("temp1.txt")
//写入utf8
str.FromString("我是utf8文件内容")
str.WriteFile("temp2.txt",true)
```

### 6.3、正则表达式处理

#### 6.3.1、搜索

```c
uo_string str
string pat,ls_msg
string ls_items[]
long ll_pos[],ll_len[] 
long i
str.FromString("ip:192.168.2.1")
pat = "(\d+).(\d+).(\d+).(\d+)"
//1、正则查询
if str.regexSearch(1,pat) then
 messagebox('1','成功!')
end if
//2、正则查询,并且将查到的项目放到 ls_items 数组里面
if str.regexSearch(1,pat,ref ls_items[]) then
 messagebox('2','成功!')
end if
//3、正则查询，（查询的起始位置，2、正则表达式，3、返回的项目数组，4、返回当前项目起始位置，5、返回当前项目的字串长度）
if str.regexSearch(1,pat,ref ls_items[],ref ll_pos[],ref ll_len[]) then
 for i = 1 to UpperBound(ls_items)
  ls_msg = ls_msg + ls_items[i] + " , ~r~npos : " + string(ll_pos[i]) + " ,~r~n len : " + string(ll_len[i]) + "~r~n"
 next
 MessageBox("3",ls_msg)
end if
```

#### 6.3.2、匹配

```c
uo_string str
string pat,ls_msg
string ls_items[]
long ll_pos[],ll_len[] 
long i 
str.FromString("192.168.2.1")
pat = "(\d+).(\d+).(\d+).(\d+)" 
//1、正则匹配
if str.regexMatch(pat) then
 messagebox('1','成功!')
end if
//2、正则匹配,并且将查到的项目放到 ls_items 数组里面
if str.regexMatch(pat,ref ls_items[]) then
 messagebox('2','成功!')
end if
//3、正则匹配，（查询的起始位置，2、正则表达式，3、返回的项目数组，4、返回当前项目起始位置，5、返回当前项目的字串长度）
if str.regexMatch(pat,ref ls_items[],ref ll_pos[],ref ll_len[]) then
 for i = 1 to UpperBound(ls_items)
  ls_msg = ls_msg + ls_items[i] + " , ~r~npos : " + string(ll_pos[i]) + " ,~r~n len : " + string(ll_len[i]) + "~r~n"
 next
 MessageBox("3",ls_msg)
end if
```

#### 6.3.3、替换

```c
uo_string str
string pat,ls_msg
//1、把 "2020/06/16" 替换成 "16-06-2020"
str.FromString("2020/06/16")
pat = "(\d+)/(\d+)/(\d+)"
ls_msg = str.regexReplace(pat,"$3-$2-$1")
MessageBox("1",ls_msg)
//2、把 "mr110.li" 替换成 "李先生"
str.FromString("hello mr110.li")
pat = "(mr\d+\.li)"
ls_msg = str.regexReplace(pat,"李先生") 
MessageBox("2",ls_msg)
```