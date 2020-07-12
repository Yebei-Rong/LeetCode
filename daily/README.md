#### 每日记录(按tag和难易程度顺序）

* 1051 heightChecker

https://leetcode-cn.com/problems/height-checker/

本题的重点在于关注元素是否出现在正确的位置上，而非最后的排序结果。因为题目中给出heighs数组的取值范围为1 <= heights[i] <= 100，就应该想到用桶排序-计数排序的思想。因为如果数组取值为0到某数时，计数排序是最简单的，并且适用于数据量大但取值范围小的情况。

``` Java
public class Solution {
	public int heightChecker(int[] heights) {
		/**
		 * return the number of integers that need to be moved in order to realize a non-decreasing list
		 * 
		 * input heights: an integer list, 1 <= heights[i] <= 100, 1 <= heights.length <= 100
		 */
		int count = 0;
		int[] arr = new int[101];
		
		// 桶排序的思想，循环结束后arr中下标为1的元素表示heights中1出现的次数；下标为2的元素表示2出现的次数；
		// 以此类推，直至heights所有元素循环结束；
		// arr[i]: 在heights中整数i出现的次数。
		// i.e. heights = {1,1,4,2,1,3,8}; arr = {0,3,1,1,1,0,0,0,1,0,0,...,0}
		for (int height : heights) {
			arr[height]++;
		}
		
		for (int i = 1, j = 0; i < arr.length; i++) { // i from 1 to 100
			while (arr[i]-- > 0) { 
				if (heights[j++] != i)
					count++;
			}
		}
		
		return count;
	}
}
/**
 * heights = {1,1,4,2,1,3,8};
 * i = 1, j = 0: arr[1]=3>0, height[0]==i; arr[1]=2>0,height[1]==i; arr[1]=1,heights[2]=4!=1, count++
 * i = 2, j = 3: arr[2]=1>0, height[3]==2;
 * i = 3, j = 4: arr[3]=1>0, height[4]=1!=i, count++;
 * i = 4, j = 5: arr[4]=1>0, height[5]=3!=i, count++;
 * i = 5, j = 6: arr[5]==0
 * i = 6, j = 6: arr[6]==0
 * i = 7, j = 6: arr[7]==0
 * i = 8, j = 6: arr[8]=1>0, height[6]=8==i
 * i > 8, arr[i] == 0
 * count = 3; return count;
 */

```
