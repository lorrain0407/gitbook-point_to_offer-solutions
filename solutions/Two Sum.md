## 1. Two Sum

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have **exactly** one solution, and you may not use the *same* element twice.

**Example:**

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].

```

<br>

解法一：O(nlogn)。先排序，再二分查找，或者从两头往中间查找的方式。

因为题目要求返回的是下标，除了用struct Node记录下标再排序之外，可以新开一个数组记录下标，然后利用lambda表达式对该数组进行排序，代码更简洁一些。

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int n = nums.size();
        vector<int> idx(n);
        for(int i = 0; i < n; i++){
            idx[i] = i;
        }
        sort(idx.begin(), idx.end(), [&](int i, int j){
            return nums[i] < nums[j];
        });
        
        int le = 0, ri = n-1;
        vector<int> rst;
        while(le < ri){
            if(nums[idx[le]] + nums[idx[ri]] == target){
                rst.push_back(idx[le]);
                rst.push_back(idx[ri]);
                return rst;
            }else if(nums[idx[le]] + nums[idx[ri]] > target) ri--;
            else le++;
        }
        return rst;
    }
};
```

<br>

解法二：O(n)。hash查找。为了避免重复，用边插入边查找的方式。

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int n = nums.size();
        unordered_map<int, int> hashMap;   // <num, index>
        vector<int> rst;
        for(int i = 0; i < n; i++){
            if(hashMap.find(target - nums[i]) != hashMap.end()){
                rst.push_back(i);
                rst.push_back(hashMap[target - nums[i]]);
                return rst;
            }else{
                hashMap[nums[i]] = i;
            }
        }
        return rst;
    }
};
```

