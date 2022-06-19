---
title: 程序员要会的一些算法问题总结
date: 2016-01-17 10:14:08
tags:
- 算法
categories:
- 技术资料
---
无论是面试还是平时工作，都需要掌握基本的数据结构和排序算，这里我总结下几种排序算法
## 算法相关
算法代码是用java实现的
### 冒泡排序
设数组长度为n
* 比较前后相邻两个数据，如果前面数据大于后面，将两个数据交换
* 对数组0～n-1个数据进行一次遍历后，最大的数据沉到n-1位置
* n=n-1，重复前两步

```java
//冒泡排序1
public void bubbleSort1() {
		int a[] = { 1, 15, 8, 12, 19 };
		int n = a.length;
		int i, j;
		for (i = 0; i < n; i++) {
			for (j = 1; j < n - i; j++) {
				if (a[j - 1] > a[j]) {
					int temp = a[j];
					a[j] = a[j - 1];
					a[j - 1] = temp;
				}
			}
		}
		System.out.println("Bubble sort 1: ");
		for (i = 0; i < n; i++) {
			System.out.println(a[i]);
		}
		System.out.println("--------");
	}
```
进行优化，设置一个boolean，如果这趟发生了交换，则为true，否则为false。如果有一趟没有发生交换，说明排序已经完成
```java
//冒泡排序2
public void bubbleSort2() {
		int a[] = { 1, 15, 8, 12, 19 };
		int n = a.length;
		int i, j;
		int k = n;
		boolean flag = true;
		while(flag){
			flag = false;
			for (j = 1; j < k ; j++) {
				if (a[j - 1] > a[j]) {
					int temp = a[j];
					a[j] = a[j - 1];
					a[j - 1] = temp;
					flag = true;
				}
			}
			k--;
		}
		System.out.println("Bubble sort 2: ");
		for (i = 0; i < n; i++) {
			System.out.println(a[i]);
		}
		System.out.println("--------");
	}
```
再做一步优化，如果有100个数的数组，仅前面10个无序，后面90个都已经排好序，且都大于前面10个数字。在第一趟遍历后，最后发生交换的位置必定小于10，只要记录下这位置，第二次从数组头部遍历到这个位置就OK。
```java
//冒泡排序3
public void bubbleSort3() {
		int a[] = { 1, 15, 8, 12, 19 };
		int n = a.length;
		int i, j;
		int flag;
		flag = n;
		while (flag > 0) {
			int k = flag;
			flag = 0;
			for (j = 1; j < k; j++) {
				if (a[j - 1] > a[j]) {
					int temp = a[j];
					a[j] = a[j - 1];
					a[j - 1] = temp;
					flag = j;
				}
			}
		}
		System.out.println("Bubble sort 3: ");
		for (i = 0; i < n; i++) {
			System.out.println(a[i]);
		}
		System.out.println("--------");
	}
```
冒泡排序我们可以简单理解为，大的数往下沉，两层循环遍历整个数组。冒泡排序效率很低，数据规模很小的时候，可以采用，规模较大时，最后采用其他排序方法。
### 直接插入排序
基本思想是，每次将一个带排序的记录，按其关键字大小插入到前面已经排好序的子序列中的适当位置，直到全部记录插入完成为止。
**设数组为a[0...n-1]**
* 初始时，a[0]自成一个有序区，无序区为a[1...n-1]。令i=1
* 将a[i]并入到当前有序区a[0...i-1]中，形成a[0...i]的有序区。
* i++并重复第二步直到i==n-1。排序完成。

```java
public void insertSort3() {
		int a[] = { 3, 14, 2, 8, 2, 1 };
		int n = a.length;
		int i, j, k;
		for (i = 1; i < n; i++) {
			for(j=i-1;j>=0&&a[j]>a[j+1];j--){
				int temp = a[j];
				a[j] = a[j+1];
				a[j+1] = temp;
			}
		}
		System.out.println("Insert sort 3: ");
		for (i = 0; i < n; i++) {
			System.out.println(a[i]);
		}
		System.out.println("--------");
	}
```
### 希尔排序
实质是分组插入排序，又称为缩小增量排序，基本思想为：
* 先将整个待排元素序列分割成若干个子序列（由相隔某个“增量”的元素组成），分别进行直接插入排序
* 依次缩减增量再进行排序，待整个序列中的元素基本有序时，再对全体元素进行一次直接插入排序
因为直接插入排序在元素基本有序情况下（接近最好情况），效率很高，，因此希尔排序在时间效率上比前两种方法有较大提高。

