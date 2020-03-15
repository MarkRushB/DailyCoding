
# Data Structures & Algorithms Content <!-- omit in toc -->

- [Data Structures](#data-structures)
  - [稀疏数组](#稀疏数组)
  - [手动实现ArrayList](#手动实现arraylist)
  - [LinkedList](#linkedlist)
    - [单向链表](#单向链表)
      - [单链表面试题](#单链表面试题)
    - [单向环形链表](#单向环形链表)
    - [双向链表](#双向链表)
  - [Bag, Stack, Queue](#bag-stack-queue)
    - [Simple growable data structures](#simple-growable-data-structures)
    - [Different](#different)
    - [Stack](#stack)
      - [Stack实现综合计算器（中缀表达式）](#stack实现综合计算器中缀表达式)
      - [前缀、中缀、后缀表达式（逆波兰表达式）](#前缀中缀后缀表达式逆波兰表达式)
        - [逆波兰计算器](#逆波兰计算器)
        - [中缀转后缀](#中缀转后缀)
    - [Queue](#queue)
  - [Priority Queue](#priority-queue)
    - [定义和使用场景](#定义和使用场景)
    - [抽象API](#抽象api)
    - [优先队列使用示例](#优先队列使用示例)
    - [计算API的调用次数](#计算api的调用次数)
    - [传统方式实现优先队列](#传统方式实现优先队列)
    - [二叉堆的定义](#二叉堆的定义)
      - [二叉堆的特性](#二叉堆的特性)
    - [恢复堆的有序性——上浮](#恢复堆的有序性上浮)
      - [上浮操作示例代码](#上浮操作示例代码)
      - [时间复杂度分析](#时间复杂度分析)
    - [恢复堆的有序性——下沉](#恢复堆的有序性下沉)
      - [下沉操作代码](#下沉操作代码)
      - [时间复杂度分析](#时间复杂度分析-1)
    - [二叉堆实现优先队列API](#二叉堆实现优先队列api)
      - [示例代码](#示例代码)
      - [时间复杂度分析](#时间复杂度分析-2)
- [Algorithms](#algorithms)
  - [Arbitrary Substitution Principle](#arbitrary-substitution-principle)
  - [Entropy](#entropy)
  - [BIg O](#big-o)
  - [Recursion（递归）](#recursion递归)
  - [Union-Find](#union-find)
    - [Quick Find](#quick-find)
    - [Quick Union](#quick-union)
    - [Conclusion](#conclusion)
    - [Improvement](#improvement)
  - [Sorts](#sorts)
    - [How to choose a sort algorithm?](#how-to-choose-a-sort-algorithm)
    - [Ten Sorts we usually use](#ten-sorts-we-usually-use)
    - [Bubble Sort](#bubble-sort)
    - [Selection Sort](#selection-sort)
    - [Insertion Sort](#insertion-sort)
    - [Shell Sort](#shell-sort)
    - [Merge Sort](#merge-sort)
    - [Quick Sort](#quick-sort)
    - [Heap Sort](#heap-sort)
  - [Binary Search](#binary-search)
  - [Random Walk](#random-walk)
    - [Introduce](#introduce)
    - [Implement](#implement)
    - [Reference](#reference)
---

| TOPIC | Data Structures & Algorithms |
|:------------:| :-------------:|
| data types | stack, queue, bag, union-find, priority queue |
| sorting | quicksort, mergesort, heapsort, radix sorts |
| searching | BST, red-black BST, hash table |
| graphs | BFS, DFS, Prim, Kruskal, Dijkstra | 
| strings | KMP, regular expressions, TST, Huffman, LZW |
| advanced | B-tree, suffix array, maxflow |

---

# Data Structures
## 稀疏数组
- 当一个数组中的大部分元素为0时，或者为同一个值的数组时，可以使用稀疏数组来保存该数组。  
稀疏数组的处理方法是:  
    1. 记录数组一共有几行几列，有多少个不同的值。
    2. 把具有不同值的元素的行列及值记录在一个小规模数组中，从而缩小程序的规模。 
        ![avatar](https://upload-images.jianshu.io/upload_images/4017523-76cd82af844fc668.png?imageMogr2/auto-orient/strip|imageView2/2/w/1022/format/webp)  
- 应用场景：编写类似五子棋、地图的应用程序的时候
- 二维数组转稀疏数组的思路：
    1. 遍历原始的二维数组，得到有效数据的个数sum
    2. 根据sum就可以创建稀疏数组sparseArr int[sum+1][3]
    3. 将二维数组的有效数据存入到稀疏数组
- 稀疏数组转原始二维数组的思路
    1. 先读取稀疏数组的第一行，根据第一行的数组，创建原始的二维数组
    2. 在读取稀疏数组后几行的数据，并赋给原始的二维数组即可
```java
public class SparseArray{
    public static void main(String[] args){
        //创建一个原始的二维数组 11*11
        //0:表示没有棋子，1:表示黑子，2：表示白子
        int chessArray1[][] = new int[11][11];
        chessArray1[1][2] = 1;
        chessArray1[2][4] = 2;
        //输出原始的二维数组
        for(int[] row: chessArray1){
            for(int data: row){
                System.out.printf("%d\t", data);
            }
            System.out.println();  
        }
        //将二维数组 转 稀疏数组的思路
        //1. 先遍历二维数组，得到非0数据的个数
        int sum = 0;
        for(int i = 0; i < chessArr1.length; i++){
            for(int j = 0; j < 11; j++){
                if(chessArr1[i][j] != 0){
                    sum++;
                }lany 
            }
        }
        //2.创建稀疏数组
        int sparseArr[][] = new int[sum+1][3];
        //给稀疏数组赋值
        sparseArr[0][0] = 11;
        sparseArr[0][1] = 11;
        sparseArr[0][2] = sum;
        //遍历二维数组，将非0的值存放到sparseArr中
        int count = 0; //count用于记录是第几个非0数据
        for(int i = 0; i < 11; i++){
            for(int j = 0; j < 11; j++){
                if(chessArr1[i][j] != 0){
                    count++;
                    sparseArr[count][0] = i;
                    sparseArr[count][1] = j;
                    sparseArr[count][2] = chessArr1[i][j];
                }
            }
        }
        //输出稀疏数组的形式
            for(int i = 0; i < sparseArr.length; i++){
                System.out.println("%d\t%d\t\n", sparseArr[i][0],sparse[i][1],sparseArr[i][2]);
            }
        //将稀疏数组恢复到原来的二维数组
        //1.先读取稀疏数组的第一行，根据第一行的数据，创建原始的二维数组
        int chessArr2[][] = new int[sparseArr[0][0]][sparseArr[0][1]];
        //2.在读取稀疏数组后几行的数据(第二行开始)，并赋给原始的二维数组即可
        for(int i = 1; i < sparseArr.length; i++){
            chessAarr2[sparseArr[i][0]][sparseArr[i][1]] = sparseArr[i][2]
        }
        
    }
}
```
## 手动实现ArrayList
```java
public class ArrayList<E> {
    private Object[] elementData;
    private int size;

    public ArrayList(){
        elementData = new Object[10];
    }
    public ArrayList(int capacity){
        elementData = new Object[capacity];
    }
    // add方法
    public void add(E element){
        elementData[size++] = obj;
    }
    // 扩容
    public void add(E element){
        // 什么时候扩容？
        if(size == element.length){
            // 扩容操作
            Object[] newArray = new Object[elementData.length+(elementData.length>>1)];
            System.arrayCopy(elementData, 0, newArray, elementData.length);
            elementData = newArray;
        }
        elementData[size++] = element
    }
    // 增加set和get方法
    public E get(int index){
        checkRange(index);
        return (E)elementdata[index];
    }
    public void set(E element, int index){
        checkRange(index);
        elementData[index] = element;
    }
    public void checkRange(int index){
        // 索引合法判断[0,size)
        if(index < 0 || index > size-1){
            throw new RuntimeException("索引不合法!");
        }
    }
    // remove方法
    public void remove(E element){
        // element,将它和所有元素挨个比较，获得第一个比较为true的，返回。
        for(int i =0; i < size; i++){
            if(element.equals(get(i))){ //容器中所有的比较操作，都是用的equals而不是==
                remove(i);
            }
        }
    }
    public void remove(int index){
        int numMoved = elementData.length - index - 1;
        if(numMoved > 0){
            System.arraycopy(elementData, index+1, elementData,
             index, numMoved);
             elementData[size-1] = null;
             size--;
        }else{
            elementData[size-1] = null;
            szie--;
        }
    }
    // size
    public int size(){
        return size;
    }
    // isEmpty
    public boolean isEmpty(){
        return size == 0?true;false;
    }
}
```
## LinkedList
  - ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200121123031.png)
  - Except when full: in which case, a copy is required
  - To be precise, average number of accesses is (N+1)/2
  - why Double LinkedList is O(1)?
    - 双向链表相比于单向链表，所谓的O(1)是指删除、插入操作。
    - 单向链表要删除某一节点时，必须要**先通过遍历的方式找到前驱节点**（通过待删除节点序号或按值查找）。若仅仅知道待删除节点，是不能知道前驱节点的，故单链表的增删操作复杂度为O(n)。 双链表（双向链表）知道要删除某一节点p时，获取其前驱节点q的方式为 q = p->prior，不必再进行遍历。故时间复杂度为O(1)。而若只知道待删除节点的序号，则依然要按序查找，时间复杂度仍为O(n)。
    - 单、双链表的插入操作，若给定前驱节点，则时间复杂度均为O(1)。否则只能按序或按值查找前驱节点，时间复杂度为O(n)。至于查找，二者的时间复杂度均为O(n)。 对于最基本的CRUD操作，双链表优势在于删除给定节点。但其劣势在于浪费存储空间（若从工程角度考量，则其维护性和可读性都更低）。
    - 双链表本身的结构优势在于，可以O(1)地找到前驱节点，若算法需要对待操作节点的前驱节点做处理，则双链表相比单链表有更加便捷的优势。

### 单向链表
- 链表介绍
    - 链表是有序的列表
    - 链表是以节点的方式来存储，是**链式存储**。
    - 每个节点包含data域，next域：指向下一个节点
    - 发现**链表的各个节点不一定是连续存放的**
    - 链表分为**带头节点的链表**和**没有头节点的链表**，根据实际的需求来确定
- 添加（创建）
    1. 先创建一个head头节点，作用就是表示单链表的头
    2. 后面我们每添加一个节点，就直接加入到链表的最后
    3. 遍历：通过一个辅助遍历，帮助遍历整个链表
```java
public class SingleLinkedList{
    public static void main(String[] args){
        //进行测试
        //先创建节点
        HeroNode hero1 = new HeroNode(1, "宋江", "及时雨");
        HeroNode hero2 = new HeroNode(2, "卢俊义", "玉麒麟");
        HeroNode hero3 = new HeroNode(3, "吴用", "智多星");
        HeroN
## LinkedListode hero4 = new HeroNode(4, "林冲", "豹子头");
        //创建一个链表
        SingleLinkedList = singleLinkedList = new SingleLinkedList();
        //加入不按照编号顺序
        singleLinkedList.add(hero1);
        singleLinkedList.add(hero4);
        singleLinkedList.add(hero3);
        singleLinkedList.add(hero2);
        //加入按照编号顺序
        singleLinkedList.addByOrder(hero1);
        singleLinkedList.addByOrder(hero4);
        singleLinkedList.addByOrder(hero3);
        singleLinkedList.addByOrder(hero2);

        //测试修改节点的代码
        HeroNode newHeroNode = new HeroNode(2,"小卢","小麒麟");
        singleLinkedList.update(newHeroNode);

        //显示一把
        singleLinkedList.list();

    }
}

//定义SingleLinkedList 管理我们的英雄
class SingleLinkedList{
    //先初始化一个头节点，头节点不要动
    private HeroNode head = new HeroNode(0,"","");

    //添加节点到单向链表
    //思路：当不考虑编号的顺序时
    //1. 找到当前链表的最后节点
    //2. 将最后这个节点的next指向新的节点
    public void add(HeroNode heroNode){
        //因为head节点不能动，因此我们需要一个辅助变量temp
        HeroNode temp = head;
        //遍历链表，找到最后
        while(true){
            //找到链表的最后
            if(temp.next == null){
                break;
            }
            //如果没有找到最后，就将temp后移
            temp = temp.next;
        }
        //当退出while循环时候，temp就指向了链表的最后
        //将最后这个节点的next指向新的节点
        temp.next = heroNode;
    } 
    //需要按编号的顺序添加
    //1. 首先找到新添加的节点的位置，是通过辅助变量（指针），通过遍历来搞定
    //2. 新的节点.next = temp.next
    //3. 将temp.next = 新的节点
    public void addByOrder(HeroHead heroHead){
        //因为头节点不能动，因此我们仍然需要通过一个temp辅助变量来帮助找到添加的位置
        //因为单链表，因为我们找的temp，是位于添加位置的前一个节点，否则插入不了
        HeroNode temp = head;
        boolean flag = false;//添加的编号是否存在，默认为false
        while(true){
            if(temp.next == null){
                //说明temp已经在链表的最后
                break;
            }
            if(temp.next.no > heroNode.no){
                //位置找到，就在temp的后面插入
                break;
            }else if(temp.next.no == heroNode.no){
                //说明希望添加的heroNode的编号已经存在
                flag = true;
                break;
            }
            temp = temp.next;//后移，相当于遍历当前链表
        }
        //判断flag的值
        if(flag){
            //不能添加，说明编号存在
            System.out.println("准备插入的英雄编号已经存在了，不能加入"heroNode.no);
        }else{
            //插入到链表中，temp的后面
            heroNode.next = temp.next;
            temp.next = heroNode;
        }
    }
    //修改节点的信息，根据no编号修改，即no编号不能改
    public void update(HeroNode newHeroNode){
        //判断是否为空
        if(head.next = null){
            System.out.println("链表为空");
            return;
        }
        //找到需要修改的节点，根据no编号
        //定义一个辅助变量
        HeroNode temp = head.next;
        boolean flag = false;//表示是否找到该节点
        while(true){
            if(temp == null){
                break;//已经遍历完链表
            }
            if(temp.no == newHeroNode.no){
            //找到
            flag = true;
            break;
            }
            temp = temp.next;
        }
        //根据flag判断是否找到要修改的节点
        if(flag){
            temp.name = newHeroNode.name;
            temp.name = newHeroNode.name;
        }else{
            //没有找到
            Sysytem.out.Println("没有找到");
        }
    }
    //从单链表中删除一个节点的思路
    //1. 我们先找到需要删除的这个节点的前一个节点temp
    //2. temp.next = temp.next.next
    //3. 被删除的节点不会有其他的引用指向，会被垃圾回收机制回收
    //思路：
    //1.head不能动，因此我们需要一个temp辅助节点找到待删除节点的前一个节点
    //2.说明我们在比较时，是temp.next.no 和 需要删除的节点的no比较
    public void delete(int no){
        HeroNode temp = head;
        boolean flag = false;//标志是否找到待删除节点的
        while(true){
            if(temp.next == null){
                //已经到链表的最后
                break;
            }
            if(temp.no == no){
                //找到待删除的节点的前一个节点temp
                flag = true;
                break;
            }
            temp = temp.next;//temp后移，遍历
        }
        //判断flag
        if(flag){//找到
            //可以删除
            temp.next = temp.next.next;
        }else{
            System.out.println("需要删除的节点不存在");
        }
    }
     
    //显示链表【遍历】
    public void list(){
        //判断链表是否为空
        if(head.next == null){
            System.out.println("链表为空");
            return;
        }
        //因为head不能动，因此我们需要一个辅助变量来遍历
        HeroNode temp = head.next;
        while(true){
            //判断时候到链表最后
            if(temp == null){
                break;
            }
            //输出节点的信息
            System.out.println(temp);
            //将temp后，一定小心
            temp = temp.next; 
        }
    }
}
//定义HeroNode，每个HeroNode对象就是一个节点
class HeroNode{
    public int no;
    public String name;
    public String nickname;
    public HeroNode next;//指向下一个节点
    //构造器
    public HeroNode(int no, Strin name, String nickname){
        this.no = no;
        this.name = name;
        this.nickname = nickname;
    }
    //为了显示方法，我们重写toString
    @override
    public String toString(){
        return "HeroNode [no="+ no +", name="+ name +", nickname=" + nickname + "]"
    }
}
```
#### 单链表面试题
1. **求单链表中有效节点的个数**
    - 思路：遍历即可
    - 如果带头节点的链表，**不要统计头节点**
    - head就是链表的头节点
    - 返回的就是有效节点的个数
    ```java
    public static int getLength(HeroNode head){
        if(head.next == null){
            //空链表
            return 0;
        }
        int length = 0;
        //定义一个辅助变量
        HeroNode cur = head.next;
        while(cur != null){
            length++;
            cur = cur.next;
        }
        return length;
    }
    ``` 
2. **查找单链表中的倒数第k个节点**
    - 思路：编写一个方法接收head节点，同时接收一个index
    - index表示倒数第index个节点
    - 先把链表从头到尾遍历一下，得出链表的总长度getLength。
    - 得到size后，我们从链表的第一个开始遍历(size-index)个，就可以得到
    - 如果找到了，返回该节点，否则返回null
    ```java
    public static HeroNode findLastIndexNode(HeroNode head, int index){
        //判断链表如果为空，返回null
        if(head.next == null){
            return null;//没有找到
        }
        //第一次遍历得到的链表长度（节点个数）
        int size = get Length(head);
        //第二次遍历size-index位置，即使我们倒数的第k个节点
        //先做一个index的校验
        if(index <= 0 || index > size){
            return null;
        }
        //定义辅助变量
        HeroNode cur = head.next;
        for(int i = 0; i < size -index; i++){
            cur = cur.next;
        }
        return cur;
    }
    ```
3. **单链表的反转**
    - 思路：先去定义一个节点reverseHead = new HeroNode();
    - 从头到尾遍历原来的链表，每遍历一个节点，就将其取出，并放在新的链表的最前端
    - 原来链表的head.next = reverseHead.next
    - ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/IMG_0272.PNG)
  ```java
  //将单链表进行反转
    public static void reverseList(HeroNode head){
        //如果当前链表为空，或者只有一个节点，无需反转，直接返回
        if(head.next == null || head.next.next == null){
            return;
        }
        //定义一个辅助的指针(变量)，帮助我们遍历原来的链表
        HeroNode cur = head.next;
        HeroNode next = null;//指向当前节点[cur]的下一个节点
        HeroNode reverseHead = new HeroNode(0,"","");
        //从头到尾遍历原来的链表，每遍历一个节点，就将其取出，并放在新的链表的最前端
        while(cur != null){
            next = cur.next;//先暂时保存当前节点的下一个节点，因为后面需要使用
            cur.next = reverseHead.next;//将cur下一个节点指向新的链表的最前端
            reverseHead.next = cur;//将cur连接到新的链表上  
            cur = next;//让cur后移
        }
        //将head.next指向reverseHead.next，实现了单链表的反转
        head.next = reverseHead.next;
    }
  ```
   - ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/reverseLinkedList.jpg)
4. **从尾到头打印单链表**
   - 方法一：先反转单链表，然后再遍历即可（这样做的问题是会破坏原来单链表的结构，不可取，**不建议**）
   - 方法二：可以利用**栈**的数据结构，将各个节点压入栈中，利用栈的先进后出的特点，就实现了逆序打印的效果
    ```java
    public static void reversePrint(Heroode head){
        if(head.next == null){
            return;//空链表，不能打印
        }
        //创建一个栈，将各个节点压入栈中
        new stack<HeroNode> stack = new Stack<HeroNode>();
        HeroNode cur = head.next;
        //将链表的所有节点压入栈中
        while(cur != null){
            stack.push(cur);
            cur = cur.next;//cur后移，这样接可以压入下一个节点
        }
        //将栈中的节点进行打印,pop出栈
        while(stack.size() > 0){
            System.out.println(stack.pop());//stack的特点是先进后出
        }
    }
    ```
### 单向环形链表
- 构建一个单向环形链表的思路
  1. 先创建第一个节点，让first指向该节点，并形成环形
  2. 后面当我们每创建一个新的节点，就把该节点，加入到已有的这个环形链表中即可
- 遍历环形链表
  1. 先让一个辅助变量指向first节点
  2. 然后通过一个while循环遍历该环形链表即可 cur.next = first的时候结束循环
```java
//创建一个环形的单向链表
class CircleSingleLinkedList{
    //创建一个firs节点，当前没有编号
    private Boy first = new Boy(-1);
    //添加小孩，构建成一个环形链表
    public void addBoy(int nums){
        //nums做一个数据校验
        if(nums < 1){
            System.out.prinltn("nums的值不正确");
            return;
        }
        Boy curBoy = null;//辅助指针，帮助构建环形链表
        //使用for循环创建我们的环形链表
        for(int i = 1; i <= nums; i++){
            //根据编号创建小孩节点
            Boy boy = new Boy(i);
            //如果是第一个小孩
            if(i == 1){
                first = boy;
                first.setNext(first);//构成环
                curBoy = first;//让curBoy指向第一个小孩
            }else{
                curBoy.setNext(boy);
                boy.serNext(first);
                curBoy = boy;
            }
        }
    }
    //遍历当前的环形链表
    public void showBoy(){
        //判断链表是否为空
        if(first = null){
            System.out.println("链表为空");
            return;
        }
        //因为first不能动，因此我们仍然使用一个辅助指针完成遍历
        Boy curBoy = first;
        while(true){
            System.out.println("小孩的编号"curBoy.getNo());
            if(curBoy.getNext() == first){
                //说明已经遍历完毕
                break;
            }
            curBoy = curBoy.getNext();//curBoy后移
        }
    }
}
//创建一个Boy类，表示一个节点
class Boy{
    private int no;//编号
    private Boy next;//指向下一个节点，默认为null

    public Boy(int no){
        this.no = no;
    }
    public int getNo(){
        return no;
    }
    public void setNo(int no){
        this.no = no;
    }
    public Boy getNext(){
        return next;
    }
    public void setNext(Boy next){
        this.next = next;
    }
}
```
- 单向环形链表应用场景：Josephu问题
  - Josephu问题为：设编号为1，2，...n的n个人围成一圈，约定编号为k(1 <= k <= n)的人从1开始报数，数到m的那个人出列，他的下一位又从1开始报数，数到m的人继续出列，以此类推，直到所有人都出列为止，由此产生一个出队编号的序列。
  - 思路：用一个**不带头节点的循环链表**来处理Josephu问题：先构成一个有n个节点的单循环链表，然后由k节点起从1开始数，计到m时候，对应节点从链表中删除，然后再从被删除节点的下一个节点又从1开始计数，直到最后一个节点从链表中删除，算法结束。
      1. 需要创建一个辅助变量helper，事先应该指向环形链表的**最后这个节点**
      2. 报数前先让first和helper移动k-1次
      3. 当小孩报数时，让first和helper**同时移动**，移动m-1次（自己要数一下）
      4. 这时就可以将first指向的小孩节点出圈  
        first = first.next  
        helper.next = first  
        原来first指向的节点就没有任何引用，就会被回收。
```java
//根据用户的输入，计算出小孩出圈的顺序
//startNo：从第几个小孩开始
//countNo：数几下
// nums：有多小孩在圈中
public void countBoy(int startNo, int countNum, int nums){
    if(first == null || startNo < 1 || startNum > nums){
        System.out.println("参数输入错误");
        return;
    }
    //创建一个辅助指针，帮助完成小孩出圈
    Boy helper = first;
    while(true){
        //这一步是为了让helper指向最后一个节点
        if(helper.getNext() == first){
            //说明helper指向了最后节点
            break;
        }
        helper = helper.getNext();
    }
    //小孩报数前，先让first和helper移动k-1次
    for(int j = 0; j < startNo -1; j++){
        first = first.getNext();
        helper = helper.getNext();
    }
    //当小孩报数时，让first和helper指针同时移动m-1次，然后出圈
    //这里是一个循环操作，直到圈中只有一个节点
    while(true){
        if(helper == first){
            //说明圈中只有一个节点
            break;
        }
        //让first和helper同时移动countNum-1次
        for(int j = 0; j < countNum - 1; j++){
            first = first.getNext();
            helper = helper.getNext();
        }
        //这时first指向的节点就是小孩要出圈的节点
        System.out.printf("小孩出圈："first.getNo());
        //这时候将first指向的小孩节点出圈
        first = first.getNext();
        helper.setNext(first);
    }
    System.out.println("最后留在圈中的小孩编号:" first.getNum());

}
```

### 双向链表
- **Intro**: Each element has three fields:
  - The **value** of this element
  - A **pointer/reference** to the **next element** (which may be null)
  - A **pointer/reference** to the **previous element** (which may be null)
- 单向链表的缺点分析
  - 单向链表，**查找的方向只能是一个方向**，而双向链表可以向前或者向后查找
  - 单向链表不能自我删除，需要靠辅助节点，而双向链表则可以**自我删除**，所以之前单链表删除节点时，总是需要找到temp（待删除节点的前一个节点）
- 分析双向链表的遍历，添加，修改，删除的操作
  - **遍历**：和单链表一样，只不过可以向前也可以向后
  - **添加**：默认添加到双向链表的最后
    1. 先找到双向链表的最后这个节点
    2. temp.next = newHeroNode
    3. newHeroNode.pre = temp
  - **修改**: 思路和原理和单向链表一样
  - **删除**:
    1. 因为是双向链表，因为我们可以实现自我删除某个节点
    2. 直接找到要删除的这个节点，比如temp 
    3. temp.pre.next = temp.next
    4. temp.next.pre = temp.pre
```java
public class DoubleLinkedListDemo{
    public static void main(String[] args){

    }
}

//创建一个双向链表的类
class DoubleLinkedList{
    //先初始化一个头节点，头节点不要动，不存放具体的数据
    private HeroNode2 head = new HeroNode2(0,"","");
    //返回头节点
    public HeroNode2 getHead(){
        return head;
    }
    
    //遍历双向链表的方法
    //显示链表【遍历】
    public void list(){
        //判断链表是否为空
        if(head.next == null){
            System.out.println("链表为空");
            return;
        }
        //因为head不能动，因此我们需要一个辅助变量来遍历
        HeroNode2 temp = head.next;
        while(true){
            //判断时候到链表最后
            if(temp == null){
                break;
            }
            //输出节点的信息
            System.out.println(temp);
            //将temp后移，一定小心
            temp = temp.next; 
        }
    }
    //添加一个节点到双向链表的最后
    public void add(HeroNode2 heroNode){
        //因为head节点不能动，因此我们需要一个辅助变量temp
        HeroNode2 temp = head;
        //遍历链表，找到最后
        while(true){
            //找到链表的最后
            if(temp.next == null){
                break;
            }
            //如果没有找到最后，就将temp后移
            temp = temp.next;
        }
        //当退出while循环时候，temp就指向了链表的最后
        //形成一个双向链表
        temp.next = heroNode;
        heroNode.pre = temp;
    }
    //修改一个节点的内容，可以看到双向链表的节点内容的修改和单向链表一样
    public void update(HeroNode2 newHeroNode){
        //判断是否为空
        if(head.next = null){
            System.out.println("链表为空");
            return;
        }
        //找到需要修改的节点，根据no编号
        //定义一个辅助变量
        HeroNode2 temp = head.next;
        boolean flag = false;//表示是否找到该节点
        while(true){
            if(temp == null){
                break;//已经遍历完链表
            }
            if(temp.no == newHeroNode.no){
            //找到
            flag = true;
            break;
            }
            temp = temp.next;
        }
        //根据flag判断是否找到要修改的节点
        if(flag){
            temp.name = newHeroNode.name;
            temp.name = newHeroNode.name;
        }else{
            //没有找到
            Sysytem.out.Println("没有找到");
        }
    }
    //从双向链表中删除一个节点
    //说明：
    //对于双向链表，我们可以直接找到要删除的这个节点
    //找到后，自我删除即可
    public void delete(int no){
        //判断当前链表是否为空
        if(head.next == null){
            //空
            System.out.println("链表为空");
            return;
        }

        HeroNode2 temp = head.next;//之前这里是head，为什么改成head呢？
        //是因为之前需要找到待删除节点的上一个节点，现在由于是双向链表，直接找到待删除节点就可以了。
        boolean flag = false;//标志是否找到待删除节点的
        while(true){
            if(temp == null){
                //已经到链表的最后节点的next
                break;
            }
            if(temp.no == no){
                //找到待删除节点的前一个节点temp
                flag = true;
                break;
            }
            temp = temp.next;//temp后移，遍历
        }
        //判断flag
        if(flag){//找到
            //可以删除
            temp.pre.next = temp.next;
            //这里我们的代码有问题？
            //如果是最后一个节点，就不需要执行下面这句话，否则会出现空指针异常
            if(temp.next = null){
                temp.next.pre = temp.pre;
            }
        }else{
            System.out.println("需要删除的节点不存在");
        }
    }

}
//定义HeroNode2，每个HeroNode2对象就是一个节点
class HeroNode2{
    public int no;
    public String name;
    public String nickname;
    public HeroNode2 next;//指向下一个节点，默认为null
    public HeroNode2 pre;//指向前一个节点，默认为null
    //构造器
    public HeroNode2(int no, Strin name, String nickname){
        this.no = no;
        this.name = name;
        this.nickname = nickname;
    }
    //为了显示方法，我们重写toString
    @override
    public String toString(){
        return "HeroNode [no="+ no +", name="+ name +", nickname=" + nickname + "]"
    }
}
```
## Bag, Stack, Queue
### Simple growable data structures
- Given an infinitely growable collection of things, what are the bare minimum operations you will need to make it useful?
  - `void add(Thing thing)`
  - `Thing remove() throws Exception`
  - `Iterator<Thing> iterator()`
  - `booleanisEmpty()`required to avoid throwingexceptionwhencallingremove().
  - Access by index (or matching value)
  - Navigation

### Different
- ||Order|
  |-|-|
  |Bag|Random|
  |Stack|BackWards LIFO|
  |Queue|Forwards FIFO|
### Stack
- Why do not we increase the size of the array by 1 when we push a new item into stack?
  - That is easy to code up, but not worth it, because it's much too expensive to do that. The reason is that you have to create a new array,  size on bigger, and copy all the items to that new array
- How to grow array?
  - If array is full, create a new array of **twice** the size, and copy items
  ```java
    public void expandCapacity(int size) {
        int len = stack.length;
        if (size > len) {
            size = size * 3 / 2 + 1;//每次扩大50%
            stack = Arrays.copyOf(stack, size);
        }
    }
  ```
- Why do not we cut it in half when it gets to be half full?
  - Well, that one doesn't exactly work because of a phenomenon called **thrashing**(抖动). I f the client happens to do push-pop-push-pop alternating when the array is **full**, then it's going to be doubling, having, doubling, having, doubling, having. Creating new arrays on every operation. Take time proportional to N for every operation, and therefore quadratic time for everything. So I don't want to do that.
  - The efficient solution is to **wait until the array gets one-quarter full** before you have it. And that's very easy to implement. We can just **test if the array is one quarter full**, if it is, we **resize it to half full**. And so then at that point, it's half full, and it can either grow by adding stuff or shrink by subtracting stuff. But there won't be another resizing array operation until it either gets totally full or half again full.
  ```java
    public String pop() {
        String item = s[--N];
        s[N] = null;
        if (N > 0 && N == s.length / 4)
            resize(s.length / 2);
        return item;
    }
  ```
- Use array to implement Stack
    ```java
    public class ResizingArrayStackOfStrings {
        private String[] s;
        private int N = 0;
        
        public FixedCapacityStackOfStrings(int capacity) {
            s = new String[capacity];
        }
        
        public boolean isEmpty() {
            return N == 0;
        }
        
        public void push (String item) {
            if (N == s.length)
                resize(2 * s.length);
            s[N++] = item;
        }
        
        public String pop() {
            String item = s[--N];
            s[N] = null;
            if (N > 0 && N == s.length / 4)
                resize(s.length / 2);
            return item;
        }
        
        private void resize(int capacity) {
            String[] copy = new String[capacity];
            for (int i = 0; i < N; i++)
                copy[i] = s[i];
            s = copy;
        }
        
        public ResizingArrayStackOfStrings() {
            s = new String[1];
        }
    }
    ```
- Use LinkedList to implement Stack
  - 课程中有关链表的操作都使用内部类定义节点元素
  ```java
  private class Node {
      String item;
      Node next;
  }
  ```
  - API 实现：
  ```java
  public class LinkedStackOfStrings {
      private Node first = null;
      
      private class Node {
          String item;
          Node next;
      }
      
      public boolean isEmpty() {
          return first == null;
      }
      
      public void push (String item) {
          Node oldfirst = first;
          first = new Node();
          first.item = item;
          first.next = oldfirst;
      }
      
      public String pop() {
          String item = first.item;
          first = first.next;
          return item;
      }
  }
  ```
- Stack implementations:  resizing array vs. linked list
  - **Linked-list implementation**
    - Every operation takes constant time in the **worst case**
    - Uses extra time and space to deal with the links
  - **Resizing-array implementation**
    - Every operation takes constant **amortized** time
    - Less wasted space
  - if I want to be sure that every operation's going to be fast, I'll use a linked list. And if I don't need that guarantee, if I just care about the total amount of time, I'll probably use the resizing-array because the total will be much less, because individual operations are fast.
- Dijkstra’s two-stack algorithm
    ```java
    package com.Dijkstra;
    import com.Stack.ArrayToStack;
    /**
    * 利用2个栈实现简单的运算操作
    * Dijkstra双栈算法
    *
    * 1、将操作数压入操作数栈；
    * 2、将运算符压入运算符栈；
    * 3、忽略左括号；
    * 4、在遇到右括号时，弹出一个运算符，弹出所需数量的操作数，并将运算符和操作数的运算结果压入操作数栈
    */
    public class Evaluate {
    
        public static void main(String[] args) {
            String s = "(1+((2+3)*(4*5)))";
            test(s);
        }
    
        public static void test(String str){
            ArrayToStack<String> ops = new ArrayToStack<String>(10);//运算符栈
            ArrayToStack<Double> vals = new ArrayToStack<Double>(10);//操作数栈
    
            for(int i =0; i<str.length();i++){
                String s = (char)str.getBytes()[i] +""; //挨个取出字符
                //读取字符。如果是运算符则压入栈
                if (s.equals("("));
                else if (s.equals("+")) ops.push(s);
                else if (s.equals("-")) ops.push(s);
                else if (s.equals("*")) ops.push(s);
                else if (s.equals("/")) ops.push(s);
                else if (s.equals("sqrt")) ops.push(s);
                else if (s.equals(")")){  //如果是 ）(右括号) 则弹出运算符和操作数。计算结果并压入操作数栈
                    String op = ops.pop();//取出操作运算符
                    double v = vals.pop();//取出操作数，并与下一个操作数进行运算
                    if(op.equals("+")) v = vals.pop() + v;
                    else if (op.equals("-")) v = vals.pop() - v;
                    else if (op.equals("*")) v = vals.pop() * v;
                    else if (op.equals("/")) v = vals.pop() / v;
                    else if (op.equals("sqrt")) v = Math.sqrt(v);
                    vals.push(v);  //将结果压入栈中
                }else {
                    vals.push(Double.parseDouble(s)); //不是运算符号，则是操作数，将操作数压入操作数栈中
                }
            }
            System.out.println(vals.pop());//打印出最终结果
        }
    }
    ```
#### Stack实现综合计算器（中缀表达式）
- 基本思路
  - ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200220123053.png)
  - 创建两个栈，一个是**numStack**，用来存放数字，一个是**operStack**，用来存放运算符
    1. 通过一个index值（索引），来遍历我们的表达式
    2. 如果我们发现是一个数字，就直接入数栈
    3. 如果发现是一个符号，就分如下情况：
       1. 如果当前的符号栈**是空**，就直接入栈
       2. 如果符号栈**有操作符号**，就进行比较：
          1. 如果当前操作符的优先级**小于或者等于**栈中的操作符，就需要从数栈中pop出两个数，再从符号栈中pop出一个符号，进行运算，将得到的结果入数栈，然后将当前的操作符入符号栈
          2. 如果当前操作符的优先级**大于**栈中的操作符，就直接入符号栈
    4. 当表达式扫描完毕后，就顺序的从数栈和符号栈中pop出相应的数和符号，并运行
    5. 最后在数栈只有一个数字，就是表达式的结果
```java
import java.util.Arrays;

public class Calculator {
    //创建一个栈
    public static class ArrayStack {
        private int[] s;
        private int N = 0;

        public void FixedCapacityStackOfStrings(int capacity) {
            s = new int[capacity];
        }

        public boolean isEmpty() {
            return N == 0;
        }

        public void push (int item) {
            if (N == s.length)
                resize(2 * s.length);
            s[N++] = item;
        }

        public int pop() {
            int item = s[--N];
            s[N] = 0;
            if (N > 0 && N == s.length / 4)
                resize(s.length / 2);
            return item;
        }

        public int peek(){
            return s[N - 1];
        }

        private void resize(int capacity) {
            int[] copy = new int[capacity];
            for (int i = 0; i < N; i++)
                copy[i] = s[i];
            s = copy;
        }

        public ArrayStack() {
            s = new int[1];
        }

        public void list(){
            if(isEmpty()){
                System.out.println("void, no data");
                return;
            }
            for(int i = N-1; i >= 0; i--){
                System.out.print(s[i]);
            }
        }
        //计算器用的栈需要扩展功能
        //返回运算符的优先级
        public int priority(int oper){
            if(oper == '*' || oper == '/'){
                return 1;
            }else if(oper == '+' || oper == '-'){
                return 0;
            }else{
                return -1;
            }
        }
        //判断是不是一个运算符
        public boolean isOper(char val){
            return val == '+' || val == '-' || val == '*' || val == '/';
        }
        //计算方法
        public int cal(int num1, int num2, int oper){
            int res = 0;
            switch(oper){
                case '+' :
                    res = num1 + num2;
                    break;
                case '-' :
                    res = num2 - num1;
                    break;
                case '*' :
                    res = num2 * num1;
                    break;
                case '?' :
                    res = num2 / num1;
                    break;
                default:
                    break;
            }
            return res;
        }
    }

    public static void main(String[] args) {
        String expression = "3+2*6-2";
        ArrayStack numStack = new ArrayStack();
        ArrayStack operStack = new ArrayStack();
        //定义需要的相关变量
        int index = 0;
        int num1 = 0;
        int num2 = 0;
        int oper = 0;
        int res = 0;
        char ch = ' ';//将每次扫描得到的char保存到ch
        while(true){
            //依次得到expression的每一个字符
            ch = expression.substring(index, index+1).charAt(0);
            //判断ch是什么，然后做相应的处理
            if(operStack.isOper(ch)){
                //如果是运算符
                //判断当前的符号栈是否为空
                if(!operStack.isEmpty()){
                    //如果当前操作符的优先级小于或者等于栈中的操作符，就需要从数栈中pop出两个数
                    //再从符号栈中pop出一个符号，进行运算，入数盏，然后将当前的操作符入符号栈
                    if(operStack.priority(ch) <= operStack.priority(operStack.peek())){
                        num1 = numStack.pop();
                        num2 = numStack.pop();
                        oper = operStack.pop();
                        res = numStack.cal(num1,num2,oper);
                        //把运算的结果入数栈
                        numStack.push(res);
                        //将当前的操作符入符号栈
                        operStack.push(ch);
                    }else{
                        //如果当前的操作符的优先级大于栈中的操作符，就直接入符号栈
                        operStack.push(ch);
                    }
                }else{
                    //如果为空直接入符号栈
                    operStack.push(ch);
                }
            }else{//如果是数，则直接入数栈
                numStack.push(ch - 48);
            }
            //让index + 1，并判断是否扫描到expression最后
            index++;
            if(index >= expression.length()){
                break;
            }
        }
        //当表达式扫描完毕，就顺序的从数栈和符号栈中pop出相应的数和符号，并运行
        while(true){
            //如果符号栈为空，则计算到最后的结果，数栈中只有一个数字[结果]
            if(operStack.isEmpty()){
                break;
            }
            num1 = numStack.pop();
            num2 = numStack.pop();
            oper = operStack.pop();
            res = numStack.cal(num1,num2,oper);
            numStack.push(res);
        }
        System.out.println("表达式:" + expression + " = "+ numStack.pop());
    }
}
```
>存在缺陷，当数字存在多位数的时候，计算器失效
- Improvement
```java
String KeepNum = "";//用于拼接多位数
//
//......
//
}else{//如果是数，则直接入数栈
    //原代码
    numStack.push(ch - 48);
//--------------------------------------------------------------
    //分析思路：
    //1. 当处理多位数时，不能发现是一个数就立即入栈，因为可能是多位数
    //2. 在处理数，需要向expression的表达式的index后再看一位，如果是数字就进行扫描，如果是符号才入栈
    //3. 因此我们需要定义一个变量（字符串）用于拼接

    //处理多位数
    KeepNum += ch;

    //如果ch已经是expression的最后一位，直接入栈
    if(index == expression.length() - 1){
        numStack.push(integer.parseInt(keepNum));
    }else{
        //判断下一个数字是不是数组，如果是数字，就继续扫描，如果是运算符，则入栈
        if(operStack.isOper(expression.substring(index + 1, index + 2).charAt(0))){
            //如果后以为是运算符，则入栈
            numStack.push(integer.parseInt(keepNum));
            //重要！！！！！！！！清空KeepNum
            KeepNum = "";
        }
    }
}
```
#### 前缀、中缀、后缀表达式（逆波兰表达式）
- 前缀表达式（波兰表达式）
  - 前缀表达式的运算符位于两个相应操作数之前，前缀表达式又被称为前缀记法或波兰式
  - 前缀表达式的计算机求值
    1. 从右至左扫描表达式
    2. 遇到数字时，将数字压入堆栈，遇到运算符时，弹出栈顶的两个数，用运算符对它们做相应的计算（**栈顶元素 op 次顶元素**），并将结果入栈
    3. 重复上述过程直到表达式最左端，最后运算得出的值即为表达式的结果
    - 示例：计算前缀表达式的值：- + 1 × + 2 3 4 5
      1. 从右至左扫描，将5，4，3，2压入堆栈；
      2. 遇到+运算符，弹出2和3（2为栈顶元素，3为次顶元素），计算2+3的值，得到5，将5压入栈；
      3. 遇到×运算符，弹出5和4，计算5×4的值，得到20，将20压入栈；
      4. 遇到1，将1压入栈；
      5. 遇到+运算符，弹出1和20，计算1+20的值，得到21，将21压入栈；
      6. 遇到-运算符，弹出21和5，计算21-5的值，得到16为最终结果
  - 可以看到，用计算机计算前缀表达式是非常容易的，不像计算后缀表达式需要使用正则匹配
- 后缀表达式（逆波兰表达式）
  - 后缀表达式与前缀表达式类似，只是运算符位于两个相应操作数之后，后缀表达式也被称为后缀记法或逆波兰式
  - 后缀表达式的计算机求值，与前缀表达式类似，只是顺序是从左至右：
    1. 从左至右扫描表达式
    2. 遇到数字时，将数字压入堆栈，遇到运算符时，弹出栈顶的两个数，用运算符对它们做相应的计算（**次顶元素op 栈顶元素** ），并将结果入栈
    3. 重复上述过程直到表达式最右端，最后运算得出的值即为表达式的结果
    - 示例：计算后缀表达式的值：1 2 3 + 4 × + 5 -
      1. 从左至右扫描，将1，2，3压入栈；
      2. 遇到+运算符，3和2弹出，计算2+3的值，得到5，将5压入栈；
      3. 遇到4，将4压入栈
      4. 遇到×运算符，弹出4和5，计算5×4的值，得到20，将20压入栈；
      5. 遇到+运算符，弹出20和1，计算1+20的值，得到21，将21压入栈；
      6. 遇到5，将5压入栈；
      7. 遇到-运算符，弹出5和21，计算21-5的值，得到16为最终结果
##### 逆波兰计算器
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.Stack;

public class PolandNotation {
    public static void main(String[] args) {
        //先定义逆波兰表达式
        //(3 + 4) * 5 - 6  => 3 4 + 5 * 6 -
        //为了方便，逆波兰表达式数字和符号使用空格隔开
        String suffixExpression = "3 4 + 5 * 6 -";
        //思路
        //1. 先将"3 4 + 5 * 6 -"放到ArrayList中
        //2. 将ArrayList传给一个方法，遍历，配合栈完成计算

        List<String> rpnList = getListString(suffixExpression);
        System.out.println(rpnList);
        int res = calculate(rpnList);
        System.out.println(res);
    }
    //将一个逆波兰表达式，依次将数据和运算符放入到ArrayList中
    public static List<String> getListString(String suffixExpression){
        //将suffixExpression分割
        String[] split = suffixExpression.split(" ");
        List<String> list = new ArrayList<String>();
        for(String ele: split){
            list.add(ele);
        }
        return list;
    }

    public static int calculate(List<String> ls){
        //创建栈，只需要一个栈即可
        Stack<String> stack = new Stack<String>();
        //遍历ls
        for(String item: ls){
            //使用正则表达式取出数
            if(item.matches("\\d+")){//匹配的是多位数
                //入栈
                stack.push(item);
            }else{
                //pop出两个数，并运算，再入栈
                int num2 = Integer.parseInt(stack.pop());
                int num1 = Integer.parseInt(stack.pop());
                int res = 0;
                if(item.equals("+")){
                    res = num1 + num2;
                }else if (item.equals("-")){
                    res = num1 -num2;
                }else if(item.equals("*")){
                    res = num1 * num2;
                }else if(item.equals("/")){
                    res = num1 / num2;
                }else{
                    throw new RuntimeException("运算符号有误");
                }
                //把res入栈
                stack.push(""+res);
            }
        }
        //最后留在stack中的数据就是运算结果
        return Integer.parseInt(stack.pop());
    }
}
```
##### 中缀转后缀
- 后缀表达式适合计算式进行运算，但是人却不容易写出来，尤其是表达式很长的情况下，因此在开发中，我们需要将中缀表达式转成后缀表达式
- 具体步骤如下
  1. 初始化两个栈：运算符栈s1和储存中间结果的栈s2
  2. 从左至右扫描中缀表达式
  3. 遇到操作数时，将其压s2
  4. 遇到运算符时，比较其与s1顶栈运算符的优先级：
     1. 如果s1为空，或栈顶运算符为左括号，则直接将此运算符入栈
     2. 否则，若优先级比栈顶运算度的高，也将运算符压入s1；
     3. 否则，将s1栈顶的运算符弹出并压入到栈s2中，再次转到（4.1）与s1中新的栈顶运算符相比较
  5. 遇到括号时：
     1. 如果是左括号，则直接压入s1
     2. 如果是右括号，则依次弹出s1栈顶的运算符，并压入s2，直到遇到左括号为止，此时将这一对括号丢弃
  6. 重复步骤2-5，直到表达式的最右边
  7. 将s1中剩余的运算符依次弹出并压入s2
  8. 依次弹出s2中的元素并输出，结果的逆序即为中缀表达式对应的后缀表达式

```java
//中缀转后缀
//说明
//1. 1 + ( ( 2 + 3 ) * 4 ) - 5 => 1 2 3 + 4 * + 5 -
//2. 直接对str进行操作，不方便，因此先将"1 + ( ( 2 + 3 ) * 4 ) - 5"转成中缀表达式对应的list
//3. 将得到的中缀表达式的List转成后缀表达式对应的List

String expression = "1+((2+3)*4)-5";

public static Lsit<String> parseSuffixExpressionList(List<String> ls){
    //定义两个栈
    Stack<String> s1 = new Stack<String>();//符号栈
    //因为s2栈在整个转换过程中没有pop操作，而且后面该需要逆序输出，因此我们直接使用List<String>s2替代
    // Stack<String> s2 = new Stack<String>();
    List<String> s2 = new ArrayList<String>();//储存中间结果的List2

    //遍历ls
    for(String item: ls){
        //如果是一个数，加入s2
        if(item.match("\\d+")){
            s2.add(item);
        }else if(item.equals("(")){
            s1.push(item);
        }else if(item.equals(")")){
            //如果是右括号，则依次弹出s1栈顶的运算符，并压入s2，直到遇到左括号为止，此时将这一对括号丢弃
            while(!s1.peek().equals("(")){
                s2.add(s1.pop());
            }
            s1.pop();//将小括号(弹出，消除小括号
        }else{
            //当item的优先级小于或者等于栈顶栈顶运算符的优先级，将s1栈顶的运算符弹出并加入到s2中，再次转到（4.1）与s1中新的栈顶运算符相比较
            //缺少一个比较优先级高低的方法
            while(s1.size != 0 && Operation.getValue(s1.peek()) >= Operation.getValue(item)){
                s2.add(s1.pop());
            }
            //将item压入栈中
            s1.push(item);
        }
    }
    //将s1中剩余的运算符依次弹出并加入s2
    while(s1.size() != 0){
        s2.add(s1.pop());
    }
    return s2;//因为是存放到List，因此按顺序输出就是正确的逆波兰表达式
}
//编写一个类Operation，可以反悔一个运算符对应的优先级
class Operation{
    private static int ADD = 1;
    private static int SUB = 1;
    private static int MUL = 2;
    private static int DIV = 2;

    //写一个方法，返回对应的优先级数字
    public static int getValue(String operation){
        int value = 0;
        switch(operation){
            case "+":
                result = ADD;
                break;
            case "-":
                result = SUB;
                break;
            case "*":
                result = MUL;
                break;
            case "/":
                result = DIV;
                break;
            default:
                System.out.println("不存在该运算符");
                break;
        }
        return result;
    }
}
//方法：将中缀表达式转成对应的list
public static List<String> toInfixExpression(String s){
    //定义一个List，存放中缀表达式对应的内容
    List<String> ls = new ArrayList<String>();
    int i = 0; //指针，用来遍历中缀表达式字符串
    String str; //对多位数的拼接
    char c; //每遍历一个字符，就放入c
    do{
        //如果c是一个非数字，我们就加入到ls中去
        if((c = s.charAt(i)) < 48 || (c = s.charAt(i)) > 57 ){
            ls.add("" + c);
            i++;//i需要后移
        }else{//如果是一个数，需要考虑多位数的问题    
            str = "";//先将str置成空串" "
            while(i < s.length() && (c = s.charAt(i)) >= 48 || (c = s.charAt(i)) <= 57 ){
                str += c;//拼接
                i++;
            }
            ls.add(str);
        }
    }while(i < s.length())
    return ls;
}
```
### Queue 
- 队列介绍
    - 队列是一个有序列表，可以用数组或者是链表来实现。
    - 遵循先入先出的原则。即：先存入队列的数据，要先取出。后存入的要后取出。
- 数组模拟队列
    - 队列本身是有序列表
    - 因为队列的输出、输入是分别从前后端来处理，因此需要两个变量front和rear分别记录队列前后端的下标，front会随着数据输出而改变，而rear则是随着数据输入而改变
```java
public class ArrayQueueDemo{
    public static void main(String[] args){
    //测试一把
    //创建一个队列
    ArrayQueue queue = new ArrayQueue(3);
    char key = ' ';//接受用户输入
    Scanner scanner = new Scanner(System.in);
    boolean loop = true;
    //输出一个菜单
        while(loop){
            System.out.println("s(show):显示队列");
            System.out.println("e(exit):退出程序");
            System.out.println("a(add):添加数据到队列");
            System.out.println("g(get):从队列取出数据");
            System.out.println("h(head):查看队列头的数据");
            key = scanner.next().charAt(0);//接受一个字符
            switch(key){
                case 's':
                    queue.showQueue();
                    break;
                case 'a':
                    System.out.println("输出一个数");
                    int value = scanner.nextInt();
                    queue.addQueue(value);
                    break;
                case 'g':
                    try{
                        int res = queue.getQueue();
                        System.out.printf("取出的数据是：" res);
                    }catch(Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'h':
                    try{
                        int res = queue.headQueue();
                        System.out.printf("队列头数据是：" res);
                    }catch(Exception e){
                        System.out.prinltn(e.getMessage());
                    }
                    break;
                case 'e':
                    scanner.close();
                    loop = false;
                    break;
                default:
                    break;
            }
        }
    System.out.println("程序退出");
    }
}
//使用数组模拟队列-编写一个ArrayQueue类
class ArrayQueue{
    private int maxSize;//数组的最大容量
    private int front;//队列头
    private int rear;//队列尾
    private int[] arr;//该数组用于存放数据，模拟队列

    //创建队列的构造器
    public ArrayQueue(int maxSize){
        this.maxSize = maxSize;
        arr = new int[maxSize];
        front = -1;//指向队列头部，分析出front是指向队列头的前一个位置
        rear = -1;//指向队列尾，指向队列尾的数据（即就是队列最后一个数据）
    }
    //判断队列是否为满
    public boolean isFull(){
        return rear == maxSize-1;
    }
    //判断队列是否为空
    public boolean isEmpty(){
        return rear == front;
    }
    //添加数据到队列
    public void addQueue(int n){
        //判断队列是否满
        if(isFull()){
            System.out.println("队列满，不能加入数据")；
            return;
        }
        rear++;//让rear后移
        arr[rear] = n;
    }
    //获取队列的数据，数据出队列
    public int getQueue(){
        //判断队列是否为空
        if(isEmpty()){
            //通过抛出异常
            throw new RuntimeException("队列空，不能获取数据");
        }
        front++;//front后移
        return arr[front];
    }
    //显示队列的所有数据
    public void showQueue(){
        //遍历
        if(isEmpty()){
            System.out.println("队列空的，没有数据");
            return;
        }
        for(int i = 0; i < arr.length; i++){
            System.out.printf("arr[%d] = %d\n",i,arr[i]);
        }
    }
    //显示队列的头数据，注意不是取出数据
    public int headQueue(){
        //判断
        if(isEmpty()){
            throw new RuntimeException("队列空的，没有数据")
        }
        return arr[front + 1];
    }
}    
```
- 问题分析并优化
    1. 目前数组使用一次就不能用了，没有达到复用的效果
    2. 将这个数组使用算法，改进成一个环形的队列 取模：%
    - 思路
        1. front变量的含义做一个调整: front就指向队列的第一个元素，也就是说arr[front]就是队列的第一个元素
        2. rear变量的含义做一个调整：rear指向队列的最后一个元素的后一个位置。因为希望空出一个空间作为约定。
        3. 当队列满时，条件是(rear + 1) % maxSize == front【满】
        4. 队列为空的条件，rear == front 空
        5. 当我们这样分析，队列中有效的数据的个数 (rear + maxSize - front) % maxSize
```java
public class ArrayQueueDemo{
    public static void main(String[] args){

    }
}

class CircleArray(){
    private int maxSize;//数组的最大容量
    // front变量的含义做一个调整: front就指向队列的第一个元素，也就是说arr[front]就是队列的第一个元素
    private int front;//队列头
    //rear变量的含义做一个调整：rear指向队列的最后一个元素的后一个位置。因为希望空出一个空间作为约定。
    private int rear;//队列尾
    private int[] arr;//该数组用于存放数据，模拟队列
    
    public CircleArray(int arrMaxSize){
        this.maxSize = maxSize;
        arr = new int[maxSize];
        front = 0;
        rear = 0;
    }
    //判断队列是否为满
    public boolean isFull(){
        return (rear + 1) % maxSize == front;
    }
    //判断队列是否为空
    public boolean isEmpty(){
        return rear == front;
    }
    //添加数据到队列
    public void addQueue(int n){
        //判断队列是否满
        if(isFull()){
            System.out.println("队列满，不能加入数据")；
            return;
        }
        //直接将数据加入就可以了
        arr[rear] = n;
        //将rear后移，这里必须考虑取模。
        rear = (rear + 1) % maxSize;
    }
    //获取队列的数据，数据出队列
    public int getQueue(){
        //判断队列是否为空
        if(isEmpty()){
            //通过抛出异常
            throw new RuntimeException("队列空，不能获取数据");
        }
        //这里需要分析出front是指向队列的第一个元素
        //1.先把front对应的值保存到一个临时变量
        //2.将front后移
        //3.将临时保存的变量返回
        int value = arr[front];  
        front = (front + 1) % maxSize;
        return value;
    }
    //显示队列的所有数据
    public void showQueue(){
        //遍历
        if(isEmpty()){
            System.out.println("队列空的，没有数据");
            return;
        }
        //思路：从front开始遍历，遍历多少个元素
        //动脑筋
        for(int i = front; i < front + size(); i++){
            System.out.printf("arr[%d] = %d\n",i % maxSize, arr[i % maxSize ]);
        }
    }
    //求出当前队列有效数据个数
    public int size(){
        //rear = 1
        //front = 0
        //maxSize = 3
        return (rear + maxSize - front) % maxSize;
    }
    //显示队列的头数据，注意不是取出数据
    public int headQueue(){
        //判断
        if(isEmpty()){
            throw new RuntimeException("队列空的，没有数据")
        }
        return arr[front ];
    }
}
```

## Priority Queue
>- [一步步地分析排序——优先队列和堆排序](https://blog.csdn.net/u010707039/article/details/103936796)

![](https://img-blog.csdnimg.cn/20200111162213756.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA3MDcwMzk=,size_16,color_FFFFFF,t_70)

### 定义和使用场景
优先队列是一个抽象数据类型，和栈、队列类似，它们都是抽象数据类型，相当于一个Java类，有自己的属性，并对外提供API。在了解它有什么API之前，先来看看优先队列的使用场景。

优先队列适用于需要对集合不断地执行插入元素、删除最大（或最小）元素的场景。这个场景大体可以分为两类：
- 第一类是业务实际情况需要，比如CPU的任务调度，待执行的任务是一个集合，每启动一个新程序就是在向集合里面插入元素。当前程序执行完后，就要从集合里面取出下一个优先级最高的程序。不断地有程序被启动和被执行，就像不断地对集合执行插入、删除最大元素的操作。
- 第二类场景是“从N个元素里获取最大的M个元素，N很大，不能一次性全部读进内存”，比如从银行成百上千万条交易记录里面找到金额最大的10笔交易；或者从全国的手机号码里面找到使用年限最长的10个号码。对于第二类场景，问题本身并不需要不断地对集合进行插入、删除操作。如果内存没有限制的话，你可以一次性将数据全部装进集合，然后随便选择一个排序算法对集合进行降序排列，接着输出最前面的10个元素。但是由于待处理的数据量过大（相对内存而言），不能使用排序算法解决该类问题，以银行交易记录为例子，你可以用优先队列通过如下步骤解决：
  1. 创建一个容量为11的集合
  2. 向集合里插入一笔交易记录，如果插入后集合的元素达到11个，删除金额最小的一笔交易（**需要注意的是，如果要获取最大的M个元素，在删除时删除的是最小的；而如果获取最小的M个元素，在删除时删除的是最大的，是反过来的**）
  3. 循环执行步骤2，直到所有交易记录遍历完毕
  4. 逐一输出集合里的所有交易记录，此时输出的便是成百上千万条交易记录里面金额最大的10笔交易，而且输出的数据是升序的

整个流程如下图所示，为了绘图方便，这里取了比较小的值，N为9，M为3：
![](https://img-blog.csdnimg.cn/20200111150819273.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA3MDcwMzk=,size_16,color_FFFFFF,t_70)

此时的集合就像是一个过滤器，如果新插入的元素比集合里面所有的元素都小，在下一次删除元素时，这个元素会被删除，就像它从来没出现过。如果新插入的元素比集合里原有的任一元素大，在下一次删除元素时，集合里面最小的元素会被“排挤”出去。

不论是哪一类场景，都是在不断地（不一定是轮流进行，没有必然的规律）对集合进行插入、删除最大（或最小）的元素，这就是优先队列的典型应用
### 抽象API
通过前文的使用场景可以看到，主要的操作就是插入和删除最小的元素，因此优先队列的API可以这样定义：
```java
// 插入元素
void insert(int a);

// 删除最大元素
int deleteMin();
```
以上是实现一个优先队列必要的API，当然如果你有需要，可以定义其它的API。
### 优先队列使用示例
以下代码示例了如何使用优先队列解决“从N个元素里面获取最大的M个元素”的问题，代码来自《算法》第四版中文版，删减了一些不需要的东西。
```java
public class TopM{
    publi static void main(String[] args){
        int M = Integer.parseInt(args[0]);
        // 注意了，需要(M+1)个空间
        MinPQ<Transaction> pq = new MinPQ<Transaction>(M+1);
        
        // 循环地向优先队列里插入元素
        while（StdIn.hasNextLine()){
            pq.insert(new Transaction(StdIn.readLine()));
            // 当优先队列里已经有（M+1）个元素，删除最小的那个
            if(pq.size() > M){
                pa.deleteMin();
            }
        }// 循环结束，最大的M个元素都在优先队列里面
        
        // 输出优先队列里的元素
        while(!pq.isEmpty()){
            StdOut.println(pq.deleteMin());
        }
    }
}
```
### 计算API的调用次数
由于我们还没谈到具体如何实现这两个API，所以暂时无法计算时间复杂度，但是我们可以计算从N个元素里面获取最大的M个元素时，插入和删除操作执行的次数。整个过程可以分为如下阶段：

1. 遍历待处理的元素：就是让待处理的元素都被集合处理一次，这一阶段还可以进一步分为两个阶段：
   1. 装满集合：集合里的元素从0个到M个，这一阶段共调用insert()M次。
   2. 过滤元素：集合元素达到M个后，每插入一个新元素，就要从集合里面删除最大的元素，该部分调用insert()和deleteMin()各(N-M)次。
2. 输出集合里的元素：这一阶段其实就是输出结果，共调用deleteMin()M次，集合元素从M个到0个。

这是为我们后续计算具体实现方案的时间复杂度作铺垫。
### 传统方式实现优先队列
优先队列最简单的实现方案就是使用有序或无序的数组（或链表），以下列举每个方案实现上述API的逻辑：
| 实现方案 | insert()                                       | deleteMin()                                                                                                                |
| -------- | ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| 有序数组 | 相当于执行插入排序的一次插入，元素按照降序排列 | 删除数组尾部的元素                                                                                                |
| 无序数组 | 向数组尾部插入一个元素              | 相当于执行选择排序的一次选择，找到最小的元素后，将它和数组尾部的元素互换位置，此时最小元素在数组尾部，直接删除尾部元素即可 |
| 有序链表 | 相当于执行插入排序的一次插入，元素按照升序排列 | 删除表头元素                                                                                                         |
| 无序链表 | 向链表头部插入一个元素              | 相当于执行选择排序的一次选择，找到最小的元素之后直接删除即可                                 |

接着列举每个方案的时间复杂度（假设元素总数为N，需要获取最大的M个元素，这里的N是远大于M的）：

| 实现方案 | 一次insert() | 一次deleteMin() | 从N个元素里面获取最小的M个元素的总时间复杂度 |
| -------- | ------------ | --------------- | -------------------------------------------- |
| 有序数组 | O(M)         | O(1)            | M² + (N-M)*M + (N-M) + M = O(NM)            |
| 无序数组 | O(1)         | O(M)            | M + (N-M) + (N-M)*M + M² = O(NM)            |
| 有序链表 | O(M)         | O(1)            | M² + (N-M)*M + (N-M) + M = O(NM)            |
| 无序链表 | O(1)         | O(M)            | M + (N-M) + (N-M)*M + M² = O(NM)            |

小结：

- 可以看出，使用数组还是链表差异不大，主要是有序和无序的差异。
- 进行一次插入或删除操作的时间复杂度和集合的大小（即M）成线性关系，解决整个问题的全部时间复杂度和待处理元素数量N和集合大小M的乘积即（NM）成线性关系。

在真实的工程应用里，单次操作的时间复杂度为线性是不可以接受的，单次操作一般都要求对数关系的时间复杂度，于是有了接下来要谈的实现方案。

### 二叉堆的定义
>声明：为了在后续二叉堆的介绍里面，更符合直觉和习惯，前文我们分析的都是实现deleteMin()，从现在起按照实现**deleteMax()** 来进行分析。

二叉堆是一个存储在数组里的堆有序的完全二叉树，第一个元素存放在数组下标1的位置。这句话有几个关键词：**堆有序的完全二叉树、存在数组里、下标1。** 他们分别从顺序、结构、存储方式对二叉堆进行了约束。

- **顺序性**：堆有序的二叉树，是指二叉树里的每个结点都大于等于它的两个子结点。比如一个由1、2、3三个元素构成的二叉树，如果要符合堆有序，根结点必须是3，至于1和2谁左谁右，没有关系。注意它和二叉查找树的差异，如果是两层的二叉查找树的话，三个元素只有一个摆放方式：2是根结点，1是左子结点，3是右子结点。然而堆有序的二叉树只要求父结点大于等于两个子结点，对两个子结点的大小关系没有约束。下图示例了二叉堆和二叉查找树的区别：
  ![](https://img-blog.csdnimg.cn/20200111153248197.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA3MDcwMzk=,size_16,color_FFFFFF,t_70)
- **结构性**：二叉堆是一棵完全二叉树，也就是在堆有序的二叉树的前提下，加上完全二叉树的约束。
- **存储方式**：不论一棵二叉树是否是完全二叉树，用链式结构存储都有一个问题：从父结点找子结点容易，从子结点找父结点不方便（除非在每个结点添加指向其父结点的指针）。而由于完全二叉树的特殊性，可以直接使用数组存放，父子结点之间不用任何指针，它们之间有算术上的关系，可以通过算术关系找到任意结点的父、子结点。那么为什么要在下标1呢？根据完全二叉树的性质，如果第一个元素在下标0上，则第k个元素的两个子元素为2k+1和2k+2，父元素为k/2（当k为奇数）或（（k/2）-1）（当k为偶数），即父元素有两类情况。但如果第一个元素在下标1上，则第k个元素的两个子元素为2k和2k+1，父元素为k/2，即父元素只有一类情况。也就是为了方便计算。
#### 二叉堆的特性
除了上述提到的父子结点的算术关系，我们还可以列举几个二叉堆的特性，加深我们对二叉堆的理解。

- **大顶堆、小顶堆**：如果一个二叉堆，每个结点都大于或等于它的两个子结点，这个二叉堆称为大顶堆；反之，如果一个二叉堆，每个结点都小于或等于它的两个子结点，这个二叉堆称为小顶堆。
- 对于大顶堆，从任一结点随便选一条路径往下走到叶子结点，可以得到一个降序序列；反之，从任意叶子结点往上走到根结点，可以得到一个升序序列。
- **一个降序排列的数组其实就是一个大顶堆，一个升序排列的数组就是一个小顶堆。**
### 恢复堆的有序性——上浮
既然二叉堆作为一个集合（数组即集合），那么在对集合进行增删改的时候，可能会破坏二叉堆原有的顺序性。比如直接将某个结点的值修改为一个比它的父结点更大的值；向数组尾部插入一个新结点，新结点的值比它的父结点大；直接修改某个结点的值让它比其中一个子结点的值小。**修改集合的方式有很多，但是造成的结果可以归为两类：某个结点变大或变小。**那么当二叉堆的顺序被破坏后，如何恢复堆的有序性？

如果某个结点的值变得比它的父结点更大，对于以该结点为根结点的子堆，显然还是符合二叉堆的特性的，只是该结点和它的父结点、祖先结点的位置关系不对，此时只要将该结点和其父结点互换位置即可。接下来面临同样的问题，如果该结点仍然比它新的父结点更大该怎么办，自然是继续互换它们的位置，直到遇到比它大的父结点，或者该结点最终变成了根结点。整个过程就像该结点沿着路经在往上爬，或者说是我们将该结点浮上去了，这个操作称为“上浮”，**上浮操作可以在我们向二叉堆插入新元素后，恢复二叉堆的有序性。**下图示例了上浮操作的过程：
![](https://img-blog.csdnimg.cn/20200111153450444.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA3MDcwMzk=,size_16,color_FFFFFF,t_70)
#### 上浮操作示例代码
```java
/*
 * 方法说明：“上浮”算法要实现的，是对位置k的结点执行“上浮”操作，将其“上浮”到合适的位置。
 * 对引用到的两个方法说明如下：
 * less(int p, int q)方法，如果下标为p的元素小于下标为q的元素，则返回true，否则返回false。
 * exchange(int p, int q)方法，互换数组里面下标为p和q的两个元素。
 */
private void swim(int k){
    // 循环判定条件：只要k还没到达根结点，且k的子结点比他还小，k就要继续往上浮
    while( k>1 && less(k/2,k) ){
        exchange(k/2, k);
        k = k/2;
    }
}
```
#### 时间复杂度分析
我们以执行“比较操作”的次数表示时间复杂度，根据完全二叉树的性质，如果根结点算作第一层的话，位置k的结点在⌊log2k⌋ + 1层。最坏的情况是在数组尾部插入新元素并且要上浮到根结点，如果结点总数为N，则要爬⌊log2N⌋层，每爬一层之前都要比较一次，爬到根结点时不用再对两个结点进行比较了，所以比较次数为⌊log2N⌋，即时间复杂度为O(logN)。
### 恢复堆的有序性——下沉
如果某个结点的值变得比它的其中一个子结点小（也有可能比它的两个子结点都小），对于该结点往上的所有结点，显然还是符合二叉堆的特性，只是该结点和它的子结点、子孙结点的位置不对。此时只要将该结点和它的两个子结点里较大的那个互换位置即可。接下来面临同样的问题，如果该结点的两个新子结点里仍然有比该结点更大的怎么办，自然是继续和它的两个子结点里较大的那个互换位置，直到该结点比它的两个子结点都大，或者该结点变成了叶子结点。整个过程就像该结点沿着路经往下滑，或者说是我们将该结点沉下去了，这个操作称为“下沉”。下图示例了下沉操作的过程：
![](https://img-blog.csdnimg.cn/20200111153646774.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA3MDcwMzk=,size_16,color_FFFFFF,t_70)
#### 下沉操作代码
```java
/*
 * 方法说明：“下沉”算法要实现的，是对位置k的结点执行“下沉”操作，将其“下沉”到合适的位置。
 * 对引用到的两个方法说明如下：
 * less(int p, int q)方法，如果下标为p的元素小于下标为q的元素，则返回true，否则返回false。
 * exchange(int p, int q)方法，互换数组里面下标为p和q的两个元素
 */
private void sink(int k){
    // 循环判断条件，位置k是否还有子结点
    while(2*k<=N){
        int j = 2*k; // j指向k的第一个子结点
        // 如果位置k的元素有两个子结点，且第二个子结点大于第一个，将j指向第二个子结点
        if( j<N && less(j, j+1) ) {
            j++;
        }
        // 当代码走到这里，不论k有一个还是两个子结点，j已经指向最大的那个子结点
        // 如果结点k大于它所有的子结点，结束
        if(less(j, k)) {
            break;
        }
        // 否则，将k和其子结点位置交换
        exchange(k, j);
        k = j;
    }
}
```
#### 时间复杂度分析
我们仍以执行“比较操作”的次数表示时间复杂度，根据完全二叉树的性质，如果根结点算作第一层的话，位置k的结点在⌊log2k⌋ + 1层。最坏的情况是从根结点下沉到树的底层，如果结点总数为N，则要下沉⌊log2N⌋层。每下沉一层都要比较两次，所以比较次数为2*⌊log2N⌋次，即时间复杂度为O(log2N)。
### 二叉堆实现优先队列API
其实当我们介绍了如何使用上浮和下沉操作恢复二叉堆的有序性后，你大概就知道该怎么使用二叉堆来实现优先队列了。对于insert()，只要将元素插入到数组尾部，然后对该元素执行上浮操作即可。

对于deleteMax()，只要删除二叉堆的根结点，就能输出集合里最小的元素，然后，将数组尾部的元素移到根结点，对根结点执行下沉操作即可恢复堆的有序性。
>删除这里有一个很有意思的一点就是，选择将根结点的元素和数组尾部的元素进行交换，然后删除根结点，并将队尾元素`sink()`即可。
#### 示例代码
```java
// 向优先队列插入一个元素
public void insert(Key v){
    pq[++N] = v; // 向数组末尾插入一个元素
    swim(N);     // 将刚插入的元素上浮到正确位置
}

// 从优先队列删除最大元素
public Key deleteMax(){
    Key max = pq[1];    // 保存堆顶的元素
    exchange(1, N--);   // 堆尾元素移到堆顶
    pq[N+1] = null;     // 删除堆尾的元素
    sink(1);            // 将堆顶元素下沉到正确位置
    return max;
}
```
#### 时间复杂度分析
前文已经分析过单次insert()或deleteMax()的时间复杂度，均是O(logN)，现在来分析解决整个“从N个元素里面获取最小的M个元素”问题的时间复杂度，注意现在分析的是deleteMax()所以是获取最小的M个元素。我们按照前文计算优先队列API调用次数时分成两个阶段来计算，先引入一个算式方便我们计算：
当N = 2k-1时（这个约束其实就是要求N的值刚好是满二叉树的结点数量），有`⌊log21⌋ + ⌊log22⌋ + … + ⌊log2N⌋ = 0 + 1 + 1 + 2 + 2 + 2 + 2 + 3 + 3 +3 +3 +3 +3 +3 + 3 + 4…`这个算式其实是有规律的，当N刚好对应满二叉树的结点数量时，值为 (N+1)*log2(N+1)-2N。
这个算式是计算时间复杂度的一部分，表达的是一个增长趋势，对于一棵完全二叉树，底层元素个数是1个和完全铺满的情况，计算差异可能很大，但是底层不论有几个元素，耗时不会超过铺满的情况。

最优情况是输入是降序的：

1. 遍历待处理的元素：
   1. 装满集合：数组从0个元素增加到M个元素，每插入一个元素，只需进行一次比较，时间复杂度为M。
   2. 过滤元素：在含有M个元素的二叉堆里交替执行insert()和deleteMax()，注意下沉操作需要进行两次比较，时间复杂度为`(N-M) + (N-M)2log2M`。
2. 输出集合里的元素：调用deleteMax()，数组元素从M个缩减为0个，删除第一个元素之后，是在（M-1）个元素里执行下沉，所以执行下沉操作的总次数为`2*(⌊log2(M-1)⌋ + ⌊log2(M-2)⌋ + … + ⌊log21⌋)` ，取(M-1)刚好是满二叉树的情况，使用前面的算式，得到`(2Mlog2M) - 4(M-1)`。

求和之后得到`N + 2Nlog2M - 4M + 4 ≈ O(Nlog2M)`

最坏的情况是输入序列是升序的：

1. 遍历待处理的元素：
   1. 装满集合：数组从0个元素增加到M个元素，从第二个元素开始（第一个元素没有可上浮的），每插入一个元素，都要从数组末尾上浮到根结点，时间复杂度为`⌊log21⌋ + ⌊log22⌋ + ⌊log23⌋ + … + ⌊log2(M-1)⌋ = (Mlog2M) - 2(M-1)`。
   2. 过滤元素：在含有M个元素的二叉堆里交替执行insert()和deleteMax()，时间复杂度为(N-M)log2M + (N-M)2log2M。
2. 输出集合里的元素：调用deleteMax()，数组元素从M个缩减为0个，删除第一个元素之后，是在（M-1）个元素里执行下沉，所以时间复杂度为`2*(⌊log2(M-1)⌋ + ⌊log2(M-2)⌋ + … + ⌊log21⌋) = (2Mlog2M) - 4(M-1)`。

求和之后得到`3Nlog2M - 6M + 6 ≈ O(Nlog2M)`。






















# Algorithms
## Arbitrary Substitution Principle
- When you have a choice of writing two different programs by substituting one variable (or argument) for another... 
  -  ... then the arbitrary substitution principle (ASP) applies. 
- You should always be on your guard in such circumstances: in my opinion, you should annotate such code with a comment or annotation such as @asp. 
- Failure so to do may result in you getting bitten!
- **What is a non-arbitrary substitution?**
  - Suppose you write: 
    - int x = a + b 
  - What would happen if you wrote instead: 
    - int x = b + a 
  - You’d be writing the exact same program because the + operator commutes. 
  - The alternative substitution here is immaterial so cannot be said to be chosen arbitrarily.
- **What is an arbitrary substitution?**
  - Suppose you write: 
    - boolean y = a(x) && b(x) 
    - where a(x) is a boolean function and b(x) is a different boolean function. 
  - What would happen if you wrote instead: 
    - boolean y = b(x) && a(x) 
  - You’d be writing a different program because the && operator does not commute. [That’s because it is a “short-circuit” operator.] 
  - To another programmer reading this code, it is not at all obvious why you chose the particular form that you did. I think that programmer is entitled to an explanation, or at least a comment to the effect that you think it doesn’t matter.
  - **Does it really matter?**
  - If a(x) and b(x) are pure functions (i.e. no side-effects), then it might not matter
    - But suppose that a(x) takes 10 μsec while b(x) takes 10 msec, it might matter quite a lot. 
  - Choosing one option arbitrarily over another should always be considered a code smell. 
  - Not only that, but such a situation can lead to an insight that improves performance:
    - Example: **Weighted Quick Union**.



## Entropy
>$-\sum_{i=1}^np(x_i)logp(x_i)$
- [信息熵及其相关概念](https://blog.csdn.net/am290333566/article/details/81187124)
- 信息论之父克劳德·香农给出的信息熵的三个性质：
  1. 单调性，发生概率越高的事件，其携带的信息量越低；
  2. 非负性，信息熵可以看作为一种广度量，非负性是一种合理的必然；
  3. 累加性，即多随机事件同时发生存在的总不确定性的量度是可以表示为各事件不确定性的量度的和，这也是广度量的一种体现。
- ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200223145600.png)

## BIg O
- ForeWord
  - ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200121130014.png)
- 大O表示法是一种特殊的表示法，指出了算法的速度有多快
-  推导大O阶方法
   1. 用常数1取代运行时间中的所有加法常数。
   2. 在修改后的运行次数函数中，只保留最高阶项。
   3. 如果最高阶项存在且不是1，则去除与这个项目相乘的常数。得到的结果就是大O阶。
- 常见的大O运行时间
  
  
    Big-O | Name | Description
    ------| ---- | -----------
    **O(1)** | constant | **This is the best.** The algorithm always takes the same amount of time, regardless of how much data there is. Example: looking up an element of an array by its index.
    **O(log n)** | logarithmic | **Pretty great.** These kinds of algorithms halve the amount of data with each iteration. If you have 100 items, it takes about 7 steps to find the answer. With 1,000 items, it takes 10 steps. And 1,000,000 items only take 20 steps. This is super fast even for large amounts of data. Example: binary search.
    **O(n)** | linear | **Good performance.** If you have 100 items, this does 100 units of work. Doubling the number of items makes the algorithm take exactly twice as long (200 units of work). Example: sequential search.
    **O(n log n)** | "linearithmic" | **Decent performance.** This is slightly worse than linear but not too bad. Example: the fastest general-purpose sorting algorithms.
    **O(n^2)** | quadratic | **Kinda slow.** If you have 100 items, this does 100^2 = 10,000 units of work. Doubling the number of items makes it four times slower (because 2 squared equals 4). Example: algorithms using nested loops, such as insertion sort.
    **O(n^3)** | cubic | **Poor performance.** If you have 100 items, this does 100^3 = 1,000,000 units of work. Doubling the input size makes it eight times slower. Example: matrix multiplication.
    **O(2^n)** | exponential | **Very poor performance.** You want to avoid these kinds of algorithms, but sometimes you have no choice. Adding just one bit to the input doubles the running time. Example: traveling salesperson problem.
    **O(n!)** | factorial | **Intolerably slow.** It literally takes a million years to do anything.  

    ![Comparison of Big O computations](https://upload.wikimedia.org/wikipedia/commons/7/7e/Comparison_computational_complexity.svg)
- Reference
  - [十分钟搞定时间复杂度](https://www.jianshu.com/p/f4cca5ce055a)
  - [从经典算法题看时间复杂度](https://zhuanlan.zhihu.com/p/73731500)
  - [如何理解算法时间复杂度的表示法，例如 O(n²)、O(n)、O(1)、O(nlogn) 等？](https://www.zhihu.com/question/21387264)
  - [各种排序算法的时间复杂度](https://blog.csdn.net/qq_30815237/article/details/90766878)


## Recursion（递归）
递归用于解决什么问题？
- 数学问题：八皇后问题，汉诺塔，阶乘，迷宫，球和篮子
- 算法中也会用到递归：快排，归并，二分查找
- 将用栈解决的问题，用递归比较简洁

递归需要遵守的重要规则
- 执行一个方法时，就创建一个**新的受保护的独立空间**（栈空间）
- 方法的局部变量是**独立**的，不会相互影响
- 如果方法中使用的是引用类型的变量（比如数组），就会共享该引用类型的数据
- 递归必须向退出递归的条件**逼近**，否则就是无限递归，陷入死循环
- 当一个方法执行完毕，或者遇到return,就会返回，遵守谁调用，就把结果返回给谁。同时当方法执行完毕或者返回时，该方法也就执行完毕。


## Union-Find
union find is a set of algorithms for solving the so-called dynamic connectivity problem
- **Reference**: [CSDN-QuickFind & QuickUnion](https://blog.csdn.net/sinat_25991865/article/details/100533334)

### Quick Find
- At the Begining, we do not use tree structure to store the data, so if we wanna link 2 nodes, we need to chage every ids of the nodes
- So called **Eager Algorithm**
- Data Structure used to support is an integer array indexed by object
- ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200130115043.png)
- P and q belong to the same connected component if and only if id [p] and id [q] are equal.
- ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200130115229.png)
- 查找操作就只要判断 id 是否相等即可，合并则需要把和 id[p] 相等的所有 id 都改成 id[q] 。
```java
  public class QuickFindUF{
      private int[] id;
      public QuickFindUF(int N){
          //set id of each objects to itself(N array accesses)
          id = new int[N];
          for(int i = o; i < N; i++){
              id[i] = i;
          }    
      }
      public boolean connected(int p, int q){
          //check whether p and q are in the same component(2 array accesses) 
          return id[p] == id[q];
      }
      public void union(int p, int q){
          int pid = id[p];
          int qid = id[q];
          for(int i = 0; i < id.length; i++){
              //change all entried with id[p] to id[q](at most 2N+2 array accesses)
              if(id[i] == pid){
                  id[i] = qid;
              }
          }
      }
  }
```
- 其中`union`的for写成下面这样是错的，id[p] 在循环中变成了 id[q] ，原来相等的关系变成不等，导致数组中排在其后面的本应改变的对象无法更新 id 。举例来说，上面那张图要是这样合并 5 和 9 的话， 6 与 7 的 id 就还会是 1 ， 而不会更新成 8 。
- 这种实现，查找操作很快，但对 n 个对象进行 n 次合并需要访问数组 n^2 次，平方级别对大规模的数据来说是**不可接受的**。
```java
for (int i = 0; i < id.length; i++) {
    if (id[i] == id[p]) {
        id[i] = id[q];
    }
}
```
- Quadratic algorithms do not scale with technology  
- QuickFind is too **slow** for huge problems
### Quick Union
- so called **lazy approach** to algorithm desgin where we try to avoid doing work until we have to
- 将连通分量抽象成树， id[p] 表示 p 的父节点。
- ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200130122351.png)
- 查找操作要检查 p q 是否有相同的根节点，合并操作则只要把 p 根节点的父节点改成 q 的根节点即可，只改变了 id[] 中的一个值。
- ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200130122415.png)
```java
public class QuickUnionUF{
    private int[] id;
    public QuickUnionUF(int N){
        //set id of each object to itself(N array accesses)
        id = new int[N];
        for(int i = 0; i < N; i++){
            id[i] = i;
        }
    }
    private int root(int i){
        //chase parent pointers until reach root(depth of i array accesses)
        while(i != id[i]){
            i = id[i];
        }
        return i;
    }
    public boolean connected(int p, int q){
        //check if p and q have same root(depth of p and q array accesses)
        return root(p) == root(q);
    }
    public void union(int p, int q){
        //change root of p to point to root of q(depth of p and q array accesses)
        int i = root(p);
        int j = root(q);
        id[i] = j;
    }
}
```
### Conclusion
Defects
  - QuickFind defect
    - Union too expensice(N array accesses)
    - trees are flat, but too expensive to keep them flat
  - QuickUnion defect
    - Trees can get tall
    - Find too expensive(could be N array accesses)
- Algorithm | Initialize | union | find
  :---: | :---: | :---: | :---:
  quick-find | N | N | 1
  quick-union | N | N† | N
  † includes cost of finding roots
### Improvement
- improvement 1: **Weighting**（带权算法 ）
  - ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200130124238.png)
  - Modify quick-union to avoid tall trees.
  - Keep track of size of each tree (number of objects).
  - Balance by **linking root of smaller tree to root of larger tree**.
  - ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200119224130.png)
  - 这样一来任意的节点 x 的深度最多为 lgN （以 2 为底），N 为 100 w 时深度最多是 20， 10 亿时是 30 ，相对来说可以支持较大规模的数据了。
  - **至于为什么是 lgN ，可以粗略的这么想：节点 x 的深度只有在其所在的树 T1 被合并到另一个更大的树 T2 时才会加一，而 size(T2) >= size(T1)，那么节点 x 所在的树的大小至少会变成两倍。而总共 N 个节点，最多可以两倍 lgN 次，深度加一 lgN 次，即深度最多为 lgN。**
  - 什么时候树的深度会至少翻倍呢，只有当两颗树合并的时候才会出现这种情况。当**一个节点加到一棵树上**的时候，**无论如何也不会出现深度增加的情况**。
  - ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200130124051.png)
  ```java
  //总体来说，weighted quick-union多了以下介个代码块

  //（由触点索引的）各个根节点所对应的分量的大小
  private int[] sz;
  public WeightedQuickUnionUf(int N){
      //...
      //此处省略
      sz = new int[N];
      for(int i = 0; i < N; i++){
          sz[i] = 1;
      }  
  }
  private int root(int p){
    //跟随链接找到跟节点
    while(p != id[p]){
        p = id[p];
    }
    return p;
  }
  public void union(int p, int q){
      int i = root(p);
      int j = root(q);
      if(i == j){
          return;
      }
      //将小树的根节点连接到大树的根节点
      if(sz[i] < sz[j]){
          id[i] = j;
          sz[j] += sz[i];
      }else{
          id[j] = i;
          sz[i] += sz[j];
      }
      count--
  }
  ```
- improvement 2: **Path Compression**（路径压缩）
  - 理想情况下，我们希望每个节点直接链接到根节点，但又不想像quick-find那样修改大量的链接。实现的方式很简单：就在检查节点的同时将这些节点全部链接到根节点。实现这样的路径压缩，只需要在find()添加一个循环，将在路径上遇到的所有节点直接连接到根节点上即可
  ```java
  private int root(int p){
  //跟随链接找到跟节点
  while(p != id[p]){
      id[p] = id[id[p]];
      p = id[p];
  }
  return p;
  }
  ```
- ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200203113926.png) 
## Sorts
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200315172500.png)
### How to choose a sort algorithm?
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200315172645.png)
### Ten Sorts we usually use
![](https://www.runoob.com/wp-content/uploads/2019/03/sort.png)
![](https://www.runoob.com/wp-content/uploads/2019/03/0B319B38-B70E-4118-B897-74EFA7E368F9.png)
- Big O for these sorts
  - 平方阶 **(O(n2))** 排序: 各类简单排序: 直接插入、直接选择和冒泡排序
  - 线性对数阶 **(O(nlog2n))** 排序: 快速排序、堆排序和归并排序
  -  **O(n1+§))** 排序，§ 是介于 0 和 1 之间的常数: 希尔排序
  - 线性阶 **(O(n))** 排序: 基数排序，此外还有桶、箱排序
- Stability
  - 稳定的排序算法: 冒泡排序、插入排序、归并排序和基数排序
  - 不稳定的排序算法: 选择排序、快速排序、希尔排序、堆排序
### Bubble Sort
  - 冒泡排序是一种简单的排序算法。它重复地走访过要排序的数列，一次比较两个元素，如果他们的顺序错误就把他们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端，如下图所示。
  - ![avatar](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20veW9qaWFrdS92aXN1YWxBbGdvcml0aG0vcmF3L21hc3Rlci9hbGdvcml0aG1Eb2N1bWVudC9saUppYVRvbmcvcGljL0J1YmJsZV9zb3J0X2FuaW1hdGlvbi5naWY)  
  - 冒泡排序对 n 个项目需要 O( n^2) 的比较次数，且可以原地排序。尽管这个算法是最简单了解和实现的排序算法之一，但它对于包含大量的元素的数列排序是很没有效率的。
- 下面就是冒泡排序的基本思路，从左到右比较，如果左边的数字大于右边的数字，调换两个数字的位置，往复如此。但是下面这个代码存在瑕疵，
```java
int[] values = {3,1,6,2,9,0,7,4,5,8};
int temp = 0;
for(int i = 0; i < value.length - 1; i++){
// 这里value.length-1是指只需要记数到倒数第二位就可以了，倒数第二位和最后一位进行比较就可以
    for(int j = 0; j < values.length -1 -i; j++){
// 这里values.length -1 -i是指当第一次排序完成后，最大的数字已经排到最右边了，所以这里后面不用在考虑这一个数字了，最外面的大循环排了几次就减去几次就可以
        if(values[j] > values[j+1]){
            temp = values[j];
            values[j] = values[j+1];
            values[j+1] = temp;
        }
        System.out.println(Arrays.toString(values));
    }
}
```
- 但是上面的方法存在瑕疵，有时存在已经排序好的情况但是循环还没有结束。所以需要进行一些小小的改动。加一个锁flag。
```java
nt[] values = {3,1,6,2,9,0,7,4,5,8};
int temp = 0;
for(int i = 0; i < value.length - 1; i++){
    boolean flag = true;
    for(int j = 0; j < values.length -1 -i; j++){
        if(values[j] > values[j+1]){
            temp = values[j];
            values[j] = values[j+1];
            values[j+1] = temp;
            flag = false;
//  这里flag的意思就是，当还存在交换的时候，我们就将flag设为flase，意思是交换还未完成，如果所有的排序和交换都已经完成了，flag的值不会改成flase，这个时候做出一个判断，并且跳出循环 
        }
        System.out.println(Arrays.toString(values));
    }
    if(flag){
        break;
    }
}
```
### Selection Sort
- **表现最稳定的排序算法之一**，因为**无论什么数据进去都是O(n2)的时间复杂度**，所以用到它的时候，数据规模越小越好。唯一的好处可能就是不占用额外的内存空间了吧。理论上讲，选择排序可能也是平时排序一般人想到的最多的排序方法了吧。
- 选择排序(Selection-sort)是一种简单直观的排序算法。它的工作原理：首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。
- n个记录的直接选择排序可经过n-1趟直接选择排序得到有序结果。具体算法描述如下：
  - 初始状态：无序区为R[1..n]，有序区为空；
  - 第i趟排序(i=1,2,3…n-1)开始时，当前有序区和无序区分别为R[1..i-1]和R(i..n）。该趟排序从当前无序区中-选出关键字最小的记录 R[k]，将它与无序区的第1个记录R交换，使R[1..i]和R[i+1..n)分别变为记录个数增加1个的新有序区和记录个数减少1个的新无序区；
  - n-1趟结束，数组有序化了。
- ![](https://www.runoob.com/wp-content/uploads/2019/03/selectionSort.gif)
```java
public class SelectSort {
    public static void main(String[] args) {
        int [] arr = {49,38,65,97,76,13,27,49};
        selectSort(arr,arr.length);
    }
 
    public static void selectSort(int [] arr,int n){
        for (int i = 0; i < n - 1; i++) {
            int index = i;
            int j;
            // 找出最小值得元素下标
            for (j = i + 1; j < n; j++) {
                if (arr[j] < arr[index]) {
                    index = j;
                }
            }
            int tmp = arr[index];
            arr[index] = arr[i];
            arr[i] = tmp;
            System.out.println(Arrays.toString(arr));
        }
 
    }
}
```
### Insertion Sort
- 插入排序（Insertion-Sort）的算法描述是一种简单直观的排序算法。它的工作原理是通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。插入排序在实现上，通常采用in-place排序（即只需用到O(1)的额外空间的排序），因而在从后向前扫描过程中，需要反复把已排序元素逐步向后挪位，为最新元素提供插入空间。
- 一般来说，插入排序都采用in-place在数组上实现。具体算法描述如下：
  - 从第一个元素开始，该元素可以认为已经被排序；
  - 取出下一个元素，在已经排序的元素序列中从后向前扫描；
  - 如果该元素（已排序）大于新元素，将该元素移到下一位置；
  - 重复步骤3，直到找到已排序的元素小于或者等于新元素的位置；
  - 将新元素插入到该位置后；
  - 重复步骤2~5。
- ![](https://www.runoob.com/wp-content/uploads/2019/03/insertionSort.gif)
```java
//Method 1
public static int[] sort(int[] ins){
    for(int i=1; i<ins.length; i++){
        for(int j=i; j>0; j--){
            if(ins[j]<ins[j-1]){
                int temp = ins[j-1];
                ins[j-1] = ins[j];
                ins[j] = temp;
            }
        }
    }
    return ins;
}
// Improvement
public void insertionSort() {
    for (int i = 1; i < array.length; i++) {
        int key = array[i];
        int j = i - 1;
        while (j >= 0 && array[j] > key) {
            array[j + 1] = array[j];
            j--;
        }
        array[j + 1] = key;
    }
}
```


### Shell Sort
希尔排序，也称递减增量排序算法，是插入排序的一种更高效的改进版本。但希尔排序是非稳定排序算法。
- 希尔排序是基于插入排序的以下两点性质而提出改进方法的：
  - 插入排序在对几乎已经排好序的数据操作时，效率高，即可以达到线性排序的效率；
  - 但插入排序一般来说是低效的，因为插入排序每次只能将数据移动一位；
  - 希尔排序的基本思想是：先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，待整个序列中的记录"基本有序"时，再对全体记录进行依次直接插入排序。
- STEP
  - 选择一个增量序列 t1，t2，……，tk，其中 ti > tj, tk = 1；
  - 按增量序列个数 k，对序列进行 k 趟排序；
  - 每趟排序，根据对应的增量 ti，将待排序列分割成若干长度为 m 的子序列，分别对各子表进行直接插入排序。仅增量因子为 1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。
- ![](https://www.runoob.com/wp-content/uploads/2019/03/Sorting_shellsort_anim.gif)
  ```java
  //希尔排序时，对有序序列在插入时采用交换法
  //这个方法比较慢，甚至比插入排序还要慢
  public static void ShellSort(int[] arr){
      int temp = 0;
      int count = 0;
      for(int gap = arr.length / 2; gap > 0; gap /= 2){
          for(int i = gap; i < arr.length; i++){
              for(int j = i - gap; j >= 0; j -= gap){
                  if(arr[j] > arr[j + gap]){
                      temp = arr[j];
                      arr[j] = arr[j + gap];
                      arr[j + gap] = temp;
                  }
              }
          }
      }
  }
  //希尔排序的改进，对有序序列在插入时采用移位法
  public static void ShellSort(int[] arr){
    for(int gap = arr.length / 2; gap > 0; gap /= 2){
        //从第gap个元素，逐个对其所在的组进行直接插入排序
        for(int i = gap; i < arr.length; i++){
            int j = i;
            int temp = arr[j];
                while(j - gap >= 0 && temp < arr[j - gap]){
                    //移动
                    arr[j] = arr[j - gap];
                    j -= gap;
                }
                //当退出while后，就给temp找到插入的位置
                arr[j] = temp;
        }
    }
  }

  ```
### Merge Sort
前言
- 归并排序是非常经典的基于分治法的递归排序算法。
- 和初级排序算法（冒泡、选择、插入）在最坏的情况下时间复杂度会达到O(N2)相比，归并排序在最坏的情况下仍有O(NlgN)的效率。
- 归并排序和另一个非常重要的排序方法：快速排序非常相似，都是基于分治法的递归排序，甚至于它们两者的过程近似于互补。如果你能很好地理解归并排序，会有助于以后快速排序的学习。

思路
- 归并排序（MergeSort），是创建在归并操作上的一种有效的排序算法，效率为O(nlogn) 。1945年由约翰·冯·诺伊曼首次提出。该算法是采用分治法（Divide and Conquer）的一个非常典型的应用，且各层分治递归可以同时进行。

复杂度
- 归并排序可用于大量数据的排序，对于 million 和 billion 级别的数据，插入排序难以完成的任务归并排序可能几分钟就完成了。
- 对于 N 个元素，归并排序最多需要 NlgN 次比较和 6NlgN 次对数组的访问，并且要使用 N 个空间的辅助数组。
- 平均时间复杂度：O(nlogn)
- 最坏时间复杂度：O(nlogn)
- 最优时间复杂度：O(nlogn)
- 最坏空间复杂度：O(n)

![](https://img-blog.csdn.net/20170612234617785?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlhb3BpbmcwOTE1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![](https://upload.wikimedia.org/wikipedia/commons/c/cc/Merge-sort-example-300px.gif)

![](https://upload.wikimedia.org/wikipedia/commons/c/c5/Merge_sort_animation2.gif)

实现

**普通的方法**
```java
public class MergeSort(){
    //合并的方法
    /**
    arr 排序的原始数组
    left 左边有序序列的初始索引
    mid 中间索引
    right 右边索引
    temp 做中转的数组
    **/
    public static void merge(int[] arr, int left, int mid, int right){
        //用于存储归并后的临时数组
        int[] temp = new int[right - left + 1];
        //用于记录第一个数组中需要遍历的下标
        int i = left;
        //记录第二个数组中需要遍历的下标
        int j = mid + 1;
        //用于记录在临时数组中存放的下标
        int index = 0;
        //遍历两个数组取出小的数字，放入临时数组中
        while(i <= mid && j <= right){
            //第一个数组的数据更小
            if(arr[i] <= arr[j]){
                //把小的数据放入临时数组中
                temp[index] = arr[i];
                //让下标向后移一位
                i++;
            }else{
                temp[index] = arr[j];
                j++;
            }
            index++;
        }
        //处理多余的数据
        while(j <= right){
            temp[index] = arr[j];
            j++;
            index++;
        }
        while(i <= mid){
            temp[index] = arr[i];
            i++;
            index++;
        }
        //把临时数组中的数据重新存入原数组
        for(int k = 0; k < temp.lenght; k++){
            arr[k + left] = temp[k];
        }
    }
    public static void mergeSort(int[] arr, int left, int right){
        int mid = (right + left) / 2;
        if(left < high){
        mergeSort(arr, left, mid);
        mergeSort(arr, mid+1, right);
        merge(arr, left, mid, right);
        }
    }
}
```
**原地归并的抽象方法**

归并排序里，一次归并的过程如前所述，很简单，但是这个思路只是纯理论式（理想化）的。因为“归并”是递归发生的行为（合并两个数组的代码会被多次调用），如果每一次归并都如前所述用一个新数组来装归并结果，整个归并的过程可能会创建大量临时数组，可能创建数组的时间花销比排序本身还大。为了解决这个问题而得出的另外一种实现归并过程的方法，就是原地归并的抽象方法。

实现思路

针对原始的归并方法存在的问题，解决的方法也并不复杂：通过反复利用同一个辅助数组来避免频繁地创建新数组。
原地归并的抽象方法如下图所示：

![](https://img-blog.csdnimg.cn/20190101112748202.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA3MDcwMzk=,size_16,color_FFFFFF,t_70)

说明：由于客户端一般都是传入一个待排序数组，不会传入两个数组，所以这里用角标low、middle、high将一个数组“划分”成两个数组。角标low ~ middle属于第一个子数组，角标middle+1 ~ high属于第二个子数组。

理解原地归并的抽象方法的重点在于：
**虽然每一次的归并都会暂时使用辅助数组，但是都是用完立即“归还”，元素会被归并（排序）到源数组，每一次的归并行为结束后，辅助数组总是空的。所以递归的过程可以反复使用同一个辅助数组。**
```java
void merge(int[] a, int low, int middle, int high){
    int i = low, j = middle+ 1;
    // 将源数组的元素复制到辅助数组
    for(int k = lo; k <= hi; k++){
        // 声明，aux[]表示一个定义在类里面（成员变量）的辅助数组
        aux[k] = a[k];
    }
    // 执行归并过程
    for(int k = low; k <= high; k++){
        if(i > mid){
            // 如果左边的子数组元素已经用尽
            a[k] = aux[j++];
        } else if (j > hi){
            // 如果右边的子数组元素已经用尽
            a[k] = aux[i++];
        } else if (aux[i] < aux[j]){
            a[k] = aux[i++];
        } else {
            a[k] = aux[j++];
        }
    }
}
```

以下开始分析这个算法的时间成本，主要分为“比较次数”和“访问数组的次数 ”。首先假设两个已经分别有序的子数组的长度加起来为N，即合并之后的数组长度为N。

**时间成本-比较次数**

最优情况：
如果左边的子数组最大的元素小于右边的子数组最小的元素，即a[middle] < a[middle+1]，单次归并的比较次数达到最少。因为此时只有左边的子数组里的元素和右边的子数组的第一个元素参与比较，当左边的子数组元素用尽（全部被复制回了源数组），右边的子数组的元素不需要参与比较就能直接被复制回源数组。如果两个子数组的长度总是相等，那么这个最少的次数就是(1/2N) 。

最糟糕情况：
如果两个子数组各自最大的元素，分别是这N个元素里面最大的和次大的元素，即a[mid]、a[high]大于其它所有元素（它们两个谁比较大没有关系），单次归并的比较次数达到最多。因为此时每复制一个元素回源数组，都会发生一次比较，只有最大那个元素被复制回源数组前的那一次比较可以免去（因为没有可以比较的元素了）。这个最多的比较次数是(N-1) = O(N) 。

一个比较极端的情况：
前面关于最优情况的算数结果，是基于左右两个子数组的长度相等。考虑一个极端情况：两个子数组的长度之和仍为N，但是左边的子数组元素只有一个（右边N-1个），且这唯一一个元素小于右边子数组的所有元素，此时最少的比较次数为1。从算数的角度来讲，为了保证严谨性，我们列举了极端情况，但是从实际使用的情况来讲，左右两个子数组的长度大多数相等或者相差一个。即使是在迭代法实现的递归排序里，两个子数组的长度可能会相差较多（主要是每一轮循环的最后两个子数组），但是一般不会这么极端。

**时间成本-访问数组的次数**

首先定义诸如a[i]、a[j]这种读取算是进行了一次数组访问，诸如a[i] = a[j]、a[i] < a[j]这种语句算是进行了两次数组访问。那么一次归并操作要访问数组的次数主要在三个方面：

- 将元素从源数组复制到辅助数组：每复制一个元素进行了两次数组访问，一共2N次。
- 将元素从辅助数组复制回源数组：同上，一共2N次。
- 两个子数组的元素比较的次数：如前所述，最多是（N-1）次比较，一次比较算是进行了两次数组访问，一共（2N-2）次。最少比较1次，那访问数组2次。

所以访问数组的次数最多（6N-2）次，最少（4N+2）次。
此外还能得出一个算术关系：一次归并访问数组的总次数 = 用于复制的访问 + 用于比较的访问 = 4N + 比较次数*2。

**空间成本**

空间成本比较直观，就是辅助数组长度：N。

**递归法: 自顶向下（Top-Down）**
  - 直接在原序列上直接归并排序，每次归并排序分别对左右两边进行归并排序，直至细分到两两分组。
```java
public class MergeSort{
    
    private static int[] aux; // 归并操作用的辅助数组
    
    // 供给外部客户端调用的归并排序方法
    public static void sort(int[] a){
        aux = new int[a.length];
        sort(a, 0, a.length - 1);
    }
    
    // 内部进行递归操作的归并排序方法
    private static void sort(a, int low, int high){
        // 递归出口，当该条件成立，说明传进来的数组只有一个元素
        // 而只有一个元素的数组，是不需要排序的，所以这是递归出口
        if(high <= low){
            return;
        }
        int middle = low + (high - low)/2;
        sort(a, low, middle);
        sort(a, middle + 1, high);
        // merge()的实现见本文前面归并排序的抽象方法
        merge(a, low, middle, high);
    }
}
```
**时间成本**

可以看出将一个数组拆分成两个数组是一个二分的过程，所以时间成本的分析可以用一个类似二叉树的结构图来进行（如果具备二叉树的基本知识，会更容易看懂这个分析过程）。如图：
![](https://img-blog.csdnimg.cn/20190101113230674.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA3MDcwMzk=,size_16,color_FFFFFF,t_70)

树状图结构说明：
这棵二叉树从上往下看，是归并排序时递归拆分数组的过程，从下往上看，是归并操作对数组进行合并排序的过程。除了元素个数为1的结点，所有结点均表示一次归并操作的结果。如元素个数为2的结点，表示一次归并操作将两个元素为1的结点合并成一个元素个数为2的结点。再如一个元素个数为4的结点，表示一次归并操作将两个元素个数为2的结点合并成一个元素个数为4的结点。由于元素个数为1的结点不参与计算，所以用单独的颜色表示。

计算：

1. 时间成本（不论是访问数组还是元素间的比较）主要都在merge()里面，也就是一次次的归并操作，所以时间成本的分析主要也是围绕归并操作来进行。
2. 每一个元素个数不为1的结点，都表示一次归并操作的执行结果。这里要强调元素个数不为1的结点才算，最底下元素个数为1的那一层不看，因为它不是归并操作的结果，不需要也不应该参与计算。
3. 记这棵树共有n层（再次强调不包括最底下元素个数为1的那一层，以上图为例就是4层），层的序号从0开始算，根结点记为第0层。在第k层，有(2k) 个结点，每个结点有(2(n-k))个元素。根据本文前面对归并操作比较次数的分析（通过归并操作的到一个长度为m的数组，需要进行O(m)次比较）。则通过归并操作得到一个结点需要进行（2（n-k））次比较，所以通过归并操作得到一层所有结点共需进行：（2k）*(2(n-k)) = (2n)次比较。
4. 由于这棵树共有n层，所以比较的总次数为：n * (2n)。
5. 对N个元素进行归并排序，根据二叉树的性质，树的层数n = lgN，用N替换n，得出对N个元素进行归并排序，在最糟糕的情况下，需要进行NlgN次比较。
6. 再根据前面得出的数组访问次数和比较次数的关系（通过归并操作的到一个长度为m的数组，需要进行（4m+比较次数 * 2）次访问），那么得到一个结点共需进行(4 * 2(n-k) + 2 * 2(n-k)) = 6 * 2(n-k)次数组访问，则一层需要6 * 2(n-k) * (2k) = 6 * (2n)，n层为6n * (2n)。以N代替n，数组访问6NlgN次。

以上结论是最糟糕的情况下时间成本的分析。简单说下最优情况，如果输入的数组原来就是排好序的，则对应最优情况，计算逻辑和上述基本一致，只是得到一个结点的比较次数由(2(n-k))变成(0.5 * 2(n-k))，对应的比较次数的最终结果变成0.5NlgN，数组访问次数的结果为5NlgN。

**时间成本计算结论：不论是最优还是最糟糕的情况，不论是比较次数还是数组访问次数，虽然有常数因子的差异，但是最终的趋势都是O(NlgN)。**

**空间成本**
空间成本就是辅助数组的长度，也就是客户端传入的数组长度：N。

**自顶向下的归并排序的优化**
只需要在代码里面做少许修改，就能明显优化归并排序，有以下三种优化方案：

1. 对小规模子数组使用插入排序。
2. 进行归并前判断数组是否已经有序。进行归并前判断数组是否已经有序。
3. 不将元素复制到辅助数组，辅助数组也可以直接用来排序。不将元素复制到辅助数组，辅助数组也可以直接用来排序。

现在一个个展开来说。

**对小规模子数组使用插入排序**
不单单是归并排序，所有基于递归的操作，在待处理的元素个数较少的时候，递归的方法调用都会显得过于频繁。对于大规模数组的处理，归并排序效率明显高于基础排序算法（选择、插入、冒泡），但是元素个数越少，这种优势越不明显。对三两个元素进行递归操作，更加显得得不偿失。于是就有对小规模数组（元素个数小于特定长度）不再使用递归，而是使用基础排序算法（一般是插入，因为插入的性能好于其它几个）。代码修改也很简单，基于前面递归调用的sort()方法进行修改：
```java
void sort(int[] a, int low, int high){
    // 当待排序的数组长度缩小到BORDER时，递归调用转变为插入排序，BORDER的值可以由开发者自行定义
    if(high <= low + BORDER){
        // 假设这是一个已经实现了的插入排序
        insertionSort(a, lo, hi);
        return;
    }

    int middle = low + (high - low)/2;
    sort(a, low, middle);
    sort(a, middle + 1, high);

    merge(a, low, middle, high);
}
```
所有长度小于BORDER的递归调用都免掉了。

**判断数组是否已经有序**
归并操作将两个分别有序的数组归并成一个更大的有序的数组，如果两个数组不但是分别有序，同时也互相有序了呢？如果a[mid] <= a[mid + 1]，整个归并操作都是不需要的，甚至连merge()方法都不需要被调用。这种改动主要是在进行归并操作之前添加判断，可以基于前面递归调用的sort()方法进行修改：
```java
void sort(int[] a, int low, int high){
    // 递归出口，当该条件成立，说明传进来的数组只有一个元素
    // 而只有一个元素的数组，是不需要排序的，所以这是递归出口
    if(high <= low){
        return;
    }

    int middle = low + (high - low)/2;
    sort(a, low, middle);
    sort(a, middle + 1, high);
    
    // 如果两个子数组不但分别有序，同时也互相有序
    if(a[mid] <= a[mid + 1]){
        return;
    }
    
    // merge()的实现见本文前面归并排序的抽象方法
    merge(a, low, middle, high);
}
```
如果两个数组不但是分别有序，同时也互相有序，对归并操作的调用就可以免掉了。

**不将元素复制到辅助数组**
归并操作（merge()方法）有相当一部分的时间成本在来回复制数组的操作上，这个对比较大小、排序操作本身并没有实际意义的操作，也是可以省略掉的，不过这就需要点技巧了。需要对进行归并操作的merge()方法和进行递归分组的sort()同时进行修改，先来看代码。
```java
// 内部的，归并方法
private void merge(int[] src, int[] dst, int low, int middle, int high) {

    int i = low, j = middle+1;
    for (int k = low; k <= high; k++) {
        if(i > middle){
            dst[k] = src[j++];
        } else if (j > high){
            dst[k] = src[i++];
        } else if (src[j] < src[i]){
            dst[k] = src[j++];
        } else{
            dst[k] = src[i++];
        }
    }
}

// 内部的，递归调用的排序方法
private void sort(int[] src, int[] dst, int low, int high) {
    
    if (high <= low){
        return;
    }
    
    int middle = low + (high - lo) / 2;
    // 重点在这里
    // 每次向下一层进行递归的时候，将源数组（src）和目标数组（dst）的角色互换
    sort(dst, src, low, middle);
    sort(dst, src, middle+1, high);

    merge(src, dst, low, middle, high);
}

// 供给外部调用的，启动整个排序过程的方法
public void sort(int[] a) {

    int[] aux = a.clone();
    sort(aux, a, 0, a.length-1); 
}
```
上述代码主要改动点：

1. 首先在merge()里面进行修改，不再使用辅助数组，而是直接将元素从源数组（src）归并到目标数组（dst），源数组和目标数组由递归的sort()指定。
2. 其次是对sort()进行修改，在每一层向下一层进行递归的时候，将源数组和目标数组的角色互换。需要注意的是，本层对merge()的调用，还是要保持源数组和目标数组的关系和传进来的时候一致。其次是对sort()进行修改，在每一层向下一层进行递归的时候，将源数组和目标数组的角色互换。需要注意的是，本层对merge()的调用，还是要保持源数组和目标数组的关系和传进来的时候一致。
3. 最后就是在供给外部调用的sort(int[] a)方法里，在启动内部的sort(int[] src, int[] dst, int low, int high)时，要注意将客户端传进来的待排序的数组当作目标数组传进去，将辅助数组当作源数组传进去。最后就是在供给外部调用的sort(int[] a)方法里，在启动内部的sort(int[] src, int[] dst, int low, int high)时，要注意将客户端传进来的待排序的数组当作目标数组传进去，将辅助数组当作源数组传进去。

**迭代法：自底向上（Bottom-Up）**

归并排序有两种实现方法，一种是比较符合直观逻辑的递归方法，称为自顶向下的归并排序，也就是前文所述的全部内容。另一种是迭代实现的方法，称为自底向上的归并排序。现在开始来讲这个。

查看本文前面自顶向下的归并排序的示意图，递归的尽头都是将数组拆到只剩1个元素，所以merge()方法也是从两个长度为1的数组开始归并，等凑出两个长度为2的数组，再将两个长度为2的数组归并，以此类推。如果客户端传入的初始数组长度不是2的次幂，可能中间子数组的长度略有不同，比如待归并的两个子数组的长度是3和2，但是整个程序的执行逻辑和过程都是一样的，都是从最小的1个元素开始归并，接着将得到的结果继续归并。循着这样一个思路，我们也可以不通过递归调用来对元素进行分组，而是通过循环来对元素进行分组，只要merge()方法所接受的数组长度从1开始，按照1、2、4这样一点点递增就可以了。执行逻辑如下图：
![](https://img-blog.csdnimg.cn/20190101115539416.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA3MDcwMzk=,size_16,color_FFFFFF,t_70)

整个过程看起来和自顶向下的归并排序明显不同，但是有一点本质没变：归并的操作总是从只有1个元素的数组开始，逐步增大，最后完成整个数组的排序。

假设序列共有 n 个元素：
  1. 先相邻两两分组进行归并排序
  2. 再相邻四四分组进行归并排序
  3. 再相邻八八分组进行归并排序
  4. 重复扩大分组规模，直到所有元素排序完毕
  5. …
```java
public static void mergeSort(Comparable[] a) {
    int N = a.length;
    //创建同待排序的数组同样大小的辅助数组
    aux = new Comparable[N];
    //size表示每次归并时被归并的单个子数组的大小，注意不是归并后的数组大小
    for (int sz = 1; sz < N; sz = sz + sz) 
        for (int lo = 0; lo < N - sz; lo += sz + sz)
            merge(a, lo, lo + sz - 1, Math.min(lo+sz+sz-1, N-1));
}
```
代码的执行流程如前面示意图所示，比较有必要理清楚的地方是两个循环里面各个边界、叠加值、以及传给merge()的参数值都是怎么算出来的。

如果你真的搞懂了自底向上的归并排序的思路，应该能理解两个循环的职责：
内层循环在控制每次传给merge()的两个子数组是哪两个，主要是通过控制low来设定起点。外层循环在控制每次待排序的子数组的尺寸，从1开始，每次翻倍。

职责都理清楚了，现在首先来看看外层循环是怎么控制的：

- size的含义：这里需要重点说明，将两个子数组归并成一个更大的数组，size是归并前单个子数组的大小，不是归并后的大数组的大小。
- size初始值：待排序的子数组的尺寸从1开始，没有什么好解释的。
- size叠加值：根据自底向上的归并排序思路，待排序数组的尺寸成倍增长。
- size边界：当size刚好等于（或大于）N，说明刚刚完成了最后一次归并，即最大的两个子数组的归并操作，反过来只有size<N，才有继续循环的必要。

接着来看内层循环是怎么控制的：

- low初始值：每一次归并结束，待排序数组尺寸翻倍后，都是要从头开始遍历排序的，所以low从0开始。
- low叠加值：size是待排序的子数组的尺寸，所以每次归并完两个子数组后，low是叠加2倍的size而不是1倍的size。
- low边界：主要是根据low+size<N而得出的low<N-size，那么为什么low+size应该小于N，首先要理解low+size表示什么。merge()接收几个和角标有关的参数：low、middle、high，其中low ~ middle表示左边的子数组，middle+1 ~ high表示右边的子数组。与此同时middle = low+size-1，high = low+size+size-1。所以low+size = middle+1，而middle+1是右边的子数组的第一个元素的角标。所以low+size<N意味着右边的子数组还有元素，所以才有继续进行归并操作的必要。如果你还看不明白，来看下图所示几种情况：
![](https://img-blog.csdnimg.cn/2019010111593578.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA3MDcwMzk=,size_16,color_FFFFFF,t_70)

如上图所示，这里有四个数组在进行自底向上的归并排序，它们的元素几乎一样，也处在相同的排序阶段（size = 2，即每次循环将两个长度为2的子数组归并成一个长度为4的数组）。low会先后等于0、4、8。当low等于0、4时，这四个数组的行为是一样的。当low等于8时，本该进行第三次归并操作，但是对于上面的两个数组，他们不需要（也没法）进行第三次归并操作，因为剩余的元素凑不出第二个子数组（第二个子数组一个元素都没有），而一个子数组本来就是有序的，此时low+size>=N。再看下面的两个子数组，它们剩余的元素超出一个子数组的长度，虽然不一定能凑满第二个子数组，但是第二个子数组还是有元素的，所以它们需要被归并。所以只有low+size<N时，才要（也必须要）继续进行归并，也就是low<N-size。

继续看传入merge()方法的参数是怎么计算的：
传入merge()的几个参数的含义和之前的是一样的，包括数组、low、middle、high，那么前面两个参数就没什么好解释的了，主要看看后面两个。由于size是待排序的子数组的尺寸，所以middle=low+size-1。同样的由于size是待排序的子数组的尺寸，所以high=low+size+size-1。那么这个N-1和数学函数Math.min()是怎么回事？来看以下这种情况：
![](https://img-blog.csdnimg.cn/20190101120206588.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA3MDcwMzk=,size_16,color_FFFFFF,t_70)

当size=2，low=8时，即将进行第三次归并操作。此时剩余的元素个数超出一个子数组的长度（即第二个子数组有元素），但是第二个子数组凑不满，如果直接用high=low+size+size-1的话，数组就越界了，所以才有Math.min(low+size+size-1, N-1)这个取较小值的操作。如果客户端最初传入的数组长度刚好是2的指数，则high=low+size+size-1总是对的。如果客户端最初传入的数组长度不是2的指数，每个size值对应的循环，进行到最后两个子数组的归并操作时，有可能出现第二个子数组凑不满的情况，此时high=low+size+size-1会导致越界，此时high=N-1才是第二个子数组的边界。

### Quick Sort
简介
- 快速排序广泛运用于系统排序和其他应用中。它也是一个递归过程，与归并排序不同的是，它先进行操作然后再递归，而不是归并排序先进性递归然后再进行 merge。

思路
- 方法其实很简单：分别从初始序列“6 1 2 7 9 3 4 5 10 8”两端开始“探测”。先从右往左找一个小于6的数，再从左往右找一个大于6的数，然后交换他们。这里可以用两个变量i和j，分别指向序列最左边和最右边。我们为这两个变量起个好听的名字“哨兵i”和“哨兵j”。刚开始的时候让哨兵i指向序列的最左边（即i=1），指向数字6。让哨兵j指向序列的最右边（即=10），指向数字。
- ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200218121956.png)
- 首先哨兵j开始出动。因为此处设置的基准数是最左边的数，所以需要让哨兵j先出动，这一点非常重要（请自己想一想为什么）。哨兵j一步一步地向左挪动（即j–），直到找到一个小于6的数停下来。接下来哨兵i再一步一步向右挪动（即i++），直到找到一个数大于6的数停下来。最后哨兵j停在了数字5面前，哨兵i停在了数字7面前。
- ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200218122030.png)
- ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200218122042.png)
- 现在交换哨兵i和哨兵j所指向的元素的值。交换之后的序列如下：
  `6 1 2 5 9 3 4 7 10 8`
- ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200218122148.png)
- ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200218122203.png)
- 到此，第一次交换结束。接下来开始哨兵j继续向左挪动（再友情提醒，每次必须是哨兵j先出发）。他发现了4（比基准数6要小，满足要求）之后停了下来。哨兵i也继续向右挪动的，他发现了9（比基准数6要大，满足要求）之后停了下来。此时再次进行交换，交换之后的序列如下：
  `6 1 2 5 4 3 9 7 10 8`
- 第二次交换结束，“探测”继续。哨兵j继续向左挪动，他发现了3（比基准数6要小，满足要求）之后又停了下来。哨兵i继续向右移动，糟啦！此时哨兵i和哨兵j相遇了，哨兵i和哨兵j都走到3面前。说明此时“探测”结束。我们将基准数6和3进行交换。交换之后的序列如下：
  `3 1 2 5 4 6 9 7 10 8`
- ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200218122258.png)
- ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200218122321.png)
- ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200218122330.png)
- 到此第一轮“探测”真正结束。此时以基准数6为分界点，6左边的数都小于等于6，6右边的数都大于等于6。回顾一下刚才的过程，其实哨兵j的使命就是要找小于基准数的数，而哨兵i的使命就是要找大于基准数的数，直到i和j碰头为止。
- OK，解释完毕。现在基准数6已经归位，它正好处在序列的第6位。此时我们已经将原来的序列，以6为分界点拆分成了两个序列，左边的序列是“3 1 2 5 4”，右边的序列是“9 7 10 8”。接下来还需要分别处理这两个序列。因为6左边和右边的序列目前都还是很混乱的。不过不要紧，我们已经掌握了方法，接下来只要模拟刚才的方法分别处理6左边和右边的序列即可。现在先来处理6左边的序列现吧。
- 左边的序列是“3 1 2 5 4”。请将这个序列以3为基准数进行调整，使得3左边的数都小于等于3，3右边的数都大于等于3。好了开始动笔吧
- 如果你模拟的没有错，调整完毕之后的序列的顺序应该是：
  `2 1 3 5 4`  
- OK，现在3已经归位。接下来需要处理3左边的序列“2 1”和右边的序列“5 4”。对序列“2 1”以2为基准数进行调整，处理完毕之后的序列为“1 2”，到此2已经归位。序列“1”只有一个数，也不需要进行任何处理。至此我们对序列“2 1”已全部处理完毕，得到序列是“1 2”。序列“5 4”的处理也仿照此方法，最后得到的序列如下：
  `1 2 3 4 5 6 9 7 10 8`
- 对于序列“9 7 10 8”也模拟刚才的过程，直到不可拆分出新的子序列为止。最终将会得到这样的序列，如下
  `1 2 3 4 5 6 7 8 9 10`
- 到此，排序完全结束。细心的同学可能已经发现，快速排序的每一轮处理其实就是将这一轮的基准数归位，直到所有的数都归位为止，排序就结束了。
- ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200218123926.png)
- 快速排序之所比较快，因为相比冒泡排序，每次交换是跳跃式的。每次排序的时候设置一个基准点，将小于等于基准点的数全部放到基准点的左边，将大于等于基准点的数全部放到基准点的右边。这样在每次交换的时候就不会像冒泡排序一样每次只能在相邻的数之间进行交换，交换的距离就大的多了。因此总的比较和交换次数就少了，速度自然就提高了。当然在最坏的情况下，仍可能是相邻的两个数进行了交换。因此快速排序的最差时间复杂度和冒泡排序是一样的都是O(N2)，它的平均时间复杂度为O(NlogN)。其实快速排序是基于一种叫做“二分”的思想。
  ```java
    public class QuickSort {
        public static void QuickSortTest(int[] arr, int left, int right){
            if(left > right){
                return;
            }
            int base = arr[left];
            int i = left;
            int j = right;
            while(i != j){
                while(arr[j] >= base && i < j){
                    j--;
                }
                while(arr[i] <= base && i < j){
                    i++;
                }
                if(i < j){
                    int temp = arr[i];
                    arr[i] = arr[j];
                    arr[j] = temp;
                }
            }
            arr[left] = arr[i];
            arr[i] = base;

            QuickSortTest(arr, left, i - 1);
            QuickSortTest(arr, i + 1, right);
        }

        public static void main(String[] args) {
            int[] m = {2,4,1,80,5,23,57,13,0,35};
            QuickSort.QuickSortTest(m, 0, m.length -1);
            System.out.println(Arrays.toString(m));
        }
    }
  ```
### Heap Sort
如果对一个大顶堆连续执行删除根结点的操作，就会输出一个降序序列，相当于排序的效果，堆排序就是基于二叉堆实现的排序。思路也很简单，首先用N个元素构造一个二叉堆，接着连续删除根结点，**为了实现原地排序的功能，删除的时候不要直接输出元素，而是将根结点和数组尾部元素互换位置，这样就不需要使用而外的数组**。

**构造堆**

最直观的方法自然是从第一个待处理的元素开始遍历数组，类似调用N次insert()向优先队列里插入N个元素那样。这个方式可以称为上浮方式构造堆，它的逻辑和前文的基本一致，就不多说了。

一个更高效的方法是下沉方式构造堆，我们知道下沉操作可以在二叉堆的某个结点变得比它的子结点小时恢复堆的有序性，而如果我们将这个变小的结点看作它的左右两个子堆的根结点，这个操作就像合并两个子堆，如下图示：
![](https://img-blog.csdnimg.cn/20200111154655432.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA3MDcwMzk=,size_16,color_FFFFFF,t_70)

特别的，如果对三个随机排列的元素的根结点执行下沉操作，就像将两个大小为一的子堆合并成一个二叉堆，如下图示：
![](https://img-blog.csdnimg.cn/20200111154745531.png)
如果从第一个非叶子结点开始，自右向左，自下而上地对每个结点执行下沉操作，就像不断地在合并子堆，子堆的大小从1开始递增，整个过程有点类似归并排序的自底向上的归并过程，如下图示：

![](https://img-blog.csdnimg.cn/20200111154814432.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA3MDcwMzk=,size_16,color_FFFFFF,t_70)

以上就是下沉方式构造堆的过程，由于叶子结点是大小为1的堆，所以从第一个不是叶子的结点开始下沉，也就是从大小为3的堆开始下沉，由于过程是自下而上的，所以每一个新处理的结点，它的两个子堆一定都是有序的，对新结点进行下沉即可合并两个子堆，直到遇到根结点，建堆完毕。当元素数量相同时，树的高度也相同，上浮建堆和下沉建堆只是方向不同，它们走的高度都是一样的，为什么下沉就更高效呢？因为结点在树的各层不是均匀分布的，这导致了以根结点作为终点（上浮）来建堆和以树的底部作为终点（下沉）来建堆的效率差异，如下图示：
![](https://img-blog.csdnimg.cn/20200111154850668.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA3MDcwMzk=,size_16,color_FFFFFF,t_70)

越是接近树的底部，结点数量越多，反之，越是接近根结点，结点的数量越少。如果以根结点作为终点，意味着结点数量最多的那一层，走的路径也是最长的，结点数量第二多的那一层，走的路径是第二长的。如果以树的底部作为终点，则结点数量最多的那一层，走的路径长为0，结点数量第二多的那一层，走的路径长为1。上浮建堆和下沉建堆是相反的，这是效率差异的原因。以15个结点作为例子进行计算，上浮建堆的路径总长度为`10 + 21 + 32 +43 = 34`，下沉建堆的路径总长度为`80 + 41 + 22 + 13 = 11`。这是从计算的角度说明。

**下沉建堆时间复杂度分析——证明下沉建堆能在线性时间内完成：**
我们以下图所示由满二叉树构成的二叉堆为例进行计算和说明：
![](https://img-blog.csdnimg.cn/20200111155023631.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA3MDcwMzk=,size_16,color_FFFFFF,t_70)

计算由2k+1-1个元素构成的数组执行下沉建堆的时间复杂度，可以涵盖由2k至2k+1-1个元素构成的数组执行下沉建堆的时间上界。例如当k取4，计算31个元素构成的数组执行下沉建堆的时间复杂度，可以涵盖由16至31个元素构成的数组执行下沉建堆的时间上界。对于时间复杂度的计算，得到上界即可，满二叉树比较有规律，方便计算。

以上图为例，对31个元素构成的升序排列的数组进行下沉建堆，构建大顶堆，
在高度为0那层，有16个元素，每个元素需要下沉0层；
在高度为1那层，有8个元素，每个元素需要下沉1层；
在高度为2那层，有4个元素，每个元素需要下沉2层；
在高度为3那层，有2个元素，每个元素需要下沉3层；
在高度为4那层，有1个元素，每个元素需要下沉4层；
这个算式其实是有规律的，在高度为h那层，有2H-h个元素，每个元素需要下沉h层，则建堆需要下沉的总次数为：
1 * 2H-1 + 2 * 2H-2 + 3 * 2H-3 + … + H*20，这是一个等差等比数列相乘求和，用错位相减法即可化简，最终结果为：
2 * 2H+1 - H - 2，再根据H和N的关系，得到最终算式：
N - log2(N+1)，由于下沉一层需要比较两次，所以比较的次数为2N - 2log2(N+1)，这个算式的值显然小于2N，所以时间复杂度为O(N)，即下沉建堆可以在线性时间复杂度完成。
重新提一下计算的前提是数量N构成满二叉树，这个算式仅在N = 2k+1-1的时候得到的值是刚好相等的，其它情况相当于是计算出了耗时上界。

**下沉排序**

下沉排序的思路类似实现优先队列的deleteMax()，但不像deleteMax()那样把根结点删了就行了，为了实现原地排序（也就是空间复杂度为常数），在删除第一个结点的时候，将根结点和倒数第一个元素互换位置，接着对新的根结点进行下沉操作。然后继续将根结点和数组倒数第二个元素互换位置，再对新的根结点进行下沉操作，以此类推。这样就能实现原地排序，最终得到一个升序数组。时间复杂度的计算和前面“输出集合里的元素”一致：
2 * (⌊log2(N-1)⌋ + ⌊log2(N-2)⌋ + … + ⌊log21⌋) = (2Nlog2N) - 4(N-1) = O(Nlog2N)。

将建堆和下沉排序的时间复杂度加起来，这个算式不好化简，但是我们可以只将它们的上界加起来，即2N和2Nlog2N，则通过下沉建堆的方式实现堆排序需要不超过(2N + 2Nlog2N)次的比较(N+Nlog2N)次的互换（比较次数的一半）。
>排序中的`sink()`请参考前文的二叉堆

**代码中的建堆**：个人理解，堆只是我们想像出来的一个数据结构，并不是具象化真实存在的，其实形式还是一个数组。我们可以把任何的数组都看成是一个无序的堆结构。我们想构建大根堆的话，需要执行`sink()`下沉操作来构建堆。
```java
public static void heapSort(int[] a){
    int N = a.length;
    
    // 下沉建堆
    for(int k = N/2; k>=1; k--){
        sink(a, k, N);
    }
    
    // 下沉排序
    while(N > 1){
        exchange(a, 1, N--);
        sink(a, 1, N);
    }
}
```

## Binary Search
- 什么是二分查找？
    - 二分查找也称折半查找（Binary Search），它是一种效率较高的查找方法。但是，折半查找要求线性表必须采用顺序存储结构，而且表中元素按关键字有序排列。
```java
//Loop
public static int myBinarySearch(int[], int value){
    int low = 0;
    int high = arr.length - 1;

    while(low <= high){
        int mid = (low + high) / 2;
        if(value == arr[mid]){
            return mid;
        }
        if(value > arr[mid]){
            low = mid + 1;
        }
        if( value < arr[mid]){
            high = mid - 1;
        }
    }
    return -1;
}
//Recursion
public class Solution {

  public static void main (String args[]){
      int[] ar = {1,2,3,4,5,6,7,8,9};
      int res = Solution.binarySearch(ar,0,ar.length-1,3);
      System.out.println(res);
  }

  static int binarySearch(int a[], int lo, int hi, int key)
    {
        if(lo > hi){
            return -1;
        }

        int mid = lo + (hi - lo) / 2;
        if(key == a[mid]){
            return mid;
        }
        else if(key > a[mid]){
            return binarySearch(a, mid + 1, hi, key);
        }
        else{
            return binarySearch(a, lo, mid - 1, key);
        }

    }


}
```
## Random Walk
### Introduce
### Implement
### Reference
- [Math of RandomWalk](http://mathworld.wolfram.com/RandomWalk2-Dimensional.html)
- [RANDOM.ORG - True Random Number Service](https://www.random.org)
- [RandomWalk from MIT](https://www.mit.edu/~kardar/teaching/projects/chemotaxis(AndreaSchmidt)/home.htm)