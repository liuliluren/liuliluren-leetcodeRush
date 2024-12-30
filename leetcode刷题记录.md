# 力扣刷题记录

模板

````
## ？	2024-？-？

### （1）？——？

**个人题解**：

```

```

> 核心思路

**官方题解**：

```

```

> 核心思路
````

标识

⭐：重要



## 0	foever

### 0.1	java中常见的类

#### **（1）StringBuilder** 

> 往往用来组合一个一个的字符【包括char、int 、float等各字符】后构建成字符串，

```java
//初始化
        StringBuilder sb = new StringBuilder();

//增加数据
            sb.append(1);
            sb.append('a');
//翻转内部数据
			sb.reverse();
```

#### **（2）Set** 

> 集合，不会出现重复的内容。

```java
//初始化
        Set<Character> occ = new HashSet<Character>();
//增加
		occ.add();
//删除
		occ.remove();
//查找是否包含元素
		occ.contains();
```

#### **（3）Map** 

> 映射关系。

```java
//初始化
        Map<Integer, Integer> hashtable = new HashMap<Integer, Integer>();
//增加，前面是key，后面是value
            hashtable.put(202361, 1231);
//删除
            hashtable.remove(202361);//返回的值是1231。
//取值
            hashtable.get(202361);//返回的值是1231。
			//map没有按照索引值进行取值的方法，都是根据key进行取值
//取值优化版
			hashtable.getOrDefault(key, defaultValue);
	//这种写法的优势在于当目标值不存在的时候，不会返回空值，而是返回一个指定的值，比如：
            List<String> list = map.getOrDefault(key, new ArrayList<String>());

//查找是否包含key
			hashtable.containsKey(202361);//返回的是boolean结果，这里返回true

//返回所有key的集合
            List<String> list = map.getOrDefault(key, new ArrayList<String>());
			map.keySet()
//返回所有value的集合
			map.values()
	        System.out.println(map.values());
```

1. 比如对于以下的map数据：

   ![image-20240429160816695](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240429160816695.png)

   - 使用System.out.println(map.keySet());打印的数据为

     ![image-20240429161017712](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240429161017712.png)

   - 使用System.out.println(map.values());打印的数据为

     ![image-20240429160928424](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240429160928424.png)




#### **（3.1）TreeMap** 

> 数图

```java
//相对于普通的HashMap来说，存放于TreeMap中的数据是有大小顺序的。
//会按照key值的大小进行排序。
        Map<Integer, int[]> map = new TreeMap<>();
        map.put(1, new int[]{1, 1});
        map.put(15, new int[]{1, 1});
        map.put(2, new int[]{1, 1});
//对于上述的存放的数据，会是(1，{1, 1})、(2，{1, 1})、(15，{1, 1})的这样一个顺序。

//对Map数据的遍历：
        Iterator<Map.Entry<Integer, int[]>> iterator = map.entrySet().iterator();

```

> 一个关于遍历的时候实现比较当前的数与下一个数的代码参考：
>
> ```java
>         Iterator<Map.Entry<Integer, int[]>> iterator = 
>                 map.entrySet().iterator();
>         Map.Entry<Integer, int[]> entryCurrent = null;
> 
>         while (iterator.hasNext()) {
>             //下一个位置的指向器
>             Map.Entry<Integer, int[]> entryNext = iterator.next();
>             //具体比较的实现
>             if (entryCurrent != null &&
>                     entryCurrent.getValue()[1] >= entryNext.getValue()[0]) {
>                     entryCurrent.setValue(new int[] {
>                             entryCurrent.getValue()[0],
>                             Math.max(entryCurrent.getValue()[1],
>                                     entryNext.getValue()[1])
>                     });
>                 //移除下一个位置的指向
>                     iterator.remove();
>                     continue;
>             }
>             //将当前指向的指向器指定为下一个
>             entryCurrent = entryNext;
>         }
> ```

#### （3.2）LinkedHashMap

> LinkedHashMap 是 HashMap 的一个子类，它在 HashMap 的基础上维护了元素的插入顺序（或者访问顺序，取决于构造函数的参数）。 它通过一个双向链表来维护这个顺序。

1. **构造函数:**
   - LinkedHashMap(): 创建一个空的 LinkedHashMap，元素按照插入顺序排列。
   - LinkedHashMap(int initialCapacity): 创建一个指定初始容量的 LinkedHashMap。
   - LinkedHashMap(int initialCapacity, float loadFactor): 创建指定容量和负载因子的 LinkedHashMap。
   - LinkedHashMap(int initialCapacity, float loadFactor, boolean accessOrder): **accessOrder** 参数控制顺序是基于插入还是访问。如果 accessOrder 为 true，则每次访问一个元素，该元素都会被移动到链表的末尾，从而保持最近访问的元素在最后。
   - LinkedHashMap(Map<? extends K, ? extends V> m): 使用另一个 Map 初始化 LinkedHashMap。
2. **put(K key, V value):** 将键值对插入到 Map 中。在 LRU 缓存中，如果 accessOrder 为 true，则访问现有元素也会调用 put，从而将其移动到链表末尾。
3. **get(Object key):** 获取与指定键关联的值。在 LRU 缓存中，如果 accessOrder 为 true，则调用 get 也会更新元素的访问顺序。
4. **getOrDefault(Object key, V defaultValue):** 获取与指定键关联的值，如果键不存在则返回默认值。
5. **remove(Object key):** 删除指定键的映射。
6. **containsKey(Object key):** 检查 Map 是否包含指定键。
7. **containsValue(Object value):** 检查 Map 是否包含指定值。
8. **size():** 返回 Map 中的键值对数量。
9. **clear():** 清空 Map。
10. **keySet():** 返回包含所有键的 Set 视图。
11. **values():** 返回包含所有值的 Collection 视图。
12. **entrySet():** 返回包含所有键值对的 Set 视图。

```java
// 一种实现LRU的基于LinkedHashMap子类实现
class LRUCache extends LinkedHashMap<Integer, Integer>{
    private int capacity;
    
    public LRUCache(int capacity) {
        super(capacity, 0.75F, true);
        this.capacity = capacity;
    }

    public int get(int key) {
        return super.getOrDefault(key, -1);
    }

    public void put(int key, int value) {
        super.put(key, value);
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
        return size() > capacity; 
    }
}
```

> 其中在LRUCache初始化时，出现了一个0.75F参数。
>
> 这是一个负载因子，其中F代表Float类型。当 HashMap 中的元素数量达到`容量 * 负载因子`时，HashMap 会进行 **重新哈希 (rehashing)**，这会创建一个更大的数组并将现有元素重新映射到新的数组中。
>
> 0.75即代表当数量达到75%的空间时，会进行重新哈希。
>
> 0.75F是比较常用的负载因子值。

#### **（4）List** 

> 链表

```java
//初始化
        ArrayList<Integer> list = new ArrayList<>();
//增加
		list.add(1);
//删除，根据序号进行删除
		list.remove();//如list.remove(0)，代表删除的是第一个数据。
//如果删除的时候想指定数据
        list.remove(Integer.valueOf(3));
//给List进行排序
        Collections.sort(list);


```

#### **（5）queue**

#### （5.1）PriorityQueue 

> 优先队列

```java
//初始化
        PriorityQueue<Integer> priorityQueue = new PriorityQueue<>((a, b) -> a - b);
		//这里初始化了一个升序排列的队列，实际上就是一个小根堆的数据结构。
		//如果需要实现一个大根堆的方式插入数据，则使用下面的初始化方式
        PriorityQueue<Integer> priorityQueue = new PriorityQueue<>((a, b) -> b - a);
//插入元素
        priorityQueue.offer(3);
        priorityQueue.offer(1);
        priorityQueue.offer(2);
//输出元素
		priorityQueue.poll()
		//依然是输出队列的队首元素，但是形象的体现在大根堆或者是小根堆上时，输出的就是根结点的元素。

```

对于小根堆的插入的理解。举例插入5 4 3 2 1 的效果

- 全部代码

  ```java
          PriorityQueue<Integer> priorityQueue = new PriorityQueue<>((a, b) -> a - b);
  
          priorityQueue.offer(5);
          priorityQueue.offer(4);
          priorityQueue.offer(3);
          priorityQueue.offer(2);
          priorityQueue.offer(1);
  ```

- 回忆小根堆的实现原理，画出如下的变化图：

  ![image-20240501165630486](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240501165630486.png)

- 检查每次插入的时候，priorityQueue内数据的变化是否符合上述规律。

  1. 第一次插入

     <img src="https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240501165535953.png" alt="image-20240501165535953" style="zoom:67%;" />

  2. 第五次插入

     <img src="https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240501165712554.png" alt="image-20240501165712554" style="zoom:67%;" />

     实测确实是按照小根堆的方式进行插入数据的。

#### （5.2）Deque

> 双端队列（Double-Ended Queue），比较常用来模拟栈的实现

Deque常见的两种实现类包括ArrayDeque、LinkedList

```java
        Deque<> stack = new ArrayDeque<>();
        Deque<> stack = new LinkedList<>(); // 最常见的用来模拟栈功能的实现
```

常用方法

```java
addFirst(), addLast(), removeFirst(), removeLast()

// 模拟栈功能：
stack.addFirst()
stack.removeFirst() // 相当于Pop
stack.peekFirst()  //Peek (查看栈顶元素而不移除)
stack.isEmpty() // 检查是否为空
```



#### （6）stack

#### （7）Comparator

> 待补充

#### （8）数组操作

```java
//数组的复制
System.arraycopy(Object src, int srcPos, Object dest, int destPos, int length)

//src：源数组，即要复制元素的数组。
//srcPos：源数组中复制元素的起始位置。
//dest：目标数组，即要复制到的数组。
//destPos：目标数组中复制元素的起始位置。
//length：要复制的元素数量。

    
//数组转化成List
List<Integer> numList = Arrays.stream(nums)//nums为int类型数组
        .boxed()//将基本类型int转化为Integer
        .sorted()//排序
        .collect(Collectors.toList());//收集为List

```



## 1	2024-04-07

### **（1）两数之和**——1

> 暴力枚举确实比较容易想到，但是对于哈希表的实现并不是很清楚

**哈希表实现**：

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> hashtable = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; ++i) {
            if (hashtable.containsKey(target - nums[i])) {
                return new int[]{hashtable.get(target - nums[i]), i};
            }
            hashtable.put(nums[i], i);
        }
        return new int[0];
    }
}
```

**哈希表的一些总结**：

- 哈希表查看是否存在键

  - hashtable.containsKey()

- 哈希表取键值

  - hashtable.get()

- 哈希表插入数据

  - hashtable.put()

  ```java
          Map<String, Integer> employeeSalaries = new HashMap<>();
          employeeSalaries.put("John", 50000);
          employeeSalaries.put("Alice", 60000);
          employeeSalaries.put("Bob", 55000);
  
  		employeeSalaries.containsKey("John")
          employeeSalaries.get("Alice")
  ```

### **（2）两数相加**

> 使用到了ListNode，此类需要自己创建，并非java原生类

**个人实现**：

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode resultNode = new ListNode();
        ListNode finalResultNode = resultNode;

        boolean overflow = false;
//        当l1和l2都不为空
        while (true) {
            resultNode.val = l1.val + l2.val;
//            是否进位？
            if (overflow){
                resultNode.val+=1;
                overflow = false;
            }
//            若当前节点值超出了9则需要-10,同时将进位标设置为true
            if(resultNode.val > 9){
                resultNode.val -= 10;
                overflow = true;
            }
//            当l1或者l2的next为空则终止当前执行
            if(l1.next == null || l2.next == null){
                break;
            }
//            都不为空,则为 resultNode创建下一个空节点继续判断
            resultNode.next = new ListNode();
            resultNode = resultNode.next;
            l1 = l1.next;
            l2 = l2.next;
        }
//        当l1还剩下多余的节点
        if (l1.next != null){
//            如果有进位,l1的数据需要进一步判断
            while(overflow){
                resultNode.next = new ListNode();
                resultNode = resultNode.next;
                if(l1.next == null){
                    resultNode.val = 1;
                    break;
                }
                l1 = l1.next;
                resultNode.val = l1.val + 1;
                overflow = false;
                if (resultNode.val > 9){
                    resultNode.val -= 10;
                    overflow = true;
                }
            }
//            没有进位了,并且l1还不为空
            if (l1.next != null){
                resultNode.next = l1.next;
            }
            overflow = false;
        }
//        与l1同理
        if (l2.next != null){
            while(overflow){
                resultNode.next = new ListNode();
                resultNode = resultNode.next;
                if(l2.next == null){
                    resultNode.val = 1;
                    break;
                }
                l2 = l2.next;
                resultNode.val = l2.val + 1;
                overflow = false;
                if (resultNode.val > 9){
                    resultNode.val -= 10;
                    overflow = true;
                }
            }
            if (l2.next != null){
                resultNode.next = l2.next;
            }
            overflow = false;
        }
//        如果到此仍然有进位内容,这种情况往往l1和l2的next都是空
        if (overflow){
            resultNode.next = new ListNode();
            resultNode = resultNode.next;
            resultNode.val = 1;
        }

//        while(finalResultNode != null){
//            System.out.println(finalResultNode.val); // 打印当前节点的值
//            finalResultNode = finalResultNode.next;
//        }

        return finalResultNode;
    }
}
```

**官方实现**：

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode head = null, tail = null;
        int carry = 0;
        while (l1 != null || l2 != null) {
            int n1 = l1 != null ? l1.val : 0;
            int n2 = l2 != null ? l2.val : 0;
            int sum = n1 + n2 + carry;
            if (head == null) {
                head = tail = new ListNode(sum % 10);
            } else {
                tail.next = new ListNode(sum % 10);
                tail = tail.next;
            }
            carry = sum / 10;
            if (l1 != null) {
                l1 = l1.next;
            }
            if (l2 != null) {
                l2 = l2.next;
            }
        }
        if (carry > 0) {
            tail.next = new ListNode(carry);
        }
        return head;
    }
}
```

> 核心在于：
>
> - **”如果两个链表的长度不同，则可以认为长度短的链表的后面有若干个 0 。“**
>
> 对%和/的效果也没有那么熟悉：
>
> ```
> 13/10=1
> 23/10=2
> 13%10=3
> 23%10=3
> ```

## 2	2024-04-08

### **（1）无重复字符的最长字串**

**个人实现**：

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s.length() == 0){
            return 0;
        }
        int maxLength = 1;
        Queue<Character> queue = new LinkedList<>();
        for (int i=0;i<s.length();i++){
            char presentChar = s.charAt(i);
//            检查队列中是否包含了当前值
            int result = checkExistence(queue, presentChar);
            queue.add(presentChar);
            while (result != -1){
                queue.remove();
                result--;
            }
            if (queue.size() > maxLength){
                maxLength = queue.size();
            }
        }
        return maxLength;
    }
//    检查当前Character在队列中是否存在
    int checkExistence(Queue<Character> queueToCheck, Character charToCheck){
        int index = 0;
        for (Character element : queueToCheck){
            if (element.equals(charToCheck)){
                return index;
            }
            index++;
        }
        return -1;
    }
}
```

**官方题解**：

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        // 哈希集合，记录每个字符是否出现过
        Set<Character> occ = new HashSet<Character>();
        int n = s.length();
        // 右指针，初始值为 -1，相当于我们在字符串的左边界的左侧，还没有开始移动
        int rk = -1, ans = 0;
      	// i相当于左指针，当occ里始终包含目标字符的时候会不断删除最左侧的char字符
        for (int i = 0; i < n; ++i) {
            if (i != 0) {
                // 左指针向右移动一格，移除一个字符
                occ.remove(s.charAt(i - 1));
            }
            while (rk + 1 < n && !occ.contains(s.charAt(rk + 1))) {
                // 不断地移动右指针
                occ.add(s.charAt(rk + 1));
                ++rk;
            }
            // 第 i 到 rk 个字符是一个极长的无重复字符子串
            ans = Math.max(ans, rk - i + 1);
        }
        return ans;
    }
}
```

> 思路基本相同
>
> 优点在于：
>
> - 使用了集合Set，判断是否存在不需要使用for进行遍历
> - 比大小使用Math.max()
>
> **总结**：
>
> - 以后判断是否存在的时候，可以考虑**集合**以及**哈希表**；

## 3	2024-04-10

### **（1）寻找两个正序数组的中位数**——4

**个人思路**：

对于满足条件任意的数组a与b；

1. 先找a的中位数，记忆其指针为am，

2. 找到a中am所指向的数在能在b中满足条件放置的位置，记忆为bm；

3. 比较left和right的值的大小；

   > left	=	数组a中am指向的数的左边的数的数量	+	数组b中bm指向的数的左边的数的数量
   >
   > right	=	数组a中am指向的数的右边的数的数量	+	数组b中bm指向的数的右边的数的数量

4. 如果left=right

   退出循环，输出left和right的中值

5. 如果left>right

   - 如果是，则将现有的am设置为ar，将al与am的中间置为新的am；

6. 如果left<right

   - 如果是，则将现有的am设置为al，将ar与am的中间置为新的am；

**个人题解**：

```java
/**
 * @author ZZR
 * @version 1.0
 */

import java.util.*;


public class Main {
    public static void main(String[] args) {
//        Test test = new Test();
//        test.testFunction();

//        int[] num1 = {2, 3, 4, 5};
//        int[] num2 = {2};

//        int[] num1 = {2, 3, 6, 7};
//        int[] num2 = {4};

//        测试数据
//        int[] num1 = {1, 1, 3, 5, 6};
//        int[] num2 = {0, 1, 2, 7 ,8};

//        int[] num1 = {3};
//        int[] num2 = {-2, -1};

//        int[] num1 = {1, 2};
//        int[] num2 = {-1, 3};

//        int[] num1 = {1, 2};
//        int[] num2 = {3, 4};

//        int[] num1 = {1};
//        int[] num2 = {2, 3, 4};

//        int[] num1 = {1, 3};
//        int[] num2 = {2, 4, 5};

//        int[] num1 = {6, 7};
//        int[] num2 = {1, 2, 3, 4, 5, 8};

        int[] num1 = {1, 2, 3};
        int[] num2 = {4, 5, 6, 7, 8};

        Solution solution = new Solution();
        solution.findMedianSortedArrays(num1, num2);

    }
}

class Solution {
    public double findMedianSortedArrays(int[] a, int[] b) {
        int al,am = 0,ar;
        int aLength = a.length;
        int bLength = b.length;
        int left;
        int right;

        double result = 0;

//        当a或者b为空的时候的判定
        if(a.length == 0){
            if(b.length % 2 != 0){
                result = (double) b[b.length/2];
            }else {
                result = (double) (b[b.length / 2 - 1] + b[b.length / 2]) / 2;
            }
            return result;
        }else if (b.length == 0){
            if(a.length % 2 != 0){
                result = (double) a[a.length/2];
            }else {
                result = (double) (a[a.length / 2 - 1] + a[a.length / 2]) / 2;
            }
            return result;
        }
        ResultClass resultClass = getResult(a, b);
        if (resultClass.compareResult > 1 || resultClass.compareResult < -1){
            System.out.println("第一次结果不行，需要置换后查找");
            int[] temp = a;
            a = b;
            b = temp;
            System.out.println("交换a、b");
            resultClass = getResult(a, b);
        }
        Double compareResult = resultClass.compareResult;
        am = resultClass.am;
        int insertPosition = resultClass.insertPosition;

        if (compareResult == 0){
            result = (double) a[am];
        }else if (compareResult == 1){
            if (am - 1 < 0){
                result = (double) (a[am] + b[insertPosition-1] ) / 2;
            } else if (insertPosition - 1 < 0) {
                result = (double) (a[am] + a[am-1] ) / 2;
            }else {
                result = (double) (a[am] + (Math.max(b[insertPosition-1], a[am-1])) ) / 2;
            }
//            if (am - 1 > -1 && ){
//                result = (double) (a[am] + (Math.max(b[insertPosition-1], a[am-1])) ) / 2;
//            } else if (insertPosition - 1 < -1) {
//
//            } else {
//                result = (double) (a[am] + b[insertPosition-1] ) / 2;
//            }
        }else if (compareResult == -1) {
            if (am + 1 >= a.length){
                result = (double) (a[am] + b[insertPosition]) / 2;
            } else if (insertPosition >= b.length) {
                result = (double) (a[am] + a[am + 1]) / 2;
            } else {
                result = (double) (a[am] + (Math.min(b[insertPosition], a[am + 1]))) / 2;
            }
//            if (am + 1 < a.length) {
//                result = (double) (a[am] + (Math.min(b[insertPosition], a[am + 1]))) / 2;
//            } else {
//                result = (double) (a[am] + b[insertPosition]) / 2;
//            }
        }
        System.out.println("当前compareResult值为：");
        System.out.println(compareResult);
        System.out.println("当前am值为：");
        System.out.println(am);
        System.out.println("当前insertPosition值为：");
        System.out.println(insertPosition);
        System.out.println("输出的结果是");
        System.out.println(result);
        return result;
    }
    ResultClass getResult(int[] a, int[] b){
        int al = 0;
        int ar = a.length - 1;
        int am = al + (ar - al) / 2;

        double compareResult = 999;
        int insertPosition = 999;

        while(al <= ar){
            am = al + (ar - al) / 2;

            insertPosition = findInsertPosition(a[am], b);
            int left = am + insertPosition;
            int right = a.length - 1 - am + b.length - insertPosition;
            compareResult = left - right;
            if (compareResult >= -1 && compareResult <= 1){
                System.out.println("正常跳出循环，当前left：");
                System.out.println(left);
                System.out.println("正常跳出循环，当前right：");
                System.out.println(right);
                System.out.println("正常跳出循环，当前compareResult：");
                System.out.println(compareResult);
                break;
            }else if (compareResult < -1){
//                右移
                System.out.println("需要右移");
                al = am + 1; // 在右半部分继续查找
            }else {
                System.out.println("需要左移");
                ar = am - 1; // 在左半部分继续查找
            }
        }
        double[] result = new double[3];
        ResultClass resultClass = new ResultClass();

        resultClass.compareResult = compareResult;
        resultClass.am = am;
        resultClass.insertPosition = insertPosition;

        return resultClass;
    }
    public static int findInsertPosition(int a, int[] b) {
        int left = 0;
        int right = b.length - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (b[mid] == a) {
                return mid; // 如果找到a，则直接返回位置
            } else if (b[mid] < a) {
                left = mid + 1; // 如果b[mid] < a，则在右半部分继续查找
            } else {
                right = mid - 1; // 如果b[mid] > a，则在左半部分继续查找
            }
        }

        // 如果没找到a，返回合适的插入位置
        return left;
    }
    class ResultClass {
        int am;
        int insertPosition;
        double compareResult;

        public ResultClass(int am, int insertPosition, double compareResult) {
            this.am = am;
            this.insertPosition = insertPosition;
            this.compareResult = compareResult;
        }

        public ResultClass() {
        }
    }
}

class Test{
    public void testFunction(){

    }
}
```

> 个人的思路在于如何将a中的数置于b中，使得当a的该数置于此位置时，（left - right）的绝对值<=1；
>
> 此时，a的该数（或者与旁边的数同为）即为中位数的决定数；
>
> 有一个关键就在于，如果将a中的数置于b中无法满足绝对值<=1，这代表应该是将b中的数置于a中来逐个判断；

**官方题解**：

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int length1 = nums1.length, length2 = nums2.length;
        int totalLength = length1 + length2;
        if (totalLength % 2 == 1) {
            int midIndex = totalLength / 2;
            double median = getKthElement(nums1, nums2, midIndex + 1);
            return median;
        } else {
            int midIndex1 = totalLength / 2 - 1, midIndex2 = totalLength / 2;
            double median = (getKthElement(nums1, nums2, midIndex1 + 1) + getKthElement(nums1, nums2, midIndex2 + 1)) / 2.0;
            return median;
        }
    }

    public int getKthElement(int[] nums1, int[] nums2, int k) {
        /* 主要思路：要找到第 k (k>1) 小的元素，那么就取 pivot1 = nums1[k/2-1] 和 pivot2 = nums2[k/2-1] 进行比较
         * 这里的 "/" 表示整除
         * nums1 中小于等于 pivot1 的元素有 nums1[0 .. k/2-2] 共计 k/2-1 个
         * nums2 中小于等于 pivot2 的元素有 nums2[0 .. k/2-2] 共计 k/2-1 个
         * 取 pivot = min(pivot1, pivot2)，两个数组中小于等于 pivot 的元素共计不会超过 (k/2-1) + (k/2-1) <= k-2 个
         * 这样 pivot 本身最大也只能是第 k-1 小的元素
         * 如果 pivot = pivot1，那么 nums1[0 .. k/2-1] 都不可能是第 k 小的元素。把这些元素全部 "删除"，剩下的作为新的 nums1 数组
         * 如果 pivot = pivot2，那么 nums2[0 .. k/2-1] 都不可能是第 k 小的元素。把这些元素全部 "删除"，剩下的作为新的 nums2 数组
         * 由于我们 "删除" 了一些元素（这些元素都比第 k 小的元素要小），因此需要修改 k 的值，减去删除的数的个数
         */

        int length1 = nums1.length, length2 = nums2.length;
        int index1 = 0, index2 = 0;
        int kthElement = 0;

        while (true) {
            // 边界情况
            if (index1 == length1) {
                return nums2[index2 + k - 1];
            }
            if (index2 == length2) {
                return nums1[index1 + k - 1];
            }
            if (k == 1) {
                return Math.min(nums1[index1], nums2[index2]);
            }
            
            // 正常情况
            int half = k / 2;
            int newIndex1 = Math.min(index1 + half, length1) - 1;
            int newIndex2 = Math.min(index2 + half, length2) - 1;
            int pivot1 = nums1[newIndex1], pivot2 = nums2[newIndex2];
            if (pivot1 <= pivot2) {
                k -= (newIndex1 - index1 + 1);
                index1 = newIndex1 + 1;
            } else {
                k -= (newIndex2 - index2 + 1);
                index2 = newIndex2 + 1;
            }
        }
    }
}
```

> 重要思路：
>
> - ```
>   根据中位数的定义，当 m+n 是奇数时，中位数是两个有序数组中的第 (m+n)/2个元素，当 m+n是偶数时，中位数是两个有序数组中的第 (m+n)/2个元素和第 (m+n)/2+1个元素的平均值。因此，这道题可以转化成寻找两个有序数组中的第 k小的数，其中 k为 (m+n)/2或 (m+n)/2+1。
>   ```
>
> - ```
>   基于递归的思想，将所有的问题递归成每次找第k小的数。
>   ```
>
> - ![image-20240412151211258](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240412151211258.png)
>
> - ```
>   而对于每次找第k小的数时，先找到两数组的第[k/2-1]的数，将这两个数进行比大小，小的一方的左侧的数全部都可以去掉，也就是去掉 0 ~ k/2-1共计k/2个数；
>                                                                                                     
>   然后下一步就是重新进行遍历，现在需要找的是第k-k/2=k/2小的数了；
>                                                                                                     
>   通过这样每次都去掉[0]......[k/2-1]的方式，逐步的减少k的值，最后逼近到临界值，就可以确认k了。
>   ```

## 4	2024-04-12

### **（1）最长回文字串**

**个人实现**：

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        if (n == 0){
            return "";
        }
        int resultL = 0,resultR = 0;
        int maxLength = 0;
        for (int i = 0; i < n && n - i >= maxLength; i++) {
            for (int j = n-1; j > i && j - i + 1 >= maxLength; j--){
                if (isPalindrome(s, i ,j)){
                    if ( j - i + 1 > maxLength){
                        maxLength = j - i + 1;
                        resultL = i;
                        resultR = j;
                    }
                }

            }
        }
        return s.substring(resultL, resultR+1);
    }
    public boolean isPalindrome(String s, int i, int j){
        while(i <= j){
            if (s.charAt(i) != s.charAt(j)){
                return false;
            }
            i++;
            j--;
        }
        return true;
    }
}
```

> - ```
>   简单的说就是从字符串最左边的数开始遍历，在每一次遍历中：
>   ```
>
>   1. ```
>      从字符串的最右边倒序遍历所有的字符，当遇到首个遍历的对象与当前的对象之间的所有字符能构成一个回文串的时候退出遍历；
>                                                                                                                                                                                                                                                           
>      记录下得到回文字符串的时候，记录长度，与maxLength进行比较，当超过当前的maxLenght的时候取缔maxLength，并记录起点与终点位置；
>      ```
>
> - ```
>   之前还有过一种思路：
>   就是构造一个类似栈的结构，这个栈的可以操作栈顶和次栈顶；
>   每次检查字符串的数据的时候，都取出字符串的该数据，将其与栈顶、次栈顶的数据进行比较，如果相同，则代表可以构成回文字符串。
>                                                                                                     
>   但是这其实存在一个问题，并不是下一个立马出现的相同的字符能使其构成最大回文字符串。
>   比如ababa，在这个思路中，会将第二个a与第一个a进行配对，构成回文字符串，然而这并不是最长回文字符串的构造方式，应该取第一个a和第三个a进行配对组成会问字符串。
>   ```

**官方题解**：

```java
class Solution {
    public String longestPalindrome(String s) {
        int len = s.length();
        if (len < 2) {
            return s;
        }

        int maxLen = 1;
        int begin = 0;
        // dp[i][j] 表示 s[i..j] 是否是回文串
        boolean[][] dp = new boolean[len][len];
        // 初始化：所有长度为 1 的子串都是回文串
        for (int i = 0; i < len; i++) {
            dp[i][i] = true;
        }

        char[] charArray = s.toCharArray();
        // 递推开始
        // 先枚举子串长度
        for (int L = 2; L <= len; L++) {
            // 枚举左边界，左边界的上限设置可以宽松一些
            for (int i = 0; i < len; i++) {
                // 由 L 和 i 可以确定右边界，即 j - i + 1 = L 得
                int j = L + i - 1;
                // 如果右边界越界，就可以退出当前循环
                if (j >= len) {
                    break;
                }

                if (charArray[i] != charArray[j]) {
                    dp[i][j] = false;
                } else {
                    if (j - i < 3) {
                        dp[i][j] = true;
                    } else {
                        dp[i][j] = dp[i + 1][j - 1];
                    }
                }

                // 只要 dp[i][L] == true 成立，就表示子串 s[i..L] 是回文，此时记录回文长度和起始位置
                if (dp[i][j] && j - i + 1 > maxLen) {
                    maxLen = j - i + 1;
                    begin = i;
                }
            }
        }
        return s.substring(begin, begin + maxLen);
    }
}
```

> **核心思路**：
>
> 1. ```
>    回文字符串的子串也都是回文的。因此找最大的回文字符串可以从所有回文的字串入手。
>    ```
>
> 2. ```
>    因此要确认一个字符串是否是回文的可以通过如下的判定方式：
>    字符串的左边界与有边界是否相等 && 字符串除开左右边界的字符之后的字串是否是回文字串。
>    当这两个条件满足之后，这个字符串也就是一个回文字符串。
>    ```
>
> 3. ```
>    另一个难点就是，如果使得字符串与其子字符串产生联系呢？
>    答案是通过数组记录的方式。
>                                                                                                                                                       
>    构建一个二维的数组，boolean[][] dp == new boolean[len][len];
>    存储是否为字符串的判断。
>    比如字符s的第i到j的字符串是否为回文字符串，可以记录dp[i][j] = true;
>    ```
>
> **思路总结**：
>
> 解答的一种很重要的思想就是：**状态转移**。
>
> ![image-20240412204045095](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240412204045095.png)
>
> **额外拓展**：
>
> ```
> 本实现还有一种更佳的，时间复杂度更低的实现方式：【O(n)】
> 
> Manacher 算法
> 
> https://leetcode.cn/problems/longest-palindromic-substring/solutions/255195/zui-chang-hui-wen-zi-chuan-by-leetcode-solution/
> ```

## 5	2024-04-20

### **（1）Z 字形变换**

**个人实现**： 	

```java

