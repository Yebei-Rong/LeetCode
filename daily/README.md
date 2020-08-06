#### 每日记录(按tag和难易程度顺序）

刷题顺序为：数组-> 链表-> 哈希表->字符串->栈与队列->树。

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
 * heights = {1,1,4,2,1,3,8};hu-zi-lcof/

这道题的关键是数组本身是有序的，目的是查找长度为n-1的该递增排序数组中缺失的数。“排序数组中的搜索问题，首先想到**二分法**解决。“（参考leetcode中这一题的精选思路）

``` Java
	public int missingNumber(int[] nums) {
		/**
		 * return the missing value in the input array nums.
		 * input nums: an ordered integer array. length = n - 1 and 0 <= nums[i] <= n-1
		 */
		
		int i = 0, j = nums.length - 1;
		
		while (i <= j) {
			int m = (i + j)/2;
			if (nums[m] != m) j = m - 1;
			else i = m + 1;
		}
		
		return i;
	}	
```
时间复杂度：O(logN)
空间复杂度：O(1)

* 剑指offer 53 I 在排序数组中查找数字出现的次数

https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/

示例 1:

输入: nums = [5,7,7,8,8,10], target = 8， 输出: 2

示例 2:

输入: nums = [5,7,7,8,8,10], target = 6， 输出: 0

1. 思路1: 遍历整个数组，有就+1，一直到最后都没有就返回0

2. 思路2: 通过两次二分法找左、右边界，最终返回right-left-1即为出现次数。

``` Java
    public int search(int[] nums, int target) {
		/*
		 * return the number of the target in the input array
		 * 
		 * input nums: an ordered integer array, 0<=nums.length <=50000
		 * input target: an input number
		 */

		int left = 0, right = nums.length - 1;
		int i = 0, m = 0, j = nums.length - 1;
		
		// Step 1: find the right border using 
		// nums = {5,7,7,8,8,8,9} right = 6
		while (i<=j) {
			m = (i + j)/2;
			
			if (nums[m] <= target)  i = m + 1;
			else j = m - 1;
		}
		right = i;
		
		// Step 2: chech if target is in the input array
		if (j > 0 && nums[j] != target) return 0;
		
		// Step 3: find the left border
		i = 0; 
		while (i<=j) {
			m = (i+j)/2;
			
			if (nums[m]<target) i = m + 1;
			else j = m - 1;
		}
		left = j;
		
		return (right - left - 1);
	}  
```

* 剑指offer 29. 顺时针打印矩阵

https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/

示例 1：

输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]

参考精选答案，初始化了left right top bottom四个变量，以及res数组存储结果，分四个循环（代表四个方向）去顺时针打印input矩阵。

``` Java
    public int[] spiralOrder(int[][] matrix) {
		/*
		 * return a 1D array of the input 2Dmatrix by clock wise order
		 * 
		 * input matrix: 2D integer matrxi
		 */
		if (matrix.length == 0) return new int[0];
		
		// initialize 
		int l = 0, r = matrix[0].length - 1, t = 0, b = matrix.length -1;
		int[] res = new int[(r+1)*(b+1)];
		int count = 0;
		
		while(true) {
			// Left to right, t++ until t > b
			for (int i = l; i<=r; i++) res[count++] = matrix[t][i];
			if (++t > b) break;
			
			// Top to bottom, r-- until r < l
			for (int i = t; i<=b; i++) res[count++] = matrix[i][r];
			if (--r < l) break;
			
			// Right to left, b-- until t > b
			for (int i = r; i>=l; i--) res[count++] = matrix[b][i];
			if (--b < t) break;
			
			// Bottom to top, l++ until l > r
			for (int i = b; i>=t; i--) res[count++] = matrix[i][l];
			if (++l > r) break;
		}
		
		return res;
    }
```

* 剑指 Offer 04. 二维数组中的查找

https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/

因输入数组在行、列上都是有序的（递增），所以从右上角开始，查找target数。

