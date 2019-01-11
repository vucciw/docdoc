二叉排序树
>二叉排序树或者是一棵空树，或者是具有下列性质的二叉树：  
（1）若左子树不空，则左子树上所有结点的值均小于它的根结点的值；  
（2）若右子树不空，则右子树上所有结点的值均大于它的根结点的值；  
（3）左、右子树也分别为二叉排序树；  
（4）没有键值相等的节点。  

关于高度Height，有的地方将"空二叉树的高度是-1"，而本文采用维基百科上的定义：树的高度为最大层次。即空的二叉树的高度是0，非空树的高度等于它的最大层次(根的层次为1，根的子节点为第2层，依次类推)。
这是树的高度。

而节点的高度这个是反过来， 叶子节点是1， 其父节点是2，其父节点的父节点是3， 以此类推， 根节点就是树的高度。

 
# LL的旋转
LL: LEFTLEFT, 也称为“左左”。 指根节点的(第一个L)左子树（准确来说是左子节点）的(第二个L)左子树有非空子节点（没有说左右，有任意一个子节点）， 而根节点的右子树没有子节点。这样，根节点的左子树比右子树要高2
LL情况下达到平衡是左单旋转  
```java
private AVLTreeNode<T> leftLeftRotation(AVLTreeNode<T> k2){
    k1 = k2.tree;
    k2.left = k1.right;
    k1.right = k2;

    k2.height = max(height(k2.left), height(k2.right))+1;
    k1.height = max(height(k1.left), k2.height)+1  //也可以写成 max(height(k1.left), height(k1.right))+1 
}
```
我的理解是，在LL前提下，这个前提很重要。前提没了，后面结论也立不住脚了：  
1. 先清除知道k1和k2代表什么， k2代表树的根节点，注意这里树有可能是子树。**也就是出现左右子节点高度之差大于等于2的节点**。
2. k1的右节点都交给k2的左节点，根据二叉排序树的定义，所以原本k1的右节点值肯定比k1节点值大，但比k2节点小。所以这样处理没逻辑问题。
3. k2作为k1的左节点。  
4. 那么k2的高度就要重新计算，就是k2左右节点最大高度+1
5. 那么k1的高度就要重新计算，就是k1左右节点最大高度+1  



注意： 上述的“根节点”不只一个，因为子树也可以是有根节点。

# RR的旋转  
RR: RightRight, 也称为“右右”。 指根节点的右子树的右子树有非空子节点（没有说左右，有任意一个子节点），而根节点的左子树没有子节点。这样，根节点的右子树比左子树高2  
RR情况下达到平衡是右单旋转  
```java
private AVLTreeNode<T> rightRightRotation(AVLTreeNode<T> k1){
    k2 = k1.right;
    k1.right = k2.left;
    k2.left = k1;

    k1.height = max(height(k1.left),height(k1.right))+1;
    k2.height = max(k1.height,height(k2.right))+1;
    return k2;
}
```
我的理解是，在RR前提下，这个前提很重要。前提没了，后面结论也立不住脚了：  
1. 先清除知道k1和k2代表什么， k1代表树的根节点，注意这里树有可能是子树。**也就是出现左右子节点高度之差大于等于2的节点**。
2. k2的左节点都交给k1的右节点，根据二叉排序树的定义，所以原本k2的左节点值肯定比k1节点值大，但比k2节点小。所以这样处理没逻辑问题。
3. k1作为k2的右节点。  
4. 那么k2的高度就要重新计算，就是k2左右节点最大高度+1
5. 那么k1的高度就要重新计算，就是k1左右节点最大高度+1  