class Solution {
    public String convert(String s, int numRows) {
        if (numRows == 1){
//            for (int i = 0 ; i < s.length(); i++){
//                System.out.println(s.charAt(i));
//            }
            return s;
        }
        int length = s.length();
        char[][] data = new char[numRows][length/2 + length % 2];
//        System.out.println(length/2 + length % 2);
        int i=0,j=0;
        boolean fallFlag = true;
        for (int count=0; count < length; count++){
            char presentChar = s.charAt(count);
//            System.out.println("当前字符是");
//            System.out.println(presentChar);
            data[i][j] = presentChar;
//            System.out.println("该字符的放置位置是：");
//            System.out.println("第"+i+"行"+j+"列");
//            System.out.println();

            if (i == 0){
                fallFlag = true;
            }
            if (fallFlag){
                i++;
                if (i == numRows){
                    fallFlag = false;
                    i-=2;
                    j++;
//                    if (numRows != 1){
//                        i-=2;
//                    }else {
//                        i=0;
//                    }
                }
            }else {
                j++;
                i--;
            }
        }
        StringBuilder sb = new StringBuilder();

        for (int m = 0; m < numRows ; m++){
            for (int n = 0 ; n < length / 2 + length % 2; n++){
                if (data[m][n] != '\0'){
                    sb.append(data[m][n]);
//                    System.out.print(data[m][n]);
                }
            }
        }
        String result = sb.toString();
        System.out.println(result); // 输出字符串s

        return result;
    }
}
```

> 构建一个数组，挨个的读取字符串的字符，将字符塞到合理的位置。
>
> 没有进一步的思考列数与numRows和s.length的关系了，而是直接通过length/2这种简单讨巧的方式。

**官方题解**：

```java
class Solution {
    public String convert(String s, int numRows) {
        int n = s.length(), r = numRows;
        if (r == 1 || r >= n) {
            return s;
        }
        int t = r * 2 - 2;
        int c = (n + t - 1) / t * (r - 1);
        char[][] mat = new char[r][c];
        for (int i = 0, x = 0, y = 0; i < n; ++i) {
            mat[x][y] = s.charAt(i);
            if (i % t < r - 1) {
                ++x; // 向下移动
            } else {
                --x;
                ++y; // 向右上移动
            }
        }
        StringBuffer ans = new StringBuffer();
        for (char[] row : mat) {
            for (char ch : row) {
                if (ch != 0) {
                    ans.append(ch);
                }
            }
        }
        return ans.toString();
    }
}
```

> **核心思路**：
>
> 和自己思路差不多，不过有很多地方有优化：
>
> 1. ```
>    首先是对lenght=1，直接返回原字符串；
>                                                                                                                                                    
>    这可以进一步，当length>numRows的时候，也可以直接返回原字符串。
>    ```
>
> 2. ```
>    然后就是找数组的列的时候，并没有像自己思考的那样简单的将列设置为length/2，而是通过数学的推导，先找到周期，然后找到每个周期使用的字符的数量，最后算出数组需用的列数。
>                                                                                                                                                    
>    其中，在求周期的这个地方有一个内容：
>    比如当要求⌈n/2⌉的时候，也就是不满的按照满处理，对于n=15的时候，n/2需要等于8，而不是7，这个时候在代码中可以使用new ArrayList
>    int result = (n + 1) / 2，这样就能满足条件了。
>                                                                                                                                                    
>    同样的，对于不只是2的底数t的时候，可以遵循下面的式子：
>                                                                                                                                                    
>    int result = (n + t - 1)/t;
>    这样，只要result除完t后还有任何余数都会被算进成为一个新的周期，也就是所谓的⌈n/t⌉的效果实现了。
>    ```
>
> 3. ```
>    另外，数在下移还是右上移的判断方式，是基于周期的判断，每个周期内的字符的数量是r + r - 2，也就是每个周期内前r个数都是下移，之后的数都是右上移。
>                                                                                                                                                    
>    而在自己的实现中，是通过创建了一个移动标识，当标识为true的时候下移动，为false的时候右上移动，标识的判别方式通过i的值来改变。每次当i触碰到行的初始位置或者是底部位置都会引起移动表示的改变。
>    ```

### （2）组合总和

**个人实现**：

```java

class Solution {
    private int[] candidates; // 将candidates设置为Solution类的成员变量
    private List<List<Integer>> resultList;

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        this.candidates = candidates; // 设置成员变量candidates为传入的candidates数组
        this.resultList = new ArrayList<>();

        ArrayList<Integer> row1 = new ArrayList<>();
        findNumDFS(row1, target);
        removeDuplicates(this.resultList);
        System.out.println(this.resultList);
        return this.resultList;
    }
    public void findNumDFS(List<Integer> findlist, int targetNum){
        for(int i = 0 ; i < candidates.length; i++){
            if (candidates[i] == targetNum){
                findlist.add(candidates[i]);
                resultList.add(new ArrayList<>(findlist));
//                resultList.add(findlist);
            }else if(targetNum-candidates[i] > 0){
                findlist.add(candidates[i]);
                findNumDFS(findlist, targetNum-candidates[i]);
            }else {
                continue;
            }
//            此处的remove什么时候会执行到，是深度遍历中的一个值得思考的地方
//            当if或者else if判断成功，并且其执行的子findNumDFS查询都失败之后，会跳出并执行remove。
//            实际上，就是对于每次的for的每一个i的判断的末尾，需要删除findlist的末尾的数据的操作。
            findlist.remove(findlist.size()-1);
        }
    }
    private static void removeDuplicates(List<List<Integer>> resultList) {
        Set<List<Integer>> set = new HashSet<>();
        List<List<Integer>> temp = new ArrayList<>();

        for (List<Integer> list : resultList) {
            Collections.sort(list); // 对每个列表进行排序
            if (set.add(list)) { // 如果加入到 set 中成功，说明之前没有相同的列表
                temp.add(list); // 将不重复的列表添加到 temp 中
            }
        }
        resultList.clear();
        resultList.addAll(temp);
    }
}
```

> 核心是深度遍历DFS。
>
> 当深度遍历的终点中是candidates中的候选字符的时候，就代表该条遍历是成功的。
>
> 每个深度的遍历的结束是targetNum-candidates[i] > 0的判断为fasle。
>
> 最后输出的结果是各种可能的深度遍历的结果，会出现类似于下面这种形式的数据：
> [[2, 2, 3], [2, 3, 2], [3, 2, 2], [7]]。
>
> 因此需要使用removeDuplicates来去除重复的数据，removeDuplicates利用了set中不会重复添加数据的特点，每一次添加判定成功的时候，都代表是不重复的数据。

**官方题解**：

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> ans = new ArrayList<List<Integer>>();
        List<Integer> combine = new ArrayList<Integer>();
        dfs(candidates, target, ans, combine, 0);
        return ans;
    }

    public void dfs(int[] candidates, int target, List<List<Integer>> ans, List<Integer> combine, int idx) {
        if (idx == candidates.length) {
            return;
        }
        if (target == 0) {
            ans.add(new ArrayList<Integer>(combine));
            return;
        }
        // 直接跳过
        dfs(candidates, target, ans, combine, idx + 1);
        // 选择当前数
        if (target - candidates[idx] >= 0) {
            combine.add(candidates[idx]);
            dfs(candidates, target - candidates[idx], ans, combine, idx);
            combine.remove(combine.size() - 1);
        }
    }
}
```

> **核心思路**：
>
> 基本思路差不多，没有使用for循环。
>
> 1. ```
>    基本思路一样，唯一的区别在于没有使用for进行循环，而是使用了
>                                                                                                                                                    
>    			dfs(candidates, target, ans, combine, idx + 1);
>                                                                                                                                                    
>    来对candidates的下一个数进行判断的操作。【下一个数的判定是idx + 1】
>                                                                                                                                                    
>    而
>                dfs(candidates, target - candidates[idx], ans, combine, idx);
>    是对当前的candidates的候选数进行继续判断。
>                                                                                                                                                    
>    下面这个图很好的解释了官方题解的运作方式。
>    ```
>
>    ![image-20240420195343821](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240420195343821.png)

## 6	2024-04-21

### （1）组合总和Ⅲ

**个人解答**：

```java
class Solution {
    private List<List<Integer>> resultList;
    private int kNum;
    public List<List<Integer>> combinationSum3(int k, int n) {
        this.resultList = new ArrayList<>();
        this.kNum = k;
        int presentNum = 1;
        List<Integer> findList = new ArrayList<>();
        dfs(n, findList, presentNum);
        return resultList;
    }
    public void dfs(int targetNum, List<Integer> findList, int presentNum){
        if (findList.size() == kNum && targetNum == 0){
            resultList.add(new ArrayList<>(findList));
            return;
        }
        if (findList.size() > kNum || presentNum > 9 || targetNum < 0){
            return;
        }
        dfs(targetNum, findList, presentNum+1);
        if (presentNum <= targetNum){
            findList.add(presentNum);
            dfs(targetNum-presentNum, findList, presentNum+1);
            findList.remove(findList.size() - 1);
        }

    }
}
```

> 思路和昨天的很类似，也是通过dfs实现的。
>
> 需要注意的几个点是：
>
> 1. ```
>    不能重复的使用候选字符。
>    因此在
>            if (presentNum <= targetNum){
>                findList.add(presentNum);
>                dfs(targetNum-presentNum, findList, presentNum+1);
>                findList.remove(findList.size() - 1);
>            }
>    中的
>    			dfs(targetNum-presentNum, findList, presentNum+1);
>    里的是presentNum+1，这是因为findList添加了当前的字符之后，就不能再选择当前的字符作为下一个dfs的判断内容了，需要使用presentNum+1作为下一个深度搜索。
>    ```
>
> 2. ```
>    自己的实现中，没有额外的创建一个数组作为candidates。
>    由于候选字符是1~9，所以直接使用了presentNum=1作为候选字符，每次都是将presentNum+1作为下一个候选字符的判断，因此在代码跳出判断中
>            if (findList.size() > kNum || presentNum > 9 || targetNum < 0){
>                return;
>            }
>    存在
>            presentNum > 9
>    这样一个判断，这个判断的作用实际上和超出candidates.length()的检查判断是一个效果。
>    但是这其实会带来一个小bug，就是当候选字符全部添加上的时候，如果跳出判断：
>            if (findList.size() > kNum || presentNum > 9 || targetNum < 0){
>                return;
>            }
>    在检测成功判断：
>            if (findList.size() == kNum && targetNum == 0){
>                resultList.add(new ArrayList<>(findList));
>                return;
>            }
>    的前边，也就是
>            if (findList.size() > kNum || presentNum > 9 || targetNum < 0){
>                return;
>            }
>            if (findList.size() == kNum && targetNum == 0){
>                resultList.add(new ArrayList<>(findList));
>                return;
>            }
>    这样一个实现，那么会导致最后的k=9,n=45的时候无法返回正确结果，具体原因可以尝试调试一下。
>    这就是为什么需要检测成功判断的if需要在跳出判断的前边。
>    ```
>
> 3. ```
>    还有一个问题是，需要在检测判断：
>            if (findList.size() == kNum && targetNum == 0){
>                resultList.add(new ArrayList<>(findList));
>                return;
>            }
>    里面加上return语句
>    这是为什么呢？
>    因为如果不在这里面加上return的话，会重复的添加相同的结果进入resultList,这会导致结果中出现若干个相同的List。
>    ```

**官方题解**：

```java
class Solution {
    List<Integer> temp = new ArrayList<Integer>();
    List<List<Integer>> ans = new ArrayList<List<Integer>>();

    public List<List<Integer>> combinationSum3(int k, int n) {
        dfs(1, 9, k, n);
        return ans;
    }

    public void dfs(int cur, int n, int k, int sum) {
        if (temp.size() + (n - cur + 1) < k || temp.size() > k) {
            return;
        }
        if (temp.size() == k) {
            int tempSum = 0;
            for (int num : temp) {
                tempSum += num;
            }
            if (tempSum == sum) {
                ans.add(new ArrayList<Integer>(temp));
                return;
            }
        }
        temp.add(cur);
        dfs(cur + 1, n, k, sum);
        temp.remove(temp.size() - 1);
        dfs(cur + 1, n, k, sum);
    }
}
```

> **核心思路**：
>
> 差不多。也是深度递归的思想。
>
> 不过官方的思路是每次都将数塞进findList，当findList的size为满足条件的k的时候，就进行一次累加，看存在于findList里的值之和是否与n相同，相同则添加进resultList。

## 7	2024-04-22

### （1）组合总和Ⅳ

**个人题解**：

```java
class Solution {
    private int count = 0;
    private int [] candidates;
    private int[] memo = new int[1001];

    public int combinationSum4(int[] nums, int target) {
        candidates = nums;
        Arrays.fill(memo, -1);
        dfs(target, 0);
        return count;
    }
    public void dfs(int targetNum, int presentNum){
        if (targetNum == 0){
            count++;
            return;
        }
        if (presentNum >= candidates.length || targetNum < 0){
            return;
        }
        if (memo[targetNum] != -1){
            count += memo[targetNum];
            return;
        }
        dfs(targetNum, presentNum+1);
        if (targetNum - candidates[presentNum] >= 0){
            int temp = count;
            dfs(targetNum - candidates[presentNum], 0);
            memo[targetNum - candidates[presentNum]] = count - temp;
        }
    }
}
```

> **核心思路**：
>
> 核心思路仍然是使用深度优先遍历。【虽然这并不是最简单快速的方法】
>
> 不过需要注意以下几点：
>
> 1. ```
>    首先是题目中，包含的结果，可以允许字符集的不同排列顺序作为计数；
>    比如[1,2,1]和[1,1,2]就是两种结果。当然还有[2,1,1]也是一种结果。
>                                                                                                                                                    
>    这样的话，会导致什么问题呢？
>    按照之前的深度优先的实现，一般都是通过一个“跳过”深度优先搜索：
>            dfs(targetNum, presentNum+1);
>    用来开启遍历，跳过完成之后，根据条件判断是否对当前的字符进行深度优先搜索：
>            if (targetNum - candidates[presentNum] >= 0){
>                int temp = count;
>                dfs(targetNum - candidates[presentNum], 0);
>                memo[targetNum - candidates[presentNum]] = count - temp;
>            }
>    这里面开启的下一个遍历的candidates的位置设置为0，而不是presentNum，
>                dfs(targetNum - candidates[presentNum], 0);
>    而在以前的类似实现中，都是写的是：
>                dfs(targetNum-presentNum, findList, presentNum+1);
>    也就是当前的字符处理完之后，从下一个字符开始进行深度优先搜索。
>                                                                                                                                                    
>    乍一看，本体的实现方式可能会导致更多的时间花费，但是这也是本体的问题所在，如果采用的是下面的这种实现，那么遍历便存在了“顺序”，也就是只会有[3,2,1],而不会出现[3,1,2]这样的结果。
>    因此，每次的深度优先搜索，都需要从头开始遍历，这样就不会导致“顺序”的存在。
>    ```
>
> 2. ```
>    本题需要添加“备忘录”这一数据结构，不然在深度搜索的时候会存在大量的重复。
>                                                                                                                                                    
>    需要用一个memo来记录每个target的count值：
>            if (targetNum - candidates[presentNum] >= 0){
>                int temp = count;
>                dfs(targetNum - candidates[presentNum], 0);
>                memo[targetNum - candidates[presentNum]] = count - temp;
>            }
>    关键也就是在：
>                memo[targetNum - candidates[presentNum]] = count - temp;
>    这段代码中，每当前面的dfs结束搜索，那么此时count的增加值就是其搜索的值的count值；
>    其搜索的是targetNum - candidates[presentNum]，增加的值也就是count - temp；因此记录为如上所示。
>                                                                                                                                                    
>    而在每次搜索开启时候，会对memo值进行检查，也就是
>            if (memo[targetNum] != -1){
>                count += memo[targetNum];
>                return;
>            }
>    如果memo已经记录的对应的target的值的count数，那么可以直接读取并增加到count中。
>    ```

**官方题解**：

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target + 1];
        dp[0] = 1;
        for (int i = 1; i <= target; i++) {
            for (int num : nums) {
                if (num <= i) {
                    dp[i] += dp[i - num];
                }
            }
        }
        return dp[target];
    }
}
```

> **核心思路**：
>
> 采用了动态规划的核心思路。
>
> 其核心思想是：
>
> 1. ```
>    如果存在一种排列，其中的元素之和等于 i，则该排列的最后一个元素一定是数组 nums 中的一个元素。
>                                                                                                                                                    
>    那么也就是说，每一个target的值，都是可以通过先前的记录来计算的。
>                                                                                                                                                    
>    计算方法为，遍历一次候选值，也就是nums，而target-nums[i]的值+num[i]的值必定等于target。
>    ```
>
> 2. ```
>    因此，构造一个dp数组，用来记录从0到target的所有的数的count值。
>                                                                                                                                                    
>    比如对于nums = [1,2,3]，已经得到了target = 1的count值为1，target=2的count值为2；
>                                                                                                                                                    
>    记录dp[1] = 1,dp [2] = 2
>                                                                                                                                                    
>    那么target = 3的值的计算方式为：
>    遍历一遍所有的候选数，从nums[0] ~ num[2]，也就是1，2，3；
>    首先对于1，有1<=target，所以dp[3] += dp[target - 1];也就是+1
>    对于2，有2<=target，所以dp[3] += dp[target - 2];也就是+2
>    对于3，有3<=target，所以dp[3] += dp[target - 3];也就是+1
>                                                                                                                                                    
>    因此dp[3] = 4；
>    ```

## 8	2024-04-23

### （1）爱生气的书店老板

**个人题解**：

```java

class Solution {
    public int maxSatisfied(int[] customers, int[] grumpy, int minutes) {
        int sum = 0;
        int[] unsatisfied = Arrays.copyOf(grumpy, grumpy.length);
        int maxAttitudeChange = 0;
        int AttitudeChange = 0;
        int restMinutes = minutes;

        for (int i = 0; i < unsatisfied.length; i++){
            unsatisfied[i] = unsatisfied[i] * customers[i];
//            System.out.println(unsatisfied[i]);

//            满意的客户数量
            sum += customers[i];
            sum -= unsatisfied[i];

            if (restMinutes == 0 && minutes != 0){
                restMinutes++;
                AttitudeChange -= unsatisfied[i-minutes];
            }
            if (restMinutes != 0){
                AttitudeChange += unsatisfied[i];
                restMinutes-- ;
            }
            maxAttitudeChange = Math.max(maxAttitudeChange, AttitudeChange);
        }
//        满意的客户数量
//        System.out.println(sum);
//        System.out.println(maxAttitudeChange);
        sum += maxAttitudeChange;
        System.out.println(sum);

        return sum;
    }
}
```

> **核心思路**：
>
> 构造不满意的用户列表unsatisfied[]，计算unsatisfied中，长度为minutes的子串的最大和值。

**官方题解**：

```java
class Solution {
    public int maxSatisfied(int[] customers, int[] grumpy, int minutes) {
        int total = 0;
        int n = customers.length;
        for (int i = 0; i < n; i++) {
            if (grumpy[i] == 0) {
                total += customers[i];
            }
        }
        int increase = 0;
        for (int i = 0; i < minutes; i++) {
            increase += customers[i] * grumpy[i];
        }
        int maxIncrease = increase;
        for (int i = minutes; i < n; i++) {
            increase = increase - customers[i - minutes] * grumpy[i - minutes] + customers[i] * grumpy[i];
            maxIncrease = Math.max(maxIncrease, increase);
        }
        return total + maxIncrease;
    }
}
```

> **核心思路**：
>
> 简单不记录。

## 9	2024-04-24

### （1）感染二叉树需要的总时间——2385

**个人题解**：

```
暂无，二叉树暂时跳过。
```

## 10	2024-04-25

### （1）总行驶距离——2739

**个人题解**：

```java
class Solution {
    public int distanceTraveled(int mainTank, int additionalTank) {
        int sum = 0;
        while(mainTank >= 5){
            if (additionalTank > 0){
                mainTank -= 4;
                additionalTank--;
                sum += 5;
            }else {
                break;
            }
        }
        sum += mainTank;
        return sum*10;
    }
}
```

> 推到不出来数学公式，无脑算

**官方题解**：

```java
class Solution {
    public int distanceTraveled(int mainTank, int additionalTank) {
        int ans = 0;
        while (mainTank >= 5) {
            mainTank -= 5;
            ans += 50;
            if (additionalTank > 0) {
                additionalTank--;
                mainTank++;
            }
        }
        return ans + mainTank * 10;
    }
}
```

> 基本没啥区别。

### （2）整数反转

**个人题解**：

```java

class Solution {
    public int reverse(int x) {
        boolean negative = false;
        if (x == -2147483648){
            return 0;
        }
        if (x < 0){
            negative = true;
            x = -x;
        }
        String xString = Integer.toString(x);
        char[] xChar = new char[xString.length()];
        for (int i = xString.length() - 1; i>=0; i--){
            xChar[xString.length() - 1 - i] = xString.charAt(i);
//            System.out.println(xChar[xString.length() -1 - i]);
        }
        xString = new String(xChar);
        long z = Long.parseLong(xString);
        if (negative){
            z = -z;
        }
        if (z > 2147483647 || z < -2147483648){
            System.out.println("超出范围");
            return 0;
        }
        x = Integer.parseInt(xString);
        if (negative){
            x = -x;
        }
        System.out.println(x);
        return x;
    }
}
```

> 核心思路：
>
> 转换成String类型，然后调换后再换回来。
>
> 实际上是一种很不好的实现方式。

**官方题解**：

```java
class Solution {
    public int reverse(int x) {
        int rev = 0;
        while (x != 0) {
            if (rev < Integer.MIN_VALUE / 10 || rev > Integer.MAX_VALUE / 10) {
                return 0;
            }
            int digit = x % 10;
            x /= 10;
            rev = rev * 10 + digit;
        }
        return rev;
    }
}
```

> 核心思路：
>
> **待查看**



## 11	2024-04-26

### （1）快照数组

**个人题解**：

```java

class SnapshotArray {
    Map<Integer, Integer> hashtable = new HashMap<>();
    List<Map<Integer, Integer>> mapList = new ArrayList<>();
    Map<Integer, Integer> snapIdMap = new HashMap<>();
    int snapCount = 0;
    Boolean change = false;

    public SnapshotArray(int length) {
    }

    public void set(int index, int val) {
//        arrayAll[snapCount][index] = val;
        hashtable.put(index, val);
        change = true;
    }

    public int snap() {
//        System.arraycopy(arrayAll[snapCount], 0, arrayAll[snapCount+1], 0, arrayAll[snapCount].length);
//        arrayAll[snapCount+1] = arrayAll[snapCount] ;

        if (change){
            mapList.add(new HashMap<>(hashtable));
            change = false;
        }
        snapIdMap.put(snapCount, mapList.size() - 1);
//        hashtable = new HashMap<Integer, Integer>();
        return snapCount++;
    }

    public int get(int index, int snap_id) {

        snap_id = snapIdMap.get(snap_id);
        if (snap_id == -1){
            return 0;
        }

        Map<Integer, Integer> tempMap = mapList.get(snap_id);
//        if (index >= tempMap.size()){
//            return 0;
//        }
        if (tempMap.containsKey(index)){
            return tempMap.get(index);
        }
        return 0;
    }
}
```

> 核心思路：
>
> 1. ```
>    对于每一次快照，新构建一个Map：
>        Map<Integer, Integer> hashtable = new HashMap<>();
>                                                                                                                                                     
>    每当进行一次set，执行：
>            hashtable.put(index, val);
>                                                                                                                                                         
>    对于每一次拍摄快照snap，将当前的hashtable存入构建的mapList
>       		List<Map<Integer, Integer>> mapList = new ArrayList<>();
>            mapList.add(new HashMap<>(hashtable));
>    ```
>
> 2. ```
>    存在的问题一：
>    不断的拍摄快照，每次快照之前并没有执行set，这意味着快照的内容都是重复的。
>                                                                                                                                                 
>    这个时候就不需要把每次的快照中的hashtable存入mapList中。
>                                                                                                                                                 
>    这样可能会导致序号问题，也就是怎么找那些快照呢？
>    所以需要构建一个snapIdMap，用来对应snapId在mapList中的位置。
>        	Map<Integer, Integer> snapIdMap = new HashMap<>();
>        	                                                                                                                                             
>    核心代码为：
>            if (change){
>                mapList.add(new HashMap<>(hashtable));
>                change = false;
>            }
>            snapIdMap.put(snapCount, mapList.size() - 1);
>    也就是如果发生改动【进行过set】，则将hashtable存入mapList，否则，不存入；
>    最后一句代码在snapIdMap存入snapId与mapList的对应关系。
>    ```

**官方题解**：

```java
class SnapshotArray {
    private int snap_cnt;
    private List<int[]>[] data;

    public SnapshotArray(int length) {
        snap_cnt = 0;
        data = new List[length];
        for (int i = 0; i < length; i++) {
            data[i] = new ArrayList<int[]>();
        }
    }

    public void set(int index, int val) {
        data[index].add(new int[]{snap_cnt, val});
    }

    public int snap() {
        return snap_cnt++;
    }

    public int get(int index, int snap_id) {
        int x = binarySearch(index, snap_id);
        return x == 0 ? 0 : data[index].get(x - 1)[1];
    }

    private int binarySearch(int index, int snap_id) {
        int low = 0, high = data[index].size();
        while (low < high) {
            int mid = low + (high - low) / 2;
            int[] pair = data[index].get(mid);
            if (pair[0] > snap_id + 1 || (pair[0] == snap_id + 1 && pair[1] >= 0)) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }
        return low;
    }
}
```

> 核心思路：
>
> **不是通过构建一个横向的数据结构，而是构建一个竖向的数据结构。**
>
> 1. ```
>    个人在实现的时候，是对于每一次的快照来考虑的；
>                                                                                                                                                 
>    而官方题解中，是构建了一个长度为length的数组。竖向的存储每次快照的内容。
>                                                                                                                                                 
>    也就是每一次set的时候，找到其进行修改的位置，然后在这个位置中，记录下最新的这一次修改。
>                                                                                                                                                 
>    举个例子：
>                                                                                                                                                 
>    对于第3次快照，进行了set(2,5);
>    在位置为2的地方插入了数据5。
>                                                                                                                                                 
>    在自己的实现中，会对进行到第三次遍历的时候，针对上一次的hashMap的修改，修改其对应位置的内容。然后在同步。
>                                                                                                                                                 
>    而在这个实现中，是优先找到其修改的位置，也就是2，在位于2的这处的data，增加一个[3,5]的数据，意味着：
>    			“在位置为2的地方，在第三次的快照中，其值变成了5。”
>    ```

## 12	2024-04-27

### （1）查询网格图中每一列的宽度——2639

**个人题解**：

```java
class Solution {
    public int[] findColumnWidth(int[][] grid) {
        int [] result = new int[grid[0].length];
        for (int j = 0; j < grid[0].length; j++){
            for (int i=0; i < grid.length; i++){
                result[j] = Math.max(result[j], String.valueOf(grid[i][j]).length());
            }
        }
        return result;
    }
}
```

> 没啥难度，题目一开始有点难读懂想表达什么意思。

**官方题解**：

```java
class Solution {
    public int[] findColumnWidth(int[][] grid) {
        int n = grid.length, m = grid[0].length;
        int[] res = new int[m];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                int x = grid[i][j];
                int length = 0;
                if (x <= 0) {
                    length = 1;
                }
                while (x != 0) {
                    length += 1;
                    x /= 10;
                }
                res[j] = Math.max(res[j], length);
            }
        }
        return res;
    }
}
```

> 官方题解没有使用转换成字符串的操作，而是通过每次/=10来一遍一遍的判断长度。
>
> 其他的思路都是一样的。

## 13	2024-04-28

### （1）负二进制转换——1017

**个人题解**：

```java

