## 3.2 排序相关题目

### 几乎有序的数组，即如果把数组排好序的话，每个元素移动的距离不超过k，k对于数组长度来说很小。用什么排序方法？

插入排序 O(N*k)

改进后堆排序 O(N*logk)

```cpp
class ScaleSort {
public:
    vector<int> sortElement(vector<int> A, int n, int k) {
        // write code here\\
        //堆化数组，大小为k的数组的最后一个元素的下边为k-1，它的父节点下标为（k-1-1）/2
        if(n==0||n<k||k<=1)return A;
        vector<int> C;
        for(int s=0;s<k;s++){
            C.push_back(A[s]);
        }
      
        int first=(k-2)/2;
        for(;first>=0;first--){//这个循环非常重要！！！！！
        MinHeapFixdown(C,first,k);
        }
         
        int count=0;
        while(count<n-k){
        A[count]=C[0];
        C[0]=A[count+k];
            count++;
        MinHeapFixdown(C,0,k);//代表从第0个元素开始，对大小为k的堆进行调整，调整成小根堆
        }//上面结束的时候得到的是一个小根堆，但是并不是将小根堆从上到下从左到右输出到B就可以了，而是还是一个一个的输出调整
        //下面需要将A数组中的前k个数据赋值给B
        while(k>=1&&count<n){
            A[count]=C[0];
            count++;
            C[0]=C[k-1];
            k--;
            MinHeapFixdown(C,0,k);
        }
        return A;
    }
    void MinHeapFixdown(vector<int> &C,int i,int n){
         int min;
        int current=C[i];
        while(2*i+1<n){
            min=2*i+1;
            if(2*i+2<n){
                if(C[min]>C[2*i+2])
                    min=2*i+2;
            }
            if(current>C[min])
                {
                  C[i]=C[min];
                  i=min;
                 
            }else{
                break;
            }
            }
        C[i]=current;//之所以放在这里是为了如果2*i+1大于n的话，而此时A[i]的位置还空着话，就要在这里进行赋值
        }
};
```

### 判断数组中是否有重复值，要求空间复杂度为O(1)

没有空间复杂度的限制，可以用哈希表

有空间限制，先排序，然后再判断，采用堆排序，非递归的堆排序，如果采用递归的堆排序，时间复杂度为O(logN)

```cpp
class Checker {
public:
     void heapSort(vector<int> &a,int n){
          int i=(n-1-1)/2;
            for(;i>=0;i--){
               MaxHeapFixdown(a,i,n);
            }
            int k=n-1;
            while(k>=0){
            int temp=a[0];
            a[0]=a[k];
            a[k]=temp;
            MaxHeapFixdown(a,0,k);
                 k--;
            }    
        }
     
           void MaxHeapFixdown(vector<int> &a, int i,int n){
            //从i开始对长度为n的向量a进行调整，调整成为大顶堆
               int max=i;
               int current=a[max];
            while(2*i+1<n){
                if(current<a[2*i+1])
                    max=2*i+1;
                if(2*i+2<n&&a[2*i+2]>a[2*i+1]&&a[2*i+2]>current)
                    max=2*i+2;
                if(i!=max){
                a[i]=a[max];
                i=max;
                }else{
                    break;
                }
            }
               a[i]=current;
             
        }
     
    bool checkDuplicate(vector<int> a, int n) {
        // 利用非递归的堆排序，所以要借助于循环！
  
         heapSort(a,n);
        for(int i=0;i<n-1;i++){
            if(a[i]==a[i+1]){
                return true;
            }
        }
        return false;
    }
};
```

### 两个有序数组合并为一个数组，第一个数组空间正好可以容纳所有元素

从后往前进行覆盖数组A，直到数组B为空

```cpp
class Merge {
public:
    int* mergeAB(int* A, int* B, int n, int m) {
        // write code here
        int i=n-1;
        int j=m-1;
        int count=n+m-1;
        while(i>=0&&j>=0){
            if(A[i]>B[j]){
                A[count]=A[i];
                count--;
                i--;
            }else{
                A[count]=B[j];
                count--;
                j--;
            }
        }
        while(i>=0){
           A[count]=A[i];
           count--;
           i--;
        }
        while(j>=0){
           A[count]=B[j];
           count--;
           j--; 
        }
        return A;
    }
};
```

