## CSS笔记

1. **关于DOCTYPE**

   DTD（文档类型定义）是一组机器可读的规则，它们定义XML或HTML的特定版本中允许有什么，不允许有什么。在解析网页时，浏览器将使用这些规则检查页面的**有效性**并采取相应的措施。浏览器通过分析页面的DOCTYPE声明来了解要使用哪个DTD，由此知道要使用HTML的哪个版本。

   * 对页面进行有效性的验证时要求有DOCTYPE声明。

   * 根据DOCTYPE是否存在选择浏览器呈现模式，这被称为DOCTYPE切换或DOCTYPE侦测。

     当浏览器厂商开始创建与标准兼容的浏览器时，他们希望确保向后兼容性。为了实现这一点，他们创建了两种呈现模式：标准模式和混杂模式。在标准模式中，浏览器根据规范呈现页面。在混杂模式中，页面以一种比较宽松的向后兼容的方式显示。DOCTYPE切换是浏览器用来区分遗留文档和符合标准文档的手段。DOCTYPE不存在或形式不正确会导致HTML和XHTML文档以混杂模式呈现。无论是否编写了有效地CSS，如果选了错误的DOCTYPE，那么页面就将以混杂模式呈现，其行为就可能会有错误或不可预测。

2. **选择器**

   要想使用CSS将样式应用于特定的HTML元素，需要想办法找到这个元素。在CSS中，执行这一任务的样式规则部分称为选择器。

   * 常用选择器：元素选择器、后代选择器（两个选择器之间使用空格表示）、ID选择器、类选择器。使用这4种选择器就可以成功找到许多元素。

   * 伪类：需要根据文档结构之外的其他条件对元素应用样式，例如表单元素或链接的状态，这要使用伪类选择器来完成。

     常见的伪类有：:link, :visited, :hover, :focus, :active

     :link和:visited成为链接伪类，只能应用于锚元素。:hover, :focus, :active称为动态伪类，理论上可以应用于任何元素。

     通过把伪类连接在一起，可以创建更复杂的行为，比如已访问链接和未访问链接上实现不同的鼠标悬停效果。

     ```css
     /*makes all visited linkes olive on hover*/
     a:visited:hover{color:olive;}
     ```

   * 通用选择器：用"*"表示，一般用来对页面上的所有元素应用样式。在与其他选择器结合使用时，通用选择器可以用来对某个元素的所有后代应用样式。

     ```css
     #nav li * {
       background-image: none;
       padding-left: 0;
     }
     /*覆盖li元素的后代上的样式*/
     ```

   * 高级选择器

     * 子选择器：后代选择器选择一个元素的所有后代，而子选择器只选择元素的直接后代，即子元素。

       ```CSS
       h1 > strong {color:red;}
       ```

       这个规则会把第一个 h1 下面的两个 strong 元素变为红色，但是第二个 h1 中的 strong 不受影响：

       ```html
       <h1>This is <strong>very</strong> <strong>very</strong> important.</h1>
       <h1>This is <em>really <strong>very</strong></em> important.</h1>
       ```

     * 相邻同胞选择器：用于定位同一个父元素下某个元素之后的元素。

       ```css
       h1 + p {margin-top:50px;}
       ```

       这个选择器读作：“选择紧接在 h1 元素后出现的段落，h1 和 p 元素拥有共同的父元素”。

     * 属性选择器：根据某个属性是否存在或属性的值来寻找元素。

       只对有 href 属性的锚（a 元素）应用样式：

       ```css
       a[href] {color:red;}
       ```

       假设希望将指向 Web 服务器上某个指定文档的超链接变成红色，可以这样写：

       ```css
       a[href="http://www.w3school.com.cn/about_us.asp"] {color: red;}
       ```

3. **层叠**

   在CSS中，要寻找同一元素可能有两个或更多规则。CSS通过一个称为层叠的过程处理这种冲突。层叠给每个规则分配一个重要度。

   层叠采用以下重要度次序:

   - 标有!important的用户样式
   - 标有!important的作者样式
   - 作者样式
   - 用户样式
   - 浏览器/用户代理应用的样式。

   然后，根据选择器的特殊性决定规则的次序。具有更特殊选择器的规则优先于具有一般选择器的规则。如果两个规则的特殊性相同，那么后定义的规则优先。如果你遇到了似乎没有起作用的CSS规则，很可能是出现了特殊性冲突。请在你的选择器中添加它的一个父元素ID，从而提高它的特殊性。如果这能够解决问题，就说明样式表中的其他地方很可能有更特殊的规则，它覆盖了你的规则。如果是这种情况，你可能需要检查代码，解决特殊性冲突，让代码尽可能简洁。