class Solution {
    public String baseNeg2(int n) {
        int sum = 0;
        int pres2Index = 0;
        int pres2Num = 1;
        int [] result;

        int[] pos2;
        int[] neg2;
        int[] index2;

        while(true){
            sum += pres2Num;
            if (sum >= n){
                pres2Index++;
                break;
            }
            pres2Num *= 4;
            pres2Index += 2;
        }
//        System.out.println(pres2Index);
        result = new int[pres2Index];
        pos2 = new int[pres2Index];
        neg2 = new int[pres2Index];
        index2 = new int[pres2Index];

        pres2Num = 1;
        sum=0;
        for (int i=0;i < pres2Index; i+=2){
            sum += pres2Num;
            pos2[i] = sum;
            index2[i] = pres2Num;
            pres2Num *= 4;
        }
        pres2Num = -2;
        sum = 0;
        for (int i=1 ; i < pres2Index; i+=2){
            sum += pres2Num;
            neg2[i] = sum;
            index2[i] = pres2Num;
            pres2Num *= 4;
        }
        int posIndex = pres2Index-1;
        int negIndex = pres2Index-2;

        while (true){
            if (n == 0){
                break;
            }
            if (n > 0){
                while (posIndex > 0 && pos2[posIndex - 2] >= n){
                    posIndex -= 2;
                }
                result[posIndex] = 1;
                n -= index2[posIndex];
                posIndex -= 2;
            }else if (n < 0){
                while (negIndex > 1 && neg2[negIndex - 2] <= n){
                    negIndex -= 2;
                }
                result[negIndex] = 1;
                n -= index2[negIndex];
                negIndex -= 2;
            }
        }
        System.out.println();
        StringBuilder sb = new StringBuilder();
//        for (int i : result) {
//            sb.append(i);
//        }
        for (int i = result.length -1; i>=0; i--){
            sb.append(result[i]);
        }
        String b = sb.toString();
        System.out.println(b);
        return b;
    }
}
```

> **核心思路**：
>
> 。。。。代码写的越来越丑陋了
>
> 
>
> - ```
>   由于给出的测试数据全是正数；
>   因此需要确认符合结果的数组的长度；
>   构建一个数组，各自存放路径上的正数合与负数合，以及一个2的次方数组；
>                                                                                                 
>   比如对于int n = 53;
>   构建的正数合数组：[1, 0, 5, 0, 21, 0, 85]
>   构建的负数合数组：[0, -2, 0, -10, 0, -42, 0]
>   构建的2的次方数组：[1, -2, 4, -8, 16, -32, 64]
>   【优化一下的话，前两个数组可以合并为一个数组，毕竟数据不冲突】
>   ```
>
> - ```
>   然后找n的核心思路就变成了：
>           while (true){
>               if (n == 0){
>                   break;
>               }
>               if (n > 0){
>                   while (posIndex > 0 && pos2[posIndex - 2] >= n){
>                       posIndex -= 2;
>                   }
>                   result[posIndex] = 1;
>                   n -= index2[posIndex];
>                   posIndex -= 2;
>               }else if (n < 0){
>                   while (negIndex > 1 && neg2[negIndex - 2] <= n){
>                       negIndex -= 2;
>                   }
>                   result[negIndex] = 1;
>                   n -= index2[negIndex];
>                   negIndex -= 2;
>               }
>           }
>   核心思路是根据当前的n，判断正负后，在各自的合数组中，找到最大的，能“包住”这个值的边缘的数。
>   比如对于n=53,在正数合数组中找，大于等于这个数的最小数，也就是85。
>   然后n减去对应位置的2的次方数，也就是64，重新找n，因此第二步找的是53-64=-11；
>   ```

**官方题解——模拟进位**：

```java
class Solution {
    public String baseNeg2(int n) {
        if (n == 0) {
            return "0";
        }
        int[] bits = new int[32];
        for (int i = 0; i < 32 && n != 0; i++) {
            if ((n & 1) != 0) {
                bits[i]++;
                if ((i & 1) != 0) {
                    bits[i + 1]++;
                }
            }
            n >>= 1;
        }
        int carry = 0;
        for (int i = 0; i < 32; i++) {
            int val = carry + bits[i];
            bits[i] = val & 1;
            carry = (val - bits[i]) / (-2);
        }
        int pos = 31;
        StringBuilder res = new StringBuilder();
        while (pos >= 0 && bits[pos] == 0) {
            pos--;
        }
        while (pos >= 0) {
            res.append(bits[pos]);
            pos--;
        }
        return res.toString();
    }
}
```

> **核心思路**：
>
> 第一种解法看不太懂。
>
> 
>
> 未掌握的知识点：
>
> 1. ```
>    a & b;
>                                                                                                                                                 
>    在Java中，&是一个位运算符，表示按位与操作。它将两个操作数的对应位进行逻辑与操作，如果两个位都为1，则结果位为1，否则为0。
>                                                                                                                                                 
>    1: 0001
>    2: 0010
>    因此1 & 2: 0000，也就是1 & 2 = 0；
>                                                                                                                                                 
>    1: 0001
>    3: 0011
>    因此1 & 3： 0001，也就是1 & 3 = 1；
>                                                                                                                                                 
>    因此&的作用往往是用来判断奇数偶数；
>    形式往往是
>                if ((n & 1) != 0) {
>                }
>    在这句代码中，能进入if判断的就是奇数。
>    ```
>
> 2. ```
>    n >>= 1;
>                                                                                                                                                 
>    这句话的作用相当于是/2，但是其处理的速度比直接使用a /= 2的效果会快一点。
>    比如对于n = 1010，执行            n >>= 1;之后，其值变成了0101,
>    也就是从10，变成了5。
>    ```

**官方解法——进制转换**：

```java
class Solution {
    public String baseNeg2(int n) {
        if (n == 0 || n == 1) {
            return String.valueOf(n);
        }
        StringBuilder res = new StringBuilder();
        while (n != 0) {
            int remainder = n & 1;
            res.append(remainder);
            n -= remainder;
            n /= -2;
        }
        return res.reverse().toString();
    }
}
```

> **核心思路**：
>
> 根据进制转换的规则，并不在意是否是负数，这个计算步骤都能正确的计算。
>
> ![image-20240428160214151](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240428160214151.png)
>
> - ```java
>   //一个非常关键核心在于
>               n -= remainder;
>   ```
>
> - ```java
>   //意识到进制转换的这个思路之后，第一次尝试编写的时候，写出的下边代码，存在错误的测试是n=6；
>   class Solution {
>       public String baseNeg2(int n) {
>           if (n == 0){
>               return "0";
>           }
>           int [] result = new int[32];
>           int index = 0;
>           while (true){
>               if (n == 1 || n == -1){
>                   if (n == -1){
>                       result[index] = 1;
>                       index++;
>                   }
>                   result[index] = 1;
>                   break;
>               }
>               if( (n & 1) == 1 ){
>                   result[index] = 1;
>               }else {
>                   result[index] = 0;
>               }
>               n /= -2;
>               index++;
>           }
>           StringBuilder sb = new StringBuilder();
>           for (int i = index; i >= 0; i--){
>               sb.append(result[i]);
>           }
>           System.out.println(sb.toString());
>           return sb.toString();
>       }
>   }
>   //自己编写的错误出现在哪呢？
>   //由于直接写的是n /= -2;
>   //而没有加上            n -= remainder;这一判断，
>   //导致到-3这一数据的时候，会出现错误。
>   //在进制计算的时候，-3以-2为进制的计算时，计算的结果应该是	2......1
>   //而在直接使用n /= 2的时候，-3/-2 = 1......-1，这里出现的余数的值是-1
>   //所以应该对余数进行限制，只能出现0和1两种结果，所以在官方题解中，使用了
>   //            n -= remainder;
>   //非常巧妙。
>   //使得每次while计算的时候，奇数的计算也会变成“进制计算”中的计算效果，同时不会影响偶数中的计算。
>   ```
>
> - ```java
>   //意识到错误之后，第二次重写的代码。
>   class Solution {
>       public String baseNeg2(int n) {
>           if (n == 0 || n == 1){
>               return String.valueOf(n);
>           }
>           StringBuilder sb = new StringBuilder();
>           while (n != 0){
>               int temp = (n & 1);
>               sb.append(temp);
>               n -= temp;
>               n /= -2;
>           }
>           return sb.reverse().toString();
>       }
>   }
>   ```

### （2）移动零——283

**个人题解**：

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int indexHeadZero = 0;
        int indexNotZero = 0;
        while(indexNotZero < nums.length && indexHeadZero < nums.length){
            if (nums[indexHeadZero] == 0 && nums[indexNotZero] != 0){
                int temp = nums[indexHeadZero];
                nums[indexHeadZero] = nums[indexNotZero];
                nums[indexNotZero] = temp;
            }
            if (nums[indexHeadZero] != 0){
                indexHeadZero++;
            }
            if (indexNotZero <= indexHeadZero || nums[indexNotZero] == 0){
                indexNotZero++;
            }
        }
    }
}
```

> **核心思路**：
>
> 使用两个指针，第一个指向排在最前的0的位置；另一个指向第一个指针之后的，最先不为0的数的位置。
>
> 两个指针各自不满足条件的时候就需要各自++；
>
> 满足条件就交换两者的位置。

**官方题解——一次遍历**：

```java
class Solution {
    public void moveZeroes(int[] nums) {
        if(nums==null) {
            return;
        }
        //第一次遍历的时候，j指针记录非0的个数，只要是非0的统统都赋给nums[j]
        int j = 0;
        for(int i=0;i<nums.length;++i) {
            if(nums[i]!=0) {
                nums[j++] = nums[i];
            }
        }
        //非0元素统计完了，剩下的都是0了
        //所以第二次遍历把末尾的元素都赋为0即可
        for(int i=j;i<nums.length;++i) {
            nums[i] = 0;
        }
    }
}
```

> 核心思路：
>
> 也是两个指针，不过指针的设置更加巧妙。
>
> 指针一个先导指针，一个后置指针；
>
> 先导指针总是不断的移动；
>
> 当先导指针的值为0的时候，只进行先导指针的移动；
>
> 当先导指针的值不为0的时候，将先导指针此处的值赋给后置指针的位置，赋值完之后，再移一次后置指针；

**官方题解——两次遍历**：

```java
class Solution {
	public void moveZeroes(int[] nums) {
		if(nums==null) {
			return;
		}
		//两个指针i和j
		int j = 0;
		for(int i=0;i<nums.length;i++) {
			//当前元素!=0，就把其交换到左边，等于0的交换到右边
			if(nums[i]!=0) {
				int tmp = nums[i];
				nums[i] = nums[j];
				nums[j++] = tmp;
			}
		}
	}
}
```

> 核心思路和前边的差不多；
>
> 其实从实现效果上，和自己写的代码是一样的实现目的，但是这个代码没用那么多的if来判断，更加的精简，算是对自己实现的方式的优化效果。

## 14	2024-04-29

### （1）将矩阵按对角线排序——1329

**个人题解**：

```java
class Solution {
    public int[][] diagonalSort(int[][] mat) {
//        int i=0;
        for (int j=0; j < mat[0].length; j++){
            int i=0;
            sortMat(mat, i, j);
        }
        for (int i=1; i < mat.length; i++){
            int j=0;
            sortMat(mat, i , j);
        }
        return mat;
    }
    public void sortMat(int[][] mat, int iStart, int jStart){
        int[] array = new int[Math.min(mat.length - iStart, mat[0].length - jStart)];
        int i,j;
        i = iStart;
        j = jStart;
        for (int index = 0 ;i < mat.length && j < mat[0].length; i++, j++, index++){
            array[index] = mat[i][j];
        }
        Arrays.sort(array);
        i = iStart;
        j = jStart;
        for (int index = 0 ;i < mat.length && j < mat[0].length; i++, j++, index++){
            mat[i][j] = array[index];
        }
    }
}
```

> **核心思路**：
>
> 按照对角线的顺序，将每个对角线的数据各自塞入Array中，使用java的Sort排序。

**官方题解**：

```java
class Solution {
    public int[][] diagonalSort(int[][] mat) {
        int n = mat.length, m = mat[0].length;
        List<List<Integer>> diag = new ArrayList<>(m + n);
        for (int i = 0; i < m + n; i++) {
            diag.add(new ArrayList<>());
        }
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                diag.get(i - j + m).add(mat[i][j]);
            }
        }
        for (List<Integer> d : diag) {
            Collections.sort(d, Collections.reverseOrder());
        }
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                mat[i][j] = diag.get(i - j + m).removeLast();
            }
        }
        return mat;
    }
}
```

> **核心思路**：
>
> 核心思路是一样的：依次遍历矩阵，将同一条对角线上的元素放在相同的数组内，对所有数组进行倒序排序，再重新遍历矩阵，将排序好的元素放回矩阵内。
>
> 1. ```java
>    //一个比较关键的地方是
>                    diag.get(i - j + m).add(mat[i][j]);
>    //这是因为对角线的所有的i-j的值是相同的，所以i-j值相同的数，也就是位于同=同一对角线的数。
>    //至于为什么是i - j + m，这是因为矩阵的下标不能为负数，因此给所有的值统一加上一个能使i - j不为0的固定的数，使得矩阵的值不会出现负数就行。
>    //而最小的情况下，也就是i最小，j最大，i最小是0，j最大是mat[0].length，也就是m，所以就给所有的数全部加上了一个m。
>    //这也是为什么
>            List<List<Integer>> diag = new ArrayList<>(m + n);
>    //选择了创建的长度是m+n，而不是选择一个更小的长度。
>    ```

### （2）字母异位词分组——49

**个人题解**：

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        List<List<String>> result = new ArrayList<>();
        String[][] strsSorted = new String[strs.length][2];
//        List<String> listInResult = new ArrayList<>();
        Map<String, Integer> map = new HashMap<>();
        int mapindex = 0;
        for (int i =0; i < strs.length; i++){
            // 将字符串转换为字符数组
            String item = strs[i];
            char[] charArray = item.toCharArray();
            // 对字符数组进行排序
            Arrays.sort(charArray);
            // 将排序后的字符数组转换回字符串
            String charList = new String(charArray);
            strsSorted[i][0] = charList;
            if (map.containsKey(charList)){
                strsSorted[i][1] = map.get(charList).toString();
            }else {
                strsSorted[i][1] = String.valueOf(mapindex);
                map.put(charList, mapindex);
                mapindex++;
            }
        }
        for (int i=0; i < strs.length; i++){
            if (Integer.valueOf(strsSorted[i][1]) >= result.size()){
                List<String> arrayList = new ArrayList<>();
                arrayList.add(strs[i]);
                result.add(arrayList);
            }else {
                result.get(Integer.valueOf(strsSorted[i][1])).add(strs[i]);
            }
        }
        return result;
    }
}
```

> **核心思路**：
>
> - 第一次的思路写的代码如下：
>
>   ```java
>   class Solution {
>       public List<List<String>> groupAnagrams(String[] strs) {
>           List<List<String>> result = new ArrayList<>();
>   
>           List<String> listInit = new ArrayList<>();
>           listInit.add(strs[0]);
>           result.add(listInit);
>   
>           for (int m = 1; m < strs.length; m++){
>   //        for (String item : strs){
>               String item = strs[m];
>   
>               // 将字符串转换为字符数组
>               char[] charArray = item.toCharArray();
>               // 对字符数组进行排序
>               Arrays.sort(charArray);
>               // 将排序后的字符数组转换回字符串
>               String charList = new String(charArray);
>   
>               boolean noAdd = true;
>               for (List<String> listInResult : result){
>                   char[] temp = listInResult.get(0).toCharArray();
>                   Arrays.sort(temp);
>                   String tempString = new String(temp);
>                   if (tempString.equals(charList)){
>                       listInResult.add(item);
>                       noAdd = false;
>                       break;
>                   }
>               }
>               if (noAdd){
>                   List<String> list = new ArrayList<>();
>                   list.add(item);
>                   result.add(list);
>               }
>           }
>           System.out.println();
>           return result;
>       }
>   }
>   ```
>
>   第一次的思路像是暴力破解，各种地方都没有使用到优化，最后果不其然的是超时了。思路是：
>
>   > **注意这是一个超时的写法！**
>
>   将目标数组strs进行遍历，在每一次对strs的遍历的item元素：
>
>   1. 先对item进行一次排序，计算item排序后的charList；
>
>   2. 依次检查result中的每一个List，计算list.get(0)的排序后的tempString；
>
>      > 需要注意的是只需要检查List.get(0)中的数据，List中的其他的数据经过排序后其实都是一样的。
>
>   3. 比较tempString与charList：
>
>      - 如果相等，则代表该item需要隶属于该List下；
>      - 如果不相等，则检查result中的下一个List；
>
>   4. 最后如果全部不符合的话，则在result中新增一个List，并存入当前的item元素
>
> - 第二次的思路中：
>
>   ```java
>   class Solution {
>       public List<List<String>> groupAnagrams(String[] strs) {
>           List<List<String>> result = new ArrayList<>();
>           String[][] strsSorted = new String[strs.length][2];
>   //        List<String> listInResult = new ArrayList<>();
>           Map<String, Integer> map = new HashMap<>();
>           int mapindex = 0;
>           for (int i =0; i < strs.length; i++){
>               // 将字符串转换为字符数组
>               String item = strs[i];
>               char[] charArray = item.toCharArray();
>               // 对字符数组进行排序
>               Arrays.sort(charArray);
>               // 将排序后的字符数组转换回字符串
>               String charList = new String(charArray);
>               strsSorted[i][0] = charList;
>               if (map.containsKey(charList)){
>                   strsSorted[i][1] = map.get(charList).toString();
>               }else {
>                   strsSorted[i][1] = String.valueOf(mapindex);
>                   map.put(charList, mapindex);
>                   mapindex++;
>               }
>           }
>           for (int i=0; i < strs.length; i++){
>               if (Integer.valueOf(strsSorted[i][1]) >= result.size()){
>                   List<String> arrayList = new ArrayList<>();
>                   arrayList.add(strs[i]);
>                   result.add(arrayList);
>               }else {
>                   result.get(Integer.valueOf(strsSorted[i][1])).add(strs[i]);
>               }
>           }
>           return result;
>       }
>   }
>   ```
>
>   思路是：
>
>   1. 核心是构建一个strsSorted [ ? ] [ 2] ，其在 [ ? ] [ 0 ]中存放的是strs[?]的对应的排序后的数据；[ ? ] [ 1 ]中存放是异位词标识；
>
>      <img src="https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240429153947774.png" alt="image-20240429153947774" style="zoom: 80%;" />
>
>   2. 第二个构建的是一个map，用来存放每个异位词的各自标识，因为List中无法存放位置关系，所以单独的构架了一个map来映射不同的异位词的对应标识。
>
>      ```java
>      //核心代码如下：
>      //当map中已经存在当前的异位词时，则在strsSorted[?][1]中赋值当前的异位词标识
>                  if (map.containsKey(charList)){
>                      strsSorted[i][1] = map.get(charList).toString();
>                  }else {
>      //当map中不存在当前的异位词时，则需要更新当前的异位词标识，并将strsSorted[?][1]赋值为新的异位词标识
>                      strsSorted[i][1] = String.valueOf(mapindex);
>                      map.put(charList, mapindex);
>                      mapindex++;
>                  }
>      ```
>
>   3. 当第一次遍历完之后，则根据strsSorted的不同标识，将strs中的元素复赋值到result中。
>
>      ```java
>              for (int i=0; i < strs.length; i++){
>      //如果result缺少了对应当前标识的arrayList，则需要新建一个arrayList并赋值。
>                  if (Integer.valueOf(strsSorted[i][1]) >= result.size()){
>                      List<String> arrayList = new ArrayList<>();
>                      arrayList.add(strs[i]);
>                      result.add(arrayList);
>                  }else {
>      //根据strsSorted的不同标识，将strs中的元素复赋值到result中
>                      result.get(Integer.valueOf(strsSorted[i][1])).add(strs[i]);
>                  }
>              }
>      ```

**官方题解——排序**：

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<String, List<String>>();
        for (String str : strs) {
            char[] array = str.toCharArray();
            Arrays.sort(array);
            String key = new String(array);
            List<String> list = map.getOrDefault(key, new ArrayList<String>());
            list.add(str);
            map.put(key, list);
        }
        return new ArrayList<List<String>>(map.values());
    }
}
```

> **核心思路**：
>
> 构建了一个完美的map数据结构。其映射关系是Map<String, List<String>>
>
> 一开始想的也是这样，但是苦于一直想不到用什么样的数据结构来存放数据导致无法实现。
>
> 其对应的关系是所有异位词排序后的数据作为key，所有的List内容的数据作为value，最后只需要将value赋值到result中就行。

**官方题解——计数**：

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<String, List<String>>();
        for (String str : strs) {
            int[] counts = new int[26];
            int length = str.length();
            for (int i = 0; i < length; i++) {
                counts[str.charAt(i) - 'a']++;
            }
            // 将每个出现次数大于 0 的字母和出现次数按顺序拼接成字符串，作为哈希表的键
            StringBuffer sb = new StringBuffer();
            for (int i = 0; i < 26; i++) {
                if (counts[i] != 0) {
                    sb.append((char) ('a' + i));
                    sb.append(counts[i]);
                }
            }
            String key = sb.toString();
            List<String> list = map.getOrDefault(key, new ArrayList<String>());
            list.add(str);
            map.put(key, list);
        }
        return new ArrayList<List<String>>(map.values());
    }
}
```

> **核心思路**：
>
> > 对第一种方法的优化实现
>
> 核心思路还是构建一个Map，但是在判断异位词的时候，不是采用排序的方式【这种方式消耗的时间更多】来确定是否为同一异位词，而是采用了根据26个词语，每个词语出现的次数，如果有两个item的26中所有字母的出现次数是一样的，则代表是异位词。

## 15	2025-04-30

### （1）满足目标工作时长的员工数目

**个人题解**:

```java
class Solution {
    public int numberOfEmployeesWhoMetTarget(int[] hours, int target) {
        int sum = 0;
        for (int i : hours){
            if (i >= target){
                sum++;
            }
        }
        return sum;
    }
}
```

> **核心思路**：
>
> 最简单的一题，直接秒了。

**官方题解**：

```java
class Solution {
    public int numberOfEmployeesWhoMetTarget(int[] hours, int target) {
        int ans = 0;
        for (int i = 0; i < hours.length; i++) {
            if (hours[i] >= target) {
                ans++;
            }
        }
        return ans;
    }
}
```

> **核心思路**：
>
> 一模一样。

### （2）最长连续序列——128

**个人实现**：

```java
class Solution {
    Set<Integer> occ = new HashSet<>();
    Map<Integer, Integer> hashMap = new HashMap<>();
    public int longestConsecutive(int[] nums) {
        if (nums.length == 0){
            return 0;
        }
        for (int item : nums){
            occ.add(item);
        }
        for (int i : occ)   {
            dfs(i);
        }
        int max = Integer.MIN_VALUE;
        for (int value : hashMap.values()) {
            if (value > max) {
                max = value;
            }
        }
        return max;
    }
    public void dfs(int i){
        if (hashMap.containsKey(i-1)) {
            hashMap.put(i, hashMap.get(i-1)+1);
            return;
        }
        if (occ.contains(i-1)){
            dfs(i-1);
            hashMap.put(i, hashMap.get(i-1)+1);
        }else {
            hashMap.put(i, 1);
        }
    }
}
```

> **核心思想**：
>
> 1. ```java
>    //将所有元素添加入一个Set对象中【用于保证元素不重复】，同时可以利用Set的contains方法。
>    ```
>
> 2. ```java
>    //对occ中的所有元素进行一次dfs遍历，遍历完构建一个HashMap
>        Map<Integer, Integer> hashMap = new HashMap<>();
>    //遍历的时候，先检查hashMap中是否已经有记录的上一个字符的数据。如果有，将其Map对应的value值+1存储。
>                hashMap.put(i, hashMap.get(i-1)+1);
>    //如果hashMap不存在目标元素，则查找occ中的元素，如果occ中存在上一个元素，则dfs遍历上一个元素，直到遍历到底之后，将目标元素加入hashMap中，然后回溯的时候，将每次的元素同样加入hashMap中，不过hashMap中的value+1。
>            if (occ.contains(i-1)){
>                dfs(i-1);
>                hashMap.put(i, hashMap.get(i-1)+1);
>            }else {
>    //如果occ中不存在目标元素，则直接将该元素加入hashMap中，value值记录为1。
>                hashMap.put(i, 1);
>            }
>    ```

**官方题解**：

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> num_set = new HashSet<Integer>();
        for (int num : nums) {
            num_set.add(num);
        }

        int longestStreak = 0;

        for (int num : num_set) {
            if (!num_set.contains(num - 1)) {
                int currentNum = num;
                int currentStreak = 1;

                while (num_set.contains(currentNum + 1)) {
                    currentNum += 1;
                    currentStreak += 1;
                }

                longestStreak = Math.max(longestStreak, currentStreak);
            }
        }

        return longestStreak;
    }
}
```

> **核心思路**：
>
> **每个数都判断一次这个数是不是连续序列的开头那个数**。
>
> 如果这个数是连续序列开头的数，则从这个数开始进行遍历查找后面的所有的数并计算连续的长度。
>
> ```
> 其实自己的代码思路差不多，但是在实现上，这个的效果确实会好点，少了更多没必要的判断。
> 
> 自己在写代码的时候很喜欢用if来判断当前状态，根据不同状态给予不同处理，但是这样每次遍历的会经过多个实际上没用的判断，导致浪费时间。
> 
> 比如想想自己的这段代码能怎么优化：
> 首先是Map的使用，其记录了所有的occ的元素的最长连续字符串的值，这实际上是没必要的，毕竟结果需要的只是最长的字符串，因此使用一个maxLength就可以。
> 
> 按照这个思路去优化的话，自己的代码的判断每个字符的最长连续长度的时候，使用的是倒序遍历，比如对于
> occ = 1、2、3、4、5、6、7、8、9。
> 当occ处理到元素5的时候，回倒序查找前面的1、2、3、4，然后得出自己的连续字符长度也就是5，
> 但是到元素6的时候，也会倒叙查找前面的1、2、3、4、5，得到自己的连续字符长度为6；
> 这样显然出现了重复的计算，这是没必要的，因此自己在实现代码的时候引入了一个map记录了每个字符的连续长度数据，这样6在往前查找的时候，会先查找map中的数据，然后找到5在map中记录的数据为5，因此6直接读取5的value值，并在其的基础上+1，也就得到了6。这样貌似优化了判断。
> 
> 但是问题还是，多浪费了空间，并且多了更多的不必要的判断。
> 
> 因此按照官方题解的方式，不是往前进行dfs，而是判断字符是否是连续序列的开头数，如果不是的话，则跳过该数，只对连续序列开头的数进行计算，这样可以减少计算量。
> ```
>
> 以后在思考解决方式的时候，可以考虑**能否将遍历的方式反转**。比如正序的遍历变成反向的遍历，有时候就能实现更快的计算效果。

## 16	2024-05-01

### （1）雇佣K位工人的总代价

**个人题解**：

```java

class Solution {
    public long totalCost(int[] costs, int k, int candidates) {
        List<Integer> list = new ArrayList<>();
        List<Integer> sublist = new ArrayList<>();
        List<Integer> headList = new ArrayList<>();
        List<Integer> tailList = new ArrayList<>();
        long sum  = 0;
        for (int i : costs){
            list.add(i);
        }
        if (list.size() > 2 * candidates){
            for (int i = 0; i < candidates; i++){
                headList.add(list.get(i));
                tailList.add(list.get(list.size() - 1 - i));
            }
            sublist = list.subList(candidates, list.size() - candidates);
            Collections.sort(headList);
            Collections.sort(tailList);
        }else {
            Collections.sort(list);
            for (int i = 0; i < k; i++){
                sum += list.get(i);
            }
            return sum;
        }
        while (k > 0 && sublist.size() > 0){
            int newData;
            if(headList.get(0) <= tailList.get(0)){
                sum += headList.get(0);
                headList.remove(0);
                newData = sublist.remove(0);
                int insertIndex = Collections.binarySearch(headList, newData);
                if (insertIndex < 0) {
                    insertIndex = -(insertIndex + 1); // 将负值转换为插入点
                }
                headList.add(insertIndex, newData);
            }else {
                sum += tailList.get(0);
                tailList.remove(0);
                newData = sublist.remove(sublist.size() - 1);
                int insertIndex = Collections.binarySearch(tailList, newData);
                if (insertIndex < 0) {
                    insertIndex = -(insertIndex + 1); // 将负值转换为插入点
                }
                tailList.add(insertIndex, newData);
            }
            k--;
        }
        if (k > 0){
            sublist.clear();
            sublist.addAll(headList);
            sublist.addAll(tailList);
            Collections.sort(sublist);
            for (int i = 0; i < k; i++){
                sum += sublist.get(i);
            }
        }
        return sum;
    }
}
```

> **核心思路**：
>
> 第一次的时候写了一种超时的方法。仔细想想确实存在大量没必要的计算内容。
>
> 另外题目中的candidates的意思代表是前candidates位人中最小的以及末尾的后candidates位人中最小的进行比较，两者之间的最小的才是题目的候选人。不是比较前后各自的第candidates位人。
>
> 以下是通过的核心思路：
>
> 1. ```
>    首先将costs的值存入list中，方便后续的操作。
>            这里为什么选择list呢？分析一下需求：
>            （1）需要能查找指定位置的数据；【排除Set】
>            （2）需要删除的时候，其他的数据能动态的变化位置。【排除Map】
>    ```
>
> 2. ```
>    将处理分为两种情况，判断标准是：list的长度是否>=candidates*2；
>                                                                                                                                           
>    （1）：如果list.size() < candidates*2；那么直接将list内的内容全部进行排序，然后从小到大，将前k个数的和直接返回就是结果了。
>    		为什么会有这样一个效果呢？这也是这个题比较容易难理解的地方，因为一旦list.size() < candidates*2；也就是前后的candidates加起来的范围已经覆盖了list的全部内容。而题目所求的是前后candidates中的最小的，在两者已经覆盖了所有的list的情况下，直接找最小的值也就是每次的目的值。
>    		                                                                                                                                       
>    （2）：如果list.size() 并没有符合上述条件。
>    		那么构造三个新的list，分别是headList与tailList；
>    		headList记录的是list的前candidates个数；
>    		tailList记录的是list的后candidates个数；
>    		第三个是除开headList与tailList之外的剩下的list的内容，记为sublist，
>    
>    
>    		将headList与tailList进行排序。每次比较headList与tailList的头部值，选较小的作为目的值，然后根据是headList还是tailList，从subList的头部或者尾部将数据塞入headList或者tailList，塞入的时候仍然要保证是排序的。		
>    ```
>
> 3. ```
>    最后当subList.size() == 0，或者k的值==0，就可以中断了。
>    
>    如果k=0的时候，结果也就出来了。
>    
>    如果是因为subList.size == 0，
>    那么需要重新将headList与tailList重新合并再排序，找到剩下的前k个数加到sum中。
>    ```
>
> 4. ```
>    可以优化的有哪些地方？
>                                                                                                                                           
>    （1）时间复杂度：
>    	第3步中，将headList与tailList合并，这里是两个已经排序的list重新排序，有办法提升合并排序的速度。
>    ```

