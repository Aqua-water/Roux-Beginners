<script type="text/javascript" src="/twistysim.js"></script>
<style type="text/css" rel="stylesheet">
/* modifies the opacity of the cube wireframe */
.ttk-shp-poly {
    stroke-opacity: 0.3;
}
</style>

# 后六棱色向

> 这一步骤是桥式解法中最为困难的部分，请合理利用虚拟魔方下方的双箭头以观察单步转动。如果你感觉理解起来很吃力，可以在学习完“箭头”情形后，直接阅读[复原流程图](#复原流程图)：了解具体的流程之后，再结合基本操作三~六，理解[色向转换原理](#色向转换原理)的思想。

## 目标

这步要将顶层和底层上剩余的块的色向还原正确，主要解决的是剩下6个棱块的色向。这步又叫做EO (Edges Oriented)。

“色向”这个词在CMLL中已经提及过。所谓块的色向(orientation)，在这里指的是块沿着上下方向的朝向。例如，某个块含有黄色面，在其朝向正确时，黄色面是向上的。而在它色向正确时，黄色面不仅可以朝上，还可以朝下。

以下是一个色向还原正确的具体例子。你可以拖动魔方，查看底层的状态：

<div id="eo-eo-eg">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwywwywggggggolorlrrrrglgyyywwyyyybbbbbbrlrolooooblb')
    ('#eo-eo-eg');
</script>
</div>

在下图中，色向错误的棱块数量为4。（注意底层也有两个。）

对于这种情形的抽象表示，我们只标出上下两层中，色向错误的棱块面的位置，使用红色表示。它不一定代表真正的红色面。

<div id="eo-eo-eg2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wowwywwowgggggggbgryrrorryryryywyyrybbbbbbbgbowooroowo')
    ('#eo-eo-eg2');
</script>
</div>

<div id="eo-eo-eg2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trtttttrttttttttttttttttttttrtttttrttttttttttttttttttt')
    ('#eo-eo-eg2');
</script>
</div>

中心块的色向通过一步`Mx`就能解决，我们先还原它们，再处理剩下6个棱块的色向。

<div id="eo-eo-c">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wlwwywwlwgggggglllrlrrlrlllylylwlylybbbbbbllloloololll')
    .case(`M'`)
    ('#eo-eo-c');
</script>
</div>

> 没有必要将中心块完全对齐。对于黄色中心块，它在顶层或底层都是一样的。

下面的介绍中，通过`Ux`能够相互转换的情形视为同一种情形。

## 四个棱块色向错误

首先，介绍两类基本操作：

### 基本操作三：	`M' Ux Mx`

这里，`Ux`被限制为`U`或`U'`，`Mx`也被限制为`M'`或`M`，因此共有4种组合。其作用是改变除UB、DB两个位置外，其余的4个棱块的色向：

<div id="eo-base3">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('tttttttrttttttttttttttttttttrtrtrttttttttttttttttttttt')
    .alg(`M' U M'`)
    ('#eo-base3');
</script>
</div>

<div id="eo-base3">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`U' M' U2 M U' M U M2 U M U2 M2 U'`)
    .alg(`M' U M'`)
    ('#eo-base3');
</script>
</div>

或者：

<div id="eo-base3-2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('tttttttrttttttttttttttttttttrtrtrttttttttttttttttttttt')
    .alg(`M' U' M`)
    ('#eo-base3-2');
</script>
</div>

<div id="eo-base3-2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`M' U' M`)
    ('#eo-base3-2');
</script>
</div>

等等。可以看到执行完基本操作三后，4个红色面都移向了侧面，意味着其对应棱块的色向已改变。UB、DB两个位置的棱块色向不会变化。

以上演示改变的都是色向错误的棱块。相应地，色向正确的棱块也能变化为错误的情形：

<div id="eo-base3-3">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .alg(`M' U' M`)
    ('#eo-base3-3');
</script>
</div>

