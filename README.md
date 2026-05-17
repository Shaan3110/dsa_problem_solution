# **DSA Problem Solution**

**Add Two Numbers**

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

<img width="483" height="342" alt="image" src="https://github.com/user-attachments/assets/a4861941-99b1-4486-a368-4d892367c61e" />

**Solution** -

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */


class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {

        ListNode head = l1;
        ListNode prev = null;

        int carry = 0;

        while(l1 != null || l2 != null)
        {
            int x = (l1 != null) ? l1.val : 0;
            int y = (l2 != null) ? l2.val : 0;

            int sum = x + y + carry;

            carry = sum / 10;
            int digit = sum % 10;

            if(l1 != null)
            {
                l1.val = digit;
                prev = l1;
                l1 = l1.next;
            }
            else
            {
                prev.next = new ListNode(digit);
                prev = prev.next;
            }

            if(l2 != null)
            {
                l2 = l2.next;
            }
        }

        // if carry is left just put it on next node 
        if(carry > 0)
        {
            prev.next = new ListNode(carry);
        }

        return head;
    }
}

```
### Two Sum

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

Example 1:

Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0,1].

**Solution** -

On the solution just keep a track if the element already appeared previously so we would add the elements visited on a map with element, index. In this way we can search if that element appeared in that case just get the value to that key.


```
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map <Integer,Integer> map = new HashMap <Integer,Integer>();
        int difference = 0;
        for(int i=0;i<nums.length;i++)
        {
            difference = target-nums[i];

            if(map.containsKey(difference))
            {
                return new int[] {map.get(difference),i};
            }
            map.put(nums[i],i);
        }
        return new int[] {0,0};
    }
}
```

### Maximum Subarray ( Kadane’s Algorithm )

Given an integer array nums, find the subarray with the largest sum, and return its sum.

 

Example 1:

Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: The subarray [4,-1,2,1] has the largest sum 6.

**Solution** -

Let's say the array is this above one, now 3 imp steps to understand the below code. I don't want to keep my sum if it's less than 0 . I will just ignore it and reset to 0.

Now if sum = sum + arr[i]
maxi = Max (maxi,sum )
if sum < 0 : sum= 0


```
class Solution {
    public int maxSubArray(int[] nums) {

        int max = nums[0];
        int current = nums[0];

        for(int i=1;i<nums.length;i++)
        {
            current = Math.max(nums[i], current + nums[i]);
            max = Math.max(max, current);
        }

        return max;
    }
}
```

### Best Time to Buy and Sell Stock

You are given an array prices where prices[i] is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

 

Example 1:

Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
Example 2:

Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.

**Solution** -

Simple for each element i come across the min value before that is imp , the difference is the max profit that i can earn on that day

```
class Solution {
    public int maxProfit(int[] prices) {
        int min = Integer.MAX_VALUE;
        int profit = 0;
        for(int i=0;i<prices.length;i++)
        {
            min = Math.min(prices[i],min);
            profit = Math.max(prices[i]-min,profit);
        }

        return profit;
        
    }
}
```

### Valid Parentheses

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.

**Solution** -

Simple just maintain a stack and let's say i get '(' I can push ')' on the stack if any of the insertion parentheses are not the element then just check if the current pop matches it.

```
class Solution {
    public boolean isValid(String s) {

        Stack<Character> stack = new Stack<>();

        for(char c : s.toCharArray())
        {
            if(c == '(') stack.push(')');
            else if(c == '{') stack.push('}');
            else if(c == '[') stack.push(']');
            else
            {
                if(stack.isEmpty() || stack.pop() != c)
                    return false;
            }
        }

        return stack.isEmpty();
    }
}
```