**官方题解**：

```java
class Solution {
    public long totalCost(int[] costs, int k, int candidates) {
        int n = costs.length;
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>((a, b) -> a[0] != b[0] ? a[0] - b[0] : a[1] - b[1]);
        int left = candidates - 1, right = n - candidates;
        if (left + 1 < right) {
            for (int i = 0; i <= left; ++i) {
                pq.offer(new int[]{costs[i], i});
            }
            for (int i = right; i < n; ++i) {
                pq.offer(new int[]{costs[i], i});
            }
        } else {
            for (int i = 0; i < n; ++i) {
                pq.offer(new int[]{costs[i], i});
            }
        }
        long ans = 0;
        for (int i = 0; i < k; ++i) {
            int[] arr = pq.poll();
            int cost = arr[0], id = arr[1];
            ans += cost;
            if (left + 1 < right) {
                if (id <= left) {
                    ++left;
                    pq.offer(new int[]{costs[left], left});
                } else {
                    --right;
                    pq.offer(new int[]{costs[right], right});
                }
            }
        }
        return ans;
    }
}
```

> **核心思路**：
>
> 从实现的算法思路上是一样的， 但是官方题解的算法中，使用了一个更优秀的方式——**小根堆**。
>
> 用小根堆的方式来代替了自己实现中的headList与tailList的实现方式，能有更快的速度。
>
> 1. ```java
>    //初始化一个小根堆代码
>            PriorityQueue<int[]> pq = new PriorityQueue<int[]>((a, b) -> a[0] != b[0] ? a[0] - b[0] : a[1] - b[1]);
>    		//对于(a, b) -> a[0] != b[0] ? a[0] - b[0] : a[1] - b[1]如何理解？
>    		//其实就是先判断输入的int[]数组的[0]是否相等，如果不相等的话，即通过a[0] - b[0]进行升序存放；
>    		//如果相等的话，就通过a[1] - b[1]进行升序排放。
>    		//[0]内的元素代表的是cost的值，[1]内的元素代表的是序号；
>    		//依据题目中的理解，也就是首先根据值的判断先后顺序，如果无法根据值判断优先顺序【也就是值相同】，那就根据序号来进行判断先后顺序。
>                                                                                                                                           
>    		if (left + 1 < right) {
>                for (int i = 0; i <= left; ++i) {
>                    //将前边的元素加入小根堆中。
>                    pq.offer(new int[]{costs[i], i});
>                }
>                for (int i = right; i < n; ++i) {
>                    //将后边的元素加入小根堆中。
>                    pq.offer(new int[]{costs[i], i});
>                }
>            } else {
>                for (int i = 0; i < n; ++i) {
>                    pq.offer(new int[]{costs[i], i});
>                }
>            }
>    //构建小根堆核心代码如上，
>    ```
>
>    ![image-20240501172129271](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240501172129271.png)
>
> 2. ```java
>            for (int i = 0; i < k; ++i) {
>                int[] arr = pq.poll();
>                int cost = arr[0], id = arr[1];
>                ans += cost;
>                //如果左边已经与右边接在了一起，则只需要对pq不断的取元素就行。
>                //与自己的代码相联系比较的话，就是subList已经空了，无法再添加元素。
>                if (left + 1 < right) {
>                    if (id <= left) {
>                        ++left;
>                        pq.offer(new int[]{costs[left], left});
>                    } else {
>                        --right;
>                        pq.offer(new int[]{costs[right], right});
>                    }
>                }
>            }
>    ```

## 17	2024-05-02

### （1）雇佣K名工人的最低成本——857

> 困难直接跳过了

### （2）盛水最多的容器——11

**个人题解**：

```java
class Solution {
    public int maxArea(int[] height) {
        int head = 0;
        int tail = height.length - 1;
        int maxCapacity = Math.min(height[head], height[tail]) * (tail - head);
        while (head < tail){
            int tempHeight;
            int tempCapacity;

            tempHeight = Math.min(height[head], height[tail]);
            tempCapacity = (tail - head) * tempHeight;
            maxCapacity = Math.max(tempCapacity, maxCapacity);

            if (height[head] < height[tail]){
                head++;
            }else {
                tail--;
            }
        }
        System.out.println();
        return maxCapacity;
    }
}
```

> **核心思路**：
>
> 1. ```
>    想了比较久也是想到了在头尾各放置一个指针，然后往内移动。
>    但是在向内移动的时候，移动头还是尾并没有什么思路。
>                                                                                                                                           
>    考虑过比较移动头和尾各自移动后的盛水量的变化，但是并不觉得是个合理的方式。
>    ```
>
> 2. ```
>    后来看了看提示，说是移动头尾中，值较小的一方。
>                                                                                                                                           
>    好像有点道理，但是并不能理解其中的数学关系。
>                                                                                                                                           
>    不过也没啥好的思路，索性编写一次看看结果。
>                                                                                                                                           
>    没想到就过了。
>    ```
>
> 3. ```
>    算是一个比较偏向数学的题目。
>    ```

**官方题解**：

```java
public class Solution {
    public int maxArea(int[] height) {
        int l = 0, r = height.length - 1;
        int ans = 0;
        while (l < r) {
            int area = Math.min(height[l], height[r]) * (r - l);
            ans = Math.max(ans, area);
            if (height[l] <= height[r]) {
                ++l;
            }
            else {
                --r;
            }
        }
        return ans;
    }
}
```

> **核心思路**：
>
> 思路确实是这样，并且解释了一下为什么需要移动其中比较小的一方的疑惑。
>
> ```
> 如果我们移动数字较大的那个指针，那么前者「两个指针指向的数字中较小值」不会增加，后者「指针之间的距离」会减小，那么这个乘积会减小。因此，我们移动数字较大的那个指针是不合理的。因此，我们移动 数字较小的那个指针。
> ```
>
> 另外也解释了为什么每次移动较小的那一方的指针最后总会找到最大的结果。
>
> 思考这样一个事实：
>
> 在最开始的时候，head和tail指针，假设head > tail，那么最开始的时候容量就是(tail - head) * tail。
>
> ```
> 如果我们移动较高的指针，也就是将head往右移动，那么无论怎么移动，即便内部的height再高，也不可能使得结果超过(tail - head) * tail的值，里面的值永远都是小于这个值的。
> ```
>
> ```
> 但是我们如果移动较矮的指针，则可能会出现更高的值。
> 当我们移动了较低的指针之后，那么出现的状况就是一个新的、规模小了1的新情况了。
> 这个时候再一次当作新的开始处理就行。
> ```

## 18	2024-05-03

### （1）去掉最低工资和最高工资后的工资平均值——1491

**个人题解**：

```java
class Solution {
    public double average(int[] salary) {
        int max = Integer.MIN_VALUE;
        int min = Integer.MAX_VALUE;
        double sum = 0;
        for (int item : salary){
            max = Math.max(item, max);
            min = Math.min(item, min);
            sum += item;
        }
        sum = sum - max - min;
        sum /= salary.length - 2;
        return sum;
    }
}
```

> **核心思路**：
>
> 简单

**官方题解**：

```java
class Solution {
    public double average(int[] salary) {
        double sum = 0;
        double maxValue = Integer.MIN_VALUE, minValue = Integer.MAX_VALUE;
        for (int num : salary) {
            sum += num;
            maxValue = Math.max(maxValue, num);
            minValue = Math.min(minValue, num);
        }
        return (sum - maxValue - minValue) / (salary.length - 2);
    }
}
```

> 一模一样。

### （2）三数之和——15

**个人题解**：

> 无，能想到的方法全部超时了。。。。
>
> (ㄒoㄒ)(ㄒoㄒ)(ㄒoㄒ)(ㄒoㄒ)

**官方题解**：

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        int n = nums.length;
        Arrays.sort(nums);
        List<List<Integer>> ans = new ArrayList<List<Integer>>();
        // 枚举 a
        for (int first = 0; first < n; ++first) {
            // 需要和上一次枚举的数不相同
            if (first > 0 && nums[first] == nums[first - 1]) {
                continue;
            }
            // c 对应的指针初始指向数组的最右端
            int third = n - 1;
            int target = -nums[first];
            // 枚举 b
            for (int second = first + 1; second < n; ++second) {
                // 需要和上一次枚举的数不相同
                if (second > first + 1 && nums[second] == nums[second - 1]) {
                    continue;
                }
                // 需要保证 b 的指针在 c 的指针的左侧
                while (second < third && nums[second] + nums[third] > target) {
                    --third;
                }
                // 如果指针重合，随着 b 后续的增加
                // 就不会有满足 a+b+c=0 并且 b<c 的 c 了，可以退出循环
                if (second == third) {
                    break;
                }
                if (nums[second] + nums[third] == target) {
                    List<Integer> list = new ArrayList<Integer>();
                    list.add(nums[first]);
                    list.add(nums[second]);
                    list.add(nums[third]);
                    ans.add(list);
                }
            }
        }
        return ans;
    }
}
```

> **核心思路**：
>
> 1. ```
>    基本的思路是三重循环，但是需要搭配最重要的排序。
>                                                                                                                                           
>    最开始的时候就将序列进行一次排序。
>    ```
>
> 2. ```
>    排序之后，依次遍历所有的数据【第一重循环】
>            for (int first = 0; first < n; ++first) {
>    		       ......
>            }
>    在遍历中，目的是寻找合为target = -nums[first]的nums[second]与nums[third]。
>                                                                                                                                                   
>    在每次循环中，设置两个指针，分别是second与third;
>    其中second = first + 1;
>    third = nums.length - 1;
>                                                                                                                                           
>    然后采取的方法是夹逼，然first与third不断的收缩，最后找到目的值。
>    ```
>
> 3. ```
>    由于已经进行过排序，所以有这样一个好处，当first、second、third移动的时候，如果遇到相同的字符，可以再一次移动；【这样也可以避免重复的字符重复添加的问题】
>           if (first > 0 && nums[first] == nums[first - 1]) {
>                    continue;
>           }
>                                                                                                                                                  
>           if (second > first + 1 && nums[second] == nums[second - 1]) {
>                    continue;
>           }
>    【其实third也可以进行跳过的，但是官方题解中并没有关于tirhd的跳过】
>    ```

**更优化的版本**：

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> ans = new ArrayList<>();
        int n = nums.length;
        for (int i = 0; i < n - 2; ++i) {
            int x = nums[i];
            if (i > 0 && x == nums[i - 1]) continue; // 跳过重复数字
            if (x + nums[i + 1] + nums[i + 2] > 0) break; // 优化一
            if (x + nums[n - 2] + nums[n - 1] < 0) continue; // 优化二
            int j = i + 1, k = n - 1;
            while (j < k) {
                int s = x + nums[j] + nums[k];
                if (s > 0) --k;
                else if (s < 0) ++j;
                else {
                    ans.add(List.of(x, nums[j], nums[k]));
                    for (++j; j < k && nums[j] == nums[j - 1]; ++j); // 跳过重复数字
                    for (--k; k > j && nums[k] == nums[k + 1]; --k); // 跳过重复数字
                }
            }
        }
        return ans;
    }
}
```

> **核心思路**：
>
> 核心思路与官方题解的方法一致，但是给出了多处进行优化的方式。
>
> 1. ```java
>    if (x + nums[i + 1] + nums[i + 2] > 0) break; // 优化一
>                                                                                                                                           
>    if (x + nums[n - 2] + nums[n - 1] < 0) continue; // 优化二
>                                                                                                                                           
>    //这两个优化的效果非常好。
>    //因为nums已经进行了排序，对于下面这样一个队列：
>    //比如当序列进行到999,1000，1001......100000......这种部分的时候，实际上已经完全没必要继续找second与third了，因为后面都只会增加，其和只会越来越大，距离0越来越远，因此可以直接break；
>                                                                                                                                           
>    //同理的对于优化二，当序列是下面这种
>    //-99999，-99998，-1，0，1，2，3，4，5，6，7....100的时候
>    //将-99999加上末尾最大的两个值也还是小于0，前面任意的遍历也是没必要的，因此可以直接continue，判断下一个元素。
>    ```
>
> 2. ```java
>    for (--k; k > j && nums[k] == nums[k + 1]; --k); // 跳过重复数字
>    //对third也进行了重复跳过优化。
>    ```

## 19	2024-05-04

### （1）规划兼职工作——1235

> 困难，跳过

### （2）找到字符串中所有字母异位词——438

**个人题解**：

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        int targetLength = p.length();
        int [] targetLettersCounts = new int [26];
        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < p.length(); i++){
            targetLettersCounts[p.charAt(i) - 'a']++;
        }
        for (int i = 0; i < s.length() + 1 - targetLength; i++){
            String subString = s.substring(i, i + targetLength);
            int [] lettersCounts = new int [26];
            for (int j = 0; j < subString.length(); j++){
                lettersCounts[subString.charAt(j) - 'a']++;
            }
            if (Arrays.equals(targetLettersCounts, lettersCounts)){
                result.add(i);
            }

        }
        return result;
    }
}
```

> **核心思路**：
>
> 对目标字串p，记录其26个字符的数量。
>
> 遍历s，对每个与p同等长度的子串，判断其26个字符数量，如果相同，则代表为异位词。

**官方题解**：

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        int sLen = s.length(), pLen = p.length();

        if (sLen < pLen) {
            return new ArrayList<Integer>();
        }

        List<Integer> ans = new ArrayList<Integer>();
        int[] sCount = new int[26];
        int[] pCount = new int[26];
        for (int i = 0; i < pLen; ++i) {
            ++sCount[s.charAt(i) - 'a'];
            ++pCount[p.charAt(i) - 'a'];
        }

        if (Arrays.equals(sCount, pCount)) {
            ans.add(0);
        }

        for (int i = 0; i < sLen - pLen; ++i) {
            --sCount[s.charAt(i) - 'a'];
            ++sCount[s.charAt(i + pLen) - 'a'];

            if (Arrays.equals(sCount, pCount)) {
                ans.add(i + 1);
            }
        }

        return ans;
    }
}
```

> **核心思路**：
>
> 思路差不多，但是官方的思路中，优化了对s的lettersCounts[]的计数方式。
>
> 在自己的实现中，对于每个s的子串，都用了一次for遍历求计数；
>
> 但实际这是可以优化的，采用滑动窗口的设计，每次都移除和新加入的字符都改动一次lettersCounts，这样可以减少很多的重复计算量。

**官方题解——更一步的优化**：

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        int sLen = s.length(), pLen = p.length();

        if (sLen < pLen) {
            return new ArrayList<Integer>();
        }

        List<Integer> ans = new ArrayList<Integer>();
        int[] count = new int[26];
        for (int i = 0; i < pLen; ++i) {
            ++count[s.charAt(i) - 'a'];
            --count[p.charAt(i) - 'a'];
        }

        int differ = 0;
        for (int j = 0; j < 26; ++j) {
            if (count[j] != 0) {
                ++differ;
            }
        }

        if (differ == 0) {
            ans.add(0);
        }

        for (int i = 0; i < sLen - pLen; ++i) {
            if (count[s.charAt(i) - 'a'] == 1) {  // 窗口中字母 s[i] 的数量与字符串 p 中的数量从不同变得相同
                --differ;
            } else if (count[s.charAt(i) - 'a'] == 0) {  // 窗口中字母 s[i] 的数量与字符串 p 中的数量从相同变得不同
                ++differ;
            }
            --count[s.charAt(i) - 'a'];

            if (count[s.charAt(i + pLen) - 'a'] == -1) {  // 窗口中字母 s[i+pLen] 的数量与字符串 p 中的数量从不同变得相同
                --differ;
            } else if (count[s.charAt(i + pLen) - 'a'] == 0) {  // 窗口中字母 s[i+pLen] 的数量与字符串 p 中的数量从相同变得不同
                ++differ;
            }
            ++count[s.charAt(i + pLen) - 'a'];
            
            if (differ == 0) {
                ans.add(i + 1);
            }
        }

        return ans;
    }
}
```

> 核心优化：选择

## 20	2024-05-05

### （1）拆炸弹——1652

**个人题解**：

```java
class Solution {
    public int[] decrypt(int[] code, int k) {
        int[] result = new int[code.length];
        int postSum = 0;
        int preSum = 0;
        if (k == 0){
        }else if (k > 0){
            for (int i = 0; i < k; i ++){
                postSum += getValue(code, i + 1);
            }
            for (int i = 0; i < code.length; i++){
                result[i] = postSum;
                postSum -= getValue(code, i + 1);
                postSum += getValue(code, i + 1 + k);
            }
        }else {
            k = -k;
            for (int i = k; i > 0; i --){
                preSum += getValue(code, -i);
            }
            for (int i = 0; i < code.length; i++){
                result[i] = preSum;
                preSum -= getValue(code, -k + i);
                preSum += getValue(code, i);
            }
        }
        return result;
    }
    public int getValue(int [] code, int i){
        while (i >= code.length){
            i -= code.length;
        }
        while (i < 0){
            i += code.length;
        }
        return code[i];
    }
}
```

> **核心思路**：
>
> 笑死，以为写的特别烂，没想到是从未有过如此美妙的成绩。
>
> 但是其实存在较多的问题，应该使用取模的方式来获得数据指定位置的数的，而不是通过一个while循环。
>
> <img src="https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240505195042515.png" alt="image-20240505195042515" style="zoom:67%;" />
>
> 构造一个用于获得数组指定位置的数的函数，要求当位置超出了数组的范围的时候，需要能回归到数组的指定位置。

**官方题解**：

```java
class Solution {
    public int[] decrypt(int[] code, int k) {
        int n = code.length;
        if (k == 0) {
            return new int[n];
        }
        int[] res = new int[n];
        int[] newCode = new int[n * 2];
        System.arraycopy(code, 0, newCode, 0, n);
        System.arraycopy(code, 0, newCode, n, n);
        code = newCode;
        int l = k > 0 ? 1 : n + k;
        int r = k > 0 ? k : n - 1;
        int w = 0;
        for (int i = l; i <= r; i++) {
            w += code[i];
        }
        for (int i = 0; i < n; i++) {
            res[i] = w;
            w -= code[l];
            w += code[r + 1];
            l++;
            r++;
        }
        return res;
    }
}
```

> **核心思路**：
>
> 采用的是拼接的方式，将两个数组相接，但是这个应该存在问题。
>
> 如果k的长度比数组的长度的两倍都还要长，那就会出问题了，那得拼接多个数组才行，非常的浪费空间。
>
> 比较好的实现方法是采用取模的方式：
>
> ![image-20240505200444944](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240505200444944.png)
>
> 但是实际上java中的取模的实现方式目前还有点犹豫。

## 21	2024-05-06

### （1）摘樱桃——741

> 困难，跳过

### （2）和为K的子数组——560

**个人题解**：

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int resultCounts = 0;
        for (int i = 0; i < nums.length; i++) {
            int currentSum = 0;
            for (int j = i; j < nums.length; j++) {
                currentSum += nums[j];
                if (currentSum == k) {
                    resultCounts++;
                }
            }
        }
        return resultCounts;
    }
}
```

> **核心思路**：
>
> 1. ```
>    最开始，且错误的思路是，使用双指针，根据每次新加入值后的大小判断，如果超出，则下次舍弃head处的指针的数【head++】，如果小于，则将tail处的指针++。
>                                                                                                                                           
>    后来发现数组中的数不只是正数，还有可能有负数，这意味着新加入数也可能让整体的和变小，这个思路就行不通了。
>    ```
>
> 2. ```java
>    //第二次的想法是构造一个Map结构。
>            Map<Integer, Map<Integer, List<int[]>>> map = new HashMap<>();
>            for (int i = 0; i < nums.length; i++){
>                Map<Integer, List<int[]>> currentMap = new HashMap<>();
>                int currentSum = 0;
>                for (int j = i; j < nums.length; j++){
>                    currentSum += nums[j];
>                    if (currentMap.containsKey(currentSum)){
>                        currentMap.get(Integer.valueOf(currentSum)).add(new int [] {i, j});
>                    }else {
>                        List<int[]> tempList = new ArrayList<>();
>                        tempList.add(new int[] {i, j});
>                        currentMap.put(currentSum, tempList);
>                    }
>                }
>                map.put(i, currentMap);
>            }
>    //在这个Map结构如何理解呢？
>    //第一层映射代表的是数组序列与二层map的映射关系；
>    //第二层map的映射是和值与位置的映射关系。
>    //见图一中，第一层的映射关系
>    ```
>
>    图一：第一层映射关系
>
>    key是数组的索引号，value是一个map，map是第二层的映射关系。
>
>    ![image-20240506144504259](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240506144504259.png)
>
>    图二：第二层映射关系
>
>    key代表的是和值，value是一个LIst，记录的是和值为key的起终点。
>
>    ①指的是数组索引号中为0的，其所有和值有两种可能，分别是-1、-2
>
>    ②指的是其中和值为-1的，有两种累积方法，分别是从0~0【也就是0处本身就是-1】，另一种方法是从0~2，也就是nums[0]+nums[1]+nums[2]。
>
>    ③指的是其中和值为-2的，有一种累计方法，从0~1。
>
>    【对nums为0的索引号分析完毕，图中后续分别是索引号1、2的map映射关系】
>
>    ![image-20240506144828104](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240506144828104.png)
>
> 3. ```
>    思考了一下这个实现的算法复杂度，时间复杂度O（nlogn），但是还是用了两层for。既然都用了两层for了，为什么还需要构建这样一个结构呢。
>                                                                                                                                           
>    然后就索性去掉了这些存储的结构，直接判断是否满足条件，于是有了个人题解中的代码。
>                                                                                                                                           
>    个人题解中，相当于是对nums的每个数往后进行一次累积和，如果存在合理的累积和，则使得最后的计数++。
>    ```

**官方题解——枚举**：

```java
public class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        for (int start = 0; start < nums.length; ++start) {
            int sum = 0;
            for (int end = start; end >= 0; --end) {
                sum += nums[end];
                if (sum == k) {
                    count++;
                }
            }
        }
        return count;
    }
}
```

> 核心思路一模一样。

**官方题解——前缀和+哈希表优化**：

```java
public class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0, pre = 0;
        HashMap < Integer, Integer > mp = new HashMap < > ();
        mp.put(0, 1);
        for (int i = 0; i < nums.length; i++) {
            pre += nums[i];
            if (mp.containsKey(pre - k)) {
                count += mp.get(pre - k);
            }
            mp.put(pre, mp.getOrDefault(pre, 0) + 1);
        }
        return count;
    }
}
```

> **核心思路**：
>
> 使用Map优化了重复的计算。
>
> 在简单的实现中，存在较多的重复的计算，可以通过记录一个和值Map来简化计算。
>
> 题解中，Map的Key是和值，Value值是出现的次数。
>
> 当遍历到当前的位置时，只需要判断是否存在 "当前的所有和 - k"的和值，如果有，那么两者之间的和值就是k。【因为 pre - (pre - k ) === k】。

## 22 2024-05-07

### （1）摘樱桃——1463

> 困难，跳过

### （2）最大子数组和——53

**个人题解**：

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int currentSum = 0;
        List<Integer> sumList = new ArrayList<>();
        sumList.add(0);
        int currentSumListMin = 0;
        int result = nums[0];
        for (int i = 0; i < nums.length; i++) {
            currentSum += nums[i];
            result = Math.max(result, currentSum - currentSumListMin);
            sumList.add(currentSum);
            currentSumListMin = Math.min(currentSumListMin, currentSum);
        }
        return result;
    }
}

//优化版
class Solution {
    public int maxSubArray(int[] nums) {
        int currentSum = 0;
        int currentSumListMin = 0;
        int result = nums[0];
        for (int i = 0; i < nums.length; i++) {
            currentSum += nums[i];
            result = Math.max(result, currentSum - currentSumListMin);
            currentSumListMin = Math.min(currentSumListMin, currentSum);
        }
        return result;
    }
}
```

> **核心思路**：
>
> 1. ```java
>    与和为K的子数组——560比较类似。
>                                                                                                                                           
>    第一次思考的时候，核心思路是：在累计和的序列中找到正向差值最大的两个数，并且需要以后边的数减去前边的数的形式。
>    【白话就是在sumList中，用一个后边的数减去一个前边的数，值最接近+∞这个值就是结果。】
>                                                                                                                                           
>    最开始的时候，通过构造一个sumList，存储所有的前边的和值，每次计算到当前的和值currentSum之后，找到位于其前边的sunList中的最小的值，然后计算currentSum-那个最小的值，然后用result来记录这个减值最大的数。
>    ```
>
> 2. ```java
>    优化版中发现，其实根本没必要维护一个sumList，直接记录一个最小的值就行，这样可以减去计算时间与存储空间。
>    ```
>
>    优化版的提交效果还是很不错的：
>
>    ![image-20240507143511720](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240507143511720.png)

**官方题解——动态规划**：

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int pre = 0, maxAns = nums[0];
        for (int x : nums) {
            pre = Math.max(pre + x, x);
            maxAns = Math.max(maxAns, pre);
        }
        return maxAns;
    }
}
```

> **核心思路**：
>
> 1. ```java
>    //核心在于
>    pre = Math.max(pre + x, x);
>                                                                                                                                           
>    //如何理解？
>    //如果前边与自己累加后还不如自己本身大，那就把前边的都扔掉，从此自己本身重新开始累加。
>    ```

**官方题解——分治**：

```java
class Solution {
    public class Status {
        public int lSum, rSum, mSum, iSum;

        public Status(int lSum, int rSum, int mSum, int iSum) {
            this.lSum = lSum;
            this.rSum = rSum;
            this.mSum = mSum;
            this.iSum = iSum;
        }
    }

    public int maxSubArray(int[] nums) {
        return getInfo(nums, 0, nums.length - 1).mSum;
    }

    public Status getInfo(int[] a, int l, int r) {
        if (l == r) {
            return new Status(a[l], a[l], a[l], a[l]);
        }
        int m = (l + r) >> 1;
        Status lSub = getInfo(a, l, m);
        Status rSub = getInfo(a, m + 1, r);
        return pushUp(lSub, rSub);
    }

    public Status pushUp(Status l, Status r) {
        int iSum = l.iSum + r.iSum;
        int lSum = Math.max(l.lSum, l.iSum + r.lSum);
        int rSum = Math.max(r.rSum, r.iSum + l.rSum);
        int mSum = Math.max(Math.max(l.mSum, r.mSum), l.rSum + r.lSum);
        return new Status(lSum, rSum, mSum, iSum);
    }
}
```

> 更加复杂，但是有其应用场景，参考
>
> **https://leetcode.cn/problems/maximum-subarray/solutions/228009/zui-da-zi-xu-he-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked** 

## 23	2024-05-08

### （1）给植物浇水——2079

**个人题解**：

```java
class Solution {
    public int wateringPlants(int[] plants, int capacity) {
        int n = plants.length;
        int currentCapacity = capacity;
        int waterLocation = 0;
        int stepCounts = 0;
        while (waterLocation < n){
            if (plants[waterLocation] <= currentCapacity){
                currentCapacity -= plants[waterLocation];
                waterLocation ++;
                stepCounts ++;
            }else {
                currentCapacity = capacity;
                stepCounts += 2 * waterLocation ;
            }
        }
        return stepCounts;
    }
}
```

> **核心思路**：
>
> 模拟。
>
> 模拟运行的状态即可，debug一下解决模拟的参数就行。

**官方题解**：

```java
class Solution {
    public int wateringPlants(int[] plants, int capacity) {
        int n = plants.length;
        int ans = 0;
        int rest = capacity;
        for (int i = 0; i < n; ++i) {
            if (rest >= plants[i]) {
                ++ans;
                rest -= plants[i];
            } else {
                ans += i * 2 + 1;
                rest = capacity - plants[i];
            }
        }
        return ans;
    }
}
```

> **核心思路**：
>
> 一样的，不过是通过for实现的循环。
>
> waterLocation确实只是通过++的方式推进，所以通过for实现也是可以的。

### （2）合并区间——56

**个人题解**：

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        Map<Integer, int []> map = new TreeMap<>();
        int result [][];

        //将数插入map中
        for (int[] item : intervals){
            if (map.containsKey(item[0])){
                map.put(item[0], new int [] {item[0], Math.max(item[1], map.get(item[0])[1])});
                continue;
            }
            map.put(item[0], item);
        }

        // 使用 entrySet() 方法获取键值对的集合，然后通过迭代器进行遍历
        Iterator<Map.Entry<Integer, int[]>> iterator =
                map.entrySet().iterator();
        Map.Entry<Integer, int[]> entryCurrent = null;

        while (iterator.hasNext()) {
            Map.Entry<Integer, int[]> entryNext = iterator.next();
            if (entryCurrent != null &&
                    entryCurrent.getValue()[1] >= entryNext.getValue()[0]) {
                    entryCurrent.setValue(new int[] {
                            entryCurrent.getValue()[0],
                            Math.max(entryCurrent.getValue()[1],
                                    entryNext.getValue()[1])
                    });
                    iterator.remove();
                    continue;
            }
            entryCurrent = entryNext;
        }
