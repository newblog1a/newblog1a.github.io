---
published: true
---
## A New Post

# 前端响应式布局\_float布局淘汰了吗-CSDN博客

文章浏览阅读599次。响应式布局今天已经是2019年，基本上所有新建的网页都会是响应式(Responsive)，以适应在手机显示。而实现响应式网页布局主要有3种方法：FloatFlexboxCSS Grid当然，以上三者都需要搭配Media Query使用。其中CSS Grid是最新，也是我最推崇的，但由于太新，较旧的浏览器并不支持。不过，大部分的浏览器其实都已经支持了，我个人不会太担心。要想知道哪些浏...\_float布局淘汰了吗

2 分鐘的閱讀時間

查看原始頁面

* * *

### [响应式布局](https://so.csdn.net/so/search?q=%E5%93%8D%E5%BA%94%E5%BC%8F%E5%B8%83%E5%B1%80&spm=1001.2101.3001.7020)

今天已经是2019年，基本上所有新建的网页都会是响应式(Responsive)，以适应在手机显示。而实现响应式网页布局主要有3种方法：

1.  Float
2.  Flexbox
3.  CSS Grid

当然，以上三者都需要搭配Media Query使用。

其中CSS [Grid](https://so.csdn.net/so/search?q=Grid&spm=1001.2101.3001.7020)是最新，也是我最推崇的，但由于太新，较旧的浏览器并不支持。不过，大部分的浏览器其实都已经支持了，我个人不会太担心。要想知道哪些浏览器支持CSS Grid（或其他新功能），可以前往Can I Use查询。CSS Grid完全就是为了网页布局及其他二维（横向加纵向）布局而设计的，相信未来的网页都会采用这一设计。

Flexbox也算新，但浏览器支持的情况比CSS Grid要好点。基本上，目前主流已经转向Flexbox，Bootstrap就是很好的例子。但其实，Flexbox是为一维布局设计的（横向或纵向），而网页布局往往是二维的，Flexbox并非最佳选择，但由于CSS Grid来得太迟，Flexbox又能完成任务，现在不少新的网页以及前端框架采用Flexbox。

Float原来是设计来处虑理文绕图之类的问题，后来被用于布局设计。Float布局有著各种各样的问题，已经在逐渐淘汰中，但由于过去应用太普遍，相信短时间内并不会消失，因此也有必要瞭解。

#### Float网页布局

Float布局的重点是  
1\. 让元素靠向同一个方向（左或右）  
2\. 用百分比控制每一个元素的宽度  
3\. 透过Media Query改变元素宽度以适应不同屏幕尺寸

W3Schools的例子

这个例子的重点有两处，一是设定左右两栅都向左float，宽度分别为75%和25%：

    /* Left column */
    .leftcolumn {   
      float: left;
      width: 75%;
    }
    
    /* Right column */
    .rightcolumn {
      float: left;
      width: 25%;
      background-color: #f1f1f1;
      padding-left: 20px;
    }
    

二是Media Query设定当屏幕尺寸小于800px时，让左右两栅的宽度都变成100%，以实现响应式设计（Responsive Design）：

    @media screen and (max-width: 800px) {
      .leftcolumn, .rightcolumn {   
        width: 100%;
        padding: 0;
      }
    }
    

或许你会注意到导航栏（.topnav）也进行了类似的处理，由于原理一样，就不多说了。  
改进：移动优先原则（Mobile First）

之前介绍过移动优先原则，即先设计小屏幕版，再透过Media Query设定桌面版。W3Schools的这个例子并没有采取这一原则，我们可以自行修改，使之符合。方法很简单，只要将Median Query里的内容和外面相应的内容反过来即可，不要忘了把Media Query从max-width改为min-width。

    /* Left column */
    .leftcolumn {   
      float: left;
      width: 100%;
    }
    
    /* Right column */
    .rightcolumn {
      float: left;
      width: 100%;
      background-color: #f1f1f1;
      padding: 0;
    }
    
    @media screen and (min-width: 800px) {
      .leftcolumn {   
        width: 75%;
      }
      .rightcolumn {
        width: 25%;
        padding-left: 20px;
      }
    }
    

#### Flexbox响应式网页布局

透过Flexbox实现响应式网页布局同样可分为三步：

1.  将容器显示为flex，并让它wrap；
2.  将需要响应的元素放在容器当中，并用百分比设定每一个元素的basis；
3.  通过Media Query将容器的flex方向改为column（预设为row）。

在W3Schools的例子中，[Flex](https://so.csdn.net/so/search?q=Flex&spm=1001.2101.3001.7020)容器是这样设定的：

    .row {  
      display: flex;
      flex-wrap: wrap;
    }
    

容器当中的两栏，即Flex项目：

    /* Sidebar/left column */
    .side {
      flex: 30%;
      background-color: #f1f1f1;
      padding: 20px;
    }
    
    /* Main column */
    .main {
      flex: 70%;
      background-color: white;
      padding: 20px;
    }
    

最后是媒体查询，断点设在700px:

    @media screen and (max-width: 700px) {
      .row {
        flex-direction: column;
      }
    }
    

改进：移动优先

同样地，我们也对这个例子进行移动优先的改进，当作是练习。

Flex容器：

    .row {  
      display: flex;
      flex-direction: column
    }
    

Flex项目：

    .side {
      padding: 20px;
      background-color: #f1f1f1;
    }
    
    /* Main column */
    .main {
      padding: 20px;
      background-color: white;
    }
    

媒体查询：

    @media screen and (min-width: 700px) {
      .row {
        flex-direction: row;
      }
      .side {
        flex: 30%;
      }
      .main {
        flex: 70%;
      }
    }
    

一样是，将媒体查询内外的内容交换。

#### CSS Grid响应式网页布局

透过CSS Grid实现响应式网页布局的方法有很多种，最简单，也最能体现CSS Grid的特点的方法是使用grid-template-areas（注意是复数，结尾有s）。这种方法同样可分为三步：  
1\. 为每一个Grid项赋与一个名字；  
2\. 用grid-template-areas来控制每一个Grid项所占的空间；  
3\. 通过Media Query改变每一个Grid项所占的空间。

在W3Schools的例子中，一开始便为每一个div取了一个相应的名称：

    .item1 { grid-area: header; }
    .item2 { grid-area: menu; }
    .item3 { grid-area: main; }
    .item4 { grid-area: right; }
    .item5 { grid-area: footer; }
    

注意这里的grid-area是单数。接著在包含这些div的容器中控制它们的所占空间比例。

    .grid-container {
      display: grid;
      grid-template-areas:
        'header header header header header header'
        'menu main main main right right'
        'menu footer footer footer footer footer';
      grid-gap: 10px;
      background-color: #2196F3;
      padding: 10px;
    }
    

重点在于grid-template-areas，可以看到这里的设定便是CSS Grid最终的显示效果。其中menu是最小宽度单位，header等于六个menu；main等于三个menu；right等于两个menu；而footer则等于五个menu。menu的宽度为1个单位，但高度却跨两行。

在W3Schools的例子中，并没有实现响应式的部分，但我们可以自行加入媒体查询的部分来实现。透过这个实现，你可以看到CSS Grid的神奇、直观、易用的特点。

    @media screen and (max-width: 700px) {
      .grid-container {
        grid-template-areas:
            'header'
            'menu'
            'main'
            'right'
            'footer'
      }
    }
    

在媒体查询的部分，只要更改grid-template-areas的设定就能改变整个布局。

同样地，这个例子也没有采用移动优先原则。你也可以将这个例子改为移动优先作为练习，由于前两篇中都做过这样的尝试，这里就不再赘述了。