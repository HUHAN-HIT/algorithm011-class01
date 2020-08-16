学习笔记

## 初级排序-O(n^2):( 记忆定义）

1.选择排序：selection sort: 每次找到最小值，然后放到待排序数组的起始位置

2.插入排序  Insertion sort : 从前到后逐步构建有序序列；对于未排序的数据，在已排序序列中从后向前扫描，找到相应的位置并插入

3.冒泡排序 Bubble Sort: 嵌套循环，每次查看相邻的元素如果逆序，则交换

## 快排

基本思想：通过一趟排序将待记录分隔成独立的两个部分，其中一个部分记录的关键字均比另一部分的关键字小，则可分别对两个部分记录继续进行排序，以达到整个序列有序

算法描述：

快速排序使用分治法来把一个串(list)分为两个子串sub-list，具体如下：

1.从数列中挑出一个元素，称为基准 pivot

2.重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区的操作

3.递归recursive 把小于基准值元素的子数列和大于基准值元素的子数列排序

java

```java
pubilc static void quickSort(int [] array, int begin, int end) {
    if (end <= begin) return ;
    int pivot = partition(array, begin, end);
    quickSort(array, begin, pivot - 1);
    quickSort(array, pivot, end);
}

static int partition(int [] array, int begin, int end) {
    // pivot: 标杆位置， counter：小于pivot元素的个数
    int pivot = end, counter = begin;
    for (int i = begin; i <  end; i++) {
        if (array[i] < array[pivot]) {
            int temp = array[i]; array[i] = array[pivot]; array[pivot] = array[i];
            counter++;
        }
    }
    int temp = array[pivot]; array[pivot] = array[counter]; array[counter] = array[pivot];
    return counter;
}
```

python

```python
def quick_sort(begin, end, nums):
    if begin >= end:
        return
    pivot_index = partition(begin, end, nums)
    quick_sort(begin, pivot_index - 1, nums)
    quick_sort(pivot_index, end, nums)

def partition(begin, end, nums):
    pivot = nums[begin]
    mark = begin
    for i in range(begin + 1, end + 1):
        if nums[i] < pivot:
            mark += 1
            nums[mark], nums[i] = nums[i], nums[mark]
    nums[begin], nums[mark] = nums[mark], nums[begin]
    return mark
```

C++

rand()不需要参数，会返回一个从0到最大随机数的任意整数，最大随机数的大小通常是固定的一个大整数；如果需要产生 0-99者100个整数的一个随机整数，则可以表达为：**int num = rand() % 100**

总的来说，可以表示为 **int num = rand() % n + a;**  其中，a是起始值， n - 1 + a 是终止值， n是整数的范围

```c++
int random_partition(vector<int>& nums, int l, int r) {
	int random_pivot_index = rand() % (r - l  + 1) + l;    // 随机选择pivot
    int pivot = nums[random_pivot_index];
    swap(nums[random_pivot_index], nums[r]);
    int i = l - 1;
    for (int j = l; j < r; j++) {
        if (nums[j] < pivot) {
            i++;
            swap(nums[i], nums[j]);
        }
    }
    int pivot_index = i + 1;
    swap(nums[pivot_index], nums[r]);
    return pivot_index;
}
void random_quicksort(vector<int> nums, int l, int r) {
    if (l < r) {
        int pivot_index = random_partition(nums, l, r);
        random_quicksort(nums, l, pivot_index - 1);
        random_quicksort(nums, pivot_index, r);
    }
}
```

C++最佳版本：

```c++
int partition(vector<int>& nums, int begin, int end) {
    int pivot = end, counter = begin;
    for (int i = begin; i < end; i++) {
        if (nums[begin] < nums[pivot]) {
            // 
            int temp = nums[begin]; nums[begin] = nums[counter]; nums[counter] = temp;
            counter++;
        }
    }
    int temp = nums[counter]; nums[counter] = nums[pivot]; nums[pivot] = temp;
    return counter;
}

void quickSort(vector<int>& nums, int begin, int end) {
    int (end <= begin) return;
    int pivot = partition(nums, begin, end);
    quickSort(nums, begin, pivot - 1);
    quickSort(nums, pivot + 1, end);
}
```

## 归并排序  Merge Sort

采用分治的思想

1.把长度为n的输入序列分成两个长度为n/2的子序列；

2.对这两个子序列分别采用归并排序

3.将两个排序好的子序列合并成一个最终的排序序列

```c++
void mergeSort(vector<int>& nums, int left, int right) {
    if (left >= right) return;
    int mid = (left + right) >> 1;      // (left + right) / 2
    mergeSort(nums, left, mid);
    mergeSort(nums, mid + 1, right);
    merge(nums, left, mid, right);
}

void merge(vector<int>& nums, int left, int mid, int right) {
    vector<int> temp(right - left + 1);
    int i = left, j = mid + 1, k = 0; // i表示第一个数组的起始位置, j表示第二个数组的起始位置, k 表示temp数组中已经填入的元素个数
    while (i <= mid && j <= right) { // i没有循环完成, j也没有循环完成
        temp[k++] = (nums[i] < nums[j]) ? nums[i++] : nums[j++]// 比较nums[i]和nums[j]之间的较小者,较小者放在temp中
    }
    while (i <= mid) temp[k++] = nums[i++];// 如果i没有走完,则将i剩余的数组
    while (j <= right) temp[k++] = nums[j++];
    for (int i = left, k = 0; i <= right;) nums[i++] = temp[k++];
}
```



