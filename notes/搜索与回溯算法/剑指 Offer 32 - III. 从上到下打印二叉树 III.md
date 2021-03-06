# 剑指 Offer 32 - III. 从上到下打印二叉树 III

```text
请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

例如:
给定二叉树: [3,9,20,null,null,15,7],
    3
   / \
  9  20
    /  \
   15   7
   
返回其层次遍历结果：
[
  [3],
  [20,9],
  [15,7]
]
```

```java
class Solution {
    //方法一：方法一：层序遍历 + 双端队列
    //双端队列，双进双出
    //利用双端队列的两端皆可添加元素的特性，设打印列表（双端队列） tmp ，并规定：
    //奇数层 则添加至 tmp 尾部 ，
    //偶数层 则添加至 tmp 头部 。
    public List<List<Integer>> levelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        //特例处理： 当树的根节点为空，则直接返回空列表 [] ；
        if (root != null) {
            queue.add(root);
        }
        //初始化： 打印结果空列表 res ，包含根节点的双端队列 deque ；
        //BFS 循环： 当 deque 为空时跳出；
        while (!queue.isEmpty()) {
            //新建列表 tmp ，用于临时存储当前层打印结果；
            LinkedList<Integer> tmp = new LinkedList<>();
            //当前层打印循环： 循环次数为当前层节点数（即 deque 长度）；
            for (int i = queue.size(); i > 0; i--) {
                //出队： 队首元素出队，记为 node；
                TreeNode node = queue.poll();
                //打印： 若为奇数层，将 node.val 添加至 tmp 尾部；否则，添加至 tmp 头部；
                if (res.size() % 2 == 0) tmp.addLast(node.val);
                else tmp.addFirst(node.val);
                //添加子节点： 若 node 的左（右）子节点不为空，则加入 deque ；
                if (node.left != null) queue.add(node.left);
                if (node.right != null) queue.add(node.right);
            }
            //将当前层结果 tmp 转化为 list 并添加入 res ；
            res.add(tmp);
        }
        //返回值： 返回打印结果列表 res 即可；
        return res;
    }
}
```

```java
public class LevelOrderLoop {
    public class TreeNode {
        int val;
        TreeNode left;
        TreeNode right;

        TreeNode(int x) {
            val = x;
        }
    }

    //方法二：层序遍历 + 倒叙
    //此方法的优点是只用列表即可，无需其他数据结构。
    //偶数层倒序： 若 res 的长度为 奇数 ，说明当前是偶数层，则对 tmp 执行 倒序 操作。
    public List<List<Integer>> levelOrderloop2(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        if (root != null) queue.add(root);
        while (!queue.isEmpty()) {
            List<Integer> tmp = new ArrayList<>();
            for (int i = queue.size(); i > 0; i--) {
                TreeNode node = queue.poll();
                tmp.add(node.val);
                if (node.left != null) queue.add(node.left);
                if (node.right != null) queue.add(node.right);
            }
            //java.util.Collections.reverse()方法是java.util.Collections类的方法。它颠倒了作为参数传递的列表中元素的顺序。
            if (res.size() % 2 == 1) Collections.reverse(tmp);
            res.add(tmp);
        }
        return res;
    }
}
```

```java
class Solution {
    //方法三：层序遍历 + 双端队列（奇偶层逻辑分离）Deque 双端队列
    //方法一代码简短、容易实现；但需要判断每个节点的所在层奇偶性，即冗余了 NN 次判断。
    //通过将奇偶层逻辑拆分，可以消除冗余的判断。
    public List<List<Integer>> levelOrderloop3(TreeNode root) {
        Deque<TreeNode> deque = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        if (root != null) deque.add(root);
        while (!deque.isEmpty()) {
            // 打印奇数层
            List<Integer> tmp = new ArrayList<>();
            for (int i = deque.size(); i > 0; i--) {
                // 从左向右打印
                TreeNode node = deque.removeFirst();
                tmp.add(node.val);
                // 先左后右加入下层节点
                if (node.left != null) deque.addLast(node.left);
                if (node.right != null) deque.addLast(node.right);
            }
            res.add(tmp);
            if (deque.isEmpty()) break; // 若为空则提前跳出
            // 打印偶数层
            tmp = new ArrayList<>();
            for (int i = deque.size(); i > 0; i--) {
                // 从右向左打印
                TreeNode node = deque.removeLast();
                tmp.add(node.val);
                // 先右后左加入下层节点
                if (node.right != null) deque.addFirst(node.right);
                if (node.left != null) deque.addFirst(node.left);
            }
            res.add(tmp);
        }
        return res;
    }
}
```