```
以n=10的数组为例，{49,38,65,97,26,13,27,49,55,4}
-- 第一次gap=10/2=5
	（竖着看）
    49 38 65 97 26
    13 27 49 55 4
   每次对同一组的数据进行直接插入排序，即分为5组，排序后
   13 27 49 55 4
	 49 38 65 97 26
-- 第二次gap=5/2=2
  （竖着看）
    13 27
    49 55
     4 49
    38 65
    97 26
    排序后，为(4 13 38 49 97)，(26,27,49,55,65)
		现在数组为{4,26,13,27,38,49,49,55,97,65}
-- 第三次gap=2/2=1
		4 26 13 27 38 49 49 55 97 65
		排序后
		4 13 26 27 38 49 49 55 65 97
```
```java
//希尔排序
public void shellSort(){
		int a[] = { 49,38,65,97,26,13,27,49,55,4 };
		int n = a.length;
		int i,j,gap;
		for(gap=n/2;gap>0;gap/=2){
			for(i=gap;i<n;i++){
				for(j=i-gap;j>=0&&a[j]>a[j+gap];j-=gap){
					int temp = a[j];
					a[j] = a[j+gap];
					a[j+gap] = temp;
				}
			}
		}
		System.out.println("Shell sort: ");
		for (i = 0; i < n; i++) {
			System.out.println(a[i]);
		}
		System.out.println("--------");
	}
```
### 直接选择排序
和直接插入排序类似，都将数据分为有序区和无序区，不同的是，直接插入排序是将无序区的第一个元素直接插入到有序区以形成一个更大的有序区，而直接选择排序是从无序区选一个最小的元素直接放到有序区的最后。<br>
设数组为a[0...n-1]
* 初始时，数组全为无序区，为a[0...n-1]，令i=0
* 在无序区a[i...n-1]中选取一个最小的元素，将其与a[i]交换，交换后a[0...i]就形成一个有序区
* i++，并重复第二步直到i=n-1。排序完成

```java
//直接选择排序
public void selectSort(){
		int a[] = { 49,38,65,97,26,13,27,49,55,4 };
		int n = a.length;
		int i,j,minIndex;
		for(i=0;i<n;i++){
			minIndex = i;
			for(j=i+1;j<n;j++){
				if(a[j]<a[minIndex]){
					minIndex = j;
				}
			}
			int temp = a[i];
			a[i] = a[minIndex];
			a[minIndex] = temp;
		}
		System.out.println("Select sort: ");
		for (i = 0; i < n; i++) {
			System.out.println(a[i]);
		}
		System.out.println("--------");
	}
```
### 归并排序
归并排序是建立在归并操作上的一种有效的排序算法，该算法是采用分治法。<br>
{% asset_img Merge-sort-example-300px.gif 归并排序 %}
首先考虑下如何将两个有序数列合并，只要从比较两个数列的第一个数，谁小就先取谁，取了后就在对应数列中删除这个数，然后再进行比较，如果有数列为空，那直接将另一个数列的数据依次取出，效率可以达到O(n)。<br>
解决了合并有序数列问题，再来看归并排序，基本想法是将数组分为A、B组，如果两组都是有序的，可以很方便的将这两组数据进行排序。如何让两组都是有序的？可以将AB继续再分为两组，依次类推，当分出来的小组只有一个数据时，可以认为这个小组达到有序，然后再合并相邻两个小组即可。
```java
//归并排序
public void mergerSort() {
		int arr[] = { 3, 5, 7, 123, 534, 345, 8, 13, 67, 90, 34, 12, 67, 90, 32 };
		int n = arr.length;
		msort(arr, 0, n - 1);
		int i;
		for (i = 0; i < n; i++) {
			System.out.println(arr[i]);
		}
		System.out.println("--------");
	}

	private void msort(int[] arr, int low, int high) {
		int mid = (low + high) / 2;
		if (low < high) {
			msort(arr, low, mid);
			msort(arr, mid + 1, high);
			merger(arr, low, mid, high);
		}
	}

	// 将两个有序数组合并成一个有序数组
	private void merger(int[] arr, int low, int mid, int high) {
		// temp数组用于暂存合并的结果
		int[] temp = new int[high - low + 1];
		// 左半边的指针
		int i = low;
		// 右半边的指针
		int j = mid + 1;
		// 合并后数组的指针
		int k = 0;
		//将记录由小到大放进temp数组
		for (; i <= mid && j <= high; k++) {
			if (arr[i] < arr[j]) {
				temp[k] = arr[i++];
			} else {
				temp[k] = arr[j++];
			}
		}
		//将剩余放到temp中
		while (i <= mid) {
			temp[k++] = arr[i++];
		}
		while (j <= high) {
			temp[k++] = arr[j++];
		}
		//将temp写入到待排数组
		for (int l = 0; l < temp.length; l++) {
			arr[low + l] = temp[l];
		}
	}
```
### 快速排序
快速排序由于排序效率在同为O(nlogn)的几种排序方法中效率最高的，它采用的是一种分治的策略，通常叫分治法。<br>
{% asset_img Sorting_quicksort_anim.gif 快速排序 %}
基本思想：
* 先从数列中取出一个数作为基准数
* 分区过程，将比这个数大的数全放在它的右边，小于或等于它的数放到它的左边
* 再对左右分区重复第二步，直到各区中只有一个数。

