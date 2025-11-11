<script type="text/javascript" src="/twistysim.js"></script>
<style type="text/css" rel="stylesheet">
/* modifies the opacity of the cube wireframe */
.ttk-shp-poly {
    stroke-opacity: 0.3;
}
</style>

# 后六棱

## 目标

这步的目标是将剩下的6个棱块（以灰色标出）复原，从而复原整个魔方。后六棱又叫做LSE(Last Six Edges)、L6E。

<div id="lse">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wlwwwwwlwggggggglgrlrrrrrlrylylylylybbbbbbblbolooooolo')
    ('#lse');
</script>
</div>

与CMLL类似，我们将后六棱分为两个步骤：

## 后六棱色向

这步要将顶层和底层上剩余的块的色向还原正确，也就是白色和黄色一致朝上或朝下。下面是一个色向还原的例子。

<div id="eo">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwywwywggggggolorlrrrrglgyyywwyyyybbbbbbrlrolooooblb')
    ('#eo');
</script>
</div>

## 后六棱归位

这步要在色向还原的基础上，将剩下的6个棱块彻底复原，从而完成整个魔方的复原。

<div id="done">
<script type="text/javascript">
  TTk.AlgorithmPuzzle(3)
    .size({width:300, height:300})
    .fc('wwwwwwwwwgggggggggrrrrrrrrryyyyyyyyybbbbbbbbbooooooooo')
    ('#done');
</script>
</div>

这两个步骤的具体内容将在以下两节介绍。