//        将map中的值转入到result中
        int index = 0;
        result = new int[map.size()][];
        for (int [] value : map.values()){
            result[index] = value;
            index ++;
        }
        return result;
    }
}
```

> **核心思路**：
>
> 构造一个TreeMap，通过遍历TreeMap的数来实现合并。
>
> 1. ```java
>            //将数插入map中
>            for (int[] item : intervals){
>                //插入的时候，如果重复的内容插入，那么更新map中的内容为，区间尾较大的区间
>                if (map.containsKey(item[0])){
>                    map.put(item[0], new int [] {item[0], Math.max(item[1], map.get(item[0])[1])});
>                    continue;
>                }
>                map.put(item[0], item);
>            }
>    ```
>
> 2. ```java
>            // 使用 entrySet() 方法获取键值对的集合，然后通过迭代器进行遍历
>            Iterator<Map.Entry<Integer, int[]>> iterator = 
>                    map.entrySet().iterator();
>            Map.Entry<Integer, int[]> entryCurrent = null;
>                                                                                                                                        
>            while (iterator.hasNext()) {
>                //下一个位置的指向器
>                Map.Entry<Integer, int[]> entryNext = iterator.next();
>                //如果当前的指针不为空【应该只在第一个数据的时候有效】
>                //并且当前指针的value中，尾部的值大于下一个指针的头部值，那么就可以进行合并
>                if (entryCurrent != null &&
>                        entryCurrent.getValue()[1] >= entryNext.getValue()[0]) {
>                        entryCurrent.setValue(new int[] {
>                                entryCurrent.getValue()[0],
>                            //合并的时候需要注意，需要将合并后的尾部设置为当前与下一个的指针的value值中，tail值较大的那个。
>                                Math.max(entryCurrent.getValue()[1],
>                                        entryNext.getValue()[1])
>                        });
>                    //移除下一个位置的指向
>                        iterator.remove();
>                        continue;
>                }
>                //将当前指向的指向器指定为下一个
>                entryCurrent = entryNext;
>            }
>    ```

**官方题解**：

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if (intervals.length == 0) {
            return new int[0][2];
        }
        Arrays.sort(intervals, new Comparator<int[]>() {
            public int compare(int[] interval1, int[] interval2) {
                return interval1[0] - interval2[0];
            }
        });
        List<int[]> merged = new ArrayList<int[]>();
        for (int i = 0; i < intervals.length; ++i) {
            int L = intervals[i][0], R = intervals[i][1];
            if (merged.size() == 0 || merged.get(merged.size() - 1)[1] < L) {
                merged.add(new int[]{L, R});
            } else {
                merged.get(merged.size() - 1)[1] = Math.max(merged.get(merged.size() - 1)[1], R);
            }
        }
        return merged.toArray(new int[merged.size()][]);
    }
}
```

> **核心思路**：
>
> ```
> 我们用数组 merged 存储最终的答案。
> 
> 首先，我们将列表中的区间按照左端点升序排序。然后我们将第一个区间加入 merged 数组中，并按顺序依次考虑之后的每个区间：
> 
> 如果当前区间的左端点在数组 merged 中最后一个区间的右端点之后，那么它们不会重合，我们可以直接将这个区间加入数组 merged 的末尾；
> 
> 否则，它们重合，我们需要用当前区间的右端点更新数组 merged 中最后一个区间的右端点，将其置为二者的较大值。
> ```
>
> 以上是官方给出的题解思路，和自己思考的解题逻辑是一样的。
>
> 1. ```java
>            Arrays.sort(intervals, new Comparator<int[]>() {
>                public int compare(int[] interval1, int[] interval2) {
>                    return interval1[0] - interval2[0];
>                }
>            });
>    //直接对题目的intervals数据，按照[0]处的值的大小进行了排序。
>    //Comparator类需要学习一下
>    ```
>
> 2. ```java
>            List<int[]> merged = new ArrayList<int[]>();
>            for (int i = 0; i < intervals.length; ++i) {
>                int L = intervals[i][0], R = intervals[i][1];
>                if (merged.size() == 0 || merged.get(merged.size() - 1)[1] < L) {
>                    merged.add(new int[]{L, R});
>                } else {
>                    merged.get(merged.size() - 1)[1] = Math.max(merged.get(merged.size() - 1)[1], R);
>                }
>            }
>    //使用了一个List来存储处理后的数据
>    ```
>
> 3. ```java
>            return merged.toArray(new int[merged.size()][]);
>    //最后将List数据转换成int [][]类型的数据并返回
>    //List转int [][]
>    ```

## 24	2024-05-09

### （1）给植物浇水Ⅱ——2105

**个人题解**：

```java
class Solution {
    public int minimumRefill(int[] plants, int capacityA, int capacityB) {
        int n = plants.length;
        int currentA = capacityA;
        int currentB = capacityB;
        int reFill = 0;
        for (int i = 0; i < n / 2; i++){
            if (currentA < plants[i]){
                reFill ++;
                currentA = capacityA - plants[i];
            }else {
                currentA -= plants[i];
            }
            if (currentB < plants[n - 1 - i]){
                reFill ++;
                currentB = capacityB - plants[n - 1 -i];
            }else {
                currentB -= plants[n - 1 -i];
            }
        }
        if ((n & 1) == 1 && Math.max(currentA, currentB) < plants[n / 2]) {
            reFill ++;
        }
        return reFill;
    }
}
```

> **核心思路**：
>
> 模拟，没啥思维难度。

**官方题解**：

```java
class Solution {
    public int minimumRefill(int[] plants, int capacityA, int capacityB) {
        int res = 0; // 灌满水罐次数
        int n = plants.length; 
        int posa = 0, posb = n - 1;  // 两人位置
        int vala = capacityA, valb = capacityB; // 两人剩余水量
        // 模拟相遇前的浇水过程
        while (posa < posb) {
            if (vala < plants[posa]) {
                ++res;
                vala = capacityA - plants[posa];
            } else {
                vala -= plants[posa];
            }
            ++posa;
            if (valb < plants[posb]) {
                ++res;
                valb = capacityB - plants[posb];
            } else {
                valb -= plants[posb];
            }
            --posb;
        }
        // 模拟相遇后可能的浇水过程
        if (posa == posb) {
            if (vala >= valb && vala < plants[posa]) {
                ++res;
            }
            if (vala < valb && valb < plants[posb]) {
                ++res;
            }
        }
        return res;
    }
}
```

> **核心思路**：
>
> 官方给出的解答方式也是模拟。
>
> 没太大区别。

### （2）轮转数组——189

**个人题解**：

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        //        过多的轮转没有必要
        k %= nums.length;
        int[] arrayHead = Arrays.copyOfRange(nums, n - k, n);
        int[] arrayTail = Arrays.copyOfRange(nums, 0, n - k);
        System.arraycopy(arrayHead, 0, nums, 0, arrayHead.length);
        System.arraycopy(arrayTail, 0, nums, arrayHead.length, arrayTail.length);
    }
}
```

> **核心思路**：
>
> 轮转的本质就是把数组划分，前后部分对调。
>
> 使用java中的数组复制的函数还是非常快的。
>
> ![image-20240509223702158](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240509223702158.png)

**官方题解——使用额外数组**：

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        int[] newArr = new int[n];
        for (int i = 0; i < n; ++i) {
            newArr[(i + k) % n] = nums[i];
        }
        System.arraycopy(newArr, 0, nums, 0, n);
    }
}
```

> **核心思路**：
>
> 官方只用了一次数组复制，节省了空间，但是浪费了计算空间。

**官方题解——环状替换**：

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        k = k % n;
        int count = gcd(k, n);
        for (int start = 0; start < count; ++start) {
            int current = start;
            int prev = nums[start];
            do {
                int next = (current + k) % n;
                int temp = nums[next];
                nums[next] = prev;
                prev = temp;
                current = next;
            } while (start != current);
        }
    }

    public int gcd(int x, int y) {
        return y > 0 ? gcd(y, x % y) : x;
    }
}
```

> **核心思路**：
>
> 

## 25	2024-05-10

### （1）统计已测试设备——2960

**个人题解**：

```java
class Solution {
    public int countTestedDevices(int[] batteryPercentages) {
        int counts = 0;
        for (int item : batteryPercentages){
            if (item - counts > 0){
                counts++;
            }
        }
        return counts;
    }
}
```

> 简单题，秒了。

**官方题解**：

```java
class Solution {
    public int countTestedDevices(int[] batteryPercentages) {
        int need = 0;
        for (int battery : batteryPercentages) {
            if (battery > need) {
                need++;
            }
        }
        return need;
    }
}
```

> 简直一模一样。

### （2）除自身以外数组的乘积——238

**个人题解**：

```java
无
```

> **核心思路**：
>
> 没写出来。
>
> 看了一下官方题解的提示：
>
> ```java
> class Solution {
>     public int[] productExceptSelf(int[] nums) {
>         int[] L = new int[nums.length];
>         int[] R = new int[nums.length];
>         Arrays.fill(L, 1);
>         Arrays.fill(R, 1);
>         int currentMulti = 1;
>         for (int i = 1; i < nums.length; i++){
>             currentMulti *= nums[i - 1];
>             L[i] = currentMulti;
>         }
>         currentMulti = 1;
>         for (int j = nums.length - 2; j > -1; j--){
>             currentMulti *= nums[j + 1];
>             R[j] = currentMulti;
>         }
>         for (int i = 0; i < nums.length; i++){
>             nums[i] = L[i] * R[i];
>         }
>         return nums;
>     }
> }
> ```
>
> 构造一个前缀乘积与后缀乘积数组，即可得到结果。

**官方题解**：

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int length = nums.length;

        // L 和 R 分别表示左右两侧的乘积列表
        int[] L = new int[length];
        int[] R = new int[length];

        int[] answer = new int[length];

        // L[i] 为索引 i 左侧所有元素的乘积
        // 对于索引为 '0' 的元素，因为左侧没有元素，所以 L[0] = 1
        L[0] = 1;
        for (int i = 1; i < length; i++) {
            L[i] = nums[i - 1] * L[i - 1];
        }

        // R[i] 为索引 i 右侧所有元素的乘积
        // 对于索引为 'length-1' 的元素，因为右侧没有元素，所以 R[length-1] = 1
        R[length - 1] = 1;
        for (int i = length - 2; i >= 0; i--) {
            R[i] = nums[i + 1] * R[i + 1];
        }

        // 对于索引 i，除 nums[i] 之外其余各元素的乘积就是左侧所有元素的乘积乘以右侧所有元素的乘积
        for (int i = 0; i < length; i++) {
            answer[i] = L[i] * R[i];
        }

        return answer;
    }
}
```

> **核心思路**：

**官方题解——空间复杂度优化**：

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int length = nums.length;
        int[] answer = new int[length];

        // answer[i] 表示索引 i 左侧所有元素的乘积
        // 因为索引为 '0' 的元素左侧没有元素， 所以 answer[0] = 1
        answer[0] = 1;
        for (int i = 1; i < length; i++) {
            answer[i] = nums[i - 1] * answer[i - 1];
        }

        // R 为右侧所有元素的乘积
        // 刚开始右边没有元素，所以 R = 1
        int R = 1;
        for (int i = length - 1; i >= 0; i--) {
            // 对于索引 i，左边的乘积为 answer[i]，右边的乘积为 R
            answer[i] = answer[i] * R;
            // R 需要包含右边所有的乘积，所以计算下一个结果时需要将当前值乘到 R 上
            R *= nums[i];
        }
        return answer;
    }
}
```

> 核心思路：
>
> 省去了L与R数组的构建。

## 26	2024-05-14

### （1）完成所有任务需要的最少轮数——2244

**个人题解**：

```java
class Solution {
    public int minimumRounds(int[] tasks) {
        Map<Integer, Integer> map = new HashMap<>();
        int result = 0;
        for (int item : tasks){
            if (!map.containsKey(item)){
                map.put(item, 1);
            }else {
                map.put(item, map.get(item) + 1);
            }
        }
        for (int itemSet : map.keySet()){
            int value = map.get(itemSet);
            if (value == 1){
                return -1;
            }else {
                result += (value + 2) / 3;
            }
        }
        return result;
    }
}
```

> **核心思路**：
>
> 构建一个映射表，存储key与出现次数的映射关系。

**官方题解**：

```java
class Solution {
    public int minimumRounds(int[] tasks) {
        Map<Integer, Integer> cnt = new HashMap<Integer, Integer>();
        for (int task : tasks) {
            cnt.put(task, cnt.getOrDefault(task, 0) + 1);
        }
        int res = 0;
        for (int v : cnt.values()) {
            if (v == 1) {
                return -1;
            }
            if (v % 3 == 0) {
                res += v / 3;
            } else {
                res += 1 + v / 3;
            }
        }
        return res;
    }
}
```

> **核心思路**：
>
> 和自己思路一样，但是代码更加的优化。主要体现在：
>
> ```java
>     for (int task : tasks) {
>         cnt.put(task, cnt.getOrDefault(task, 0) + 1);
>     }
> ```
>
> 而自己写的是：
>
> ```javascript
>         for (int item : tasks){
>             if (!map.containsKey(item)){
>                 map.put(item, 1);
>             }else {
>                 map.put(item, map.get(item) + 1);
>             }
>         }
> ```
>
> 其实自己的思路就是，如果map中没有这个数的话，那就加入这个数，并设置为1，但是这是可以优化的。
>
>  
>
> 另外一个可以优化的地方是：
>
> ```java
>         for (int v : cnt.values()) {
>             if (v == 1) {
>                 return -1;
>             }
>             if (v % 3 == 0) {
>                 res += v / 3;
>             } else {
>                 res += 1 + v / 3;
>             }
>         }
> ```
>
> 而自己的实现是：
>
> ```java
>     for (int itemSet : map.keySet()){
>         int value = map.get(itemSet);
>         if (value == 1){
>             return -1;
>         }else {
>             result += (value + 2) / 3;
>         }
>     }
> ```
>
> 实际上并没有必要读取key值再读取对应的value，直接对所有的value值进行遍历就行。

### （2）矩阵置0——73

**个人题解**：

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        List<Integer> xList = new ArrayList<>();
        List<Integer> yList = new ArrayList<>();
        for (int i = 0; i < matrix.length; i++){
            for (int j = 0; j < matrix[0].length; j++){
                if (matrix[i][j] == 0){
                    xList.add(i);
                    yList.add(j);
                }
            }
        }
        for (int xitem : xList){
            for (int j = 0; j < matrix[0].length; j++){
                matrix[xitem][j] = 0;
            }
        }
        for (int yitem : yList){
            for (int i = 0; i < matrix.length; i++){
                matrix[i][yitem] = 0;
            }
        }
    }
}
```

> **核心思路**：
>
> 记录下所有为0的点的x值与y值，分别记录于xList与yList中。
>
> ![image-20240514214702365](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240514214702365.png)
>
> 后面想了想，好像不该用List，用Set应该更好：
>
> ```java
> class Solution {
>     public void setZeroes(int[][] matrix) {
>         Set<Integer> xSet = new HashSet<>();
>         Set<Integer> ySet = new HashSet<>();
>         for (int i = 0; i < matrix.length; i++){
>             for (int j = 0; j < matrix[0].length; j++){
>                 if (matrix[i][j] == 0){
>                     xSet.add(i);
>                     ySet.add(j);
>                 }
>             }
>         }
>         for (int xitem : xSet){
>             for (int j = 0; j < matrix[0].length; j++){
>                 matrix[xitem][j] = 0;
>             }
>         }
>         for (int yitem : ySet){
>             for (int i = 0; i < matrix.length; i++){
>                 matrix[i][yitem] = 0;
>             }
>         }
>     }
> }
> ```
>
> ![image-20240514214814864](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240514214814864.png)
>
> 但是奇怪的是，时间复杂度出乎我意料的更高了，空间复杂度倒是没有太大的提升。

**官方题解**：

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean[] row = new boolean[m];
        boolean[] col = new boolean[n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    row[i] = col[j] = true;
                }
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (row[i] || col[j]) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
}
```

> **核心思路**：
>
> 也是标记的方式，记录下x值与y值。

**官方题解——空间复杂度优化**：

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean flagCol0 = false, flagRow0 = false;
        for (int i = 0; i < m; i++) {
            if (matrix[i][0] == 0) {
                flagCol0 = true;
            }
        }
        for (int j = 0; j < n; j++) {
            if (matrix[0][j] == 0) {
                flagRow0 = true;
            }
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = matrix[0][j] = 0;
                }
            }
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        if (flagCol0) {
            for (int i = 0; i < m; i++) {
                matrix[i][0] = 0;
            }
        }
        if (flagRow0) {
            for (int j = 0; j < n; j++) {
                matrix[0][j] = 0;
            }
        }
    }
}
```

> **核心思路**：
>
> 将第一行与第一列作为了记录数组，可以减少时间复杂度的开销。

## 27	2024-05-15

### （1）完成所有任务的最少时间——2589

**个人题解**：

```java

```

> **思路历程**：
>
> 1. 采用贪心算法？
>

## 28	2024-05-19

### （1）找出数组游戏的赢家——1535

**个人题解**：

```java
class Solution {
    public int getWinner(int[] arr, int k) {
        int maxIndex = 0;
        int minIndex = 1;
        int counts = 0;
        while (minIndex < arr.length && counts < k){
            if (arr[maxIndex] > arr[minIndex]){
                minIndex ++;
                counts ++;
            }else {
                maxIndex = minIndex;
                minIndex ++;
                counts = 1;
            }
        }
        return arr[maxIndex];
    }
}
```

> **核心思路**：
>
> 用两个指针，分别指向较大的数和较小的数；
>
> 每次比较的时候都用一个计数器记录当前战胜【大于的数】的数量。
>
> 当最小指针到末尾或者是当前的计数值已经到了k，就可以跳出循环。
>
>  
>
> 需要注意的是，第一轮下来，肯定能找到目标值。
>
> 如果一轮下来没有找到目标值，则可以直接将目标值返回为数组中最大的值。
>
> ![image-20240519132535508](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240519132535508.png)

**官方题解**：

```java
class Solution {
    public int getWinner(int[] arr, int k) {
        int prev = Math.max(arr[0], arr[1]);
        if (k == 1) {
            return prev;
        }
        int consecutive = 1;
        int maxNum = prev;
        int length = arr.length;
        for (int i = 2; i < length; i++) {
            int curr = arr[i];
            if (prev > curr) {
                consecutive++;
                if (consecutive == k) {
                    return prev;
                }
            } else {
                prev = curr;
                consecutive = 1;
            }
            maxNum = Math.max(maxNum, curr);
        }
        return maxNum;
    }
}
```

> **核心思路**：
>
> 思路差不多。

### （2）螺旋矩阵——54

**个人题解**：

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        int i = 0, j = 0;
        int xLength = matrix[0].length;
        int yLength = matrix.length;
        int xBarrier = 0, yBarrier = 0;
        int status = 0;
        if (xLength == 1){
            status = 1;
        }
        List<Integer> result = new ArrayList<>();
        while (result.size() < xLength * yLength){
            result.add(matrix[i][j]);
            switch (status){
                case 0 : {
                    j ++;
                    if (j == (xLength - xBarrier - 1)){
                        status = 1;
                    }
                    break;
                }
                case 1 : {
                    i ++;
                    if (i == (yLength - yBarrier - 1)){
                        status = 2;
                    }
                    break;
                }
                case 2 : {
                    j --;
                    if (j == xBarrier){
                        status = 3;
                        yBarrier++;
                    }
                    break;
                }
                case 3 : {
                    i --;
                    if (i == yBarrier){
                        status = 0;
                        xBarrier++;
                    }
                    break;
                }

                default:
                    break;
            }
        }
        return result;
    }
}
```

> **核心思路**：
>
> 模拟。
>
> 使用status标识状态值，具体而言：
>
> ```java
> 0代表	→
> 1代表	↓
> 2代表	←
> 3代表	↑
> ```
>
> 然后根据不同的status，对i、j指针做不同的处理。
>
> 然后使用xBarrier与yBarrier来作为每次螺旋后对边界的限制。
>
> ![image-20240519143548618](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240519143548618.png)

**官方题解**：

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> order = new ArrayList<Integer>();
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return order;
        }
        int rows = matrix.length, columns = matrix[0].length;
        int left = 0, right = columns - 1, top = 0, bottom = rows - 1;
        while (left <= right && top <= bottom) {
            for (int column = left; column <= right; column++) {
                order.add(matrix[top][column]);
            }
            for (int row = top + 1; row <= bottom; row++) {
                order.add(matrix[row][right]);
            }
            if (left < right && top < bottom) {
                for (int column = right - 1; column > left; column--) {
                    order.add(matrix[bottom][column]);
                }
                for (int row = bottom; row > top; row--) {
                    order.add(matrix[row][left]);
                }
            }
            left++;
            right--;
            top++;
            bottom--;
        }
        return order;
    }
}
```

> **核心思路**：
>
> ![image-20240519163911344](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240519163911344.png)

### （3）旋转图像——48

**个人题解**：

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        int currFloors = 0;
        int currN = n;
        while (currN > 1){
            for (int j = currFloors; j < n - currFloors - 1; j++){
                int temp = matrix[currFloors][j];
                matrix[currFloors][j] = matrix[currN - j - 1 + 2 * currFloors][currFloors];
                matrix[currN - j - 1 + 2 * currFloors][currFloors] = matrix[currN - 1 + currFloors][currN - j - 1 + 2 * currFloors];
                matrix[currN - 1 + currFloors][currN - j - 1 + 2 * currFloors] = matrix[j][currN - 1 + currFloors];
                matrix[j][currN - 1 + currFloors] = temp;
            }
            currFloors++;
            currN = n - 2 * currFloors;
        }
    }
}
```

> **核心思路**：
>
> 每次都对最外层的数据进行处理。
>
> ![image-20240519181143862](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240519181143862.png)
>
> 核心的两个变量是currFloors与currN。
>
> 其实currN可以省略，因为其值就是n - 2 * currFloors，但是为了代码的可读性，还是加了该变量。
>
> 所以核心就是currFloors，其代表的是当前进行的层，比如当currFloors=0的时候，处理的是最外层，当currFloors=1，处理的是第二层，以此类推。
>
> currN在抽象上，代表的是当前的处理的层的长度，也就是每层中处理的nxn，第一层也就是6x6，第二层就是4x4。currN也就分别等于6、4。
>
> 知道了这两数之后，算法的核心就是外层遍历每层，内部遍历的是对每层中，首行的前n-1个数。如下图所示，第一次遍历的内部遍历就是其中的绿色部分。
>
> ![image-20240519181754701](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240519181754701.png)
>
> 对绿色部分的每个遍历，都会找到其对应的其他三个数。其最关键的对应关系就是
>
> | i j  | n-j-1+2c i | n-1+c n-j-1+2c | j n-1+c |
> | ---- | ---------- | -------------- | ------- |
>
> n代表当前的层的长度；
>
> i和j不用解释；
>
> c代表的是currFloors；
>
> 下面的表格给出了对于上面的数据中，遍历到第二层与第三层的数据的时候的处理。
>
> |         |         | i j  | n-j-1+2c i | n-1+c n-j-1+2c | j n-1+c |
> | ------- | ------- | ---- | ---------- | -------------- | ------- |
> |         |         |      |            |                |         |
> | n=4 c=1 | i=1 j=1 | 11   | 41         | 44             | 14      |
> |         | i=1 j=2 | 12   | 31         | 43             | 24      |
> |         |         |      |            |                |         |
> | n=2 c=2 | i=2 j=2 | 22   | 32         | 33             | 23      |
>
> 既然找到了对应关系，剩下的就是替换了。
>
> ```java
>             for (int j = currFloors; j < n - currFloors - 1; j++){
>                 int temp = matrix[currFloors][j];
>                 matrix[currFloors][j] = matrix[currN - j - 1 + 2 * currFloors][currFloors];
>                 matrix[currN - j - 1 + 2 * currFloors][currFloors] = matrix[currN - 1 + currFloors][currN - j - 1 + 2 * currFloors];
>                 matrix[currN - 1 + currFloors][currN - j - 1 + 2 * currFloors] = matrix[j][currN - 1 + currFloors];
>                 matrix[j][currN - 1 + currFloors] = temp;
>             }
> ```

**官方题解**：

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        for (int i = 0; i < n / 2; ++i) {
            for (int j = 0; j < (n + 1) / 2; ++j) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[n - j - 1][i];
                matrix[n - j - 1][i] = matrix[n - i - 1][n - j - 1];
                matrix[n - i - 1][n - j - 1] = matrix[j][n - i - 1];
                matrix[j][n - i - 1] = temp;
            }
        }
    }
}
```

> **核心思路**：
>
> 可以看到核心思路中也是那个规律。
>
> ![image-20240519182449275](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240519182449275.png)
>
> 其他部分大差不差。

**官方题解——用翻转代替旋转**：

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        // 水平翻转
        for (int i = 0; i < n / 2; ++i) {
            for (int j = 0; j < n; ++j) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[n - i - 1][j];
                matrix[n - i - 1][j] = temp;
            }
        }
        // 主对角线翻转
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
    }
}
```

> **核心思路**：
>
> 先进行水平轴翻转，再进行主对角线翻转可以得到答案。
>
> ![image-20240519182823405](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20240519182823405.png)
>
> 当面对翻转类似的题时，可以记住这种方式：
> 对称翻转+主对角线翻转。

## 29	2024-05-24

### （1）找出最具竞争力的子序列——1673

**个人题解**：

未解决。

> **核心思路**：
>
> ```java
> class Solution {
>     public int[] mostCompetitive(int[] nums, int k) {
>         List<int []> list = new ArrayList<>();
>         int [] result = new int [k];
>         int indexResult = 0;
>         int indexList = 0;
>         int leftBorder = 0;
>         for (int i = 0; i < nums.length; i++){
>             list.add(new int []{nums[i], i});
>         }
>         Collections.sort(list, new Comparator<int[]>() {
>             @Override
>             public int compare(int[] a, int[] b) {
>                 return Integer.compare(a[0], b[0]);
>             }
>         });
>         while (k > 0){
>             int temp = list.get(indexList)[1];
>             if (temp < leftBorder){
>                 list.remove(indexList);
>                 continue;
>             }
>             if (temp < nums.length - k + 1){
>                 leftBorder = temp;
>                 result[indexResult++] = nums[temp];
>                 list.remove(indexList);
>                 k--;
>                 indexList = 0;
>                 continue;
>             }
>             indexList++;
>         }
>         return result;
>     }
> }
> ```
>
> 以上为超时解法。想不出用什么办法再来优化时间复杂度。

**官方题解**：

```java
class Solution {
    public int[] mostCompetitive(int[] nums, int k) {
        Deque<Integer> stack = new ArrayDeque<Integer>();
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && n - i + stack.size() > k && stack.peek() > nums[i]) {
                stack.pop();
            }
            stack.push(nums[i]);
        }
        int[] res = new int[k];
        while (stack.size() > k) {
            stack.pop();
        }
        for (int i = k - 1; i >= 0; i--) {
            res[i] = stack.pop();
        }
        return res;
    }
}
```

> **核心思路**：
>
> 官方的思路是单调栈。
>
> 核心代码是：
>
> ```java
>             while (!stack.isEmpty() && n - i + stack.size() > k && stack.peek() > nums[i]) {
>                 stack.pop();
>             }
> ```
>
> 每次当nums中的最新元素满足上述条件时，就把栈顶元素弹出，直到不能再弹出之后，将元素加入之中。

## 30	2024-09-11

### （1）两个线段获得的最多奖品——2555

**个人题解：**

未解决。

> 核心问题是：对一个数组，划分两个长度为k的区域，要求两个区域之内数组的数之和最大。两个范围可以重叠，但是重叠时，范围内的数只会计算一次。

**官方题解：**

```
class Solution {
    public int maximizeWin(int[] prizePositions, int k) {
        int n = prizePositions.length;
        if (k * 2 + 1 >= prizePositions[n - 1] - prizePositions[0]) {
            return n;
        }
        int ans = 0;
        int left = 0;
        int[] mx = new int[n + 1];
        for (int right = 0; right < n; right++) {
            while (prizePositions[right] - prizePositions[left] > k) {
                left++;
            }
            ans = Math.max(ans, mx[left] + right - left + 1);
            mx[right + 1] = Math.max(mx[right], right - left + 1);
        }
        return ans;
    }
}
```

> **核心思路**：
>
> 核心是DP的思想：
>
> 通过遍历第二条线段，依次找到右端不超过第二条线段左侧【1】的第一条线段中所有可能中最大的值作为每次遍历的结果。从所有遍历的结果中选最大的作为最终结果【2】。
>
> 【1】：这里是出于最大和的目的；当两条线段重叠时，肯定比不重叠的情况下小。而有些结果中两个长度为k的区域重叠的情况，实际上就是一个长度为k的区域加上另外一个长度小于k的区域的情况。所以最终就理解为：长度为k的第二条线段，与第二条线段左侧，长度在0~k变化的第一条线段。
> 另外，重叠的发生情况，也只在当第二条线段占用了过多的区域，第一条线段的可扩充区域不足的情况下发生，否则第一条线段肯定是越长越好的。
>
> 【2】：用 dp[i] 表示右端点不超过 prizePositions[i]，一条长度为 k 的线段最多可以覆盖的奖品的最大数量，可以推导如下：
>
> - 如果不选择位于 prizePositions[i] 处的奖品，则 dp[i]=dp[i−1]；
>
> - 如果选择位于 prizePositions[i] 处的奖品，则通过二分查找找到线段最左侧可以覆盖的奖品位置 prizePositions[j]，则此时 dp[i]=i−j+1；
>
> 我们取二者的最大值，可以得到动态规划递推公式如下：
>
> ```
> dp[i]=max(dp[i−1],i−j+1)
> ```
>
> 我们依次枚举第二条线段的最右侧端点 prizePositions[i]，此时第二条线段覆盖最左侧的奖品为 prizePositions[j]，则此时最多可以覆盖的奖品数量为：
>
> i−j+1+dp[j−1]
>
> 此时 dp[j−1] 表示第一条线段右端点不超过 prizePositions[j−1] 时最多可以覆盖的奖品数量。枚举过程中取最大值即为最终结果，返回即可。
>

## 31	2024-09-12

### （1）求出最多标记下标——2576

**个人题解**：

```java
class Solution {
    public int maxNumOfMarkedIndices(int[] nums) {
        int result = 0;
        int length = nums.length;
        int left = 0;
        int right = length / 2;
        List<Integer> numList = Arrays.stream(nums)
                .boxed()
                .sorted()
                .collect(Collectors.toList());
        while(right < numList.size()){
            if (2 * numList.get(left) <= numList.get(right)){
                numList.remove(right);
                numList.remove(left);
                right--;
            }else {
                right++;
            }
        }
        return length - numList.size();
    }
}
```

> 核心思路：
>
> 将nums放入list之后，对List进行排序。
>
> 因为需要最大匹配，因此取left与right；其中left从0开始，right从中间开始。
>
> ```
> 最开始有一个错误想法就是，从最小的数开始，尽量的用满足其两倍的且尽可能小的数来与之匹配对应。
> 后来意识到，尽可能小也不一定满足最多匹配，其同时也带走了一个比较小的数。
> 以2 4 5 9为例：
> 当从2开始，如果按照这个想法用4与之匹配对应，那么带走的4，就只剩下了5 与 9 。
> 而实际上最佳匹配方式是2 ~ 5、4 ~ 9；
> ```

**官方题解——方法二：双指针**：

```
class Solution {
    public int maxNumOfMarkedIndices(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        int m = n / 2;
        int res = 0;
        for (int i = 0, j = m; i < m && j < n; i++) {
            while (j < n && 2 * nums[i] > nums[j]) {
                j++;
            }
            if (j < n) {
                res += 2;
                j++;
            }
        }
        return res;
    }
}
```

> 官方的这个思路也是从中间开始找右侧对应数；
>
> 但是这里并没有引入List，减少了一定的计算复杂度。

## 32	2024-09-13

### （1）搜索二维矩阵二——240

**个人题解**：

```
class Solution {
    public int [][] matrix;
    public int [][] matrixMark;
    public int target;
    public boolean searchMatrix(int[][] matrix, int target) {
        this.matrix = matrix;
        this.target = target;
        this.matrixMark = new int[matrix.length][matrix[0].length];
        for (int i = 0; i < matrixMark.length; i++) {
            for (int j = 0; j < matrixMark[i].length; j++) {
                matrixMark[i][j] = 0;
            }
        }
        return dfs(0,0);
    }
    public boolean dfs(int i, int j){
        if (matrix[i][j] == target)
            return true;
        if (matrixMark[i][j] == 1){
            return false;
        }
        matrixMark[i][j] = 1;
        if (j+1 < matrix[0].length && matrix[i][j+1] <= target){
            if (dfs(i, j+1))
                return true;
        }
        if (i+1 < matrix.length && matrix[i+1][j] <= target){
            if (dfs(i+1, j))
                return true;
        }
        return false;
    }
}
```

> 核心思路：
>
> BFS+标记矩阵减少重复。

**官方题解**——：

```