4. Sass, less是CSS预处理器，它们的存在就是使css可编程化。比如bootstrap早期就是使用less编写的，后来使用Sass编写。bootstrap的CSS源码里，有很多类似的css代码块，如果用编程方式写css的话，就可以分分钟搞定啦。

5. CSS的学习中需要掌握最重要的3个概念是**浮动**、**定位**和**盒模型**。这些概念控制在页面上安排和显示元素的方式，形成CSS的基本布局。

   1. 盒模型

      一般使用外边距控制元素之间的间隔。

      在CSS中，width和height指的是内容区域的宽度和高度。

      IE的早期版本中，包括IE6，在混杂模式中只用自己的非标准盒模型。width属性不是内容的宽度，而是内容、内边距和边框的宽度的总和。

   2. 相对定位和绝对定位

      相对定位是“相对于”元素在文档流中的初始位置，而绝对定位是“相对于”距离它最近的已经定位的祖先元素，如果不存在已定位的祖先元素，那么相对于初始包含快。

      因为绝对定位的框与文档流无关，所以它可以覆盖页面上的其他元素。可以通过设置z-index属性来控制这些框的叠放次序。z-index值越高，框在栈中的位置就越高。

   3. ​

6. **box-sizing**

   box-sizing 属性允许您以特定的方式定义匹配某个区域的特定元素。
   例如，假如您需要并排放置两个带边框的框，可通过将 box-sizing 设置为 "border-box"。这可令浏览器呈现出带有指定宽度和高度的框，并把边框和内边距放入框中。默认值：content-box。

   语法：

   ```css
   box-sizing: content-box|border-box|inherit;
   ```

   | 值           | 描述                                       |
   | :---------- | :--------------------------------------- |
   | content-box | 这是由 CSS2.1 规定的宽度高度行为。宽度和高度分别应用到元素的内容框。在宽度和高度之外绘制元素的内边距和边框。 |
   | border-box  | 为元素设定的宽度和高度决定了元素的边框盒。就是说，为元素指定的任何内边距和边框都将在已设定的宽度和高度内进行绘制。通过从已设定的宽度和高度分别减去边框和内边距才能得到内容的宽度和高度。 |
   | inherit     | 规定应从父元素继承 box-sizing 属性的值。               |

7. **display有哪些值？说明他们的作用。position的值relative和absolute定位原点是？absolute与fixed共同点与不同点？**

   ```reStructuredText
   ## 1、display
   block 象块类型元素一样显示。
   none 缺省值。象行内元素类型一样显示。
   inline-block 象行内元素一样显示,但其内容象块类型元素一样显示。
   list-item 象块类型元素一样显示,并添加样式列表标记。  

   ## 2、position	
   absolute  生成绝对定位的元素,相对于 static 定位以外的第一个父元素进行定位。 
   fixed （老IE不支持）生成绝对定位的元素,相对于浏览器窗口进行定位。 
   relative 生成相对定位的元素,相对于其正常位置进行定位。 
   static  默认值。没有定位,元素出现在正常的流中  
   inherit 规定从父元素继承 position 属性的值。  
   *（忽略 top, bottom, left, right z-index 声明）

   ## absolute与fixed共同点与不同点
   A、共同点：
   1.改变行内元素的呈现方式,display被置为block; 
   2.让元素脱离普通流,不占据空间; 
   3.默认会覆盖到非定位元素上;    
   B、不同点：
   absolute的”根元素“是可以设置的,而fixed的”根元素“固定为浏览器窗口。
   当你滚动网页,fixed元素与浏览器窗口之间的距离是不变的。
   ```

