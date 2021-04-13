# 插入排序

```java
/*
插入排序：
1.直接插入排序(稳定)
   1.1 思路 ：假定第一个元素作为一个有序序列，从第二个元素开始与前面的元素进行比较，如果前面的元素大于当前元素，就把前面的元素往后移动，并将当前元素插入。遍历到的元素前面的一定为有序序列。
   
   1.2 时间复杂度
       最优：数据本来就有序，因此只需要执行外循环 n-1 次,每次只进行比较，不进行移动。即复杂度为 O(n)
       最差：数据反序，需要外循环执行 n-1 次,同时每次内循环都要移动前面的所有元素，即比较和移动 1+2+3+...+(n-1)次,复杂度为 O(n^2)
       平均：O(n^2)
        
   1.3 空间复杂度
        仅使用 i, j,temp 三个辅助变量，空间复杂度为 O(1)
        
2.希尔排序(不稳定)
   2.1 思路 ： 希尔排序是基于插入排序的排序算法,对插入排序进行了一些改动。因为直接插入排序只能交换相邻的元素，如果最小值在最后，那么把他移动到正确的位置就要经过 n-1次移动。希尔排序有一个增量序列，根据增量序列来进行分组,使得可以交换不相邻的元素，在组内进行直接插入排序
   2.2 时间复杂度
        shell排序的时间复杂度是根据选中的 增量d 有关的
        最优：O(n)   (元素已经排序好顺序)
        最差：时间复杂度为：O(n ^ 2)；
        平均：O(n^1.3)
   2.3 空间复杂度
       O(1)
 */
```

## 代码

```java
public class InsertSort {
    @Test
    public void Insertsort(){
        int[] arr = new int[]{3, 4, 51, 7, 4, 8, 9, 2, 0, 4, 7, 8, 2, 3};
        int j;
        for (int i = 1; i < arr.length; i++) {
            int temp = arr[i];
            for (j = i; j >= 1 && arr[j - 1] > temp; j--)
                arr[j] = arr[j - 1];
            arr[j] = temp;
        }
        for (int i : arr) {
            System.out.println(i);
        }
    }
    //shellSort
    @Test
    public  void shellSort() {
        int[] arr = new int[]{3, 4, 51, 7, 4, 8, 9, 2, 0, 4, 7, 8, 2, 3};
        int j;
        //增量为长度的一般，依次/2，称为希尔增量，但这个增量序列并不是最优的
        for (int gap = arr.length / 2; gap >= 1; gap /= 2) {
            //下面就是插入排序，只是进行了分组，即j - gap
            //从 gap 开始，即gap 为第一分组的第二个元素，gap + 1 为第二分组的第二个元素
            for (int i = gap; i < arr.length; i++) {
                // 遍历 i 前面距离 gap 的元素，如果小，就将j-gap 和j交换，因为 i 是从左往右遍历，所以		左边的都是有序的，
                int temp = arr[i];
                for (j = i; j >= gap && arr[j - gap] > temp; j -= gap)
                    arr[j] = arr[j - gap];
                arr[j] = temp;
            }
        }
        for (int i : arr) {
            System.out.println(i);
        }
    }
}
```



# 堆排序

## 数据结构中的堆

### 1.堆的定义

1.堆是一种完全二叉树

2.由数组实现

3.与二叉搜索树的区别：

```xml
1.二叉搜索树的左节点一定比父节点小，右节点一定比父节点大，所以它可以使用中序遍历来显示升序

2.堆中的左右节点之间没有大小关系，但他们一定小于或大于父节点。
```


​	

### 2.堆的操作

1.有两种排序，大根堆和小根堆。（大根堆中父节点一定比子节点大，小根堆中一定比子节点小）

2.排序时为保证堆的属性（最大最小）主要有两种操作，上浮和下沉。 

### 3.插入

1.将当前节点放至堆的最后，也就是数组的最后,然后进行up()(上浮操作)

2.判断当前节点是否右父节点，如数组的第一个元素就没有父节点

3.判断父节点 val 是否大于/小于（最小堆/最大堆）当前节点

4.进行交换

```java
class Heap{
    //创建大小为1000的数组来存储堆，size记录数组中元素个数
    int[] heap;
    int size = 0 ;
    int MAXSIZE = 1000;
    public int init(){
        heap = new int[MAXSIZE];
        //进行插入操作，参数val没有定义，自行创建
        insert(val);
        //返回堆顶元素
        return heap[0]
    }
    //这边以小根堆为例（最小堆）
    public void insert(int val){
        heap[size] = val;
        up();
        size++;
    }
    //上浮
    public void up(){
        //当前节点在数组的下标
        int  i = size;
        // 父节点
        int fa = (int)Math.ceil((i / 2.0 )) - 1;
        //父节点存在且大于当前节点
        while( fa >= 0 && heap[i] < heap[fa]){
            //交换
            int temp = heap[fa];
            heap[fa] = heap[i];
            heap[i] = temp;
            //当前节点变为父节点，父节点也继续变化，直到父节点不存在或者父节点小于当前节点
            i = fa;
            fa = (int)Math.ceil((i / 2.0 )) - 1;
        }    
    }
}
```



### 4.删除堆顶元素

1.交换堆顶元素和最后一个元素，数组中元素个数减一

2.进行堆排序，使用下沉down(),保证堆的属性

3.下沉首先判断是否存在子节点

4.子节点中较小的(小根堆)的节点是否还小于当前节点，小于就交换

5.不存在子节点或者子节点都大于当前节点时结束循环

```java
public int remove(){
    //将堆顶元素和最后一个元素交换，然后进行down（）下沉操作保证堆的属性
    // 也是小根堆的例子
    int val = heap[0];
    heap[0] = heap[--size];
    down();
    return val;
}
public void down(){
    int i = 0;
    //保证当前节点存在子节点
    while( i * 2 + 1 < size){
        //找出子结点中较小的那个
        int child = i * 2 + 1;
        if(child + 1 < size && heap[child] > heap[child + 1])child += 1;
        //判断较小的子节点是否小于当前节点，小于就交换
        if(heap[child] < heap[i]){
            int temp = heap[child];
            heap[child] = heap[i];
            heap[i] = temp;
            i = child; 
        }
        //较小的子节点大于当前节点，说明当前节点已经处于正确的位置，结束循环
        else
            break;
    }
}

//这里也可以使用递归，初始i为0
public void down(int i){
    int child = i * 2 + 1;
    int t = i;
    if(child < size){
        if(child + 1 < size && heap[child + 1] < heap[child])child = child + 1;
        if(heap[child] < heap[i]){
            int temp = heap[child];
            heap[child] = heap[i];
            heap[i] = temp;
            i = child;
        }
        if(t != i)down(i);
    }       
}
```