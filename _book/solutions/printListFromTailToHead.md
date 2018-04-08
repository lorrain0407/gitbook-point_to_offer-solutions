## 1.1 从尾到头打印链表

输入一个链表，从尾到头打印链表每个节点的值。

<br>

我的解法：

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
    vector<int> printListFromTailToHead(ListNode* head) {
        vector<int> res;
        if(head == NULL){
            return res;
        }
        ListNode *t = head;
        while(t -> next != NULL){
            res.push_back(t -> val);
            t = t -> next;
        }
        res.push_back(t -> val);
        reverse(res.begin(), res.end());
        return res;
    }
};
```

<br>

用库函数，每次扫描一个节点，将该结点数据存入vector中，如果该节点有下一节点，将下一节点数据直接插入vector最前面。其实和我的解法类似了！

```cpp
class Solution {
public:
    vector<int> printListFromTailToHead(struct ListNode* head) {
        vector<int> value;
        if(head != NULL)
        {
            value.insert(value.begin(),head->val);
            while(head->next != NULL)
            {
                value.insert(value.begin(),head->next->val);
                head = head->next;
            }         
             
        }
        return value;
    }
};
```

<br>

使用递归的解法，不过最好还是避免递归吧，毕竟慢啊！

```cpp
class Solution {
public:
    vector<int> printListFromTailToHead(struct ListNode* head) {
        vector<int> value;
        if(head != NULL)
        {
            value.insert(value.begin(),head->val);
            if(head->next != NULL)
            {
                vector<int> tempVec = printListFromTailToHead(head->next);
                if(tempVec.size()>0)
                value.insert(value.begin(),tempVec.begin(),tempVec.end());  
            }         
             
        }
        return value;
    }
};
```

<br>

利用栈的先进后出，emmmm。

```cpp
class Solution {
public:
    vector<int> printListFromTailToHead(struct ListNode* head) {  
        stack<int> stack;
        vector<int> vector;
        struct ListNode *p = head;
        if (head != NULL) {
            stack.push(p->val);
            while((p=p->next) != NULL) {
                stack.push(p->val);
            }
            while(!stack.empty()) {
                vector.push_back(stack.top());
                stack.pop();
            }
        }
        return vector;
    }
};
```

<br>

链表的就地反转（感觉还是蛮高级的），学习学习！

```cpp
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        vector<int> s;
        if(head==NULL){
            return s;
        }
        else if(head->next==NULL){
            s.push_back(head->val);
            return s;
        }
        ListNode* now=head->next;
        ListNode* pre=head;
        ListNode* temp=head->next->next;
        while(now){
            temp=now->next;
            now->next=pre;
            pre=now;
            now=temp;
        }
        head->next=NULL;
        while(pre){
            s.push_back(pre->val);
            pre=pre->next;
        }
        return s;
    }
};
```