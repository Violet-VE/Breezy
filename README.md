### 书源教程

原理是对在线阅读的小说网站进行转码阅读，所以需要了解HTML/CSS/JavaScript基础内容，以及Jsoup选择器的功能。

参考内容：[Jsoup选择器文档](https://jsoup.org/apidocs/org/jsoup/select/Selector.html) 和 [Jsoup调试](https://try.jsoup.org)

##### 1. 选择网站

教程以 [纵横中文网](http://www.zongheng.com) 为例讲解。

##### 2. 基础信息

根据源网站信息创建书源文件：**纵横中文网.json**

```json
{
    "name": "纵横中文网",
    "version": 100,
    "category": 1,
    "url": "http://www.zongheng.com",
    "charset": "utf-8"
}
```

属性讲解

| 属性       | 含义  | 讲解                                        |
|:--------:|:---:| ----------------------------------------- |
| name     | 名字  | 源网站名字                                     |
| version  | 版本  | 默认100（1.0）当内容变化时递增，如101（1.1）              |
| category | 类别  | 0（出版）、1（网文）、2（轻小说）、3（综合）、4（拓展）            |
| url      | 网站  | 源网站链接                                     |
| charset  | 字符  | utf-8 gbk gbk2312 影响search等请求时的编码，非源网站的编码 |

##### 3. 搜索

在对应源网站中的搜索框进行搜索，并根据源码的内容，填充如下：

```json
{
    ...第2步：基础信息
    "search": {
        "link": "http://search.zongheng.com/s?keyword=${key}",
        "list": ".search-result-list"
    }
}
```

属性讲解

| 属性   | 含义  | 讲解                           |
|:----:|:---:| ---------------------------- |
| link | 地址  | ${key}代表搜索关键词，搜索时自动替换为用户输入的词 |
| list | 结果  | 通过Jsoup选择结果元素                |

上例中的搜索是GET方式请求，若源网站是POST方式请求时，使用@post->声明需要传递的参数，写法如下：

```json
{
    ...第2步：基础信息
    "search": {
        "link": "http://search.zongheng.com/s@post->keyword=${key}",
        "list": ".search-result-list"
    }
 }
```

其中list字段根据搜索结果列表得出，源网站搜索结果如下（删减无用元素），可以看出每一个搜索结果（*div class="search-result-list clearfix"*）都带有相同的class（*search-result-list*），根据Jsoup语法 **.search-result-list** 选择全部带有search-result-list的div元素。

```html
<!-- 搜索主内容 -->
<div class="search-html-box clearfix">
    <div class="search-main fl">
        <div class="search-tab">
            <div class="search-result-list clearfix">...</div>
            <div class="search-result-list clearfix">...</div>
            <div class="search-result-list clearfix">...</div>
            <div class="search-result-list clearfix">...</div>
            <div class="search-result-list clearfix">...</div>
        </div>
    </div>
</div>
<!-- end 搜索主内容 -->
```


