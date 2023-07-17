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
```

```

## 6. 写一个带有TTL机制的map
```

```

## 7. 文件中检索单词数
```

```

## 8. 有序链表去重
```

```

## 9. 股票买卖最大价格
```

```

## 10. 拼手气金额划分算法
```

```

## 11. 二叉搜索树判断
```

```

## 12. 二叉树非迭代后续遍历
```

```

## 13. 数组中的零移动到数组后方
```

```











