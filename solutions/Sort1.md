## 3.1 经典排序的实现以及时空复杂度分析

### 常用几种排序及其时间复杂度

* 冒泡排序：O(n^2)，一趟最大的数在数组的最后

```cpp
int* bubbleSort(int* A, int n) {
    // write code here
    int i,j;
    for(i=0;i<n-1;i++){
        for(j=0;j<n-i-1;j++){
            if(A[j]>A[j+1]){
               int temp=A[j];
                A[j]=A[j+1];
                A[j+1]=temp;
            }  
        }
    }
    return A;
}
```

* 选择排序：O(n^2)，选出最大的从前往后放置

```cpp
int* selectionSort(int* A, int n) {
    int min;
    int i,j;
    for(i=0;i<n-1;i++){
        min=i;
        for(j=i;j<n;j++){
            if(A[j]<A[min])
                min=j;
        }
        int temp=A[i];
        A[i]=A[min];
        A[min]=temp;
    }
    return A;
}
```

* 插入排序：O(n^2)，当前位置的数与它前面的数进行比较

```cpp
int* insertionSort(int* A, int n) {
    // write code here
    int i,j;
    for(i=1;i<n;i++){
        int temp=A[i];
        for(j=i-1;j>=0;j--){
            if(A[j]>temp)
                A[j+1]=A[j];
            else
                break;
        }
        A[j+1]=temp;
　　　　　}
    return A;
}
```

* 归并排序：O(N*logN)，相邻有序区间合并

```cpp
class MergeSort {
public:
    int* mergeSort(int* A, int n) {
        // write code here
        merging(A,0,n-1);
        return A;
    }
    void merging(int* A,int start,int end){
        if(start<end){
            int middle=start+(end-start)/2;
            merging(A,start,middle);
            merging(A,middle+1,end);
            merge(A,start,middle,end);
        }else
            return;
    }
    void merge(int* A,int start,int middle,int end){
        int left=start;
        int right=middle+1;
        int* B=new int[end-start+1];
        int count=0;
        while(left<=middle&&right<=end){
            if(A[left]<A[right])
                B[count++]=A[left++];
            else
                B[count++]=A[right++];
        }
        while(left<=middle)
            B[count++]=A[left++];
        while(right<=end)
            B[count++]=A[right++];
        memcpy(A+start,B,sizeof(int)*(end-start+1));
        delete[] B;
    }
};
```

* 快速排序：O(N*logN)

```cpp
class QuickSort {
public:
    int* quickSort(int* A, int n) {
        // write code here
        quicksorting(A,0,n-1);
        return A;
    }
    void quicksorting(int* A,int start,int end){
        if(start<end){
        int pivot=partition(A,start,end);
        quicksorting(A,start,pivot-1);
        quicksorting(A,pivot+1,end);
        }else
            return ;
    }
   int partition(int* A,int start,int end){
        int low=start;
        int high=end;
        int p=A[start];
       while(low<high){
           while(A[high]>p&&low<high){
               high--;
           }
           if(low<high)
               A[low++]=A[high];
           while(A[low]<p&&low<high){
               low++;
           }
           if(low<high)
               A[high--]=A[low];
       }
       A[low]=p;
       return low;
   }
};
```

* 堆排序：O(N*logN)，大根堆，堆顶元素与最后一个元素交换，再调整为大根堆

```cpp
class HeapSort {
public:
    int* heapSort(int* A, int n) {
        // write code here
        int i=(n-1-1)/2;
        for(;i>=0;i--){
            updown(A,i,n);
        }
        for(i=0;i<n-1;i++){
            int temp=A[0];
            A[0]=A[n-i-1];
            A[n-i-1]=temp;
            updown(A,0,n-i-1);
        }
        return A;
    }
    void updown(int* A,int start,int n){
        int max;
        while(start<n){
            max=2*start+1;
            if(max>=n)
                break;
            if(2*start+2<n&&A[2*start+2]>A[max])
                max=2*start+2;
            if(A[start]<A[max]){
                int temp=A[max];
                A[max]=A[start];
                A[start]=temp;
                start=max;
            }else
                break;
        }
    }
};
```

