## 1.6 两个链表的第一个公共结点

输入两个链表，找出它们的第一个公共结点。

这个题所谓公共结点是指这个结点之后所有的都一样了。。。因为单链表第一个公共点之后的next都是一样的，所以必然有公共的尾部。

如果有公共节点，1）若两个链表长度相等，那么遍历一遍后，在某个时刻，p1 == p2
                2)若两个链表长度不相等，那么短的那个链表的指针pn（也就是p1或p2）必先为null，那么这时再另pn = 链表头节点。经过一段时间后，则一定会出现p1 == p2。
如果没有公共节点：这种情况可以看成是公共节点为null，顾不用再考虑。

<br>

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
    ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) {
        ListNode *p1 = pHead1;
        ListNode *p2 = pHead2;
        while(p1!=p2){
            p1 = (p1==NULL ? pHead2 : p1->next);
            p2 = (p2==NULL ? pHead1 : p2->next);
        }
        return p1;
    }
};
```

<br>

找出2个链表的长度，然后让长的先走两个链表的长度差，然后再一起走
（因为2个链表用公共的尾部）

```cpp
class Solution {
public:
    ListNode* FindFirstCommonNode( ListNode *pHead1, ListNode *pHead2) {
        int len1 = findListLenth(pHead1);
        int len2 = findListLenth(pHead2);
        if(len1 > len2){
            pHead1 = walkStep(pHead1,len1 - len2);
        }else{
            pHead2 = walkStep(pHead2,len2 - len1);
        }
        while(pHead1 != NULL){
            if(pHead1 == pHead2) return pHead1;
            pHead1 = pHead1->next;
            pHead2 = pHead2->next;
        }
        return NULL;
    }
     
    int findListLenth(ListNode *pHead1){
        if(pHead1 == NULL) return 0;
        int sum = 1;
        while(pHead1 = pHead1->next) sum++;
        return sum;
    }
     
    ListNode* walkStep(ListNode *pHead1, int step){
        while(step--){
            pHead1 = pHead1->next;
        }
        return pHead1;
    }
};
```