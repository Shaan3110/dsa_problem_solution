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

### Merge Two Sorted Linked Lists

<img width="662" height="302" alt="image" src="https://github.com/user-attachments/assets/cb7894fa-fe40-4cf8-affc-4a26f0eb5300" />

**Solution** -

```
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {

        ListNode dummy = new ListNode(-1);
        ListNode current = dummy;

        while(list1 != null && list2 != null)
        {
            if(list1.val < list2.val)
            {
                current.next = list1;
                list1 = list1.next;
            }
            else
            {
                current.next = list2;
                list2 = list2.next;
            }

            current = current.next;
        }

        if(list1 != null)
            current.next = list1;

        if(list2 != null)
            current.next = list2;

        return dummy.next;
    }
}
```

### Reverse a Linked List

**Solution** - Just maintain 3 nodes at a time prev, current and next. current -> next = prev and current = next

```
class Solution {
    public ListNode reverseList(ListNode head) {

        ListNode prev = null;
        ListNode current = head;

        while(current != null)
        {
            ListNode next = current.next;

            current.next = prev;

            prev = current;
            current = next;
        }

        return prev;
    }
}
```

### BFS Level Traversal Tree ( Binary Tree )

Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

<img width="277" height="302" alt="image" src="https://github.com/user-attachments/assets/6d9f8c13-8497-449d-8879-4aa17177048e" />

Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]

**Solution** - Simple approach to traverse a binary tree from top to bottom based on level we need to maintain two data structures one is a queue ( FIFO ) and another is a array list to main that level elements. 

```
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {

        List<List<Integer>> result = new ArrayList<>();

        if(root == null)
            return result;

        Queue<TreeNode> queue = new LinkedList<>();

        queue.offer(root);

        while(!queue.isEmpty())
        {
            int size = queue.size();

            List<Integer> level = new ArrayList<>();

            for(int i=0;i<size;i++)
            {
                TreeNode node = queue.poll();

                level.add(node.val);

                if(node.left != null)
                    queue.offer(node.left);

                if(node.right != null)
                    queue.offer(node.right);
            }

            result.add(level);
        }

        return result;
    }
}
```

### Traverals in Trees

```
class Solution {

    public List<Integer> inorderTraversal(TreeNode root) {

        List<Integer> result = new ArrayList<>();

        inorder(root, result);

        return result;
    }

     public void preorder(TreeNode node, List<Integer> result) {

        if(node == null)
            return;

        result.add(node.val);

        preorder(node.left, result);

        preorder(node.right, result);
    }

    public void inorder(TreeNode node, List<Integer> result) {

        if(node == null)
            return;

        inorder(node.left, result);

        result.add(node.val);

        inorder(node.right, result);
    }

    public void postorder(TreeNode node, List<Integer> result) {

        if(node == null)
            return;

        postorder(node.left, result);

        postorder(node.right, result);

        result.add(node.val);
    }
}
```

### Move Zeroes at end of Array

**Solution** - Simply traverse through the array and on the loop find all the non-zero elements and append it on the zero indexes. And once done just paste remaining size of the array with 0.

```
class Solution {
    public void moveZeroes(int[] nums) {

        int index = 0;

        for(int i=0;i<nums.length;i++)
        {
            if(nums[i] != 0)
            {
                nums[index++] = nums[i];
            }
        }

        while(index < nums.length)
        {
            nums[index++] = 0;
        }
    }
}
```

### Anagram String 

Given two strings s and t, return true if t is an anagram of s, and false otherwise.

 
Example 1:

Input: s = "anagram", t = "nagaram"

Output: true

Example 2:

Input: s = "rat", t = "car"

Output: false

**Solution** - Two Strings are anagram if the characters of the both arrays are same just they are rearranged.

```
class Solution {
    public boolean isAnagram(String s, String t) {

        if(s.length() != t.length())
            return false;

        int[] count = new int[26];

        for(char c : s.toCharArray())
            count[c-'a']++;

        for(char c : t.toCharArray())
        {
            count[c-'a']--;

            if(count[c-'a'] < 0)
                return false;
        }

        return true;
    }
}
```

### Minimum Length of Anagram Concatenation

You are given a string s, which is known to be a concatenation of anagrams of some string t.

Return the minimum possible length of the string t.

An anagram is formed by rearranging the letters of a string. For example, "aab", "aba", and, "baa" are anagrams of "aab".

 

Example 1:

Input: s = "abba"

Output: 2

Explanation:

One possible string t could be "ba".

Example 2:

Input: s = "cdef"

Output: 4

Explanation:

One possible string t could be "cdef", notice that t can be equal to s.

Example 2:

Input: s = "abcbcacabbaccba"

Output: 3

Link: https://leetcode.com/problems/minimum-length-of-anagram-concatenation/solutions/5113882/100-correct-solution-without-gcd-by-adit-9q6h/

GCD will not work if the string is abba then it can be 2 as there are two unique characters. But for aabbbaab then it can't be 2 as aa will be t and it can't be a anagram of ab. So it should be 4 aabb and abba are really anagram.


```
class Solution {
    bool ok(string s, int k) {
        int n = s.length();
        int cnt[26] = {0};
        for (int i = 0; i < k; i++)
            cnt[s[i] - 'a']++;
        for (int i = k; i < n; i += k) {
            int cnt2[26] = {0};
            for (int j = i; j < i + k; j++)
                cnt2[s[j] - 'a']++;
            for (int j = 0; j < 26; j++)
                if (cnt[j] != cnt2[j])
                    return false;
        }
        return true;
    }

public:
    int minAnagramLength(string s) {
        int n = s.size();
        for (int i = 1; i <= n; i++)
            if (n % i == 0 && ok(s, i))
                return i;
        return n;
    }
};

```

### Longest Substring Without Repeating Characters

**Solution** - Simply maintain a hash set so no duplicates are present now let's say you get a element duplicate. Maintain two indices which is left and right , now on right keep adding elements till a duplicate appeared and as soon a duplicate came, keep incrementing the left till the duplicate is not present . In this way keep a count of max, and the difference of right -left is the size of max.

```
public static boolean isAnagram(String s, String t) {

        if(s.length() != t.length())
            return false;

        int[] count = new int[26];

        for(char c : s.toCharArray())
            count[c-'a']++;

        for(char c : t.toCharArray())
        {
            count[c-'a']--;

            if(count[c-'a'] < 0)
                return false;
        }

        return true;
    }
```
