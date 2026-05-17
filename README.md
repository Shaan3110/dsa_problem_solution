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


