---
title: NeetCode - Median of Two Sorted Arrays
author: chulmin
date: 2024-10-21 00:00:00 -0500
categories: [Daily Smashing Through Random Problems, Neetcode, Median of Two Sorted Arrays]
tags: [coding test, c++, neetcode, Binary search]
math: true
---

## Idea
### Before solving the problem
The initial approach I tried involves converting both input arrays into deques, allowing me to efficiently pop elements from both the front and the back. This method is designed to help compare and remove the minimum and maximum values from the current state of both arrays.

### After solving the problem
Upon recognizing that the problem can be efficiently solved using binary search, I implemented a more optimized solution. The goal is to divide the combined arrays into two parts such that all elements in the left partition are less than or equal to those in the right partition. This leads us to the binary search strategy.

## Code
``` c++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        deque<int> nums1_deque( nums1.begin(), nums1.end() );
        deque<int> nums2_deque( nums2.begin(), nums2.end() );

        int min_num = INT_MAX;
        int max_num = INT_MIN;

        while( !nums1_deque.empty() && !nums2_deque.empty()){
            if ( nums1_deque.front() > nums2_deque.front() ){
                min_num = nums2_deque.front();
                nums2_deque.pop_front();
            }
            else{
                min_num = nums1_deque.front();
                nums1_deque.pop_front();
            }
            if ( nums1_deque.back() > nums2_deque.back() ){
                max_num = nums1_deque.back();
                nums1_deque.pop_back();
            }
            else{
                max_num = nums2_deque.back();
                nums2_deque.pop_back();
            }
        };

        while( nums1_deque.size() > 1 ){
            min_num = nums1_deque.front();
            nums1_deque.pop_front();
            max_num = nums1_deque.back();
            nums1_deque.pop_back();
        }
        while( nums2_deque.size() > 1 ){
            min_num = nums2_deque.front();
            nums2_deque.pop_front();
            max_num = nums2_deque.back();
            nums2_deque.pop_back();
        }

        printf("%d %d\n", nums1_deque.size(), nums2_deque.size());
        printf("%d %d\n", min_num, max_num);

        if ( nums2_deque.size() == 1 ){
            return nums2_deque.front();
        }
        else if ( nums1_deque.size() == 1 ){
            return nums1_deque.front();
        }
        else{
            return (max_num + min_num ) * 0.5;
        }

    }
};
```
``` c++

class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int len1 = nums1.size();
        int len2 = nums2.size();
        if( len1 > len2 ){
            return findMedianSortedArrays(nums2, nums1);
        }
        int low   = 0;
        int high  = len1;
        int total = len1 + len2;
        int half  = (total+1)/2;


        while( low <= high ){
            int mid1 = (low + high)/2;
            int mid2 = half - mid1;

            int left1  = (mid1 == 0)    ? INT_MIN:nums1[mid1-1];
            int right1 = (mid1 == len1) ? INT_MAX:nums1[mid1];
            int left2  = (mid2 == 0)    ? INT_MIN:nums2[mid2-1];
            int right2 = (mid2 == len2) ? INT_MAX:nums2[mid2];

            if ( left1 <= right2 && left2 <= right1){
                if (total % 2 == 1){
                    return max(left1, left2);
                }
                return ( max(left1, left2) + min(right1, right2) ) * 0.5;
            }
            else if( left1 > right2 ){
                high = mid1 - 1;
            }
            else{
                low  = mid1 + 1;
            }
        }
        return -1;
    }
};


```


## Reference
- [Median of Two Sorted Arrays](https://neetcode.io/problems/median-of-two-sorted-arrays)