8. **如何创建块级格式化上下文(block formatting context),BFC有什么用?**

   ```reStructuredText
   BFC,块级格式化上下文,一个创建了新的BFC的盒子是独立布局的,盒子里面的子元素的样式不会影响到外面的元素,在同一个BFC中的两个毗邻的块级盒在垂直方向（和布局方向有关系）的margin会发生折叠。

   ## 创建规则：
   根元素
   浮动元素（float不是none）
   绝对定位元素（position取值为absolute或fixed）
   display取值为inline-block,table-cell, table-caption,flex, inline-flex之一的元素
   overflow不是visible的元素

   ## 作用：
   可以包含浮动元素
   不被浮动元素覆盖
   阻止父子元素的margin折叠

   ## W3C CSS 2.1 规范中的一个概念,它决定了元素如何对其内容进行布局,以及与其他元素的关系和相互作用。
   ```

9. **解释下 CSS sprites,以及你要如何在页面或网站中使用它？**

   CSS Sprites其实就是把网页中一些背景图片整合到一张图片文件中;
   再利用CSS的“background-image”,“background- repeat,background-position”的组合进行背景定位;
   background-position可以用数字能精确的定位出背景图片的位置;

   这样可以减少很多图片请求的开销,因为请求耗时比较长；请求虽然可以并发,但是也有限制,一般浏览器都是6个;
   对于未来而言,就不需要这样做了,因为有了`http2`;

   **优点：**

   减少HTTP请求数,极大地提高页面加载速度;
   增加图片信息重复度,提高压缩比,减少图片大小;
   更换风格方便,只需在一张或几张图片上修改颜色或样式即可实现;

   **缺点：**

   图片合并麻烦;
   维护麻烦,修改一个图片可能需要从新布局整个图片,样式;

10. **display: none;与visibility: hidden;的区别?**

   ```reStructuredText
   ## 联系：它们都能让元素不可见

   ## 区别:
   display:none;  会让元素完全从渲染树中消失,渲染的时候不占据任何空间；
   visibility:hidden;  不会让元素从渲染树消失,渲染师元素继续占据空间,只是内容不可见；
   display:none;  是非继承属性,子孙节点消失由于元素从渲染树消失造成,通过修改子孙节点属性无法## 显示:
   visibility:hidden;  是继承属性,子孙节点消失由于继承了hidden,设置visibility: visible;可以让子孙节点显式；

   tip: 修改常规流中元素的display通常会造成文档重排。修改visibility属性只会造成本元素的重绘；
   tip: 浏览器不会读取display: none的元素内容；会读取visibility: hidden的元素内容；	
   ```

11. **请解释一下浮动和它的工作原理？清除浮动的技巧?**

    ```reStructuredText
     浮动元素脱离文档流,不占据空间。浮动元素碰到包含它的边框或者浮动元素的边框停留。
     
     浮动元素引起的问题：
    （1）父元素的高度无法被撑开,影响与父元素同级的元素
    （2）与浮动元素同级的非浮动元素会跟随其后
    （3）若非第一个元素浮动,则该元素之前的元素也需要浮动,否则会影响页面显示的结构

     解决方法： 
     使用CSS中的clear:both;属性来清除元素的浮动可解决2、3问题;
     对于问题1,添加如下样式,给父元素添加clearfix样式：
     .clearfix:after{content: ".";display: block;height: 0;clear: both;visibility: hidden;}
     .clearfix{display: inline-block;} /* for IE/Mac */

     清除浮动的几种方法：
     1,额外标签法:<div style="clear:both;"></div>（缺点：不过这个办法会增加额外的标签使HTML结构看起来   不够简洁。）
     2,使用after伪类
     #parent:after{
        content:".";
        height:0;
        visibility:hidden;
        display:block;
        clear:both;
        }
     3,浮动外部元素
     4,设置`overflow`为`hidden`或者auto
    ```

