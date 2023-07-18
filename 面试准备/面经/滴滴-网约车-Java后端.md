# 算法题
## 1. 两个线程交替打印a b 两个字符
``` java
public class AlternatePrinting {
    // 线程锁
    private static Object lock = new Object();
    // 判断当前是要打印a，还是打印b
    private static boolean isPrintA = true;


    // 线程执行类 printA 用于打印字母A
    static class printA implements Runnable {
        @override
        public void run(){
            for(int i = 0;i < 10;i++){
                // 尝试获取锁
                synchronized (lock) {
                    // 一直等待输出A的轮次
                    while(!isPrintA) {
                        // 不输出A则释放锁，等待被唤醒
                        try {
                            lock.wait();
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                    // 获取锁后，打印字符，并设置当前轮次该输出B
                    System.out.print("a");
                    isPrintA = false;
                    // 唤醒等待线程
                    lock.notifyAll();
                }
            }
        }
    }

    // 打印字符B的线程执行类
    static class PrintB implements Runnable {
        @Override
        public void run() {
            for (int i = 0; i < 10; i++) {
                synchronized (lock) {
                    while (printA) {
                        try {
                            lock.wait();
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                    System.out.print("b ");
                    printA = true;
                    lock.notifyAll();
                }
            }
        }
    }

    // 主方法
    public static void main(String[] args) {
        Thread threadA = new Thread(new PrintA());
        Thread threadB = new Thread(new PrintB());

        threadA.start();
        threadB.start();
    }


} 
```

## 2. IP地址转换为32位无符号整数
``` java
public class IPConverter {
    public static long ipToLong(String ipAddress) {
        String[] ipAddressInArray = ipAddress.split("\\.");

        long result = 0;
        for (int i = 0; i < ipAddressInArray.length; i++) {
            int power = 3 - i;
            int octet = Integer.parseInt(ipAddressInArray[i]);
            result += octet * Math.pow(256, power);
        }

        return result;
    }

    public static void main(String[] args) {
        String ipAddress = "192.168.0.1";
        long numericIp = ipToLong(ipAddress);
        System.out.println("Numeric IP: " + numericIp);
    }
}

```

## 3. 有序列链表的反转
``` java
// 链表类
public class ListNode {
    int val;
    ListNode next;
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}

// 反转链表
public ListNode reverseList(ListNode head) {
    ListNode pre = null;
    ListNode next = null;
    while(head != null) {
        next = head.next;
        head.next = pre;
        pre = head;
        head = next;
    }

    return pre;
}


```

## 4. 排序算法(快速排序)
``` java

// [7, 2, 1, 6, 8, 5, 3, 4]
// [ ]

public static void quickSort(int low, int higt, int[] arr){
    if(low < high) {
        int index = patition(low, high, arr);

        quickSort(low, index-1,arr);
        quickSort(index, high, arr);
    }
}

public static int partition(int low, int high, int[] arr){

    // 选择最右边的元素作为基准值
    int pivot = arr[high];
    
    // 设置分区索引为最左边元素的索引
    int i = low - 1;

    for (int j = low; j < high; j++) {
        // 如果当前元素小于或等于基准值，则将其与分区索引右侧的元素交换
        if (arr[j] <= pivot) {
            i++;
            swap(arr, i, j);
        }
    }

    // 将基准值放置到分区索引的右侧，完成一次分区操作
    swap(arr, i + 1, high);

    // 返回分区索引
    return i + 1;
}

public static void swap(int[] arr, int i, int j){
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}


```

## 5. 在矩阵字符串中找到字符串的存在路径
``` java

public boolean exist(char[][] board, String word) {
    if (board == null || board.length == 0 || board[0].length == 0) {
        return false;
    }

    int rows = board.length;
    int cols = board[0].length;

    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (backtrack(board, word, i, j, 0)) {
                return true;
            }
        }
    }

    return false;
}


private static boolean backtrack(char[][] board, int i, int j, int index){

    // 判断是否找到了目标字符串
    if(index == word.length) {
        return true;
    }

    // 判断边界
    if(i < 0 || i >= board.length || j < 0 || j >= board[0].length || board[i][j] != word.cahrAt(index)) {
        return false;
    }

    // 新找到一个字符，避免搜索回头，将当前这个字符临时设置为# 
    char temp = board[i][j];
    board[i][j] = '#';

    boolean found = backtrack(board, word, row - 1, col, index + 1)
                || backtrack(board, word, row + 1, col, index + 1)
                || backtrack(board, word, row, col - 1, index + 1)
                || backtrack(board, word, row, col + 1, index + 1);

    // 恢复状态
    board[i][j] = temp;

    return found;

}

```