``` Java
    public static boolean findNumberIn2DArray(int[][] matrix, int target) {
        /*
        * return if target exists in the input matrix
        * input matrix: a 2D n*m integer matrix (increasing in row and column), 0 <= n <= 1000, 0 <= m <= 1000
        * input matrix: an integer
        */

        // Condition 1: matrix is null or matrix is empty
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0){
            return false;
        }

        // Condition 2:
        int rows = matrix.length;
        int columns = matrix[0].length;
        int row = 0;
        int column = columns - 1; // start searching from the ipper-right point

        while (row < rows && column >= 0){
            int num = matrix[row][column]; // store the number in variable num becasue we need to use it later

            if (target == num) {
                return true;
            }else if(target < num) {
                column--;
            }else {
                row++;
            }
        }

        return false;
    }
```

* 剑指offer 03. 数组中重复的数字

https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/

本题旨在找到一个在输入数组nums中重复出现过的数字，是任意的，所以只要找到返回即可。首先需要考虑特殊情况，nums 为null/长度小于等于1时，此时返回-1（无重复元素）。

1. 方法一：哈希集合

一般情况下，解决这个问题我们可以创建一个HashSet对象，向里面添加元素，add方法会自动判断里面的元素是否有相同，从而保证元素的唯一。

``` Java
	public static int findRepeatNumber(int[] nums) {
		/*
		 * return one integer in nums if it is repeated
		 * 
		 * input nums: a n length 1D array, 0 <= nums[i] <= n-1, 2 <= nums.length <= 100000
		 */
		if (nums == null || nums.length <= 1) return -1; // repeated number not found
		
		Set<Integer> set = new HashSet<Integer>();
		
		for (int num: nums) {
			if (! set.add(num)) return num;
		}
		
		return -1; // repeated number not found
	}

```
时间复杂度：O（n），空间复杂度：O（n）最坏的情况是没有重复的元素，所有元素都会被存入哈希集合中。

2. 方法二：原地置换

注意这道数组题依旧是长度为n的数组，数组元素在0～n-1范围内，意味着如果无重复元素。所以思路为遍历nums数组，下标为i，下标指向的数记为m，如果m！=i并且则将该数和nums[m]交换（m应该出指向nums[m]）,如果m==nums[m]则表示又重复数字发生，终止循环返回m。

``` Java
	public static int findRepeatNumber2(int[] nums) {
		if (nums == null || nums.length <= 1) return -1; // repeated number not found
		
		
		for (int i = 0; i < nums.length; i++) {
			int m = nums[i];
			if (m != i) {
				if (nums[i] == nums[nums[i]]) return m;
				else {
					nums[i] = nums[nums[i]];
					nums[nums[i]] = m;
				}
				
			}
		}
		return -1;
	}
```

* 53. 最大子序和

https://leetcode-cn.com/problems/maximum-subarray/

方法一：贪心算法。当前和小于0的时候，就舍弃当前和代表的子序列，将sum变量重置为0。

``` Java
    public int maxSubArray(int[] nums) {
        if (nums.length == 1) return nums[0];

        int sum = 0;
        int max = nums[0];

        for (int i = 0; i < nums.length; i++){
            sum += nums[i];

            if (sum > max){
                max = sum;
            }
            if (sum < 0){
                sum = 0;
            }
        }
	
        return max;
    }
```

时间复杂度： O(n)，只遍历了一次数组；空间复杂度：O(1)，只使用了常数空间

方法二：分治法

``` Java
	public static int maxSum(int[] sequence, int left, int right) {
		if (left == right) {
			if (sequence[left] > 0)
				return sequence[left];
			else
				return 0;
		}
		
		int mid = (left + right) / 2;
		int maxLeftSum = maxSum(sequence, left, mid);
		int maxrightSum = maxSum(sequence, mid+1, right);
		
		int leftBorderSum = 0, maxLeftBorderSum = 0;
		for (int i = mid; i >= left; i--) {
			leftBorderSum += sequence[i];
			if (leftBorderSum > maxLeftBorderSum)
				maxLeftBorderSum = leftBorderSum;
			
		}
		
		int rightBorderSum = 0, maxRightBorderSum = 0;
		for (int i = mid+1; i <= right; i++) {
			rightBorderSum += sequence[i];
			if (rightBorderSum > maxRightBorderSum)
				maxRightBorderSum = rightBorderSum;
		}
		
		return max3(maxLeftSum, maxRightBorderSum, maxLeftBorderSum + maxRightBorderSum);
	}
	
	public static int max3(int a, int b, int c) {
		int max = (a>b)? a:b;
		max = (max>c)? max:c;
		
		return max;
```

