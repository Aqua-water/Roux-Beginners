<script type="text/javascript" src="/twistysim.js"></script>
<style type="text/css" rel="stylesheet">
/* modifies the opacity of the cube wireframe */
.ttk-shp-poly {
    stroke-opacity: 0.3;
}
</style>

# 后六棱色向

## 目标

这步要将顶层和底层上剩余的块的色向还原正确，主要解决的是剩下6个棱块的色向。这步又叫做EO (Edges Oriented)。

“色向”这个词在CMLL中已经提及过。所谓块的色向(orientation)，在这里指的是块沿着上下方向的朝向。例如，某个块含有黄色面，在其朝向正确时，黄色面是向上的。而在它色向正确时，黄色面不仅可以朝上，还可以朝下。

以下是一个色向还原正确的具体例子。你可以拖动魔方，查看底层的状态：

<div id="eo-eo-eg">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwywwywggggggolorlrrorglgyyywwyyyybbbbbbrlrolooroblb')
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

为了方便，我们称色向错误的棱块为**坏块**，色向正确的棱块为**好块**。

## 基本操作

我们介绍两类基本操作。

基本操作三、四为一类，主要作用是改变4个棱块的色向，其中3个位于顶层，1个位于底层。

基本操作五、六为一类，作用是交换上下层各1个棱块的位置，但不改变色向。

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

等等。可以看到执行完基本操作三后，4个红色面都从上/下面移向了侧面，意味着其对应棱块的色向已改变。当这4个棱块都是“坏块”时，执行操作后会都变为“好块”。

相应地，如果对应棱块是“好块”，执行操作后会变为“坏块”：

<div id="eo-base3-3">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .alg(`M' U' M`)
    ('#eo-base3-3');
</script>
</div>

因此，这类基本操作能够改变“坏块”的总数量。如果对应的4个棱块恰好有`x`个“坏块”、`(4 - x)`个“好块”，那么执行操作后，这4个棱块将恰有`(4 - x)`个“坏块”，总体数量的变化为`(4 - x) - x = 4 - 2x`。除非`x = 2`，否则操作一定会改变“坏块”的总数。例如，如果这四个位置有1个色向错误，那么执行完后这四个棱块变为3个色向错误，总数量加2。

### 基本操作四：	`M Ux Mx`

基本操作四是基本操作三的镜像。与基本操作三相比，其第一步换成了`M`，而`Ux`与`Mx`同样被限制为90°转动。其作用是改变除UF、DF两个位置外，其余的4个棱块的色向：

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

### 基本操作五：	`M' U2 M`

将M层与F层交界的两个棱块（UF位、DF位）位置互换。

<div id="eo-base5">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('tttttttrtttttttttttttttttrtttttttttttttttttttttttttttt')
    .alg(`M' U2 M`)
    ('#eo-base5');
</script>
</div>

可以发现,两个棱块虽然交换了位置，但色向都没有改变。

因此，这类基本操作能够改变色向的分布，但不能改变“坏块”的总数量。

### 基本操作六：	`M U2 M'`

基本操作六是基本操作五的镜像，它将M层与B层交界的两个棱块（UB位、DB位）位置互换。

<div id="eo-base6">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('trttttttttttttttttttttttttttttttttttttttttttttttttttrt')
    .alg(`M U2 M'`)
    ('#eo-base6');
</script>
</div>

## 基本情形

