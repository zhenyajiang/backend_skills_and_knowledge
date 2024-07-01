# 基础排序算法
### 冒泡算法
原理：
1.从头到尾，两两比对，更小的往头移，更大的往尾移。直到末尾。
2.重复这个过程，从头到尾（此时忽略末尾最后一个数），即从0到n-1。
3.继续重复这个过程，从头到尾，即从0到n-2。
...
n.直到比对到从0到1。结束。

0 ~ n

0 ~ n -1

0 ~ n -2

...

0 ~ 1

<b>基本思路：每次循环保证末尾的数为最大。</b>

```java
public static void bubbleSort(int[] arr) {
		if (arr == null || arr.length < 2) {
			return;
		}
        //重复比对，每次循环，end位置都前移一位
		for (int end = arr.length - 1; end > 0; end--) {
            //从头到尾比对，如果前者更大，则交换
			for (int i = 0; i < end; i++) {
				if (arr[i] > arr[i + 1]) {
					swap(arr, i, i + 1);
				}
			}
		}
	}

	// 交换arr的i和j位置上的值
	public static void swap(int[] arr, int i, int j) {
		int tmp = arr[i];
		arr[i] = arr[j];
		arr[j] = tmp;
	}
```

### 插入算法
原理：
每次保证 0 ~ n 有序，重复此步骤

0 ~ 1 有序

0 ~ 2 有序

0 ~ 3 有序

...

0 ~ n 有序

对比方式为：从后往前比，只要前者比后者大则交换。直到发现没有前者或者前者比自己小就停止。

<b>基本思路：每一次循环都能保证0 ~ n 是有序的。因此，只要前一个循环中的某个数比自己小就停止。</b>


```java
public static void insertionSort(int[] arr) {
		if (arr == null || arr.length < 2) {
			return;
		}
		for (int i = 1; i < arr.length; i++) { // 0 ~ i 做到有序
			for (int j = i - 1; j >= 0 && arr[j] > arr[j + 1]; j--) {
				swap(arr, j, j + 1);
			}
		}
	}

	// i和j,数交换
	public static void swap(int[] arr, int i, int j) {
		int tmp = arr[i];
		arr[i] = arr[j];
		arr[j] = tmp;
	}
```

### 选择排序
原理：
从头到尾遍历，找出最小数下标，直接和最前位置交换。每次循环都保证最小的数在最前。重复此步骤

0 ~ n

1 ~ n

2 ~ n

...

n - 1 ~ n

```java
public static void selectionSort(int[] arr) {
		if (arr == null || arr.length < 2) {
			return;
		}
		for (int i = 0; i < arr.length - 1; i++) { 
			int minIndex = i;
			for (int j = i + 1; j < arr.length; j++) {
				if(arr[j] < arr[minIndex]) {
					minIndex = j;
				}
			}
			swap(arr, i, minIndex);
		}
	}

	public static void swap(int[] arr, int i, int j) {
		int tmp = arr[i];
		arr[i] = arr[j];
		arr[j] = tmp;
	}
```

<b>基本思路：每次循环保证头数最小。</b>