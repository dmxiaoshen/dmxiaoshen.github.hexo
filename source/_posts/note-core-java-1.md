title: Java核心技术---排序
date: 2015-04-19 19:17:11
categories: 笔记
tags: [java]
---

最近在看《Java核心技术这本书》，算是巩固下基础吧，网上有电子版PDF下载。推荐一个网址**[百度网盘搜索引擎](http://www.pan1234.com)**，这里可以搜索大量资源。

书目前看到第三章，里面讲java各种基本类型以及String和数组。突然心血来潮，想整理下java常规的几个排序。**冒泡排序，选择排序，插入排序，快速排序**。

先普及下算法的时间复杂度是什么以及如何计算：

> 一般来说,时间复杂度是总运算次数表达式中受n的变化影响最大的那一项(不含系数)
> 比如：一般总运算次数表达式类似于这样：
> a\*2^n+b\*n^3+c\*n^2+d\*n\*lg(n)+e\*n+f
> a0时,时间复杂度就是O(2^n);
> a=0,b0 =>O(n^3);
> a,b=0,c0 =>O(n^2)依此类推
> 那么,总运算次数又是如何计算出的呢?
> 一般来说,我们经常使用for循环,就像刚才五个题,我们就以它们为例
> 1.循环了n\*n次,当然是O(n^2)
> 2.循环了(n+n-1+n-2+...+1)≈(n^2)/2,因为时间复杂度是不考虑系数的,所以也是O(n^2)
> 3.循环了(1+2+3+...+n)≈(n^2)/2,当然也是O(n^2)
> 4.循环了n-1≈n次,所以是O(n)
> 5.循环了(1^2+2^2+3^2+...+n^2)=n(n+1)(2n+1)/6(这个公式要记住哦)≈(n^3)/3,不考虑系数,自然是O(n^3)
> 另外,在时间复杂度中,log(2,n)(以2为底)与lg(n)(以10为底)是等价的,因为对数换底公式：
> log(a,b)=log(c,b)/log(c,a)
> 所以,log(2,n)=log(2,10)\*lg(n),忽略掉系数,二者当然是等价的

**常见时间复杂度:O(1)<O(lg n)<O(n)<O(n lg n)<O(n^2)<O(n^3)<O(2^n)**

##冒泡排序

通俗点讲，冒泡排序就是相邻两数比较，大的放后面，这样一遍下来，**最后的数**就是最大的数，如此往复，直到元素从小到大有序排列。

我们来思考下时间复杂度，假设有n个数：　　
第1遍，遍历了n个数，共进行了n-1次比较。　　
第2遍，遍历了n-1个数(最后一个已经排序),共需要n-2次比较。　　
……
第n-1遍，遍历了2个数，仅需1次比较。　　
最后只剩一个数，无需遍历，它就是最小的。

那么最后遍历共n-1遍,比较共（1+2+3+……+n-1）次。复杂度为（n-1)\*(1+2+3+……+n-1),影响最大的项是n^2(系数忽略)。

因此，时间复杂度为**O(n^2)**.

以下是简单实现:
```java
public static void bubbleSort(int[] a){
	int length = a.length;
	int temp = 0;
	for(int i=1;i<length;i++){
		for(int j=0;j<length-i;j++){//每次要从第一个数开始比较，因为从后往前排的
			if(a[j]>a[j+1]){
				temp = a[j];
				a[j] = a[j+1];
				a[j+1] = temp;
			}
		}
	}
}
```

##选择排序

选择排序就是遍历一边所有的数，把最小的数找出来放到第一个位置，一遍以后**第一个数**就是最小的数，如此往复，直到所有元素都排序。

时间复杂度为共遍历(n-1)遍，共比较(n-1+……+3+2+1）次，复杂度为（n-1)\*(1+2+3+……+n-1),影响最大的项是n^2(系数忽略)。

因此，时间复杂度为**O(n^2)**.

以下是简单实现:
```java
public static void selectSort(int[] a){
	int length = a.length;
	int min = 0;
	int pos = 0;

	for(int i=0;i<length-1;i++){
		min = a[i];
		pos = i;
		for(int j=i;j<length;j++){//每次都要比较到最后一个数，因为从前往后排的
			if(a[j]<min){
				min = a[j];
				pos = j;
			}
		}
		if(pos!=i){
			a[pos] = a[i];
			a[i] = min;
		}
	}
}
```

##插入排序

插入排序是这样的，假设第一个数是已排序的，后面的数依次跟前面已排序的数比较，直到找到它该有的位置（类似于玩扑克时候的摸牌）。

时间复杂度是：　　
第1次，取第二个数，与前面1个数比较　　
第2次，取第三个数，与前面2个数比较　　
……　　
第n-1次，取第n个数，与前面n-1个数比较

共需循环取数(n-1)次，共比较(1+2+3+……+n-1)次。复杂度为（n-1)\*(1+2+3+……+n-1),影响最大的项是n^2(系数忽略)。

因此，时间复杂度为**O(n^2)**.

以下是简单实现:
```java
public static void insertSort(int[] a){
	int length = a.length;
	int temp = 0;
	int pos = 0;

	for(int i =1;i<length;i++){
		temp = a[i];
		pos = i;
		for(int j=i-1;j>=0;j--){
			if(a[j]>temp){
				a[j+1] = a[j];
				pos = j;
			}
		}

		if(pos!=i){
			a[pos] = temp;
		}
	}
}
```

##快速排序

简单的说，快速排序就是选一个基数，小于它的数排在它左边，大于它的数排在它右边，这样一遍下来以后，如果以该数的位置做一条竖线，可以划分为左右两部分。左右两边又可以依照刚才的方法分别处理。  

这其实体现了一种分治的思想。因此可以用到递归。

![快速排序](http://photo.hanyu.iciba.com/upload/encyclopedia_2/5e/e2/bk_5ee213ee925122f6aef374cf940c4f95_3Cw3Ah.jpg)

时间复杂度，快速排序算法的时间复杂度依赖于给定的数据，试想一下，如果给定的数据已经是有序的，那么该种情况下二分的意义已经不是很大，n个数几乎分了n次，时间复杂度为**O(n^2)**，这是最糟糕的情况。

但是对于一般的情况，我们认为二分总是有效的。因此，快速排序算法的平均时间复杂度为**O(n lg n)**。

以下是简单实现:
```java
public static void quickSort(int[] a,int left,int right){
	if(left >= right)
		return;

	int low = left;
	int high = right;
	boolean fromRight = true;//先从右边遍历,因为基数选择了左边第一个，确保那个基数原先的位置可以“动”起来
	int base = a[left];

	while(low<high){
		if(fromRight){
			if(a[high]<base){
				a[low] = a[high];
				low++;//此处++是为了从左边处理的时候可以少一次++操作，注释掉代码依旧可行
				fromRight = false;
			}else{
				high--;
			}
		}else{
			if(a[low]>base){
				a[high] = a[low];
				high--;//此处--是为了从右边处理的时候可以少一次--操作，注释掉代码依旧可行
				fromRight = true;
			}else{
				low++;
			}
		}
	}

	a[low] = base;//此时 low==high
	quickSort(a,left,low-1);
	quickSort(a,low+1,right);
}
```

以上就是几个简单的排序算法分析，下次如果有机会再分析几种。