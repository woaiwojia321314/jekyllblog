---
layout: post
title: markdown的基本语法使用
category: frontend
tags: [markdown, jekyll]
---
markdown的基本语法
# 标题一 h1

## 标题二 h2

### 标题三 h3

#### 标题四 h4

##### 标题五 h5

###### 标题六 h6

*斜体*

**粗体**

> 引用的文字内容
第二行不必再写>

1. 有序列表
2. 有序列表
3. 有序列表

+ 无序列表
+ 无序列表
+ 无序列表

- 无序列表
- 无序列表
- 无序列表

* 无序列表
* 无序列表
* 无序列表

[baidu链接](http://www.baidu.com)

![图片名称]({{ site.url }}/images/common/logo.png)

简单超链接：
<http://example.com/>
<lifeidejiayuan@126.com>

`layout行内代码`

{% highlight ruby linenos %}
function handleSearcher() {
    代码块
    new Promise((resolve, reject) => {
        let searchVal = searchText.value.toLowerCase().trim()
        let res = posts.filter((item) => {
            return (item.title.toLowerCase().indexOf(searchVal) > 0) || (item.url.toLowerCase().indexOf(searchVal) > 0)
        })
        resolve(res)
    })
}
{% endhighlight %}

{% capture text %}...
<body>
  <div id="sidebar"> ... </div>
  <div id="main">
    |.{content}.|
  </div>
</body>
...{% endcapture %}

分割线：

***

---

