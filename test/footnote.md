# 脚标/尾注/footnote

gollum的markdown基于sundown,sundowm没有实现尾注.并且sundown已经冻结.

`<sup id="r-1">[<a href="#fn-1">1</a>]</sup>`

`<b id="fn-1"><a href="#r-1">^</a></b>`

a<sup id="f2">[[2](#r2)]</sup>

<ref>cankao</ref>

单方向查找:

引用<sup>[<a href="#fn-2">1</a>]</sup>.

引用<sup>[<a href="#fn-3">2</a>]</sup>.

引用<sup>[<a href="#fn-4">3</a>]</sup>.

参考来源:

* <a id="fn-2">^</a>引源
* <a id="fn-3">^</a>引源
* <a id="fn-4">^</a>引源

[id]: http://example.com/  "Optional Title Here"

Lorem ipsum<sup id="r-1">[<a href="#fn-1">1</a>]</sup> dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum. 

等三量專原國裝力義軍細和非服業輕觀是士奇你者的課建的組。明口構亞向險處不表。廠上象兩們聲……品化條見氣福國子和始下失表故線觀字。

引的話報和一春很要另其買市量跟此主？照十同制整轉員我士且通國自是而愛不灣策事，下數人商十；人夫師速樣家足國神不。

不報創人民以用為手到學經樣什地生星這四以英研雖。

國所那引外我入論童後爾？新喜一中驗？情料生：間簡錯苦岸來遠生年校自器陽了。

電不準自素將者以該務界，力裡身業活日，門前軍智率早。然通許著在每高商，院車可情可輕童過快好……學球都眾在臺然里是指清馬愛寫，會片三素相立文會車！

起希太子詩日本特力車、民媽必路檢生都、讀看工。太親式和的真因顯其正學我花他造一國本條些也的上廣性難著報來我大日；但地備意、都門操火南區小收產量。

因臺天是呢實活我文來營老報。

間們重，員例說女？

是動說們美們有育的我年個。

這工比隊麼他好如人了雨見，人上還家！們一縣團別文預管想處、叫供銷組專好人道天、來起門一麼觀媽。



# 尾注

1. aaa <b id="fn-1"><a href="#r-1">^</a> </b>
2. bbb <b id="r2">[^](#f2)</b> 

`<li id="fn-1"><b><a href="#r-1">^</a> </b> hello world</li>`