#### 计数排序

* 1051 heightChecker https://leetcode-cn.com/problems/height-checker/

* 特点：

1. 计数排序要求输入的数据必须是有确定范围的整数，最好是数据量大且取值范围小的数据。当输入的元素是n个0到k之间的整数时，它的运行时间是O(n + k)。
2. 非比较排序，排序时间效率优于比较排序。

``` Java
import java.util.Arrays;

public class SortTest {
	
	public static void main(String[] args) {
		int[] arr1 = {1,4,2,1,9,6};
		int[] arr2 = {100,99,98,97,96};
		int[] arr3 = {};
		
		System.out.println(Arrays.toString(countSort1(arr1)));
		System.out.println(Arrays.toString(countSort1(arr2)));
		System.out.println(Arrays.toString(countSort1(arr3)));
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

