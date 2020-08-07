---
title: 几种常见的排序算法的java实现
date:  2019-08-15 10:45:13 +0800
category: develop
tags: algorithm
excerpt: 读数据结构与算法笔记。
---

首先看一下排序的定义：
>排序，就是重新排列表中的元素，使表中的元素满足按关键字递增或递减的过程。为了查找方便，通常需要计算机中的表示按关键字有序的。

接着是算法的稳定性：
>若待排序表中有两个元素Ri和Rj，其对应的关键字keyi=keyj，且在排序前Ri在Rj前面，若使用某一排序算法后，Ri仍然在Rj前面，则称这个排序算法是**稳定的**，否则称这个排序算法是**不稳定的**。需要注意的是，算法是否具有稳定性并不能衡量一个算法的优劣，它主要是对算法的性质进行描述。

### 一、插入排序
>插入排序是一种简单直观的排序方法，其基本思想是每次将一个待排序的记录按其关键字大小插到前面已排好的子序列中，直到全部记录插入完成。
由插入排序的思想可以引申出三个重要的排序算法：直接插入排序、折半插入排序和希尔排序。

#### 1、直接插入排序
简单粗暴的代码先上⬇️
```java
void insert_sort(int s[]) {
        int temp = 0;
        for (int i = 1; i < s.length; i++) {
            if (s[i] < s[i - 1]) {
                temp = s[i];
                while(i>0 && temp<s[i-1]){
                    s[i]=s[i-1];
                    i--;
                }
                s[i]=temp;
            }
        }
    }
```
空间复杂度O(1)，时间复杂度为O(n2)，是一个稳定排序方法，适用于顺序存储和链式存储的线性表。

#### 2、折半插入排序
折半插入排序（Binary Insertion Sort）是对插入排序算法的一种改进，所谓排序算法过程，就是不断的依次将元素插入前面已排好序的序列中。
排序思想：有一组数据待排序，排序区间为Array[0] ~ Array[n-1]。将数据分为有序数据和无序数据，第一次排序时默认Array[0]为有序数据，Array[1] ~ Array[n-1]为无序数据。有序数据分区的第一个元素位置为low，最后一个元素的位置为high。
遍历无序区间的所有元素，每次取无序区间的第一个元素Array[i]，因为0 ~ i-1是有序排列的，所以用中点m将其平分为两部分，然后将待排序数据同中间位置为m的数据进行比较，若待排序数据较大，则low~m-1分区的数据都比待排序数据小，反之，若待排序数据较小，则m+1 ~ high分区的数据都比 待排序数据大，此时将low或high重新定义为新的合适分区的边界，对新的小分区重复上面操作。直到low和high 的前后顺序改变，此时high+1所处位置为待排序数据的合适位置。

下面是代码：
```java
void half_insert_sort(int s[]) {
        int i,j,low,high,mid;
        int temp;
        for (i = 1; i < s.length; i++) {
            temp = s[i];
            low = 0;
            high = i-1;
            while (low <= high) {
                mid = (low + high) / 2;
                if(s[mid]>temp) high = mid - 1;
                else low = mid + 1;
            }
            for (j = i - 1; j >= high + 1; --j) s[j + 1] = s[j];
            s[high+1]=temp;
        }
    }
```
折半插入排序仅减少了比较元素的次数，约为O(nlog2n)，时间复杂度O(n2)，空间复杂度O(1)，是一个稳定排序方法。

#### 3、希尔排序
>希尔排序的基本思想是：先将排序表分割成若干形如L[i,i+d,i+2d,...,i+kd]的“特殊”子表，分别进行插入排序，当整个表中的元素已呈“基本有序”时，再对全体记录进行一次直接插入排序。

代码如下：
```java
void ShellSort(int s[]) {
        int n = s.length;
        int dk;
        int temp;
        int i,j;
        for (dk = n / 2; dk >= 1; dk = dk / 2) {
            for (i = dk; i < n; i++) {
                if(s[i]<s[i-dk]){
                    temp = s[i];
                    for (j = i - dk; j >=0 && temp<=s[j]; j -= dk) {
                        s[j+dk]=s[j];
                    }
                    s[j+dk]=temp;
                }
            }
        }
    }
```
该算法空间复杂度O(1)，时间复杂度O(n2)，不稳定，仅适用于线性表为顺序存储的情况。

### 二、交换排序
>所谓交换，是指根据序列中两个关键字的比较结果来对换这两个记录在序列中的位置，基于交换的排序算法很多，主要有冒泡排序和快速排序。

#### 1、冒泡排序
冒泡排序的基本思想是：假设待排序表长为n，从后往前（或从前往后）两两比较相邻元素的值，若为逆序（即A[i-1]>A[i]），则交换他们，直到序列比较完。我们称它为一趟冒泡，结果是将最小的元素交换到待排序列的第一个位置。下一趟排列时，前一趟确定的最小元素不再参加比较，待排序列减少一个元素，每趟冒泡的结果就是把序列中的最小元素放到了最终的位置，这样最多做n-1趟冒泡就能把所有元素排好序。

算法代码如下：
```java
void BubbleSort(int s[]) {
        int temp;
        for(int i=0;i<s.length-1;i++){ //遍历数组长度的次数
            for(int j=0;j<s.length-i-1;j++){
                if(s[j]>s[j+1]){
                    temp=s[j];
                    s[j]=s[j+1];
                    s[j+1]=temp;
                }
            }
        }
    } //向后冒泡

void Bubble(int s[]) {
        int temp;
        for (int i = 0; i < s.length-1; i++) {
            for(int j=s.length-1;j>i;j--){
                if(s[j]<s[j-1]){
                    temp = s[j];
                    s[j] = s[j - 1];
                    s[j - 1] = temp;
                }
            }
        }
    } //向前冒泡
```

以上包含了向前冒泡和向后冒泡。空间复杂度为O(1)，时间复杂度为O(n2)，是一种稳定的排序方法。

#### 2、快速排序
快速排序是很重要的排序方法，面试中经常会问。快速排序是对冒泡排序的一种改进，其基本思想是分治，更多详细内容请戳：[https://www.runoob.com/w3cnote/quick-sort.html](https://www.runoob.com/w3cnote/quick-sort.html)

下面是实现代码：
```java
void quick_sort(int s[], int low, int high)
    {
        if (low < high)
        {
            int i = low, j = high, x = s[low];
            while (i < j)
            {
                while(i < j && s[j] >= x) // 从右向左找第一个小于x的数
                    j--;  
                if(i < j) 
                    s[i++] = s[j];
                
                while(i < j && s[i] < x) // 从左向右找第一个大于等于x的数
                    i++;  
                if(i < j) 
                    s[j--] = s[i];
            }
            s[i] = x;
            quick_sort(s, low, i - 1); // 递归调用
            quick_sort(s, i + 1, high);
        }
    }
```
*对算法的最好理解方式是手动地模拟一遍这些算法*
该算法空间复杂度最坏情况下是O(n)，在平均情况下是O(log2n)，时间复杂度为O(n2)，是一个不稳定的排序方法。
注：在快速排序算法中，并不产生有序子序列，但每趟排序后悔将一个元素（基准元素）放到其最终的位置上。