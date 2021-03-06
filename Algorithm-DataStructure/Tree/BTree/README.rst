++++++++++++++++++++++++++++++++++++++++++++++++
B Tree
++++++++++++++++++++++++++++++++++++++++++++++++

B树的定义方法
=============================================
B树有两种定义方法，一种是按度数(degree)，一种是按阶数(order)。设B树的度数为t，那么结点至少有t-1个key，至少有t个子结点，至多有2t-1个key，至多有2t个子结点；设B树的阶为m，那么每个结点至多有m个子结点，至多有m-1个key，至少有ceil(m/2)个子结点，至少有ceil(m/2)-1个key。

删除
--------------------------------------
从结点中删除key时，我们需要确保结点中的key数量不能太少(root结点除外)。与插入过程类似，我们在向下遍历的过程中，要保证每一个结点至少有t个key(注意，比最小key数t-1要多1)，因为之后可能会需要把结点下放到子结点中去。

假设我们递归地对结点调用删除函数，并不断向下前进，思考以下几种情况：

1. 若键k在叶子结点x中，将其从x中删除即可
2. 若键k在内部结点x中，考虑以下情况：
    - 若紧挨着键k的前一个子结点y至少有t个key，那么找出以y为根的子树中k的前趋k`。递归地删除k`，并用k`代替x中的k。
    - 若y中不足t个key，对称的，检查紧挨着k的下一个子结点z。若z有至少t个key，那就在以z根的子树中找出k的后继k`。递归地删除k`，并用k`替代k。
    - 如果y和z都只有t-1个key，将k与z中的所有key都合并到y中，这样x结点失去了k和指向z的指针，y现在包含2t-1个key。将z释放，并递归地从y中删除k。
3. 若k不在内部结点x中，确定包含k的子树的根x.c(i)(如果k的确存在于整个B树中的话)。如果该子树x.c(i)只有t-1个key，执行下面的其中一种步骤：
    - 如果x.c(i)有一个紧挨着的同级结点拥有至少t个key，那么从x中下移一个key到x.c(i)中，然后从那个同级节点中上移一个key和一个子结点指针到x中。
    - 如果x.c(i)紧邻的2个同级结点都只有t-1个key，将x.c(i)与其中一个合并(需要从x中下移一个key，与分裂正好是逆操作)