* 希尔排序：O(N*logN)，插入的改良，插入步长不为1，是可以调整的，如步长为3，2，1

```cpp
class ShellSort {
public:
    int* shellSort(int* A, int n) {
        // write code here
        int shell=n/2;
        while(shell>=1){
        shellSort1(A,shell,n);
            shell=shell/2;
        }
        return A;
    }
    void shellSort1(int* A,int shell,int n){
        for(int i=0;i<n;i++){
            int j=i-shell;//shell为增量，直接插入排序的时候为1
            int temp=i;
            int current=A[i];
            while(j>=0){
                if(current<A[j]){
                    A[temp]=A[j];
                    j=j-shell;
                    temp=temp-shell; 
                }else{
                    break;
                }
            }
          A[temp]=current;
        }
        }
};
```

### 思想来自于桶排序的两种排序 

* 计数排序：O(n)，例如员工按照身高排序，根据厘米数

```cpp
class CountingSort {
public:
    int* countingSort(int* A, int n) {
        // write code here
         int max=A[0];
        for(int i=0;i<n;i++){
           if(A[i]>max)
               max=A[i];
        }
        int* B=new int[max+1];//实际上有max+1个数据，因为还有一个0
        int* C=new int[n];//C中存放的是排好序的数据
        //int B[max+1];
        for(int k=0;k<max+1;k++)
        {
            B[k]=0;
        }//代表max+1个桶
        int index;//代表第index个桶，第index个桶里面存放的是值为index的数据的个数
        for(int j=0;j<n;j++){
            index=A[j];
            B[index]=B[index]+1;
        }
        for(int m=1;m<max+1;m++){
            B[m]=B[m]+B[m-1];//这样B中的值代表m所处排好序的序列中的位置
             
        }
        //借助于另一个数组可以降低时间复杂度
        int pos,value;
        for(int p=n-1;p>=0;p--){
            value=A[p];
            pos=B[value]-1;
            C[pos]=value;
            B[value]--;
        }
       // for(int r=0;r<n;r++){
           // A[r]=C[r];
        //}
        memcpy(A,C,n*sizeof(int));
        delete C;
        delete B;
        return A;
    }
};
```

* 基数排序：O(n)，按照个位数大小进桶，然后依次十位数等进桶等

```cpp
class RadixSort
{ //基数排序
public:
  int*
  radixSort (int* A, int n)
  {
    // write code here
    int** B = new int*[10];
    for (int w = 0; w < 10; w++)
      {
    B[w] = new int[n + 1];
    B[w][0] = 0;
      } //B相当于桶，一共10个桶，代表0-9个数字
    int value, index;
    bool flag = false;
    int count = 1;
    while (!flag )
      {
    flag = true;
    for (int i = 0; i < n; i++)
      {
        value = A[i] / count % 10; //value是数据个位上的数
        if (value != 0)
          flag = false;
        B[value][0]++;
        index = B[value][0];
        B[value][index] = A[i];
      }
    if (flag == 1)
      break;
     int y = 0;
    for (int m = 0; m <= 9; m++)
      {
        for (int ii = 1; ii <= B[m][0]; ii++)
          {
        A[y] = B[m][ii];
        y = y + 1;
          }
        B[m][0] = 0;  //记得每次都要B清零,只需要将每个桶的总数量清零就可以，不必将桶中所有的数据都清除
      }
    count = 10 * count;
      }
    for (int z = 0; z < 10; z++)
      {
    if (B[z] != NULL)
      {
        delete[] B[z];
      }
      }
    if (B != NULL)
      {
    delete[] B;
      }
    return A;
  }
};
```

### 空间复杂度分析

* O(1)：插入排序、选择排序、冒泡排序、堆排序、希尔排序

* O(logN) ~ O(N)：快速排序

* O(N)：归并排序

* O(M)：M为选择桶的数量，计数排序、基数排序

### 稳定性分析

* 稳定的排序算法：冒泡排序、插入排序、归并排序、计数排序、基数排序、桶排序

* 不稳定的排序算法：选择排序、快速排序、希尔排序、堆排序