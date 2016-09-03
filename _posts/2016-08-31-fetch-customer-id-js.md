---
layout: post
title: js获取客户端真实IP
category: frontend
tags: [js]
---
java获取客户端程序本身还有对应的对象来实现。而js却不行。搜罗了一些资料，发现比较全而好的前端获取客户端IP的方法基本都是通过三方接口。也就是调用别人写好的接口。用浏览器已有的控件ActiveXObject的控件方式，有一定的兼容性问题。

## 方法一：（所有的平台及浏览器）
使用搜狐接口：

{% highlight ruby %}

<script src="http://pv.sohu.com/cityjson?ie=utf-8"></script>
<script type="text/javascript">
document.write(returnCitySN["cip"]+','+returnCitySN["cname"])
</script>

{% endhighlight %}

## 方法二：（所有的平台及浏览器）
使用新浪接口：(我测试过，此方法好像不行。访问网站也不能访问了。)

{% highlight ruby %}

<script type="text/javascript" src="http://counter.sina.com.cn/ip/" charset="gb2312"></script>       <!--获取接口数据，注意charset -->
<script type="text/javascript">
document.writeln("IP地址："+ILData[0]+"<br />");             //输出接口数据中的IP地址
document.writeln("地址类型："+ILData[1]+"<br />");         //输出接口数据中的IP地址的类型
document.writeln("地址类型："+ILData[2]+"<br />");         //输出接口数据中的IP地址的省市
document.writeln("地址类型："+ILData[3]+"<br />");         //输出接口数据中的IP地址的
document.writeln("地址类型："+ILData[4]+"<br />");         //输出接口数据中的IP地址的运营商
</script>

{% endhighlight %}

## 方法三：
个人Robert Hashemian写的：

{% highlight ruby %}

<script language="JavaScript" src="http://www.hashemian.com/js/visitorIP.js.php"></script>
<script language="JavaScript">
    VIH_BackColor = "palegreen";
    VIH_ForeColor = "navy";
    VIH_FontPix = "16";
    VIH_DisplayFormat = "You are visiting from:<br>IP Address: %%IP%%<br>Host: %%HOST%%";
    VIH_DisplayOnPage = "yes";
</script>

{% endhighlight %}

## 方法四：（只针对IE且客户端的IE允许AcitiveX运行，通过平台：XP，SERVER03，2000）。
利用ActiveXObject控件，ie浏览器里要开启此控件：

{% highlight ruby %}

<script language="JavaScript">
function GetLocalIPAddr(){ var oSetting = null; var ip = null; try{ oSetting = new ActiveXObject("rcbdyctl.Setting"); ip = oSetting.GetIPAddress; if (ip.length == 0){ return "没有连接到Internet"; } oSetting = null; }catch(e){ return ip; } return ip; } document.write(GetLocalIPAddr()+"<br/>")
</script>

{% endhighlight %}

### 参考
* [JS获取客户端IP地址、MAC和主机名的7个方法汇总](http://www.jb51.net/article/52484.htm)
* [js获取客户端外网ip的简单实例](http://www.jb51.net/article/43586.htm)