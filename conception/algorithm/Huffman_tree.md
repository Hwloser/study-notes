## 相关名词：

路径：在一棵树中，一个结点到另一个结点之间的通路，称为路径。

路径长度：在一条路径中每经过一个结点，路径长度都要加1。

结点的权：给每个结点赋予一个新的数值，被称为这个结点的权。

结点的带权路劲的长度：指的是从根节点到该节点之间的路劲之间的路径长度于该结点的权的乘积。

树的带权路径长度为树中所有叶子结点的带权路径长度之和。通常基座"WPL"。

## 一、哈夫曼树

1. 哈夫曼树简介

哈夫曼树(Huffman)又称最优二叉树，是指对于一组带有确定权值的叶子节点所构造的具有带权路径长度最短的二叉树。从树中的一个结点到另一个结点之间的分支构成了两结点之间的路径，路径上的分支个数称为路径长度。二叉树的路径长度是指由根节点到所有叶子节点的路径长度之和。如果二叉树中的叶子结点由一定的权值，则可将这一概念拓展：设二叉树具有n个带权值的叶子结点，则从根节点到每一个叶子结点的路径长度于该叶子结点权值的乘积之和称为二叉树路径长度，记作：WPL = W1L1 + W2L2 + ...... + WnLn;

在构建哈夫曼树时，要使树的带权路径长度最小，只需要遵循一个原则，那就是：权重越大的结点离树根越近。