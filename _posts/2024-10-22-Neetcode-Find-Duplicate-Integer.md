---
title: NeetCode - Find Duplicate Integer
author: chulmin
date: 2024-10-22 00:00:00 -0500
categories: [Daily Smashing Through Random Problems, Neetcode, Find Duplicate Integer]
tags: [coding test, c++, neetcode]
math: true
---

## Idea
### Before solving the problem
This problem includes a caveat: the array should not be modified, and the solution must have 'O(1)' space complexity. This adds a layer of complexity. I couldn't find the solution on my own, but ChatGPT assisted me.

## Code
``` c++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int slow = nums[0];
        int fast = nums[0];

        do {
            slow = nums[slow];  
            fast = nums[nums[fast]];
        } while (slow != fast);  

        slow = nums[0];
        while (slow != fast) {
            printf("%d %d\n", slow, fast);
            slow = nums[slow];
            fast = nums[fast];
        }
        printf("%d %d\n", slow, fast);
        return slow;
    }
};

```

## Reference
- [Find Duplicate Integer](https://neetcode.io/problems/find-duplicate-integer)