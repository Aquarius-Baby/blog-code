---
title: 8大基本排序算法
date: 2020-02-10 23:42:04
tags:
    - 排序
categories:
    - 算法
---
排序算法	平均时间复杂度
- 冒泡排序	O(n2)
- 选择排序	O(n2)
- 插入排序	O(n2)
- 希尔排序	O(n1.5)
- 快速排序	O(N*logN)
- 归并排序	O(N*logN)
- 堆排序	O(N*logN)
- 基数排序	O(d(n+r))
<!--more-->

# 冒泡排序
## 基本思想
两个数比较大小，较大的数冒起来，较小的数沉起来。

## 过程
（1）比较相邻的两个数据，如果第二个数小，就交换位置。
（2）从前向后两两比较，一直到比较最后两个数据。最终最大的数被交换到最后一个位置，这样第一个最大数的位置就排好了。
（3）继续重复上述过程，依次将第2.3...n-1个最大数排好位置。
## 代码实现
```java
public class BubbleSort {
    public static void main(String[] args) {
        int[] nums = {9, 1, 5, 3, 2, 7};
        new BubbleSort().bubbleSort2(nums);
        System.out.println(Arrays.toString(nums));
    }

    /**
     * 解法1 ：先确定大的数
     */
    private void bubbleSort(int[] nums) {
        for (int i = nums.length - 1; i > 0; i--) {
            for (int j = 0; j < i; j++) {
                if (nums[j] > nums[j + 1]) {
                    int t = nums[j];
                    nums[j] = nums[j + 1];
                    nums[j + 1] = t;
                }
            }
        }
    }

    /**
     * 解法2：先确定小得数
     */
    private void bubbleSort1(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = nums.length - 1; j > i; j--) {
                if (nums[j - 1] > nums[j]) {
                    int t = nums[j];
                    nums[j] = nums[j - 1];
                    nums[j - 1] = t;
                }
            }
        }
    }

    
}
```
## 优化思路
```
/**
     * 解法3：1基础的优化，如果一次排序中不存在交换顺序，说明已经排好序可，可以退出了
     *
     * @param nums
     */
    private void bubbleSort2(int[] nums) {
        boolean flag = true;
        for (int i = 0; i < nums.length; i++) {
            flag = true;
            for (int j = 0; j < nums.length - 1 - i; j++) {
                if (nums[j] > nums[j + 1]) {
                    flag = false;
                    int t = nums[j];
                    nums[j] = nums[j + 1];
                    nums[j + 1] = t;
                }
            }
            if (flag) {
                break;
            }
        }
    }
```
# 选择排序
## 思路
在长度为N的无序数组中，第一次遍历n-1个数，找到最小的数值与第一个元素交换；
                   第二次遍历n-2个数，找到最小的数值与第二个元素交换；
。。。
第n-1次遍历，找到最小的数值与第n-1个元素交换，排序完成。

