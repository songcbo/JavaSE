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