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

Python -

```
class Solution:
    def twoSum(self, nums, target):
        mp = {}

        for i in range(len(nums)):
            diff = target - nums[i]

            if diff in mp:
                return [mp[diff], i]

            mp[nums[i]] = i
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

### Sparsh Array

<img width="588" height="594" alt="Screenshot 2026-05-22 at 12 00 04 AM" src="https://github.com/user-attachments/assets/7663d50a-2977-461e-9524-aa5e237a953f" />

**Solution** - Simply use a map to keep track of the occurance of the element . Next time just refer to it for the occurance of the element on the original array.

```
public static List<Integer> matchingStrings(List<String> stringList, List<String> queries) {
    // Write your code here
        Map <String,Integer> map = new HashMap<>();
        List<Integer> result = new ArrayList<>();
        for(int i=0;i<stringList.size();i++)
        {
            int occurance = map.getOrDefault(stringList.get(i), 0);
            map.put(stringList.get(i),occurance+1);
        }
        
        for(int j=0;j<queries.size();j++)
        {
            result.add(map.getOrDefault(queries.get(j), 0));
        }
        
        return result;
    }
```

Python Code -

```
def matchingStrings(stringList, queries):

    freq = {}
    result = []

    # Count occurrences
    for s in stringList:

        occurrence = freq.get(s, 0)

        freq[s] = occurrence + 1

    # Answer queries
    for q in queries:

        result.append(freq.get(q, 0))

    return result
```

### Sort a Array List Syntax

```
List<Integer> sorted = new ArrayList<>(arr);

Collections.sort(sorted);
```
Python -

```
sorted_arr = sorted(arr)
```

## Clone and sort a Array

```
import java.util.Arrays;

public class Main {

    public static void main(String[] args) {

        int[] arr = {5, 2, 8, 1, 3};

        // Clone original array
        int[] sorted = arr.clone();

        // Sort cloned array
        Arrays.sort(sorted);

        System.out.println("Original Array:");
        System.out.println(Arrays.toString(arr));

        System.out.println("Sorted Cloned Array:");
        System.out.println(Arrays.toString(sorted));
    }
}
```

### Grading Students

HackerLand University has the following grading policy:

Every student receives a  grade in the inclusive range from  to .
Any grade less than  is a failing grade.
Sam is a professor at the university and likes to round each student's  according to these rules:

If the difference between the  and the next multiple of  is less than , round  up to the next multiple of .
If the value of grade is less than , no rounding occurs as the result will still be a failing grade.
Examples

 grade is 85 round to  (85 - 84 is less than 3)
 grade is 37 do not round (result is less than 38)
 grade is 60 do not round (60 - 57 is 3 or higher)

**Solution** -

```
def gradingStudents(grades):

    for i in range(len(grades)):

        # Ignore failing grades
        if grades[i] < 38:
            continue

        remainder = grades[i] % 5

        # Difference to next multiple of 5
        diff = 5 - remainder

        if diff < 3:
            grades[i] += diff

    return grades
```

### Longest Substring Without Repeating Characters


The idea is simple using window approach we keep adding elements on our map as ele, index. Now let's say I come across a character in the string which has a occurance so directly jump to that next index and avoid it. And after each iteration of window increase just keep doing left = Math.max(maxlength, right-left+1) 

**Solution** -

```
class Solution {

    public int lengthOfLongestSubstring(String s) {

        Map<Character, Integer> map = new HashMap<>();

        int left = 0;
        int maxLength = 0;

        for(int right = 0; right < s.length(); right++) {

            char ch = s.charAt(right);

            if(map.containsKey(ch)) {

                left = Math.max(left, map.get(ch) + 1);
            }

            map.put(ch, right);

            maxLength = Math.max(maxLength, right - left + 1);
        }

        return maxLength;
    }
}
```

Python-

```
class Solution:

    def lengthOfLongestSubstring(self, s: str) -> int:

        mp = {}

        left = 0
        max_length = 0

        for right in range(len(s)):

            ch = s[right]

            if ch in mp:

                left = max(left, mp[ch] + 1)

            mp[ch] = right

            max_length = max(max_length, right - left + 1)

        return max_length
