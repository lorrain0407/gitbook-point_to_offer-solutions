## 2.1 翻转数组

给定一个长度为n的整数数组a，元素均不相同，问数组是否存在这样一个片段，只将该片段翻转就可以使整个数组升序排列。其中数组片段[l,r]表示序列a[l], a[l+1], ..., a[r]。原始数组为
a[1], a[2], ..., a[l-2], a[l-1], a[l], a[l+1], ..., a[r-1], a[r], a[r+1], a[r+2], ..., a[n-1], a[n]，
将片段[l,r]反序后的数组是
a[1], a[2], ..., a[l-2], a[l-1], a[r], a[r-1], ..., a[l+1], a[l], a[r+1], a[r+2], ..., a[n-1], a[n]。

**Example:**

```
输入：

第一行数据是一个整数：n (1≤n≤105)，表示数组长度。
第二行数据是n个整数a[1], a[2], ..., a[n] (1≤a[i]≤109)。

4
2 1 3 4

输出：

输出“yes”，如果存在；否则输出“no”，不用输出引号。

yes

```


<br>

```cpp
#include<iostream>
#include<algorithm>
using namespace std;

int main(){
	int n;
	while(cin>>n){
		int a[n];
		int aSort[n];
		for(int i = 0; i < n; i++){
			cin>>a[i];
			aSort[i] = a[i];
		}
		sort(aSort,aSort+n);
		int l = 0, h = n - 1;
		while(a[l] == aSort[l]){
			l++;			
		}
		while(a[h] == aSort[h]){
			h--;
		}
		int count = h - l + 1;
		int i = 0;
		bool ok = true;
		while(i < count / 2){
			if(a[l] == aSort[h] && a[h] == aSort[l]){
				l++;
				h--;
			}
			else{
				ok = false;
			}
			i++;
		}
		if(ok)
			cout<<"yes"<<endl;
		else
			cout<<"no"<<endl;
	}
	
	return 0;
}
```