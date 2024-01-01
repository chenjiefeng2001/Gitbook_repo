# 4. 两个排序的数组的中值问题

> ## 问题描述
>
>
> Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.
>
> The overall run time complexity should be `O(log (m+n))`.

## 例子


**Example 1:**

<pre><strong>Input:</strong> nums1 = [1,3], nums2 = [2]
<strong>Output:</strong> 2.00000
<strong>Explanation:</strong> merged array = [1,2,3] and median is 2.
</pre>

**Example 2:**

<pre><strong>Input:</strong> nums1 = [1,2], nums2 = [3,4]
<strong>Output:</strong> 2.50000
<strong>Explanation:</strong> merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.</pre>

---

### 条件

* `nums1.length == m`
* `nums2.length == n`
* `0 <= m <= 1000`
* `0 <= n <= 1000`
* `1 <= m + n <= 2000`
* `-10<sup>6</sup> <= nums1[i], nums2[i] <= 10<sup>6</sup>`

### 代码思路

   采用二分排序的方法可以将时间复杂度缩减到$O(\log(n+m)))$


代码如下：

```C++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& a, vector<int>& b) {
        vector<int> arr;
        int i =0;
        int j =0;
        int cnt=0;
      
        int n1 = a.size();
        int n2 = b.size();
        int n = n1 + n2;
        int ind2 = n/2;
        int ind1 = ind2-1;
        int indel1 = -1;
        int indel2 = -1;
        while( i< n1 && j< n2){
            if(a[i]<b[j]){
               if(cnt == ind1) indel1=a[i];
               if(cnt == ind2) indel2=a[i];
               i++;
               cnt++;
            }
            else{
               if(cnt == ind1) indel1=b[j];
               if(cnt == ind2) indel2=b[j];
                cnt++;
                j++;
            }
        }
        while(i< n1){
            if(cnt == ind1) indel1=a[i];
            if(cnt == ind2) indel2=a[i];
            i++;
            cnt++;
          
        }
        while(j<n2){
            if(cnt == ind1) indel1=b[j];
            if(cnt == ind2) indel2=b[j];
            cnt++;
            j++;
        }

      
        if( n% 2 == 1){
            return indel2;
        }
        return (double)((double)(indel1)+ (double)(indel2))/2.0;
    }
};
```
