![image-20220418160631135](http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/image-20220418160631135.png)

## HTML4

**标记语言**

**元素 属性**



**基础：**标题 <h> 段落 <p> 连接 <a href=""> 图像 <img src="">

**文本格式化：**加粗 <b> 着重文字 <em> 斜体 <i> 小号字体 <small> 加重 <strong> 下标字 <sub> 上标字 <sup> 下划线 <ins> 删除字<del>

**属性：**class id style title

**连接：**

*  <a href="https://www.baidu.com" target ="_blank" rel="noopener noreferrer"></a>  target = "_blank" 会在新窗口打开

<a href="https://www.runoob.com/html/html-head.html">**头部：**</a>浏览器工作栏标题、收藏夹中标题、搜索引擎结果页面的标题

<img src="http://blog-shifty.oss-cn-shanghai.aliyuncs.com/uPic/image-20220418165205504.png" alt="image-20220418165205504" style="zoom: 50%;" />



* 基本链接地址<base href="">

* 文档与外部资源的关系，通常用于链接到样式表 <link rel="stylesheet" type="text/css" href="mystyle.css">

* 样式文件引用地址 <style type="text/css">

* 基本元素信息 

  * 搜索定义关键词 <meta name="keywords" content="HTML,CSS,XML,JavaScript">

  * 网页定义描述内容 <meta name="description" content="免费的web">
  * 网页定义作者 <meta name="author" content="yang">
  * 没30秒刷新此页面 <meta name="refresh" content="30">

* 加载脚本 <script>

**表格**：<table> <tr><td>

<table>
<tr>
  <td></td>
  <td></td>
</tr>
  <tr>
  <td></td>
  <td></td>
</tr>
</table>

**列表**：

* 无序 <ul><li>

  <ul>
  	<li></li> 
  </ul>

* 有序 <ol><li>

  <ol>
  	<li></li>
    <li></li>
  </ol>

* 自定义列表<dl><dt><dd>

  <dl>
  	  <dt>Coffe</dt>
    	<dd>black</dd>
    	<dt>milk</dt>
    	<dd>white</dd>
  </dl>

**区块**：块级 部分块设计样式<div> 行内元素 部分文本设计样式<span>

**表单**：<form><input type="text" name=""> type 密码：password 单选：radio 复选：checkbox 提交：submit

**框架：**<iframe src="url.html" loading="lazy" width="" height=""> 该url指向不同的网页

​			frameborder="0" 移除边框

​			<a href="http://www.runoob.com" target="iframe_a"> target是iframe_a，点击链接会显示在框架中

**脚本**：<script>document.write("Hello World!")

> **JS 常用功能：图片操作、表单验证、内容动态更新**

**字符实体**：空格 &nbsp；&lt；

<a href="https://www.runoob.com/html/html-quicklist.html" style="color:green">**速查表单**</a>



## HTML5

定义块元素

<script>document.createElement("myHero")
<style>myHero{display: block;...}

**新元素：** <canvas> 标签定义图形，绘制图形

**新多媒体元素：** 音频 <audio> 视屏<video> 多媒体资源 <source> 嵌入内容 插件 <embed> 为 video audio 规定外部文本轨道

**新表单元素**：定义选项列表 <datalist> 表单秘钥生成器字段 <keygen> 指定不同类型输出 <output>

**新语义和结构元素**：

**拖放**：<img draggable="true"> 

**Input类型**：<input type="date"> date color datetime datetime-local email month number range search tel time url week 

**表单元素**：datalist 元素  <datalist id="browsers"> <option value="">

​					keygen 元素 <form action="demo_keygen.asp" method="get"> <input type="test" name="usr_name"> <keygen name="security"> <input type="submit">

**output元素**：

**web存储**：localstorage sessionstorage

**应用程序缓存**：

* 三个优势：1. 离线浏览 2. 速度，已缓存资源加载更快 3， 减少服务器负载
* Cache Manifest 基础
* q