## 6. 写一个带有TTL机制的map
``` java

import java.util.Map;
import java.util.concurrent.*;

public class TTLMap<K, V> {
    private final Map<K, V> map;
    private final ScheduledExecutorService scheduler;

    public TTLMap() {
        map = new ConcurrentHashMap<>();
        scheduler = Executors.newSingleThreadScheduledExecutor();
    }

    public void put(K key, V value, long ttl) {
        map.put(key, value);
        scheduler.schedule(() -> map.remove(key), ttl, TimeUnit.MILLISECONDS);
    }

    public V get(K key) {
        return map.get(key);
    }

    public void remove(K key) {
        map.remove(key);
    }

    public int size() {
        return map.size();
    }
}

```

## 7. 文件中检索单词数
```

```

## 8. 有序链表去重
``` java
// 删除所有重复数字，仅保留一个
public ListNode deleteDuplicates(ListNode head) {

    if(head == null) {
        return null;
    }

    if(head.next == null) {
        return head;
    }

    // ListNode preNode = head;
    ListNode cur = head;

    while(cur.next != null) {
        if(cur.val == cur.next.val){
            cur.next = cur.next.next;
        }
        else {
            cur = cur.next;
        }
    }

    return head;
}

// 删除所有重复的数据，仅保留不重复的
public ListNode deleteDuplicates(ListNode head) {
    if (head == null) {
        return head;
    }
    
    ListNode dummy = new ListNode(0, head);

    ListNode cur = dummy;
    while (cur.next != null && cur.next.next != null) {
        if (cur.next.val == cur.next.next.val) {
            int x = cur.next.val;
            while (cur.next != null && cur.next.val == x) {
                cur.next = cur.next.next;
            }
        } else {
            cur = cur.next;
        }
    }

    return dummy.next;
}

```

## 9. 股票买卖最大价格
``` java

public int maxPrice(int[] prices){

    int[][] dp = new int[prices.length + 1][2];

    // 初始化 第一天持有股票的收益为-price
    dp[0][1] = -price[0];

    // dp[i][0] 表示第i天持有股票时的最大收益
    // dp[i][1] 表示第i天没有持有股票的最大收益

    for(int i = 1;i < prices.length;i++){
        // 第i天持有股票的收益分两种情况，持续持有或买进
        dp[i][0] = Math.max(dp[i-1][0], -price[i]);
        // 第i天未持有股票的收益情况分为持续不持有或卖出
        dp[i][1] = Math.max(dp[i-1][1], dp[i-1][0] + price[i]);
    }

    // 返回最后一天不持有的收益
    return dp[price.length - 1][1];

}


```

## 10. 拼手气金额划分算法
```

```

## 11. 二叉搜索树判断
``` java
public boolean isValidBST(TreeNode root) {
    
    // 记录二叉树中序遍历结果
    List<Integer> ans = new ArrayList<>();

    // 辅助栈
    Stack<TreeNode> stack = new Stack<>();

    TreeNode cur = root;

    while(cur != null || !stack.isEmpty()) {
        // 左子节点不为空，则持续入栈，并将指针移动到左子节点
        if(cur != null) {
            stack.push(cur);
            cur = cur.left;
        }
        // 左节点为空，则回溯至中间节点
        else {
            TreeNode top = stack.pop();
            // 左子节点大于中间节点，不是搜索树
            if(ans.size() > 0 && top.val <= ans.get(ans.size()-1)){
                return false;
            }
            // 记录中序遍历序列
            ans.add(top.val);
            // 节点移动至右节点
            cur = top.right;
        }
    }

    return true;
}
```

## 12. 二叉树非迭代后续遍历
``` java

// 前序遍历
public List<Integer> preOrderTraversal(TreeNode root) {

    List<Integer> ans = new ArrayList<>();

    Stack<Integer> myStack = new Stack<>();
    myStack.push(root);

    while(!myStack.isEmpty()) {
        TreeNode top = myStack.pop();

        ans.add(top.val);

        if(top.right != null) {
            myStack.push(top.right);
        }

        if(top.left != null) {
            myStack.push(top.left);
        }
    }

    return ans;
}


// 中序遍历
public List<Integer> inorderTraversal(TreeNode root){

    List<Integer> ans = new ArrayList<>();

    Stack<Integer> myStack = new Stack<>();

    TreeNode cur = root;

    while(cur != null || !myStack.isEmpty()) {
        if(cur != null) {
            myStack.push(cur);
            cur = cur.left;
        }
        else {
            TreeNode top = myStack.pop();
            ans.add(top.val);
            cur = top.right;
        }
    }

    return ans;
}


// 后续遍历
public List<Integer> postorderTraversal(TreeNode root) {

    List<Integer> ans = new ArrayList<>();

    Stack<Integer> myStack = new Stack<>();
    myStack.push(root);

    // 前序遍历的出栈顺序是中左右，所以入栈顺序是右左中
    // 后续遍历的出栈顺序是左右中，所以需要保证出栈顺序是中右左，再取反
    while(!myStack.isEmpty()) {
        
        TreeNode top = myStack.pop();
        ans.add(top.val);

        if(top.left != null) {
            myStack.push(top.left);
        }

        if(top.right != null) {
            myStack.push(top.right);
        }

    }

    return ans;
}

```

## 13. 数组中的零移动到数组后方
```

```











