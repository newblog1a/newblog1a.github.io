---
published: true
---
tailwind

https://npmtrends.com/bootstrap-vs-sass-vs-tailwindcss
![2025-02-21 01_34_14-bootstrap vs tailwindcss _ npm trends - Brave.jpg]({{site.baseurl}}/img/2025-02-21 01_34_14-bootstrap vs tailwindcss _ npm trends - Brave.jpg)

優點
==

1.  Css檔案變小非常很多
2.  因為html和css綁定在一起，所以沒有樣式汙染問題，在寫scss時，時常有的會將共同樣式一起寫，之後再單獨寫獨特樣式，造成一個component，有好幾處scss需要更改，造成維護困難，在tailwind也不會發生。
3.  不用命名，其實我覺得BEM滿好用，雖然有時候命名頭也滿痛。
4.  編譯速度算滿快，到底多快我沒有測試，約都在1秒內，且我想看畫面沒有延遲過。當然現在一堆飆車等級的編譯工具，例如vite and turborepo都很棒。
5.  layout很快，速度相比scss，速度上快非常多\
    html寫完結構，直接就可以在class寫樣式，相比傳統scss，scss需要先命名class，將命名的class寫到scss檔案中，並寫好巢狀，才開始寫樣式。
6.  複製貼上html可以順便把css帶著走\
    有一種情境，這邊有的樣式，另一個頁也有類似的，先前作法上可能會就會把html複製貼過去，之後再去複製scss，還要仔細檢查有沒有複製到不會用到的scss，時間上就會造成不必要的浪費。\
    在tailwind，直接複製貼上，刪除不要的html，就能一起刪除了。
7.  學習成本低，上手容易，比較可被團隊所接受，觀察大部分同事學習成本約一個禮拜後，就可以順暢切版。
8.  生態系強大，以TailwindCss為基礎所開發的UI已經很成熟，例如：Radix跟shadcn/ui

缺點
==

1.  前期設定Style guide，需要設定config較久，才可以開始layout，之後就可以直接在class寫樣式，如果config沒寫完整，就需要常常回去tailwind.config.js補樣式，其實會滿心累。\
    (雖然不設定也可以用，但之後維護會比較混亂)
2.  沒有辦法直接複製貼上網路上、設計稿或開發者工具上的樣式\
    例如:\
    1\. 在設計稿上的box-shadow可以直接複製貼上到CSS檔案中，但使用Tailwind就需要重新撰寫，反而會變得比較麻煩。\
    2\. 在開發者工具上會去layout完，結果沒辦法直接複製貼上到程式碼庫中。
3.  如果沒有辦法撰寫html，就無法編輯class\
    例如:\
    1.用CMS系統，會在特定的狀況下無法去修改CMS的html。當然也可以在css檔案中，去選CMS給的class，其中使用@apply的方式去呼叫tailwind語法出來使用。\
    2.用一些ui庫或是library也有相同狀況，沒辦法直接修改到某個DOM的class
4.  樣式複雜，維護性就很差\
    樣式如果複雜，就會看到class中整坨的tailwind，看了眼睛很累，雖然有自動排序tailwind class的prettier，自己用了是覺得還好。
5.  沒有辦法註解\
    例如:之前在撰寫SCSS時，會把某一行的css給註解起來做測試之類的用途。tailwind的話，因為都在html中，註解會把整個html註解起來。