```

## 33	2024-11-14

### （1）统计好节点的数目——3249

**个人题解**：

```
class Solution {
    Map<Integer, List<Integer>> cstnMap = new HashMap<>();
    Map<Integer, Integer> countMap = new HashMap<>();

    public int countGoodNodes(int[][] edges) {
        int resultCount = 0;
        int size = edges.length;
        Set<Integer> showUpSet = new HashSet<>();

        Queue<int[]> queue = new LinkedList<>();
        // 将所有数据加入队列
        for (int[] edge : edges) {
            queue.offer(edge);
        }

        cstnMap.put(0, new ArrayList<>());
        showUpSet.add(0);

        while (!queue.isEmpty()){
            int[] current = queue.poll();  // 取出队列的第一个元素
            int L = current[0];
            int R = current[1];
            if (!showUpSet.contains(L) && !showUpSet.contains(R)){
                queue.offer(current);
            }
            if (cstnMap.containsKey(L)){
                cstnMap.get(L).add(R);
                showUpSet.add(R);
                continue;
            }
            if (cstnMap.containsKey(R)){
                cstnMap.get(R).add(L);
                showUpSet.add(L);
                continue;
            }
            if (showUpSet.contains(L)){
                cstnMap.put(L,new ArrayList<>());
                cstnMap.get(L).add(R);
                showUpSet.add(R);
            }else {
                cstnMap.put(R, new ArrayList<>());
                cstnMap.get(R).add(L);
                showUpSet.add(L);
            }
        }
        for (List<Integer> list : cstnMap.values()){
            if (list.size() == 1){
                resultCount++;
                continue;
            }
            int balance = cstn(list.get(0));
            boolean sign = true;
            for(int i = 1; i < list.size(); i++){
                if (cstn(list.get(i)) != balance){
                    sign = false;
                    break;
                }
            }
            if (sign)
                resultCount++;
        }
        resultCount += size - cstnMap.size() + 1;
        return resultCount;
    }
    int cstn(int a){
        if (countMap.containsKey(a)){
            return countMap.get(a);
        }
        if (!cstnMap.containsKey(a))
            return 1;
        List<Integer> list = cstnMap.get(a);
        int sum = 1;
        for (int i : list){
            sum+=cstn(i);
        }
        if (!countMap.containsKey(a)){
            countMap.put(a,sum);
        }
        return sum;
    }
}
```

> 核心思路：
>
> 耶耶耶我是if大王哈哈哈哈😎嘻嘻啊啊啊 啊啊啊啊啊为什么！！！！😭
>
> 核心思路是
>
> 1. `构建一个cstnMap`，其用来记录非叶节点的子节点。
> 2. 构建完成之后，对每个非叶结点的子节点作为根节点的树 的节点数进行遍历，同时比对各子节点作为根节点的树 的节点数是否相同。引入一个数组减少重复计算量。

**官方题解**：

```
class Solution {
    int res = 0;
    List<Integer>[] g;

    public int countGoodNodes(int[][] edges) {
        int n = edges.length + 1;
        g = new List[n];
        for (int i = 0; i < n; i++) {
            g[i] = new ArrayList<Integer>();
        }
        for (int[] edge : edges) {
            g[edge[0]].add(edge[1]);
            g[edge[1]].add(edge[0]);
        }
        dfs(0, -1);
        return res;
    }

    public int dfs(int node, int parent) {
        boolean valid = true;
        int treeSize = 0;
        int subTreeSize = 0;
        for (int child : g[node]) {
            if (child != parent) {
                int size = dfs(child, node);
                if (subTreeSize == 0) {
                    subTreeSize = size;
                } else if (size != subTreeSize) {
                    valid = false;
                }
                treeSize += size;
            }
        }
        if (valid) {
            res++;
        }
        return treeSize + 1;
    }
}
```

> 核心思路：
>
> 也是通过构建一个链表，记录与这个节点有连接的节点。即，构造`领接表`。
>
> 自己的思路中，构建的是一个领接表的链表形式，每个节点的链中不会包含与自己相连接的根节点。
>
> 而后也是遍历通过深度优先搜索，检查所有节点子节点作为根的树节点数，同时判断是否满足题目的数目相同需求。

## 34	2024-11-15

### （1）最少翻转次数使二进制矩阵回文1——3239

**个人题解**：

```
class Solution {
    public int minFlips(int[][] grid) {
        int X = grid.length;
        int Y = grid[0].length;
//        按照行进行遍历检查
        int XCheckCounts = 0;
        for (int j = 0; j < Y; j++){
            List<Integer> list = new ArrayList<>();
            for (int i = 0 ; i < X ; i++){
                list.add(grid[i][j]);
            }
            XCheckCounts += check(list);
        }
//        按照列进行遍历检查
        int YCheckCounts = 0;
        for (int i = 0; i < X; i++){
            List<Integer> list = new ArrayList<>();
            for (int j = 0 ; j < Y ; j++){
                list.add(grid[i][j]);
            }
            YCheckCounts += check(list);
        }
        return XCheckCounts < YCheckCounts ? XCheckCounts:YCheckCounts ;
    }

//        实现对List<Integer>的检查，输出将该List修改为回文需要修改的次数
    public int check(List<Integer> list){
        int changeCount = 0;
        int L = 0;
        int R = list.size()-1;
        while (R - L > 0){
            if (list.get(L) != list.get(R))
                changeCount++;
            L++;
            R--;
        }
        return changeCount;
    }
}
```

> ![image-20241115111453492](https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20241115111453492.png)
>
> 时间开销较大、但内存消耗小。
>
> 核心思路
>
> 计算按照 行 与 列 的两种方式下的各自全部需要变动数。
>
> 提供一个函数check，计算输入的List变成回文串需要变动的次数。
>
> 另外自己的这里命名X与Y有点歧义，说是按照行进行遍历，但是实际上计算结果是垂直回文需要的改变数。

**官方题解**：

```
class Solution {
    public int minFlips(int[][] grid) {
        int rowCnt = 0, colCnt = 0;
        int m = grid.length, n = grid[0].length;
        for (int i = 0; i < m; i++) {
            for (int j1 = 0, j2 = n - 1; j1 < j2; j1++, j2--) {
                if ((grid[i][j1] ^ grid[i][j2]) != 0) {
                    rowCnt++;
                }
            }
        }
        for (int j = 0; j < n; j++) {
            for (int i1 = 0, i2 = m - 1; i1 < i2; i1++, i2--) {
                if ((grid[i1][j] ^ grid[i2][j]) != 0) {
                    colCnt++;
                }
            }
        }
        return Math.min(colCnt, rowCnt);
    }
}
```

> 核心思路
>
> 也是按照行、列两次计算，取其中的小值作为结果。
>
> 以行计算为例，没有将每一行值单独取出来存在List中进行处理，而是直接在遍历该行的时候直接进行回文判断。

## 35	2024-11-16

### （1）最少翻转次数使而二进制矩阵回文2——3240

**个人题解**：

```
无
```

> 核心思路
>
> 已经想的过于复杂，不太可能如此不简洁。

**官方题解**：

```

```

> 核心思路



## 36	2024-11-30

### （1）删除链表的倒数第N个结点——19

**个人题解**：

```
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode calcNode = head;
        int length = 1;
        while (calcNode.next != null){
            length ++;
            calcNode = calcNode.next;
        }
        if (n == length){
            return head.next;
        }
        calcNode = head;
        int restNode = length - 1;
        while (restNode > n){
            restNode --;
            calcNode = calcNode.next;
        }
        calcNode.next = calcNode.next.next == null ? null : calcNode.next.next;

        System.out.println();
        return head;
    }
}
```

> 核心思路：
>
> 需要遍历两遍，第一遍计算长度，第二遍执行删除。

**官方题解——栈**：

```
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0, head);
        Deque<ListNode> stack = new LinkedList<ListNode>();
        ListNode cur = dummy;
        while (cur != null) {
            stack.push(cur);
            cur = cur.next;
        }
        for (int i = 0; i < n; ++i) {
            stack.pop();
        }
        ListNode prev = stack.peek();
        prev.next = prev.next.next;
        ListNode ans = dummy.next;
        return ans;
    }
}
```

> 核心思想：
>
> 将链表结点全部加入栈中，然后从栈顶开始往外pop。

**官方题解——双指针**：

```
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0, head);
        ListNode first = head;
        ListNode second = dummy;
        for (int i = 0; i < n; ++i) {
            first = first.next;
        }
        while (first != null) {
            first = first.next;
            second = second.next;
        }
        second.next = second.next.next;
        ListNode ans = dummy.next;
        return ans;
    }
}
```

> 使用两个指针first与second，设置两个指针的间距为n，然后两者同时往后移动，当second到达底部的时候，first位置的指针即需要删除的对象。

## 37	2024-12-01

### （1）环形链表——142

**个人题解**：

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null)
            return null;
        Set<ListNode> set = new HashSet<>();
        set.add(head);
        while (head.next != null) {
            if (set.contains(head.next)) {
                return head.next;
            }else {
                set.add(head.next);
            }
            head = head.next;
        }
        return null;
    }
}
```

> 核心思路：
>
> 构建一个Set，存放已经出现过的ListNode

**官方题解——哈希表**：

```
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode pos = head;
        Set<ListNode> visited = new HashSet<ListNode>();
        while (pos != null) {
            if (visited.contains(pos)) {
                return pos;
            } else {
                visited.add(pos);
            }
            pos = pos.next;
        }
        return null;
    }
}
```

> 核心思路
>
> 和自己想法一样

**官方题解——双指针**

```
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null) {
            return null;
        }
        ListNode slow = head, fast = head;
        while (fast != null) {
            slow = slow.next;
            if (fast.next != null) {
                fast = fast.next.next;
            } else {
                return null;
            }
            if (fast == slow) {
                ListNode ptr = head;
                while (ptr != slow) {
                    ptr = ptr.next;
                    slow = slow.next;
                }
                return ptr;
            }
        }
        return null;
    }
}
```

> 核心思路：
>
> 快慢指针。
>
> 其实自己也想到了快慢指针能用来确认是否存在环，但是陷入了两个指针无法找到入环点的困境中：
>
> ```
> public class Solution {
>     public ListNode detectCycle(ListNode head) {
>         if (head == null) {
>             return null;
>         }
>         ListNode first = head.next;
>         ListNode second = head;
> 
>         while (first != null){
>             first = first.next;
>             if (second == first){
>                 return second;
>             }
>             if (first == null)
>                 return null;
>             first = first.next;
>             second = second.next;
>         }
>         return null;
>     }
> }
> ```
>
> 这个时候应该再引入第三个指针来寻找入环点。
>
> 同时，根据数学计算，最终会发现当frist与second指针相遇的位置恰好满足一个等式，使得第三个用来确认入环点的指针的计算不会花费过多的计算。参考[官方题解](https://leetcode.cn/problems/linked-list-cycle-ii/solutions/441131/huan-xing-lian-biao-ii-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)。

## 38	2024-12-02

### （1）两两交换链表中的节点——24

**个人题解**：

```
class Solution {
    public ListNode swapPairs(ListNode head) {
        if (head == null || head.next == null){
            return head;
        }
        ListNode origin = new ListNode();
        ListNode t = head.next;
        origin.next = t;

        head.next = t.next;
        t.next = head;

        while (head.next != null && head.next.next != null){
            head = head.next;
            t.next.next = head.next;
            t = head.next;
            head.next = t.next;
            t.next = head;
        }
        return origin.next;
    }
}
```

> 核心思路
>
> 就是对链表的操作，注意一下next的变换就行。
>
> 执行效率比较高。
>
> <img src="https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20241202201718751.png" alt="image-20241202201718751" style="zoom:50%;" />

**官方题解——递归**：

```
class Solution {
    public ListNode swapPairs(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode newHead = head.next;
        head.next = swapPairs(newHead.next);
        newHead.next = head;
        return newHead;
    }
}
```

> 核心思路
>
> 官方的方法更简洁，同时通过递归的方式解决了自己实现中遇到的一个问题：
>
> 当指针都往后移动指向新的节点之后，便无法再改变原本指向该新节点的旧节点的指向。

**官方题解——迭代**：

```
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        ListNode temp = dummyHead;
        while (temp.next != null && temp.next.next != null) {
            ListNode node1 = temp.next;
            ListNode node2 = temp.next.next;
            temp.next = node2;
            node1.next = node2.next;
            node2.next = node1;
            temp = node1;
        }
        return dummyHead.next;
    }
}
```

> 核心思路：
>
> 和自己的实现思路相同。其中用到了一个temp节点，与原本自己实现中使用到的tip结点类似，但是后来将tip结点优化掉了。

## 39	2024-12-03

### （1）随机链表的复制——138

**个人题解**：

```
class Solution {
    Map<Node, Node> map = new HashMap<>();
    int length = 0;
    public Node copyRandomList(Node head) {
        if (head == null){
            return null;
        }
        Node node = new Node (head.val);
        map.put(head, node);
        if (head.next != null){
            node.next = copyRandomList(head.next);
            length ++;
        }
        if (map.containsKey(head.random)){
            node.random = map.get(head.random);
        }
        return node;
    }
}
```

> 核心思路
>
> 状态不错哦，一遍成功。
>
> <img src="https://gitee.com/hanhandehanpi/cloudimage/raw/master/cloudimage/image-20241203203411225.png" alt="image-20241203203411225" style="zoom: 67%;" />
>
> 最开始纠结的点在于如何将head与新的node的节点进行对应。
>
> 然后理所当然的想到了map，不过最开始纠结在使用Integer与Node类型的对应，但是因为Node中random属性是node，并不是Integer，这使得如果要对应上会需要第二个map表，这并不合理。
>
> 然后灵光一现想到构建Map<Node, Node>，问题解决。

**官方题解——回溯+哈希表**：

```
class Solution {
    Map<Node, Node> cachedNode = new HashMap<Node, Node>();

    public Node copyRandomList(Node head) {
        if (head == null) {
            return null;
        }
        if (!cachedNode.containsKey(head)) {
            Node headNew = new Node(head.val);
            cachedNode.put(head, headNew);
            headNew.next = copyRandomList(head.next);
            headNew.random = copyRandomList(head.random);
        }
        return cachedNode.get(head);
    }
}
```

> 核心思路
>
> 和官方题解基本一致。

**官方题解——迭代+节点拆分**：

```
class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) {
            return null;
        }
        for (Node node = head; node != null; node = node.next.next) {
            Node nodeNew = new Node(node.val);
            nodeNew.next = node.next;
            node.next = nodeNew;
        }
        for (Node node = head; node != null; node = node.next.next) {
            Node nodeNew = node.next;
            nodeNew.random = (node.random != null) ? node.random.next : null;
        }
        Node headNew = head.next;
        for (Node node = head; node != null; node = node.next) {
            Node nodeNew = node.next;
            node.next = node.next.next;
            nodeNew.next = (nodeNew.next != null) ? nodeNew.next.next : null;
        }
        return headNew;
    }
}
```

## 40	2024-12-04

### （1）排序列表——148

**个人题解**：

```
class Solution {
    public ListNode sortList(ListNode head) {
//        初始化一个小根堆
        PriorityQueue<ListNode> priorityQueue = new PriorityQueue<>((a, b) -> a.val - b.val);
        ListNode resultListNode = new ListNode();
        ListNode curr = resultListNode;
        while (head != null) {
            priorityQueue.add(head);
            head = head.next;
        }
        while (!priorityQueue.isEmpty()) {
            curr.next = new ListNode(priorityQueue.poll().val);
            curr = curr.next;
        }
        return resultListNode.next;
    }
}
```

> 核心思路
>
> 使用大根堆进行解答。
>
> 大根堆来解答该题相当方便的，没有太多的难点。
>
> 需要注意到，出现过一个错误提示：`Find cycle in the LIstNode`。
>
> 错误来源是因为直接使用了priorityQueue.poll()的值作为结果值，这会导致原本的Node之间存在的粘黏现象，所以需要取ListNode的值来新构建ListNode，这样就能避免原本ListNode之间的粘黏现象。

**官方题解——自顶向下归并排序⭐**：

```java
class Solution {
    public ListNode sortList(ListNode head) {
        return sortList(head, null);
    }

    public ListNode sortList(ListNode head, ListNode tail) {
        if (head == null) {
            return head;
        }
        if (head.next == tail) {
            head.next = null;
            return head;
        }
        ListNode slow = head, fast = head;
        while (fast != tail) {
            slow = slow.next;
            fast = fast.next;
            if (fast != tail) {
                fast = fast.next;
            }
        }
        ListNode mid = slow;
        ListNode list1 = sortList(head, mid);
        ListNode list2 = sortList(mid, tail);
        ListNode sorted = merge(list1, list2);
        return sorted;
    }

    public ListNode merge(ListNode head1, ListNode head2) {
        ListNode dummyHead = new ListNode(0);
        ListNode temp = dummyHead, temp1 = head1, temp2 = head2;
        while (temp1 != null && temp2 != null) {
            if (temp1.val <= temp2.val) {
                temp.next = temp1;
                temp1 = temp1.next;
            } else {
                temp.next = temp2;
                temp2 = temp2.next;
            }
            temp = temp.next;
        }
        if (temp1 != null) {
            temp.next = temp1;
        } else if (temp2 != null) {
            temp.next = temp2;
        }
        return dummyHead.next;
    }
}
```

> 核心思路
>
> 核心在于归并。对链表自顶向下归并排序的过程如下。
>
> 找到链表的中点，以中点为分界，将链表拆分成两个子链表。寻找链表的中点可以使用快慢指针的做法，快指针每次移动 2 步，慢指针每次移动 1 步，当快指针到达链表末尾时，慢指针指向的链表节点即为链表的中点。
>
> 对两个子链表分别排序。
>
> 将两个排序后的子链表合并，得到完整的排序后的链表。可以使用「21. 合并两个有序链表」的做法，将两个有序的子链表进行合并。
>
> 上述过程可以通过递归实现。递归的终止条件是链表的节点个数小于或等于 1，即当链表为空或者链表只包含 1 个节点时，不需要对链表进行拆分和排序。

## 41	2024-12-05

### （1）两数相加——2

**个人题解**：

```
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        int addCount = 0;

        ListNode resultListNode = new ListNode(0);
        ListNode curr = resultListNode;
        while (l1 != null && l2 != null) {
            int sum = l1.val + l2.val + addCount;
            l1 = l1.next;
            l2 = l2.next;
            addCount = 0;
            if (sum >= 10){
                sum = sum - 10;
                addCount = 1;
            }
            curr.next = new ListNode(sum);
            curr = curr.next;
        }
        while (l1 != null) {
            int currVal = l1.val + addCount;
            l1 = l1.next;
            addCount = 0;
            if (currVal >= 10) {
                currVal -= 10;
                addCount = 1;
            }
            curr.next = new ListNode(currVal);
            curr = curr.next;
        }
        while (l2 != null) {
            int currVal = l2.val + addCount;
            l2 = l2.next;
            addCount = 0;
            if (currVal >= 10) {
                currVal -= 10;
                addCount = 1;
            }
            curr.next = new ListNode(currVal);
            curr = curr.next;
        }
        if (addCount == 1) {
            curr.next = new ListNode(1);
            curr = curr.next;
        }
        return resultListNode.next;
    }
}

```

> 核心思路
>
> 简单容易理解没有难度，一开始看错链表的方向了。

**官方题解**：

```
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode head = null, tail = null;
        int carry = 0;
        while (l1 != null || l2 != null) {
            int n1 = l1 != null ? l1.val : 0;
            int n2 = l2 != null ? l2.val : 0;
            int sum = n1 + n2 + carry;
            if (head == null) {
                head = tail = new ListNode(sum % 10);
            } else {
                tail.next = new ListNode(sum % 10);
                tail = tail.next;
            }
            carry = sum / 10;
            if (l1 != null) {
                l1 = l1.next;
            }
            if (l2 != null) {
                l2 = l2.next;
            }
        }
        if (carry > 0) {
            tail.next = new ListNode(carry);
        }
        return head;
    }
}
```

> 核心思路
>
> 对自己的实现的代码上的简化。

## 42	2024-12-06

### （1）LRU缓存——146

**个人题解**：

```java
public class LRUCache{
    Integer capacity;
    DLinkedNode head = new DLinkedNode();
    DLinkedNode tail = new DLinkedNode();

    Map<Integer, DLinkedNode> map = new HashMap<>();
    class DLinkedNode {
        int key;
        int value;
        DLinkedNode prev;
        DLinkedNode next;
        public DLinkedNode() {}
        public DLinkedNode(int _key, int _value) {key = _key; value = _value;}
    }
    public LRUCache(int capacity) {
        this.capacity = capacity;
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
//        1 如果不存在该node
        if (!map.containsKey(key)) {
            return -1;
        }else {
//        2 如果存在该node
            DLinkedNode node = map.get(key);
//            将该node的前后节点链接
            node.prev.next = node.next;
            node.next.prev = node.prev;
//            将该节点在head tail中添加至最前
            node.next = head.next;
            node.prev = head;
            head.next.prev = node;
            head.next = node;
            return node.value;
        }
    }

    public void put(int key, int value) {
//        1 当map中不存在该DLinkedNode
//          1.1   将该节点加入map中
//              1.1.1   如果map没有超过容量
//              1.1.2   如果map已经超过容量
//          1.2   添加到head 与 tail之间
//              1.2.1   超过容量的处理方式
//              1.2.2   没超过容量的处理方式
        if (!map.containsKey(key)) {
            DLinkedNode node = new DLinkedNode(key, value);
            if (map.size() == capacity) {
//                map删除最后一个node的键值对
                map.remove(tail.prev.key);
//                head tail处理删除最后一个节点
                tail.prev.prev.next = tail;
                tail.prev = tail.prev.prev;
            }
//            将该node移动至最前
            node.next = head.next;
            node.prev = head;
            head.next.prev = node;
            head.next = node;
//            将该node加入map中
            map.put(key, node);
            return;
        }else {
//        2 当map中存在该DLinkedNode
            DLinkedNode node = map.get(key);
//            2.1   将node的前后节点链接
            node.prev.next = node.next;
            node.next.prev = node.prev;
//            2.2   将node移动至最前方
            node.next = head.next;
            node.prev = head;
            head.next.prev = node;
            head.next = node;
//            2.3   修改该node的值
            node.value = value;
        }
    }
}
```

> 核心思路
>
> 核心在于使用双向ListNode。
>
> 关键的地方其实就在于将head tail之间的节点链接与断链

**官方题解**：

```java
public class LRUCache {
    class DLinkedNode {
        int key;
        int value;
        DLinkedNode prev;
        DLinkedNode next;
        public DLinkedNode() {}
        public DLinkedNode(int _key, int _value) {key = _key; value = _value;}
    }

    private Map<Integer, DLinkedNode> cache = new HashMap<Integer, DLinkedNode>();
    private int size;
    private int capacity;
    private DLinkedNode head, tail;

    public LRUCache(int capacity) {
        this.size = 0;
        this.capacity = capacity;
        // 使用伪头部和伪尾部节点
        head = new DLinkedNode();
        tail = new DLinkedNode();
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
        DLinkedNode node = cache.get(key);
        if (node == null) {
            return -1;
        }
        // 如果 key 存在，先通过哈希表定位，再移到头部
        moveToHead(node);
        return node.value;
    }

    public void put(int key, int value) {
        DLinkedNode node = cache.get(key);
        if (node == null) {
            // 如果 key 不存在，创建一个新的节点
            DLinkedNode newNode = new DLinkedNode(key, value);
            // 添加进哈希表
            cache.put(key, newNode);
            // 添加至双向链表的头部
            addToHead(newNode);
            ++size;
            if (size > capacity) {
                // 如果超出容量，删除双向链表的尾部节点
                DLinkedNode tail = removeTail();
                // 删除哈希表中对应的项
                cache.remove(tail.key);
                --size;
            }
        }
        else {
            // 如果 key 存在，先通过哈希表定位，再修改 value，并移到头部
            node.value = value;
            moveToHead(node);
        }
    }

    private void addToHead(DLinkedNode node) {
        node.prev = head;
        node.next = head.next;
        head.next.prev = node;
        head.next = node;
    }

    private void removeNode(DLinkedNode node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    private void moveToHead(DLinkedNode node) {
        removeNode(node);
        addToHead(node);
    }

    private DLinkedNode removeTail() {
        DLinkedNode res = tail.prev;
        removeNode(res);
        return res;
    }
}
```

> 核心思路
>
> 同样的思路。将其中涉及到的一些步骤用统一的函数封装。

## 43	2024-12-08

### （1）二叉树的中序遍历——94

**个人题解**：

```java
class Solution {
    List<Integer> result = new ArrayList<>();
    public List<Integer> inorderTraversal(TreeNode root) {
        if (root == null) {
            return result;
        }
        if (root.left != null) {
            inorderTraversal(root.left);
        }
        result.add(root.val);
        if (root.right != null) {
            inorderTraversal(root.right);
        }
        return result;
    }
}
```

> 核心思路
>
> 简单题

**官方题解——递归**：

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        inorder(root, res);
        return res;
    }

    public void inorder(TreeNode root, List<Integer> res) {
        if (root == null) {
            return;
        }
        inorder(root.left, res);
        res.add(root.val);
        inorder(root.right, res);
    }
}
```

> 核心思路
>
> 没啥区别

**官方题解——迭代⭐：**

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        Deque<TreeNode> stk = new LinkedList<TreeNode>();
        while (root != null || !stk.isEmpty()) {
            while (root != null) {
                stk.push(root);
                root = root.left;
            }
            root = stk.pop();
            res.add(root.val);
            root = root.right;
        }
        return res;
    }
}
```

> 核心思路：
>
> 通过一个栈，通过pop的时候模拟中序遍历的效果。
>
> 核心在于        `while (root != null || !stk.isEmpty())`    判断逻辑。

### （2）二叉树的层序遍历——102

**个人题解：**

```java
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    int currLevel = 0;
    public List<List<Integer>> levelOrder(TreeNode root) {
        if (root == null) {
            return result;
        }
        levelOrder2(root, 0);
        return result;
    }
    public void levelOrder2(TreeNode root, int level) {
        if (level == result.size()) {
            List<Integer> currLevelList = new ArrayList<>();
            currLevelList.add(root.val);
            result.add(currLevelList);
        }else {
            result.get(level).add(root.val);
        }
        if (root.left != null) {
            levelOrder2(root.left, level + 1);
        }
        if (root.right != null) {
            levelOrder2(root.right, level + 1);
        }
    }
}
```

> 核心思路
>
> 使用一个level作为标识，记录当前的level。
>
> 效率还挺高：
>
> <img src="https://gitee.com/hanhandehanpi/cloudimage/raw/master/image-20241208172658959.png" alt="image-20241208172658959" style="zoom:33%;" />

**官方题解：**

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> ret = new ArrayList<List<Integer>>();
        if (root == null) {
            return ret;
        }

        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            List<Integer> level = new ArrayList<Integer>();
            int currentLevelSize = queue.size();
            for (int i = 1; i <= currentLevelSize; ++i) {
                TreeNode node = queue.poll();
                level.add(node.val);
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
            ret.add(level);
        }
        
        return ret;
    }
}
```

> 核心思路
>
> 将位于同一层级的节点都加入queue中，然后遍历queue中的每个节点（遍历的同时将这个节点删除）检查其是否存在子节点，如果有，就再加入queue中。
>
> 这样的实现会使得每一次while循环的时候，queue内部的都是位于同一个层级的所有节点。

## 44	2024-12-09

### （1）验证二叉搜索树——98

**个人题解**：

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }
        if (!checkLeftMax(root) || !checkRightMin(root)) {
            return false;
        }
        if (!isValidBST(root.left) || !isValidBST(root.right)) {
            return false;
        }
        return true;
    }
    public boolean checkLeftMax(TreeNode root) {
        TreeNode leftMaxNode = root.left;
        if (leftMaxNode == null) {
            return true;
        }
        while (leftMaxNode.right != null) {
            leftMaxNode = leftMaxNode.right;
        }
        if (root.val <= leftMaxNode.val) {
            return false;
        }else
            return true;
    }
    public boolean checkRightMin(TreeNode root) {
        TreeNode rightMinNode = root.right;
        if (rightMinNode == null) {
            return true;
        }
        while (rightMinNode.left != null) {
            rightMinNode = rightMinNode.left;
        }
        if (root.val >= rightMinNode.val) {
            return false;
        }else
            return true;
    }
}
```

> 核心思路
>
> 感觉解答的有点丑陋，全是if，并不优美，但实际上执行效率却意外挺高。
>
> <img src="https://gitee.com/hanhandehanpi/cloudimage/raw/master/image-20241209105141702.png" alt="image-20241209105141702" style="zoom:33%;" />
>
> 核心思路也比较直观，分别检查当前节点的左右子树。检查左子树的最大节点，检查右子树的最小节点，然后判断当前root节点是否大于左子树的最大节点，是否小于右子树的最小节点。
>
> 然后通过一个递归的方式，检查其左子树与右子树的所有节点。