# LR的旋转  
LR：LeftRight，也称为“左右”。指根节点的左子树(L)的右子树(R)还有非空子节点，而根节点的右子树没有子节点，导致根节点的左子树比右子树高2
LR情况下达到平衡是需要经过左双旋转，也就是需要经过两次旋转才能让AVL树恢复平衡。  
这里涉及到三个元素，分别是根节点k3， k3的左节点k1， k1的右节点k2，出问题的k3节点，其左右节点高度差为2。这样第一次旋转是围绕k1进行RR旋转，第二次旋转围绕k3进行LL旋转。  
```java
private AVLTreeNode<T> leftRightRotation(AVLTreeNode<T> k3){
    k3.left = rightRightRotation(k3.left);
    return leftLeftRotation(k3);
}
```

我的理解是，在LR前提下，这个前提很重要。前提没了，后面结论也立不住脚了：  
1. 先清除知道k1、k2、k3代表什么， 根节点k3， k3的左节点k1， k1的右节点k2，出问题的k3节点，注意这里树有可能是子树。**也就是出现左右子节点高度之差大于等于2的节点**。
2. 在熟悉掌握LL和RR的情况下，就可以记住第一次先对k1进行RR旋转，这样k3为树根节点就变成LL情况了
3. 此时就可以进行LL旋转。  

# RL的旋转
RL: RightLeft，也称为“右左”，跟LR是对称关系，那么其旋转就是右双旋转，也就是经过两次才能让AVL树恢复平衡。第一次围绕k1进行LL旋转，第二次围绕k3进行RR旋转。
这里涉及到三个元素，分别是根节点k3， k3的右节点k1， k1的左节点k2，出问题的k3节点，其左右节点高度差为2。这样第一次旋转是围绕k1进行LL旋转，第二次旋转围绕k3进行RR旋转。  
```java
private AVLTreeNode<T> rightLeftRotation(AVLTreeNode<T> k3){
    k3.right = leftLeftRotation(k3.right);
    return rightRightRotation(k3);
}
```

我的理解是，在LR前提下，这个前提很重要。前提没了，后面结论也立不住脚了：  
1. 先清除知道k1、k2、k3代表什么， 根节点k3， k3的右节点k1， k1的左节点k2，出问题的k3节点，注意这里树有可能是子树。**也就是出现左右子节点高度之差大于等于2的节点**。
2. 在熟悉掌握LL和RR的情况下，就可以记住第一次先对k1进行LL旋转，这样k3为树根节点就变成RR情况了
3. 此时就可以进行RR旋转。  



这里关键点的是： 
1. 定义和前提，要清楚定义和前提，才会有后面的结论。
2. 中文名称比较让人混淆，如“右单旋转”，看名字以为是往右旋转。是不然，而是围绕某个点，往左旋转，所以我比较喜欢说RR旋转，或许R还是会有点混淆，但起码比中文好理解点。
3. 有没发现树的特点，就是其对称性，LL对应RR，LR对应RL，其方法就是改变下顺序，树的遍历也一样，前序遍历，中序遍历，后序遍历的函数（方法）都是改变下函数内语句的顺序，不需要你重新写一个完全不一样的函数，复用性有点高。  












插入节点的代码
```java
/* 
 * 将结点插入到AVL树中，并返回根节点
 *
 * 参数说明：
 *     tree AVL树的根结点
 *     key 插入的结点的键值
 * 返回值：
 *     根节点
 */
private AVLTreeNode<T> insert(AVLTreeNode<T> tree, T key) {
    if (tree == null) {
        // 新建节点
        tree = new AVLTreeNode<T>(key, null, null);

    } else {
        int cmp = key.compareTo(tree.key);

           if (cmp < 0) {    // 应该将key插入到"tree的左子树"的情况
            tree.left = insert(tree.left, key);
            // 插入节点后，若AVL树失去平衡，则进行相应的调节。
            if (height(tree.left) - height(tree.right) == 2) {
                if (key.compareTo(tree.left.key) < 0)           //左子树出平衡问题的节点的左子节点大于插入节点key，则是LL情况
                    tree = leftLeftRotation(tree);
                else                                            //左子树出平衡问题的节点的左子节点小于插入节点key，则是LR情况
                    tree = leftRightRotation(tree);
            }
        } else if (cmp > 0) {    // 应该将key插入到"tree的右子树"的情况
            tree.right = insert(tree.right, key);
            // 插入节点后，若AVL树失去平衡，则进行相应的调节。
            if (height(tree.right) - height(tree.left) == 2) {
                if (key.compareTo(tree.right.key) > 0)
                    tree = rightRightRotation(tree);            //右子树出平衡问题的节点的右子节点小于插入节点key，则是RR情况
                else
                    tree = rightLeftRotation(tree);             //右子树出平衡问题的节点的右子节点大于插入节点key，则是RR情况
            }
        } else {    // cmp==0
            System.out.println("添加失败：不允许添加相同的节点！");
        }
    }

    tree.height = max( height(tree.left), height(tree.right)) + 1;

    return tree;
}

public void insert(T key) {
    mRoot = insert(mRoot, key);
}
```


