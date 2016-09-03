---
layout: post
title: java获取客户端真实IP（转载）
category: backdev
tags: [java]
---
有时候希望记录用户登录ip，用于获取他的登录地址。网上搜了很多资料，发现常用的1种方式比较全面而实用的方式：request获取。

转载自[**Java获取客户端IP**](http://www.cnblogs.com/ITtangtang/p/3927768.html)

## request获取方式

在开发工作中,我们常常需要获取客户端的IP。一般获取客户端的IP地址的方法是: **request.getRemoteAddr()** ;但是在通过了Apache,Squid等反向代理软件就不能获取到客户端的真实IP地址了。

原因:

由于在客户端和服务之间增加了中间代理,因此服务器无法直接拿到客户端的IP,服务器端应用也无法直接通过转发请求的地址返回给客户端。

**现在图示代理上网和IP的关系：**

*第一种情况：不通过代理上网,服务器端拿到真实IP*

![fetch-customer-id-01]({{ site.url }}/images/fetch-customer-id-01.png)

*第二种情况：通过代理服务器如:Nginx,Squid等一层代理或多层代理上网，如下图:*

![fetch-customer-id-02]({{ site.url }}/images/fetch-customer-id-02.png)

需要注意的是X-Forwarded-For和X-Real-IP都不是http的正式协议头，而是squid等反向代理软件最早引入的，之所以resin能拿到，是因为NGINX里一般缺省都会这么配置转发的http请求：

* **proxy_pass       http://yourdomain.com;**

* **proxy_set_header   Host             $host;**

* **proxy_set_header   X-Real-IP        $remote_addr;**

* **proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;**

 从X-Forwarded-For的定义来看，ips[0]才是原始客户端ip，如果这个都不是，那拿第二个就更不靠谱了，我们平时检验的时候，可能是直接在内网挂代理去访问的，跟外面网友访问经过的网络路径不一样，后面不停添加的是经过的每一层代理ip才对,下面举例说明;

* **request.getRemoteAddr() 192.168.239.196**

* **request.getHeader("X-Forwarded-For") 58.63.227.162, 192.168.237.178, 192.168.238.218**

* **request.getHeader("X-Real-IP") 192.168.238.218**

所以访问的流程应该是这样，客户端58.63.227.162发出请求，经过192.168.237.178, 192.168.238.218两层转发，到了192.168.239.196这台NGINX上，NGINX就把X-Real-IP头设成了自己看到的remote_addr，也就是直接发给到他的192.168.238.218，这时候resin收到这个包，对resin来说直接发给他的remote_addr就是NGINX的ip，也就是192.168.239.196，那么resin里面的request.getRemoteAddr()就是192.168.239.196，那么在resin里拿最原始的ip逻辑（也就是拿能够知道的最外层的ip）应该是这样：

1. **如果XFF不为空，拿XFF的左边第一个**

1. **如果XFF为空，拿XRI**

1. **如果XRI为空，只能拿request.getRemoteAddr()，也就是只能拿到最直接发给他的机器ip了，**

其他都不可考究,参考代码如下(已经封装成一个工具类)：

{% highlight ruby %}

package com.le.util;

import javax.servlet.http.HttpServletRequest;

public class IPFetch {

	/**
	 * fetch customer real ip
	 * @param request
	 * @return string
	 */
	public static String getRealIp(HttpServletRequest request) {

        String ip = request.getHeader("X-Forwarded-For");

        if(isNotEmpty(ip) && !"unKnown".equalsIgnoreCase(ip)){
            //多次反向代理后会有多个ip值，第一个ip才是真实ip
            int index = ip.indexOf(",");
            if(index != -1){
                return ip.substring(0,index);
            }else{
                return ip;
            }
        }
        ip = request.getHeader("X-Real-IP");
        if(isNotEmpty(ip) && !"unKnown".equalsIgnoreCase(ip)){
            return ip;
        }
        return request.getRemoteAddr();
    }//method

	/**
	 * Check whether string is empty
	 * @param str String
	 * @return boolean
	 */
	public static boolean isNotEmpty(String str) {
		return !(str == null || str.length() == 0);
	}//method

}

{% endhighlight %}




