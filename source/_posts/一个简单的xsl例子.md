title: 一个简单的xsl例子
date: 2014-12-01 12:25:30
tags: xml
---
##xsl是什么
---
XSL 之于 XML ,就像 CSS 之于 HTML。它是指可扩展样式表语言 (EXtensible Stylesheet Language)。这是一种用于以可读格式呈现 XML 数据的语言。XSL 实际上包含两个部分：
 - XSLT – 用于转换 XML 文档的语言
 - XPath – 用于在 XML 文档中导航的语言
XSLT 可以将 XML 文档转换为其它 XML 文档、XHTML 输出或简单的文本。这通常是通过将每个 XML 元素转换为 HTML 元素来完成的。由于 XML  标签是用户定义的，浏览器不知道如何解释或呈现每个标签，因此必须使用 XSL。XML 标签的意义是为了方便用户（而不是计算机）理解。
##xsl应用实例
---
xml源文件
```
<?xml version="1.0" encoding="UTF-8"?>  
<?xml-stylesheet type="text/xsl" href="cdcatalog.xsl"?>  
<catalog>  
 <cd>  
  <title>后来</title>  
  <artist>刘若英</artist>  
  <country>台湾</country>  
  <company>不知道</company>  
  <price>1090</price>  
  <year>1985</year>  
 </cd>  
 <cd>  
  <title>恶狼传说</title>  
  <artist>歌神</artist>  
  <country>香港</country>  
  <company>不知道</company>  
  <price>9900</price>  
  <year>1988</year>  
 </cd>  
 <cd>  
  <title>mylove</title>  
  <artist>西城男孩</artist>  
  
  <country>USA</country>  
  <company>do not know</company>  
  <price>990</price>  
  <year>1982</year>  
 </cd>  
 <cd>  
  <title>红豆</title>  
  <artist>王菲</artist>  
  <country>中国</country>  
  <company>华宜</company>  
  <price>1020</price>  
  <year>1990</year>  
 </cd>  
 <cd>  
  <title>the day you went away</title>  
  <artist>w3m</artist>  
  <country>am</country>  
  <company>Pickwick</company>  
  <price>8.50</price>  
  <year>1990</year>  
 </cd>  
</catalog>  
```
上面是一个xml文件，定义了一些标签，并且是有层次结构。下面就是cdcatalog.xsl文件
```
<?xml version="1.0" encoding="UTF-8"?>  
<xsl:stylesheet version="1.0" xmlns:xsl="<a href="http://www.w3.org/1999/XSL/Transform">http://www.w3.org/1999/XSL/Transform</a>">   <!--调用w3c标准-->  
<xsl:output method='html' version='1.0' encoding='UTF-8' indent='yes'/>       <!--定义输出为html-->  
<xsl:template match="/">      <!--  match="/" 定义当前的整个文档，在这里就是上面的xml文件-->
  <html>  
  <body>  
  <h2>我的收藏</h2>  
    <table border="1">  
      <tr bgcolor="#9acd32">  
        <th align="left">作品名</th>  
        <th align="left">作者名</th>  
      </tr>  
      <xsl:for-each select="catalog/cd">  <!--取得当前文档中的catalog标签中cd子标签中的的值，for-each根php的foreach类似-->  
      <xsl:sort select="price"/>          <!--按照price，进行升序排序，price标签在cd下面  -->
      <xsl:if test="price &gt; 900">      <!--显示出price 大于900的数据，你可把它当做php的if-->
      <tr>  
        <td><xsl:value-of select="title"/></td>   <!--提取出title的值，value-of-->
       <xsl:choose>             <!--choose when 你可以把它当做switch case来理解-->
          <xsl:when test="price &gt; 1900">  
            <td bgcolor="#ff00ff">  
            <xsl:value-of select="artist"/></td>  
          </xsl:when>  
          <xsl:otherwise>  
            <td><xsl:value-of select="artist"/></td>  
          </xsl:otherwise>  
        </xsl:choose>  
      </tr>  
      </xsl:if>  
      </xsl:for-each>  
    </table>  
 <xsl:apply-templates/>          <!--添加节点-->  
  </body>  
  </html>  
</xsl:template>  
<xsl:template match="cd">      <!-- match="cd" 定义当前文档在cd标签下-->  
 <p>  
  <xsl:apply-templates select="title"/>  
  <xsl:apply-templates select="artist"/>  
 </p>  
</xsl:template>  
 <xsl:template match="title">   <!--match="title" 定义当前文档在title标签下-->
  
 Title: <span style="color:#ff0000">  
 <xsl:value-of select="."/></span>  
 <br />  
</xsl:template>  
 <xsl:template match="artist">  
 Artist: <span style="color:#00ff00">  
 <xsl:value-of select="."/></span>  
<br />  
</xsl:template>  
</xsl:stylesheet>  
```
页面访问一下http://localhost/test/cdcatalog.xml可以看到xml中的数据。