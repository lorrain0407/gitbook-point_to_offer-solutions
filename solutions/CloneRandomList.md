## 1.5 复杂链表的复制

输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

刚看到题目的时候觉得不太会，有提示是分解让复杂问题简单，也没想到什么解法，试了试递归，AC不了。。。后来看了讨论区解法，才有了一点点思路。

主要是三步法：

1、复制每个节点，如：复制节点A得到A1，将A1插入节点A后面

2、遍历链表，A1->random = A->random->next;

3、将链表拆分成原链表和复制后的链表

<br>

```cpp
struct RandomListNode {
    int label;
    struct RandomListNode *next, *random;
    RandomListNode(int x) :
            label(x), next(NULL), random(NULL) {
    }
};
class Solution {
public:
    RandomListNode* Clone(RandomListNode* pHead)
    {
        /*递归的方法不通过呀！！！
        if(pHead == NULL){
            return NULL;
        }
        RandomListNode* c = new RandomListNode(pHead -> label);
        c -> next = pHead -> next;
        c -> random = pHead -> random;
        c -> next = Clone(pHead -> next);
        return c;*/
        if(pHead == NULL){
            return NULL;
        }
        RandomListNode* h = pHead;
        while(h){
            //RandomListNode* n = h;这个地方是不对的！！！
            RandomListNode* n = new RandomListNode(h -> label);
            n -> next = h -> next;
            h -> next = n;
            h = n -> next;
        }
        
        h = pHead;
        while(h){
            if(h -> random){
                h -> next -> random = h -> random -> next;
            }
            h = h -> next -> next;
        }
        
        RandomListNode* c = pHead -> next;
        h = pHead;
        while(h -> next){
            RandomListNode* t = h -> next;
            h -> next = t -> next;
            h = t;
        }
        return c;
    }
};
```

<br>

还可以用哈希表解

```cpp
class Solution {
public:
    RandomListNode* Clone(RandomListNode* pHead)
    {
        if(pHead==NULL)
            return NULL;
         
        //定义一个哈希表
        unordered_multimap<RandomListNode*,RandomListNode*> table;
         
        // 开辟一个头结点
        RandomListNode* pClonedHead=new RandomListNode(pHead->label);
        pClonedHead->next=NULL;
        pClonedHead->random=NULL;
         
        // 将头结点放入map中
        table.insert(make_pair(pHead,pClonedHead));
         
        //设置操作指针
        RandomListNode* pNode=pHead->next;
        RandomListNode* pClonedNode=pClonedHead;
         
        // 第一遍先将简单链表复制一下
        while(pNode!=NULL)
        {
            // 不断开辟pNode的拷贝结点
            RandomListNode* pClonedTail=new RandomListNode(pNode->label);
            pClonedTail->next=NULL;
            pClonedTail->random=NULL;
             
            //连接新节点，更新当前节点
            pClonedNode->next=pClonedTail;
            pClonedNode=pClonedTail;
             
            //将对应关系  插入到哈希表中
            table.insert(make_pair(pNode,pClonedTail));
             
            //向后移动操作节点
            pNode=pNode->next;
        }
         
        //需从头开始设置random节点，设置操作指针
        pNode=pHead;
        pClonedNode=pClonedHead;
         
        // 根据map中保存的数据，找到对应的节点
        while(pNode!=NULL)
        {
             
            if(pNode->random!=NULL)
            {
                //找到对应节点，更新复制链表
                pClonedNode->random=table.find(pNode->random)->second;
            }
             
            //向后移动操作节点
            pNode=pNode->next;
            pClonedNode=pClonedNode->next;
        }
         
        return pClonedHead;
    }
};
```