## 代码实现
```java
public class SelectionSort {
    private void selectionSort(int[] nums) {
        for (int i = 0; i < nums.length - 1; i++) {
            int index = i;
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[j] < nums[index]) {
                    index = j;
                }
            }
            if (index != i) {
                int t = nums[i];
                nums[i] = nums[index];
                nums[index] = t;
            }
        }
    }
}
```
# 插入排序
## 思路
在要排序的一组数中，假定前n-1个数已经排好序。
现在将第n个数插到前面的有序数列中，使得这n个数也是排好顺序的。
如此反复循环，直到全部排好顺序。
## 代码实现
```java
public class InsertionSort {
    public static void main(String[] args) {
        int[] nums = {9, 1, 5, 3, 2, 7};
        new InsertionSort().insertionSort2(nums);
        System.out.println(Arrays.toString(nums));
    }

    private void insertionSort(int[] nums) {
        for (int i = 1; i < nums.length; i++) {
            int temp = nums[i];
            int j = i - 1;
            // 找到第一个比插入数要小的index，将数插入index+1的位置
            while (j >= 0 && nums[j] > temp) {
                nums[j + 1] = nums[j];
                j--;
            }
            j++;
            nums[j] = temp;
        }
    }
}

```
# 希尔排序
## 思路
在要排序的一组数中，根据某一增量分为若干子序列，并对子序列分别进行插入排序。
然后逐渐将增量减小,并重复上述过程。直至增量为1,此时数据序列基本有序,最后进行插入排序。
## 代码实现
```java
public class ShellSort {
    public static void main(String[] args) {
        int[] nums = {9, 1, 5, 3, 2, 7, 8};
        new ShellSort().shellSort(nums);
        System.out.println(Arrays.toString(nums));
    }

    public static void shellSort(int[] array) {
        int temp = 0;
        int length = array.length;
        int incre = length;
        while (true) {
            // 增量减半
            incre = incre / 2;
            for (int k = 0; k < incre; k++) {    //根据增量分为若干子序列
                ///循环插入排序
                for (int i = k + incre; i < length; i += incre) {
                    for (int j = i; j > k; j -= incre) {
                        if (array[j] < array[j - incre]) {
                            temp = array[j - incre];
                            array[j - incre] = array[j];
                            array[j] = temp;
                        } else {
                            break;
                        }
                    }
                }
            }
            if (incre == 1) {
                break;
            }
        }
    }
}
```
# 快速排序
## 思路
1.先从数列中取出一个数作为key值；
2.将比这个数小的数全部放在它的左边，大于或等于它的数全部放在它的右边；
3.对左右两个小数列重复第二步，直至各区间只有1个数。
## 代码实现
```java
public class QuickSort {
    public static void main(String[] args) {
        int[] nums = {4, 2, 5, 9, 3, 7};
        new QuickSort().quickSort(nums);
        System.out.println(Arrays.toString(nums));
    }

    private void quickSort(int[] nums) {
        int length = nums.length;
        int left = 0;
        int right = length - 1;
        quickSortHelp(nums, left, right);
    }

    private void quickSortHelp(int[] nums, int left, int right) {
        if (left >= right) {
            return;
        }
        //设置最左边的元素为基准值
        int key = nums[left];
        //数组中比key小的放在左边，比key大的放在右边，key值下标为i
        int i = left;
        int j = right;
        while (i < j) {
            //j向左移，找到第一个比key小的值，位置为j
            while (nums[j] >= key && i < j) {
                j--;
            }
            // 将右侧比key小的数放到左边
            if (i < j) {
                nums[i] = nums[j];
            }
            //i向右移，直到遇到比key大的值
            while (nums[i] <= key && i < j) {
                i++;
            }
            if (i < j) {
                nums[j] = nums[i];
            }
        }
        nums[i] = key;
        quickSortHelp(nums, left, i - 1);
        quickSortHelp(nums, i + 1, right);
    }
}
```

# 归并排序
## 思路
归并排序是建立在归并操作上的一种有效的排序算法。
该算法是采用分治法的一个非常典型的应用。
首先考虑下如何将2个有序数列合并。
    只要从比较2个数列的第一个数，谁小就先取谁，取了后就在对应数列中删除这个数。
    然后再进行比较，如果有数列为空，那直接将另一个数列的数据依次取出即可。
