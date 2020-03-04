
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
  - [Heap](#heap)
  - [Priority Queue](#priority-queue)
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
    - [Ten Sorts we usually use](#ten-sorts-we-usually-use)
    - [Bubble Sort](#bubble-sort)
    - [Selection Sort](#selection-sort)
    - [Insertion Sort](#insertion-sort)
    - [Shell Sort](#shell-sort)
    - [Merge Sort](#merge-sort)
    - [Quick Sort](#quick-sort)
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
## Heap
未完待续

## Priority Queue
- **优先队列**：是用堆实现的数据结构（也可以用数组实现），优先队列可以随时取队中最大值，插入等，手动实现也可以一定程度上实现删除


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
- 递归法: 自顶向下（Top-Down）
  - 直接在原序列上直接归并排序，每次归并排序分别对左右两边进行归并排序，直至细分到两两分组。
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
- 迭代法：自底向上（Bottom-Up）
  - 假设序列共有 n 个元素：
    1. 先相邻两两分组进行归并排序
    2. 再相邻四四分组进行归并排序
    3. 再相邻八八分组进行归并排序
    4. 重复扩大分组规模，直到所有元素排序完毕
    5. …
```java
public static void mergeSort(Comparable[] a) {
    int N = a.length;
    aux = new Comparable[N];
    for (int sz = 1; sz < N; sz = sz + sz) 
        for (int lo = 0; lo < N - sz; lo += sz + sz)
            merge(a, lo, lo + sz - 1, Math.min(lo+sz+sz-1, N-1));
}
```
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