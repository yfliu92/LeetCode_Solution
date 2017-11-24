# House Robber
***
**1.**

> You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.
> 
> Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.


Solution(1-1):

* dp[i][0]: do not rob the i-th house 
	
	dp[i][0] = max(dp[i-1][1], dp[i-1][0])

* dp[i][1]: rob the i-th house

	dp[i][1] = dp[i-1][0] + val

```
 class Solution {
    public int rob(int[] nums) {
        if(nums==null || nums.length==0) return 0;
        int[][] dp = new int[nums.length][2];
        dp[0][0] = 0;
        dp[0][1] = nums[0];
        for(int i = 1; i < nums.length; i++){
            dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1]);
            dp[i][1] = dp[i-1][0] + nums[i];
        }
        return Math.max(dp[nums.length-1][0], dp[nums.length-1][1]);
            
    }
}
```

Solution(1-2):

```
class Solution {
    public int rob(int[] nums) {
        int rob = 0;
        int notRob = 0;
        for(int i = 0; i < nums.length; i++){
            int temp = rob;
            rob = notRob + nums[i];
            notRob = Math.max(temp, notRob);
        }
        return Math.max(notRob, rob);
    }
}
```

***

**2. Circle** 

> After robbing those houses on that street, the thief has found himself a new place for his thievery so that he will not get too much attention. This time, all houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, the security system for these houses remain the same as for those in the previous street.
> 
> Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

Solution:

* not rob house 1:

	rob from house 2 to house n
	 
* not rob house n:

	rob from house 1 to house n-1	
	



```
class Solution {
    public int rob(int[] nums) {
        if(nums==null || nums.length==0) return 0;
        int len = nums.length;
        if(len ==1) return nums[0];
        
        int sum = 0;
        int rob = 0;
        int notRob = 0;
        // not rob house 0
        for(int i = 1; i < len; i++){
            int temp = rob;
            rob = notRob + nums[i];
            notRob = Math.max(temp, notRob);
        }
        sum = Math.max(rob, notRob);
        rob = 0;
        notRob = 0;
        // not rob house n-1
        for(int i = 0 ; i < len-1; i++){
            int temp = rob;
            rob = notRob + nums[i];
            notRob = Math.max(temp, notRob);
        }
        return Math.max(Math.max(rob, notRob), sum);
        
    }
}

```

***

**3. A binary tree**

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.

	
	Example 1:
	     3
	    / \
	   2   3
	    \   \ 
	     3   1
	     
Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.

	Example 2:
	     3
	    / \
	   4   5
	  / \   \ 
	 1   3   1
	 
Maximum amount of money the thief can rob = 4 + 5 = 9.

Solution:

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int rob(TreeNode root) {
        if(root == null) return 0;
        int[] result = helper(root);
        return Math.max(result[0], result[1]);
    }
    // result[]
    // index 0: the value of not rob the current house
    // index 1: the value of rob the current house
    
    public int[] helper(TreeNode node){
        if(node == null) return new int[2];
        int[] res = new int[2];
        int[] left = helper(node.left);
        int[] right = helper(node.right);
        res[0] = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
        res[1] = node.val + left[0] + right[0];
        return res;
    }
}

```

