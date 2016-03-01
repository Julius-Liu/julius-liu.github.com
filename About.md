---
layout: page
title: "About"
description: "嘿，你总算找到我啦"
header-img: "img/plane.jpg"
---

{% for post in site.posts %}
  {% capture post_count %}
    {{ post_count | plus: 1 }}
  {% endcapture %}
{% endfor %}

<center>
  <img src="/images/breaking-bad.png" align="center">
  <b>成立于2016年1月11日</b>
  <br/>
  <b>文章总数：{{post_count}} 篇</b>  
</center>

## Tags ##

*	计算机爱好者
*	文艺小清新
*	喜欢[5sing](http://5sing.kugou.com)，喜欢中国风
*	爱看小说，特别爱看武侠
*	笑点低
*	推理电影控
*	最大的心愿：还原生活的本来面貌