```java
//快速排序
public void quickSort(){
		int arr[] = {3,5,7,123,534,345,8,13,67,90,34,12,67,90,32};
		int n = arr.length;
		qsort(arr,0,n-1);
		System.out.println("Quick sort: ");
		int i;
		for (i = 0; i < n; i++) {
			System.out.println(arr[i]);
		}
		System.out.println("--------");
	}

	private static void qsort(int[] arr, int low, int high){
		if(low<high){
			int pivot = partition(arr,low,high);
			qsort(arr, low, pivot-1);
			qsort(arr,pivot+1,high);
		}
	}
	private static int partition(int[] arr, int low,int high){
		int pivot = arr[low];
		while(low<high){
			while(low<high && arr[high]>=pivot) --high;
			arr[low] = arr[high];
			while(low<high && arr[low]<=pivot) ++low;
			arr[high] = arr[low];
		}
		arr[low] = pivot;
		return low;
	}
```
### 堆排序
{% asset_img Sorting_heapsort_anim.gif 堆排序 %}
先复习下数据结构的二叉堆
#### 二叉堆
二叉堆是完全二叉树或者是近似完全二叉树。<br>
二叉堆需要满足的特性：
* 父节点的键值总是大于或等于（小于或等于）任何一个子节点的键值
* 每个节点的左子树和右子树都是一个二叉堆（最大堆或最小堆）
当父节点的键值总是大于或等于任何一个子节点的键值时，为最大堆。当父节点的键值总是小于或等于任何一个子节点的键值时，为最小堆。
#### 堆的存储
一般都用数组来表示堆，i节点的父节点下标为(i-1)/2，它的左右子节点下标分别为2×i+1和2×i+2。如第0个节点的左右子节点分别为1和2。
#### 堆的操作——建立插入删除
1. 建立堆： 数组具有对应的树表示形式。一般情况，树并不满足堆的条件，通过重新排列元素，可以建立一棵“堆化”的树
2. 插入： 新元素被加到表层，随后树被更新以恢复堆次序。
3. 删除： 删除总是发生在根A[0]处，表中最后一个元素被用来填补空缺位置，结果树被更新以恢复堆条件。
#### 堆的插入
每次插入都是将新数据放在数组最后。可以发现，从这个新数据的父节点到根节点必然为一个有序的数列。将这个新数据插入到这个有序数据中，这就类似于[直接插入排序](#直接插入排序)，我们可以写出一个插入一个新数据时堆的调整代码：
```java
private void minHeapFixup(int[] a, int i) {
		int j, temp;
		temp = a[i];
		j = (i - 1) / 2;// 父节点
		while (j >= 0 && i != 0) {
			if (a[j] <= temp)
				break;
			a[i] = a[j];// 把较大的子节点往下移动，替换它的子节点
			i = j;
			j = (i - 1) / 2;
		}
		a[i] = temp;
	}
```
#### 堆的删除
按照定义，堆中每次只能删除第0个数据。为了便于重建堆，实际的操作是将最后一个数据的值赋给根节点，然后再从根节点开始进行一次从上向下的调整。调整时，先在左右儿子节点中找最小的，如果父节点比这个最小的子节点还小，则不需要调整，反之将父节点和它交换，然后再考虑后面的节点。
```java
// 从i节点开始调整，n为节点总数，
private void minHeapFixdown(int[] a, int i, int n) {
		int j, temp;
		temp = a[i];
		j = 2 * i + 1;
		while (j < n) {
			if (j + 1 < n && a[j + 1] < a[j])// 在左右子节点中找最小的
				j++;
			if (a[j] > temp)
				break;
			a[i] = a[j];// 把较小的子节点往上移动，替换父节点
			i = j;
			j = 2 * i + 1;
		}
		a[i] = temp;
	}
//在最小堆中删除数
private void minHeapDeleteNumber(int[] a, int n){
	int temp = a[0];
	a[0] = a[n-1];
	a[n-1] = temp;
	minHeapFixdown(a, 0, n-1);
}
```
#### 堆化数组
有了堆的插入和删除后，考虑下如何对一个数据进行堆化操作。<br>
考虑一个数组，A[0] = {9,12,17,30,50,20,60,65,4,49}
```
//初始表
            9
        /       \
      12         17
    /    \      /  \
   30    50    20  60
	/ \   /
 65 4  19
```
很明显，对于叶子节点，65，4，19，20，60，可以认为它们是一个合法的堆。只要从A[4]=50开始往下调整就可以，然后再取A[3]=30,A[2]=17,A[1]=12,A[0]=9分别作一次向下调整操作就可以了。
{% asset_img Heap-Sort-Array.PNG 堆化数组示意图 %}
可以写出堆化数组的代码：
```java
private void makeMinHeap(int[] a, int n){
	for( int i = n/2 - 1; i >= 0; i--)
		minHeapFixdown(a, i, n);
}
```
#### 堆排序就是堆插入和堆删除
OK，我们回归正题。从上面可以看到，堆建好之后堆中第0个数据是堆中最小的数据。取出这个数据再执行下堆的删除操作，这样堆中第0个数据又是堆中最小的数据，重复上述步骤，直到堆中只有一个数据时，就取出这个数据。由于堆也是用数组模拟的，所以[堆化数组](#堆化数组)后，第一次将A[0]与A[n-1]交换，再对A[0...n-2]重新恢复堆。第二次将A[0]与A[n-2]交换，再对A[0...n-3]重新恢复堆，重复这样的操作直到A[0]与A[1]交换。由于每次都是将最小的数据并入到后面有序区间，故操作完成后整个数组就有序了。
```java
public void heapSort() {
		int a[] = { 3, 5, 7, 123, 534, 345, 8, 13, 67, 90, 34, 12, 67, 90, 32 };
		int n = a.length;
		//建最小堆
		for(int i = n -1;i>=1;i--){
			minHeapFixup(a,i);
		}
		//
		for (int i = n - 1; i >= 1; i--) {
			int temp = a[0];
			a[0] = a[i];
			a[i] = temp;
			minHeapFixdown(a, 0, i);
		}
		int i;
		for (i = 0; i < n; i++) {
			System.out.println(a[i]);
		}
		System.out.println("--------");
	}
```
注意：我们用的是最小堆排序，这样排序后得到的是递减数组，如果要得到递增数组，可以使用最大堆。由于每次重新恢复堆的时间复杂度为O(logn)，共n-1次重新恢复堆操作，再加上前面建立堆时n/2次向下调整，每次调整时间复杂度也是O(logn),两次操作时间相加还是O(nlogn)(最终的时间复杂度)。

## 常用排序算法时间复杂度和空间复杂度
稳定排序是所有相等的数经过排序后，仍能保持它们在排序之前的相对次序；反之，就是非稳定的排序。

| 排序法 | 最差时间分析 | 平均时间复杂度 | 空间复杂度 | 稳定性 |
| ------ | ------ | ------ | ------ | ------ |
| 冒泡排序 | $O(n^2)$ | $O(n^2)$ | $O(1)$ | 稳定 |
| 直接插入排序 | $O(n^2)$ | $O(n^2)$ | $O(1)$ | 稳定 |
| 希尔排序 | $O(n^2)$ | $O(n^{1.3})$ | $O(1)$ | 不稳定 |
| 直接选择排序 | $O(n^2)$ | $O(n^2)$ | $O(1)$ | 不稳定 |
| 归并排序 | $O(nlogn)$ | $O(nlogn)$ | $O(n)$ | 稳定 |
| 快速排序 | $O(n^2)$ | $O(nlogn)$ | $O(nlogn)$ | 不稳定 |
| 堆排序 | $O(nlogn)$ | $O(nlogn)$ | $O(1)$ | 不稳定 |

## 参考
1. [Wikipedia-堆排序](https://zh.wikipedia.org/wiki/%E5%A0%86%E6%8E%92%E5%BA%8F)
2. [白话经典算法](http://blog.csdn.net/MoreWindows)
3. [数据结构与算法（Java 描述）](https://dsa.cs.tsinghua.edu.cn/~deng/ds/dsaj/dsaj.pdf)