```

### Longest Palindrome Substring

This problem can only be solved in O(n^2) complexity itself where you would expand for every index like for 0,0 then 0,1 so when it reaches center it keeps expanding each time.


**Solution** -

```
class Solution {

    int maxLength = 0;
    String answer = "";

    public String longestPalindrome(String s) {

        if(s == null || s.length() < 2)
            return s;

        for(int i = 0; i < s.length(); i++) {

            expand(s, i, i);

            expand(s, i, i + 1);
        }

        return answer;
    }

    public void expand(String s, int left, int right) {

        while(left >= 0 &&
              right < s.length() &&
              s.charAt(left) == s.charAt(right)) {

            int currentLength = right - left + 1;

            if(currentLength > maxLength) {

                maxLength = currentLength;

                answer = s.substring(left, right + 1);
            }

            left--;
            right++;
        }
    }
}
```

### Meeting Rooms II

The task provides an array of meeting time intervals, each with a start and end time. The goal is to find the smallest number of meeting rooms needed so that no two meetings in the same room overlap in time. For example, given intervals 
[[0,30],[5,10],[15,20]]
[[0,30],[5,10],[15,20]], the answer is 2.

**Solution** - To solve this in minimum time complexity we just to too worry about sorting this given array based on interval gaps ( end_time - start_time ) and `current meeting start >= earliest ending meeting` . So we will make a priority heap map store the end time of the meeting and in case of currentStart >= earliestEnd we poll and remove the top. Otherwise push the new earliestEnd as well to the queue. In this way size of our would be the number of meeting rooms. Complexity would be O(nlogn).

```
class Solution {

    public int minMeetingRooms(int[][] intervals) {

        if(intervals.length == 0)
            return 0;

        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);

        PriorityQueue<Integer> minHeap = new PriorityQueue<>();

        minHeap.offer(intervals[0][1]);

        for(int i = 1; i < intervals.length; i++) {

            int currentStart = intervals[i][0];

            int earliestEnd = minHeap.peek();

            if(currentStart >= earliestEnd) {

                minHeap.poll();
            }

            minHeap.offer(intervals[i][1]);
        }

        return minHeap.size();
    }
}
```

PRIORITY QUEUE SOLVES THIS

Priority Queue (Min Heap):

always gives smallest element first

Example:

Insert: `30, 10, 20`

Heap internally organizes as: `10 on top`

So: `minHeap.peek()`

instantly gives: `10`

Exactly what we need.

Python -

```
import heapq

class Solution:

    def minMeetingRooms(self, intervals):

        if not intervals:
            return 0

        # Sort by start time
        intervals.sort(key=lambda x: x[0])

        # Min heap to store ending times
        min_heap = []

        # Add first meeting end time
        heapq.heappush(min_heap, intervals[0][1])

        # Process remaining meetings
        for i in range(1, len(intervals)):

            current_start = intervals[i][0]

            earliest_end = min_heap[0]

            # Reuse room if meeting ended
            if current_start >= earliest_end:

                heapq.heappop(min_heap)

            # Add current meeting end
            heapq.heappush(min_heap, intervals[i][1])

        return len(min_heap)
```

### Product of Max(A) and Min(B)

You just have to find maximum element of A and minimum of B and give it's product. Just handle the negative elements properly on this problem statement.

```
class Solution {

