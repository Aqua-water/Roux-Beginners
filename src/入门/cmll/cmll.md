<script type="text/javascript" src="/twistysim.js"></script>
<style type="text/css" rel="stylesheet">
/* modifies the opacity of the cube wireframe */
.ttk-shp-poly {
    stroke-opacity: 0.3;
}
</style>

# CMLL

## 目标

这步要还原顶层的四个角块的朝向和相对位置，如图所示。CMLL的含义是：复原最后一层的角块，而不关心M层的情况(**C**orners (regardless of **M** slice) of **L**ast **L**ayer)。

完成后，顶层可以自由旋转，角块不必一定和左右桥对齐。

<div id="CMLL">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wtwwtwwtwgggggggtgrtrrtrrtrytytttytybbbbbbbtbotootooto')
    ('#CMLL');
</script>
</div>

我们将CMLL分为两个步骤：

## 复原四角色向

这步要将顶层的四个角块在顶层上的颜色还原正确，也就是黄色一致朝上。

<div id="CMLL-o">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wtwwtwwtwggggggltlrtrrtrltlytytttytybbbbbbltlotootoltl')
    ('#CMLL-o');
</script>
</div>

## 复原角块相对位置

这步将彻底完成CMLL步骤，也就是将顶层的四个角块的相对位置也还原正确。

## 公式

这两个步骤的具体内容将在以下两节介绍。我们将引入**公式**（在本教程中，指的是4步以上的转动序列）。

你可以选择最少的记忆量（1条公式：公式〇），但会多很多转动步数，并且需要理解如何叠加公式。

你也可以记忆全部9条公式（公式一~三、进阶公式一~六），避免叠加造成多余步数。

我们将主要介绍折中的方案（3条公式：公式一~三），并顺带提及前面两种方案。然而，当你对桥式复原法较为熟练之后，建议将9条公式全部掌握。