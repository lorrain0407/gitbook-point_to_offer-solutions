## 1.3 合并两个排序的链表

输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

<br>

这应该是学数据结构链表时候最最基础的题了，做的很顺。但唯一要注意的是

```cpp
ListNode* head = new ListNode(0);
return head -> next;
```
否则会出错。

个人感觉前面的判断可有可无，在一个链表遍历完之后使用if直接指向剩余的或者用while一个一个填加都可以，但是用if感觉会好一些。解法如下：

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
    ListNode* Merge(ListNode* pHead1, ListNode* pHead2)
    {
        /*if(pHead1 == NULL && pHead2 == NULL){
            return NULL;
        }
        if(pHead1 == NULL){
            return pHead2;
        }
        if(pHead2 == NULL){
            return pHead1;
        }*/
        ListNode* head = new ListNode(0);
        ListNode* h = head;
        while(pHead1 && pHead2){
            if(pHead1 -> val <= pHead2 -> val){
                h -> next = pHead1;
                pHead1 = pHead1 -> next;
            }else{
                h -> next = pHead2;
                pHead2 = pHead2 -> next;
            }
            h = h -> next;
        }
        /*while(pHead1){
            h -> next = pHead1;
            h = h -> next;
            pHead1 = pHead1 -> next;
        }
        while(pHead2){
            h -> next = pHead2;
            h = h -> next;
            pHead2 = pHead2 -> next;
        }*/
        if(pHead1){
            h -> next = pHead1;
        }
        if(pHead2){
            h -> next = pHead2;
        }
        return head -> next;
    }
};
```

<br>

还有使用递归的解法：

```cpp
class Solution {
public:
    ListNode* Merge(ListNode* pHead1, ListNode* pHead2)
    {
        ListNode* node=NULL;
        if(pHead1==NULL){return node=pHead2;}
        if(pHead2==NULL){return node=pHead1;}
        if(pHead1->val>pHead2->val){
            node=pHead2;
            node->next=Merge(pHead1,pHead2->next);
        }else
            {
            node=pHead1;
            node->next=Merge(pHead1->next,pHead2);
        }
        return node;
    }
     
};
```