**官方题解——递归**：

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    public boolean isValidBST(TreeNode node, long lower, long upper) {
        if (node == null) {
            return true;
        }
        if (node.val <= lower || node.val >= upper) {
            return false;
        }
        return isValidBST(node.left, lower, node.val) && isValidBST(node.right, node.val, upper);
    }
}
```

> 核心思路
>
> 官方的核心思路，是针对当前的root节点，判断并检查限制其值是否在一个（l，r）的范围之内。
>
> 在最开始的root节点时，当前的(l,r)是(-inf, inf)。在java中即`(Long.MIN_VALUE, Long.MAX_VALUE)`，当开始遍历root的左节点的时，则将对左子树的范围限制为(-inf,root.val)，对右子树的检查范围限制在(root.val,inf)之间。

**官方题解——中序遍历⭐**

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        Deque<TreeNode> stack = new LinkedList<TreeNode>();
        double inorder = -Double.MAX_VALUE;

        while (!stack.isEmpty() || root != null) {
            while (root != null) {
                stack.push(root);
                root = root.left;
            }
            	// 个人注解： 这里使用了栈，放入的时候并不是中序遍历，但是pop出来的时候是中序遍历。
            root = stack.pop();
              // 如果中序遍历得到的节点的值小于等于前一个 inorder，说明不是二叉搜索树
            if (root.val <= inorder) {
                return false;
            }
            inorder = root.val;
            root = root.right;
        }
        return true;
    }
}
```

> 核心思路：
>
> 这个应该才是解决这个问题的最优雅方案。
>
> 自己通过之前实现的中序遍历，进行了些许改造后实现的内容：
>
> ```java
> class Solution {
>     Integer tempValue;
>     public boolean isValidBST(TreeNode root) {
>         return inorderTraversal(root);
>     }
>     public Boolean inorderTraversal(TreeNode root) {
>         if (root == null) {
>             return true;
>         }
>         if (root.left != null) {
>             if (!inorderTraversal(root.left)){
>                 return false;
>             }
>         }
>         if (tempValue != null && root.val <= tempValue) {
>             return false;
>         }
>         tempValue = root.val;
>         if (root.right != null) {
>             if (!inorderTraversal(root.right)){
>                 return false;
>             }
>         }
>         return true;
>     }
> }
> ```

## 45	2024-12-10

### （1）二叉搜索树中第K小的元素——230

**个人题解**：

```java
class Solution {
    Integer count;
    Integer result;
    public int kthSmallest(TreeNode root, int k) {
        count = k;
        inorder(root);
        return result;
    }
    public void inorder(TreeNode root) {
        if (root.left != null) {
            inorder(root.left);
        }
        if (--count == 0){
            result = root.val;
        }
        if (root.right != null) {
            inorder(root.right);
        }
    }
}
```

> 核心思路
>
> 中序遍历，找第k小的数。

**官方题解**：

```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        Deque<TreeNode> stack = new ArrayDeque<TreeNode>();
        while (root != null || !stack.isEmpty()) {
            while (root != null) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            --k;
            if (k == 0) {
                break;
            }
            root = root.right;
        }
        return root.val;
    }
}
```

> 核心思路
>
> 使用在`二叉树的中序遍历——94`部分通过栈实现的的中序遍历效果，很方便的能找到第k个元素。优势在于不需要用到那么多全局变量。不过同时牺牲了一定的空间【也不能这么说，毕竟递归本质上是系统在维护一个栈，该方法是自己维护一个栈而已】。

## 46	2024-10-11

### （1）二叉树的右视图——199

**个人题解**：

```
class Solution {
    List<Integer> result = new ArrayList<>();
    public List<Integer> rightSideView(TreeNode root) {
        checkView(root, 0);
        return result;
    }
    public void checkView(TreeNode root, int height) {
        if (root == null) {
            return;
        }
        if (height >= result.size()) {
            result.add(root.val);
        }
        checkView(root.right, height + 1);
        checkView(root.left, height + 1);
    }
}
```

> 核心思路
>
> 没想到这么简单。。。核心就是遍历节点，先遍历右边的。如果当前的高度是没有记录过的值，就将其加入到resultList中。
>
> 效率也挺高：
>
> <img src="https://gitee.com/hanhandehanpi/cloudimage/raw/master/image-20241211102046349.png" alt="image-20241211102046349" style="zoom:33%;" />

**官方题解**：

```java
// 官方的题解比较抽象，也并不合理。
```

> 核心思路

**评论区题解——java dfs：**

```java
class Solution {
    private List<Integer> ans;

    public List<Integer> rightSideView(TreeNode root) {
        ans = new ArrayList<>();
        dfs(root, 0);
        return ans;
    }

    private void dfs(TreeNode node, int depth) {
        if (node == null) return;
        if (ans.size() <= depth) 
            ans.add(node.val);
        else 
            ans.set(depth, node.val);
        dfs(node.left, depth + 1);
        dfs(node.right, depth + 1);
    }
}
```

> 核心思路：
>
> 这个和自己的实现就很一致了。不过参考这个，能发现一些自己的实现可以优化的地方：
>
> 1. 在class内申请全局变量的时候，使用private 进行申请；
> 2. 申请全局变量的时候，不需要在最开始就将其初始化，可以在需要的时候在进行初始化；
>
> 不过，这个代码也有些缺陷：
>
> 主要集中在：
>
> ```
>         if (ans.size() <= depth) 
>             ans.add(node.val);
>         else 
>             ans.set(depth, node.val);
>         dfs(node.left, depth + 1);
>         dfs(node.right, depth + 1);
> ```
>
> 这里逻辑上先对左枝进行判断，如果后续有右枝的话让右枝内容替换左枝，其实这个并不如直接用右枝进行判断，可以减少一定的计算开销，也就是自己的实现方式。

**评论区题解——java bfs：**

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        if (root == null) return ans;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        int count;
        while (!q.isEmpty()) {
            count = q.size();
            for (int i = 0; i < count; i++) {
                TreeNode node = q.poll();
                if (node.left != null) q.offer(node.left);
                if (node.right != null) q.offer(node.right);
                if (i == count - 1) ans.add(node.val);
            }
        }
        return ans;
    }
}
```

> 核心思路，换成了BFS。
>
> 也和前边的dfs一样存在一点问题，进行了一点优化如下：
>
> ```java
> class Solution {
>     public List<Integer> rightSideView(TreeNode root) {
>         List<Integer> ans = new ArrayList<>();
>         if (root == null) return ans;
>         Deque<TreeNode> q = new LinkedList<>();
>         q.offer(root);
>         while (!q.isEmpty()) {
>             int count = q.size();
>             ans.add(q.peek().val);
>             for (int i = 0; i < count; i++) {
>                 TreeNode node = q.poll();
>                 if (node.right != null) q.offer(node.right);
>                 if (node.left != null) q.offer(node.left);
>             }
>         }
>         return ans;
>     }
> }
> ```

## 47	2024-12-12

### （1）二叉树展开为链表—114

**个人题解**：

```java
class Solution {
    Queue<TreeNode> treeNodeList = new LinkedList<>();
    public void flatten(TreeNode root) {
        if (root == null) {
            return;
        }
        LFS(root);
        while (!treeNodeList.isEmpty()) {
            TreeNode node = treeNodeList.poll();
            node.left = null;
            node.right = treeNodeList.peek();
        }
    }
    public void LFS(TreeNode root) {
        treeNodeList.add(root);
        if (root.left != null) {
            LFS(root.left);
        }
        if (root.right != null) {
            LFS(root.right);
        }
    }
}
```

> 核心思路
>
> 先将先序遍历的结果放在Queue中，然后按照Queue的结果将root排序好并返回。
>
> 很容易就看出用先序遍历+记录的实现方式，但是想着有没有更加节省空间的方式：
> 有一个思路是，对root节点分别对右子节点以及左子节点进行处理，对其中左子节点处理后返回tailNode值，右子节点并不需要做额外处理，然后将
>
> ```
> tailNode.right = root.right;
> root.right = tailNode；
> ```
>
> 但是实现中发现没法很好的传递每个节点的对应tailNode，真要进行传递还需要一张额外的表来记录每个节点对应的tailNode 的值，这就和最开始的节省空间的思路相背离了。

**官方题解——通过迭代实现的前序遍历**：

```java
class Solution {
    public void flatten(TreeNode root) {
        List<TreeNode> list = new ArrayList<TreeNode>();
        Deque<TreeNode> stack = new LinkedList<TreeNode>();
        TreeNode node = root;
        while (node != null || !stack.isEmpty()) {
            while (node != null) {
                list.add(node);
              	// 这里的前序遍历体现在加入List中的元素是前序遍历的。与之前的中序遍历官方题解不同。
                stack.push(node);
                node = node.left;
            }
            node = stack.pop();
            node = node.right;
        }
        int size = list.size();
        for (int i = 1; i < size; i++) {
            TreeNode prev = list.get(i - 1), curr = list.get(i);
            prev.left = null;
            prev.right = curr;
        }
    }
}
```

> 核心思路
>
> 思路一样，只是通过迭代来实现其中的前序遍历。

**官方题解——前序遍历和展开同步进行**：

```java
class Solution {
    public void flatten(TreeNode root) {
        if (root == null) {
            return;
        }
        Deque<TreeNode> stack = new LinkedList<TreeNode>();
        stack.push(root);
        TreeNode prev = null;
        while (!stack.isEmpty()) {
            TreeNode curr = stack.pop();
            if (prev != null) {
                prev.left = null;
                prev.right = curr;
            }
            TreeNode left = curr.left, right = curr.right;
            if (right != null) {
                stack.push(right);
            }
            if (left != null) {
                stack.push(left);
            }
            prev = curr;
        }
    }
}
```

> 核心思想：
>
> 有点类似于自己思路中提到的tailNode，但是相当的巧妙。
>
> 这个栈的推出非常的恰到好处，就是比较难想到。

## 48	2024-12-13

### （1）从前序与中序遍历序列构造二叉树——105

**个人题解**：

```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        TreeNode root = new TreeNode(preorder[0]);
        return buildTreePro(preorder, inorder, 0, preorder.length - 1, 0, inorder.length-1, root);
    }
    public TreeNode buildTreePro(int[] preorder, int[] inorder, int startPointerPre, int endPointerPre, int startPointerInorder, int endPointerInorder, TreeNode root) {
        if (startPointerPre == endPointerPre || startPointerInorder == endPointerInorder) {
            return new TreeNode(preorder[startPointerPre]);
        }
        int currPreorderPointer = startPointerPre, currInorderPointer = startPointerInorder;
        int leftNodesCount = 0;
        while (inorder[currInorderPointer] != preorder[currPreorderPointer]) {
            leftNodesCount ++;
            currInorderPointer++;
        }
        int rightRootLocation = currPreorderPointer + leftNodesCount + 1;

        TreeNode rightRootNode = null;
        TreeNode leftRootNode = null;

        if (leftNodesCount != 0) {
            leftRootNode = new TreeNode(preorder[++currPreorderPointer]);
        }
        if (rightRootLocation <= endPointerPre) {
            rightRootNode = new TreeNode(preorder[rightRootLocation]);
        }
        root.left = leftRootNode;
        root.right = rightRootNode;

        if (leftRootNode != null) {
            buildTreePro(preorder, inorder, currPreorderPointer, rightRootLocation - 1, startPointerInorder, currInorderPointer - 1, leftRootNode);
        }
        if (rightRootNode != null) {
            buildTreePro(preorder, inorder, rightRootLocation, endPointerPre, currInorderPointer + 1, endPointerInorder, rightRootNode);
        }
        return root;
    }
}
```

> 核心思路
>
> 实现的不太优美，又开始有点变成if大王👑了。
>
> 核心逻辑就是根据传统思路，感觉优美的实现应该是通过栈来实现。

**官方题解——递归**：

```java
class Solution {
    private Map<Integer, Integer> indexMap;

    public TreeNode myBuildTree(int[] preorder, int[] inorder, int preorder_left, int preorder_right, int inorder_left, int inorder_right) {
        if (preorder_left > preorder_right) {
            return null;
        }

        // 前序遍历中的第一个节点就是根节点
        int preorder_root = preorder_left;
        // 在中序遍历中定位根节点
        int inorder_root = indexMap.get(preorder[preorder_root]);
        
        // 先把根节点建立出来
        TreeNode root = new TreeNode(preorder[preorder_root]);
        // 得到左子树中的节点数目
        int   = inorder_root - inorder_left;
        // 递归地构造左子树，并连接到根节点
        // 先序遍历中「从 左边界+1 开始的 size_left_subtree」个元素就对应了中序遍历中「从 左边界 开始到 根节点定位-1」的元素
        root.left = myBuildTree(preorder, inorder, preorder_left + 1, preorder_left + size_left_subtree, inorder_left, inorder_root - 1);
        // 递归地构造右子树，并连接到根节点
        // 先序遍历中「从 左边界+1+左子树节点数目 开始到 右边界」的元素就对应了中序遍历中「从 根节点定位+1 到 右边界」的元素
        root.right = myBuildTree(preorder, inorder, preorder_left + size_left_subtree + 1, preorder_right, inorder_root + 1, inorder_right);
        return root;
    }

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        int n = preorder.length;
        // 构造哈希映射，帮助我们快速定位根节点
        indexMap = new HashMap<Integer, Integer>();
        for (int i = 0; i < n; i++) {
            indexMap.put(inorder[i], i);
        }
        return myBuildTree(preorder, inorder, 0, n - 1, 0, n - 1);
    }
}
```

> 核心思路
>
> 官方代码中对自己的实现主要的优化在于引入了一个Map映射，用于减少一些特殊情况下的重复遍历次数。
>
> 特别是当出现类似于以下这种结构的树时，官方的代码会明显减少很多重复的遍历。
>
> 以下图中的结构为例，在自己的实现中，第一次在inorder中找root 1 时，没有出现问题；
>
> <img src="https://gitee.com/hanhandehanpi/cloudimage/raw/master/image-20241213224523264.png" alt="image-20241213224523264" style="zoom:33%;" />
>
> 第二次找root 2时，会发现遍历了一整遍数组
>
> <img src="https://gitee.com/hanhandehanpi/cloudimage/raw/master/image-20241213224725637.png" alt="image-20241213224725637" style="zoom:33%;" />
>
> 然后其中找root 3 时确实仅有一次遍历，但是会发现当找root 4时会出现异常，这个时候在中序遍历中又按照5、7、6、4的这样一个顺序遍历了一遍数组，这个时候就出现重复遍历了。
>
> 同样的，root 5没有异常，但是6又出现异常，重复遍历了一遍7、6。当这种结构的树更庞大时，这种重复遍历的次数是相当多且没有必要的，这个时候可以通过引入一个Map来记录数组中被遍历过的node，并将其位置记录在Map中，这样可以有效减少一些特殊情况下的遍历次数。

**官方题解——迭代**：

```
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if (preorder == null || preorder.length == 0) {
            return null;
        }
        TreeNode root = new TreeNode(preorder[0]);
        Deque<TreeNode> stack = new LinkedList<TreeNode>();
        stack.push(root);
        int inorderIndex = 0;
        for (int i = 1; i < preorder.length; i++) {
            int preorderVal = preorder[i];
            TreeNode node = stack.peek();
            if (node.val != inorder[inorderIndex]) {
                node.left = new TreeNode(preorderVal);
                stack.push(node.left);
            } else {
                while (!stack.isEmpty() && stack.peek().val == inorder[inorderIndex]) {
                    node = stack.pop();
                    inorderIndex++;
                }
                node.right = new TreeNode(preorderVal);
                stack.push(node.right);
            }
        }
        return root;
    }
}
```

> 核心思路：
> 是一个相当巧妙的实现，核心在于index与栈顶元素的检查对比，
>
> 当index指向的元素与栈顶元素不同时，将该元素入栈；
>
> 但发现index指针与栈顶元素相同时，那么不断弹出栈元素同时将index右移，直到不同时，这代表上一次弹出的元素是当前入栈指针指向元素的父节点。
>
> <img src="https://gitee.com/hanhandehanpi/cloudimage/raw/master/image-20241213233722474.png" alt="image-20241213233722474" style="zoom:33%;" />

## 49	2024-12-14

### （1）路径总和——437

**个人题解**：

```

```

> 核心思路
>
> 自己的实现总是会存在路径重复。

**官方题解——深度优先搜索**：

```java
class Solution {
    public int pathSum(TreeNode root, long targetSum) {
        if (root == null) {
            return 0;
        }

        int ret = rootSum(root, targetSum);
        ret += pathSum(root.left, targetSum);
        ret += pathSum(root.right, targetSum);
        return ret;
    }

    public int rootSum(TreeNode root, long targetSum) {
        int ret = 0;

        if (root == null) {
            return 0;
        }
        int val = root.val;
        if (val == targetSum) {
            ret++;
        } 

        ret += rootSum(root.left, targetSum - val);
        ret += rootSum(root.right, targetSum - val);
        return ret;
    }
}
```

> 核心思路
>
> 实现一个rootSum函数，其核心逻辑是返回**以当前节点为起点的路径**中和为targetSum的路径数量。

**官方题解——前缀和⭐**：

```java
class Solution {
    public int pathSum(TreeNode root, int targetSum) {
        Map<Long, Integer> prefix = new HashMap<Long, Integer>();
        prefix.put(0L, 1);
        return dfs(root, prefix, 0, targetSum);
    }

    public int dfs(TreeNode root, Map<Long, Integer> prefix, long curr, int targetSum) {
        if (root == null) {
            return 0;
        }

        int ret = 0;
        curr += root.val;

        ret = prefix.getOrDefault(curr - targetSum, 0);
        prefix.put(curr, prefix.getOrDefault(curr, 0) + 1);
        ret += dfs(root.left, prefix, curr, targetSum);
        ret += dfs(root.right, prefix, curr, targetSum);
        prefix.put(curr, prefix.getOrDefault(curr, 0) - 1);

        return ret;
    }
}
```

> 核心思路：
>
> 需要仔细的看官方的题解说明。
>
> 其中答案中使用到的Map是一个<long, Integer>的对应，其中前者long代表的是记录的前缀和，后者Integer类型记录的是该前缀和值出现的次数。
>
> 在这里面的dfs的返回结果，返回的最终结果是一个累加和，最终返回的结果代表以root作为根节点的树中，满足路径和为target的路径数量。其中共出现三次对ret结果的变动：
>
> ```java
>     ret = prefix.getOrDefault(curr - targetSum, 0);  
>     // 第一次变动，这个计算的结果是，代表当前节点作为前缀和的终点，从Map中获取此前出现过的满足前缀和=curr - targetSum的次数，即当前节点作为路径终点的路径中，满足和为target的路径数量。
>     ret += dfs(root.left, prefix, curr, targetSum);
> 		// 第二次变动，与dfs(root,.....)中对root节点的处理同理，计算以root.left为根节点的树中，满足路径和为target的路径数量。
>     ret += dfs(root.right, prefix, curr, targetSum);
> 		// 第三次变动，与dfs(root,.....)中对root节点的处理同理，计算以root.right为根节点的树中，满足路径和为target的路径数量。
> ```
>
> 因此，在与dfs(root,.....)中，将第一、二、三次计算的结果相加，最终得到以root节点作为根节点的树中，满足路径和为target的路径数量。
>
> 此外，其中对prefix的映射表的处理也值得思考：
>
> ```java
>         prefix.put(curr, prefix.getOrDefault(curr, 0) + 1);
> 				// ....
>         prefix.put(curr, prefix.getOrDefault(curr, 0) - 1);
> ```
>
> 在prefix映射表中，记录的是当前节点对prefix表的前缀和记录贡献，即将当前节点为终点的前缀和记录在prefix表中，在其子节点root.left以及root.right中因此可以查看到该前缀和的记录。
>
> 但当对子节点的处理完成后，需要返回DFS的上一级时，需要抹除掉该前缀和的记录，避免对上一级的处理造成影响。

## 50	2024-12-15

### （1）二叉树的最近公共祖先——236

**个人题解**：

```

```

> 核心思路
>
> 第一种错误实现：
>
> ```java
> class Solution {
>     Integer rankQ, rankP;
>     TreeNode targetP, targetQ;
>     Map<Integer, TreeNode> map = new HashMap<>();
>     public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
>         targetP = p;
>         targetQ = q;
>         levelOrderFillMap(root, 1);
>         TreeNode result =  map.get(findCommonAncestor(rankP, rankQ));
>         return result;
>     }
>     public void levelOrderFillMap(TreeNode root, int currRank) {
>         if (root == null) {
>             return;
>         }else {
>             map.put(currRank, root);
>         };
>         if (root == targetP) {
>             rankP = currRank;
>         }
>         if (root == targetQ) {
>             rankQ = currRank;
>         }
>         levelOrderFillMap(root.left, 2 * currRank);
>         levelOrderFillMap(root.right, 2 * currRank + 1);
>     }
>     public int findCommonAncestor(Integer big, Integer small) {
>         if (small > big){
>             Integer temp = big;
>             big = small;;
>             small = temp;
>         }
>         if (2 * small == big || 2 * small + 1 == big){
>             return small;
>         }
>         else return findCommonAncestor(big / 2, small);
>     }
> }
> ```
>
> 错误原因可能在于溢出，对于测试示例中，错误如下所示：
>
> ![image-20241215125555607](https://gitee.com/hanhandehanpi/cloudimage/raw/master/image-20241215125555607.png)
>
> 但是并不应该是错位原因，而是因为到了9999这种，此时map中的Integer就已经是2^9999了。

**官方题解**：

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) {
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left != null && right != null) {
            return root;
        }
        return left != null ? left : right;
    }
}
```

> 核心思路
>
> 核心逻辑就是判断当前的root节点的左右子树中是否存在p或者q
>
> 如果存在则将该树标记为`存在树`，在代码中反映的是返回这个root节点。
>
> 当一个节点的左右子树都是`存在树`，那这个节点毫无疑问就是p、q的最近公共祖先节点。体现在
>
> ```java
>         if (left != null && right != null) {
>             return root;
>         }
> ```
>
> 如果该节点只有左边是存在树，右边不是存在树，那么代表p、q都存在于左子树中，则将左子树的lowestCommonAncestor()结果返回。层层遍历，直到出现一个节点满足左子树与右子树都是存在树时，返回这个节点。
>
> 其中
>
> ```java
>         if (root == null || root == p || root == q) {
>             return root;
>         }
> ```
>
> 实现比较巧妙，恰好能满足即便p、q本身作为对方的祖先节点的情况。

## 51	2024-12-16

### （1）岛屿数量——200

**个人题解**：

```java
class Solution {
    public int numIslands(char[][] grid) {
        int result = 0;
        boolean[][] tip = new boolean[grid.length][grid[0].length];
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == '1' && !tip[i][j]) {
                    result++;
                    checkConnect(i, j, grid, tip);
                }
            }
        }
        return result;
    }

    public void checkConnect(int i, int j, char[][] grid, boolean[][] tip) {
        tip[i][j] = true;
//        检查上边
        if (i > 0 && grid[i - 1][j] == '1' && !tip[i - 1][j]) {
            checkConnect(i - 1, j, grid, tip);
        }
//        检查左边
        if (j > 0 && grid[i][j - 1] == '1' && !tip[i][j - 1]) {
            checkConnect(i, j - 1, grid, tip);
        }
//        检查下边
        if (i < grid.length - 1 && grid[i + 1][j] == '1' && !tip[i + 1][j]) {
            checkConnect(i + 1, j, grid, tip);
        }
//        检查右边
        if (j < grid[0].length - 1 && grid[i][j + 1] == '1' && !tip[i][j + 1]) {
            checkConnect(i, j + 1, grid, tip);
        }
    }
}
```

> 核心思路
>
> 最开始的想法比较简单，遍历所有的点，当为1的时候进行判断，如果这个点的左边与上边都是1，则这个点是被之前遍历过的点所连接的点，如果这个点的左边与上边都不是1，那么这个点是新的岛屿，result++。
>
> 但是这个问题其实当碰到下述的情况就出现了问题：
>
> <img src="https://gitee.com/hanhandehanpi/cloudimage/raw/master/image-20241216105826531.png" alt="image-20241216105826531" style="zoom:33%;" />
>
> 对于其中的`grid[2][0]`，在判断逻辑中会显示其为“新的岛屿”，但这很显然并不是。
>
> 后来想了想还是得从每个点进行遍历并找出其相邻点并进行标记，也就是个人题解中记录的方案。其实这种方式在整体数组中岛屿面积比较小的时候计算时间还是比较小的，但是当岛屿面积特别大的时候，相当于进行了第二次遍历，且第二次遍历是无效的。

**官方题解——深度优先搜索**：

```java
class Solution {
    void dfs(char[][] grid, int r, int c) {
        int nr = grid.length;
        int nc = grid[0].length;

        if (r < 0 || c < 0 || r >= nr || c >= nc || grid[r][c] == '0') {
            return;
        }

        grid[r][c] = '0';
        dfs(grid, r - 1, c);
        dfs(grid, r + 1, c);
        dfs(grid, r, c - 1);
        dfs(grid, r, c + 1);
    }

    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }

        int nr = grid.length;
        int nc = grid[0].length;
        int num_islands = 0;
        for (int r = 0; r < nr; ++r) {
            for (int c = 0; c < nc; ++c) {
                if (grid[r][c] == '1') {
                    ++num_islands;
                    dfs(grid, r, c);
                }
            }
        }

        return num_islands;
    }
}
```

> 核心思路
>
> 这种方法是修改原本数组的一种实现方式。但同时也比较的节省空间。
>
> 当发现一个数是1时，从这个节点开始进行深度优先遍历（广度优先遍历也行），将所有相邻的为1 的节点全部设置为0。
>
> 这样每次在每次遍历数组的时候，开始进行深度优先遍历时就必然是一个与之前岛屿未相邻的的新岛屿了。

**官方题解——并查集**：

```java
class Solution {
    class UnionFind {
        int count;
        int[] parent;
        int[] rank;

        public UnionFind(char[][] grid) {
            count = 0;
            int m = grid.length;
            int n = grid[0].length;
            parent = new int[m * n];
            rank = new int[m * n];
            for (int i = 0; i < m; ++i) {
                for (int j = 0; j < n; ++j) {
                    if (grid[i][j] == '1') {
                        parent[i * n + j] = i * n + j;
                        ++count;
                    }
                    rank[i * n + j] = 0;
                }
            }
        }

        public int find(int i) {
            if (parent[i] != i) parent[i] = find(parent[i]);
            return parent[i];
        }

        public void union(int x, int y) {
            int rootx = find(x);
            int rooty = find(y);
            if (rootx != rooty) {
                if (rank[rootx] > rank[rooty]) {
                    parent[rooty] = rootx;
                } else if (rank[rootx] < rank[rooty]) {
                    parent[rootx] = rooty;
                } else {
                    parent[rooty] = rootx;
                    rank[rootx] += 1;
                }
                --count;
            }
        }

        public int getCount() {
            return count;
        }
    }

    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }

        int nr = grid.length;
        int nc = grid[0].length;
        int num_islands = 0;
        UnionFind uf = new UnionFind(grid);
        for (int r = 0; r < nr; ++r) {
            for (int c = 0; c < nc; ++c) {
                if (grid[r][c] == '1') {
                    grid[r][c] = '0';
                    if (r - 1 >= 0 && grid[r-1][c] == '1') {
                        uf.union(r * nc + c, (r-1) * nc + c);
                    }
                    if (r + 1 < nr && grid[r+1][c] == '1') {
                        uf.union(r * nc + c, (r+1) * nc + c);
                    }
                    if (c - 1 >= 0 && grid[r][c-1] == '1') {
                        uf.union(r * nc + c, r * nc + c - 1);
                    }
                    if (c + 1 < nc && grid[r][c+1] == '1') {
                        uf.union(r * nc + c, r * nc + c + 1);
                    }
                }
            }
        }

        return uf.getCount();
    }
}
```

> 核心思路：
>



## 52	2024-12-17

### （1）腐烂的橘子——994

**个人题解**：

```java
class Solution {
    /**
     * 首先建立一个新的表，用来记录所有的点被腐蚀的最短时间
     * 更新这个表的数据的方式，通过对所有的最开始就被腐蚀的点进行遍历，遍历这些点对其所有处于同一“岛屿”的点的腐蚀时间。
     * 当一个点存在复数次被腐蚀时，选择其中较小的时间作为这个点的最终被腐蚀时间。
     * 最终再一次遍历这个表，选取其中数字最大的腐蚀时间作为结果；
     * 如果最终遍历时，如果存在点的腐蚀时间是0，那么返回-1，代表存在点不能被腐蚀。
     * 对于此题中，关于空间的优化方式：
     *  还是在现有的grid表上进行处理，将其中所有的点的数额-2；
     *      其中0变成-2，代表这个点不是橘子，不能被腐蚀；
     *      其中1变成-1，代表这个点是橘子，但还没被腐蚀；
     *      其中2变成0，代表这个点是腐化的橘子，即将开始进行腐蚀周边的点。
     *
     * 为了优化处理速度，可以不对grid表进行额外的处理，这样就不需要新表了，也不需要额外计算，但是如果点在第一轮就被腐化的话，这个点的数额会被记录为3，而不是显示的代表回合的1这个数。
     * 这样的话，最终对表中是否存在未被腐蚀的点的检查方式是：判断其中是否还存在1。
     * 在第一轮就腐蚀的点的数额会被记录为3，第二轮是4。因此最终如果全被腐蚀的话，最终的结果需要-2。（包括最开始就是2的初始腐蚀点）
     */
    public int orangesRotting(int[][] grid) {
        int result = Integer.MIN_VALUE;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == 2) {
                    corrodeRecursion(i ,j, grid, 1);
                }
            }
        }
//        第二次遍历，检查所有的点，如果存在点是1.则返回-1，否在返回最大值
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == 1) {
                    return -1;
                }
                result = grid[i][j] > result ? grid[i][j] : result;
            }
        }
//				返回结果这里需要对数组全是0的时候做一个额外处理。      
        return result == 0 ? 0 : result - 2;
    }
    public void corrodeRecursion(int i,  int j, int[][] grid, int corrodeRound) {
//        检查上方

//        如果上边是橘子节点，且
//        如果上边的节点还没有被腐蚀过，或者是
//        上边的橘子节点当前被记录的腐蚀时长比corrodeRound+2还要大，则更新那个橘子节点的腐蚀时间为更小的腐蚀时间。
        if (i > 0 && (grid[i - 1][j] == 1 || grid[i - 1][j] > corrodeRound + 2) ){
            grid[i - 1][j] = corrodeRound + 2;
            corrodeRecursion(i - 1, j, grid, corrodeRound + 1);
        }
//        检查下方
        if (i < grid.length - 1 && (grid[i + 1][j] == 1 || grid[i + 1][j] > corrodeRound + 2) ){
            grid[i + 1][j] = corrodeRound + 2;
            corrodeRecursion(i + 1, j, grid, corrodeRound + 1);
        }
//        检查左方
        if (j > 0 && (grid[i][j - 1] == 1 || grid[i][j - 1] > corrodeRound + 2) ){
            grid[i][j - 1] = corrodeRound + 2;
            corrodeRecursion(i, j - 1, grid, corrodeRound + 1);
        }
//        检查右方
        if (j < grid[0].length - 1 && (grid[i][j + 1] == 1 || grid[i][j + 1] > corrodeRound + 2) ){
            grid[i][j + 1] = corrodeRound + 2;
            corrodeRecursion(i, j + 1, grid, corrodeRound + 1);
        }
    }
}
```