    public int productOfMaxAAndMinB(int[] A, int[] B) {

        int maxA = Integer.MIN_VALUE;

        int minB = Integer.MAX_VALUE;

        // Find maximum in A
        for(int num : A) {

            if(num > maxA) {

                maxA = num;
            }
        }

        // Find minimum in B
        for(int num : B) {

            if(num < minB) {

                minB = num;
            }
        }

        return maxA * minB;
    }
}
```

Python -

```
class Solution:

    def product_of_maxA_minB(self, A, B):

        maxA = float('-inf')

        minB = float('inf')

        # Find maximum in A
        for num in A:

            if num > maxA:

                maxA = num

        # Find minimum in B
        for num in B:

            if num < minB:

                minB = num

        return maxA * minB
```

### Length of Longest Fibonacci Subsequence

Given a strictly increasing array arr, the goal is to return the length of the longest subsequence satisfying arr[i] + arr[j] = arr[k] for consecutive triplets in the subsequence.
If no such subsequence of length ≥ 3 exists, return 0.

Example:

Input:  arr = [1, 2, 3, 4, 5, 6, 7, 8]
Output: 5
Explanation: [1, 2, 3, 5, 8] is the longest Fibonacci-like subsequence.
Input:  arr = [1, 3, 7, 11, 12, 14, 18]
Output: 3
Explanation: Possible subsequences: [1,11,12], [3,11,14], [7,11,18].

**Solution** - Simply try to maintain a hash set first, so there are no duplicates on it. Now add every number on that hashset . While traversing now, do pick first element and second element and keep checking if the addition of them are available on the set. If so then continue with a=b and b=c and continue. Now the catch is rather than doing it in O^3 we do it on O^2 with picking all 2 elements with i=0 and j always as i+1.

```
class Solution {

    public int lenLongestFibSubseq(int[] arr) {

        Set<Integer> set = new HashSet<>();

        for(int num : arr) {

            set.add(num);
        }

        int maxLength = 0;

        int n = arr.length;

        for(int i = 0; i < n; i++) {

            for(int j = i + 1; j < n; j++) {

                int a = arr[i];

                int b = arr[j];

                int length = 2;

                while(set.contains(a + b)) {

                    int c = a + b;

                    a = b;

                    b = c;

                    length++;
                }

                maxLength = Math.max(maxLength, length);
            }
        }

        return maxLength >= 3 ? maxLength : 0;
    }
}
```

Python -

```
class Solution:

    def lenLongestFibSubseq(self, arr):

        nums = set(arr)

        max_length = 0

        n = len(arr)

        for i in range(n):

            for j in range(i + 1, n):

                a = arr[i]
                b = arr[j]

                length = 2

                while a + b in nums:

                    c = a + b

                    a = b
                    b = c

                    length += 1

                max_length = max(max_length, length)

        return max_length if max_length >= 3 else 0
```

### Longest Substring Without Repeating Characters

Given a string s, find the length of the longest substring without duplicate characters.


Example 1:

Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3. Note that "bca" and "cab" are also correct answers.
Example 2:

Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
Example 3:

Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
 

Constraints:

0 <= s.length <= 5 * 104
s consists of English letters, digits, symbols and spaces.

**Solution** -

The main key here is to just maintain a set of characters that we already iterated and a window. Let's say a string like wewwbd comes then we just have to make sure that we remove all duplicates , it can be at start or in-between. So, just use a while statement for that.

```
class Solution {

    public int lengthOfLongestSubstring(String s) {

        Set<Character> set = new HashSet<>();

        int left = 0;

        int maxLength = 0;

        for(int right = 0; right < s.length(); right++) {

            while(set.contains(s.charAt(right))) {

                set.remove(s.charAt(left));

                left++;
            }

            set.add(s.charAt(right));

            maxLength = Math.max(maxLength,
                                 right - left + 1);
        }

        return maxLength;
    }
}
```

Python -

```
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        left = max_length = 0
        char_set = set()
        
        for right in range(len(s)):
            while s[right] in char_set:
                char_set.remove(s[left])
                left += 1

            char_set.add(s[right])
            max_length = max(max_length, right - left + 1)
        
        return max_length
```

### 
