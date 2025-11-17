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

<div id="eo-eg">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwywwywggggggolorlrrrrglgyyywwyyyybbbbbbrlrolooooblb')
    ('#eo-eg');
</script>
</div>

在下图中，色向错误的棱块数量为4。（注意底层有两个）

<div id="eo-eg2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wowwywwowgggggggbgryrrorryryryywyyrybbbbbbbgbowooroowo')
    ('#eo-eg2');
</script>
</div>

为了直观，我们只使用红色标出上下两层中，色向错误的棱块面的位置。它不一定代表真正的红色面。

<div id="eo-eg3">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trtttttrttttttttttttttttttttrtttttrttttttttttttttttttt')
    ('#eo-eg3');
</script>
</div>

中心块的色向通过一步`Mx`就能解决，我们先还原它们，再处理剩下6个棱块的色向。

<div id="eo-c">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wlwwywwlwgggggglllrlrrlrlllylylwlylybbbbbbllloloololll')
    .case(`M'`)
    ('#eo-c');
</script>
</div>

> 在这里，黄色中心块在顶层并不比在底层更好。

下面的介绍中，通过`Ux`能够相互转换的情形视为同一种情形。

## 恰有4个色向错误的棱块的复原

### 箭头(Arrow,或3-1)

箭头的顶层有3个色向错误的棱块，底层有1个。通过做`Ux`可调整其形态如下：

箭头一：

<div id="arrow1">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('tttttttrttttttttttttttttttttrtrtrttttttttttttttttttttt')
    ('#arrow1');
</script>
</div>

箭头二：

<div id="arrow2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trttttttttttttttttttttttttttttrtrtrttttttttttttttttttt')
    ('#arrow2');
</script>
</div>

不管哪种箭头，上面的朝向使得4个色向错误的棱块分为两对：一对左右相对，一对上下相对。

为了解决箭头的情形，我们引入两类基本操作：

#### 基本操作三：	`M' Ux Mx`

这里，`Ux`被限制为`U`或`U'`，`Mx`也被限制为`M'`或`M`，因此共有4种组合。其作用是改变箭头一中标红位置的棱块的色向，例如：

<div id="base3">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('tttttttrttttttttttttttttttttrtrtrttttttttttttttttttttt')
    .alg(`M' U M'`)
    ('#base3');
</script>
</div>

或者：

<div id="base3-2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('tttttttrttttttttttttttttttttrtrtrttttttttttttttttttttt')
    .alg(`M' U' M`)
    ('#base3-2');
</script>
</div>

等等。可以看到执行完基本操作三后，4个红色面都移向了侧面，意味着其对应棱块的色向已改变。剩余两个棱块虽然没有标出，但它们的移动效果和`M0`或`M2`是一样的，因此色向不会变化。

#### 基本操作四：	`M Ux Mx`

与基本操作三相比，其第一步换成了`M`，而`Ux`与`Mx`同样被限制为90°转动。其作用是改变箭头二中标红位置的棱块的色向，例如：

<div id="base4">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trttttttttttttttttttttttttttttrtrtrttttttttttttttttttt')
    .alg(`M U M'`)
    ('#base4');
</script>
</div>

因此，基本操作三和基本操作四能够改变特定的四个棱块的色向，并能直接解决箭头的情形。

箭头一-基本操作三:

<div id="arrow1-1">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`U' M' U2 M U' M U M2 U M U2 M2 U'`)
    .alg(`M' U M'`)
    ('#arrow1-1');
</script>
</div>

箭头二-基本操作四：

<div id="arrow2-1">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`M U M2 U2 M2 U2 M U2`)
    .alg(`M U M'`)
    ('#arrow2-1');
</script>
</div>

“箭头”是最简单的情形。后续所有的情形，都将先转化为“箭头”情形，然后复原。

### 2a-2

2a-2情形的含义为："2a"代表顶层有2个色向错误的棱块，并且位置相邻(adjacent)；"-2"代表底层有2个色向错误的棱块。后边情形的命名方式都与此类似。

<div id="case2a-2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trtttttrttttttttttttttttttttttrtttrttttttttttttttttttt')
    ('#case2a-2');
</script>
</div>

和“箭头”相比，2a-2的顶层少了一个色向错误的棱块，而底层多了一个。

顶层上位于M层的两个棱块中，恰有一个色向错误。如果做`M2`，将改变M层的四个棱块的上下位置。这样，色向错误的棱块数不变（4个），而底层将恰有一个色向错误的棱块，因此转化为“箭头”情形。

