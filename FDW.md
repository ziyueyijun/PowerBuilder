# FDW

## 基础

先从网上下载一个html网站模板，放到fdw\res\html目录下，记得要先把之前的页面删除

假设你下载的html模板默认首页是index.html。然后我们用PB打开fdw中的demo.pbl，打开w_login下的open事件

将以下代码中的Login.html

```c
ln_hs.of_init(this,luo_mb,"fdw2://fdw/res/html/logon.html","fdw2.zip","fdw2demo")
```

改为index.html

```c
ln_hs.of_init(this,luo_mb,"fdw2://fdw/res/html/index.html","fdw2.zip","fdw2demo")
```

然后运行程序，你就可以看到一个网页版的PB程序了。

**问题1：为什么w_logn窗口里什么都没有？我这个事件应该写到哪里？应该怎么写呢？**

首先我们打开fdw自带的login.html中，可以看到登录按钮调用了onclick点击事件，调用的是pb中的logon事件

```html
 <button type="button" class="layui-btn layui-btn-normal" id="login" onclick="logon();">登录</button>
```

然后右键w_logon这个窗口，点击Edit Source查看PB的源码，你会看到这样的一段话

```c
event ln_hs::ue_action3;call super::ue_action3;if as_id = "logon" then
   g_global.gs_ryh = "admin"  //3 个参数分别是登录号，姓名，密码，需要自己根据数据库记录情况判断
   return "success"
end if
return ""
end event
```

这个就JS中调用的logon事件，他们之间的关系是这样的

ln_hs就是n_htmlservice这个类

ue_action3 就是n_htmlservice这个类下面的方法

ue_action3的参数（

1、 as_id （事件id）

2、as_arg1  （参数1）

3、as_arg2  （参数2）

4、as_arg3  （参数3）

）

根据login.html我们可以知道事件id是logon，而那3个参数分别是（登录名）、（姓名）、（密码）



**问题2：那我应该怎么判断登录名或密码是否正确呢？**

对于PB老手来说，看到上面的代码，几乎就明白是怎么回事了。但对于PB新手来说，此时已经晕了

其实也很简单，仔细再看一下刚才这句话

```c
event ln_hs::ue_action3;call super::ue_action3;if as_id = "logon" then
```

你会发现as_id = 'logon'，这个as_id正好是n_htmlservice类下面的ue_action3方法中的第一个参数，所以如果你想获取其他两个参数，直接调用就可以了

可以改成这样

```c
event ln_hs::ue_action3;call super::ue_action3;if as_id = "logon" and as_arg1 =  'admin'then
```

或者直接在代码里面判断

```c
event ln_hs::ue_action3;call super::ue_action3;if as_id = "logon" then

   if as_arg1 =  ‘admin’ then

     return "success"

  esle

    return “error”

   end if

end event
```



**问题3：这个as_arg1和as_arg2和arg3我明白了，但是这个登录名、姓名、密码是怎么传过来的？**

现在我们继续打开login.html，可以看到JS中有这样一段话

```javascript
function logon() {
var ret = pb.invoke("w_logon.ln_hs.ue_action3()","logon", document.getElementById("name").value, 		  document.getElementById("loginname").value, document.getElementById("psd").value);
     if (ret == "success") {
        layer.msg('登录成功', { icon: 1 }, function () {
         pb.invoke("w_logon.ln_hs.ue_action()","open");
       });
      }
      else
       layer.msg('登录失败', { icon: 1 });
}
```

```c
pb.invoke（函数或事件，函数或事件的参数1，函数或事件的参数2，函数或事件的参数3，函数或事件的参数4....）
```

JS中获取id=name的值，也就是登录名

```javascript
document.getElementById("name").value
```

JS中获取id=loginname,也就是登录姓名

```javascript
document.getElementById("loginname").value
```

JS中获取id=psd，也就是登录密码

```javascript
document.getElementById("psd").value
```

这样分析一下，是不是瞬间就明白了呢？如果还不明白，就先去学JS吧

## PB 调用 JS

### PB 调用 JS 函数

1、在JS中创建func_test函数

```javascript
<script>
 func_test(a,b){
  alert(a+b)
 } 
<script>
```

2、在PB调用js中的func_test方法

```c
uo_1.mbRunJS("func_test('1','2');")
```

### PB  调用 JS 的 DOM 对象

```c
uo_1.mbRunJS("document.getElementById('id').style.backgroundColor='red';)
```

## JS 调用 PB

**例1：**

1、先在PB中绑定一个事件

```c
uo_1.mbbind(w_main,’wf_test’ )   //事件或函数名称，
```

然后在此事件中写个弹窗测试一下

```c
messagebox('提示','我是wf_test事件')
```

2、然后直接在JS中调用此窗口下的事件

```javascript
<script>

 Windows.wf_test

</script>
```

**例2：**

1、直接调用PB中的wf_test事件

```html
<input id = "quitpb" type = "button" value=  "弹窗测试"  onclick="javascript:pb.invoke('w_main.wf_test()');" />
```

2、直接调用PB中的wf_test事件，并传参数

```html
<input id = "quitpb" type = "button" value=  "弹窗测试"  onclick="javascript:pb.invoke('w_main.wf_test()','参数1','参数2');"  />
```

3、直接调用全局函数gf_test，并传递1个参数

```c
pb.invoke('gf_test()','hello');
```

4、触发窗口下的用户对象中的事件，并传递1个参数

```c
 pb.invoke("w_main.ue_test()","参数1");
```

5、直接调用窗口属性，获取窗口标题

```html
 <input id = "quitpb" type="button" value= "title"  onclick="javascript:alert(pb.get('w_test.title');" />
```

6、 直接修改窗口属性，修改窗口标题

```html
<input id = "quitpb" type="button" value = "title"  onclick= "javascript:pb.set('w_page.title','我是新的窗口标题';" />
```

**例3：**

PB控件与HTML通过DIV混合布局

1、先在html中创建div和embed标签，embed标签的type属性必须是application/x-fdw

```html
<div id="main" style="z-index: 1;">        
<embed id= "test" type=  "application/x-fdw"/>
</div>
```

2、 通过JS代码将PB控件绑定到指定embed标签上（以下示例是将 w_main窗口下的dw_1数据窗口放到了embed标签）

```c
var plugin=document.getElementById('test');
plugin.attachControl(pb.w_main.dw_1);
```

3、或者在PB中，直接将控件绑定到embed标签

```c
uo_1.mbAttachControl(dw_1,"test")
```