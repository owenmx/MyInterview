#### 确定面试过程中应该交流的内容

#### 340 至多包含k个不同字符串的子串

#### 二叉树转化为链表，要求后序遍历的顺序

#### 二叉树的序列化和反序列化，要求尽可能减少存储空间。

#### 230. 二叉搜索树中第K小的元素，进阶版问题，**面对多个查询如何优化**，这一类问题如何优化

#### 222. 完全二叉树的节点个数，位运算

#### 1110. 删点成林

#### 98. 验证二叉搜索树

#### 572. 另一个树的子树，序列化二叉树，利用KMP方法进行匹配。

#### 如果超时，试试二分法或者dp。

#### 450. 删除二叉搜索树中的节点

#### 剑指 Offer 38. 字符串的排列

#### 80. 删除有序数组中的重复项 II

#### 双栈解决基础计算器问题

#### 二叉树公共祖先 多个查询

#### 155 最小栈 除了利用一个辅助栈之外，是否存在其他的实现方法。

#### 378. 有序矩阵中第 K 小的元素 二分解法

#### 719. 找出第 k 小的距离对，二分解法

#### 5727. 找出游戏的获胜者



## 字符串常见问题

#### KMP问题

#### 5 最长回文子串 马拉车算法







## 数据结构上非常难的题目

#### 1439. 有序矩阵中的第 k 个最小数组和

#### 480. 滑动窗口中位数

#### 5729. 求出 MK 平均值

#### 218. 天际线问题

#### 817. 最少加油次数

#### 剑指 Offer 50. 第一个只出现一次的字符

#### 从序列化的数组中恢复出二叉树





#### 5. 最长回文子串

Manacher算法





## 待解决

+ 股票 121,122, 123,188,309,714

+ 打家劫舍 198, 213, 337

+ 数学题 343

+ 最大区间  452,435,646

+ 位运算 136 http://graphics.stanford.edu/~seander/bithacks.html#ReverseParallel

+ [321. 拼接最大数](https://leetcode-cn.com/problems/create-maximum-number/)

+ https://leetcode-cn.com/problems/remove-duplicate-letters/solution/yi-zhao-chi-bian-li-kou-si-dao-ti-ma-ma-zai-ye-b-4/

+ [103. 二叉树的锯齿形层序遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

+ 402的变种，给定一组字符串数字，计算最大拼接数，不改变原有的顺序

+ 312的子问题，两个数组合并，如何得到最大的数，或者第k大的

+ [135. 分发糖果](https://leetcode-cn.com/problems/candy/) 如何利用单调栈找到数组中最近的那个最大数

+ [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/) 快慢指针

+ next permutation

+ 公共父节点

+ 质因数分解

+ [524. 通过删除字母匹配到字典里最长单词](https://leetcode-cn.com/problems/longest-word-in-dictionary-through-deleting/)

+ KMP 算法

+ [330. 按要求补齐数组](https://leetcode-cn.com/problems/patching-array/) 贪心

+ 🚀 **复习一下流水线调度问题之类的思想** 🚀

+ [二分查找](https://www.zhihu.com/question/36132386/answer/530313852)

+ 鸡蛋掉落

+ 第k小数

+ 期望为线性的选择算法 [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

+ 多路快排

+ 二叉树的四种排序 94 102 144-145

+ [752. 打开转盘锁](https://leetcode-cn.com/problems/open-the-lock/)

+ 37 数独

+ 750 买卖股票的最佳时机含手续费

+ 650 素数分解问题，**搜集与素数相关的问题**

+ 单调栈 84，85，121

+ 分石子

+ 记忆化搜索 dfs

+ 树型DP问题

+ 最长摇摆序列问题

+ 主定理

+ 数学相关问题：https://www.cnblogs.com/liuzhen1995/p/13767374.html

+ 扫描线问题

+ 哨兵技巧

+ **502 IPO 问题** 思路较难

+ **659 分割数组为连续子序列** [solution](https://blog.csdn.net/birdmanqin/article/details/110631173?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.control&dist_request_id=827965de-f71e-49a7-9122-2e51ed5a7c06&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.control)

+ 博弈问题：比如谁先谁后，先下后下



#### 倍增LCA

```c++

const int N = 5e5 + 5;
int n, m, root, fa[N][50], depth[N];
std::vector<int> sons[N];

// root表示当前子树根结点, pre表示它的父亲结点
// 该函数递归完成depth和fa数组的更新 
// depth[u]维护的是结点u的深度, 整棵树的根结点初始为1
// fa[u][i]维护的是结点u向上走2 ^ i步后所到的结点, 如果走出去了fa[u][i]就是-1, 因为i >= 0, 所以u结点至少走一步, 所以对于走0步(即不动)需要特判一下
void dfs(int root, int pre) {
  depth[root] = depth[pre] + 1; 

  fa[root][0] = pre;
  // depth[root]- (1 << i) >= 1表示最多走到深度为1的树的根结点
  for (int i = 1; depth[root]- (1 << i) >= 1; ++i) {
    fa[root][i] = fa[fa[root][i - 1]][i - 1]; // root + 2^4 == [root + 2^ 3] + 2^3
  }
  // 到这里root结点的depth和fa数组就都已经更新好了, 接下去递归更新它的儿子结点
  for (int i = 0; i < sons[root].size(); ++i) {
    int v = sons[root][i];
    if (v != pre) 
      dfs(v, root);
  }
}
// 返回当前结点u向上走d步后的结点
int Up(int u, int d) {
  if (d == 0) return u;
  // 将d用二进制拆分
  for (int i = 0; i < 30; ++i) {
    if ((1 << i) & d) {
      u = fa[u][i];
    }
  }
  return u;
}
// 返回结点u和结点v的最近公共祖先
int LCA(int u, int v) {
  if (depth[u] > depth[v]) std::swap(u, v);
  v = Up(v, depth[v] - depth[u]);
  if (u == v) return u;
  // 现在u和v在同一深度上(即同一层), upper是算出最多可以跳的范围的上界(从depth[u] - 2 ^ k >= 1推导而来)
  // k是从upper从大到小枚举, 如果从小到大, 最开始跳的很小, 后面跳的很大, 但是到后面如果需要一步很小的跳时, 就直接跳出去了, 所以不能从小到大
  // upper直接等于30也是可以的, 因为这样相当于向上跳2 ^ 30次方, 肯定u和v都跳出去了, 但是它们还是都相等, 所以不影响(我们是不等才往上跳)
  int upper = std::log2(depth[u] - 1);
  upper = 30;
  for (int k = upper; k >= 0; --k) {
    // 加不加下面这句都是可行的, 因为就算跳出去, 那么它们都等于-1(fa[u][k] == fa[v][k]), 不影响
    // if (fa[u][k] == -1) continue;
    if (fa[u][k] != fa[v][k]) {
      u = fa[u][k];
      v = fa[v][k];
    }
  }
  return fa[u][0];
}

```