### 基本操作四：	`M Ux Mx`

与基本操作三相比，其第一步换成了`M`，而`Ux`与`Mx`同样被限制为90°转动。其作用是改变除UF、DF两个位置外，其余的4个棱块的色向：

<div id="eo-base4">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trttttttttttttttttttttttttttttrtrtrttttttttttttttttttt')
    .alg(`M U M'`)
    ('#eo-base4');
</script>
</div>

<div id="eo-base4">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`M U M2 U2 M2 U2 M U2`)
    .alg(`M U M'`)
    ('#eo-base4');
</script>
</div>

可见，基本操作三和基本操作四能够改变特定的四个棱块的色向：3个位于顶层，1个位于底层。

### "3-1"(箭头)

箭头(Arrow)的顶层有3个色向错误的棱块，底层有1个。通过做`Ux`可调整其形态为以下二者之一：

<div id="eo-arrow">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('tttttttrttttttttttttttttttttrtrtrttttttttttttttttttttt')
    ('#eo-arrow');
</script>
</div>

<div id="eo-arrow">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trttttttttttttttttttttttttttttrtrtrttttttttttttttttttt')
    ('#eo-arrow');
</script>
</div>

两种箭头的区别在于，两个色向正确的棱块是位于后面(UB、DB)还是前面（UF、DF）。显然，[基本操作三](#基本操作三m-ux-mx)和[基本操作四](#基本操作四m-ux-mx)能直接解决箭头的情形。

“箭头”是最简单的情形。后续所有的情形，都将先转化为“箭头”情形，然后复原。

### "2a-2"

2a-2情形的含义为："2a"代表顶层有2个色向错误的棱块，并且位置相邻(adjacent)；"-2"代表底层有2个色向错误的棱块。后边情形的命名方式都与此类似。

和“箭头”相比，2a-2的顶层少了一个色向错误的棱块，而底层多了一个。相应解法如下：

<div id="eo-case2a-2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trtttttrttttttttttttttttttttttrtttrttttttttttttttttttt')
    ('#eo-case2a-2');
</script>
</div>

<div id="eo-case2a-2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`U2 M U' M2 U2 M U' M' U M' U`)
    .alg(`M2 U' M' U M'`)
    ('#eo-case2a-2');
</script>
</div>

以上解法的原理为：2a-2的顶层上位于M层的两个棱块中，恰有一个色向错误。如果做`M2`，将改变M层的四个棱块的上下位置。这样，色向错误的棱块数不变（4个），而底层将恰有一个色向错误的棱块，因此转化为“箭头”情形。

<div id="eo-case2a-2-1">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trtttttrttttttttttttttttttttttrtttrttttttttttttttttttt')
    .alg(`M2`)
    ('#eo-case2a-2-1');
</script>
</div>

> 下面我们再引入两类基本操作，以完成上下两个棱块的单独互换，并且不改变所有棱块的色向。

### 基本操作五：	`M' U2 M`

将M层与F层（前面）交界的两个棱块（UF位、DF位）位置互换。

<div id="eo-base5">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('tttttttrtttttttttttttttttrtttttttttttttttttttttttttttt')
    .alg(`M' U2 M`)
    ('#eo-base5');
</script>
</div>

可以发现,交换位置的两个棱块的色向都没有改变，即一个正确、一个错误。

### 基本操作六：	`M U2 M'`

将M层与B层（后面）交界的两个棱块（UB位、DB位）位置互换。

<div id="eo-base6">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trttttttttttttttttttttttttttttttttttttttttttttttttttrt')
    .alg(`M U2 M'`)
    ('#eo-base6');
</script>
</div>

因此，当两个互相交换的棱块有不同的色向时，这两类基本操作能将顶层色向错误的棱块数增加或减少一个，同时保证色向错误的棱块总数不变。

### "2o-2"

2o-2情形下，"2o"代表顶层有2个色向错误的棱块，并且位置相对(opposite)；"-2"代表底层有2个色向错误的棱块。

一种可行的解法如下：

<div id="eo-case2o-2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trtttttrttttttttttttttttttttttrtrttttttttttttttttttttt')
    ('#eo-case2o-2');
</script>
</div>

<div id="eo-case2o-2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`M U2 M' U2 M' U' M U2 M2 U M2 U'`)
    .alg(`M U2 M' U2 M' U M'`)
    ('#eo-case2o-2');
</script>
</div>

以上解法的原理如下。与2a-2情形一样，2o-2中色向错误的棱块数也是4个。对于2a-2，我们使用`M2`让顶层和底层的四个棱块两两互换，从而将顶层色向错误的棱块数改变为3，转化为“箭头”。这里用同样的办法就不能奏效了。

对于2o-2情形，可以做[基本操作五](#基本操作五-m-u2-m)或[六](#基本操作六-m-u2-m)转换为箭头。以基本操作六为例，在B面（后面）上，一个底层色向错误的棱块与一个顶层色向正确的棱块交换，便转化为“箭头”。

<div id="eo-case2o-2-1">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trtttttrttttttttttttttttttttttrtrttttttttttttttttttttt')
    .alg(`M U2 M'`)
    ('#eo-case2o-2-1');
</script>
</div>

### "4-0"

4-0情形下，顶层有4个色向错误的棱块，而底层没有。

可以想象，`M2`就能将4-0转为2o-2的情形。因此将2o-2解法的第一步由`M`改为`M'`，就能解决这类情形。

<div id="eo-case4-0">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('ttttttttttttttttttttttttttttrtrtrtrttttttttttttttttttt')
    ('#eo-case4-0');
</script>
</div>

<div id="eo-case4-0">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`M' U2 M' U2 M' U' M U2 M2 U M2 U'`)
    .alg(`M' U2 M' U2 M' U M'`)
    ('#eo-case4-0');
</script>
</div>

## 色向转换原理

我们已经展示了恰有4个色向错误的棱块的情形，转化为[“箭头”](#3-1箭头)情形的办法，即通过[基本操作五](#基本操作五-m-u2-m)、[六](#基本操作六-m-u2-m)或`M2`来改变上下面中色向错误的棱块的分配。

对于色向错误的棱块数不为4的情形，其统一的复原步骤如下：

1. 通过至多两次[基本操作三](#基本操作三-m-ux-mx)、[四](#基本操作四-m-ux-mx)，将色向错误的棱块总数转为4个。通过精心选择要执行的基本操作，魔方可以直接转为“箭头”情况。如果没能转为“箭头”，则使用基本操作五、六再做转换。

2. 通过基本操作三或四，解决“箭头”情况，完成色向复原。

> 基本操作三或四能够同时改变4个棱块的色向。除非这四个位置恰好有两个色向错误的棱块，否则基本操作三、四一定会改变色向错误的棱块总数。例如，如果这四个位置有1个色向错误，那么执行完后这四个棱块变为3个色向错误，总数量加2。

共有5类情况，一一介绍：

## 两个棱块色向错误

### "2a-0"

“2a”代表顶层色向错误的两个棱块位置相邻。

执行[基本操作三](#基本操作三-m-ux-mx)如`M' U M'`后，色向正确的棱块总数增加2，转化为“箭头”情形。

<div id="eo-case2a-0">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('ttttttttttttttttttttttttttttttrtttrttttttttttttttttttt')
    ('#eo-case2a-0');
</script>
</div>

<div id="eo-case2a-0">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`U M U2 M U' M U M' U2 M U' M'`)
    .alg(`M' U M' U2 M' U M'`)
    ('#eo-case2a-0');
</script>
</div>

### "2o-0"

“2o”代表顶层色向错误的两个棱块位置相对。

可通过[基本操作三](#基本操作三-m-ux-mx)如`M' U M`转换。

<div id="eo-case2o-0">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('ttttttttttttttttttttttttttttrtttttrttttttttttttttttttt')
    ('#eo-case2o-0');
</script>
</div>

<div id="eo-case2o-0">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`M2 U M U M U' M' U M U M2`)
    .alg(`M' U M U' M' U M'`)
    ('#eo-case2o-0');
</script>
</div>

### "0-2"

可通过[基本操作三](#基本操作三-m-ux-mx)如`M' U M'`转换。

<div id="eo-case0-2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trtttttrtttttttttttttttttttttttttttttttttttttttttttttt')
    ('#eo-case0-2');
</script>
</div>

<div id="eo-case0-2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`M' U' M' U M U M U M U2 M'`)
    .alg(`M' U M' U' M U M'`)
    ('#eo-case0-2');
</script>
</div>

由于0-2与2o-0只差一步`M2`，也可以将2o-0解法的第一步由`M'`替换为`M`。

### "1-1"

有两种情形。

以下朝向下，可执行[基本操作四](#基本操作四-m-ux-mx)如`M' U M`转换：

<div id="eo-case1-1-1">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trttttttttttttttttttttttttttrttttttttttttttttttttttttt')
    ('#eo-case1-1-1');
</script>
</div>

<div id="eo-case1-1-1">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`U M' U' M' U2 M U M U`)
    .alg(`M U M' U M U M'`)
    ('#eo-case1-1-1');
</script>
</div>

另一种的转化方法类似（[基本操作三](#基本操作三-m-ux-mx)，如`M' U M`）:

<div id="eo-case1-1-3">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('tttttttrttttttttttttttttttttttttttrttttttttttttttttttt')
    ('#eo-case1-1-3');
</script>
</div>

<div id="eo-case1-1-3">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`M' U' M' U2 M U M U M U' M'`)
    .alg(`M' U M U M' U M'`)
    ('#eo-case1-1-3');
</script>
</div>

## 六个棱块色向错误

### "4-2"

4-2的所有6个棱块的色向都错误。

无论是执行[基本操作三](#基本操作三-m-ux-mx)还是[基本操作四](#基本操作四-m-ux-mx)，都会导致顶层色向错误的棱块数量减3、底层减1，因此只能先转化为1-1情形，再进一步转化为“箭头”。以下解法首先做基本操作四`M U M'`。

<div id="eo-case4-2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trtttttrttttttttttttttttttttrtrtrtrttttttttttttttttttt')
    ('#eo-case4-2');
</script>
</div>

<div id="eo-case4-2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`U' M' U' M' U2 M U M U M' U' M' U M2 U`)
    .alg(`M U M' U2 M' U M U M' U M'`)
    ('#eo-case4-2');
</script>
</div>

另有一种简便解法：`R U' r' U' M' U r U r`。

<div id="eo-case4-2-3">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`U' M' U' M' U2 M U M U M' U' M' U M2 U`)
    .alg(`R U' r' U' M' U r U r'`)
    ('#eo-case4-2-3');
</script>
</div>

## 复原流程图

直观的色向复原流程图（作者：甘浩東/Anto Kam）如下：

<img src="./images/EO_flowchart.png">

在这张图中，红色棱块代表色向错误的棱块。图中无法看到DB位置棱块的色向，但请记住：色向错误的棱块总数一定为偶数。因此从图中就能足够推断它的色向情况。

阅读时应当将复原流程与基本操作三~六的功能联系起来，理解[基本操作三](#基本操作三-m-ux-mx)、[四](#基本操作四-m-ux-mx)是如何改变色向错误的棱块总数，[基本操作五](#基本操作五-m-u2-m)、[六](#基本操作六-m-u2-m)是如何改变色向错误的棱块的分布的，如同我们在[色向转换原理](#色向转换原理)中所介绍的那样。

注意每次转换完成后，还需要通过`Ux`，调整为图中相应的朝向。