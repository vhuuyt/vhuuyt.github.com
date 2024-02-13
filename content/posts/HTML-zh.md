---
title: "HTML入门笔记（暂鸽中）"
date: "2022-03-23"
tags: ["编程"]
---

> HTML 课程：[mozilla](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Getting_started)

# HTML文档剖析(anotomy)

```html
<!DOCTYPE html><!--最短有效的文档声明-->
<html>
  <head><!--容器container，不会外显，内容包括你想在搜索结果中出现的关键字和页面描述，CSS样式，字符集声明等-->
    <meta charset="utf-8"><!--metadata(元数据)表示无法由其他 HTML 元相关元素（如 <base>、<link>、<script>、<style>或<title>）表示的元数据。字符集属性将文档的字符集设置为 UTF-8-->
    <title>标题</title><!--出现在标签tab页、或作为书签bookmarked的标题-->
  </head>
  <body>
      <!--展示在页面的所有内容：文图、视频、图像、游戏、可播放的音轨等-->
    <p>段落</p>
  </body>
</html>
```

## 主要语言 primary language

全局设置

```html
<html lang="en-US">
```

局部设置

```html
<p>Japanese example: <span lang="ja">ご飯が熱い。</span>.</p>
```

标准来自： [ISO 639-1](https://en.wikipedia.org/wiki/ISO_639-1)	更多查看： [Language tags in HTML and XML](https://www.w3.org/International/articles/language-tags/).

## &lt;head&gt; 里有啥

### &lt;title&gt; 标题

```html
<title>My test page</title>
```

 `<title>`用于搜索结果、标签页描述和默认书签名

### &lt;meta&gt; 元数据

#### 字符编码

```html
<meta charset="utf-8">
```

#### 作者和描述

```html
<meta name="specifies the type of meta element it is; what type of information it contains.">
<meta content="specifies the actual meta content.">
```

例如：

```html
<meta name="author" content="Chris Mills">
<meta name="description" content="The MDN Web Docs Learning Area aims to providecomplete beginners to the Web with all they need to know to get started with developing web sites and applications.">
```

**Note:** In Google, you will see some relevant subpages of MDN Web Docs listed below the main homepage link — these are called sitelinks, and are configurable in [Google's webmaster tools](https://www.google.com/webmasters/tools/) — a way to make your site's search results better in the Google search engine.

**Note:** Many `<meta>` features just aren't used any more. For example, the keyword `<meta>` element (`<meta name="keywords" content="fill, in, your, keywords, here">`) — which is supposed to provide keywords for search engines to determine relevance of that page for different search terms — is ignored by search engines, because spammers were just filling the keyword list with hundreds of keywords, biasing results.

#### 外链优化显示 proprietary metadata

For example, [Open Graph Data](https://ogp.me/) is a metadata protocol that Facebook invented to provide richer metadata for websites. In the MDN Web Docs sourcecode, you'll find this:

```html
<meta property="og:image" content="https://developer.mozilla.org/static/img/opengraph-logo.png">
<meta property="og:description" content="The Mozilla Developer Network (MDN) provides
information about Open Web technologies including HTML, CSS, and APIs for both Web sites
and HTML5 Apps. It also documents Mozilla products, like Firefox OS.">
<meta property="og:title" content="Mozilla Developer Network">
```

One effect of this is that when you link to MDN Web Docs on facebook, the link appears along with an image and description: a richer experience for users.

### custom icons

You may see (depending on the browser) favicons(favorites icon) displayed in the browser tab containing each open page, and next to bookmarked pages in the bookmarks panel.

```html
<link rel="icon" href="favicon.ico" type="image/x-icon">
```

```html
<!-- third-generation iPad with high-resolution Retina display: -->
<link rel="apple-touch-icon-precomposed" sizes="144x144" href="https://developer.mozilla.org/static/img/favicon144.png">
<!-- iPhone with high-resolution Retina display: -->
<link rel="apple-touch-icon-precomposed" sizes="114x114" href="https://developer.mozilla.org/static/img/favicon114.png">
<!-- basic favicon -->
<link rel="icon" href="https://developer.mozilla.org/static/img/favicon32.png">
```

The comments explain what each icon is used for — these elements cover things like providing a nice high resolution icon to use when the website is saved to an iPad's home screen.

**Note:** If your site uses a Content Security Policy (CSP) to enhance its security, the policy applies to the favicon. If you encounter problems with the favicon not loading, verify that the [`Content-Security-Policy`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy) header's [`img-src` directive](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/img-src) is not preventing access to it.

### Applying CSS and JavaScript to HTML

#### CSS

```html
<link rel="stylesheet" href="my-css-file.css">
```

The [`<link>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link) element should always go inside the head of your document. This takes two attributes, `rel="stylesheet"`, which indicates that it is the document's stylesheet, and `href`.

#### JavaScript

##### 放头部

```html
<script src="my-js-file.js" defer></script>
```

The [`<script>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script) element should also go into the head, and should include a `src` attribute containing the path to the JavaScript you want to load, and `defer`, which basically instructs the browser to load the JavaScript after the page has finished parsing the HTML. This is useful as it makes sure that the HTML is all loaded before the JavaScript runs, so that you don't get errors resulting from JavaScript trying to access an HTML element that doesn't exist on the page yet. 

There are actually a number of ways to handle loading JavaScript on your page, but this is the most reliable one to use for modern browsers (for others, read [Script loading strategies](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/What_is_JavaScript#script_loading_strategies)).

##### 放尾部

`<script>` 部分没必要非要放在文档头部；实际上，把它放在文档的尾部（在 `</body>`标签之前）是一个更好的选择，这样可以确保在加载脚本之前浏览器已经解析了HTML内容（如果脚本加载某个不存在的元素，浏览器会报错）。

```html
<script src="my-js-file.js"></script>
```

**Note:** The `<script>` element may look like an empty element, but it's not, and so needs a closing tag. Instead of pointing to an external script file, you can also choose to put your script inside the `<script>` element.

# Element

The terms *block* and *inline*, as used in this article, should not be confused with [the types of CSS boxes](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/The_box_model#types_of_css_boxes) that have the same names. While the names correlate by default, changing the CSS display type doesn't change the category of the element, and doesn't affect which elements it can contain and which elements it can be contained in. One reason HTML5 dropped these terms was to prevent this rather common confusion.

## block-level element 块级元素

it can not be nested inside an inline element ! It will cause a new line to appear in the document. (default CSS styling)

```html
<h1>I am the title of the story.</h1><!--支持到<h6>-->
<p>I am a paragraph, oh yes I am.</p>

<ul>
  <li>unordered lists</li>
    <ol>
      <li>ordered lists</li>
      <li>nesting lists</li>    
    </ol>
  <li>unordered lists</li>
</ul>
```

## inline element 内联元素

### 简述

They are surround only small parts of the document's content. It will not cause a new line to appear in the document.

```html
<a>anchor: 锚，使成为超链接 hyper link</a>
<strong>非常重要 strong importance</strong>
<em>强调 emphasis</em>
```

### 详述

```html
<em>emphasis: Browsers style this as italic by default, but you shouldn't use this tag purely to get italic styling.</em>
To do that, you'd use a <span> element and some CSS, or perhaps an <i> element.

<p>Browsers style <strong>strong importance</strong> as bold text by default.</p>
but you shouldn't use this tag purely to get bold styling. To do that, you'd use a <span> element and some CSS, or perhaps a <b> element.
    
You can nest strong and emphasis inside one another if desired:
<p>This liquid is <strong>highly toxic</strong> —
if you drink it, <strong>you may <em>die</em></strong>.</p>
```

#### italic, bold, underline...

They came about so people could write bold, italics, or underlined text in an era when CSS was still supported poorly or not at all. Elements like this, which only affect presentation and not semantics, are known as **presentational elements** and should no longer be used because, as we've seen before, semantics is so important to accessibility, SEO(搜索引擎优化), etc.

- [`<i>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/i) 被用来传达传统上用斜体表达的意义：外国文字，分类名称，技术术语，一种思想……
	is used to convey a meaning traditionally conveyed by italic: foreign words, taxonomic designation, technical terms, a thought...
- [`<b>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/b) 被用来传达传统上用粗体表达的意义：关键字，产品名称，引导句……
	is used to convey a meaning traditionally conveyed by bold: key words, product names, lead sentence...
- [`<u>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/u) 被用来传达传统上用下划线表达的意义：专有名词，拼写错误……
	is used to convey a meaning traditionally conveyed by underline: proper name, misspelling...

**Note:** People strongly associate underlining with hyperlinks. Therefore, on the web, it's best to underline only links. Use the `<u>` element when it's semantically appropriate, but consider using CSS to change the default underline to something more appropriate on the web. The example below illustrates how it can be done.

```html
<!-- scientific names -->
<p>
  The Ruby-throated Hummingbird (<i>Archilochus colubris</i>)
  is the most common hummingbird in Eastern North America.
</p>

<!-- foreign words -->
<p>
  The menu was a sea of exotic words like <i lang="uk-latn">vatrushka</i>,
  <i lang="id">nasi goreng</i> and <i lang="fr">soupe à l'oignon</i>.
</p>

<!-- a known misspelling -->
<p><!--看这里的格式-->
  Someday I'll learn how to <u style="text-decoration-line: underline; text-decoration-style: wavy;">spel</u> better.
</p>

<!-- Highlight keywords in a set of instructions -->
<ol>
  <li>
    <b>Slice</b> two pieces of bread off the loaf.
  </li>
  <li>
    <b>Insert</b> a tomato slice and a leaf of
    lettuce between the slices of bread.
  </li>
</ol>
```





## Empty elements 空元素只有 Single Tag

```html
<img src="https://.png" alt="alter:更改、改变">
<img src="images/cat.jpg" alt="cat" /> 后面加/(backslash)来适配XML
```

# Attributes 属性

Elements can alse have attributes. 属性始终要**添加引号**，单双都行，但不能混用，双引号里的单引号可以显示出来（反之同理）。如果要把同类型引号当作文本显示，得使用[实体引用 HTML entities](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Getting_started#entity_references_including_special_characters_in_html):`<a href='https://www.example.com' title='Isn&apos;t this fun?'>A link to my example.</a>`

```html
class: <p class="editor-nots"></p>
```

## anchors 可添加的 attributes

```html
<a href="web address"></a> here reference somewhere?
<a title=""></a> description of the page being linked to. when cursor(鼠标) hovers(盘旋、彷徨、靠近), it appears.
<a target="_blank"></a> : open in a new tags(标签页)
```

## Boolean attributes 布尔属性

布尔属性的值和属性名称是一样的

```html
<input type="text" disabled="disabled">标记表单输入使之变为不可用(变灰色)
<input type="text" disabled><!-- using the disabled attribute prevents the end user from entering text into the input box -->
<input type="text"><!-- text input is allowed, as it doesn't contain the disabled attribute -->
```

# syntax 规则

```html
nesting: elements can be placed within other elements
<p>
    This is called <strong>nesting(嵌套)</strong>
</p>
overlap 交叠：<p><strong></p></p><strong>
```

## Witespace in HTML

the HTML parser reduces each sequence of whitespace to a single space when rendering the code.

```html
<p>Dogs are silly.</p>
<p>Dogs        are
         silly.</p>
```

## Entity references 实体引用

`<`less than,`>`,`"`,`'`,`&`ampersand他们都是特殊字符，使用的时候需要特殊引用。Each character reference starts with an ampersand (&), and ends with a semicolon (;).

| Literal character <br />原义字符 | Character reference equivalent<br />等价字符引用 |
| :------------------------------: | :----------------------------------------------: |
|                <                 |                 &lt ; less than                  |
|                >                 |                      &gt ;                       |
|                "                 |                &quot ; quotation                 |
|                '                 |                     &apos ;                      |
|                &                 |                      &amp ;                      |

## HTML comments 注释

Browsers ignore comments, effectively making comments invisible to the user.

```html
<!--<p>I am!</p>This is very useful if you return to a code base after being away for long enough that you don't completely remember it.
Likewise, comments are invaluable as different people are making changes and updates.-->
```

