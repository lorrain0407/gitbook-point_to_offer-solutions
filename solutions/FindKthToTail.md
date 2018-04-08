## 1.4 链表中倒数第k个结点

输入一个链表，输出该链表中倒数第k个结点。

<br>

怎么说呢，我能想到的方法感觉特别low，遍历一遍得到总数sum，然后从前到后找第sum - k + 1个

注意当k > sum时，返回的是NULL，这个不等价于返回ListNode* r = new ListNode(0);

而且这个题最后返回的也不是单纯的一个结点啊。

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
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        ListNode* h = pListHead;
        int sum = 0;
        while(h != NULL){
            sum++;
            h = h -> next;
        }
        if(k > sum){
            return NULL;
        }
        int n = sum - k;
        h = pListHead;
        while(n--){
            h = h -> next;
        }
        return h;
    }
};
```

<br>

果然评论区有大神直接，膜拜一下~

两个指针，先让第一个指针和第二个指针都指向头结点，然后再让第一个指针走(k-1)步，到达第k个节点。然后两个指针同时往后移动，当第一个结点到达末尾的时候，第二个结点所在位置就是倒数第k个节点了。。看时间复杂度的话好像没什么差，所以。。

```cpp
class Solution {
public:
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        if(pListHead==NULL||k==0)
            return NULL;
        ListNode*pTail=pListHead,*pHead=pListHead;
        for(int i=1;i<k;++i)
        {
            if(pHead->next!=NULL)
                pHead=pHead->next;
            else
                return NULL;
        }
        while(pHead->next!=NULL)
        {
            pHead=pHead->next;
            pTail=pTail->next;
        }
        return pTail;
    }
};
```

还有就是用vector、stack、递归的~感觉没必要啊~