### 三色排序

```cpp
class ThreeColor {
public:
    vector<int> sortThreeColor(vector<int> A, int n) {
        // write code here
    //其中最左边的left指向的是第一个不为0的数据；这个数据只可能为0或者1
        //最右边的right指向的是第一个不为2的数据，这个数据可能为0 1 2都有可能；
         int left=0;
         int right=n-1;
         int cur=0;
         int temp;
        while(cur<=right){
            if(A[cur]==1)
                cur++;
            else if(A[cur]==0){
                temp=A[cur];
                A[cur]=A[left];
                A[left]=temp;
                cur++;
                left++;
            }else if(A[cur]==2){
                temp=A[cur];
                A[cur]=A[right];
                A[right]=temp;
                right--;
        }
    }
        return A;
    }
};
```

### 有序矩阵查找，从右上开始找 O(M+N) O(1)

```cpp
class Finder {
public:
    bool findX(vector<vector<int> > mat, int n, int m, int x) {
        // write code here
        int j=m-1;//代表列
        int i=0;//代表行
        while(j>=0&&i<=n-1){
            if(mat[i][j]>x){
                i=i;
                j=j-1;  
        }
            else if(mat[i][j]<x){
                i=i+1;
                j=j;
            }
            else if(mat[i][j]==x){
                return true;
        }
        }
        return false;
    }
};
```

### 最短子数组， O(n) O(1)

```cpp
class Subsequence {
public:
    int shortestSubsequence(vector<int> A, int n) {
        // write code here
        int max,min,right,left;
        max=A[0];
        min=A[n-1];
        right=0;
        left=0;
        for(int i=0;i<n;i++){
            if(A[i]>max)
                max=A[i];
            else if(A[i]<max){
                right=i;
            }
        }
        for(int j=n-1;j>=0;j--){
            if(A[j]<min)
                min=A[j];
            else if(A[j]>min){
                left=j;
        }
        }
        if(right==0&&left==0){
            return 0;
        }
        else{
            return  (right-left+1);
        }
    }
     
};
```

### 排序之后相邻两数最大差值，采用桶排序 O(N) O(N)

```cpp
class Gap
{
public:
  int
  maxGap (vector<int> A, int n)
  {
    // write code here
    int max = A[0];
    int min = A[0];
    for (int i = 0; i < n; i++)
      {
    if (A[i] < min)
      min = A[i];
    if (A[i] > max)
      max = A[i];
      }
    //一共有n+1个桶，最后一个桶放最大值
    int interval = max - min; //必须用double类型，如果用整形，区间的计算长度经常都是分数，就会失真！！！
    //可以申请两个数组，长度都是n，一个存放每个桶中的最大值，一个存放每个桶中的最小值
    //int interval=(max-min)/n;一开始本来想直接计算出区间的，但是区间通常都是分数，如果变成int类型就会失真，导致后面的结果发生错误！！！
    vector<int> maxBucket (n + 1, INT_MIN);
    vector<int> minBucket (n + 1, INT_MAX);
    int index;
    for (int i = 0; i < n; i++)
      {
    index = (A[i] - min) * n / interval; //这里为了让结果不失真，将本来是(A[i] - min)/((max-min)/n)进行了整顿直接变成这个公式，防止了Bucket数组的越界以及结果失真
    if (A[i] > maxBucket[index])
      maxBucket[index] = A[i];
    if (A[i] < minBucket[index])
      minBucket[index] = A[i];
      }
    int result = 0;
    int pre = maxBucket[0]; //需要注意的是0桶里一定至少有一个值，就是整个数组中的最小值
    for (int j = 1; j < n+1; j++)//这里一定是n+1！！！！！！因为是n+1个桶啊啊啊啊啊！！！！！！
      {
    if (minBucket[j] != INT_MAX)
      { //这里的判断就是为了越过空桶！！！
        if (minBucket[j] - pre > result)
          result = minBucket[j] - pre; //注意这里最好不要使用max min这样的<algrithm>头文件中的函数，因为不支持！
        pre = maxBucket[j];
      }
 
      }
    return result;
 
  }
};
```


