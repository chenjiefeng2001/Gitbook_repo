# Quesion16 3个数的闭集

## 题目描述

Given an integer array nums of length n and an integer target, find three integers in nums such that the sum is closest to target.

Return the sum of the three integers.

You may assume that each input would have exactly one solution.

 

Example 1:

Input: nums = [-1,2,1,-4], target = 1
Output: 2
Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
Example 2:

Input: nums = [0,0,0], target = 1
Output: 0
Explanation: The sum that is closest to the target is 0. (0 + 0 + 0 = 0).
 

Constraints:

3 <= nums.length <= 500
-1000 <= nums[i] <= 1000
-10e4 <= target <= 10e4

## 代码思路
本题和上题的思路差不多，都是三指针的方式去找——首先使用双指针确定两数和，再用第三个指针去找第三个数，最后构成所需要的闭集

```cpp
class Solution {
public:

    //return 2sum thats closest
    int twosumclosest(int i, const vector<int>& nums, int target)
    {
        //find sum closest to tgt
        int start = i;
        int stop = nums.size()-1;
        int delta = std::numeric_limits<int>::max();
        int twosum = 0;
        while(start < stop)
        {
            int tmp = nums[start]+nums[stop];
            int tmpdelta = abs(target-tmp);


            //cout<<"2sum="<<tmp<<" delta="<<tmpdelta<<endl;
            if( tmp == target)
            {
                delta = 0;
                twosum = tmp;
                break;
            }
            else if(tmp > target)
            {
                if(tmpdelta < delta)
                {
                    delta = tmpdelta;
                    twosum = tmp;
                }
                stop--;
            }
            else
            {
                if(tmpdelta < delta)
                {
                    delta = tmpdelta;
                    twosum = tmp;
                }

                start++;
            }
        }

        return twosum;

    }


    int threeSumClosest(vector<int>& nums, int target) {


        /*
            sort ascending

            pick a number and 2 sum closest rest over loop

            track delta initialized to max
            and then keep updating when lesser delta found
            and also update the sum


        */

        sort(nums.begin(), nums.end());

        int delta = numeric_limits<int>::max();
        int res = 0;

        for(int i = 0; i < nums.size()-2; i++)
        {
            int twosum = twosumclosest(i+1, nums, target - nums[i]);

            //cout<<"twosum closest to "<<nums[i]<<" = "<<twosum<<endl;


            int threesum = (nums[i] + twosum);

            //cout<<"threesum="<<threesum<<endl;
            int tmpdelta = abs(target - threesum );
            if(tmpdelta < delta)
            {
                delta = tmpdelta;
                res = threesum; 
            }


        }

        return res;
        
    }
};

```