6.  Vscode extension "Tailwind CSS IntelliSense"，在智能提示框顏色顯示方面只能顯示RGB，無法顯示hex\
    目前似乎無解。hex的可讀性比較高，我自己使用上也比較習慣，設計稿上用hex也是設計師預設的方式，所以比較難去更改。我通常都要先去style guide介面中，找顏色，再直接複製貼上到class裡面，反而變麻煩很多。\
    [Color Hex Code in intellisense box #418](https://github.com/tailwindlabs/tailwindcss-intellisense/issues/418)
7.  nuxt.js中改tailwind.config.js，會需要退出，再重新啟動，才能使用。
8.  沒辦法在class中使用calc()，需要用其他方式變通。\
    [is it possible to add min-h-[calc(100vh --- pt-16)], the calc value is className its self?](https://github.com/tailwindlabs/tailwindcss/discussions/11324)
  
## Tailwind 是 Utility Framework ，而Bootstrap 則是 UI Framework。

Utility Framework 指的是Tailwind 已經將各種在撰寫CSS時會用到的功能寫成一個個獨立的class，而撰寫元件時便必須自己透過Tailwind提供的class，組合出想要的樣式。

像是`mt-1` 指的便是 `margin-top:0.25rem`

Tailwind 最大的罩門便是沒辦法像撰寫CSS in JS 一樣可以傳入各種變數，並在Styled-components 本身進行運算等等…

這部分變得回歸過去透過classList 動態變更style 或是 inline style的方式來達成。
  https://medium.com/@peter50710t/tailwind-css-%E9%9A%A8%E7%AD%86-%E7%B0%A1%E4%BB%8B%E5%8F%8A%E5%88%86%E6%9E%90-65d6b75606f6
  
## Atomic CSS
Atomic CSS：

```
.bg-blue {background-color: #357edd; }
.margin-0 { margin: 0; }
```

而 Tailwind CSS 就是實作了 Atomic CSS 這個概念的一個 CSS 框架。

大家都知道在寫網頁的時候要注重關注點分離（separation of concerns），讓 HTML 做好它的事情，只關注內容，而 CSS 只關注樣式，兩者透過 class name 連結在一起。可是作者發現這樣子的概念，其實會帶來許多負面影響

JSX 的發展，也是直接破壞掉了以往 JavaScript 跟 HTML 應該要分開的 best practice。

### 那共用元件像是 button 該怎麼辦？難道我要每個地方都改樣式？
Atomic CSS 的出現並沒有要完全取代傳統 semantic 的做法，正確的做法應該是哪個適合就用哪個。

而官網 FAQ 也有特別提到類似的事情：

> If changing some styling requires you to edit multiple files, then you should use the classic CSS approach

舉例來說，你的程式裡面有個按鈕會一直重複出現，這時候如果每次都複製貼上 HTML，要改 class 的時候就要每個檔案都改，顯然是不合理。

Atomic CSS 是在維護大型專案的時空背景之下誕生的，如果你沒有碰到那種「牽一髮而動全身，改一個地方要檢查好多地方會不會壞掉」的狀況，那你用了 Atomic CSS，可能確實感覺不到它的好處
若是在一個「相同的元件卻四處分散，當你改 HTML 時需要同時改很多地方」的專案用上了 Atomic CSS，那確實是不適合

Atomic CSS 帶來了兩個獨特的好處：

降低 CSS 檔案大小
最大程度縮減 scope，讓維護變得容易

有些人可能會疑惑說：「但你講的這個，CSS-in-JS 或是 CSS modules 也都做得到啊」

但跟 Atomic CSS 比起來有兩樣是做不到的。

第一點是 CSS-in-JS 跟 CSS modules 這兩個方案據我所知，應該都需要搭配一些前端的 library 或是框架來使用，例如說 React 或 Vue，但 Atomic CSS 不需要。舉例來說，假設今天一個專案它的 component 是在後端用 template engine 達成的，而非在前端，那就沒辦法用這兩套解決方案。

第二點是 CSS 大小，沒辦法跟 Atomic CSS 一樣這麼小。
  https://blog.huli.tw/2022/05/23/atomic-css-and-tailwind-css/
  
## Bootstrap的缺點

### 1\. 客製化門檻高

Bootstrap 在客製化的時候，因為需要學習 sass，因此門檻較高。

### 2\. 專案的維護成本較高

不論是 sass 的新手或是老手，為了要客製化將 sass 導入，勢必會提高專案的維護成本，比較麻煩。


## 原子类

在写样式表的时候，我已经注意到了 CSS 的一个难点：取一个好的 class 名字。在查找解决方案的时候，了解到了一种叫做"原子类"的命名风格，类似于

css

```
.btn {
    color: red;
    font-size: 20px;
}

/*拆分成颗粒原子类*/
.text-red {
    corlor: red;
}
.text-20 {
    font-size: 20px;
}
```

然后通过组合的方式构建页面

html

```
<button class="btn"></button>
<button class="text-red text-20"></button>
```

在我的刻板印象中，原子类介于行内样式和 class 样式之间

-   避免了行内样式无法样式复用的问题，CSS 选择器一个非常重要的功能就是样式代码复用
-   避免了类选择器的取名难题，不需要再想一个语义化的名字
-   通过组合的方式可以快速的实现一个带样式的页面，却不需要写一行 CSS 代码（当前前提是原子类丰富，且你能够记得住那么多样式）
-   样式写的少了，样式表的体积也就少了

纵然有这么多优点，但原子类的缺点也很明显

-   需要花时间去熟悉，如果原子类的定义没有规范，则需要花费更多的时间。比如上面的`text-red`这个红色到底是哪个颜色，如果设计师定义了多个红色，改怎么处理？起码得有个文档吧
-   每个标签的 class 都很长，没有语义化的可读性；class 很长会导致 HTML 文件变得很庞大，尽管 gzip 可以帮助压缩一部分重复的内容，但在控制台调试的时候也不太方便（sourcemap 也没用了）
-   如果全都使用原子类来构建页面，则维护起来是很麻烦的，比如一个.btn 在 10 个地方使用，现在需要给这个类加一个 hover，常规的话只需要改这一个地方；而原子类需要先找到需要添加样式的标签，然后依次增加 hover 的原子类，删除同理
-   选择器嵌套、后代选择器等 css 的特点看起来也是不太好用上了

第 3 点是我无法接受的硬伤，以至于 2020 前当我第一次了解到`tailwindcss`时，看了一下描述，嗤之以鼻，随后就关闭了官网。

### 现代化 CSS 开发流程的问题

经过很长一段时间的摸索，总结出一套比较适合开发 CSS 的姿势。

借助`@mixin`、`@extend`等预处理器提供的方法，可以复用 css 代码片段，甚至在 css 中写一些逻辑。

借助`autoprefixer`、`px2rem`等 postcss 工具，不需要写繁琐的前缀和单位转换。

借助`css module`、`css scoped`等工具，不用考虑样式冲突，以至于不用过多地考虑命名了。

借助各种 MVVM 框架，基本上不用考虑通过选择器获取 DOM 节点进行操作，以至于不用过多地考虑命名了。

剩下需要命名的场景，BEM 就搞定了。
  https://www.shymean.com/article/%E5%85%B3%E4%BA%8ETailwindCSS%E7%9A%84%E4%B8%80%E4%BA%9B%E6%80%9D%E8%80%83

## 把 CSS 样式的方案分为四种粒度

**一、四种粒度**
----------

```html
<div style="{ borderRadius: '0.5rem', padding: '1rem' }"> Click </div>

<div class="rounded-lg p-4"> Click </div>

<div class="button"> Click </div>

<Button> Click </Button>

```

越往下走，颗粒度越来越大，约束性变高，自由性不足。而 `TailwindCSS` 位于第二层。

---
Tailwind 让你不写样式的方式是把 CSS 用类的形式添加到标签中。它没有封装任何组件样式，一个都没有。它仅仅让你换一种方式添加样式而已。例如 width: 0.25rem 变成了 w-1，background-color: white 变成了 bg-white。它没有一个叫 nav 的样式，让你添加到一个指定结构的 nav 就变成了一个真的 Navbar，没有的。所以它自称是底层 CSS 框架，毕竟它真的就是换了一种方式写 CSS。

---
为什么 2010 年人人喊打的原子类在 2020 年让人大喊真香？归根结底，是时代变了。

在 2010 年，编写 CSS 被认为是在为 HTML 文档进行排版。因此，"内容（HTML）、样式（CSS）、行为（JS）需要分离"这句话，连同着"CSS 代码应当体现 HTML 文档结构/语义"的观念深入人心，以至于刻到那个时代入行的前端工程师的 DNA 里（当然，这些说法在这几年入行的前端工程师看来，基本上属于不知所云的口号）。因此大家一直习惯于：

-   为 HTML 文档里的一些元素起一些所谓"能反映它在文档里的角色/语义"的类名 / ID（例如 header、sidebar、menu、navbar 等等，甚至发明出 [BEM](https://zhida.zhihu.com/search?content_id=335198028&content_type=Answer&match_order=1&q=BEM&zhida_source=entity) 之类的"命名规范"），然后用类选择器或 ID 选择器来选中它们
-   偶尔搭配使用那些依赖于文档结构的 CSS 选择器（后代选择器、子选择器、兄弟选择器等等）

但是，在 2020 年，没有人还会认为自己在编写 HTML 文档，或者说，认为自己的 HTML 是所谓的"内容"。HTML 实际上是类似于 GUI 像素 / 控件一类的东西，是组件的渲染产物。对于这种"渲染产物"，大家需要的是能够方便地指定其外观的方案，而不愿意再走" 纵览文档结构 -> 为元素起名字 -> 用选择器选中 -> 写 CSS 语句"这套传统流程：

1.  由于组件化开发模式的推行，已经不存在一个所谓的"文档结构"了（现在只有[组件树结构](https://zhida.zhihu.com/search?content_id=335198028&content_type=Answer&match_order=1&q=%E7%BB%84%E4%BB%B6%E6%A0%91%E7%BB%93%E6%9E%84&zhida_source=entity)，而且你通常不会在乎它），也就是传统流程的第一步早已不复存在
2.  为了消灭第二、三步， [CSS-in-JS](https://zhida.zhihu.com/search?content_id=335198028&content_type=Answer&match_order=1&q=CSS-in-JS&zhida_source=entity) 开始流行起来
3.  而 tailwind 这种原子类方案就更激进了，它不但跳过了传统流程的前三步，连第四步都帮你简化了（用一个简短的单词，也就是一个原子类，来代替一个 CSS 语句）

---
为什么以前总是说样式要和 html 分离，现在却又使用 tailwindCSS 这样的一种形式让样式与 html 紧密的结合在了一起。

造成这种反转的原因是 react，vue ，angular 这些前端技术出现后前端组件化变得非常容易了。

样式分离的目的主要是为了复用和方便维护，但在组件化面前根本就不够打的。

反而像 vue 单文件组件这样的形式 html模板，css ，js 放在一起更方便修改

对于组件的样式而言采用嵌套的 css 选择器结构:.body > .heder {color:red}来设定样式是不如直接在模板内内联样式：<body> <header style={color:red}></header></body> 更方便维护的。

然后内联样式又不如原子类简洁，tailwind 又是原子类的大成者，故我用 tailwind

作者：绅士喵
链接：https://www.zhihu.com/question/337939566/answer/896450767
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
