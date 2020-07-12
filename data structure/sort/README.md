#### 计数排序

* 1051 heightChecker https://leetcode-cn.com/problems/height-checker/

1. 计数排序要求输入的数据必须是有确定范围的整数，最好是数据量大且取值范围小的数据。当输入的元素是n个0到k之间的整数时，它的运行时间是O(n + k)。

2. 非比较排序，排序时间效率优于比较排序。

3. 最简单的情况：输入数据在0-100之间

``` Java
import java.util.Arrays;

public class SortTest {
	
	public static void main(String[] args) {
		int[] arr1 = {1,4,2,1,9,6};
		int[] arr2 = {100,99,98,97,96};
		int[] arr3 = {};
		// test countSort1
		System.out.println(Arrays.toString(countSort1(arr1)));
		System.out.println(Arrays.toString(countSort1(arr2)));
		System.out.println(Arrays.toString(countSort1(arr3)));
		
		// test countSort2
		int[] arr4 = {630, 629, 750, 630, 645, 620, 629};
		int minVal = findMin(arr4);
		int maxVal = findMax(arr4);
		
		//System.out.println(minVal + ", " + maxVal);
		
		System.out.println(Arrays.toString(countSort2(arr4, minVal, maxVal)));
	}
	
	public static int[] countSort1(int[] arr) {
		/**
		 * return a new sorted list by using countSort
		 * 
		 * input arr: an integer array, 0 <= arr[i] <= 100
		 */
		if (arr.length <= 1) {
			return arr;
		}
		
		int[] result = new int[arr.length]; // create an array that has the same length as the input array
		int[] count = new int[101]; // here the maximum is 100 so we need 101 elements
		
		// Step 1: count the number of each integer in the input array.
		// Result: In the count array, count[i]: the # of integer i 
		for (int a : arr) {
			count[a]++;
		}
		//System.out.println(Arrays.toString(count));
		// Step 2: fulfill the result array
		for (int i=0, j=0; i < count.length; i++) {
			while (count[i]-- > 0) {
				result[j++] = i;
			}
		}
		return result;
	}
 }

```

4.改进：上面countSort1假设了输入元素在0-100之间，那么如果输入数组中的元素均为数值大的数，如果仍然生成0-maxVal这样的数组来计数，会导致前半部分皆为0。此时，将countSort1升级为countSort2(int[] arr, int minVal, int maxVal)方法，利用minVal maxVal建立一个长度为（maxVal-minVal+1）的计数数组，仅需改动下标即可实现计数排序，代码如下：

``` Java
	public static int[] countSort2(int[] arr, int minVal, int maxVal) {
		/**
		 * return a new sorted array of the input array
		 * 
		 * input arr: an integer array, containing non-negative integers
		 * input minVal: the smallest integer in the input array
		 * input maxVal: the largest integer in the input array
		 */
		if (arr.length <= 1) {
			return arr;
		}
		
		int range = maxVal-minVal+1;
		int[] result = new int[arr.length]; // create an array that has the same length as the input array
		int[] count = new int[range];
		
		// Step 1: count the number of each element
		for (int a: arr) {
			count[a-minVal]++;
		}
		
		// Step 2: fulfill the result array
		for (int i=0, j=0; i < count.length; i++) {
			while(count[i]-- > 0) {
				result[j++] = i + minVal;
			}
		}
		
		return result;
	}
	
	public static int findMax(int[] arr) {
		int result = arr[0];
		
		for (int a : arr) {
			if (a > result) {
				result = a;
			}
		}
		
		return result;
	}
	
	public static int findMin(int[] arr) {
		int result = arr[0];
		
		for (int a: arr) {
			if (a < result) result = a;
		}
		return result;
	}
```
			