<div id="case2a-2-1">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trtttttrttttttttttttttttttttttrtttrttttttttttttttttttt')
    .alg(`M2`)
    ('#case2a-2-1');
</script>
</div>

进而可以得出一种可行的解法：

<div id="case2a-2-2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`U2 M U' M2 U2 M U' M' U M' U`)
    .alg(`M2 U' M' U M'`)
    ('#case2a-2-2');
</script>
</div>

### 2o-2

2o-2情形下，"2o"代表顶层有2个色向错误的棱块，并且位置相对(opposite)；"-2"代表底层有2个色向错误的棱块。

<div id="case2o-2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trtttttrttttttttttttttttttttttrtrttttttttttttttttttttt')
    ('#case2o-2');
</script>
</div>

与2a-2情形一样，2o-2中色向错误的棱块数也是4个。对于2a-2，我们使用`M2`让顶层和底层的四个棱块两两互换，从而将顶层色向错误的棱块数改变为3，转化为“箭头”。这里用同样的办法就不能奏效了。

我们再引入两类基本操作，以完成上下两个棱块的单独互换，并且不改变所有棱块的色向：

#### 基本操作五：	`M' U2 M`

将M层与F层（前面）交界的两个棱块（UF位、DF位）位置互换。

<div id="base5">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('tttttttrtttttttttttttttttrtttttttttttttttttttttttttttt')
    .alg(`M' U2 M`)
    ('#base5');
</script>
</div>

可以发现,交换位置的两个棱块的色向都没有改变，即一个正确、一个错误。

#### 基本操作六：	`M U2 M'`

将M层与B层（后面）交界的两个棱块（UB位、DB位）位置互换。

<div id="base6">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trttttttttttttttttttttttttttttttttttttttttttttttttttrt')
    .alg(`M U2 M'`)
    ('#base6');
</script>
</div>

因此，当两个互相交换的棱块有不同的色向时，这两类基本操作能将顶层色向错误的棱块数增加或减少一个，同时保证色向错误的棱块总数不变。

对于2o-2情形，可以做基本操作六：

在F面上，一个底层色向错误的棱块与一个顶层色向正确的棱块交换，便转化为“箭头”。（基本操作五同样可行，原理类似）

<div id="case2o-2-1">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trtttttrttttttttttttttttttttttrtrttttttttttttttttttttt')
    .alg(`M U2 M'`)
    ('#case2o-2-1');
</script>
</div>

一种可行的解法如下：

<div id="case2o-2-2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`M U2 M' U2 M' U' M U2 M2 U M2 U'`)
    .alg(`M U2 M' U2 M' U M'`)
    ('#case2o-2-2');
</script>
</div>

### 4-0

4-0情形下，顶层有4个色向错误的棱块，而底层没有。

可以想象，`M2`就能将4-0转为2o-2的情形。因此将2o-2解法的第一步由`M`改为`M'`，就能解决这类情形。

<div id="case4-0">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('ttttttttttttttttttttttttttttrtrtrtrttttttttttttttttttt')
    ('#case4-0');
</script>
</div>

<div id="case4-0">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`M' U2 M' U2 M' U' M U2 M2 U M2 U'`)
    .alg(`M' U2 M' U2 M' U M'`)
    ('#case4-0');
</script>
</div>

## 其他情形的复原

### 色向转换原理