归并和快排具有相似性，但步骤相反；

归并：先排序左右子数组，然后合并两个有序的子数组

快排：先调配左右子数组，然后对左右子数组进行排序

## 堆排序  heap sort

堆排序（Heapsort）是指利用**堆这种数据结构**所设计的一种排序算法。堆积是一个近似完全二叉树的结构，并同时满足堆积的性质：即子结点的键值或索引总是小于（或者大于）它的父节点。

#### 7.1 算法描述

- 将初始待排序关键字序列(R1,R2….Rn)构建成大顶堆，此堆为初始的无序区；
- 将堆顶元素R[1]与最后一个元素R[n]交换，此时得到新的无序区(R1,R2,……Rn-1)和新的有序区(Rn),且满足R[1,2…n-1]<=R[n]；
- 由于交换后新的堆顶R[1]可能违反堆的性质，因此需要对当前无序区(R1,R2,……Rn-1)调整为新堆，然后再次将R[1]与无序区最后一个元素交换，得到新的无序区(R1,R2….Rn-2)和新的有序区(Rn-1,Rn)。不断重复此过程直到有序区的元素个数为n-1，则整个排序过程完成。

堆插入O(logN) 取最大最小值O(1)

1.数组元素依次建立小顶堆

2.依次取堆顶元素，并删除

```c++
void heap_sort(vector<int>& a, int len) {
    priority_queue<int, vector<int>, greater<int>> q;
    for (int i = 0; i < len; i++) {
        q.push(a[i]);
    }
    for (int i = 0; i < len; i++) {
        a[i] = q.pop();
    }
}
```

手动维护一个堆

```c++
void heapify(vector<int>& array, int length, int i) {
    int left = 2 * i + 1, right = 2 * i + 2;
    int largest = i;
    
    
    if (left < length && array[left] > array[largest]) {
        largest = left;
    }
    if (right < length && array[right] > array[largest]) {
        largest = right;
    }
    
    if (largest != i) {
        int temp = array[i]; array[i] = array[largest]; array[largest] = temp;
        heapify(array, length, largest);
    }
    return;
}

void heapSort(vector<int>& array) {
    if (array.size() == 0) return;
    
    int length = array.size();
    for (int i = length / 2 - 1; i >= 0; i--) {
        int temp = array[0]; array[0] = array[i]; array[i] = temp;
        heapify(array, i, 0);
    }
    return;
}
```



## [题目767. 重构字符串](https://leetcode-cn.com/problems/reorganize-string/)

给定一个字符串`S`，检查是否能重新排布其中的字母，使得两相邻的字符不同。

若可行，输出任意可行的结果。若不可行，返回空字符串。

**示例 1:**

```
输入: S = "aab"
输出: "aba"
```

**示例 2:**

```
输入: S = "aaab"
输出: ""
```

**注意:**

- `S` 只包含小写字母并且长度在`[1, 500]`区间内。

思路：

利用了贪心的思想，每次输出剩余出现次数最多的字符就可以了。

可以这么做的原因在于，只有当一个字符出现的次数超过 (N+1) / 2 的情况下，这个问题才无解。只要初试情况下这个条件满足，每次都输出剩余出现次数最多的字符就可以将这个条件一直保持下去。
堆是一种天然能返回当前剩余出现次数最大字符的数据结构。

算法

在堆中存储 (count, letter) 这种格式的元素（在 Python 的实现中存储的是 count 的负数形式）。
每次从堆中弹出两个元素出来（代表两个剩余次数最大和第二大的字符），接着将这两个字符中与之前输出字符不同，出现次数最大的字符输出。之后把重新计算的剩余次数和字符再压入栈中。
最后，堆中可能会剩下一个元素，这个元素出现次数一定是 1。如果不是的话，那就不可能有这种排列。

```c++
class Solution {
public:
    string reorganizeString(string S) {
        map<char, int> maps;         // 用map数据结构不会重复
        string res;
        for (auto c : S) {
            maps[c]++;
            if (maps[c] > (S.size() + 1) / 2) return res;
        }
        priority_queue<pair<int, char>, vector<pair<int, char>, greater<int, char>> q;
        for (auto m : maps) {
            q.push({m.second, m.first});
        }
        
        string res = S;
        int i = 0; 
        while (!q.empty()) {
            char c = q.top().second;
            int cnt = q.top().first;
            q.pop();
            while (cnt--) {
                i = i >= S.size() ? 1 : i;
                res[i] = c;
                i += 2;
            }
        }
        return res;
    }
}
```



# 