删除节点的代码



```java
/* 
 * 删除结点(z)，返回根节点
 *
 * 参数说明：
 *     tree AVL树的根结点
 *     z 待删除的结点
 * 返回值：
 *     根节点
 */
private AVLTreeNode<T> remove(AVLTreeNode<T> tree, AVLTreeNode<T> z) {
    // 根为空 或者 没有要删除的节点，直接返回null。
    if (tree==null || z==null)
        return null;

    int cmp = z.key.compareTo(tree.key);
    if (cmp < 0) {        // 待删除的节点在"tree的左子树"中
        tree.left = remove(tree.left, z);
        // 删除节点后，若AVL树失去平衡，则进行相应的调节。
        if (height(tree.right) - height(tree.left) == 2) {
            AVLTreeNode<T> r =  tree.right;
            if (height(r.left) > height(r.right))
                tree = rightLeftRotation(tree);
            else
                tree = rightRightRotation(tree);
        }
    } else if (cmp > 0) {    // 待删除的节点在"tree的右子树"中
        tree.right = remove(tree.right, z);
        // 删除节点后，若AVL树失去平衡，则进行相应的调节。
        if (height(tree.left) - height(tree.right) == 2) {
            AVLTreeNode<T> l =  tree.left;
            if (height(l.right) > height(l.left))
                tree = leftRightRotation(tree);
            else
                tree = leftLeftRotation(tree);
        }
    } else {    // tree是对应要删除的节点。
        // tree的左右孩子都非空
        if ((tree.left!=null) && (tree.right!=null)) {
            if (height(tree.left) > height(tree.right)) {
                // 如果tree的左子树比右子树高；
                // 则(01)找出tree的左子树中的最大节点
                //   (02)将该最大节点的值赋值给tree。
                //   (03)删除该最大节点。
                // 这类似于用"tree的左子树中最大节点"做"tree"的替身；
                // 采用这种方式的好处是：删除"tree的左子树中最大节点"之后，AVL树仍然是平衡的。
                AVLTreeNode<T> max = maximum(tree.left);
                tree.key = max.key;
                tree.left = remove(tree.left, max);
            } else {
                // 如果tree的左子树不比右子树高(即它们相等，或右子树比左子树高1)
                // 则(01)找出tree的右子树中的最小节点
                //   (02)将该最小节点的值赋值给tree。
                //   (03)删除该最小节点。
                // 这类似于用"tree的右子树中最小节点"做"tree"的替身；
                // 采用这种方式的好处是：删除"tree的右子树中最小节点"之后，AVL树仍然是平衡的。
                AVLTreeNode<T> min = maximum(tree.right);
                tree.key = min.key;
                tree.right = remove(tree.right, min);
            }
        } else {
            AVLTreeNode<T> tmp = tree;
            tree = (tree.left!=null) ? tree.left : tree.right;
            tmp = null;
        }
    }

    return tree;
}

public void remove(T key) {
    AVLTreeNode<T> z; 

    if ((z = search(mRoot, key)) != null)
        mRoot = remove(mRoot, z);
}

```

参考：https://www.cnblogs.com/skywang12345/p/3577479.html