我们已经展示了恰有4个色向错误的棱块的情形，转化为[“箭头”](#箭头arrow或3-1)情形的办法，即通过[基本操作五](#基本操作五m-u2-m)、[六](#基本操作六m-u2-m)或`M2`来改变上下面中色向错误的棱块的分配。

对于色向错误的棱块数不为4的情形，其统一的复原步骤如下：

1. 通过至多两次[基本操作三](#基本操作三-m-ux-mx)、[四](#基本操作四-m-ux-mx)，将色向错误的棱块总数转为4个。通过精心选择要执行的基本操作，魔方可以直接转为“箭头”情况。如果没能转为“箭头”，则使用基本操作五、六再做转换。

2. 通过基本操作三或四，解决“箭头”情况，完成色向复原。

> 基本操作三或四能够同时改变4个棱块的色向。除非这四个位置恰好有两个色相错误的棱块，否则基本操作三、四一定会改变色向错误的棱块总数。例如，如果这四个位置有1个色向错误，那么执行完后这四个棱块变为3个色向错误，总数量加2。

共有5类情况，一一介绍：

### 2a-0

“2a”代表顶层色向错误的两个棱块位置相邻。

执行[基本操作三](#基本操作三-m-ux-mx)如`M' U M'`后，色向正确的棱块总数增加2，转化为“箭头”情形。

<div id="case2a-0">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('ttttttttttttttttttttttttttttttrtttrttttttttttttttttttt')
    ('#case2a-0');
</script>
</div>

<div id="case2a-0">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`U M U2 M U' M U M' U2 M U' M'`)
    .alg(`M' U M' U2 M' U M'`)
    ('#case2a-0');
</script>
</div>

### 2o-0

“2o”代表顶层色向错误的两个棱块位置相对。

可通过[基本操作三](#基本操作三-m-ux-mx)如`M' U M`转换。

<div id="case2o-0">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('ttttttttttttttttttttttttttttrtttttrttttttttttttttttttt')
    ('#case2o-0');
</script>
</div>

<div id="case2o-0">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`M2 U M U M U' M' U M U M2`)
    .alg(`M' U M U' M' U M'`)
    ('#case2o-0');
</script>
</div>

### 0-2

可通过[基本操作三](#基本操作三-m-ux-mx)如`M' U M'`转换。

<div id="case0-2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trtttttrtttttttttttttttttttttttttttttttttttttttttttttt')
    ('#case0-2');
</script>
</div>

<div id="case0-2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`M' U' M' U M U M U M U2 M'`)
    .alg(`M' U M' U' M U M'`)
    ('#case0-2');
</script>
</div>

由于0-2与2o-0只差一步`M2`，也可以将2o-0解法的第一步由`M'`替换为`M`。

### 1-1

有两种情形。

以下朝向下，可执行[基本操作四](#基本操作四-m-ux-mx)如`M' U M`转换：

<div id="case1-1-1">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trttttttttttttttttttttttttttrttttttttttttttttttttttttt')
    ('#case1-1-1');
</script>
</div>

<div id="case1-1-1">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`U M' U' M' U2 M U M U`)
    .alg(`M U M' U M U M'`)
    ('#case1-1-1');
</script>
</div>

另一种的转化方法类似（[基本操作三](#基本操作三-m-ux-mx)，如`M' U M`）:

<div id="case1-1-3">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('tttttttrttttttttttttttttttttttttttrttttttttttttttttttt')
    ('#case1-1-3');
</script>
</div>

<div id="case1-1-3">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`M' U' M' U2 M U M U M U' M'`)
    .alg(`M' U M U M' U M'`)
    ('#case1-1-3');
</script>
</div>

### 4-2

4-2的所有6个棱块的色向都错误。

无论是执行[基本操作三](#基本操作三-m-ux-mx)还是[基本操作四](#基本操作四-m-ux-mx)，都会导致顶层色向错误的棱块数量减3、底层减1，因此只能先转化为1-1情形，再进一步转化为“箭头”。以下解法首先做基本操作四`M U M'`。

<div id="case4-2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trtttttrttttttttttttttttttttrtrtrtrttttttttttttttttttt')
    ('#case4-2');
</script>
</div>

<div id="case4-2">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`U' M' U' M' U2 M U M U M' U' M' U M2 U`)
    .alg(`M U M' U2 M' U M U M' U M'`)
    ('#case4-2');
</script>
</div>

另有一种简便解法：`R U' r' U' M' U r U r`。

<div id="case4-2-3">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`U' M' U' M' U2 M U M U M' U' M' U M2 U`)
    .alg(`R U' r' U' M' U r U r'`)
    ('#case4-2-3');
</script>
</div>

## 复原流程图

直观的色向复原流程图（作者：甘浩東/Anto Kam）如下：

<img src="./images/EO_flowchart.png">

在这张图中，红色棱块代表色向错误的棱块。图中无法看到DB位置棱块的色向，但请记住：色向错误的棱块总数一定为偶数。因此从图中就能足够推断它的色向情况。

不要去硬记每种情形的完整转换公式。应当将复原流程与基本操作三~六的功能联系起来，理解[基本操作三](#基本操作三-m-ux-mx)、[四](#基本操作四-m-ux-mx)是如何改变色向错误的棱块总数，[基本操作五](#基本操作五-m-u2-m)、[六](#基本操作六-m-u2-m)是如何改变色向错误的棱块的分布的，如同我们在[色向转换原理](#色向转换原理)中所介绍的那样。

注意每次转换完成后，还需要通过`Ux`，调整为图中相应的朝向。