12. **如何水平居中一个元素？**

    1、如果需要居中的元素为常规流中inline元素,为父元素设置text-align: center;即可实现;  

    2、如果需要居中的元素为常规流中block元素： 

    ​     1）为元素设置宽度  

    ​     2）设置左右margin为auto 

    ​     3）IE6下需在父元素上设置text-align: center  

    ​     4）再给子元素恢复需要的值

    ```html
    <body>
    	<div class="content">
    	   aaaaaa aaaaaa a a a a a a a a
    	</div>
    	</body>
    	<style>
    	    body {
    	        background: #DDD;
    	        text-align: center; 
    	    }
    	    .content {
    	        width: 500px;      
    	        text-align: left;  
    	        margin: 0 auto;    
    	        background: purple;
    	    }
    	</style>
    ```

    3、如果需要居中的元素为浮动元素：

    ​            1）为元素设置宽度

    ​            2）position: relative

    ​            3）浮动方向偏移量（left或者right）设置为50%

    ​            4）浮动方向上的margin设置为元素宽度一半乘以-1

    ```html
    <body>
    	<div class="content">
    	  aaaaaa aaaaaa a a a a a a a a
    	</div>
    	</body>
    	<style>
    	    body {
    	        background: #DDD;
    	    }
    	    .content {
    	        width: 500px;         
    	        float: left;

    	        position: relative;   
    	        left: 50%;            
    	        margin-left: -250px;  
    	        background-color: purple;
    	    }
    	</style>
    ```

     4、如果需要居中的元素为绝对定位元素：

    ​	1）为元素设置宽度

    ​	2）偏移量设置为50%

    ```html
    <body>
        <div class="content">
    		aaaaaa aaaaaa a a a a a a a a
    	</div>
    	</body>
    	<style>
    	    body {
    	        background: #DDD;
    	        position: relative;
    	    }
    	    .content {
    	        width: 800px;

    	        position: absolute;
    	        margin: 0 auto;
    	        left: 0;
    	        right: 0;
    	        background-color: purple;
    	    }
    	</style>
    ```