“坏块”（色向错误的棱块）的总数量一定是偶数，即0、2、4或6个。下面介绍的基本情形分别有4、2、6个“坏块”，使用[基本操作三](#基本操作三-m-ux-mx)和[基本操作四](#基本操作四-m-ux-mx)进行相互转化：

### "3-1"(箭头)

箭头(Arrow)的“坏块”总数为4，顶层有3个“坏块”，底层有1个。此为"3-1"的含义。通过做`Ux`可调整其形态为以下二者之一：

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

两种箭头互为镜像，区别在于，两个色向正确的棱块是位于后面(UB、DB)还是前面（UF、DF），分别称为“箭头一”和“箭头二”。显然，[基本操作三](#基本操作三-m-ux-mx)和[基本操作四](#基本操作四-m-ux-mx)能直接解决箭头的情形。

“箭头”是最简单的情形。其余所有的情形，都将先转化为“箭头”情形，然后复原。

### "1-1"

"1-1"的“坏块”总数为2，顶层和底层各自有1个“坏块”。有两种互为镜像的情形。

以下朝向下，位于顶层的“坏块”不会被[基本操作三](#基本操作三-m-ux-mx)影响，但底层的“坏块”会。因此被影响的“坏块”数`x = 1`，基本操作三增加的“坏块”数为`(4 - 2x) = 2`。这意味着执行基本操作三后恰好有4个“坏块”。

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
    .alg(`M' U M U`)
    ('#eo-case1-1-3');
</script>
</div>

由上可见变为了“箭头一”情形，再使用基本操作三解决即可。

1-1的另一种情形如下，可执行[基本操作四](#基本操作四-m-ux-mx)如`M' U M`转换：

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
    .alg(`M U M' U`)
    ('#eo-case1-1-1');
</script>
</div>

变为“箭头二”情形，再使用基本操作四解决即可。

### "4-2"

4-2的“坏块”总数为6，也是唯一一种“坏块”数量为6的情形。

无论是执行[基本操作三](#基本操作三-m-ux-mx)还是[基本操作四](#基本操作四-m-ux-mx)，都会将4个“坏块”转化为“好块”，“坏块”总数变为2。例如，基本操作四`M U M'`会将其转化为"1-1"的第一种情况：

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
    .alg(`M U M' U2`)
    ('#eo-case4-2');
</script>
</div>

按照之前的介绍，"1-1"可进一步转化为“箭头”，从而复原。

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

### 转换原理

无论“坏块”数量为多少，我们都已经有了至少一类情形的解法。因此，其余的情形可以通过[基本操作五](#基本操作五-m-u2-m)、[六](#基本操作六-m-u2-m)，在“坏块”数不变的同时，转化为已知情形。（显然，只有将一个“坏块”与一个“好块”交换位置，才会改变情形。）

> “坏块”数为2时，实际上总是可以通过一次[基本操作三](#基本操作三-m-ux-mx)、[四](#基本操作四-m-ux-mx)转化为“箭头”，而不必先转化为"1-1"，但转换规律会更难理解一些。可以结合[复原流程图](./lse.md#色向复原流程)来学习。

下面分别演示：

## 四个“坏块”

目标是转化为“箭头”。

### "2a-2"

"2a"代表顶层的2个“坏块”位置相邻(adjacent)。底层也有两个“坏块”。以下朝向下，通过[基本操作五](#基本操作五-m-u2-m)可转化为“箭头二”：

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
    .alg(`M' U2 M U'`)
    ('#eo-case2a-2');
</script>
</div>

使用`M2`转换更加高效：

<div id="eo-case2a-3">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`U2 M U' M2 U2 M U' M' U M' U`)
    .alg(`M2 U'`)
    ('#eo-case2a-3');
</script>
</div>

### "2o-2"

"2o"代表顶层有2个色向错误的棱块，并且位置相对(opposite)。通过[基本操作六](#基本操作六-m-u2-m)可转化为“箭头一”：

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
    .alg(`M U2 M' U2`)
    ('#eo-case2o-2');
</script>
</div>

### "4-0"

"4-0"中，顶层有4个“坏块”，而底层没有。通过[基本操作五](#基本操作五-m-u2-m)可转化为“箭头一”：

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
    .alg(`M' U2 M U2`)
    ('#eo-case4-0');
</script>
</div>

## 两个“坏块”

目标是转化为"1-1"情形。

### "2a-0"

"2a-0"中，顶层两个“坏块”位置相邻，底层没有“坏块”。通过[基本操作五](#基本操作五-m-u2-m)可转化为"1-1"：

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
    .alg(`U' M' U2 M U'`)
    ('#eo-case2a-0');
</script>
</div>

也可以使用一次[基本操作三](#基本操作三-m-ux-mx)直接转化为“箭头”情形：

<div id="eo-case2a-1">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`U M U2 M U' M U M' U2 M U' M'`)
    .alg(`M' U M' U2`)
    ('#eo-case2a-1');
</script>
</div>

### "2o-0"

"2o-0"中，顶层两个“坏块”位置相对，底层没有“坏块”。通过[基本操作五](#基本操作五-m-u2-m)可转化为"1-1"：

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
    .alg(`M' U2 M`)
    ('#eo-case2o-0');
</script>
</div>

也可通过[基本操作三](#基本操作三-m-ux-mx)直接转化为“箭头”情形：

<div id="eo-case2o-1">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`M2 U M U M U' M' U M U M2`)
    .alg(`M' U M U'`)
    ('#eo-case2o-1');
</script>
</div>

### "0-2"

"0-2"中，顶层没有“坏块”，底层有两个“坏块”。通过[基本操作六](#基本操作六-m-u2-m)可转化为"1-1"：

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
    .alg(`M U2 M'`)
    ('#eo-case0-2');
</script>
</div>

也可通过[基本操作三](#基本操作三-m-ux-mx)直接转化为“箭头”情形：

<div id="eo-case0-2-1">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    .case(`M' U' M' U M U M U M U2 M'`)
    .alg(`M' U M' U'`)
    ('#eo-case0-2-1');
</script>
</div>