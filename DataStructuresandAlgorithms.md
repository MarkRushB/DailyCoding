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


## 队列 
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
## LinkedList
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
        HeroNode hero4 = new HeroNode(4, "林冲", "豹子头");
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
  ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/IMG_0272.PNG)
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
3. **从尾到头打印单链表**
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
### 双向链表
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
# Algorithms
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
## 手动实现LinkedList
```java
public class Node{
    Node Previous;
    Node next;
    Node element;

    public Node(Node Previous, Node next, Node eleent){
        super();
        this.previous = previous;
        this.next = next;
        this.element = element;
    }
    public Node(Object element){
        super();
        this.element = element;
    }
}

public class LinkedList01{
    private Node first;
    private Node last;
    private int size;

    public void add(Object obj){
        Node node = new Node(obj);

        if(first == null){
        //  node.previous = null;
        //  node.next = null;
            first = node;
            last = node;
        }else{
            node.previous = last;
            node.next =null;

            last.next = node;
            last = node;
        }
    }
}
```
## 大O表示法
- 大O表示法是一种特殊的表示法，指出了算法的速度有多快
- 常见的大O运行时间
    - O(1) 常量时间：给定一个大小为n的输入，该算法只需要一步就可以完成任务
    - O(log n) 也叫对数时间，给定大小为n的输入，该算法每执行一步，它完成任务所需要的步骤数目会以一定的因子减少。这样是算法包括二分查找法
    - O(n) 也叫线性时间，给定大小为n的输入，该算法完成任务所需要的步骤直接和n相关（1对1的关系）。这样的算法包括简单查找
    - O(n*log n) 这样是算法包括快速排序--一种比较快的排序算法
    - O(n2) 给定大小为n的输入，完成任务所需要的步骤是n的平方。这样的算法包括选择排序--一种比较慢的排序算法
    - O(n!) 这样的算法包括旅行商问题的解决方案--一种非常慢的算法
---
## 冒泡排序
- 什么是冒泡排序？
    - 冒泡排序是一种简单的排序算法。它重复地走访过要排序的数列，一次比较两个元素，如果他们的顺序错误就把他们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端，如下图所示。
    ![avatar](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20veW9qaWFrdS92aXN1YWxBbGdvcml0aG0vcmF3L21hc3Rlci9hbGdvcml0aG1Eb2N1bWVudC9saUppYVRvbmcvcGljL0J1YmJsZV9zb3J0X2FuaW1hdGlvbi5naWY)  
    冒泡排序对 n 个项目需要 O( n^2) 的比较次数，且可以原地排序。尽管这个算法是最简单了解和实现的排序算法之一，但它对于包含大量的元素的数列排序是很没有效率的。
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
---
## 二分查找
- 什么是二分查找？
    - 二分查找也称折半查找（Binary Search），它是一种效率较高的查找方法。但是，折半查找要求线性表必须采用顺序存储结构，而且表中元素按关键字有序排列。
```java
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
```