## 代码实现
```java
public class MergeSort {
    public static void mergeSort(int a[], int first, int last, int temp[]) {
        if (first < last) {
            int middle = (first + last) / 2;
            mergeSort(a, first, middle, temp);//左半部分排好序
            mergeSort(a, middle + 1, last, temp);//右半部分排好序
            mergeArray(a, first, middle, last, temp); //合并左右部分
        }
    }

    public static void mergeArray(int a[], int first, int middle, int end, int temp[]) {
        int i = first;
        int m = middle;
        int j = middle + 1;
        int n = end;
        int k = 0;
        while (i <= m && j <= n) {
            if (a[i] <= a[j]) {
                temp[k] = a[i];
                k++;
                i++;
            } else {
                temp[k] = a[j];
                k++;
                j++;
            }
        }
        while (i <= m) {
            temp[k] = a[i];
            k++;
            i++;
        }
        while (j <= n) {
            temp[k] = a[j];
            k++;
            j++;
        }
        for (int index = 0; index < k; index++) {
            a[first + index] = temp[index];
        }
    }
}
```
# 堆排序
## 思路
## 代码实现
```java
public class HeapSort {
    public static void main(String[] args) {
        int[] nums = {9, 1, 5, 3, 2, 7, 8};
        new HeapSort().heapSort(nums);
        System.out.println(Arrays.toString(nums));
    }

    public void heapSort(int[] arr) {
        //构造大根堆
        heapInsert(arr);
        int size = arr.length;
        while (size > 1) {
            //固定最大值
            swap(arr, 0, size - 1);
            size--;
            //构造大根堆
            heapify(arr, 0, size);
        }
    }

    //构造大根堆（通过新插入的数上升）
    public void heapInsert(int[] arr) {
        for (int i = 0; i < arr.length; i++) {
            //当前插入的索引
            int currentIndex = i;
            //父结点索引
            int fatherIndex = (currentIndex - 1) / 2;
            //如果当前插入的值大于其父结点的值,则交换值，并且将索引指向父结点
            //然后继续和上面的父结点值比较，直到不大于父结点，则退出循环
            while (arr[currentIndex] > arr[fatherIndex]) {
                //交换当前结点与父结点的值
                swap(arr, currentIndex, fatherIndex);
                //将当前索引指向父索引
                currentIndex = fatherIndex;
                //重新计算当前索引的父索引
                fatherIndex = (currentIndex - 1) / 2;
            }
        }
    }

    //将剩余的数构造成大根堆（通过顶端的数下降）
    //拿顶端的数与其左右孩子较大的数进行比较，
    // 如果顶端的数大于其左右孩子较大的数，则停止
    // 如果顶端的数小于其左右孩子较大的数，则交换，然后继续与下面的孩子进行比较
    public void heapify(int[] arr, int index, int size) {
        int left = 2 * index + 1;
        int right = 2 * index + 2;
        while (left < size) {
            int largestIndex;
            //判断孩子中较大的值的索引（要确保右孩子在size范围之内）
            if (arr[left] < arr[right] && right < size) {
                largestIndex = right;
            } else {
                largestIndex = left;
            }
            //比较父结点的值与孩子中较大的值，并确定最大值的索引
            if (arr[index] > arr[largestIndex]) {
                largestIndex = index;
            }
            //如果父结点索引是最大值的索引，那已经是大根堆了，则退出循环
            if (index == largestIndex) {
                break;
            }
            //父结点不是最大值，与孩子中较大的值交换
            swap(arr, largestIndex, index);
            //将索引指向孩子中较大的值的索引
            index = largestIndex;
            //重新计算交换之后的孩子的索引
            left = 2 * index + 1;
            right = 2 * index + 2;
        }
    }

    //交换数组中两个元素的值
    public void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}

```
# 基数排序
## 思路
BinSort想法非常简单，首先创建数组A[MaxValue]。
然后将每个数放到相应的位置上（例如17放在下标17的数组位置。
最后遍历数组，即为排序后的结果。
## 代码实现
```java
public class RadixSort {
    /**
     *
     * @param A 原数组
     * @param temp 临时数组
     * @param n 序列的数字个数
     * @param k 最大的位数2
     * @param r 基数10
     * @param cnt 存储bin[i]的个数
     */
    public static void RadixSort(int A[], int temp[], int n, int k, int r, int cnt[]) {
        for (int i = 0, rtok = 1; i < k; i++, rtok = rtok * r) {
            //初始化
            for (int j = 0; j < r; j++) {
                cnt[j] = 0;
            }
            //计算每个箱子的数字个数
            for (int j = 0; j < n; j++) {
                cnt[(A[j] / rtok) % r]++;
            }
            //cnt[j]的个数修改为前j个箱子一共有几个数字
            for (int j = 1; j < r; j++) {
                cnt[j] = cnt[j - 1] + cnt[j];
            }
            for (int j = n - 1; j >= 0; j--) {      //重点理解
                cnt[(A[j] / rtok) % r]--;
                temp[cnt[(A[j] / rtok) % r]] = A[j];
            }
            for (int j = 0; j < n; j++) {
                A[j] = temp[j];
            }
        }
    }
}
```