* 剑指 Offer 58 - I. 翻转单词顺序

1. 方法一：使用空格分割字符串

``` Java
    public String reverseWords(String s) {
        /*
        * input: "  the sky is blue!"
        * output: "blue! is sky the"
        */

        String res = "";
        String[] split = s.trim().split("\\s+");

        int length = split.length;

        if (length <= 1) return s.trim();

        for (int i = length-1; i >= 0; i--){
            if (i==0){
                res += split[0];
            }else{
                res += (split[i] + " ");
            }
        }

        return res;
    }
```

时间复杂度：O(n), 遍历一遍split过后的字符串数组；空间复杂度：O(n), 创建空间给result字符串和字符串数组

注意：由于trim，split为内置函数，面试时不建议使用该方法。

2. 方法二：双指针

首先，将字符串首末的空格删去；接着，初始化新的字符串""res存储结果，以及i，j两个指针，指向最后一个字符。

其次，（1）从字符串末尾开始，j指向单词的最后一个字符，对i进行i--操作直到找到一个空格；（2）取s[i+1, j+1] + “ ” 加到res中；（3）继续i--跳过相邻单词中的空格；（4）将i赋给j，此时j指向下一个单词的最后一个字符。

最后返回res.trim()

``` Java
    public String reverseWords(String s) {
        /*
        * input: "  the sky is blue!"
        * output: "blue! is sky the"
        */
		s = s.trim();

		String res = "";
		int j = s.length()-1;
		int i = j;
			
		while (i >= 0) {
			// Step 1: find the first blank space
			while(i >= 0 && s.charAt(i) != ' ') 
				i--;
			// Step 2: append the substring s[i+1, j+1] to the result string also ' ' 
			res += (s.substring(i+1, j+1) + ' ');
			// Step 3: skip blank spaces between 2 words
			while (i >= 0 && s.charAt(i) == ' ')
				i--;
			// Step 4: let j = i, so j points to the end of the next word
			j = i;
				
		}
			
		return res.trim();

    }
```

时间复杂度：O(n), 遍历了字符串；空间复杂度：O(n)，新建了res字符串存储结果。

* 剑指 Offer 58 - II. 左旋转字符串

本题思路很简单，主要运用了java字符串的内置方法substring，直接附上代码：

``` Java
    public String reverseLeftWords(String s, int n) {
        assert n < s.length() && n >= 1: "Input n is wrong. Please check and input again.";

        String res = "";

        res = s.substring(n, s.length()) + s.substring(0,n);
        
        return res;

    }
```

* 剑指 Offer 67. 把字符串转换成整数

https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/

![offere67](https://github.com/Yebei-Rong/LeetCode/blob/master/image/offere67.jpg?raw=true)

``` Java
    public int strToInt(String str) {
        char[] characters = str.trim().toCharArray();
        if (characters.length == 0) return 0;

        int res = 0;
        int bndry = Integer.MAX_VALUE / 10;
        int i = 1;
        int sign = 1; // 1: positive, -1: negative
    
        if (characters[0] == '-') sign = -1;
        else if (characters[0] != '+') i = 0;
        
        for (int j = i; j < characters.length; j++) {
        	if(characters[j] > '9' || characters[j] < '0') break;
        	if (res > bndry || res == bndry && characters[j] > '7') return sign == 1 ? Integer.MAX_VALUE: Integer.MIN_VALUE;
        	
        	res = res * 10 + (characters[j] - '0');     	
        }
  
        return sign * res;        
    }
```