13. **如何竖直居中一个元素?**

    1、绝对定位居中(Absolute Centering)：

    ```css
    .Center-Container {  
      position: relative;  
    }  
    	  
    .Absolute-Center {  
      width: 50%;  
      height: 50%;  
      overflow: auto;  
      margin: auto;  
      position: absolute;  
      top: 0; left: 0; bottom: 0; right: 0;  
    }  
    ```

    	*优点：
    		1.支持跨浏览器,包括IE8-IE10
    		2.无需其他特殊标记,CSS代码量少
    		3.支持百分比%属性值和min-/max-属性
    		4.只用这一个类可实现任何内容块居中
    		5.不论是否设置padding都可居中（在不使用box-sizing属性的前提下）
    		6.内容块可以被重绘
    		7.完美支持图片居中
    	*缺点：
    		1.必须声明高度（查看可变高度Variable Height）
    		2.建议设置overflow:auto来防止内容越界溢出。（查看溢出Overflow）
    		3.在Windows Phone设备上不起作用
    	*浏览器兼容性：
    		Chrome,Firefox, Safari, Mobile Safari, IE8-10.
    2、负外边距(Negative Margins)居中：如果块元素尺寸已知,最流行的方法：

    ​	外边距margin取负数,大小为width/height（不使用box-sizing: border-box时包括padding,）的一半;

    再加上top: 50%; left: 50%;

    ```css
    .is-Negative {
      width: 300px;
      height: 200px;
      padding: 20px;
      position: absolute;
      top: 50%; 
      left: 50%;
      margin-left: -170px; /* (width + padding)/2 */
      margin-top: -120px; /* (height + padding)/2 */
    }
    *优点：
    1. 良好的跨浏览器特性,兼容IE6-IE7。
    2. 代码量少
    *缺点：
    1. 不能自适应。不支持百分比尺寸和min-/max-属性设置。
    2. 内容可能溢出容器。
    3. 边距大小与padding,和是否定义box-sizing: border-box有关,计算需要根据不同情况。
    ```

    3、变形（Transforms）：元素高度可变,最简单的方法：

    ​	内容块定义transform: translate(-50%,-50%)必须带上浏览器厂商的前缀；
    ​	再加上top: 50%; left: 50%;

    ```css
    .is-Transformed {
      width: 50%;
      margin: auto;
      position: absolute;
      top: 50%; 
      left: 50%;
      -webkit-transform: translate(-50%,-50%);
      -ms-transform: translate(-50%,-50%);
      transform: translate(-50%,-50%);
    }
    *优点：
    1.内容可变高度
    2.代码量少
    *缺点：
    1.IE8不支持
    2.属性需要写浏览器厂商前缀
    3.可能干扰其他transform效果
    4.某些情形下会出现文本或元素边界渲染模糊的现象

    ```

    4、表格单元格（Table-Cell）：最好的方法:

    ​	因为内容块高度会随着实际内容的高度变化,浏览器对此的兼容性也好;
    ​	最大的缺点是需要大量额外的标记,需要三层元素让最内层的元素居中。

    ```html
    <div class="Center-Container is-Table">
      <div class="Table-Cell">
        <div class="Center-Block">
        <!-- CONTENT -->
        </div>
      </div>
    </div>
    ```
    ```css
    .Center-Container.is-Table { 
      display: table; 
    }
    .is-Table .Table-Cell {
      display: table-cell;
      vertical-align: middle;
    }
    .is-Table .Center-Block {
      width: 50%;
      margin: 0 auto;
    }
    *优点：
    1.高度可变
    2.内容溢出会将父元素撑开。
    3.跨浏览器兼容性好。
    *缺点：
    需要额外html标记
    ```

     5、行内块元素（Inline-Block）：受欢迎的方式

    ​	使用display: inline-block, vertical-align: middle和一个伪元素让内容块处于容器中央；

    ​	如果内容块宽度大于容器宽度,比如放了一个很长的文本,但内容块宽度设置最大不能超过容器的100%减去	0.25em,
    ​	否则使用伪元素:after内容块会被挤到容器顶部,使用:before内容块会向下偏移100%。
    ​	如果你的内容块需要占据尽可能多的水平空间,可以使用max-width: 99%;（针对较大的容器）或max-width: calc(100% -0.25em)（取决于浏览器和容器宽度）

    ```html
    <div class="Center-Container is-Inline">
      <div class="Center-Block">
        <!-- CONTENT -->
      </div>
    </div>
    ```

    ```css
    .Center-Container.is-Inline {
      text-align: center;
      overflow: auto;
    }
    .Center-Container.is-Inline:after,
    .is-Inline .Center-Block {
      display: inline-block;
      vertical-align: middle;
    }
    .Center-Container.is-Inline:after {
      content: '';
      height: 100%;
      margin-left: -0.25em; /* To offset spacing. May vary by font */
    }
    .is-Inline .Center-Block {
      max-width: 99%; /* Prevents issues with long content causes the content block to be pushed to the top */
      /* max-width: calc(100% - 0.25em) /* Only for IE9+ */
    }
    *优点：
    1.高度可变
    2.内容溢出会将父元素撑开。
    3.支持跨浏览器,也适应于IE7。
    *缺点：
    1.需要一个容器
    2.水平居中依赖于margin-left: -0.25em;该尺寸对于不同的字体/字号需要调整。
    3.内容块宽度不能超过容器的100% - 0.25em。
    ```
    6、Flexbox:	这是CSS布局未来的趋势。

    ​	Flexbox是CSS3新增属性,设计初衷是为了解决像垂直居中这样的常见布局问题。
    ​	记住Flexbox不只是用于居中,也可以分栏或者解决一些令人抓狂的布局问题。

    ```css
    body {
      /* Remember to use the other versions for IE 10 and older browsers! */
      display: flex;
      justify-content: center;
      align-items: center;
    }
    *优点：
    1.内容块的宽高任意,优雅的溢出。
    2.可用于更复杂高级的布局技术中。
    *缺点：
    1. IE8/IE9不支持。
    2. Body需要特定的容器和CSS样式。
    3.运行于现代浏览器上的代码需要浏览器厂商前缀。
    4.表现上可能会有一些问题

    *其他：
    曾经你使用负边距（Negative Margins）的地方,现在可以用绝对居中(Absolute Centering)替代了。
    你不再需要处理讨厌的边距计算和额外的标记,而且还能让内容块自适应大小居中;
    ```
    如果你的站点需要可变高度的内容,可以试试单元格(Table-Cell)和行内块元素(Inline-Block)这两种方法;	
    如果你处在流血的边缘,试试Flexbox,体验一下这一高级布局技术的好处吧。

14. **什么叫外边距折叠(collapsing margins)？**

    毗邻的两个或多个margin会合并成一个margin,叫做外边距折叠。规则如下：

    1、两个或多个毗邻的普通流中的块元素垂直方向上的margin会折叠
    2、浮动元素/inline-block元素/绝对定位元素的margin不会和垂直方向上的其他元素的margin折叠
    3、创建了块级格式化上下文的元素,不会和它的子元素发生margin折叠
    4、元素自身的margin-bottom和margin-top相邻时也会折叠

15. ​

16. ​

