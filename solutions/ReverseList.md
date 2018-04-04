## 1.2 反转链表

输入一个链表，反转链表后，输出链表的所有元素。

<br>

怎么说呢，上个题，从尾到头输出链表，其实也可以通过反转后输出呢。我的解法如下：

```cpp
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
          val(x), next(NULL) {
    }
};

class Solution {
public:
    ListNode* ReverseList(ListNode* pHead) {
        if(pHead == NULL || pHead -> next == NULL){
            return pHead;
        }
        
        ListNode* now = pHead -> next;
        ListNode* pre = pHead;
        
        while(now){
            ListNode* temp = now -> next;
            now -> next = pre;
            pre = now;
            now = temp;
        }
        pHead -> next = NULL;
        
        return pre;
    }
};
```

<br>

上讨论区瞅瞅大家的解法。。。好像都大同小异，那就这样吧~