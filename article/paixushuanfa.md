---
title: 十大排序算法
date: 2020-12-22 20:33:22
categories:
  - study
---

# 十大排序算法

[![2QDphq.md.png](https://z3.ax1x.com/2021/06/02/2QDphq.md.png)](https://imgtu.com/i/2QDphq)
[![2QDC90.md.png](https://z3.ax1x.com/2021/06/02/2QDC90.md.png)](https://imgtu.com/i/2QDC90)

## 选择排序

> 时间复杂度 O(n^2);稳定的排序方法，

首先在未排序序列中找到最小元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小元素，然后放到已未排序序列的第一个。

```c
void selection_sort(int arr[], int len)
{
    int i,j;
        for (i = 0 ; i < len - 1 ; i++)
    {
                int min = i; //用于记录最小值的下标
                for (j = i + 1; j < len; j++)     //从后面的元素开始找
                        if (arr[j] < arr[min])    //找到比当前元素小的值
                                min = j;    //紀錄最小值的下标
                swap(&arr[min], &arr[i]);    //交换当前值与最小值
        }
}
```

## 冒泡排序

> 时间复杂度 O(n^2);稳定的排序方法，

比较相邻的元素。如果第一个比第二个大，就交换它们两个；

```c
void bubble_sort(int arr[], int len) {
        int i, j, temp;
        for (i = 0; i < len - 1; i++)
                for (j = 0; j < len - 1 - i; j++)
                        if (arr[j] > arr[j + 1]) {
                                temp = arr[j];
                                arr[j] = arr[j + 1];
                                arr[j + 1] = temp;
                        }
}
```

## 插入排序

> 时间复杂度为 O(n^2) ;稳定的排序方法，n 值较小时的最佳排序方法；

取出下一个元素，在已经排序的元素序列中从后向前扫描；如果已排序中的元素大于该元素，则将比它大的那个元素后移到下一位置,重复以上步骤，直到找到已排序的元素*小于或者等于新元素*的位置；将新元素插入到该位置后；

```c
void insertion_sort(int arr[], int len){ //传入数组和数组长度
        int i,j,key;
        for (i=1;i<len;i++){
                key = arr[i]; //key等于当前元素
                j=i-1;  //j为前一个元素下标
                while((j>=0) && (arr[j]>key)) {  //如果前面的元素大于当前元素
                        arr[j+1] = arr[j];  //则比它大的那个元素后移一位
                        j--;  //j下标前移
                }
                arr[j+1] = key;  //前面大的元素已经后移一位，现在只需插入当前元素
        }
}
```

## 插入排序-希尔排序

> 时间复杂度为 O(n^(3/2));不稳定的排序方法，

将整个待排序的记录序列分割成为若干子序列,分别进行直接插入排序，具体算法描述：

选择一个增量序列 t1，t2，…，tk，其中 ti>tj，tk=1；(dlta[k]=2^(t-k+1)-1, t 为排序趟数，1<=k<=log(2)(n+1)]
按增量序列个数 k，对序列进行 k 趟排序；
每趟排序，根据对应的增量 ti，将待排序列分割成若干长度为 m 的子序列，分别对各子表进行直接插入排序。仅增量因子为 1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。

```c
void shell_sort(int arr[], int len) {
        int gap, i, j;
        int temp;
        for (gap = len >> 1; gap > 0; gap >>= 1) //
                for (i = gap; i < len; i++) {
                        temp = arr[i];
                        for (j = i - gap; j >= 0 && arr[j] > temp; j -= gap){ //
     arr[j + gap] = arr[j];
   }
                        arr[j + gap] = temp;
                }
}
```

## 归并排序

> 时间复杂度 O(nlogn);n 值较大时，所需时间较堆排序省，但所需的辅助存储量最多。

把长度为 n 的输入序列分成两个长度为 n/2 的子序列；
对这两个子序列分别采用归并排序；
将两个排序好的子序列合并成一个最终的排序序列。

```c
int min(int x, int y) {
    return x < y ? x : y;
}
void merge_sort(int arr[], int len) {
    int *a = arr;
    int *b = (int *) malloc(len * sizeof(int));
    int seg, start;
    for (seg = 1; seg < len; seg += seg) {
        for (start = 0; start < len; start += seg * 2) {
            int low = start, mid = min(start + seg, len), high = min(start + seg * 2, len);
            int k = low;
            int start1 = low, end1 = mid;
            int start2 = mid, end2 = high;
            while (start1 < end1 && start2 < end2)
                b[k++] = a[start1] < a[start2] ? a[start1++] : a[start2++];
            while (start1 < end1)
                b[k++] = a[start1++];
            while (start2 < end2)
                b[k++] = a[start2++];
        }
        int *temp = a;
        a = b;
        b = temp;
    }
    if (a != arr) {
        int i;
        for (i = 0; i < len; i++)
            b[i] = a[i];
        b = a;
    }
    free(b);
}
```

递归版：

```c
void merge_sort_recursive(int arr[], int reg[], int start, int end) {
    if (start >= end)
        return;
    int len = end - start, mid = (len >> 1) + start;
    int start1 = start, end1 = mid;
    int start2 = mid + 1, end2 = end;
    merge_sort_recursive(arr, reg, start1, end1);
    merge_sort_recursive(arr, reg, start2, end2);
    int k = start;
    while (start1 <= end1 && start2 <= end2)
        reg[k++] = arr[start1] < arr[start2] ? arr[start1++] : arr[start2++];
    while (start1 <= end1)
        reg[k++] = arr[start1++];
    while (start2 <= end2)
        reg[k++] = arr[start2++];
    for (k = start; k <= end; k++)
        arr[k] = reg[k];
}

void merge_sort(int arr[], const int len) {
    int reg[len];
    merge_sort_recursive(arr, reg, 0, len - 1);
}
```

## 快速排序

> 时间复杂度 O(nlogn),不稳定的排序方法，最坏情况 O(n^2)

从数列中挑出一个元素，称为 “基准”（pivot）；
重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；
递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序。

## 堆排序

> 时间复杂度 O(nlogn);不稳定的排序方法，

将初始待排序关键字序列(R1,R2….Rn)构建成大顶堆，此堆为初始的无序区；
将堆顶元素 R[1]与最后一个元素 R[n]交换，此时得到新的无序区(R1,R2,……Rn-1)和新的有序区(Rn),且满足 R[1,2…n-1]<=R[n]；
由于交换后新的堆顶 R[1]可能违反堆的性质，因此需要对当前无序区(R1,R2,……Rn-1)调整为新堆，然后再次将 R[1]与无序区最后一个元素交换，得到新的无序区(R1,R2….Rn-2)和新的有序区(Rn-1,Rn)。不断重复此过程直到有序区的元素个数为 n-1，则整个排序过程完成。

## 基数排序

> 时间复杂度 O(d(n+rd));稳定的排序方法，适用于 n 值较大而关键词较小的序列
