---
title: 树及其操作（C语言版）
date: 2018-11-09 16:49:37
tags: [数据结构,树]
categories: 数据结构与算法
mathjax: true
---

# 树及其操作（C语言版）

### 树定义和性质：

1.  **树的定义：** 树是n个（n>=0）有限节点组成一个具有层次关系的集合。

2.  **根及子树：** 在任意一个非空树中，有且仅有一个特定的称为根的结点，当n>1时，其余结点又可以分为m（m>0）个互不相交的有限集$ T_1,T_2,...T_m$,其中每一个集合本身又是一颗树，并称为根的子树。

3.  **基本操作：**
 - InitTree(&T):构造一颗空树
 - DestroyTree(&T):销毁一颗树
 - ClearTree(&T):清空一颗树
 - TreeDepth(&T):求一颗树的深度
 - TraverseTree(&T,Visit()):遍历树 

4. **结点：** 树中包含一个数据元素和若干指向其子树的分支的集合叫做树的结点

5. **度：** 结点拥有的子树称为结点的度

6. **叶子(终端结点)：** 度为0的结点称为终端结点或者是叶子

7. **分支结点(非终端结点)：** 度不为0的结点称为分支结点或者是非终端结点。

8. **孩子/双亲/兄弟：** 结点的子树称为该结点的孩子，相应的，该结点称为孩子的双亲，同一个双亲的孩子互称兄弟。

9. **祖先/子孙** 结点的祖先是从根到该节点所经过的分支上的所有结点，反之，以某结点为根的子树中的任意结点都是该节点的子孙。

10. **结点的层次：** 结点的层次从根开始算起，根为第一层，根的孩子为第二层.....

11. **树的深度：** 树中结点的最大层次称为树的深度或者是高度
 
12. **有序树/无序树：** 如果树中结点的各子树看成从左到右是有次序的（不能互换），则树称为有序树，否则称为无序树。
    
13. **森林：** m（m>=0）颗互不相交的树的集合

### 二叉树：

1. **定义：** 树中每个结点最多只有两颗子树（即结点的度不大于2），而且子树有左右之分（子树次序不能任意颠倒）的树称为二叉树。

2. **满二叉树：** 一颗深度为$k$且有$2^k-1$个结点的树称为满二叉树

3. **完全二叉树：** 深度为$k$，有n个结点的二叉树，当且仅当其每一个结点都与深度为$k$的满二叉树中编号从1至n的结点一一对应时，称之为完全二叉树。 

4. **二叉树性质**：
 - 在二叉树的第$i$层上最多有$2^(i-1)$个结点（i>=1）
 - 深度为$k$的二叉树最多有$2^k-1$个结点（k>=1）
 - 对任何一个二叉树T，如果其终端结点数为$n_0$,度为2的结点数为$n_2$,则$n_0 = n_2+1$

5. **完全二叉树的性质**
 - 具有n个结点的完全二叉树的深度为$\lfloor log_(2^n) \rfloor+1$

### 线索二叉树：

1. 定义：若做如下规定：若结点有左子树，则其lchild域指示其左孩子，否则令其指向其前驱，同样的，若结点有右子树，则其rchild域指向其右孩子，否则令其指向其后继。为了区分，我们增设一个tag字段。

$$  LTag =
\begin{cases}
0  & \text{lchild域指示结点的左孩子} \\
1 & \text{lchild域指示结点的前驱}
\end{cases}$$
$$  RTag =
\begin{cases}
0  & \text{lchild域指示结点的右孩子} \\
1 & \text{lchild域指示结点的后继}
\end{cases}$$
以这种结点结构构成的二叉链表表示的二叉树叫做线索二叉树，其中指向前驱和后继的指针叫做线索。

2. 线索化：以某种遍历顺序使二叉树称为线索二叉树的过程叫做二叉树的线索化

### 树的存储结构：

1. 双亲表示法：

以一组连续的空间存储树的结点，同时在每一个结点中附设一个指示器指示其双亲结点在链表中的位置

```c
#define MAX_TREE_SIZE 100
typedef struct PTNode{//结点结构
    TElemTye data;//数据元素类型
    int parent;//双亲结点域
}PTNode;

typedef struct {//树结构
    PTNode nodes[MAX_TREE_SIZE];//存储的结点
    int r,n;//根的位置和节点数
}PTNode;
```

2. 孩子表示法：

把每个结点的孩子结点排列起来，看成一个线性表，并且以单链表做存储结构，则n个结点有n个孩子链表（叶节点的孩子链表为空表），而n个头结点又组成一个线性表。

```c
#define MAX_TREE_SIZE 100
typedef struct CTNode{//孩子结点
    int child;//孩子数
    struct CTNode* next;//下一个孩子结点指针
}* ChildPtr;

typedef struct{
    TElemType data;//数据域
    ChildPtr firstChild;//孩子链表头指针
}CTBox;

typedef struct{
    CTBox nodes[MAX_TREE_SIZE];//存储的结点
    int n,r;//结点和根的位置
```

3. 孩子兄弟表示法：

又称二叉树表示法，或者二叉链表表示法。链表中结点的两个链域分别指向该节点的第一个孩子结点和下一个兄弟结点

```c
typedef struct CSNode{
    TElemType data;
    struct CSNode *firstChild, *nextSibling;
}CSNode , *CSTree;
```

### 树的实现：

```c
#include <stdio.h>
#include <malloc.h>

/**
 *定义的树节点
 */
typedef struct BiTNode{
    char data;
    struct BiTNode *Lchild,*Rchild;
}BiTNode, *BiTree;


/**
递归方式，先序遍历建立二叉树
**/
bool createBiTree(BiTree *T){
    char ch;
    //读取字符
    scanf("%c",&ch);

    if(ch == ' '){
        *T = NULL;
    }else{
        //申请新的结点空间
        if(!(*T = (BiTNode *)malloc(sizeof(BiTNode)))){
            exit(0);
        }
        (*T)->data = ch;
        createBiTree(&((*T)->Lchild));
        createBiTree(&((*T)->Rchild));
    }
    return true;
}

/*先序遍历*/
bool preOrderTraverse(BiTree T,bool (*printElem)(char elem)){
    //检验是否为空
    if(T){
        if(printElem(T->data)){//打印根元素的值
            if(preOrderTraverse(T->Lchild,printElem)){//打印左子树
                if(preOrderTraverse(T->Rchild,printElem)){//打印右子树
                    return true;
                }
            }
        }
        return false;
    }else{
        return true;
    }

}

/**
 * 打印函数
 */
bool printElem(char elem){
    printf("%c\n",elem);
    return true;
}

int main()
{   BiTree tree;
    bool res;
    res = createBiTree(&tree);
    if(res){
        printf("建立成功\n");
    }else{
        printf("建立失败\n");
    }
    //先序遍历打印树
    preOrderTraverse(tree,printElem);
    return 0;
}





```