> 核心思路
>
> 核心思路全在注释里了。
>
> 这道题解答的方案还算不错，执行用时以及消耗内存都属于比较优秀的实现。
>
> ![image-20241217221736589](https://gitee.com/hanhandehanpi/cloudimage/raw/master/image-20241217221736589.png)

**官方题解**：

```
class Solution {
    int[] dr = new int[]{-1, 0, 1, 0};
    int[] dc = new int[]{0, -1, 0, 1};

    public int orangesRotting(int[][] grid) {
        int R = grid.length, C = grid[0].length;
        Queue<Integer> queue = new ArrayDeque<Integer>();
        Map<Integer, Integer> depth = new HashMap<Integer, Integer>();
        for (int r = 0; r < R; ++r) {
            for (int c = 0; c < C; ++c) {
                if (grid[r][c] == 2) {
                    int code = r * C + c;
                    queue.add(code);
                    depth.put(code, 0);
                }
            }
        }
        int ans = 0;
        while (!queue.isEmpty()) {
            int code = queue.remove();
            int r = code / C, c = code % C;
            for (int k = 0; k < 4; ++k) {
                int nr = r + dr[k];
                int nc = c + dc[k];
                if (0 <= nr && nr < R && 0 <= nc && nc < C && grid[nr][nc] == 1) {
                    grid[nr][nc] = 2;
                    int ncode = nr * C + nc;
                    queue.add(ncode);
                    depth.put(ncode, depth.get(code) + 1);
                    ans = depth.get(ncode);
                }
            }
        }
        for (int[] row: grid) {
            for (int v: row) {
                if (v == 1) {
                    return -1;
                }
            }
        }
        return ans;
    }
}
```

> 核心思路
>
> 比较抽象且没必要。

## 53	2024-12-18

### （1）课程表——207

**个人题解**：

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        Map<Integer, Set<Integer>> map = new HashMap<>();
        for (int [] value : prerequisites) {
            if (!map.containsKey(value[0])) {
                map.put(value[0], new HashSet<>());
            }
            map.get(value[0]).add(value[1]);
        }
        boolean result = true;
        for (int i : map.keySet()) {
            Set<Integer> set = new HashSet<>();
            set.add(i);
            if (!noLoopCheck(set, map.getOrDefault(i, new HashSet<>()), map)){
                result = false;
                break;
            }
        }
        return result;
    }

    /**
     * 当返回的结果是true时，代表没有loop
     * false代表存在loop
     * @param targetSet
     * @param map
     * @return
     */
    public boolean noLoopCheck(Set<Integer> targetSet, Set<Integer> toCheckSet, Map<Integer, Set<Integer>> map) {
        if (toCheckSet.size() == 0){
            return true;
        }
        for (int temp : targetSet) {
            if (toCheckSet.contains(temp)) {
                return false;
            }
        }
        boolean result = true;
        for (int i : toCheckSet) {
            targetSet.add(i);
            if (!noLoopCheck(targetSet, map.getOrDefault(i, new HashSet<>()), map)) {
                result = false;
                break;
            }
            targetSet.remove(i);
        }
        return result;
    }
}
```

> 核心思路
>
> 这个实现方法超时了，最开始想法很美好，不过发现漏洞一堆，漏洞越填补越不合理。

**官方题解**：

```java
class Solution {
    List<List<Integer>> edges;
    int[] visited;
    boolean valid = true;

    public boolean canFinish(int numCourses, int[][] prerequisites) {
        edges = new ArrayList<List<Integer>>();
        for (int i = 0; i < numCourses; ++i) {
            edges.add(new ArrayList<Integer>());
        }
        visited = new int[numCourses];
        for (int[] info : prerequisites) {
            edges.get(info[1]).add(info[0]);
        }
        for (int i = 0; i < numCourses && valid; ++i) {
            if (visited[i] == 0) {
                dfs(i);
            }
        }
        return valid;
    }

    public void dfs(int u) {
        visited[u] = 1;
        for (int v: edges.get(u)) {
            if (visited[v] == 0) {
                dfs(v);
                if (!valid) {
                    return;
                }
            } else if (visited[v] == 1) {
                valid = false;
                return;
            }
        }
        visited[u] = 2;
    }
}
```

> 核心思路
>
> 本质上是去寻找是否存在一个环，而与环有关的一个概念是：是否存在**拓扑排序**，如果一个有向图中至少存在一个拓扑排序，那么这个图就是一个无环图。于是本题的目的就变成了去寻找一个存在的拓扑排序。
>
> 对于图中的任何一个节点，存在三种状态：
>
> - 未搜索
> - 搜索中
> - 搜索过
>
> 每次开始从任意一个未搜索的节点开始进行；
>
> 如果碰到搜索中的节点，那么代表出现回路；
>
> 碰到搜索过的点，不需要进行额外的判断。
>
> ```
> 我们将当前搜索的节点 u 标记为「搜索中」，遍历该节点的每一个相邻节点 v：
> 
> 如果 v 为「未搜索」，那么我们开始搜索 v，待搜索完成回溯到 u；
> 
> 如果 v 为「搜索中」，那么我们就找到了图中的一个环，因此是不存在拓扑排序的；
> 
> 如果 v 为「已完成」，那么说明 v 已经在栈中了，而 u 还不在栈中，因此 u 无论何时入栈都不会影响到 (u,v) 之前的拓扑关系，以及不用进行任何操作。
> 
> 当 u 的所有相邻节点都为「已完成」时，我们将 u 放入栈中，并将其标记为「已完成」。
> ```
>

## 54	2024-12-19

### （1）实现Trie(前缀树)——208

**个人题解**：

```java
class Trie {
    Set<String> stringSet = new HashSet<>();
    public Trie() {

    }
    public void insert(String word) {
        stringSet.add(word);
    }

    public boolean search(String word) {
        return stringSet.contains(word);
    }

    public boolean startsWith(String prefix) {
        boolean flag = false;
        for (String s : stringSet) {
            int i = 0;
            for (; i < prefix.length() && i < s.length(); i++) {
                if (prefix.charAt(i) != s.charAt(i)) {
                    break;
                }
            }
            if (i == prefix.length()) {
                flag = true;
                break;
            }
        }
        return flag;
    }
}
```

> 核心思路
>
> 没有做任何的时间复杂度优化，时间开销相当的大。

**官方题解⭐**：

```java
class Trie {
    private Trie[] children;
    private boolean isEnd;

    public Trie() {
        children = new Trie[26];
        isEnd = false;
    }

    public void insert(String word) {
        Trie node = this;
        for (int i = 0; i < word.length(); i++) {
            char ch = word.charAt(i);
            int index = ch - 'a';
            if (node.children[index] == null) {
                node.children[index] = new Trie();
            }
            node = node.children[index];
        }
        node.isEnd = true;
    }

    public boolean search(String word) {
        Trie node = searchPrefix(word);
        return node != null && node.isEnd;
    }

    public boolean startsWith(String prefix) {
        return searchPrefix(prefix) != null;
    }

    private Trie searchPrefix(String prefix) {
        Trie node = this;
        for (int i = 0; i < prefix.length(); i++) {
            char ch = prefix.charAt(i);
            int index = ch - 'a';
            if (node.children[index] == null) {
                return null;
            }
            node = node.children[index];
        }
        return node;
    }
}
```

> 核心思路
>
> 官方的这个实现方式才是贴切于其原本的目的。
>
> 核心逻辑是在Trie中添加的两个属性：
>
> ```java
>     private Trie[] children;
>     private boolean isEnd;
> ```
>
> 其中children代表了二十六字母中的二十六中指向方向。
>
> 其中isEnd用来表示这个位置是否为终末的节点。这个isEnd的设置本身也非常的巧妙，其并不是代表了这棵树的终点，而是代表了这个位置是否是某个字符串的结算点。
>
> 该方式实现的结构是一种非常优秀的实现。
>
> 以下是当插入`apple`时的目标结构：
>
> ![image-20241219173737022](https://gitee.com/hanhandehanpi/cloudimage/raw/master/image-20241219173737022.png)

## 55	2024-12-20

### （1）子集——78

**个人题解**：

```java
class Solution {
    List<List<Integer>> result;
    public List<List<Integer>> subsets(int[] nums) {
        result = new ArrayList<>();
        insertList(new ArrayList<>(), 0, nums);
        return result;
    }
    public void insertList(List<Integer> pastList, int currIndex, int [] nums){
        List<Integer> currListAdd = new ArrayList<>(pastList);
        List<Integer> currListNotAdd = new ArrayList<>(pastList);
        currListAdd.add(nums[currIndex]);

        if (currIndex == nums.length - 1 ){
            result.add(currListAdd);
            result.add(currListNotAdd);
            return;
        }

        insertList(currListAdd, currIndex + 1, nums);
        insertList(currListNotAdd, currIndex + 1, nums);

    }
}
```

> 核心思路
>
> 花了接近一个小时，还是花了太多时间了。
>
> 思考了几种解决方式都不太像是正确答案。
>
> 其中有一种试图去推理分析每个数组中的数所在的次序 与 其在最终的resultList中的落位的对应关系，如下图所示。
>
> <img src="https://gitee.com/hanhandehanpi/cloudimage/raw/master/image-20241220211136293.png" alt="image-20241220211136293" style="zoom:33%;" />
>
> 但是发现这个对应关系除了第一个比较好找之外，后边的对应关系就不简单了，不是很容易用一个数学关系去推导。【但是应该是有某种函数对应关系，能计算出这个对应关系的。】
>
>  
>
> 另外， 原本还构想了一种方法，比较的贴近当前的这个个人题解了。
>
> 核心思路是通过`currListIndex`指针记录父亲`List<Integer>`的List内容，将其内容进行拷贝，将其内容粘贴到当前resultList的末尾，需要粘贴两次，在其中的一个中添加当前`currNumsIndex`指向的数。然后将`currListIndex`设置为末尾的位置并进行下一次递归。
>
> 这种方式比现在的这个实现更节省空间，不需要每次都传递一个的`List<Integer> currList`。
>
> 但是最终毁在了首部的处理。因此采用了一个妥协的方式，即，每次都传递一个currList。

**官方题解——迭代法实现子集枚举**：

```java
class Solution {
    List<Integer> t = new ArrayList<Integer>();
    List<List<Integer>> ans = new ArrayList<List<Integer>>();

    public List<List<Integer>> subsets(int[] nums) {
        int n = nums.length;
        for (int mask = 0; mask < (1 << n); ++mask) {
            t.clear();
            for (int i = 0; i < n; ++i) {
              	// 个人注释：优秀的位运算判断
                if ((mask & (1 << i)) != 0) {
                    t.add(nums[i]);
                }
            }
            ans.add(new ArrayList<Integer>(t));
        }
        return ans;
    }
}
```

> 核心思路
>
> 这种实现利用了位运算。其中mask是一个int类型的数，其进行从1~2^n的遍历。比较关键的代码即
>
> ```java
>                 if ((mask & (1 << i)) != 0) {
> ```
>
> 这段代码判断了当前进行中的位运算中，该位的数是否为1。
>
> 如果是1的话，就代表这位数需要添加进当前的list中。

**官方题解——递归法实现子集枚举**：

```java
class Solution {
    List<Integer> t = new ArrayList<Integer>();
    List<List<Integer>> ans = new ArrayList<List<Integer>>();

    public List<List<Integer>> subsets(int[] nums) {
        dfs(0, nums);
        return ans;
    }

    public void dfs(int cur, int[] nums) {
        if (cur == nums.length) {
            ans.add(new ArrayList<Integer>(t));
            return;
        }
        t.add(nums[cur]);
        dfs(cur + 1, nums);
        t.remove(t.size() - 1);
        dfs(cur + 1, nums);
    }
}
```

> 核心思路：
>
> 是对自己的实现的一种优化简洁版本，核心思路一致。

**评论区题解——非常好的简化版本**⭐：

```c++
本质是动态规划思想，属于较简单的线性动规。

可以这么表示，dp[i]表示前i个数的解集，dp[i] = dp[i - 1] + collections(i)。其中，collections(i)表示把dp[i-1]的所有子集都加上第i个数形成的子集。

【具体操作】

因为nums大小不为0，故解集中一定有空集。令解集一开始只有空集，然后遍历nums，每遍历一个数字，拷贝解集中的所有子集，将该数字与这些拷贝组成新的子集再放入解集中即可。时间复杂度为O(n^2)。

例如[1,2,3]，一开始解集为[[]]，表示只有一个空集。
遍历到1时，依次拷贝解集中所有子集，只有[]，把1加入拷贝的子集中得到[1]，然后加回解集中。此时解集为[[], [1]]。
遍历到2时，依次拷贝解集中所有子集，有[], [1]，把2加入拷贝的子集得到[2], [1, 2]，然后加回解集中。此时解集为[[], [1], [2], [1, 2]]。
遍历到3时，依次拷贝解集中所有子集，有[], [1], [2], [1, 2]，把3加入拷贝的子集得到[3], [1, 3], [2, 3], [1, 2, 3]，然后加回解集中。此时解集为[[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]]。
class Solution {

    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> lists = new ArrayList<>(); // 解集
        lists.add(new ArrayList<Integer>()); // 首先将空集加入解集中
        for(int i = 0; i < nums.length; i++){
            int size = lists.size(); // 当前子集数
            for(int j = 0; j < size; j++){ 
                List<Integer> newList = new ArrayList<>(lists.get(j));// 拷贝所有子集
                newList.add(nums[i]); // 向拷贝的子集中加入当前数形成新的子集
                lists.add(newList); // 向lists中加入新子集
            }
        }
        return lists;
    }
}
```

> 核心思路：
> 核心思路都在前边的解释，是一种比较优秀的动态规划。其中最核心的一句：
>
> “可以这么表示，dp[i]表示前i个数的解集，dp[i] = dp[i - 1] + collections(i)。其中，collections(i)表示把dp[i-1]的所有子集都加上第i个数形成的子集。”

## 56	2024-12-21

### （1）全排列——46

**个人题解**：

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> initlist = new ArrayList<>();
        initlist.add(nums[0]);
        res.add(initlist);
        for (int i = 1; i < nums.length; i++) {
            int resSize = res.size();
            for (int j = 0; j < resSize; j++) {
                List<Integer> currList = res.get(0);
                res.remove(currList);
                for (int p = 0; p <= currList.size(); p++){
                    List<Integer> newList = new ArrayList<>(currList);
                    newList.add(p, nums[i]);
                    res.add(newList);
                }
            }
        }
        return res;
    }
}
```

> 核心思路
>
> 核心思路是动态规划，新nums值`nums[i]`的处理`dp[i]`等于对之前的结果集`dp[i-1]`的每个结果list中遍历自己可能存在的所有位置。下图中的黑框标注的是结果集，红框标志的是对于这个结果集的每个可能插入位置。
>
> ![image-20241221233200880](https://gitee.com/hanhandehanpi/cloudimage/raw/master/image-20241221233200880.png)
>
> <img src="https://gitee.com/hanhandehanpi/cloudimage/raw/master/image-20241221233004865.png" alt="image-20241221233004865" style="zoom:33%;" />

**官方题解**：

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();

        List<Integer> output = new ArrayList<Integer>();
        for (int num : nums) {
            output.add(num);
        }

        int n = nums.length;
        backtrack(n, output, res, 0);
        return res;
    }

    public void backtrack(int n, List<Integer> output, List<List<Integer>> res, int first) {
        // 所有数都填完了
        if (first == n) {
            res.add(new ArrayList<Integer>(output));
        }
        for (int i = first; i < n; i++) {
            // 动态维护数组
            Collections.swap(output, first, i);
            // 继续递归填下一个数
            backtrack(n, output, res, first + 1);
            // 撤销操作
            Collections.swap(output, first, i);
        }
    }
}
```

> 核心思路
>
> 记忆自己的实现方式。

## 57	2024-12-22

### （1）电话号码的字母组合——17

**个人题解**：

```java
class Solution {
    public List<String> letterCombinations(String digits) {
        List<String> res = new ArrayList<>();
        if (digits == null || digits.length() == 0) {
            return res;
        }
        res.add("");
        for (Character currCharDigits : digits.toCharArray()) {
            int resSize = res.size();
            String newString =  numsToString(currCharDigits);
            for (int i = 0; i < resSize; i++) {
                String currString = res.get(0);
                res.remove(0);
                for (Character currCharNewString : newString.toCharArray()) {
                    res.add(currString + currCharNewString);
                }
            }
        }
        return res;
    }
    public String numsToString(Character character){
        switch (character){
            case '2':
                return "abc";
            case '3':
                return "def";
            case '4':
                return "ghi";
            case '5':
                return "jkl";
            case '6':
                return "mno";
            case '7':
                return "pqrs";
            case '8':
                return "tuv";
            case '9':
                return "wxyz";
            default:
                return null;
        }
    }
}
```

> 核心思路
>
> 这个题和前几个动态规划的题都非常的相似，核心思路都是将目标拆分为
>
> ![image-20241221233200880](https://gitee.com/hanhandehanpi/cloudimage/raw/master/image-20241221233200880.png)
>
> 将问题拆成子项。典型的汉诺塔类型动态规划思考。

**官方题解**：

```java
class Solution {
    public List<String> letterCombinations(String digits) {
        List<String> combinations = new ArrayList<String>();
        if (digits.length() == 0) {
            return combinations;
        }
        Map<Character, String> phoneMap = new HashMap<Character, String>() {{
            put('2', "abc");
            put('3', "def");
            put('4', "ghi");
            put('5', "jkl");
            put('6', "mno");
            put('7', "pqrs");
            put('8', "tuv");
            put('9', "wxyz");
        }};
        backtrack(combinations, phoneMap, digits, 0, new StringBuffer());
        return combinations;
    }

    public void backtrack(List<String> combinations, Map<Character, String> phoneMap, String digits, int index, StringBuffer combination) {
        if (index == digits.length()) {
            combinations.add(combination.toString());
        } else {
            char digit = digits.charAt(index);
            String letters = phoneMap.get(digit);
            int lettersCount = letters.length();
            for (int i = 0; i < lettersCount; i++) {
                combination.append(letters.charAt(i));
                backtrack(combinations, phoneMap, digits, index + 1, combination);
                combination.deleteCharAt(index);
            }
        }
    }
}
```

> 核心思路
>
> 回溯思想，记忆自己的实现方式就行。

## 58	2024-12-23

### （1）括号生成——22

**个人题解**：

```

```

> 核心思路
>
>  不对劲，思考的解法相当的复杂，不太可能是题解。

**官方题解——暴力法**：

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> combinations = new ArrayList<String>();
        generateAll(new char[2 * n], 0, combinations);
        return combinations;
    }

    public void generateAll(char[] current, int pos, List<String> result) {
        if (pos == current.length) {
            if (valid(current)) {
                result.add(new String(current));
            }
        } else {
            current[pos] = '(';
            generateAll(current, pos + 1, result);
            current[pos] = ')';
            generateAll(current, pos + 1, result);
        }
    }

    public boolean valid(char[] current) {
        int balance = 0;
        for (char c: current) {
            if (c == '(') {
                ++balance;
            } else {
                --balance;
            }
            if (balance < 0) {
                return false;
            }
        }
        return balance == 0;
    }
}
```

> 核心思路
>
> 将所有的可能结果进行一次遍历，每次一遍历之后，将遍历之后的结果进行一次valid验证。
>
> 其中对括号是否符合标准的判断方式值得记忆。即：
>
> 对于一个以char[]方式记录的括号数组进行有效验证⭐。
>
> 其中很好的找到了括号组合是否有效规律的数字化表示，特别是当balance<0时，返回false。
>
> ```java
>     public boolean valid(char[] current) {
>         int balance = 0;
>         for (char c: current) {
>             if (c == '(') {
>                 ++balance;
>             } else {
>                 --balance;
>             }
>             if (balance < 0) {
>                 return false;
>             }
>         }
>         return balance == 0;
>     }
> ```

**官方题解——回溯法**：

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList<String>();
        backtrack(ans, new StringBuilder(), 0, 0, n);
        return ans;
    }

    public void backtrack(List<String> ans, StringBuilder cur, int open, int close, int max) {
        if (cur.length() == max * 2) {
            ans.add(cur.toString());
            return;
        }
        if (open < max) {
            cur.append('(');
            backtrack(ans, cur, open + 1, close, max);
            cur.deleteCharAt(cur.length() - 1);
        }
        if (close < open) {
            cur.append(')');
            backtrack(ans, cur, open, close + 1, max);
            cur.deleteCharAt(cur.length() - 1);
        }
    }
}
```

> 核心思路：
> 是一种对方法一暴力解法的优化。
>
> 方法一中有很多可以进行的剪枝，比如当左括号的数量超过n，这个时候就无需再等到满了再判断。
>
> 还有就是任何时候，右括号的数量永远都是<=左括号的数量的。最终的核心代码就是：
>
> ```java
>     if (open < max) {
>         cur.append('(');
>         backtrack(ans, cur, open + 1, close, max);
>         cur.deleteCharAt(cur.length() - 1);
>     }
>     if (close < open) {
>         cur.append(')');
>         backtrack(ans, cur, open, close + 1, max);
>         cur.deleteCharAt(cur.length() - 1);
>     }
> ```
>
> 这个方法可以保证足够的剪枝，同时满足最终的cur是左括号与右括号数量相等的，且满足从头开始到任意一点的右括号计数不会超过左括号计数。
>
> 优化后个人的题解：
>
> ```java
> class Solution {
>     public List<String> generateParenthesis(int n) {
>         List<String> res = new ArrayList<>();
>         generate(res, n, n, "");
>         return res;
>     }
>     public void generate(List<String> res, int l, int r, String curr) {
>         if (l == 0 && r == 0) {
>             res.add(curr);
>             return;
>         }
>         if (l == r){
>             generate(res, l - 1, r, curr + "(");
>         }else {
>             if (l > 0) {
>                 generate(res, l - 1, r, curr + "(");
>             }
>             if (l < r) {
>                 generate(res, l, r - 1, curr + ")");
>             }
>         }
>     }
> }
> ```
>
> 这个方法中其实最后的
>
> ```java
>         if (l < r) {
>             generate(res, l, r - 1, curr + ")");
>         }
> ```
>
> 可以直接替换成
>
> ```java
>             generate(res, l, r - 1, curr + ")");
> ```
>
> 也就是不需要添加`l < r`的逻辑判断了。因为前边的对`l`的处理总是会优先进行，而每当其中r变小，到达`l == r`的触发条件时，又会使得`l`被处理，所以其实隐含满足了`l < r`的这个条件了。

**官方题解——按括号序列的长度递归**：

> 这个方法就相当的不友好了，非常反直觉。

## 59	2024-12-25

### （1）单词搜索——79

**个人题解**：

```java
class Solution {
    boolean [][] tip;
    boolean res = false;
    public boolean exist(char[][] board, String word) {
        tip = new boolean[board.length][board[0].length];
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[i].length; j++) {
                myExist(i, j, board, word, 0);
            }
        }
        return res;
    }
    public void myExist(int i, int j, char[][] board, String word, int index) {
        if (board[i][j] != word.charAt(index) || tip[i][j]) {
            return;
        }
        tip[i][j] = true;
        if (index == word.length() - 1) {
            res = true;
            return;
        }
//        lookup
        if (i > 0){
            myExist(i - 1, j, board, word, index + 1);
        }
//        lookdown
        if (i < board.length - 1){
            myExist(i + 1, j, board, word, index + 1);
        }
//        lookleft
        if (j > 0){
            myExist(i, j - 1, board, word, index + 1);
        }
//        lookright
        if (j < board[0].length - 1){
            myExist(i, j + 1, board, word, index + 1);
        }
        tip[i][j] = false;
    }
}
```

> 核心思路
>
> 遍历，没有什么特点。

**官方题解——回溯**：

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        int h = board.length, w = board[0].length;
        boolean[][] visited = new boolean[h][w];
        for (int i = 0; i < h; i++) {
            for (int j = 0; j < w; j++) {
                boolean flag = check(board, visited, i, j, word, 0);
                if (flag) {
                    return true;
                }
            }
        }
        return false;
    }

    public boolean check(char[][] board, boolean[][] visited, int i, int j, String s, int k) {
        if (board[i][j] != s.charAt(k)) {
            return false;
        } else if (k == s.length() - 1) {
            return true;
        }
        visited[i][j] = true;
        int[][] directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        boolean result = false;
        for (int[] dir : directions) {
            int newi = i + dir[0], newj = j + dir[1];
            if (newi >= 0 && newi < board.length && newj >= 0 && newj < board[0].length) {
                if (!visited[newi][newj]) {
                    boolean flag = check(board, visited, newi, newj, s, k + 1);
                    if (flag) {
                        result = true;
                        break;
                    }
                }
            }
        }
        visited[i][j] = false;
        return result;
    }
}
```

> 核心思路
>
> 几乎一模一样。

## 60	2024-12-30

### （1）分割回文串——131

**个人题解**：

```

```

> 核心思路
>
> 没有解决思路，想了几种方法但是又不太合理，遂直接查看官方题解。

**官方题解——回溯+动态规划预处理**：

```java
class Solution {
    boolean[][] f;
    List<List<String>> ret = new ArrayList<List<String>>();
    List<String> ans = new ArrayList<String>();
    int n;

    public List<List<String>> partition(String s) {
        n = s.length();
        f = new boolean[n][n];
        for (int i = 0; i < n; ++i) {
            Arrays.fill(f[i], true);
        }

        for (int i = n - 1; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                f[i][j] = (s.charAt(i) == s.charAt(j)) && f[i + 1][j - 1];
            }
        }

        dfs(s, 0);
        return ret;
    }

    public void dfs(String s, int i) {
        if (i == n) {
            ret.add(new ArrayList<String>(ans));
            return;
        }
        for (int j = i; j < n; ++j) {
            if (f[i][j]) {
                ans.add(s.substring(i, j + 1));
                dfs(s, j + 1);
                ans.remove(ans.size() - 1);
            }
        }
    }
}
```

> 核心思路
>
> 首先维护一个二维的数组`f`，`f`数组里边有用的信息仅仅是右上三角。里边记录的是从`i`到`j`，是否为回文串的判断记录。如，当从0~2所在的子串是回文串时，则`f[0][2] = true`。否则，`f[0][2] = false`。其中官方那个题解中对于f数组构建的描述不是相当人性化。
>
> 对于`f`数组的理解可以参考：
>
> ```java
> 官解 @力扣官方题解 又一次不说人话，我们先看下官解的骚操作。
> 
> 官解通过这句 for (int i = 0; i < n; ++i) Arrays.fill(f[i], true); 先把二维dp所有值都置为true，然后再通过递推式把对角线上侧右上三角区域应该为false的置为false。
> 这样做虽然对结果没影响，但蛮不讲理，不熟悉这个回文dp的同学看了会比较懵逼。不合理之处在于：
> 
> 首先，二维数组对角线下侧左下三角区域i > j，根据dp的定义，这是没有意义的区域，应当都是false (boolean数组默认已经是false了，不用处理)。所以官解一开始全置为true就是乱搞，虽然对于这题不影响最终的结果。
> 再来看递推，针对右上三角，从下往上倒着递推没错，但是官解的界完全没展现出递推是如何从「边界」开始的。
> 😓我尝试提供一版比较正常的dp。
> 
> // 边界1: 对角线，即单个字符，都是回文
> for(int i = 0; i < n; i++){ 
>     dp[i][i] = true;
> }
> // 边界2:对角线上侧紧邻斜线，即两个字符，判断是否相等，相等则为回文
> for(int i = 0; i < n - 1; i++){ 
>     dp[i][i + 1] = s.charAt(i) == s.charAt(i+ 1);
> }
> // 从下到上，边界1和边界2确定了两条斜线，所以只需要从倒数第三行开始往上补全右上三角
> for(int i = n - 3; i >= 0; i--){  
>     for(int j = i + 2; j < n; j++){ 
>         dp[i][j] = dp[i + 1][j - 1] && (s.charAt(i) == s.charAt(j));
>     }
> }
> ```
>
> 后续的dfs是本题动态规划的重点。
>
> ```java
> public void dfs(String s, int i) {
>     if (i == n) {
>         ret.add(new ArrayList<String>(ans));
>         return;
>     }
>     for (int j = i; j < n; ++j) {
>         if (f[i][j]) {
>             ans.add(s.substring(i, j + 1));
>             dfs(s, j + 1);
>             ans.remove(ans.size() - 1);
>         }
>     }
> }
> ```
>
> 设置了一个开始指针`i`，每当开始指针的值 等于 `s`的长度时，将当前的`ans`结果记录到最终结果`ret`中。
>
> 当开始指针并没有指向终点时，使用一个j来进行遍历从i到末尾。
>
> 每当从`i~j（包含j）`构成一个回文串时，将当前的将当前的回文串子串加入到ans中，然后遍历从`j之后的所有子串中（不包含j）`构成回文串的可能性，其中终将会遍历到末尾，然后通过`i == n`的判断条件，再将ans添加到最终的`ret`中。
>
> 同时，需要记得抹除当前的处理对`ans`的结果影响，即 `ans.remove(ans.size() - 1);`。
