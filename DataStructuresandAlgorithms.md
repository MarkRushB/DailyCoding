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