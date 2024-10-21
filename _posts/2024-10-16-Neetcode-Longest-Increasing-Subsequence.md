---
title: NeetCode - Longest Increasing Subsequence
author: chulmin
date: 2024-10-16 00:00:00 -0500
categories: [Smashing Through Random Problems, Neetcode, Longest Increasing Subsequence]
tags: [coding test, c++, neetcode, DP]
math: true
---

## Idea
### before solving the problem
Although the difficulty level of this problem is categorized as medium, it becomes straightforward if you have a solid understanding of dynamic programming. The main concept is to store the best output that considers previous cases at the current state. This allows us to build upon previously computed results to find the solution efficiently.
## Code

``` c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {

        vector<int> dp(nums.size(), 1);

        int answer = 1;
        for ( int ii = 1; ii < nums.size(); ++ii ){
            for ( int jj = 0; jj < ii; ++jj ){
                if ( dp[ii] <= dp[jj] && nums[ii] > nums[jj]  ){
                    dp[ii] = dp[jj] + 1;
                }
            }
            if (dp[ii] > answer){
                answer = dp[ii];
            }
        }
        
        for ( int ii = 0; ii < nums.size(); ++ii ){
            printf("%d %d \n", nums[ii], dp[ii] );
        }

        return answer;
    }
};

```

## Reference
- [Longest increasing subsequence](https://neetcode.io/problems/longest-increasing-subsequence)